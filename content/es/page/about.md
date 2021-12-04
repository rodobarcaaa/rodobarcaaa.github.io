---
title: Sobre mi
subtitle: Rodolfo Echemendía Quintana
comments: false
---

```scala
def profile = {
  val pronouns     = "He" -> "Him"
  val education    = """👨‍🎓 Computer Science Engineer's (Cuba-UCI / Class of 2011)"""
  val blogUrl      = "https://rodobarcaaa.github.io/"
  val askMeAbout   = ("tech", "webdev", "soccer")
  val technologies = for {
    backend   <- {
      val java   = "My first programming language!"
      val scala  = List("Play", "Slick", "Tapir", "Sangria", "Cats")
      val python = "Learning FastApi!⚡"
      val node   = "Learning NestJs!"
      java +: scala :+ python :+ node
    }
    database  <- List("MySQL", "PostgreSQL", "Redis", "MongoDB")
    devops    <- List("Docker", "AWS", "CI/CD", "Github Actions")
  } yield println(List(backend, database, devops).mkString(" - "))
}
```

### Resumen

Ingeniero de software con amplia experiencia trabajando en la industria de la tecnología. Con habilidades para desarrollar productos web, principalmente BackEnd, como servicios web REST y GraphQL, conexiones a bases de datos e interacción con terceros. Además de ser un lider de equipo con buena administración y habilidades de comunicación.

##### Además puedes ver más acá: [es-online-cv](https://rodobarcaaa.github.io/es-online-cv/)
