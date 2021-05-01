---
title: Sobre mi
subtitle: Rodolfo Echemendía Quintana
comments: false
---

```scala
def perfil(): Unit = {
  val educación    = """👨‍🎓 Ingeniero en Ciencias Informáticas (Cuba-UCI / Graduación 2011)"""
  val preguntameSobre   = ("tecnología", "desarrollo web", "futbol")
  val tecnologías = for {
    backEnd   <- {
      val scala  = List("Play", "Slick", "Tapir", "Sangria", "Cats")
      val python = "Learning FastApi"
      scala :+ python
    }
    databases <- List("MySQL", "PostgreSQL", "Redis", "MongoDB")
    devOps    <- List("Docker", "AWS", "CI/CD", "Github Actions")
    frontEnd  <- List("HTML", "CSS", "JS", "TS", "Learning ReactJS")
  } yield println(List(backEnd, databases, devOps, frontEnd).mkString(", "))
}
```

### Resume

Ingeniero de software experimentado que trabaja en la industria de la tecnología. Experto en **Scala** para desarrollar productos web,
back-end principalmente, como servicios web **REST** y **GraphQL**, conexiones a bases de datos e interacción con terceros.
Además de ser un líder de equipo con buena gestión y habilidades interpersonales, actualmente a cargo de la planificación de proyectos
y adopción de nuevas tecnologías del equipo de [Travelonux](https://www.travelonux.com/).

#### Además puedes ver más acá: [es-online-cv](https://rodobarcaaa.github.io/es-online-cv/)
