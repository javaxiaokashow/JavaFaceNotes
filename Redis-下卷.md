## Redis

#### 1.查看配置语法

```
CONFIG GET CONFIG_SETTING_NAME
```

#### 2.获取所有配置项

```
127.0.0.1:6379> config get *
  1) "dbfilename"
  2) "dump.rdb"
  3) "requirepass"
  4) ""
  5) "masterauth"
  6) ""
  7) "cluster-announce-ip"
  8) ""
  9) "unixsocket"
 10) ""
 11) "logfile"
 12) ""
...
```

#### 3.设置字符串

```
127.0.0.1:6379> set xk www.javaxks.com
OK
```

#### 4.获取字符串

```
127.0.0.1:6379> get xk
"www.javaxks.com"
```

#### 5.获取随机key

```
127.0.0.1:6379> randomkey
"xk"
```

#### 6.获取key存储的类型

```
127.0.0.1:6379> randomkey
"xk"
```

#### 7.判断key是否存在

```
127.0.0.1:6379> randomkey
"xk"
```

#### 8.修改key的名称

```
127.0.0.1:6379> randomkey
"xk"
```

#### 9.返回key存储的字符串的长度

```
127.0.0.1:6379> randomkey
"xk"
```

#### 10.同时设置多个kv对

```
127.0.0.1:6379> mset date 5.21 mood haha
OK
```

#### 11.获取多个key的值

```
127.0.0.1:6379> mget date mood
1) "5.21"
2) "haha"
```

#### 12.设置key 10秒后过期

```
127.0.0.1:6379> expire date 10
(integer) 1
```

#### 13.查看当前还剩几秒过期

```
127.0.0.1:6379> ttl date
(integer) 4
```

#### 14.过期后获取key

```
127.0.0.1:6379> get date
(nil)
```

#### 15.列表最多可以存多少元素

2的32次方 - 1 

#### 16.从左向列表中添加元素

```
127.0.0.1:6379> lpush fruit apple
(integer) 1
127.0.0.1:6379> lpush fruit strawberry
(integer) 2
127.0.0.1:6379> lpush fruit pear
(integer) 3
127.0.0.1:6379> lpush fruit orange
(integer) 4
```

#### 17.获取列表中所有的元素

```
127.0.0.1:6379> lrange fruit 0 -1
1) "orange"
2) "pear"
3) "strawberry"
4) "apple"
```

#### 18.从左弹出元素

```
127.0.0.1:6379> lpop fruit
"orange"
127.0.0.1:6379> lrange fruit 0 -1
1) "pear"
2) "strawberry"
3) "apple"
```

#### 19.获取指定位置的元素

```
127.0.0.1:6379> lindex fruit 2
"apple"
```

#### 20.获取列表长度

```
127.0.0.1:6379> llen fruit
(integer) 3
```

#### 21.集合

set是string类型的无序集合，集合中的元素都是唯一的。

#### 22.向集合中添加元素

```
127.0.0.1:6379> sadd books java
(integer) 1
127.0.0.1:6379> sadd books python
(integer) 1
127.0.0.1:6379> sadd books php
(integer) 1
127.0.0.1:6379> sadd books go
(integer) 1
127.0.0.1:6379> sadd books ruby
(integer) 1
```

#### 23.获取集合中的元素

```
127.0.0.1:6379> smembers books
1) "go"
2) "python"
3) "java"
4) "php"
5) "ruby"
```

#### 24.删除集合中的指定元素

```
127.0.0.1:6379> srem books ruby
(integer) 1
127.0.0.1:6379> smembers books
1) "php"
2) "java"
3) "python"
4) "go"
```

#### 25.zset类型

有序的集合

元素为string类型

元素唯一，不重复。每个元素有自己的权重。

#### 26.向有序集合中添加元素

```
127.0.0.1:6379> zadd animal 1 monkey
(integer) 1
127.0.0.1:6379> zadd animal 2 chicken
(integer) 1
127.0.0.1:6379> zadd animal 3 mouse
(integer) 1
127.0.0.1:6379> zadd animal 4 horse
(integer) 1
```

#### 27.获取有序集合元素个数

```
127.0.0.1:6379> zcard animal
(integer) 4
```

#### 28.获取所有元素

```
127.0.0.1:6379> zrange animal 0 -1
1) "monkey"
2) "chicken"
3) "mouse"
4) "horse"
```

#### 29.获取所有元素并带上分值

```
127.0.0.1:6379> zrange animal 0 -1 withscores
1) "monkey"
2) "1"
3) "chicken"
4) "2"
5) "mouse"
6) "3"
7) "horse"
8) "4"
```

#### 30.HyperLogLog

Redis 在 2.8.9 版本添加了 HyperLogLog 结构。

Redis HyperLogLog 是用来做基数统计的算法，HyperLogLog 的优点是，在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定 的、并且是很小的。

在 Redis 里面，每个 HyperLogLog 键只需要花费 12 KB 内存，就可以计算接近 2^64 个不同元素的基 数。这和计算基数时，元素越多耗费内存就越多的集合形成鲜明对比。

但是，因为 HyperLogLog 只会根据输入元素来计算基数，而不会储存输入元素本身，所以 HyperLogLog 不能像集合那样，返回输入的各个元素。

#### 31.向HyperLogLog中添加元素

```
127.0.0.1:6379> pfadd color blue
(integer) 1
127.0.0.1:6379> pfadd color yellow
(integer) 1
127.0.0.1:6379> pfadd color pink
(integer) 1
127.0.0.1:6379> pfadd color white
(integer) 1
127.0.0.1:6379> pfadd color black
(integer) 1
```

#### 32.获取HyperLogLog元素个数

```
127.0.0.1:6379> pfcount color
(integer) 5
```

#### 33.发布订阅

种消息通信模式：发送者 (pub) 发送消息，订阅者 (sub) 接收消息。

Redis 客户端可以订阅任意数量的频道。

#### 34.创建订阅频道

```
127.0.0.1:6379> subscribe everydayNews
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "everydayNews"
3) (integer) 1
1) "message"
```

#### 35.在对应频道发布消息

```
127.0.0.1:6379> publish everydayNews "morning"
(integer) 1
```

订阅者收到:

```
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "everydayNews"
3) (integer) 1
1) "message"
2) "everydayNews"
3) "morning"
```

#### 36.非后台执行备份

```
127.0.0.1:6379> save
OK
```

#### 37.后台执行备份

```
127.0.0.1:6379> BGSAVE
Background saving started
```

#### 38.查看是否需要密码验证

```
127.0.0.1:6379> config get requirepass
1) "requirepass"
2) ""
```



#### 参考：

https://redis.io/

http://redisdoc.com/

https://www.runoob.com/redis/

![WechatIMG360](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/common1.png)