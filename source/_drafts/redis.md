`GETSET`（设置新值返回旧值）
可用做计数器，隔一段时间读取key的值，并将其设为0
`MGET`（获取多个key对应的值，返回一个list，未找到的返回nil）
redis的List是`Linked List`，两头增加、删除。
最新数据，可以`LPUSH`到list，再使用`LRANGE`取出
使用`LTRIM`可以定长List，即让List保持一定的长度
List上的阻塞操作：
问题：consumer消费空list，需要polling这个list来拿到数据
解决方案：使用`BRPOP`，`BLPOP`，阻塞拿消费list（阻塞POP操作，直到有数据才返回），设置timeout。
`BRPOP`注意点：
1. 被阻塞的clients是有顺序的，当list有数据是，按照这个顺序，消费list
2. 返回值和`RPOP`不同，返回两个元素的数组，该数组包含key的名称，因为`BRPOP`，`BLPOP`可以阻塞等待多个list。
3. timeout触发时，返回NULL
原子地操作两个list使用`RPOPLPUSH/BRPOPLPUSH src dest`，返回值是被pop的元素，该操作可以构成一个循环list，当src和dest相同的时候
