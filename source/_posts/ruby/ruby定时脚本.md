title: ruby定时脚本
date: 2015/05/11 17:56
updated: 2015/05/11 17:56
categories: programming
tags: ruby
---
ruby定时脚本的实现涉及到三个方面：
1. 要定时执行的代码
2. 定时控制（设置定时的时间）
3. 将脚本后台化

实例：

~~~ruby
# in  func.rb
def func
  # the function body goes here
end

# in scheduler.rb
# require func.rb
require 'rufus/scheduler' # we use this gem
s = Rufus::Scheduler.new
s.cron '3 3 * * *' do
  begin
     func  # call func
  rescue Exception => e
     # log error
  end
end

s.join

# in schedulerd.rb
require 'daemons'  # we use this gem 
Daemons.run(File.expand_path("scheduler.rb", __FILE__))

# start daemon
ruby schedulerd.rb start
~~~
其中会用到rufus，daemons这两个gem，更多的使用方法可以在各自的github上看到。