title: sequel注意点.md
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


