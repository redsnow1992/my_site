title: implicit笔记（一般常识）
date: 2015/10/20 19:01
categories: programming
tags: 
- scala
- implicit
---
本篇内容摘自<Scala for the Impatient>的第21章。
# `implicit`的使用都在以下几处：
## 1. 类型之间的相互转换
~~~scala
implicit def int2Fraction(n: Int) = Fraction(n, 1)
val result = 3 * Fraction(4, 5) // Calls int2Fraction(3
~~~
可以用于扩展已有类：
~~~scala
val contents = new File("README").read
class RichFile(val from: File) {
  def read = Source.fromFile(from.getPath).mkString
}
implicit def file2RichFile(from: File) = new RichFile(from)
~~~

## 2. 作为隐式参数，传给函数
~~~scala
case class Delimiters(left: String, right: String)

def quote(what: String)(implicit delims: Delimiters) =
  delims.left + what + delims.right
~~~
`quote`的调用（使用显式参数）：
~~~scala
quote("Bonjour le monde")(Delimiters("«", "»"))
~~~
若要使用如下形式的隐式调用，则需要提供一个隐式转换，示例如下：
~~~scala
import FrenchPunctuation.quoteDelimiters
// or import FrenchPunctuation._

quote("Bonjour le monde")
~~~
~~~scala
object FrenchPunctuation {
  implicit val quoteDelimiters = Delimiters("«", "»")
  ...
}
~~~
上例中可以看出，`quote`的隐式参数可以是多个参数，以下代码虽然可以执行，但最好不要给隐式参数使用相同的类型。
~~~scala
def quote(what: String)(implicit left: String, right: String) // No!
implicit def test2String = ","
~~~

## 3. 隐式参数的隐式转换
~~~scala
def smaller[T](a: T, b: T) = if (a < b) a else b
~~~
以上代码并不能编译通过，因为编译器不能确定`T`是不是支持`<`这个运算符。
一般的隐式转换：
~~~scala
def smaller[T](a: T, b: T)(implicit order: T => Ordered[T])
  = if (order(a) < b) a else b
~~~
更简洁的隐式转换：
~~~scala
def smaller[T](a: T, b: T)(implicit order: T => Ordered[T])
  = if (a < b) a else b
~~~

## 上下文边界
在类定义中，声明一个上下文边界`T:M`就表示在当前的scope中存在一个隐式值`M[T]`，所以的当要使用这种形式的定义是，要确定有这样一个隐式值（若没有编译器会报错）。
示例：
~~~scala
class Pair[T : Ordering](val first: T, val second: T) {
  def smaller(implicit ord: Ordering[T]) =
    if (ord.compare(first, second) < 0) first else second
}
~~~
在 *scala/src/library/scala/math/Ordering.scala* 中如下代码保证了`Ordering[Int]`：
~~~scala
trait IntOrdering extends Ordering[Int] {
    def compare(x: Int, y: Int) =
      if (x < y) -1
      else if (x == y) 0
      else 1
  }
implicit object Int extends IntOrdering
~~~
`implicitly`的使用可以可以拿到ordering，代码如下：
~~~scala
class Pair[T : Ordering](val first: T, val second: T) {
  def smaller =
    if (implicitly[Ordering[T]].compare(first, second) < 0) first else second
}
~~~
`implicitly`的源码在Predef.scala中：
~~~scala
def implicitly[T](implicit e: T) = e
  // For summoning implicit values from the nether world
~~~
让我们更进一步，将ordering的部分，使用运算符替代：
~~~scala
class Pair[T : Ordering](val first: T, val second: T) {
  def smaller = {
    import Ordered._  // 引入Ordering到Ordered的隐式转换
    if (first < second) first else second
  }
}
~~~
自定义一个`implicit value`：
~~~scala
implicit object PointOrdering extends Ordering[Point] {
  def compare(a: Point, b: Point) = ...
}
~~~

# 有关implicit的一些tips
1. 在REPL中，键入`:implicits`可以看到所有不是从`Predef`引入的implicit。若要看所有的implicit（包含`Predef`中的implicit），则可以使用`:implicits -v`
2. 若想察看，编译器为什么没有使用你想的那个隐式转换，可以**显式**调用隐式转换的函数。例如：`fraction2Double(3) * Fraction(4, 5)`，看是否会报错。
3. 如果想察看某个scala代码中使用的隐式转换，可以使用`scalac -Xprint:typer MyProg.scala`看到隐式转换后的代码。

