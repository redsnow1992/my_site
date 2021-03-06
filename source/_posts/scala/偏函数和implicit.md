title: 偏函数和implicit
date: 2015/07/29 16:01
categories: programming
tags: 
- scala
- implicit
---
# 偏函数
## 示例
偏函数不同于currying，它表示这个函数只对某个类型下特定的值集有定义，例如：
~~~scala
val isEven: PartialFunction[Int, String] = {
  case x if x % 2 == 0 => x+" is even"
}
~~~
以上的`isEven`函数只对偶数的`Int`有定义，当以`isEven(9)`调用时会报错，我们可以使用`isDefinedAt`验证是否可以调用该函数。
~~~
if(isEven.isDefinedAt(9))  // =>  false
  isEven(9)
~~~
## case与偏函数
~~~scala
case class PhoneExt(name: String, ext: Int)
val extensions = List(PhoneExt("steve", 100), PhoneExt("robey", 200))
extensions.filter { case PhoneExt(name, extension) => extension < 200 }
~~~
这里`filter`后面的部分其实是个`PartialFunction`!!
`filter(p: (A) ⇒ Boolean)`，而`PartialFunction`是Function的子类型。
**与中缀表达式结合**
~~~scala
List(2).map{case 2 => "ok"}

val p: PartialFunction[Int, String] = {case 2 => "Ok"}
List(2).map p
<console>:8: error: missing arguments for method map in class List;
follow this method with `_' if you want to treat it as a partially applied function
              List(2).map p

List(2).map(p)  // ok
List(2) map p   // ok
~~~
# [`implicit`示例](http://docs.scala-lang.org/tutorials/FAQ/finding-implicits.html)
## 隐式参数类型约束
~~~scala
import scala.math._
def foo[T](t: T)(implicit integral: Integral[T])
  {println(integral)}
foo(0)  //=> scala.math.Numeric$IntIsIntegral$@53b97d73
foo(0L) //=> scala.math.Numeric$LongIsIntegral$@550571f6
foo(0.09)  //=> could not find implicit value for parameter integral: scala.math.Integral[Double]
~~~
## 隐式参数类型转换
~~~scala
"abc".map(_.toInt)
String => StringOps
~~~
**结合两者**   
~~~scala
def getIndex[T, CC](seq: CC, value: T)(implicit conv: CC => Seq[T]) = 
  seq.indexOf(value)

getIndex("abc", 'a')  //=> 0
getIndex("abc", "a")  //=> No implicit view available from String => Seq[String]

// 语法糖写法
def getIndex[T, CC <% Seq[T]](seq: CC, value: T) = seq.indexOf(value)
~~~
## 上下文约束(Context Bounds)
隐式参数的另一个常见模式是*type class pattern*。该模式可以给没有声明它们的那些类提供一个统一的接口。
示例1:
~~~scala
def sum[T](list: List[T])(implicit integral: Integral[T]): T = {
  import integral._   // get the implicits in question into scope
  list.foldLeft(integral.zero)(_+_)
}

// 语法糖写法
def sum[T: Integral](list: List[T]): T = {
  val integral = implicitly[Integral[T]]
  import integral._   // get the implicits in question into scope
  list.foldLeft(integral.zero)(_+_)
}
~~~
示例2:
~~~scala
def reverseSort[T: Ordering](seq: Seq[T]) = seq.reverse.sorted
~~~
## `Implicits`的查找
1. First look in current scope
   * Implicits defined in current scope
   * Explicit imports
   * wildcard imports
2. Now look at associated types in
   * Companion objects of a type
   * Implicit scope of an argument's type
   * Implicit scope of type arguments
   * Outer objects for nested types

示例:
### Implicits defined in current scope
~~~scala
implicit val n: Int = 5
def add(x: Int)(implicit y: Int) = x + y
add(5)   // takes n from the current scope
~~~
### Explicit imports
~~~scala
import scala.collection.JavaConversions.mapAsScalaMap
def env = System.getenv()  // java map
def term = env("TERM")  // implicit conversion from Java Map to Scala Map
// Java Map not support such operations
~~~
### wildcard imports
~~~scala
def sum[T : Integral](list: List[T]): T = {
  val integral = implicitly[Integral[T]]
  import integral._   // get the implicits in question into scope
  list.foldLeft(integral.zero)(_ + _)
}
~~~
### Companion objects of a type
~~~scala
for{
  x <- List(1, 2, 3)
  y <- Some('x')
} yield (x, y)
~~~
以上表达式将会编译成：
~~~scala
List(1, 2, 3).flatMap(x => Some('x').map(y => (x, y)))
//=> List[(Int, Char)] = List((1,x), (2,x), (3,x))
~~~
`List.flatMap(f: (A) => GenTraversableOnce[B])`需要一个`TraversableOnce`，但`Option`却不是，
编译器会察看`Option`的伴生对象并找到一个向`Iterable`的转换。`Option.map`生成的是一个`Option`，并不是一个`TraversableOnce`。
在文档的`Type Hierarchy`里可以看到`Option[A] --(implicitly)-->(Iterable[A])`。
需要注意的是超类的伴生对象也会被察看，例如：
~~~scala
class A(val n: Int)
object A {
  implicit def str(a: A): String = "A: %d" format a.n
}
class B(val x: Int, y: Int) extends A(y)
val b = new B(5, 2)
val s: String = b
~~~
这里找到的是`Numeric[Int]`并非`Integral[Int]`。
### Implicit scope of an argument's type
~~~scala
class A(val n: Int) {
  def +(other: A) = new A(n + other.n)
}
object A {
  implicit def fromInt(n: Int) = new A(n)
}
1 + new A(1)
// converted to this:
A.fromInt(1) + new A(1)
~~~
### Implicit scope of type arguments
~~~scala
class A(val n: Int)
object A{
  implicit val ord = new Ordering[A] {
    def compare(x: A, y: A) =
      implicitly[Ordering[Int]].compare(x.n, y.n)
  } 
}
// 以下调用将合法
List(new A(5), new A(2)).sorted
~~~

### Outer objects for nested types
~~~scala
class A(val n: Int) {
  class B(val m: Int) { require(m < n) }
}
object A {
  implicit def bToString(b: A#B) = "B: %d" format b.m
}
val a = new A(5)
val b = new a.B(3)
val s: String = b
~~~


