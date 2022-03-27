# Widgets

```scala mdoc:js
import calico.*
import calico.dsl.io.*
import calico.syntax.*
import calico.widget.*
import cats.effect.*
import cats.effect.syntax.all.*
import cats.effect.unsafe.implicits.*
import fs2.*
import fs2.concurrent.*

case class Person(firstName: String, lastName: String, age: Int)

val app = SignallingRef[IO].of(Person("", "", 0)).toResource.flatMap { personRef =>
  personRef.discrete.renderableSignal.flatMap { personSig =>
    div(
      div(h3("View"), Widget.view(personSig.discrete)),
      div(
        h3("Edit"),
        Widget.edit(personSig.discrete)(_.foreach(personRef.set(_)))
      )
    )
  }
}

app.renderInto(node).allocated.unsafeRunAndForget()
```