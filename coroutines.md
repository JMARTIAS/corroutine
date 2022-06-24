# Corrutinas

para iniciar un proyecto con corrutinas hay que implementar unas dependencias
```gradle
 // Coroutines
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-core:1.3.5'
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-android:1.3.5'
```

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







