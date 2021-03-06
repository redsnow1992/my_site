title: scala-杂项
date: 2015/05/15 14:32
updated: 2015/05/15 14:32
categories: programming
tags: scala
---
## 特殊之处
In Scala assignment always results in the unit value, ()

```scala
var a = 1
(a = 2).getClass     //=> Class[Unit] = void
```

## 读取文件
```scala
import scala.io.Source
if(args.length > 0){
  for(line <- Source.fromFile(args(0)).getLines())
    println(line.length + " " + line)
}
else{
  Console.err.println("Please enter filename")  
}
```
## Closures
```scala
def inc(more: Int) = (x: Int) => x + more
val inc1 = inc(1)
val inc100 = inc(100)
```
## Special function call forms
### Repeated parameters
```scala
def echo(args: String*) =
  for (arg <- args) println(arg)
echo: (args: String*)Unit
echo("first", "second")
val arr = Array("What's", "up", "doc?")
echo(arr: _*)
```

### Named arguments
```scala
def speed(distance: Float, time: Float): Float =
  distance/time
speed(time = 10, distance = 100)  // diff in invoking
```

### Default parameter values
```scala
def printTime(out: java.io.PrintStream = Console.out) =
  out.println("time = " + System.currentTimeMillis())
def printTime2(out: java.io.PrintStream = Console.out, divisor: Int = 1) =
  out.println("time = " + System.currentTimeMillis()/divisor)

printTime2(out = Console.err)
printTime2(divisor = 1000)
```

### High-order function
```scala
def filesMatching(matcher: String => Boolean) =
  for(file <- filesHere; if matcher(file.getName))
  yield file

def filesEnding(query: String) =
  filesMatching(_.endsWith(query))
```
## Case
### [for 语句中的模式](http://windor.gitbooks.io/beginners-guide-to-scala/content/chp3-pattern-everywhere.html)
模式在 `for` 语句中也非常重要。 所有能在值定义的左侧使用的模式都适用于 `for` 语句的值定义。 因此，如果我们有一个球员得分集，想确定谁能进名人堂（得分超过一定上限）， 用 `for` 语句就可以解决：
`for`中隐含着`case`。
```scala
def gameResults(): Seq[(String, Int)] =
  ("Daniel", 3500) :: ("Melissa", 13000) :: ("John", 7000) :: Nil
def hallOfFame = for {
    result <- gameResults()
    (name, score) = result
    if (score > 5000)
  } yield name
```

### [模式匹配与匿名函数](http://windor.gitbooks.io/beginners-guide-to-scala/content/chp4-pattern-matching-anonymous-functions.html)
```scala
val wordFrequencies = ("habitual", 6) :: ("and", 56) :: ("consuetudinary", 2) ::
  ("additionally", 27) :: ("homely", 5) :: ("society", 13) :: Nil
wordFrequencies.filter(wf => wf._2 > 3 && wf._2 < 25).map(_._1)
// => List(habitual, homely, society)
changed to:  wordFrequencies.filter{case (_, i) => i > 3 && i < 25}.map{case (n, _) => n}
if we use:
wordFrequencies.filter(case (_, i) => i > 30)
error: illegal start of simple expression  //  error !!!
```

**PartialFunction**

```scala
val pf = new PartialFunction[(String, Int), String] {
  def apply(wordFrequency: (String, Int)) = wordFrequency match {
    case (word, freq) if freq > 3 && freq < 25 => word
  }
  def isDefinedAt(wordFrequency: (String, Int)) = wordFrequency match {
    case (word, freq) if freq > 3 && freq < 25 => true
    case _ => false
  }
}

wordFrequencies.map(pf) // will throw a MatchError
wordFrequencies.collect(pf)  // right
```

## [类型 Option](http://windor.gitbooks.io/beginners-guide-to-scala/content/chp5-the-option-type.html)
`Option[A]` 是一个类型为 A 的可选值的容器： 如果值存在， `Option[A]` 就是一个 `Some[A]` ，如果不存在， `Option[A]` 就是对象 `None` 。
在类型层面上指出一个值是否存在，使用你的代码的开发者（也包括你自己）就会被编译器强制去处理这种可能性， 而不能依赖值存在的偶然性。
`Option` 是强制的！不要使用 `null` 来表示一个值是缺失的。
The most idiomatic way to use an `scala.Option` instance is to treat it as a collection or monad and use `map`,`flatMap`, `filter`, or `foreach`.
```scala
val greeting: Option[String] = Some("Hello world")
val greeting: Option[String] = None
```

在实际工作中，你不可避免的要去操作一些 Java 库， 或者是其他将 null 作为缺失值的JVM 语言的代码。 为此， `Option` 伴生对象提供了一个工厂方法，可以根据给定的参数创建相应的 `Option` ：

```scala
val absentGreeting: Option[String] = Option(null) // absentGreeting will be None
val presentGreeting: Option[String] = Option("Hello!") // presentGreeting will be Some("Hello!")
```

```scala
case class User(
    id: Int,
    firstName: String,
    lastName: String,
    age: Int,
    gender: Option[String]
  )

val user = User(2, "Johanna", "Doe", 30, None)
println("Gender: " + user.gender.getOrElse("not specified")) // will print "not specified"

val user = User(2, "Johanna", "Doe", 30, None)
user.gender match {
  case Some(gender) => println("Gender: " + gender)
  case None => println("Gender: not specified")
}
```
##  [Try 与错误处理](http://windor.gitbooks.io/beginners-guide-to-scala/content/chp6-error-handling-with-try.html)
### Try 的语义
`Option[A]` 是一个可能有值也可能没值的容器， `Try[A]` 则表示一种计算： 这种计算在成功的情况下，返回类型为 `A` 的值，在出错的情况下，返回 `Throwable` 。 这种可以容纳错误的容器可以很轻易的在并发执行的程序之间传递。

`Try` 有两个子类型：
1. `Success[A]`：代表成功的计算。
2. 封装了 `Throwable` 的 `Failure[A]`：代表出了错的计算。

```scala
import scala.util.Try
import java.net.URL
def parseURL(url: String): Try[URL] = Try(new URL(url))
```

`flatMap`将最后生成的嵌套值进行flat（多见于Option, Try）。
`Option[Option[String]]` => `Option[String]`

**Catching exceptions and `finally` clause**

~~~scala
import java.io.FileReader
import java.io.FileNotFoundException
import java.io.IOException

try {
  val f = new FileReader("input.txt")
} catch {
  case ex: FileNotfoundexception => // 
  case ex: IOException =>   // 
} finally {
  f.close()
}
~~~

既然 `Try` 支持 `flatMap` 、 `map` 、 `filter` ，能够使用 `for` 语句也是理所当然的事情， 而且这种情况下的代码更可读。 
```scala
import scala.io.Source
def getURLContent(url: String): Try[Iterator[String]] =
  for {
   url <- parseURL(url)
   connection <- Try(url.openConnection())
   is <- Try(connection.getInputStream)
   source = Source.fromInputStream(is)
  } yield source.getLines()
```
