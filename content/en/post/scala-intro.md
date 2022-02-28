---
title: "Why Scala?"
date: 2021-04-28T08:51:49-03:00
draft: false
tags:
- scala

---

## What is Scala?

Scala is an open-source language created by Martin Odersky, by concept, it is a general-purpose **multi-paradigm**
language, which combines **object-oriented** and **functional** programming in a high-level, type-safe language. Thanks
to its static types, it helps to avoid errors in very complex applications. It is executed in the JVM, it also
integrates with JavaScript, it allows you to build **high performance** systems with easy access to huge **library
ecosystems**.

In today's software development world it is considered a **complex language to learn** and they often say that few
developers know how to (really) work with Scala; Something with which it does not coincide... Why do we look at why it
arises? Scala emerges with the goal of being a better language, changing those aspects of Java that we consider old,
tedious, and frustrating, in order to have a cleaner, more organized result, easy to use, and with better final
productivity.

Scala is an acronym for **scalable** language, favors the creation of **abstractions**, programming
**distributed easily or simultaneously**, allows **immutability** and therefore is writing code using immutable
information, includes first-class values, case classes, higher-order functions, higher-class types, type classes, via
implicit, which makes it much easier to work with container types like Futures or Tasks.

#### How can I learn Scala?

There is a wide range of documentation to learn Scala on the net, many great books and countless blogs (In the end we
recommend), but an option that I always highly recommend for beginners and from which I learned a lot
is [Scala Exercises](https://www.scala-exercises.org/)
an interactive and practical platform where first they explain how it works and then you have to do a corresponding
exercise. In addition, we always have, free of charge or optional payment for the certification, some excellent courses
in [Coursera](https://es.coursera.org/specializations/scala) where we can learn from pi to pa as tenths the Cubans.

#### Let's look at a Hello World and some of its syntax

How are we going to present a language if we don't explain its **syntax** and do the typical **Hello World**. If we
compare with other languages such as Java or JavaScript, Scala has a very similar syntax but with the particularity that
neither the **";"** at the end of each line nor the reserved word **return** is required to return the value of a
method, it has **type inference**, everything is an **object**, including functions, well everything.

Let's look at the **Hello World** and other examples:

```scala
object HelloWorld {

  /**
   * To insert a comment at the beginning of a method and that is included in the ScalaDoc
   *
   * @param args we describe input arguments or other input value
   */
  def main(args: Array[String]): Unit = {
    val message = "Hello, world!"
    println(message) //Hello, world!
  }
}
```

We can also have a reduced Hello World version, we extend an App trait that makes it easier to write and execute any
code statement:

```scala
object HelloWorldMini extends App {
  println("Hello, world!") // Hello, world!
}
```

**Variables**:
In Scala, variables can be mutable or immutable, they only differ in the reserved word **var or val**, we already saw an
immutable example above, let's see a mutable:

```scala
object HelloWorldVar extends App {
  var value: String = ""
  value = "world"
  println(s"Hello, $value!") // Hello, world!
}
```

**Conditionals**:
The **if ... else** statement is similar to the other languages with the particularity that it doesn't allow ternaries:

```scala
object HelloWorldIfElse extends App {
  val language = "en"

  def message = {
    if (language == "es") "¡Hola, mundo!"
    else if (language == "pt") "¡Olá, mundo!"
    else "Hello, world!"
  }

  println(message) // Hello, world!
}
```

We can print this same example using **pattern matching**, it is like the equivalent of java's **switch...case**:

```scala
object HelloWorldPatternMatching extends App {
  val language = "pt"

  //no hay necesidad de colocar break el rompe donde primero encuentra
  val message = language match {
    case "es" => "¡Hola, mundo!"
    case "pt" => "¡Olá, mundo!"
    case _ => "Hello, world!"
  }
  println(message) //¡Olá, mundo!
}
```

**Loops or cycles**:
Los ciclos se comportan igual como otros lenguajes con la particularidad que se les puede incluir filtros o
condicionales internas, hay ejemplos como el **for...yield** que son muy interesantes por la particularidad y
legibilidad que aporta para el trabajo con collections y monads

```scala
object Numbers extends App {
  val numbers = List(1, 2, 3, 4, 5)

  // filter the pairs
  def pairNumbers: List[Int] = for {
    number <- numbers
    if number % 2 == 0
  } yield number

  println(pairNumbers.mkString(", ")) // 2, 4

  //Similar example using filter for odd numbers
  println(numbers.filter(_ % 2 != 0).mkString(", ")) // 1, 3, 5  
}
```

**Functions:**
With the functions, the same thing only changes the reserved word def, although being a functional language it allows
passing other functions as parameters, in addition to Currying, the use of implicits, or creating functions as a
variable, but that is for another post. However, here are some simple examples:

```scala
object Functions extends App {
  // There are two ways to declare functions.

  // A basic function and known here as callByValue
  def sum(x: Int, y: Int) = x + y
  // how do we invoke it
  sum(1, 2) // 3

  // Another one known here as callByName where we pass another function
  def product(x: => Int, y: => Int) = x * y

  // how do we invoke it
  def two() = 2

  product(two(), 6) // 12

  // Or another one a little more advanced where we pass a function to execute
  // And also that we can put default parameters
  def productF(f: Int => Int, x: Int, y: Int = 2) = f(x * y)

  // how do we invoke it
  // We define a function val
  val sum5: Int => Int = x => x + 5 // or simply _ + 5
  // we pass the same by parameters
  productF(sum5, 6, 6) // (6 * 6) + 5 = 41
  // We invoke without the last value so that it takes 2 po default
  productF(sum5, 5) // (5 * 2) + 5 = 15


  // We can pass as in other languages an array of parameters
  def sumAll(x: Int, y: Int, others: Int*) = {
    x + y + others.sum
  }
  // how do we invoke it
  sumAll(1, 2, 3, 4, 5, 6, 7, 8, 9) // 45


  // Or we can implement the product method with an example recursively
  def productRecursive(numbers: List[Int]): Int = numbers match {
    case Nil => 1 // If the list is empty we return 1
    case x :: tail => x * productRecursive(tail) // otherwise we multiply
  }

  productRecursive(List(1, 2, 3, 4, 5)) // 120

  // We close with an example of currying
  def sumCurrying(x: Int)(y: Int): Int = x + y
  // how do we invoke it
  sumCurrying(1)(2) // 3
  // but we can then store a partial sum in a variable
  val sum2 = sumCurrying(2) _
  // and then summon her with that advance
  sum2(3) // 5


  // Or that the second parameter is implicit from a particular context
  def productCurrying(x: Int)(implicit y: Int): Int = x * y

  // we define the implicit
  implicit val number3: Int = 3
  // now we consume with a single parameter and the other one will find it and use number3  
  productCurrying(5) // 15
}
```

In addition, scala has **classes** that can be instantiated for typical OOP use, also has a **case class** that are
immutable classes which export their constructor parameters, implement **.equals** and **.toString** by default, allow
the use of the very useful copy method and much more... but let's leave room for other posts.

#### Current Featured Uses

In recent years it has experienced spectacular growth and has become a standard for many companies, startups and
universities around the world.

In addition, they not only use Scala to create their new projects, but also for other tools with a great impact on the
market, such as Frameworks:

* [Play Framework](https://www.playframework.com/) -> The High Velocity Web Framework For Java and Scala
* [Apache Spark](https://spark.apache.org/) -> Unified engine for large-scale data analytics
* [Akka](https://akka.io/) -> Build powerful reactive, concurrent, and distributed applications more easily
* [Apache Kafka](https://kafka.apache.org/) -> Open-source distributed event streaming platform used by thousands of
  companies for high-performance

... Or great libraries that promote and make functional programming easier, to name a few:

* [Cats](https://typelevel.org/cats) -> Lightweight, modular, and extensible library for functional programming
* [Scalaz](https://scalaz.github.io/) -> Purely functional data structures to complement those from the Scala standard
  library
* [ZIO](https://zio.dev/) -> Type-safe, composable asynchronous and concurrent programming for Scala

A part to many more open-source projects for data access, communication between APIs, which greatly encourages new
and existing fans to see even more, the power of this beautiful language and confirm how interesting it is!

### What books are recommended to start:

* [Programming in Scala: A comprehensive Step-by-Step Guide](https://www.amazon.com/Programming-Scala-Comprehensive-Step-Step-ebook/dp/B004Z1FTXS/ref=sr_1_3?dchild=1&keywords=Programming+in+Scala%3A+A+comprehensive+Step-by-Step+Scala+Programming+Guide&qid=1587963475&s=books&sr=1-3)
  by Martin Odersky, Lex Spoon, Bill Venners
* [Scala for the Impatient](https://www.amazon.com/-/es/Cay-S-Horstmann-ebook/dp/B01MR67YSO/ref=sr_1_1?__mk_es_US=%C3%85M%C3%85%C5%BD%C3%95%C3%91&dchild=1&keywords=Scala+for+the+Impatient&qid=1587963619&s=books&sr=1-1)
  by Cay Hortsmann
* [Scala Cookbook](https://www.amazon.com/-/es/Alvin-Alexander-ebook/dp/B00EA66OM8/ref=sr_1_1?__mk_es_US=%C3%85M%C3%85%C5%BD%C3%95%C3%91&dchild=1&keywords=Scala+Cookbook&qid=1587963666&s=books&sr=1-1)
  by Alvin Alexander
* [Introduction to the Art of Programming Using Scala](https://www.amazon.com/-/es/Mark-C-Lewis-ebook/dp/B00AR3E2ZY/ref=sr_1_1?__mk_es_US=%C3%85M%C3%85%C5%BD%C3%95%C3%91&dchild=1&keywords=Introduction+to+the+Art+of+Programming+Using+Scala&qid=1587963691&s=books&sr=1-1)
  by Mark C. Lewis
* [Functional Programming in Scala](https://www.amazon.com/-/es/dp/B07MDLD48D/ref=sr_1_1?dchild=1&keywords=Functional+Programming+in+Scala&qid=1587963785&s=books&sr=1-1)
  by Paul Chiusano and Rúnar Bjarnason

### References and other links of interest:

* Scala Lang: [https://www.scala-lang.org/](https://www.scala-lang.org/)
* ScalaDays Blog: [https://blog.scaladays.org/](https://blog.scaladays.org/)
* Scala Times Blog: [https://scalatimes.com/](https://scalatimes.com/)
* SoftwareMill Blog: [https://blog.softwaremill.com/tagged/scala](https://blog.softwaremill.com/tagged/scala)
* Alvin Alexander / Scala: [https://alvinalexander.com/scala](https://alvinalexander.com/scala)
* Fragmentos del
  código: [Post1Intro.scala](https://github.com/rodobarcaaa/scala-blog-snippets/blob/main/src/main/scala/com/rodobarcaaa/Post1Intro.scala)
