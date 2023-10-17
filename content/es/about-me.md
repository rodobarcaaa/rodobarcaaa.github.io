---
title: Sobre mi 
subtitle: Rodolfo Echemendía Quintana 
comments: false
---

### Resumen

Ingeniero de software con amplia experiencia trabajando en la industria de la tecnología. Con habilidades para
desarrollar productos web, principalmente BackEnd, como servicios web REST y GraphQL, conexiones a bases de datos e
interacción con terceros. Además de ser un lider de equipo con buena administración y habilidades de comunicación.

```scala
def profile = {
  val pronouns     = "He" -> "Him"
  val education    = """👨‍🎓 Computer Science Engineer's (Cuba-UCI / Class of 2011)"""
  val blogUrl      = "https://blog.echemendía.com/"
  val askMeAbout   = ("tech", "webdev", "soccer")
  val technologies = for {
    backend   <- {
      val java            = "My first programming language!"
      val scalaFuture     = List("Play-Framework", "Tapir", "Sangria", "Slick")
      val scalaFunctional = List("Http4s", "Tapir", "Cats", "Cats-Effect", "Monix-Task")
      val python          = "Learning FastApi!⚡"
      val node            = "Learning NestJs!𓃠"
      java +: scalaFuture :+ scalaFunctional :+ python :+ node
    }
    database  <- List("MySQL", "PostgreSQL", "Redis", "MongoDB")
    devops    <- List("Docker", "AWS", "CI/CD", "Github Actions")
  } yield println(List(backend, database, devops).mkString(" - "))
}
```

##### Además puedes ver más acá: [es-online-cv](https://blog.echemendía.com/es-online-cv/)
