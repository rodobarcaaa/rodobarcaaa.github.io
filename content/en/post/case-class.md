---
title: "Class, Case Class, and their secrets!"
date: 2021-04-30T08:51:49-03:00
draft: false
tags:
- scala
- class
- case class
---

Como ya sabemos del artículo anterior [¿Porque Scala, que es y tiene de interesante?](https://rodobarcaaa.github.io/art%C3%ADculos/scala/),
Scala es lenguaje de programación que combina la programación orientada a objetos y funcional, pues hoy veremos las **class y case class**,
como usarlas, algunos secretos de las **case class** y porque son las favoritas para el uso en el día a día.


## ¿Que son las Clases?
Al igual que otros lenguajes de programación, una clase es una plantilla que define la forma de un objeto; pueden contener valores,
variables, tipos y métodos que mayormente operan sobre estos. En Scala, una clase se define con la palabra reservada **class** y
un identificador o nombre que comience con Mayúsculas y describa la misma. Veamos un ejemplo:


```scala
class MyFirstClass

val x = new MyFirstClass  
```
La palabra reservada del lenguaje **new** se usa para crear una instancia de la clase. **MyFirstClass** tiene un constructor predeterminado que
no toma argumentos porque no se define en ella otro constructor. Pero, a menudo queremos un constructor y un cuerpo en cada clase. Veamos otro
ejemplo de clase con constructor de parámetros:


```scala
  class Vehicle(
      var passengers: Int, //número de pasajeros
      var speed: Int,      //velocidad
      val unit: String     //unidad de velocidad
  ) {
    override def toString: String = s"(passengers: $passengers, speed: $speed $unit)"
  }

  val bicycle = new Vehicle(1, 30, "km")
  bicycle.passengers // 1
  bicycle.passengers = 2 
  bicycle.passengers // 2
  println(bicycle) // (passengers: 2, speed: 30 km)
```
Como podemos ver esta clase Vehículo tiene **4 miembros**: las variables **passengers, speed y unit** que se pueden editar, el método **toString**,
a diferencia de otros lenguajes, el constructor principal está en la firma de la clase y no posteriormente. Los constructores como los
métodos también pueden tomar un valor predeterminado, definir las variables mutables o inmutables como ya vimos en el ejemplo anterior
y privadas o publicas como veremos a continuación:


```scala
  class Vehicle1(
      var passengers: Int,            //número de pasajeros
      private val speed: Int,         //ó simplemente speed: Int
      private val unit: String = "km" //ó simplemente val unit: String
  ) {
    val speedDescription: String  = s"$speed $unit"
    override def toString: String = s"(passengers: $passengers, speed: $speedDescription)"
  }

  object Vehicle1 {
    def speed220km(passengers: Int): Vehicle1 = new Vehicle1(passengers, 220, "km")
  }

  val motorcycle = new Vehicle1(2, 100)
  motorcycle.passengers       // 2
  motorcycle.speed            // no compila no tiene acceso atributo privado
  motorcycle.speedDescription // 100 km
  println(motorcycle)         // (passengers: 2, speed: 100 km)

  val car: Vehicle1 = Vehicle1.speed220km(5)
  println(car) // (passengers: 5, speed: 220 km)
```

En este último fragmento podemos ver un objeto acompañante donde podemos definir otros constructores predeterminados como el de crear un vehículo
de velocidad 220 km, cabe decir que también se pueden sobre-escribir constructores internos pero no mostramos ningún ejemplo pues la verdad tampoco se usa
tanto como en otros lenguajes, de hecho, muchas publicaciones aconsejan escribir el código lo más funcional posible, y para eso recomiendan crear
sus atributos de clase como **val** o también usar **case class** que ya vamos a explicar a continuación.


## ¿Que son las Case Clases?
Una **case class** es una clase con todas sus funcionalidades y más, cuando el compilador de Scala ver la palabra reservada "case" delante
de cada **class** genera por nosotros multiples beneficios tales como:

* Los parámetros del constructor son **_val y public_** de forma predeterminada, por lo que se generan métodos de acceso para cada uno de ellos.
* Automáticamente se genera en el objeto acompañante un método **_apply_** que permite crear instancias sin usar la palabra **_new_**.
* Se genera un método **_unapply_** que le permite usar de más formas las case clases en expresiones **match/pattern matching**.
* Se genera un método **_copy_** muy útil y que se usa todo el tiempo en la programación funcional.
* Además de **_equals, hashCode y toString_**, permitiendo una mejor comparación, usos de claves de mapas, escrituras concisas, etc.

Todas esas características intentaremos mostrar a continuación (partiendo y transformando el mismo ejemplo del vehículo):

```scala
case class Vehicle2(passengers: Int, speed: Int, unit: String){
    val speedDescription: String  = s"$speed $unit"
}
```
Ya podemos ver una clase mucho más limpia y concisa, vemos como crear instancias de objetos de multiples maneras:

```scala
// Constructor normal y el más usado
val myCar  = Vehicle2(5, 200, "km")          
// Usando apply explícitamente
val myCar1 = Vehicle2.apply(5, 200, "km")    
// Mediante "tupla" de valores
val myCar2 = Vehicle2.tupled((5, 200, "km")) 
// Con parámetros modo currying
val myCar3 = Vehicle2.curried(5)(200)("km")  
```
Usamos los métodos generados automáticamente:

```scala
myCar.passengers     // 5
myCar.speed          // 200
myCar.speed = 300    // no compila -> error: reassignment to val
myCar.unit           // km
println(myCar)       // Vehicle2(5,200,km)

val myFastCar = myCar.copy(passengers = 2, speed = 320)
println(myFastCar) // Vehicle2(2,320,km)
```
Comparamos por estructura y no por referencia:

```scala
myCar == myCar1    // true
myCar == myCar2    // true
myCar == myCar3    // true
myCar == myFastCar // false
```
Usamos el unapply en expresiones match (Modo Simple):

```scala
def recognizeVehicle(x: Vehicle2): String = x match {
  case Vehicle2(10, speed, unit) =>
    s"Minivan de 10 pasajeros con velocidad de $speed $unit"
  case Vehicle2(2, speed, _) if speed > 300 =>
    s"Auto deportivo de alta velocidad ${x.speedDescription}"
  case _ =>
    "Cualquier auto no minivan, ni deportivo: " + x
}

val minivan = Vehicle2(10, 100, "km")
println(recognizeVehicle(minivan))   
// Minivan de 10 pasajeros con velocidad de 100 km

println(recognizeVehicle(myFastCar)) 
// Auto deportivo de alta velocidad 320 km

println(recognizeVehicle(myCar))
// Cualquier auto no minivan, ni deportivo: Vehicle2(5,200,km)     
```       

Veamos ahora un ejemplo un poco más abarcador usando **_unapply y pattern matching_**:
```scala
sealed trait Animal {
  def name: String
}

case class Dog(name: String, owner: String) extends Animal
case class Cat(name: String, color: String) extends Animal

def recognizeAnimal(a: Animal): String = a match {
  case Dog(name, owner) => s"El perro $name es de $owner."
  case Cat(name, color) => s"${name.capitalize} es un gato $color muy hermoso."
}

val bony = Dog("Bony", "Pedrito")
val tom  = Cat("tom", "negro")

println(recognizeAnimal(bony)) 
// El perro Bony es de Pedrito.

println(recognizeAnimal(tom))  
// Tom es un gato negro muy hermoso.
```
Esto funciona gracias al estándar de Scala de que un método **_unapply_** devuelva los atributos de constructor de cada case class en una **tupla** que está
envuelta en un **Option** (Ya veremos en posteriores artículos que significa). Esta característica es considerada según el mismísimo Martin Odersky en
su libro [Programming in Scala](https://www.amazon.com/Programming-Scala-Updated-2-12/dp/0981531687/) como la de mayor ventaja de las **case class**, pues
los pattern matching es una característica fundamental en todos los lenguajes de programación funcional.


### Referencias y otros links de interés:
* Scala Lang(class): [Classes](https://docs.scala-lang.org/tour/classes.html)
* Alvin Alexander / Scala: [Case Classes](https://alvinalexander.com/scala/scala-class-examples-constructors-case-classes-parameters/)
* SCALA EXERCISES: [Classes vs Case Classes](https://www.scala-exercises.org/scala_tutorial/classes_vs_case_classes)
* Fragmentos del código: [Post2CaseClass.scala](https://github.com/rodobarcaaa/scala-blog-snippets/blob/main/src/main/scala/com/rodobarcaaa/Post2CaseClass.scala)





