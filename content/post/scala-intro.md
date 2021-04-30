---
title: "¿Porque Scala? ¿Que es y tiene de interesante?"
date: 2021-04-28T08:51:49-03:00
draft: false
tags:
- scala
---


## ¿Qué es Scala?
Scala es un lenguaje open source creado por Martin Odersky, por concepto es un lenguaje **multi-paradigma** de propósito general,
que combina programación **orientada a objetos** y **funcional** en un lenguaje de alto nivel y con tipos seguros.
Gracias a sus tipos estáticos ayuda evitar errores en aplicaciones muy complejas. Es ejecutado en la JVM, además se integra
con JavaScript, permite construir sistemas de **alto rendimiento** y con fácil acceso a enormes **ecosistemas de librerías**.


En el mundo del desarrollo de software actual es considerado como un **lenguaje complejo para aprender** y suelen decir que
pocos desarrolladores saben cómo trabajar (de verdad) con Scala; Algo con lo que no coincido ... ¿Porque miremos porque surge?  Scala surge con el
objetivo de ser un mejor lenguaje, cambiando esos aspectos de Java que consideramos antiguos, tediosos y frustrantes,
para así tener un resultado más limpio, organizado, fácil de utilizar y con una mejor productividad final.  


Scala es un acrónimo de lenguaje **escalable**, favorece la creación de **abstracciones**, la programación
**distribuida o simultánea**, permite **inmutabilidad** y por ello es fácil escribir código mediante el uso de  
información inmutable, incluye: first-class vals, case classes, higher order functions, higher-kinded types,
typeclasses, through implicits, que hace mucho más fácil trabajar con tipos containers como Futures o Tasks.


#### ¿Como puedo aprender Scala?
Hay una amplia gama de documentación para aprender Scala en la red, muchos libros buenísimos e innumerables blogs (Al final recomendamos),
pero una opción que siempre recomiendo mucho, para principiantes y de la cual aprendí un montón es [Scala Exercises](https://www.scala-exercises.org/)
una plataforma interactiva y práctica donde primero te explican el funcionamiento y posteriormente debe realizar un ejercicio acorde.
Además tenemos siempre de forma gratuita u opcional pago para la certificación unos excelentes cursos en [Coursera](https://es.coursera.org/specializations/scala) donde podemos aprender del pi al pa como
decimos los cubanos.   


#### Veamos un Hola Mundo y parte de su sintaxis
Como vamos a presentar un lenguaje si no explicamos su **sintaxis** y hacemos el típico **Hola Mundo**. Si comparamos con
otros lenguajes como Java o JavaScript, Scala tiene una sintaxis muy similar pero con la particularidad que no es obligatorio ni el **";"** al final de cada línea ni la palabra reservada **return** para retornar el valor de un método, cuenta con **inferencia de tipos**, todo es un **objeto**, incluyendo las funciones, bueno todo todo.


Veamos el **Hola Mundo** y otros ejemplos:

```scala
object HelloWorld {

  /**
    * Como insertamos un comentario en un inicio de un método y que se incluye en el ScalaDoc
    *
    * @param args describimos la entrada de argumentos o otro input value
    */
  def main(args: Array[String]): Unit = {
    //Comentario interno y creación de una variable o valiable
    //como algunos le llamamos a los val inmutables
    val message = "¡Hola, mundo!"
    println(message) //¡Hola, mundo!
  }
}
```

También podemos tener una versión del Hola Mundo reducida, extendemos de un trait App que facilita poder escribir y ejecutar cualquier sentencia de código:

```scala
object HelloWorldMini extends App {
  println("¡Hola, mundo!") //¡Hola, mundo!
}
```
**Variables**:
En Scala las variables pueden ser mutables o inmutables solo difieren en la palabra reservada **var o val** ya apreciamos arriba un ejemplo inmutable veamos una mutable:

```scala
object HelloWorldVar extends App {
  // Declaramos una variable mutable vacía de tipo String
  var value: String = ""
  // Le asignamos un valor
  value = "mundo"
  // Imprimimos el valor y acá podemos ver como usar string interpolation
  println(s"¡Hola, $value!") //¡Hola, mundo!
}
```
**Condicionales**:
La sentencia **if ... else** es similar a los otros lenguajes con la particularidad que no permite ternarias
```scala
object HelloWorldIfElse extends App {
  // Podemos ver como vimos antes que si no ponemos tipo scala lo infiere por nosotros
  val language = "en"

  // podemos notar que si no tiene parámetros la función no necesita paréntesis
  def message = {
    if (language == "es") "¡Hola, mundo!"
    else if (language == "pt") "¡Olá, mundo!"
    else "Hello, world!"
  }

  println(message) //Hello, world!
}
```

Este mismo ejemplo lo podemos imprimir usando **pattern matching**, viene siendo como el equivalente al *switch...case* de java.
```scala
object HelloWorldPatternMatching extends App {
  val language = "pt"

  //no hay necesidad de colocar break el rompe donde primero encuentra
  val message = language match {
    case "es" => "¡Hola, mundo!"
    case "pt" => "¡Olá, mundo!"
    case _    => "Hello, world!"
  }
  println(message) //¡Olá, mundo!
}
```

**Bucles o ciclos**:
Los ciclos se comportan igual como otros lenguajes con la particularidad que se les puede incluir filtros o condicionales internas,
hay ejemplos como el **for...yield** que son muy interesantes por la particularidad y legibilidad que aporta para el trabajo con collections y monads

```scala
object Numbers extends App {
  // Podemos inicializamos una lista y al mismo tiempo le pre-populamos valores
  val numbers = List(1,2,3,4,5)

  // Recorremos la lista de números y filtramos los pares
  def pairNumbers: List[Int] = for {
    number <- numbers 
    if number % 2 == 0 
  } yield number
  
  // Imprimimos el resultado separados por ", " 
  println(pairNumbers.mkString(", ")) // 2, 4
  
  //Plus: Ejemplo similar usando filter para los impares
  println(numbers.filter(_ % 2 != 0).mkString(", ")) // 1, 3, 5  
}
```

**Funciones:**
Con las funciones lo mismo solo cambia palabra reservada **def**, aunque al ser un lenguaje funcional permite pasar otra funciones como parámetros, además de Currying, el uso de los implicitos, o crear funciones como variable, pero eso da para otro post. No obstante presentamos algunos ejemplos sencillos:
```scala
object Functions extends App {
 //Existen dos formas de declarar las funciones   

  // Una función básica y conocida acá como callByValue
  def sum(x: Int, y: Int) = x + y
  // Como la invocamos
  sum(1,2) // 3

  // Otra conocida acá como callByName donde pasamos otra función
  def product(x: => Int, y: => Int) = x * y
  // Como la invocamos
  def two() = 2
  product(two(), 6) // 12

  // U otro un poco más avanzado donde pasamos una función a ejecutar
  // Y además notemos que podemos poner parámetros por default
  def productF(f: Int => Int, x: Int, y: Int = 2) = f(x * y)
  // Como la invocamos
  // Definimos una función val 
  val sum5: Int => Int = x => x + 5 // ó simplemente _ + 5
  // Pasamos la misma por parametros
  productF(sum5, 6, 6) // (6 * 6) + 5 = 41
  // Invocamos sin el ultimo valor para que tome 2 po default
  productF(sum5, 5) // (5 * 2) + 5 = 15


  // Podemos pasar como en otros lenguajes un array de parámetros
  def sumAll(x: Int, y: Int, others: Int*) = {
     x + y + others.sum
  }
  // Como la invocamos
  sumAll(1,2,3,4,5,6,7,8,9) // 45
  

  // O podemos implementar el método product con un ejemplo de forma recursiva
  def productRecursive(numbers: List[Int]): Int = numbers match { 
       case Nil => 1 // Si la lista es vacía retornamos 1
       case x :: tail => x * productRecursive(tail) // Sino multiplicamos 
   } 
  productRecursive(List(1,2,3,4,5)) // 120
   
  //Cerramos con un ejemplo de currying
  def sumCurrying(x: Int)(y: Int): Int = x + y 
  // Como la invocamos
  sumCurrying(1)(2) // 3
  // Pero podemos entonces almacenar una suma parcial en una variable
  val sum2 = sumCurrying(2) _
  // y luego invocarla con ese adelanto
  sum2(3) // 5
  
   
  //O que el segundo parámetro se inplicito de un contexto particular
  def productCurrying(x: Int)(implicit y: Int): Int = x * y 
  // Definimos el implicito
  implicit val number3: Int = 3
  // Ahora consumimos con un solo parámetro y el otro lo encontrará y usará number3  
  productCurrying(5) // 15
}
```

Además scala posee **clases** que se le puede crear instancias para tipico uso de POO, cuenta con **case class** que son clases inmutales las cuales exportan sus parámetros constructores, implementan por default el .equals y .toString, permiten usar el muy útil metodo copy y mucho más ... pero dejemos espacio para otros posts.


#### Usos Destacados Actuales
En los últimos años viene experimentado un crecimiento espectacular y ha pasado a convertirse en un estándar para muchas
empresas, startups y universidades de todo el mundo.


Además no solo utilizan Scala para crear sus nuevos proyectos, sino también para otras herramientas de gran impacto en el mercado como los Frameworks:
* [Play Framework](https://www.playframework.com/) -> Un framework de alta velocidad para el desarrollo con Java y Scala.
* [Apache Spark](https://spark.apache.org/) -> Motor de análisis para el procesamiento de datos a gran escala.
* [Akka](https://akka.io/) -> Framework para aplicaciones reactivas, concurrentes y distribuidas con mayor facilidad.
* [Apache Kafka](https://kafka.apache.org/) -> Herramienta para construir canalizaciones de datos y aplicaciones de transmisión en tiempo real.

... O grandes librerías que promueven y hacen que sea más fácil la programación funcional, por mencionar algunas:
* [Cats](https://typelevel.org/cats) -> Librería ligera, modular y extensible para programación funcional.
* [Scalaz](https://scalaz.github.io/) -> Conjunto de estructuras puramente funcionales para complementar las de Scala.
* [Magnolia](https://magnolia.work/opensource/magnolia) -> Una Macro genérica para la materialización automática de clases de tipos para tipos de datos compuestos.
* [ZIO](https://zio.dev/) -> Este último creando un amplio y potente ecosistema que va a hacer en mi opinión más interesante Scala en el futuro.

¡Además de muchísimos más proyectos open source para acceso a datos, comunicación entre APIs, que favorece con creces que los nuevos y actuales seguidores vean aún más, la potencia de este hermoso lenguaje y confirmen lo interesante que es!

### Que libros se recomiendan para empezar:
* [Programming in Scala: A comprehensive Step-by-Step Guide](https://www.amazon.com/Programming-Scala-Comprehensive-Step-Step-ebook/dp/B004Z1FTXS/ref=sr_1_3?dchild=1&keywords=Programming+in+Scala%3A+A+comprehensive+Step-by-Step+Scala+Programming+Guide&qid=1587963475&s=books&sr=1-3) por Martin Odersky, Lex Spoon, Bill Venners
* [Scala for the Impatient](https://www.amazon.com/-/es/Cay-S-Horstmann-ebook/dp/B01MR67YSO/ref=sr_1_1?__mk_es_US=%C3%85M%C3%85%C5%BD%C3%95%C3%91&dchild=1&keywords=Scala+for+the+Impatient&qid=1587963619&s=books&sr=1-1) por Cay Hortsmann
* [Scala Cookbook](https://www.amazon.com/-/es/Alvin-Alexander-ebook/dp/B00EA66OM8/ref=sr_1_1?__mk_es_US=%C3%85M%C3%85%C5%BD%C3%95%C3%91&dchild=1&keywords=Scala+Cookbook&qid=1587963666&s=books&sr=1-1) por Alvin Alexander
* [Introduction to the Art of Programming Using Scala](https://www.amazon.com/-/es/Mark-C-Lewis-ebook/dp/B00AR3E2ZY/ref=sr_1_1?__mk_es_US=%C3%85M%C3%85%C5%BD%C3%95%C3%91&dchild=1&keywords=Introduction+to+the+Art+of+Programming+Using+Scala&qid=1587963691&s=books&sr=1-1) por Mark C. Lewis
* [Functional Programming in Scala](https://www.amazon.com/-/es/dp/B07MDLD48D/ref=sr_1_1?dchild=1&keywords=Functional+Programming+in+Scala&qid=1587963785&s=books&sr=1-1) por Paul Chiusano and Rúnar Bjarnason

### Referencias y otros links de interés:
* Scala Lang: [https://www.scala-lang.org/](https://www.scala-lang.org/)
* ScalaDays Blog: [https://blog.scaladays.org/](https://blog.scaladays.org/)
* Scala Times Blog: [https://scalatimes.com/](https://scalatimes.com/)
* SoftwareMill Blog: [https://blog.softwaremill.com/tagged/scala](https://blog.softwaremill.com/tagged/scala)
* Alvin Alexander / Scala: [https://alvinalexander.com/scala](https://alvinalexander.com/scala)
* Fragmentos del código: [Post1Intro.scala](https://github.com/rodobarcaaa/scala-blog-snippets/blob/main/src/main/scala/com/rodobarcaaa/Post1Intro.scala)
