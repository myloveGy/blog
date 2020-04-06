---
title: PHP面试-基础一
date: 2019-05-09 18:24:35
tags:
- php
---

- **PHP中数组 + 和 array_merge 区别**

1. 当键名为数字时，array_merge不会覆盖原来的值，+会抛弃后面的值
```php
$array1 = [1, 2];
$array2 = [3, 4];

var_dump($array1 + $array2); // [1, 2]
var_dump(array_merge($array1, $array2)); // [1, 2, 3, 4]
```
2. 当键名为字符串时，+ 会舍弃掉后面键相同的元素，array_merge键相同的后面元素覆盖前面元素
```php
$array1 = ['user' => 1, 'name' => 2];
$array2 = ['user' => 3, 'name' => 4];

var_dump($array1 + $array2); // ['user' => 1, 'name' => 2]
var_dump(array_merge($array1, $array2)); // ['user' => 3, 'name' => 4]
```

- **GET和POST的区别？**

1. GET在浏览器回退时是无害的，而POST会再次提交请求。
2. GET产生的URL地址可以被Bookmark，而POST不可以。
3. GET请求会被浏览器主动cache，而POST不会，除非手动设置。
4. GET请求只能进行url编码，而POST支持多种编码方式。
5. GET请求参数会被完整保留在浏览器历史记录里，而POST中的参数不会被保留。
6. GET请求在URL中传送的参数是有长度限制的，而POST没有。
7. 对参数的数据类型，GET只接受ASCII字符，而POST没有限制。
8. GET比POST更不安全，因为参数直接暴露在URL上，所以不能用来传递敏感信息。
9. GET参数通过URL传递，POST放在Request body中。
10. GET产生一个TCP数据包，POST产生两个TCP数据包。

- **COOKIE和SESSION的区别？**
1. COOKIE保存在客户端，而SESSION则保存在服务器端
2. 从安全性来讲，SESSION的安全性更高
3. 从保存内容的类型的角度来讲，COOKIE只保存字符串（及能够自动转换成字符串）
4. 从保存内容的大小来看，COOKIE保存的内容是有限的，比较小，而SESSION基本上没有这个限制
5. 从性能的角度来讲，用SESSION的话，对服务器的压力会更大一些
SEEION依赖于COOKIE，但如果禁用COOKIE，也可以通过url传递
- **单引号和双引号的区别？**
1. 单引号不解析特殊字符，双引号能够解析变量和特殊字符串
2. 单引号的速度大于双引号

- **isset 和 empty 的区别？**
1. isset 判断变量是否已经定义，empty 判断一个变量是否为空
2. 当变量为空、空字符、0, isset 和 empty 返回都为 true
3. 当变量未定义、或者变量为null, isset 返回 false, empty 返回ture

- **echo、print、print_r、var_dump 的区别？**
1. echo: 可以一次输出多个值，多个值之间用逗号分隔。echo是语言结构(language construct)，而并不是真正的函数，因此不能作为表达式的一部分使用。
2. print: 函数print()打印一个值（它的参数），如果字符串成功显示则返回true，否则返回false。
3. print_r: 可以把字符串和数字简单地打印出来，而数组则以括起来的键和值得列表形式显示，并以Array开头。但print_r()输出布尔值和NULL的结果没有意义，因为都是打印"\n"。因此用var_dump()函数更适合调试。
4. var_dump: 判断一个变量的类型与长度,并输出变量的数值,如果变量有值输的是变量的值并回返数据类型。此函数显示关于一个或多个表达式的结构信息，包括表达式的类型与值。数组将递归展开值，通过缩进显示其结构。

- **什么是MVC？**

> MVC是一种软件架构的思想，将软件按照模型、视图、控制器
来划分。模型负责封装业务处理逻辑，视图负责输入和输出(
表示逻辑)，控制器负责协调模型和视图。

> * Model 模型：
    
>> 需要先写接口,然后实现接口中声明的方法。
业务处理逻辑：业务本身的处理流程，另外，还包括
为保证业务处理正常可靠执行的基础服务(事务、安全、
日志等等)。

> * View 视图:

>> 输入：提供相应的操作界面，方便用户使用。
输出：将模型返回的结果以合适的方式来展现。
> * Controller 控制器:

>> 协调: 视图向控制器发请求，由控制器来选择相应的
模型来处理；模型返回的结果给控制器，由控制器来
选择合适的视图，生成相应的界面给用户。

- **传值和传引用的区别？**
1. 按值传递：函数内对值的任何改变在函数外部都会被忽略。
2. 引用传递：函数内对值的任何改变在函数外部也能反映出这些修改。
3. 应用场景：按值传递时，php必须复制值，而按引用传递则不需要复制值，故引用传递一般用于大字符串或对象。

### 进阶篇

- 简述 S.O.L.I.D 设计原则

\- | - | -
--- | --- | ---
SRP	| 单一职责原则	| 一个类有且只有一个更改的原因
OCP	| 开闭原则	| 能够不更改类而扩展类的行为
LSP	| 里氏替换原则	| 派生类可以替换基类使用
ISP	| 接口隔离原则	| 使用客户端特定的细粒度接口
DIP	| 依赖反转原则	| 依赖抽象而不是具体实现

- PHP7 和 PHP5 的区别，具体多了哪些新特性？

> 1. 性能提升了两倍
> 2. 增加了结合比较运算符 (<=>)
> 3. 增加了标量类型声明、返回类型声明
> 4. `try...catch` 增加多条件判断，更多 Error 错误可以进行异常处理
> 5. 增加了匿名类，现在支持通过new class 来实例化一个匿名类，这可以用来替代一些“用后即焚”的完整类定义

- 为什么 PHP7 比 PHP5 性能提升了？

> 1. 变量存储字节减小，减少内存占用，提升变量操作速度
> 2. 改善数组结构，数组元素和 hash 映射表被分配在同一块内存里，降低了内存占用、提升了 cpu 缓存命中率
> 3. 改进了函数的调用机制，通过优化参数传递的环节，减少了一些指令，提高执行效率

- 简述一下 PHP 垃圾回收机制（GC）

> PHP 5.3 版本之前都是采用引用计数的方式管理内存，PHP 所有的变量存在一个叫 `zval` 的变量容器中，当变量被引用的时候，引用计数会+1，变量引用计数变为0时，PHP 将在内存中销毁这个变量。
>
> 但是引用计数中的循环引用，引用计数不会消减为 0，就会导致内存泄露。
>
> 在 5.3 版本之后，做了这些优化：
>
> 1. 并不是每次引用计数减少时都进入回收周期，只有根缓冲区满额后在开始垃圾回收；
> 2. 可以解决循环引用问题；
> 3. 可以总将内存泄露保持在一个阈值以下。

了解更多可以查看 PHP 手册，[垃圾回收机制](http://docs.php.net/manual/zh/features.gc.performance-considerations.php)。

- 如何解决 PHP 内存溢出问题

> 1. 增大 PHP 脚本的内存分配
> 2. 变量引用之后及时销毁
> 3. 将数据分批处理

- Redis、Memecached 这两者有什么区别？

> 1. Redis 支持更加丰富的数据存储类型，String、Hash、List、Set 和 Sorted Set。Memcached 仅支持简单的 key-value 结构。
> 2. Memcached key-value存储比 Redis 采用 hash 结构来做 key-value 存储的内存利用率更高。
> 3. Redis 提供了事务的功能，可以保证一系列命令的原子性
> 4. Redis 支持数据的持久化，可以将内存中的数据保持在磁盘中
> 5. Redis 只使用单核，而 Memcached 可以使用多核，所以平均每一个核上 Redis 在存储小数据时比 Memcached 性能更高。

- Redis 如何实现持久化？

> 1. RDB 持久化，将 Redis 在内存中的的状态保存到硬盘中，相当于备份数据库状态。
> 2. AOF 持久化（Append-Only-File），AOF 持久化是通过保存 Redis 服务器锁执行的写状态来记录数据库的。相当于备份数据库接收到的命令，所有被写入 AOF 的命令都是以 Redis 的协议格式来保存的。

### Web 安全防范

- CSRF 是什么？如何防范？

> CSRF（Cross-site request forgery）通常被叫做「跨站请求伪造」，可以这么理解：攻击者盗用用户身份，从而欺骗服务器，来完成攻击请求。

防范措施：

1. 使用验证码
2. 给每一个请求添加令牌 token 并验证

- XSS 是什么？如何防范？

> XSS(Cross Site Scripting)，跨站脚本攻击，攻击者往 Web 页面里插入恶意 Script 代码，当用户浏览该页之时，嵌入其中 Web 里面的 Script 代码会被执行，从而达到恶意攻击用户的目的。

防止 XSS 攻击的方式有很多，其核心的本质是：永远不要相信用户的输入数据，始终保持对用户数据的过滤。

- 什么是 SQL 注入？如何防范？

> SQL 注入就是攻击者通过一些方式欺骗服务器，结果执行了一些不该被执行的 SQL。

SQL 注入的常见场景

1. 数据库里被注入了大量的垃圾数据，导致服务器运行缓慢、崩溃。
2. 利用 SQL 注入暴露了应用程序的隐私数据

防范措施：

1. 保持对用户数据的过滤
2. 不要使用动态拼装 SQL
3. 增加输入验证，比如验证码
4. 对隐私数据加密，禁止明文存储

### 说明
* 本文载自：[https://github.com/todayqq/PHPerInterviewGuide/blob/master/php.md](https://github.com/todayqq/PHPerInterviewGuide/blob/master/php.md)

### 扩展阅读

- [3年PHPer的面试总结](http://coffeephp.com/articles/4?utm_source=laravel-china.org)
- [垃圾回收机制](http://docs.php.net/manual/zh/features.gc.performance-considerations.php)
- [S.O.L.I.D 面向对象设计](https://laravel-china.org/articles/4160/solid-object-oriented-design-and-programming-oodoop-notes?order_by=created_at&)
- [浅谈IOC--说清楚IOC是什么](http://www.cnblogs.com/DebugLZQ/archive/2013/06/05/3107957.html)
- [Redis和Memcached的区别](https://www.biaodianfu.com/redis-vs-memcached.html)
- [CSRF攻击与防御](https://www.cnblogs.com/phpstudy2015-6/p/6771239.html)
- [XSS跨站脚本攻击](https://www.cnblogs.com/phpstudy2015-6/p/6767032.html#_label9)
