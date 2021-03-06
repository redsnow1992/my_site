title: ruby Time 毫秒
date: 2015/05/26 17:06
updated: 2015/05/26 17:06
categories: programming
tags: ruby
---
今天和java那边对接遇到了一个问题：
我这边需要传一个url参数”timestamp=当前Unix毫秒数时间戳”，而我使用的代码是：`timestamp=Time.now.to_i`。
当两个请求在同一秒内的时候，`timestamp`的值是相同的，所以传值会有问题，而且我这个timestamp生成的其实是“当前Unix秒数时间戳”。
以前没注意到“毫秒时间戳”这个问题，而java那边多用`System.currentTimeMillis()`这样的代码，他们比较关心这个。

那么把代码改成如下是否就可以了呢？    
~~~ruby
timestamp = Time.now.to_f*1000.to_i
~~~
还是有问题，`to_i`应该作用在整个结果上，当代码写得比较密的时候，这个优先级很难看出来。
而写成这样就比较清晰了。   
~~~ruby
timestamp = Time.now.to_f * 1000.to_i

# 正确版
timestamp = (Time.now.to_f * 1000).to_i

t = Time.now  #=> 2015-05-26 17:10:04 +0800
t.to_i => 1432631404
t.to_f => 1432631404.1833367
~~~

