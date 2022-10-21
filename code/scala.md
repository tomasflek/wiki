# Scala

### Interface

``` scala
trait TailWagger {
    def startTail(): Unit
    def stopTail(): Unit
}
```
interface implementation
``` scala
class Dog extends TailWagger {
    // the implemented methods
    def startTail(): Unit = println("tail is wagging")
    def stopTail(): Unit = println("tail is stopped")
}
```

### `val` vs `var`  

`val` is ummutable object
`var` is muttable object

### `case class` vs `class`

### [companion objects](https://docs.scala-lang.org/overviews/scala-book/companion-objects.html)

``` scala
class Pizza {
}

object Pizza {
}
```
