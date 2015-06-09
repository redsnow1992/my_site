title: sequel misc
categories: programming
tags: ruby
---

ds.escape_like("foo\\%_") # 'foo\\\%\_'

def escape_like(string)
  string.gsub(/[\\%_]/){|m| "\\#{m}"}
end