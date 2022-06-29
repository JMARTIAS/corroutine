# Corrutinas

se usan hace muchos años en otros lenguajes, en kotlin es algo "nuevo" y da solución a varios problemas comunes de android, 
- permiten reemplazar las callbacks y transformarlo en programación secuencial
- mantienen el código Main Safe, esto quiere decir que se pueden llamar corrutinas desde el Main Thread y todo va a estar bien

Si el código para una network request se llama directamente de main thread, lo bloquearía, ya que toma unos segundos en hacer dicha consulta, si se hace esto mismo con callbacks 
para iniciar un proyecto con corrutinas hay que implementar unas dependencias llama a la network request en otro thread y cuando este lista la data la pasará de vuelta al Main Thread. un callback es el manejo que hace la librería que pasa de un hilo en segundo plano al principal.

Si bien los callbacks son una buena solución
- No manejan bien los errores sin tener que escribir mas código (no maneja excepciones)
- pueden ser abrumadores al llamar a muchos callbacks en la misma función 

Las corrutinas manejan esto, se llaman como función `suspend` en la main thread, pero esta sigue funcionando, la corrutina se supende y cuando tiene el resultado se reanuda, la corrutina también usa otro hilo.

Las corrutinas también permiten encadenar varias llamadas a tareas que demoran mucho 

`Kotlin has a method Deferred.await() that is used to wait for the result from a coroutine started with the async builder.`

## Coroutine Scope 

Todas las corrutinas corren en un `CoroutineScope` A scope controls the lifetime of coroutines through its job, si cancelas el job de un scope cancela todas las corrutinas inicializadas en ese scope. Los scopes te permiten controlar un dispatcher por defecto, el dispatcher controla que hilo corre una corrutina.

Una corrutina inicializada en el Main thread puede cambiar el dispatcher en cualquier momento luego de que ha empezado.

## Usar viewModelScope
La librería 
```gradle
implementation "androidx.lifecycle:lifecycle-viewmodel-ktx:x.x.x"
```
agrega el CoroutineScope a los ViewModels que esta configurado para comenzar corrutinas relacionadas con la UI. Agrega una extension function `viewModelScope`, el que esta vinculado a Dispatchers.Main y se cancelará automaticamente cuando ViewModel sea limpiado?

_cuando necesito cambiar entre dispatchers, porque lo que voy a realizar podría bloquear el hilo, se usa el `withContext`, el que cambia el dispatcher solo por el lambda, cuando tiene el resultado vuelve al dispatcher inicial_

hay 3 
- Main
- IO
- Default


```gradle
 // Coroutines
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-core:1.3.5'
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-android:1.3.5'
```
- kotlinx-coroutines-core — Main interface for using coroutines in Kotlin
- kotlinx-coroutines-android — Support for the Android Main thread in coroutines

en android es esencial evitar bloquear el main Thread, este maneja todas las actualizaciones de la UI, para que se vea bien la pantalla se debe actualizar cada 16 ms, la mayoría de las tareas toman mas que eso y si las llamamos del main thread la aplicación se podría pausar, stutter o incluso freezear. Si se bloquea por mucho rato se puede crashear y aparece el dialogo de `application not responding`

las corrutinas corren en un scope, el mas usado es el GlobalScope, y esto solo quiere decir que la corrutina existirá mientras exista nuestra app
si termina su trabajo, tambien morirá creo? este GlobalScope lanza un nuevo hilo donde se ejecuta la corrutina 

```kotlin
GlobalScope.launch {
  Log.d( TAG
}
```
Las corrutinas se puedes suspender, se pueden pausar o continuar,
la función sleep de un Thread, bloqueará el hilo por una cantidad determinada de milisegundos 

las corrutinas tienenn una funcion delay(), la que funciona muy distinto a sleep, ya que no bloquea todo el hilo, solo pausa la corrutina

suspend funtions solo pueden ser usadas en suspend functions o en corrutinas 

## Coroutine Context

Podemos pasarle un contexto a la función launch, el Dispatchers
dependiendo de lo que necesitamos hacer es el Dispatcher le vamos a pasar

- Dispatchers.Main -> las lanza en el hilo principal y sirve para hacer cambios en la UI, ya que estos solo se harán en este hilo 
- Dispatchers.IO  -> Se usa para operaciones de data como Networking, bases de datos, leer y escribir archivos
- Dispatchers.Default -> es el que debes usar si quieres hacer algo que tome mucho tiempo, por ejemplo quieres analizar una lista de 10000 elementos, lo haces aqui para no bloquear el hilo principal
- Dispatchers.Unconfined -> no esta confinado a vivir en un hilo, si usas este va a vivir en el hilo donde se llamó la función suspend

Tambien podemos empezar nuestro propio hilo que se hace con newSingleThreadContext("Mythread")

lo bueno de las corrutinas es que te puedes cambiar facilmente de thread con withContext()

runBlocking bloquea el main thread
 las corrutinas se pueden cancelar, pero les toma tiempo darse cuenta de esto, así que habria que agregarle un if que vea si esta activa,
 tambien tienen una funcion que es 
 
 ```kotlin
 withTimeout(3000L) {}
```
que me permite cancelarlas después de un tiempo 

## Scope







