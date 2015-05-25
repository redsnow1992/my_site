title: 优先级和lisp式前缀表达式
categories: programming
tags: scheme
mathjax: true
---
~~~ruby
if(f(a,b) > 0)    # 1
if(f(a, b>0))     # 2
~~~
上面在调用`f`的时候，1使用括号表明了参数的优先级，不然会出现2这样的情况。
而使用lisp式的前缀表达式则不会有不明确的情况，因为优先级已经暗含在里面了。

~~~scheme
(if (> (f a b) 0)
   ;; then-body
   ;; else-body)
~~~

$$ x^2+y^2 = z^2 $$

Simple inline $a = b + c$.

{% codeblock lang:objc %}
[rectangle setX: 10 y: 10 width: 20 height: 20];
{% endcodeblock %}
