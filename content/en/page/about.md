---
title: About me
subtitle: Rodolfo Echemendía Quintana
comments: false
---

```scala
def profile(): Unit = {
  val education    = """👨‍🎓 Computer Science Engineer's (Cuba-UCI / Class of 2011)"""
  val askMeAbout   = ("tech", "webdev", "soccer")
  val technologies = for {
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

Experienced software engineer working in the technology industry. Skilled in **Scala** to develop web products, back-end mainly, such as web services **REST** and **GraphQL**, connections to databases, and interaction with third parties. In addition to being a team leader with good management and interpersonal skills, currently in charge of project planning and adoption of new technologies of the [Travelonux](https://www.travelonux.com/) team.

##### You can also see here: [online-cv](https://rodobarcaaa.github.io/online-cv/)
