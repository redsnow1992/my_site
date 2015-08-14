foldLeft vs reduce
# chapter 1
you can think of this as an overloaded form of () operator
# chapter 2
+ An `if` expression has a value
+ A block has a value - the value of its last expression
+ The Scala `for` loop is like an **enhanced** Java `for` loop
+ Semicolons are (mostly) optional
+ The `void` type is `Unit`
+ Avoid using `return` in a function
+ Beware of missing `=` in a function definition
+ Exception work just like in Java or C++, but you use a **pattern matching** syntax for `catch` 
+ Scala has no checked exceptions

`pattern matching` is a more powerful `switch` like in Java or C++

| syntax | Scala | Java or C++|
| --- | --- | --- |
| empty type | `Unit` or `()` | `void` |
| branch switch | pattern match | `switch` |
| block expression | { expressions } | { statements } |
| assignment | return Unit | the value that is being assigned |

## For Comprehensions
~~~scala
for (i <- 1 to 3; j <- 1 to 3 if i != j) {
  print((10 * i + j) + " ")
}
~~~
**`if`之前并没有`;`**

~~~scala
for (i <- 1 to 3; from = 4 - i; j <- from to 3) {
  print((10 * i + j) + " ")
}
~~~
`for`中定义变量

## Function
`method`作用在`object`上，而`function`却不是。      
`function`中不必显式`return`，匿名函数中的`return`更复杂，它将会`break`到外面的调用函数。
### 可变参数
~~~scala
def sum(args: Int*) = {
  var result = 0
  for (arg <- args.elements) result += arg
  result
}
// 调用方式
sum(1, 4, 9, 10)
sum(1 to 5: _*)
~~~
**Caution**   
当调用java的可变参数函数时，我们需要手动转化成基础类型：    
~~~scala
val str = MessageFormat.format("The answer to {0} is {1}", "everything", 42.asInstanceOf[AnyRef])
~~~
## Procedures
当一个函数体以大括号包裹，并且前面没有 `=` ，那么返回值类型就是 `Unit`。这样的函数称为 `procedure`。
当且仅当你需要副作用的时候，才调用一个 `procedure`。
