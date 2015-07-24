
# 偏函数
~~~scala
scala> case class PhoneExt(name: String, ext: Int)
defined class PhoneExt

scala> val extensions = List(PhoneExt("steve", 100), PhoneExt("robey", 200))
extensions: List[PhoneExt] = List(PhoneExt(steve,100), PhoneExt(robey,200))

scala> extensions.filter { case PhoneExt(name, extension) => extension < 200 }
res0: List[PhoneExt] = List(PhoneExt(steve,100))
这里filter后面的部分其实是个PartialFunction!!
filter(p: (A) ⇒ Boolean)，而PartialFunction是Function的子类型。

scala> List(2).map{case 2 => "ok"}
res2: List[String] = List(ok)

scala> val p: PartialFunction[Int, String] = {case 2 => "Ok"}
scala> List(2).map p
<console>:8: error: missing arguments for method map in class List;
follow this method with `_' if you want to treat it as a partially applied function
              List(2).map p

scala> List(2).map(p)
scala> List(2) map p
~~~
# `implicit`示例
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
