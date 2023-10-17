---
title: About me 
subtitle: Rodolfo Echemendía Quintana 
comments: false
---

### Resume

Experienced software engineer working in the technology industry. Skilled in Scala to develop web products, back-end
mainly, such as web services REST and GraphQL, connections to databases, and interaction with third parties. In addition
to being a team/tech leader with good management and interpersonal skills.

```scala
def profile = {
  val pronouns     = "He" -> "Him"
  val education    = """👨‍🎓 Computer Science Engineer's (Cuba-UCI / Class of 2011)"""
  val blogUrl      = "https://blog.rechemendía.com/"
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

##### You can also see here: [online-cv](https://blog.rechemendía.com/online-cv/)
