title: sequel注意点
categories: programming
tags: 
- ruby
- sequel
---
# sequel关联对象的更新问题
数据库中的对象 vs 内存中的对象(正确性 vs 效率)

~~~ruby
# in controller
@basic_content.update(name: params[:name])
@copyright.to_topic_hash

# in model Copyright
class Copyright
  many_to_one :basic_content

  def after_save
    bc1 = BasicContent.with_pk!(self.basic_content_id) # 数据库中的对象
    bc2 = self.basic_content # 内存中的对象
    # bc1和bc2并不完全相同
  end
end
~~~
# sequel uses raw sql
insert example:
~~~ruby
# generate sql
sql = "insert into tb_test (#{table.columns.join(',')}) values "
sql += records.map{|r| "(" + %Q[value1, null, "string-value", now(), "#{Time.now.strftime("%F %T")}"] + ")"}.join(",")
DB[sql].insert
~~~
+ 在插入`datetime`类型时，有些mysql版本不支持这样的值`"#{Time.now}"`(`2015-07-09 11:49:41 +0800`这里的"+8000"mysql无法解析)，这里统一转化一下(`strftime("%F %T")`)。
+ 插入`null`和`now()`时不需要用括号引起来。
