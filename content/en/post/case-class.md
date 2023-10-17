---
title: "Class, Case Class, and their secrets!"
date: 2021-04-30T08:51:49-03:00
draft: false
tags:
- scala
- class
- case class
---

As we already know from the previous article [Why Scala, what is it and what is interesting about it?](https://blog.rechemendía.com/art%C3%ADculos/scala/),
Scala is a programming language that combines object-oriented and functional programming, then today we will see the **class and case class**,
how to use them, some secrets of the **case class** and why they are the favorites for everyday use.

## What are Classes?
Like other programming languages, a class is a template that defines an object; can contain values, variables, types, and methods that mostly operate on them. In Scala, a class is defined with the keyword **class** and
an identifier or name to describe it. Let's see an example:

```scala
class MyFirstClass

val x = new MyFirstClass  
```
The language keyword **new** is used to create an instance of the class. **MyFirstClass** has a default constructor that
don't take arguments. But, we often want a constructor and a body in each class to give properties and behavior. Let's see another example with parameters constructor:


```scala
  class Vehicle(
      var passengers: Int, 
      var speed: Int,      
      val unit: String     
  ) {
    override def toString: String = s"(passengers: $passengers, speed: $speed $unit)"
  }

  val bicycle = new Vehicle(1, 30, "km")
  bicycle.passengers // 1
  bicycle.passengers = 2 
  bicycle.passengers // 2
  println(bicycle)   // (passengers: 2, speed: 30 km)
```
As we can see, this Vehicle class has **4 members**: the variables **passengers, speed and unit** that can be edited, the method **toString**,
unlike other languages, the main constructor is in the class signature and not after. builders like
methods can also take a default value, define mutable or immutable variables as we saw in the [previous example](https://blog.rechemendía.com/art%C3%ADculos/scala/)
and private or public as we will see below:

```scala
  class Vehicle1(
      var passengers: Int,            
      private val speed: Int,         
      private val unit: String = "km" 
  ) {
    val speedDescription: String  = s"$speed $unit"
    override def toString: String = s"(passengers: $passengers, speed: $speedDescription)"
  }

  object Vehicle1 {
    def speed220km(passengers: Int): Vehicle1 = new Vehicle1(passengers, 220, "km")
  }

  val motorcycle = new Vehicle1(2, 100)
  motorcycle.passengers       // 2
  motorcycle.speed            // don't compile because the attribute is private
  motorcycle.speedDescription // 100 km
  println(motorcycle)         // (passengers: 2, speed: 100 km)

  val car: Vehicle1 = Vehicle1.speed220km(5)
  println(car) // (passengers: 5, speed: 220 km)
```

In this last fragment, we can see a companion object where we can define other default constructors, such as the one to create a vehicle with 220 km of speed, also you can create an internal constructor that can also be overwritten but we don't show any example because the truth is not used either
as much as in other languages, in fact, many publications advise writing the most functional code possible, and for this, they recommend creating
their class attributes like **val** or they also use **case class** which we will explain next.


## What are case classes?
A **case class** is a class with all its features and more, when the Scala compiler sees the reserved word "case" in front of it.
of each **class** generates multiple benefits such as:

* The constructor parameters are **_val and public_** by default, also are **_immutable_**, so accessor methods are generated for each of them.
* A **_apply_** method is automatically generated on the companion object that allows instantiating without using the **_new_** keyword.
* A **_unapply_** method is generated that allows you to use case classes in more ways in **matching/pattern matching** expressions.
* A very useful **_copy_** method is generated and is used all the time in functional programming.
* In addition to **_equals, hashCode, and toString_**, allowing for better matching, use of map keys, terse typing, etc.

We will try to show all these characteristics below (starting and transforming the same example of the vehicle):

```scala
case class Vehicle2(passengers: Int, speed: Int, unit: String){
    val speedDescription: String  = s"$speed $unit"
}
```
We can already see a much cleaner and more concise class, we see how to instantiate objects in multiple ways:

```scala
// Normal constructor and the most used
val myCar  = Vehicle2(5, 200, "km")          
// Using apply explicitly
val myCar1 = Vehicle2.apply(5, 200, "km")    
// By "tuple" of values
val myCar2 = Vehicle2.tupled((5, 200, "km")) 
// Through currying mode parameters
val myCar3 = Vehicle2.curried(5)(200)("km")  
```
We can use the automatically generated methods:

```scala
myCar.passengers     // 5
myCar.speed          // 200
myCar.speed = 300    // don't compile -> error: reassignment to val
myCar.unit           // km
println(myCar)       // Vehicle2(5,200,km)

val myFastCar = myCar.copy(passengers = 2, speed = 320)
println(myFastCar) // Vehicle2(2,320,km)
```
We compare by structure and not by reference:

```scala
myCar == myCar1    // true
myCar == myCar2    // true
myCar == myCar3    // true
myCar == myFastCar // false
```
We can use the unapply in match expressions (Simple Mode):

```scala
def recognizeVehicle(x: Vehicle2): String = x match {
  case Vehicle2(10, speed, unit) =>
    s"10 passenger minivan with speed of $speed $unit"
  case Vehicle2(2, speed, _) if speed > 300 =>
    s"High speed sports car ${x.speedDescription}"
  case _ =>
    "Any non-minivan or sports car: " + x
}

val minivan = Vehicle2(10, 100, "km")
println(recognizeVehicle(minivan))   
// 10 passenger minivan with speed of 100 km

println(recognizeVehicle(myFastCar)) 
// High speed sports car 320 km

println(recognizeVehicle(myCar))
// Any non-minivan or sports car: Vehicle2(5,200,km)     
```       

Let's now look at a slightly more comprehensive example using **_unapply and pattern matching_**:
```scala
sealed trait Animal {
  def name: String
}

case class Dog(name: String, owner: String) extends Animal
case class Cat(name: String, color: String) extends Animal

def recognizeAnimal(a: Animal): String = a match {
  case Dog(name, owner) => s"The dog $name belongs to $owner."
  case Cat(name, color) => s"${name.capitalize} is a very beautiful $color cat."
}

val bony = Dog("Bony", "Pedrito")
val tom  = Cat("tom", "negro")

println(recognizeAnimal(bony)) 
// The dog Bony belongs to Pedrito.

println(recognizeAnimal(tom))  
// Tom is a very beautiful black cat.
```

This works thanks to the Scala standard that an **_unapply_** method returns the constructor attributes of each case class in a **tuple** that is
wrapped in an **Option** (We'll see in later articles what it means). This characteristic is considered according to Martin Odersky himself in
his book [Programming in Scala](https://www.amazon.com/Programming-Scala-Updated-2-12/dp/0981531687/) as the one with the greatest advantage of the **case class**, since
Pattern matching is a fundamental feature in all functional programming languages.

### References and other links of interest:
* Scala Lang(class): [Classes](https://docs.scala-lang.org/tour/classes.html)
* Alvin Alexander / Scala: [Case Classes](https://alvinalexander.com/scala/scala-class-examples-constructors-case-classes-parameters/)
* SCALA EXERCISES: [Classes vs Case Classes](https://www.scala-exercises.org/scala_tutorial/classes_vs_case_classes)
* Fragmentos del código: [Post2CaseClass.scala](https://github.com/rodobarcaaa/scala-blog-snippets/blob/main/src/main/scala/com/rodobarcaaa/Post2CaseClass.scala)





