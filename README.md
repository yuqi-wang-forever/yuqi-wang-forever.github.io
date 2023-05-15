 [PostgreSQL](https://yuqi-wang-forever.github.io/postgres).  
# Redis data types tutorial Redis 数据类型教程

Learning the basic Redis data types and how to use them  
学习基本的 Redis 数据类型以及如何使用它们

The following is a hands-on tutorial that teaches the core Redis data types using the Redis CLI. For a general overview of the data types, see the [data types introduction](https://redis.io/docs/data-types/).  
以下是使用 Redis CLI 教授核心 Redis 数据类型的实践教程。有关数据类型的一般概述，请参阅数据类型介绍。

## Keys

Redis keys are binary safe, this means that you can use any binary sequence as a key, from a string like "foo" to the content of a JPEG file. The empty string is also a valid key.  
Redis 密钥是二进制安全的，这意味着您可以使用任何二进制序列作为密钥，从“foo”这样的字符串到 JPEG 文件的内容。空字符串也是一个有效的键。

A few other rules about keys:  
关于键的其他一些规则：

-   Very long keys are not a good idea. For instance a key of 1024 bytes is a bad idea not only memory-wise, but also because the lookup of the key in the dataset may require several costly key-comparisons. Even when the task at hand is to match the existence of a large value, hashing it (for example with SHA1) is a better idea, especially from the perspective of memory and bandwidth.  
    很长的键不是一个好主意。例如，1024 字节的键不仅在内存方面是个坏主意，而且因为在数据集中查找键可能需要多次代价高昂的键比较。即使手头的任务是匹配一个大值的存在，对其进行哈希处理（例如使用 SHA1）也是一个更好的主意，尤其是从内存和带宽的角度来看。
-   Very short keys are often not a good idea. There is little point in writing "u1000flw" as a key if you can instead write "user:1000:followers". The latter is more readable and the added space is minor compared to the space used by the key object itself and the value object. While short keys will obviously consume a bit less memory, your job is to find the right balance.  
    非常短的键通常不是一个好主意。如果您可以写成“user:1000:followers”，那么将“u1000flw”写成键就没什么意义了。后者更具可读性，与键对象本身和值对象使用的空间相比，增加的空间较小。虽然短键显然会消耗更少的内存，但您的工作是找到合适的平衡点。
-   Try to stick with a schema. For instance "object-type:id" is a good idea, as in "user:1000". Dots or dashes are often used for multi-word fields, as in "comment:4321:reply.to" or "comment:4321:reply-to".  
    尝试坚持模式。例如“object-type:id”是个好主意，如“user:1000”。点或破折号通常用于多词字段，如“comment:4321:reply.to”或“comment:4321:reply-to”。
-   The maximum allowed key size is 512 MB.  
    允许的最大密钥大小为 512 MB。

## Strings

The Redis String type is the simplest type of value you can associate with a Redis key. It is the only data type in Memcached, so it is also very natural for newcomers to use it in Redis.  
Redis 字符串类型是可以与 Redis 键相关联的最简单的值类型。它是Memcached中唯一的数据类型，所以新手在Redis中使用它也是很自然的。

Since Redis keys are strings, when we use the string type as a value too, we are mapping a string to another string. The string data type is useful for a number of use cases, like caching HTML fragments or pages.  
由于 Redis 键是字符串，当我们也使用字符串类型作为值时，我们是将一个字符串映射到另一个字符串。字符串数据类型可用于许多用例，例如缓存 HTML 片段或页面。

Let's play a bit with the string type, using `redis-cli` (all the examples will be performed via `redis-cli` in this tutorial).  
让我们使用 `redis-cli` 来尝试一下字符串类型（本教程中的所有示例都将通过 `redis-cli` 执行）。

```
> set mykey somevalue
OK
> get mykey
"somevalue"
```

As you can see using the [`SET`](https://redis.io/commands/set) and the [`GET`](https://redis.io/commands/get) commands are the way we set and retrieve a string value. Note that [`SET`](https://redis.io/commands/set) will replace any existing value already stored into the key, in the case that the key already exists, even if the key is associated with a non-string value. So [`SET`](https://redis.io/commands/set) performs an assignment.  
如您所见，使用 `SET` 和 `GET` 命令是我们设置和检索字符串值的方式。请注意，如果键已经存在，即使键与非字符串值相关联， `SET` 也会替换已存储到键中的任何现有值。所以 `SET` 执行一个赋值。

Values can be strings (including binary data) of every kind, for instance you can store a jpeg image inside a value. A value can't be bigger than 512 MB.  
值可以是各种类型的字符串（包括二进制数据），例如，您可以在值中存储 jpeg 图像。值不能大于 512 MB。

The [`SET`](https://redis.io/commands/set) command has interesting options, that are provided as additional arguments. For example, I may ask [`SET`](https://redis.io/commands/set) to fail if the key already exists, or the opposite, that it only succeed if the key already exists:  
`SET` 命令有一些有趣的选项，它们作为附加参数提供。例如，如果键已经存在，我可能会要求 `SET` 失败，或者相反，只有当键已经存在时它才会成功：

```
> set mykey newval nx
(nil)
> set mykey newval xx
OK
```

Even if strings are the basic values of Redis, there are interesting operations you can perform with them. For instance, one is atomic increment:  
即使字符串是 Redis 的基本值，您也可以使用它们执行一些有趣的操作。例如，一个是原子增量：

```
> set counter 100
OK
> incr counter
(integer) 101
> incr counter
(integer) 102
> incrby counter 50
(integer) 152
```

The [INCR](https://redis.io/commands/incr) command parses the string value as an integer, increments it by one, and finally sets the obtained value as the new value. There are other similar commands like [INCRBY](https://redis.io/commands/incrby), [DECR](https://redis.io/commands/decr) and [DECRBY](https://redis.io/commands/decrby). Internally it's always the same command, acting in a slightly different way.  
INCR 命令将字符串值解析为整数，将其加一，最后将得到的值设置为新值。还有其他类似的命令，如 INCRBY 、 DECR 和 DECRBY 。在内部它总是相同的命令，以稍微不同的方式起作用。

What does it mean that INCR is atomic? That even multiple clients issuing INCR against the same key will never enter into a race condition. For instance, it will never happen that client 1 reads "10", client 2 reads "10" at the same time, both increment to 11, and set the new value to 11. The final value will always be 12 and the read-increment-set operation is performed while all the other clients are not executing a command at the same time.  
INCR 是原子的是什么意思？即使多个客户端针对同一个密钥发出 INCR，也永远不会进入竞争状态。例如，永远不会发生客户端 1 读取“10”，客户端 2 同时读取“10”，两者都递增到 11，并将新值设置为 11。最终值将始终为 12，并且读取 -在所有其他客户端未同时执行命令时执行增量设置操作。

There are a number of commands for operating on strings. For example the [`GETSET`](https://redis.io/commands/getset) command sets a key to a new value, returning the old value as the result. You can use this command, for example, if you have a system that increments a Redis key using [`INCR`](https://redis.io/commands/incr) every time your web site receives a new visitor. You may want to collect this information once every hour, without losing a single increment. You can [`GETSET`](https://redis.io/commands/getset) the key, assigning it the new value of "0" and reading the old value back.  
有许多用于操作字符串的命令。例如， `GETSET` 命令将一个键设置为一个新值，并返回旧值作为结果。例如，如果您的系统在您的网站每次收到新访问者时使用 `INCR` 递增 Redis 密钥，您就可以使用此命令。您可能希望每小时收集一次此信息，而不会丢失任何增量。您可以 `GETSET` 键，为其分配新值“0”并读回旧值。

The ability to set or retrieve the value of multiple keys in a single command is also useful for reduced latency. For this reason there are the [`MSET`](https://redis.io/commands/mset) and [`MGET`](https://redis.io/commands/mget) commands:  
在单个命令中设置或检索多个键的值的能力对于减少延迟也很有用。为此，有 `MSET` 和 `MGET` 命令：

```
> mset a 10 b 20 c 30
OK
> mget a b c
1) "10"
2) "20"
3) "30"
```

When [`MGET`](https://redis.io/commands/mget) is used, Redis returns an array of values.  
使用 `MGET` 时，Redis 返回一个值数组。

## Altering and querying the key space  
更改和查询密钥空间

There are commands that are not defined on particular types, but are useful in order to interact with the space of keys, and thus, can be used with keys of any type.  
有些命令没有在特定类型上定义，但对于与键空间交互很有用，因此可以与任何类型的键一起使用。

For example the [`EXISTS`](https://redis.io/commands/exists) command returns 1 or 0 to signal if a given key exists or not in the database, while the [`DEL`](https://redis.io/commands/del) command deletes a key and associated value, whatever the value is.  
例如， `EXISTS` 命令返回 1 或 0 以指示给定键是否存在于数据库中，而 `DEL` 命令删除键和关联值，无论值是什么。

```
> set mykey hello
OK
> exists mykey
(integer) 1
> del mykey
(integer) 1
> exists mykey
(integer) 0
```

From the examples you can also see how [`DEL`](https://redis.io/commands/del) itself returns 1 or 0 depending on whether the key was removed (it existed) or not (there was no such key with that name).  
从示例中，您还可以看到 `DEL` 本身如何返回 1 或 0，这取决于键是否被删除（它存在）或不存在（不存在具有该名称的键）。

There are many key space related commands, but the above two are the essential ones together with the [`TYPE`](https://redis.io/commands/type) command, which returns the kind of value stored at the specified key:  
与键空间相关的命令有很多，但以上两个是必不可少的，再加上 `TYPE` 命令，它返回指定键存储的值的种类：

```
> set mykey x
OK
> type mykey
string
> del mykey
(integer) 1
> type mykey
none
```

## Key expiration

Before moving on, we should look at an important Redis feature that works regardless of the type of value you're storing: key expiration. Key expiration lets you set a timeout for a key, also known as a "time to live", or "TTL". When the time to live elapses, the key is automatically destroyed.  
在继续之前，我们应该了解一个重要的 Redis 功能，无论您存储的值是什么类型，它都可以工作：键过期。密钥过期允许您为密钥设置超时，也称为“生存时间”或“TTL”。当生存时间过去时，密钥将自动销毁。

A few important notes about key expiration:  
关于密钥过期的一些重要说明：

-   They can be set both using seconds or milliseconds precision.  
    可以使用秒或毫秒精度设置它们。
-   However the expire time resolution is always 1 millisecond.  
    但是，过期时间分辨率始终为 1 毫秒。
-   Information about expires are replicated and persisted on disk, the time virtually passes when your Redis server remains stopped (this means that Redis saves the date at which a key will expire).  
    关于过期的信息被复制并保存在磁盘上，当您的 Redis 服务器保持停止时，时间实际上已经过去了（这意味着 Redis 会保存密钥过期的日期）。

Use the [`EXPIRE`](https://redis.io/commands/expire) command to set a key's expiration:  
使用 `EXPIRE` 命令设置密钥的过期时间：

```
> set key some-value
OK
> expire key 5
(integer) 1
> get key (immediately)
"some-value"
> get key (after some time)
(nil)
```

The key vanished between the two [`GET`](https://redis.io/commands/get) calls, since the second call was delayed more than 5 seconds. In the example above we used [`EXPIRE`](https://redis.io/commands/expire) in order to set the expire (it can also be used in order to set a different expire to a key already having one, like [`PERSIST`](https://redis.io/commands/persist) can be used in order to remove the expire and make the key persistent forever). However we can also create keys with expires using other Redis commands. For example using [`SET`](https://redis.io/commands/set) options:  
密钥在两次 `GET` 调用之间消失了，因为第二次调用延迟了 5 秒以上。在上面的示例中，我们使用 `EXPIRE` 来设置过期（它也可以用来为已经有过期的密钥设置不同的过期，例如 `PERSIST` 可以用来删除过期并使密钥永远存在）。然而，我们也可以使用其他 Redis 命令创建带有过期时间的键。例如使用 `SET` 选项：

```
> set key 100 ex 10
OK
> ttl key
(integer) 9
```

The example above sets a key with the string value `100`, having an expire of ten seconds. Later the [`TTL`](https://redis.io/commands/ttl) command is called in order to check the remaining time to live for the key.  
上面的示例设置了一个字符串值为 `100` 的键，过期时间为 10 秒。稍后调用 `TTL` 命令以检查密钥的剩余生存时间。

In order to set and check expires in milliseconds, check the [`PEXPIRE`](https://redis.io/commands/pexpire) and the [`PTTL`](https://redis.io/commands/pttl) commands, and the full list of [`SET`](https://redis.io/commands/set) options.  
为了设置和检查以毫秒为单位的过期时间，请检查 `PEXPIRE` 和 `PTTL` 命令，以及 `SET` 选项的完整列表。

## Lists

To explain the List data type it's better to start with a little bit of theory, as the term _List_ is often used in an improper way by information technology folks. For instance "Python Lists" are not what the name may suggest (Linked Lists), but rather Arrays (the same data type is called Array in Ruby actually).  
要解释 List 数据类型，最好从一些理论开始，因为信息技术人员经常以不正确的方式使用术语 List。例如，“Python 列表”并不是顾名思义（链接列表），而是数组（相同的数据类型实际上在 Ruby 中称为数组）。

From a very general point of view a List is just a sequence of ordered elements: 10,20,1,2,3 is a list. But the properties of a List implemented using an Array are very different from the properties of a List implemented using a _Linked List_.  
从非常一般的角度来看，列表只是有序元素的序列：10,20,1,2,3 是一个列表。但是使用数组实现的列表的属性与使用链表实现的列表的属性有很大不同。

Redis lists are implemented via Linked Lists. This means that even if you have millions of elements inside a list, the operation of adding a new element in the head or in the tail of the list is performed _in constant time_. The speed of adding a new element with the [`LPUSH`](https://redis.io/commands/lpush) command to the head of a list with ten elements is the same as adding an element to the head of list with 10 million elements.  
Redis 列表是通过链表实现的。这意味着即使列表中有数百万个元素，在列表的头部或尾部添加新元素的操作也是在常数时间内执行的。使用 `LPUSH` 命令向具有 10 个元素的列表的头部添加一个新元素的速度与向具有 1000 万个元素的列表的头部添加一个元素的速度相同。

What's the downside? Accessing an element _by index_ is very fast in lists implemented with an Array (constant time indexed access) and not so fast in lists implemented by linked lists (where the operation requires an amount of work proportional to the index of the accessed element).  
缺点是什么？通过索引访问元素在使用数组实现的列表中非常快（恒定时间索引访问），而在链表实现的列表中则不是那么快（其中操作需要与访问元素的索引成比例的工作量）。

Redis Lists are implemented with linked lists because for a database system it is crucial to be able to add elements to a very long list in a very fast way. Another strong advantage, as you'll see in a moment, is that Redis Lists can be taken at constant length in constant time.  
Redis 列表是用链表实现的，因为对于数据库系统来说，能够以非常快的方式将元素添加到一个很长的列表中是至关重要的。另一个强大的优势，您马上就会看到，Redis 列表可以在恒定时间内以恒定长度获取。

When fast access to the middle of a large collection of elements is important, there is a different data structure that can be used, called sorted sets. Sorted sets will be covered later in this tutorial.  
当快速访问大量元素的中间很重要时，可以使用一种不同的数据结构，称为排序集。排序集将在本教程的后面部分介绍。

### First steps with Redis Lists Redis 列表的第一步

The [`LPUSH`](https://redis.io/commands/lpush) command adds a new element into a list, on the left (at the head), while the [`RPUSH`](https://redis.io/commands/rpush) command adds a new element into a list, on the right (at the tail). Finally the [`LRANGE`](https://redis.io/commands/lrange) command extracts ranges of elements from lists:  
`LPUSH` 命令将新元素添加到列表的左侧（头部），而 `RPUSH` 命令将新元素添加到列表的右侧（尾部）。最后， `LRANGE` 命令从列表中提取元素范围：

```
> rpush mylist A
(integer) 1
> rpush mylist B
(integer) 2
> lpush mylist first
(integer) 3
> lrange mylist 0 -1
1) "first"
2) "A"
3) "B"
```

Note that [LRANGE](https://redis.io/commands/lrange) takes two indexes, the first and the last element of the range to return. Both the indexes can be negative, telling Redis to start counting from the end: so -1 is the last element, -2 is the penultimate element of the list, and so forth.  
请注意，LRANGE 有两个索引，即要返回的范围的第一个和最后一个元素。两个索引都可以是负数，告诉 Redis 从末尾开始计数：-1 是最后一个元素，-2 是列表的倒数第二个元素，依此类推。

As you can see [`RPUSH`](https://redis.io/commands/rpush) appended the elements on the right of the list, while the final [`LPUSH`](https://redis.io/commands/lpush) appended the element on the left.  
如您所见， `RPUSH` 将元素附加到列表的右侧，而最后的 `LPUSH` 将元素附加到列表的左侧。

Both commands are _variadic commands_, meaning that you are free to push multiple elements into a list in a single call:  
这两个命令都是可变参数命令，这意味着您可以在一次调用中自由地将多个元素推送到列表中：

```
> rpush mylist 1 2 3 4 5 "foo bar"
(integer) 9
> lrange mylist 0 -1
1) "first"
2) "A"
3) "B"
4) "1"
5) "2"
6) "3"
7) "4"
8) "5"
9) "foo bar"
```

An important operation defined on Redis lists is the ability to _pop elements_. Popping elements is the operation of both retrieving the element from the list, and eliminating it from the list, at the same time. You can pop elements from left and right, similarly to how you can push elements in both sides of the list:  
Redis 列表上定义的一个重要操作是弹出元素的能力。弹出元素是同时从列表中检索元素并将其从列表中删除的操作。您可以从左侧和右侧弹出元素，类似于在列表两侧推送元素的方式：

```
> rpush mylist a b c
(integer) 3
> rpop mylist
"c"
> rpop mylist
"b"
> rpop mylist
"a"
```

We added three elements and popped three elements, so at the end of this sequence of commands the list is empty and there are no more elements to pop. If we try to pop yet another element, this is the result we get:  
我们添加了三个元素并弹出了三个元素，所以在这个命令序列的末尾列表是空的，没有更多的元素可以弹出。如果我们尝试弹出另一个元素，这就是我们得到的结果：

```
> rpop mylist
(nil)
```

Redis returned a NULL value to signal that there are no elements in the list.  
Redis 返回一个 NULL 值，表示列表中没有元素。

### Common use cases for lists 列表的常见用例

Lists are useful for a number of tasks, two very representative use cases are the following:  
列表对许多任务都很有用，两个非常有代表性的用例如下：

-   Remember the latest updates posted by users into a social network.  
    记住用户发布到社交网络中的最新更新。
-   Communication between processes, using a consumer-producer pattern where the producer pushes items into a list, and a consumer (usually a _worker_) consumes those items and executes actions. Redis has special list commands to make this use case both more reliable and efficient.  
    进程之间的通信，使用消费者-生产者模式，其中生产者将项目推送到列表中，消费者（通常是工作人员）消费这些项目并执行操作。 Redis 有特殊的列表命令来使这个用例更加可靠和高效。

For example both the popular Ruby libraries [resque](https://github.com/resque/resque) and [sidekiq](https://github.com/mperham/sidekiq) use Redis lists under the hood in order to implement background jobs.  
例如，流行的 Ruby 库 resque 和 sidekiq 都在底层使用 Redis 列表来实现后台作业。

The popular Twitter social network [takes the latest tweets](http://www.infoq.com/presentations/Real-Time-Delivery-Twitter) posted by users into Redis lists.  
流行的 Twitter 社交网络将用户发布的最新推文放入 Redis 列表中。

To describe a common use case step by step, imagine your home page shows the latest photos published in a photo sharing social network and you want to speedup access.  
为了逐步描述一个常见用例，假设您的主页显示了照片共享社交网络中发布的最新照片，并且您想加快访问速度。

-   Every time a user posts a new photo, we add its ID into a list with [`LPUSH`](https://redis.io/commands/lpush).  
    每次用户发布新照片时，我们都会将其 ID 添加到带有 `LPUSH` 的列表中。
-   When users visit the home page, we use `LRANGE 0 9` in order to get the latest 10 posted items.  
    当用户访问主页时，我们使用 `LRANGE 0 9` 以获取最新的 10 个发布项目。

### Capped lists

In many use cases we just want to use lists to store the _latest items_, whatever they are: social network updates, logs, or anything else.  
在许多用例中，我们只想使用列表来存储最新的项目，无论它们是什么：社交网络更新、日志或其他任何东西。

Redis allows us to use lists as a capped collection, only remembering the latest N items and discarding all the oldest items using the [`LTRIM`](https://redis.io/commands/ltrim) command.  
Redis 允许我们将列表用作上限集合，只记住最新的 N 个项目并使用 `LTRIM` 命令丢弃所有最旧的项目。

The [`LTRIM`](https://redis.io/commands/ltrim) command is similar to [`LRANGE`](https://redis.io/commands/lrange), but **instead of displaying the specified range of elements** it sets this range as the new list value. All the elements outside the given range are removed.  
`LTRIM` 命令与 `LRANGE` 类似，但不是显示指定范围的元素，而是将此范围设置为新的列表值。给定范围之外的所有元素都将被删除。

An example will make it more clear:  
举个例子会更清楚：

```
> rpush mylist 1 2 3 4 5
(integer) 5
> ltrim mylist 0 2
OK
> lrange mylist 0 -1
1) "1"
2) "2"
3) "3"
```

The above [`LTRIM`](https://redis.io/commands/ltrim) command tells Redis to take just list elements from index 0 to 2, everything else will be discarded. This allows for a very simple but useful pattern: doing a List push operation + a List trim operation together in order to add a new element and discard elements exceeding a limit:  
上面的 `LTRIM` 命令告诉 Redis 只获取从索引 0 到 2 的列表元素，其他所有元素都将被丢弃。这允许一个非常简单但有用的模式：一起执行列表推送操作 + 列表修剪操作以添加新元素并丢弃超过限制的元素：

```
LPUSH mylist <some element>
LTRIM mylist 0 999
```

The above combination adds a new element and takes only the 1000 newest elements into the list. With [`LRANGE`](https://redis.io/commands/lrange) you can access the top items without any need to remember very old data.  
上面的组合添加了一个新元素，并且只将最新的 1000 个元素放入列表中。使用 `LRANGE` ，您可以访问最前面的项目，而无需记住非常旧的数据。

Note: while [`LRANGE`](https://redis.io/commands/lrange) is technically an O(N) command, accessing small ranges towards the head or the tail of the list is a constant time operation.  
注意：虽然 `LRANGE` 在技术上是 O(N) 命令，但访问列表头部或尾部的小范围是恒定时间操作。

## Blocking operations on lists 列表上的阻塞操作

Lists have a special feature that make them suitable to implement queues, and in general as a building block for inter process communication systems: blocking operations.  
列表有一个特殊的特性，使它们适合于实现队列，并且通常作为进程间通信系统的构建块：阻塞操作。

Imagine you want to push items into a list with one process, and use a different process in order to actually do some kind of work with those items. This is the usual producer / consumer setup, and can be implemented in the following simple way:  
想象一下，您想要使用一个进程将项目推送到列表中，并使用不同的进程来实际对这些项目进行某种处理。这是通常的生产者/消费者设置，可以通过以下简单方式实现：

-   To push items into the list, producers call [`LPUSH`](https://redis.io/commands/lpush).  
    要将项目推送到列表中，生产者调用 `LPUSH` 。
-   To extract / process items from the list, consumers call [`RPOP`](https://redis.io/commands/rpop).  
    要从列表中提取/处理项目，消费者调用 `RPOP` 。

However it is possible that sometimes the list is empty and there is nothing to process, so [`RPOP`](https://redis.io/commands/rpop) just returns NULL. In this case a consumer is forced to wait some time and retry again with [`RPOP`](https://redis.io/commands/rpop). This is called _polling_, and is not a good idea in this context because it has several drawbacks:  
然而，有时列表可能是空的，没有任何东西要处理，所以 `RPOP` 只返回 NULL。在这种情况下，消费者被迫等待一段时间，然后使用 `RPOP` 重试。这称为轮询，在这种情况下不是一个好主意，因为它有几个缺点：

1.  Forces Redis and clients to process useless commands (all the requests when the list is empty will get no actual work done, they'll just return NULL).  
    强制 Redis 和客户端处理无用的命令（列表为空时的所有请求都不会完成任何实际工作，它们只会返回 NULL）。
2.  Adds a delay to the processing of items, since after a worker receives a NULL, it waits some time. To make the delay smaller, we could wait less between calls to [`RPOP`](https://redis.io/commands/rpop), with the effect of amplifying problem number 1, i.e. more useless calls to Redis.  
    为项目的处理添加延迟，因为在 worker 收到 NULL 后，它会等待一段时间。为了使延迟更小，我们可以减少调用 `RPOP` 之间的等待时间，从而放大问题 1，即更多无用的 Redis 调用。

So Redis implements commands called [`BRPOP`](https://redis.io/commands/brpop) and [`BLPOP`](https://redis.io/commands/blpop) which are versions of [`RPOP`](https://redis.io/commands/rpop) and [`LPOP`](https://redis.io/commands/lpop) able to block if the list is empty: they'll return to the caller only when a new element is added to the list, or when a user-specified timeout is reached.  
因此，Redis 实现了名为 `BRPOP` 和 `BLPOP` 的命令，它们是 `RPOP` 和 `LPOP` 的版本，如果列表为空则能够阻塞：只有在列表中添加新元素时，它们才会返回给调用者，或者达到用户指定的超时时间。

This is an example of a [`BRPOP`](https://redis.io/commands/brpop) call we could use in the worker:  
这是我们可以在 worker 中使用的 `BRPOP` 调用示例：

```
> brpop tasks 5
1) "tasks"
2) "do_something"
```

It means: "wait for elements in the list `tasks`, but return if after 5 seconds no element is available".  
这意味着：“等待列表 `tasks` 中的元素，但如果 5 秒后没有元素可用则返回”。

Note that you can use 0 as timeout to wait for elements forever, and you can also specify multiple lists and not just one, in order to wait on multiple lists at the same time, and get notified when the first list receives an element.  
请注意，您可以使用 0 作为超时来永远等待元素，您还可以指定多个列表而不仅仅是一个列表，以便同时等待多个列表，并在第一个列表接收到元素时得到通知。

A few things to note about [`BRPOP`](https://redis.io/commands/brpop):  
关于 `BRPOP` 的一些注意事项：

1.  Clients are served in an ordered way: the first client that blocked waiting for a list, is served first when an element is pushed by some other client, and so forth.  
    客户端以有序方式提供服务：阻塞等待列表的第一个客户端在其他客户端推送元素时首先提供服务，依此类推。
2.  The return value is different compared to [`RPOP`](https://redis.io/commands/rpop): it is a two-element array since it also includes the name of the key, because [`BRPOP`](https://redis.io/commands/brpop) and [`BLPOP`](https://redis.io/commands/blpop) are able to block waiting for elements from multiple lists.  
    返回值与 `RPOP` 不同：它是一个双元素数组，因为它还包含键的名称，因为 `BRPOP` 和 `BLPOP` 能够阻止等待来自多个列表的元素。
3.  If the timeout is reached, NULL is returned.  
    如果达到超时，则返回 NULL。

There are more things you should know about lists and blocking ops. We suggest that you read more on the following:  
关于列表和阻塞操作，您应该了解更多内容。我们建议您阅读以下内容：

-   It is possible to build safer queues or rotating queues using [`LMOVE`](https://redis.io/commands/lmove).  
    可以使用 `LMOVE` 构建更安全的队列或旋转队列。
-   There is also a blocking variant of the command, called [`BLMOVE`](https://redis.io/commands/blmove).  
    该命令还有一个阻塞变体，称为 `BLMOVE` 。

## Automatic creation and removal of keys  
自动创建和删除密钥

So far in our examples we never had to create empty lists before pushing elements, or removing empty lists when they no longer have elements inside. It is Redis' responsibility to delete keys when lists are left empty, or to create an empty list if the key does not exist and we are trying to add elements to it, for example, with [`LPUSH`](https://redis.io/commands/lpush).  
到目前为止，在我们的示例中，我们从来不需要在推送元素之前创建空列表，也不需要在空列表中不再包含元素时删除空列表。 Redis 负责在列表为空时删除键，或者在键不存在并且我们试图向其中添加元素时创建一个空列表，例如，使用 `LPUSH` 。

This is not specific to lists, it applies to all the Redis data types composed of multiple elements -- Streams, Sets, Sorted Sets and Hashes.  
这并不特定于列表，它适用于由多个元素组成的所有 Redis 数据类型——流、集合、有序集合和哈希。

Basically we can summarize the behavior with three rules:  
基本上我们可以用三个规则来总结行为：

1.  When we add an element to an aggregate data type, if the target key does not exist, an empty aggregate data type is created before adding the element.  
    当我们向聚合数据类型添加元素时，如果目标键不存在，则在添加元素之前创建一个空的聚合数据类型。
2.  When we remove elements from an aggregate data type, if the value remains empty, the key is automatically destroyed. The Stream data type is the only exception to this rule.  
    当我们从聚合数据类型中删除元素时，如果值仍然为空，则键会自动销毁。 Stream 数据类型是该规则的唯一例外。
3.  Calling a read-only command such as [`LLEN`](https://redis.io/commands/llen) (which returns the length of the list), or a write command removing elements, with an empty key, always produces the same result as if the key is holding an empty aggregate type of the type the command expects to find.  
    使用空键调用只读命令，如 `LLEN` （返回列表的长度），或删除元素的写命令，总是产生相同的结果，就好像该键持有一个空聚合类型键入期望查找的命令。

Examples of rule 1: 规则 1 的示例：

```
> del mylist
(integer) 1
> lpush mylist 1 2 3
(integer) 3
```

However we can't perform operations against the wrong type if the key exists:  
但是，如果键存在，我们不能对错误的类型执行操作：

```
> set foo bar
OK
> lpush foo 1 2 3
(error) WRONGTYPE Operation against a key holding the wrong kind of value
> type foo
string
```

Example of rule 2: 规则 2 示例：

```
> lpush mylist 1 2 3
(integer) 3
> exists mylist
(integer) 1
> lpop mylist
"3"
> lpop mylist
"2"
> lpop mylist
"1"
> exists mylist
(integer) 0
```

The key no longer exists after all the elements are popped.  
弹出所有元素后，键不再存在。

Example of rule 3: 规则 3 示例：

```
> del mylist
(integer) 0
> llen mylist
(integer) 0
> lpop mylist
(nil)
```

## Hashes

Redis hashes look exactly how one might expect a "hash" to look, with field-value pairs:  
Redis 散列与人们期望的“散列”看起来完全一样，带有字段值对：

```
> hset user:1000 username antirez birthyear 1977 verified 1
(integer) 3
> hget user:1000 username
"antirez"
> hget user:1000 birthyear
"1977"
> hgetall user:1000
1) "username"
2) "antirez"
3) "birthyear"
4) "1977"
5) "verified"
6) "1"
```

While hashes are handy to represent _objects_, actually the number of fields you can put inside a hash has no practical limits (other than available memory), so you can use hashes in many different ways inside your application.  
虽然散列可以很方便地表示对象，但实际上您可以放入散列中的字段数量没有实际限制（可用内存除外），因此您可以在应用程序中以多种不同方式使用散列。

The command [`HSET`](https://redis.io/commands/hset) sets multiple fields of the hash, while [`HGET`](https://redis.io/commands/hget) retrieves a single field. [`HMGET`](https://redis.io/commands/hmget) is similar to [`HGET`](https://redis.io/commands/hget) but returns an array of values:  
命令 `HSET` 设置散列的多个字段，而 `HGET` 检索单个字段。 `HMGET` 类似于 `HGET` 但返回一个值数组：

```
> hmget user:1000 username birthyear no-such-field
1) "antirez"
2) "1977"
3) (nil)
```

There are commands that are able to perform operations on individual fields as well, like [`HINCRBY`](https://redis.io/commands/hincrby):  
有些命令也能够对单个字段执行操作，例如 `HINCRBY` ：

```
> hincrby user:1000 birthyear 10
(integer) 1987
> hincrby user:1000 birthyear 10
(integer) 1997
```

You can find the [full list of hash commands in the documentation](https://redis.io/commands#hash).  
您可以在文档中找到哈希命令的完整列表。

It is worth noting that small hashes (i.e., a few elements with small values) are encoded in special way in memory that make them very memory efficient.  
值得注意的是，小散列（即一些具有小值的元素）在内存中以特殊方式编码，这使得它们的内存效率很高。

## Sets

Redis Sets are unordered collections of strings. The [`SADD`](https://redis.io/commands/sadd) command adds new elements to a set. It's also possible to do a number of other operations against sets like testing if a given element already exists, performing the intersection, union or difference between multiple sets, and so forth.  
Redis 集是无序的字符串集合。 `SADD` 命令将新元素添加到集合中。还可以对集合执行许多其他操作，例如测试给定元素是否已经存在，执行多个集合之间的交集、并集或差集，等等。

```
> sadd myset 1 2 3
(integer) 3
> smembers myset
1. 3
2. 1
3. 2
```

Here I've added three elements to my set and told Redis to return all the elements. As you can see they are not sorted -- Redis is free to return the elements in any order at every call, since there is no contract with the user about element ordering.  
在这里，我向集合中添加了三个元素，并告诉 Redis 返回所有元素。如您所见，它们没有排序——Redis 可以在每次调用时自由地以任何顺序返回元素，因为与用户没有关于元素排序的合同。

Redis has commands to test for membership. For example, checking if an element exists:  
Redis 有测试成员资格的命令。例如，检查一个元素是否存在：

```
> sismember myset 3
(integer) 1
> sismember myset 30
(integer) 0
```

"3" is a member of the set, while "30" is not.  
“3”是集合的成员，而“30”不是。

Sets are good for expressing relations between objects. For instance we can easily use sets in order to implement tags.  
集合有利于表达对象之间的关系。例如，我们可以轻松地使用集合来实现标签。

A simple way to model this problem is to have a set for every object we want to tag. The set contains the IDs of the tags associated with the object.  
对此问题建模的一种简单方法是为我们要标记的每个对象设置一个集合。该集合包含与对象关联的标签的 ID。

One illustration is tagging news articles. If article ID 1000 is tagged with tags 1, 2, 5 and 77, a set can associate these tag IDs with the news item:  
一个例子是标记新闻文章。如果文章 ID 1000 被标记为标签 1、2、5 和 77，则集合可以将这些标签 ID 与新闻项相关联：

```
> sadd news:1000:tags 1 2 5 77
(integer) 4
```

We may also want to have the inverse relation as well: the list of all the news tagged with a given tag:  
我们可能还希望拥有反向关系：用给定标签标记的所有新闻的列表：

```
> sadd tag:1:news 1000
(integer) 1
> sadd tag:2:news 1000
(integer) 1
> sadd tag:5:news 1000
(integer) 1
> sadd tag:77:news 1000
(integer) 1
```

To get all the tags for a given object is trivial:  
获取给定对象的所有标签很简单：

```
> smembers news:1000:tags
1. 5
2. 1
3. 77
4. 2
```

Note: in the example we assume you have another data structure, for example a Redis hash, which maps tag IDs to tag names.  
注意：在示例中，我们假设您有另一个数据结构，例如 Redis 哈希，它将标签 ID 映射到标签名称。

There are other non trivial operations that are still easy to implement using the right Redis commands. For instance we may want a list of all the objects with the tags 1, 2, 10, and 27 together. We can do this using the [`SINTER`](https://redis.io/commands/sinter) command, which performs the intersection between different sets. We can use:  
还有其他重要的操作仍然可以使用正确的 Redis 命令轻松实现。例如，我们可能想要一个包含标签 1、2、10 和 27 的所有对象的列表。我们可以使用 `SINTER` 命令执行此操作，该命令执行不同集合之间的交集。我们可以用：

```
> sinter tag:1:news tag:2:news tag:10:news tag:27:news
... results here ...
```

In addition to intersection you can also perform unions, difference, extract a random element, and so forth.  
除了交集，您还可以执行并集、差集、提取随机元素等。

The command to extract an element is called [`SPOP`](https://redis.io/commands/spop), and is handy to model certain problems. For example in order to implement a web-based poker game, you may want to represent your deck with a set. Imagine we use a one-char prefix for (C)lubs, (D)iamonds, (H)earts, (S)pades:  
提取元素的命令称为 `SPOP` ，可以方便地对某些问题进行建模。例如，为了实现基于 Web 的扑克游戏，您可能希望用一组来表示您的套牌。想象一下，我们为 (C)lubs、(D)iamonds、(H)earts、(S)pades 使用一个字符前缀：

```
> sadd deck C1 C2 C3 C4 C5 C6 C7 C8 C9 C10 CJ CQ CK
  D1 D2 D3 D4 D5 D6 D7 D8 D9 D10 DJ DQ DK H1 H2 H3
  H4 H5 H6 H7 H8 H9 H10 HJ HQ HK S1 S2 S3 S4 S5 S6
  S7 S8 S9 S10 SJ SQ SK
(integer) 52
```

Now we want to provide each player with 5 cards. The [`SPOP`](https://redis.io/commands/spop) command removes a random element, returning it to the client, so it is the perfect operation in this case.  
现在我们想给每个玩家提供 5 张牌。 `SPOP` 命令删除一个随机元素，将其返回给客户端，因此在这种情况下它是完美的操作。

However if we call it against our deck directly, in the next play of the game we'll need to populate the deck of cards again, which may not be ideal. So to start, we can make a copy of the set stored in the `deck` key into the `game:1:deck` key.  
然而，如果我们直接跟注对抗我们的牌组，在游戏的下一场比赛中，我们将需要再次填充牌组，这可能并不理想。因此，首先，我们可以将存储在 `deck` 键中的集合复制到 `game:1:deck` 键中。

This is accomplished using [`SUNIONSTORE`](https://redis.io/commands/sunionstore), which normally performs the union between multiple sets, and stores the result into another set. However, since the union of a single set is itself, I can copy my deck with:  
这是使用 `SUNIONSTORE` 完成的，它通常在多个集合之间执行并集，并将结果存储到另一个集合中。然而，由于单个集合的联合本身，我可以复制我的套牌：

```
> sunionstore game:1:deck deck
(integer) 52
```

Now I'm ready to provide the first player with five cards:  
现在我准备为第一个玩家提供五张牌：

```
> spop game:1:deck
"C6"
> spop game:1:deck
"CQ"
> spop game:1:deck
"D1"
> spop game:1:deck
"CJ"
> spop game:1:deck
"SJ"
```

One pair of jacks, not great...  
一对千斤顶，不太好...

This is a good time to introduce the set command that provides the number of elements inside a set. This is often called the _cardinality of a set_ in the context of set theory, so the Redis command is called [`SCARD`](https://redis.io/commands/scard).  
现在是介绍 set 命令的好时机，该命令提供集合中元素的数量。这在集合论的上下文中通常称为集合的基数，因此 Redis 命令称为 `SCARD` 。

```
> scard game:1:deck
(integer) 47
```

The math works: 52 - 5 = 47.  
数学有效：52 - 5 = 47。

When you need to just get random elements without removing them from the set, there is the [`SRANDMEMBER`](https://redis.io/commands/srandmember) command suitable for the task. It also features the ability to return both repeating and non-repeating elements.  
当您只需要获取随机元素而不将它们从集合中移除时，可以使用适合该任务的 `SRANDMEMBER` 命令。它还具有返回重复和非重复元素的能力。

## Sorted sets

Sorted sets are a data type which is similar to a mix between a Set and a Hash. Like sets, sorted sets are composed of unique, non-repeating string elements, so in some sense a sorted set is a set as well.  
有序集合是一种数据类型，类似于 Set 和 Hash 的混合。与集合一样，有序集合由唯一的、不重复的字符串元素组成，因此在某种意义上，有序集合也是一个集合。

However while elements inside sets are not ordered, every element in a sorted set is associated with a floating point value, called _the score_ (this is why the type is also similar to a hash, since every element is mapped to a value).  
然而，虽然集合中的元素没有排序，但有序集合中的每个元素都与一个浮点值相关联，称为分数（这就是为什么该类型也类似于哈希，因为每个元素都映射到一个值）。

Moreover, elements in a sorted set are _taken in order_ (so they are not ordered on request, order is a peculiarity of the data structure used to represent sorted sets). They are ordered according to the following rule:  
此外，有序集合中的元素是按顺序获取的（因此它们不是按要求排序的，顺序是用于表示有序集合的数据结构的特性）。它们根据以下规则排序：

-   If B and A are two elements with a different score, then A > B if A.score is > B.score.  
    如果 B 和 A 是两个具有不同分数的元素，则如果 A.score > B.score，则 A > B。
-   If B and A have exactly the same score, then A > B if the A string is lexicographically greater than the B string. B and A strings can't be equal since sorted sets only have unique elements.  
    如果 B 和 A 的分数完全相同，则如果 A 字符串在字典序上大于 B 字符串，则 A > B。 B 和 A 字符串不能相等，因为排序集只有唯一元素。

Let's start with a simple example, adding a few selected hackers names as sorted set elements, with their year of birth as "score".  
让我们从一个简单的例子开始，将一些选定的黑客名字添加为有序集合元素，并将他们的出生年份作为“score”。

```
> zadd hackers 1940 "Alan Kay"
(integer) 1
> zadd hackers 1957 "Sophie Wilson"
(integer) 1
> zadd hackers 1953 "Richard Stallman"
(integer) 1
> zadd hackers 1949 "Anita Borg"
(integer) 1
> zadd hackers 1965 "Yukihiro Matsumoto"
(integer) 1
> zadd hackers 1914 "Hedy Lamarr"
(integer) 1
> zadd hackers 1916 "Claude Shannon"
(integer) 1
> zadd hackers 1969 "Linus Torvalds"
(integer) 1
> zadd hackers 1912 "Alan Turing"
(integer) 1
```

As you can see [`ZADD`](https://redis.io/commands/zadd) is similar to [`SADD`](https://redis.io/commands/sadd), but takes one additional argument (placed before the element to be added) which is the score. [`ZADD`](https://redis.io/commands/zadd) is also variadic, so you are free to specify multiple score-value pairs, even if this is not used in the example above.  
如您所见， `ZADD` 与 `SADD` 类似，但有一个额外的参数（位于要添加的元素之前），即分数。 `ZADD` 也是可变的，所以你可以自由指定多个分值对，即使这在上面的例子中没有使用。

With sorted sets it is trivial to return a list of hackers sorted by their birth year because actually _they are already sorted_.  
使用排序集，返回按出生年份排序的黑客列表是微不足道的，因为实际上他们已经排序了。

Implementation note: Sorted sets are implemented via a dual-ported data structure containing both a skip list and a hash table, so every time we add an element Redis performs an O(log(N)) operation. That's good, but when we ask for sorted elements Redis does not have to do any work at all, it's already all sorted:  
实现注意事项：排序集是通过包含跳跃列表和哈希表的双端口数据结构实现的，因此每次我们添加一个元素时，Redis 都会执行 O(log(N)) 操作。这很好，但是当我们要求排序的元素时，Redis 根本不需要做任何工作，它已经全部排序了：

```
> zrange hackers 0 -1
1) "Alan Turing"
2) "Hedy Lamarr"
3) "Claude Shannon"
4) "Alan Kay"
5) "Anita Borg"
6) "Richard Stallman"
7) "Sophie Wilson"
8) "Yukihiro Matsumoto"
9) "Linus Torvalds"
```

Note: 0 and -1 means from element index 0 to the last element (-1 works here just as it does in the case of the [`LRANGE`](https://redis.io/commands/lrange) command).  
注意：0 和 -1 表示从元素索引 0 到最后一个元素（-1 在这里的作用与在 `LRANGE` 命令的情况下一样）。

What if I want to order them the opposite way, youngest to oldest? Use [ZREVRANGE](https://redis.io/commands/zrevrange) instead of [ZRANGE](https://redis.io/commands/zrange):  
如果我想以相反的方式排序，从小到大怎么办？使用 ZREVRANGE 而不是 ZRANGE ：

```
> zrevrange hackers 0 -1
1) "Linus Torvalds"
2) "Yukihiro Matsumoto"
3) "Sophie Wilson"
4) "Richard Stallman"
5) "Anita Borg"
6) "Alan Kay"
7) "Claude Shannon"
8) "Hedy Lamarr"
9) "Alan Turing"
```

It is possible to return scores as well, using the `WITHSCORES` argument:  
也可以使用 `WITHSCORES` 参数返回分数：

```
> zrange hackers 0 -1 withscores
1) "Alan Turing"
2) "1912"
3) "Hedy Lamarr"
4) "1914"
5) "Claude Shannon"
6) "1916"
7) "Alan Kay"
8) "1940"
9) "Anita Borg"
10) "1949"
11) "Richard Stallman"
12) "1953"
13) "Sophie Wilson"
14) "1957"
15) "Yukihiro Matsumoto"
16) "1965"
17) "Linus Torvalds"
18) "1969"
```

### Operating on ranges 在范围内操作

Sorted sets are more powerful than this. They can operate on ranges. Let's get all the individuals that were born up to 1950 inclusive. We use the [`ZRANGEBYSCORE`](https://redis.io/commands/zrangebyscore) command to do it:  
排序集比这更强大。他们可以在范围内操作。让我们获取 1950 年（含）之前出生的所有个人。我们使用 `ZRANGEBYSCORE` 命令来做：

```
> zrangebyscore hackers -inf 1950
1) "Alan Turing"
2) "Hedy Lamarr"
3) "Claude Shannon"
4) "Alan Kay"
5) "Anita Borg"
```

We asked Redis to return all the elements with a score between negative infinity and 1950 (both extremes are included).  
我们要求 Redis 返回得分在负无穷大和 1950 之间（包括两个极端）的所有元素。

It's also possible to remove ranges of elements. Let's remove all the hackers born between 1940 and 1960 from the sorted set:  
也可以删除元素范围。让我们从排序集中删除所有出生在 1940 年到 1960 年之间的黑客：

```
> zremrangebyscore hackers 1940 1960
(integer) 4
```

[`ZREMRANGEBYSCORE`](https://redis.io/commands/zremrangebyscore) is perhaps not the best command name, but it can be very useful, and returns the number of removed elements.  
`ZREMRANGEBYSCORE` 可能不是最好的命令名称，但它可能非常有用，并返回已删除元素的数量。

Another extremely useful operation defined for sorted set elements is the get-rank operation. It is possible to ask what is the position of an element in the set of the ordered elements.  
为有序集合元素定义的另一个非常有用的操作是 get-rank 操作。可以询问元素在有序元素集中的位置。

```
> zrank hackers "Anita Borg"
(integer) 4
```

The [`ZREVRANK`](https://redis.io/commands/zrevrank) command is also available in order to get the rank, considering the elements sorted a descending way.  
考虑到按降序方式排序的元素， `ZREVRANK` 命令也可用于获取排名。

### Lexicographical scores 辞典分数

With recent versions of Redis 2.8, a new feature was introduced that allows getting ranges lexicographically, assuming elements in a sorted set are all inserted with the same identical score (elements are compared with the C `memcmp` function, so it is guaranteed that there is no collation, and every Redis instance will reply with the same output).  
在 Redis 2.8 的最新版本中，引入了一个新功能，允许按字典顺序获取范围，假设排序集中的元素都以相同的相同分数插入（元素与 C `memcmp` 函数进行比较，因此可以保证有没有排序规则，每个 Redis 实例都会回复相同的输出）。

The main commands to operate with lexicographical ranges are [`ZRANGEBYLEX`](https://redis.io/commands/zrangebylex), [`ZREVRANGEBYLEX`](https://redis.io/commands/zrevrangebylex), [`ZREMRANGEBYLEX`](https://redis.io/commands/zremrangebylex) and [`ZLEXCOUNT`](https://redis.io/commands/zlexcount).  
操作字典范围的主要命令是 `ZRANGEBYLEX` 、 `ZREVRANGEBYLEX` 、 `ZREMRANGEBYLEX` 和 `ZLEXCOUNT` 。

For example, let's add again our list of famous hackers, but this time use a score of zero for all the elements:  
例如，让我们再次添加我们的著名黑客列表，但这次对所有元素使用零分：

```
> zadd hackers 0 "Alan Kay" 0 "Sophie Wilson" 0 "Richard Stallman" 0
  "Anita Borg" 0 "Yukihiro Matsumoto" 0 "Hedy Lamarr" 0 "Claude Shannon"
  0 "Linus Torvalds" 0 "Alan Turing"
```

Because of the sorted sets ordering rules, they are already sorted lexicographically:  
由于排序集的排序规则，它们已经按字典顺序排序：

```
> zrange hackers 0 -1
1) "Alan Kay"
2) "Alan Turing"
3) "Anita Borg"
4) "Claude Shannon"
5) "Hedy Lamarr"
6) "Linus Torvalds"
7) "Richard Stallman"
8) "Sophie Wilson"
9) "Yukihiro Matsumoto"
```

Using [`ZRANGEBYLEX`](https://redis.io/commands/zrangebylex) we can ask for lexicographical ranges:  
使用 `ZRANGEBYLEX` 我们可以请求字典范围：

```
> zrangebylex hackers [B [P
1) "Claude Shannon"
2) "Hedy Lamarr"
3) "Linus Torvalds"
```

Ranges can be inclusive or exclusive (depending on the first character), also string infinite and minus infinite are specified respectively with the `+` and `-` strings. See the documentation for more information.  
范围可以是包含的或排除的（取决于第一个字符），字符串无限和负无限分别用 `+` 和 `-` 字符串指定。有关详细信息，请参阅文档。

This feature is important because it allows us to use sorted sets as a generic index. For example, if you want to index elements by a 128-bit unsigned integer argument, all you need to do is to add elements into a sorted set with the same score (for example 0) but with a 16 byte prefix consisting of **the 128 bit number in big endian**. Since numbers in big endian, when ordered lexicographically (in raw bytes order) are actually ordered numerically as well, you can ask for ranges in the 128 bit space, and get the element's value discarding the prefix.  
这个特性很重要，因为它允许我们使用排序集作为通用索引。例如，如果你想通过 128 位无符号整数参数索引元素，你需要做的就是将元素添加到具有相同分数（例如 0）但具有由 128 组成的 16 字节前缀的有序集合中big endian 中的位数。由于 big endian 中的数字，当按字典顺序（按原始字节顺序）排序时，实际上也按数字排序，您可以请求 128 位空间中的范围，并获取元素的值并丢弃前缀。

If you want to see the feature in the context of a more serious demo, check the [Redis autocomplete demo](http://autocomplete.redis.io/?_gl=1*1mr1wl5*_ga*MTE1NDkyMjc2NS4xNjgzNzAxNzIw*_ga_8BKGRQKRPV*MTY4MzcwMTcyMC4xLjEuMTY4MzcwNTQ3NS42MC4wLjA.).  
如果您想在更严肃的演示上下文中查看该功能，请查看 Redis 自动完成演示。

## Updating the score: leader boards  
更新分数：排行榜

Just a final note about sorted sets before switching to the next topic. Sorted sets' scores can be updated at any time. Just calling [`ZADD`](https://redis.io/commands/zadd) against an element already included in the sorted set will update its score (and position) with O(log(N)) time complexity. As such, sorted sets are suitable when there are tons of updates.  
在切换到下一个主题之前，只是关于排序集的最后一点说明。排序集的分数可以随时更新。仅针对已包含在已排序集合中的元素调用 `ZADD` 将以 O(log(N)) 时间复杂度更新其分数（和位置）。因此，当有大量更新时，排序集是合适的。

Because of this characteristic a common use case is leader boards. The typical application is a Facebook game where you combine the ability to take users sorted by their high score, plus the get-rank operation, in order to show the top-N users, and the user rank in the leader board (e.g., "you are the #4932 best score here").  
由于这个特性，一个常见的用例是排行榜。典型的应用程序是 Facebook 游戏，在该游戏中，您结合了按高分对用户进行排序的功能，加上获取排名操作，以显示前 N 个用户，以及排行榜中的用户排名（例如，“你是这里的#4932 最高分”）。

## Bitmaps

Bitmaps are not an actual data type, but a set of bit-oriented operations defined on the String type. Since strings are binary safe blobs and their maximum length is 512 MB, they are suitable to set up to 2^32 different bits.  
位图不是一种实际的数据类型，而是定义在 String 类型上的一组面向位的操作。由于字符串是二进制安全的 blob，并且它们的最大长度为 512 MB，因此它们适合设置最​​多 2^32 个不同的位。

Bit operations are divided into two groups: constant-time single bit operations, like setting a bit to 1 or 0, or getting its value, and operations on groups of bits, for example counting the number of set bits in a given range of bits (e.g., population counting).  
位操作分为两组：恒定时间的单个位操作，如将位设置为 1 或 0，或获取其值，以及对位组的操作，例如计算给定位范围内设置位的数量（例如，人口统计）。

One of the biggest advantages of bitmaps is that they often provide extreme space savings when storing information. For example in a system where different users are represented by incremental user IDs, it is possible to remember a single bit information (for example, knowing whether a user wants to receive a newsletter) of 4 billion of users using just 512 MB of memory.  
位图的最大优点之一是它们在存储信息时通常可以极大地节省空间。例如，在一个系统中，不同的用户由递增的用户 ID 表示，仅使用 512 MB 的内存就可以记住 40 亿用户的单个位信息（例如，知道用户是否要接收新闻通讯）。

Bits are set and retrieved using the [`SETBIT`](https://redis.io/commands/setbit) and [`GETBIT`](https://redis.io/commands/getbit) commands:  
使用 `SETBIT` 和 `GETBIT` 命令设置和检索位：

```
> setbit key 10 1
(integer) 0
> getbit key 10
(integer) 1
> getbit key 11
(integer) 0
```

The [`SETBIT`](https://redis.io/commands/setbit) command takes as its first argument the bit number, and as its second argument the value to set the bit to, which is 1 or 0. The command automatically enlarges the string if the addressed bit is outside the current string length.  
`SETBIT` 命令的第一个参数是位数，第二个参数是设置位的值，即 1 或 0。如果寻址的位超出当前字符串长度，该命令会自动扩大字符串。

[`GETBIT`](https://redis.io/commands/getbit) just returns the value of the bit at the specified index. Out of range bits (addressing a bit that is outside the length of the string stored into the target key) are always considered to be zero.  
`GETBIT` 只返回指定索引处位的值。超出范围的位（寻址超出存储到目标密钥的字符串长度的位）始终被视为零。

There are three commands operating on group of bits:  
对位组进行操作的命令有以下三种：

1.  [`BITOP`](https://redis.io/commands/bitop) performs bit-wise operations between different strings. The provided operations are AND, OR, XOR and NOT.  
    `BITOP` 在不同的字符串之间执行按位运算。提供的操作是 AND、OR、XOR 和 NOT。
2.  [`BITCOUNT`](https://redis.io/commands/bitcount) performs population counting, reporting the number of bits set to 1.  
    `BITCOUNT` 执行人口计数，报告设置为 1 的位数。
3.  [`BITPOS`](https://redis.io/commands/bitpos) finds the first bit having the specified value of 0 or 1.  
    `BITPOS` 查找具有指定值 0 或 1 的第一个位。

Both [`BITPOS`](https://redis.io/commands/bitpos) and [`BITCOUNT`](https://redis.io/commands/bitcount) are able to operate with byte ranges of the string, instead of running for the whole length of the string. The following is a trivial example of [`BITCOUNT`](https://redis.io/commands/bitcount) call:  
`BITPOS` 和 `BITCOUNT` 都能够对字符串的字节范围进行操作，而不是对字符串的整个长度运行。以下是 `BITCOUNT` 调用的一个简单示例：

```
> setbit key 0 1
(integer) 0
> setbit key 100 1
(integer) 0
> bitcount key
(integer) 2
```

Common use cases for bitmaps are:  
位图的常见用例是：

-   Real time analytics of all kinds.  
    各种实时分析。
-   Storing space efficient but high performance boolean information associated with object IDs.  
    存储与对象 ID 关联的节省空间但高性能的布尔信息。

For example imagine you want to know the longest streak of daily visits of your web site users. You start counting days starting from zero, that is the day you made your web site public, and set a bit with [`SETBIT`](https://redis.io/commands/setbit) every time the user visits the web site. As a bit index you simply take the current unix time, subtract the initial offset, and divide by the number of seconds in a day (normally, 3600*24).  
例如，假设您想知道网站用户每日访问的最长连续时间。您从零开始计算天数，即您公开网站的那一天，并在每次用户访问该网站时用 `SETBIT` 设置一个位。作为位索引，您只需使用当前 unix 时间，减去初始偏移量，然后除以一天中的秒数（通常为 3600*24）。

This way for each user you have a small string containing the visit information for each day. With [`BITCOUNT`](https://redis.io/commands/bitcount) it is possible to easily get the number of days a given user visited the web site, while with a few [`BITPOS`](https://redis.io/commands/bitpos) calls, or simply fetching and analyzing the bitmap client-side, it is possible to easily compute the longest streak.  
这样，对于每个用户，您都有一个包含每天访问信息的小字符串。使用 `BITCOUNT` 可以很容易地得到给定用户访问网站的天数，而通过几个 `BITPOS` 调用，或者简单地获取和分析客户端的位图，可以很容易地计算最长条纹。

Bitmaps are trivial to split into multiple keys, for example for the sake of sharding the data set and because in general it is better to avoid working with huge keys. To split a bitmap across different keys instead of setting all the bits into a key, a trivial strategy is just to store M bits per key and obtain the key name with `bit-number/M` and the Nth bit to address inside the key with `bit-number MOD M`.  
位图很容易分成多个键，例如为了分片数据集，因为通常最好避免使用大键。要将位图拆分到不同的键中而不是将所有位设置为一个键，一个简单的策略是为每个键存储 M 位并使用 `bit-number/M` 获取键名称，并使用 `bit-number MOD M` 在键中寻址第 N 位.

## HyperLogLogs

A HyperLogLog is a probabilistic data structure used in order to count unique things (technically this is referred to estimating the cardinality of a set). Usually counting unique items requires using an amount of memory proportional to the number of items you want to count, because you need to remember the elements you have already seen in the past in order to avoid counting them multiple times. However there is a set of algorithms that trade memory for precision: you end with an estimated measure with a standard error, which in the case of the Redis implementation is less than 1%. The magic of this algorithm is that you no longer need to use an amount of memory proportional to the number of items counted, and instead can use a constant amount of memory! 12k bytes in the worst case, or a lot less if your HyperLogLog (We'll just call them HLL from now) has seen very few elements.  
HyperLogLog 是一种概率数据结构，用于计算独特的事物（从技术上讲，这是指估计集合的基数）。通常计算唯一项目需要使用与要计算的项目数成比例的内存量，因为您需要记住过去已经看到的元素以避免多次计算它们。然而，有一组算法以内存换取精度：您以标准误差的估计度量结束，在 Redis 实现的情况下，该标准误差小于 1%。该算法的神奇之处在于，您不再需要使用与计数的项目数成正比的内存量，而是可以使用恒定量的内存！在最坏的情况下为 12k 字节，如果您的 HyperLogLog（我们从现在开始称它们为 HLL）看到的元素很少，则更少。

HLLs in Redis, while technically a different data structure, are encoded as a Redis string, so you can call [`GET`](https://redis.io/commands/get) to serialize a HLL, and [`SET`](https://redis.io/commands/set) to deserialize it back to the server.  
Redis 中的 HLL，虽然在技术上是一种不同的数据结构，但被编码为 Redis 字符串，因此您可以调用 `GET` 序列化 HLL，调用 `SET` 将其反序列化回服务器。

Conceptually the HLL API is like using Sets to do the same task. You would [`SADD`](https://redis.io/commands/sadd) every observed element into a set, and would use [`SCARD`](https://redis.io/commands/scard) to check the number of elements inside the set, which are unique since [`SADD`](https://redis.io/commands/sadd) will not re-add an existing element.  
从概念上讲，HLL API 就像使用 Set 来完成相同的任务。您会将每个观察到的元素 `SADD` 放入一个集合中，并使用 `SCARD` 检查集合中元素的数量，这是唯一的，因为 `SADD` 不会重新添加现有元素。

While you don't really _add items_ into an HLL, because the data structure only contains a state that does not include actual elements, the API is the same:  
虽然您并没有真正将项目添加到 HLL 中，因为数据结构仅包含不包含实际元素的状态，但 API 是相同的：

-   Every time you see a new element, you add it to the count with [`PFADD`](https://redis.io/commands/pfadd).  
    每次你看到一个新元素时，你都会用 `PFADD` 将它添加到计数中。
    
-   Every time you want to retrieve the current approximation of the unique elements _added_ with [`PFADD`](https://redis.io/commands/pfadd) so far, you use the [`PFCOUNT`](https://redis.io/commands/pfcount).  
    每次你想检索到目前为止用 `PFADD` 添加的唯一元素的当前近似值时，你使用 `PFCOUNT` 。
    
    ```
      > pfadd hll a b c d
      (integer) 1
      > pfcount hll
      (integer) 4
    ```
    

An example of use case for this data structure is counting unique queries performed by users in a search form every day.  
此数据结构的一个用例示例是计算用户每天在搜索表单中执行的唯一查询。

Redis is also able to perform the union of HLLs, please check the [full documentation](https://redis.io/commands#hyperloglog) for more information.  
Redis 还能够执行 HLL 联合，请查看完整文档以获取更多信息。

## Other notable features 其他显着特点

There are other important things in the Redis API that can't be explored in the context of this document, but are worth your attention:  
Redis API 中还有其他重要内容无法在本文档的上下文中探讨，但值得您注意：

-   It is possible to [iterate the key space of a large collection incrementally](https://redis.io/commands/scan).  
    可以增量地迭代大型集合的键空间。
-   It is possible to run [Lua scripts server side](https://redis.io/commands/eval) to improve latency and bandwidth.  
    可以在服务器端运行 Lua 脚本来改善延迟和带宽。
-   Redis is also a [Pub-Sub server](https://redis.io/topics/pubsub).  
    Redis 也是一个 Pub-Sub 服务器。

## Learn more

This tutorial is in no way complete and has covered just the basics of the API. Read the [command reference](https://redis.io/commands) to discover a lot more.  
本教程绝不是完整的，它只涵盖了 API 的基础知识。阅读命令参考以发现更多信息。

Thanks for reading, and have fun hacking with Redis!  
感谢阅读，祝您使用 Redis 愉快！

