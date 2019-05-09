---
title: PHP面试-并发
date: 2019-05-09 18:35:33
tags:
- php
---

#### 目录
- [高并发解决方案](#高并发解决方案)
- [缓存](#缓存)
- [MySQL数据库](#数据库)
- [Nginx](#脚本执行时间超时)

# 高并发解决方案

- 硬件方面：
    
    服务器性能： 网络-硬盘读写速度-内存大小-CPU处理数度
- 软件方面：
    - 服务器： 使用nginx 代替 apache, nginx 
    - 开启GZIP压缩优化网站、压缩网站内容可以节省网站流量
    - 构架部署：
        - HTML静态化
        - 前端资源服务器分离(文件单独部署服务器)
        - 数据库集群、库表散列（数据库读写分离、使用索引、分表）
        - 缓存（redis、memcache）
        - 负载均衡
        - CDN加速
- PHP:
    - 升级PHP版本到PHP7
    - 加载近可能少的模块（关闭不需要的模块）
    - 使用Opcache

- 代码层面的优化：
    - SQL查询的优化
    - 尽量多使用内置函数

# 缓存
## Redis

### Redis 简介：
    
Redis 是一个 key/value存储系统。和Memcache类似，它支持存储的value类型相当更多。包括 string(字符串)、list(链表)、set(集合)和zset(有序集合)。这些数据类型都支持 push/pop、add/remove 及取交集和差集及更丰富的操作，而且这些操作都是原子性的。这此基础上，Redis 支持各种不同方式的排序。与Memcached 一样，为了保证效率，数据都是缓存在内存中。区别是 Redis 会周期性的把跟新数据写入磁盘或者把修改操作写入追加的记录文件，并且在此基础上实现master-slave（主从）同步
    
### Redis支持的数据类型

#### String(字符串)
        
string是redis最基本的类型，你可以理解成与Memcached一模一样的类型，一个key对应一个value。
string类型是二进制安全的。意思是redis的string可以包含任何数据。比如jpg图片或者序列化的对象.string类型是Redis最基本的数据类型，一个键最大能存储512MB。

```
set name 'string'
get name
string
```
#### Hash(哈希)

Redis hash 是一个键名对集合。
Redis hash是一个string类型的field和value的映射表，hash特别适合用于存储对象

```
# 设置key 为 user 的 name 字段为 liujx
hset user name liujx
hget user name
liujx

# 同时设置多个字段
hmset use name ljx age 18 
hmget user name age
ljx
18
```
#### List(列表)
Redis 列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）。
```
lpush users liujx
rpop users
liujx
```
#### Set(集合)

Redis的Set是string类型的无序集合。
集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1)。

```
sadd key member
```

#### zset(sorted set：有序集合)
Redis zset 和 set 一样也是string类型元素的集合,且不允许重复的成员。
不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。
zset的成员是唯一的,但分数(score)却可以重复。
```
zadd key member
```

## Memcache
    
Memcache 简介：

Memcache 是一种高性能、分布式对象缓存系统，最初设计与缓解动态网站数据库加载的延迟性，你可以把它想象成一个大的内存HashTable,就是一个 key/value 键值对缓存

### Redis 和 Memcache 区别
1. Redis 支持更加丰富的数据存储类型，String、Hash、List、Set 和 Sorted Set。Memcached 仅支持简单的 key-value 结构。
2. Memcached key-value存储比 Redis 采用 hash 结构来做 key-value 存储的内存利用率更高。
3. Redis 提供了事务的功能，可以保证一系列命令的原子性
4. Redis 支持数据的持久化，可以将内存中的数据保持在磁盘中
5. Redis 只使用单核，而 Memcached 可以使用多核，所以平均每一个核上 Redis 在存储小数据时比 Memcached 性能更高。

#### Redis 如何实现持久化
1. RDB 持久化，将 Redis 在内存中的的状态保存到硬盘中，相当于备份数据库状态。
2. AOF 持久化（Append-Only-File），AOF 持久化是通过保存 Redis 服务器锁执行的写状态来记录数据库的。相当于备份数据库接收到的命令，所有被写入 AOF 的命令都是以 Redis 的协议格式来保存的。

# 数据库
## 存储引擎
###  MyISAM 与 InnoDB的区别
-  InnoDB支持事务，MyISAM不支持，对于InnoDB每一条SQL语言都默认封装成事务，自动提交，这样会影响速度，所以最好把多条SQL语言放在begin和commit之间，组成一个事务
-  InnoDB支持外键，而MyISAM不支持
-  InnoDB是聚集索引，数据文件是和索引绑在一起的，必须要有主键，通过主键索引效率很高;而MyISAM是非聚集索引，数据文件是分离的，索引保存的是数据文件的指针。主键索引和辅助索引是独立的
-   Innodb不支持全文索引，而MyISAM支持全文索引，查询效率上MyISAM要高
-   MyISAM只支持表级锁,Innodb支持行级锁;但是InnoDB的行锁，只是在WHERE的主键是有效的，非主键的WHERE都会锁全表的

## 索引
作用
- 能够大大提高查询效率
- 可以通过建立唯一索引或者主键索引,保证数据库表中每一行数据的唯一性.
- 建立索引可以大大提高检索的数据,以及减少表的检索行数
- 在分组和排序字句进行数据检索,可以减少查询时间中 分组 和 排序时所消耗的时间(数据库的记录会重新排序)

索引也会有它的缺点：虽然索引大大提高了查询速度，同时却会降低更新表的速度，如对表进行INSERT、UPDATE和DELETE。因为更新表时，MySQL不仅要保存数据，还要保存一下索引文件。建立索引会占用磁盘空间的索引文件

### 普通索引

这是最基本的索引，它没有任何限制，比如上文中为title字段创建的索引就是一个普通索引，MyIASM中默认的BTREE类型的索引，也是我们大多数情况下用到的索引。
    
```
–- 直接创建索引
CREATE INDEX index_name ON table(column(length))

–- 修改表结构的方式添加索引
ALTER TABLE table_name ADD INDEX index_name ON (column(length))

–- 创建表的时候同时创建索引
CREATE TABLE `table` (
    `id` int(11) NOT NULL AUTO_INCREMENT,
    `title` char(255) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL ,
    `content` text CHARACTER SET utf8 COLLATE utf8_general_ci NULL ,
    `time` int(10) NULL DEFAULT NULL,
    PRIMARY KEY (`id`),
    INDEX index_name (title(length))
)

–- 删除索引
DROP INDEX index_name ON table    
```

### 唯一索引

与普通索引类似，不同的就是：索引列的值必须唯一，但允许有空值（注意和主键不同）。如果是组合索引，则列值的组合必须唯一，创建方法和普通索引类似。

```
–- 创建唯一索引
CREATE UNIQUE INDEX indexName ON table(column(length))

–- 修改表结构
ALTER TABLE table_name ADD UNIQUE indexName ON (column(length))

–- 创建表的时候直接指定
CREATE TABLE `table` (
    `id` int(11) NOT NULL AUTO_INCREMENT ,
    `title` char(255) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL ,
    `content` text CHARACTER SET utf8 COLLATE utf8_general_ci NULL ,
    `time` int(10) NULL DEFAULT NULL ,
    PRIMARY KEY (`id`),
    UNIQUE indexName (title(length))
);
```

### 全文索引

MySQL从3.23.23版开始支持全文索引和全文检索，FULLTEXT索引仅可用于 MyISAM 表；他们可以从CHAR、VARCHAR或TEXT列中作为CREATE TABLE语句的一部分被创建，或是随后使用ALTER TABLE 或CREATE INDEX被添加。////对于较大的数据集，将你的资料输入一个没有FULLTEXT索引的表中，然后创建索引，其速度比把资料输入现有FULLTEXT索引的速度更为快。不过切记对于大容量的数据表，生成全文索引是一个非常消耗时间非常消耗硬盘空间的做法。
```
–- 修改表结构添加全文索引
ALTER TABLE article ADD FULLTEXT index_content(content)

–- 直接创建索引
CREATE FULLTEXT INDEX index_content ON article(content)

–- 创建表的时候直接指定
CREATE TABLE `table` (
    `id` int(11) NOT NULL AUTO_INCREMENT ,
    `title` char(255) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL ,
    `content` text CHARACTER SET utf8 COLLATE utf8_general_ci NULL ,
    `time` int(10) NULL DEFAULT NULL ,
    PRIMARY KEY (`id`),
    FULLTEXT (content)
);
```

### 单列索引、多列索引
多个单列索引与单个多列索引的查询效果不同，因为执行查询时，MySQL只能使用一个索引，会从多个索引中选择一个限制最为严格的索引。
### 组合索引(最左前缀)
多个单列索引与单个多列索引的查询效果不同，因为执行查询时，MySQL只能使用一个索引，会从多个索引中选择一个限制最为严格的索引。
```
-- 建立的索引
ALTER TABLE `article` ADD INDEX `index_titme_time` (`title`(50), `time`(10));

-- 使用上面索引
SELECT * FROM `article` WHREE `title` = '测试' AND `time` = 1234567890;
SELECT * FROM `article` WHREE `title` = '测试';

-- 没有使用索引
SELECT * FROM `article` WHREE `time` = 1234567890;
```
#### 创建原则
- 选择唯一性索引
- 为经常需要排序、分组和联合操作的字段建立索引
- 为常作为查询条件的字段建立索引
- 限制索引的数目
- 尽量使用数据量少的索引
- 尽量使用前缀来索引
- 删除不再使用或者很少使用的索引
- 最左前缀匹配原则，非常重要的原则
- 尽量选择区分度高的列作为索引
- 索引列不能参与计算，保持列“干净”
- 尽量的扩展索引，不要新建索引

#### 索引优化
MySQL只对一下操作符才使用索引：<,<=,=,>,>=,between,in,以及某些时候的like(不以通配符%或_开头的情形)


## SQL优化

#### 索引问题
- 避免对索引字符进行计算
- 避免在索引字段上面使用not,<>,!=
- 避免在索引字段列上使用IS NULL 和 IS NOT NULL
- 避免在索引字段列上出现数据类型转换
- 避免在索引字段上使用函数
- 避免建立的索引列中存在空值

#### 前导模糊查询不能使用索引
```
-- 没有使用索引
SELECT * FROM `order` WHERE `desc` LIKE '%abc'

-- 使用索引
SELECT * FROM `order` WHERE `desc` LIKE 'abc%'
```

#### 明确知道只有一条数据,limit 1 可以提高效率

#### 不用使用SELECT * 而是指定查询字段

# Nginx
## nginx + php-fpm 出现502问题排查

nginx出现502有很多原因，但大部分原因可以归结为资源数量不够用,也就是说后端php-fpm处理有问题，nginx将正确的客户端请求发给了后端的php-fpm进程，但是因为php-fpm进程的问题导致不能正确解析php代码，最终返回给了客户端502错误。

#### php-fpm 进程数不够用

使用 netstat -napo |grep "php-fpm" | wc -l 查看一下当前fastcgi进程个数，如果个数接近conf里配置的上限，就需要调高进程数。
但也不能无休止调高，可以根据服务器内存情况，可以把php-fpm子进程数调到100或以上，在4G内存的服务器上200就可以
    
#### 调高调高linux内核打开文件数量
    
可以使用这些命令(必须是root帐号)

```
echo 'ulimit -HSn 65536' >> /etc/profile
echo 'ulimit -HSn 65536' >> /etc/rc.local
source /etc/profile
```

#### 脚本执行时间超时

如果脚本因为某种原因长时间等待不返回 ，导致新来的请求不能得到处理，可以适当调小如下配置。
- nginx.conf里面主要是如下
```
fastcgi_connect_timeout 300;
fastcgi_send_timeout 300;
fastcgi_read_timeout 300;
```
- php-fpm.conf里如要是如下

```
request_terminate_timeout = 10s
```
#### 缓存设置比较小

修改或增加配置到nginx.conf

```
proxy_buffer_size 64k;
proxy_buffers  512k;
proxy_busy_buffers_size 128k;
```

这部分参见: [http://www.nginx.cn/102.html](http://www.nginx.cn/102.html)
