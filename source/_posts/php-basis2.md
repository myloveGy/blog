---
title: PHP面试-基础二
date: 2019-05-09 18:32:26
tags:
- php
---

## 目录
  1. [PHP基础数据类型](#基础数据类型)
  2. [PHP预定义变量](#预定义变量)
  3. [PHP魔术函数](#魔术函数)
  4. [PHP魔术常量](#魔术常量)
  5. [COOKE和SESSION](#魔术常量)
  6. [PHP设计模式](#设计模式)
  7. [PHP实现排序算法](#排序算法)
  8. [PHP编码规范PSR](https://psr.phphub.org/)

# 基础数据类型
  ● 四种标量类型
  1. [Boolean](https://secure.php.net/manual/zh/language.types.boolean.php) 	布尔类型;判断方法 is_bool(); 类型强转(bool),(boolean)
  2. [Integer](https://secure.php.net/manual/zh/language.types.integer.php) 	整型;判断方法is_integer();类型强转(int),(integer)
  3. [Float](https://secure.php.net/manual/zh/language.types.float.php) 		浮点型，也称作 Double；判断方法is_float();类型强转(float),(double),(real)
  4. [String](https://secure.php.net/manual/zh/language.types.string.php) 		字符型；判断方法is_string();类型强转(string)
  
  ● 两种复合类型
  1. [Array](https://secure.php.net/manual/zh/language.types.array.php) 		数组;判断方法is_array();类型强转(array)
  2. [Object](https://secure.php.net/manual/zh/language.types.object.php)		对象	;判断方法is_object();类型强转(object)
  
● 两种特殊类型
  1. [Resource](https://secure.php.net/manual/zh/language.types.resource.php) 	资源;判断方法is_resource()
  2. [NULL](https://secure.php.net/manual/zh/language.types.null.php) 		NULL;判断方法 is_null();类型强转(unset)
  
● 伪类型

   伪类型（pseudo-types） 是 PHP 文档里用于指示参数可以使用的类型和值。 请注意，它们不是 PHP 语言里原生类型。 所以不能把伪类型用于自定义函数里的类型约束（typehint）。
  1. [mixed](https://secure.php.net/manual/zh/language.pseudo-types.php#language.types.mixed)
  2. [number](https://secure.php.net/manual/zh/language.pseudo-types.php#language.types.number)
  3. [callback](https://secure.php.net/manual/zh/language.pseudo-types.php#language.types.callback)
  
- 补充说明：

1. 通过gettype()获取变量的类型字符串
2. 字符串单引号和双引号的区别

    - 转义字符不同     
    - 对变量的解析不同(双引号解析变量)
    - 解析速度不同(单引号快)


# 预定义变量
- [$GLOBALS](https://secure.php.net/manual/zh/reserved.variables.globals.php)  引用全局作用域中可用的全部变量
- [$_SERVER](https://secure.php.net/manual/zh/reserved.variables.server.php)  这种超全局变量保存关于报头、路径和脚本位置的信息。
- [$_REQUEST](https://secure.php.net/manual/zh/reserved.variables.request.php)     HTTP Request 变量(默认情况下包含了 $_GET，$_POST 和 $_COOKIE 的数组。)
- [$_GET](https://secure.php.net/manual/zh/reserved.variables.get.php)     通过 URL 参数传递给当前脚本的变量的数组。 
- [$_POST](https://secure.php.net/manual/zh/reserved.variables.post.php)    当 HTTP POST 请求的 Content-Type 是 application/x-www-form-urlencoded 或 multipart/form-data 时，会将变量以关联数组形式传入当前脚本
- [$_FILES](https://secure.php.net/manual/zh/reserved.variables.files.php)       通过 HTTP POST 方式上传到当前脚本的项目的数组
- [$_ENV](https://secure.php.net/manual/zh/reserved.variables.environment.php)         通过环境方式传递给当前脚本的变量的数组。
- [$_COOKIE](https://secure.php.net/manual/zh/reserved.variables.cookies.php)      通过 HTTP Cookies 方式传递给当前脚本的变量的数组。
- [$_SESSION](https://secure.php.net/manual/zh/reserved.variables.session.php)     当前脚本可用 SESSION 变量的数组。
- 补充说明$_GET和$_POST区别

1.  GET使用URL或Cookie传参。而POST将数据放在BODY中。
2.  GET的URL会有长度上的限制，则POST的数据则可以非常大。
3.  POST比GET安全，因为数据在地址栏上不可见。
  
# 魔术函数
- [__construct()](https://secure.php.net/manual/zh/language.oop5.decon.php#object.construct) 

    PHP 5 允行开发者在一个类中定义一个方法作为构造函数。具有构造函数的类会在每次创建新对象时先调用此方法，所以非常适合在使用对象之前做一些初始化工作。
- [__destruct()](https://secure.php.net/manual/zh/language.oop5.decon.php#object.destruct)

    PHP 5 引入了析构函数的概念，这类似于其它面向对象的语言，如 C++。析构函数会在到某个对象的所有引用都被删除或者当对象被显式销毁时执行。
- [__call()](https://secure.php.net/manual/zh/language.oop5.overloading.php#object.call)

    在对象中调用一个不可访问方法时，__call() 会被调用。
- [__callStatic()](https://secure.php.net/manual/zh/language.oop5.overloading.php#object.callstatic)
    
    在静态上下文中调用一个不可访问方法时，__callStatic() 会被调用。
- [__get()](https://secure.php.net/manual/zh/language.oop5.overloading.php#object.get)

    读取不可访问属性的值时，__get() 会被调用。
- [__set()](https://secure.php.net/manual/zh/language.oop5.overloading.php#object.set) 

    在给不可访问属性赋值时，__set() 会被调用。
- [__isset()](https://secure.php.net/manual/zh/language.oop5.overloading.php#object.isset)

    当对不可访问属性调用 isset() 或 empty() 时，__isset() 会被调用
- [__unset()](https://secure.php.net/manual/zh/language.oop5.overloading.php#object.unset)

    当对不可访问属性调用 unset() 时，__unset() 会被调用。
- [__sleep()](https://secure.php.net/manual/zh/language.oop5.magic.php#object.sleep)
    
    serialize() 函数会检查类中是否存在一个魔术方法 __sleep()。如果存在，该方法会先被调用，然后才执行序列化操作。
- [__wakeup()](https://secure.php.net/manual/zh/language.oop5.magic.php#object.wakeup)

    与之相反，unserialize() 会检查是否存在一个 __wakeup() 方法。如果存在，则会先调用 __wakeup 方法，预先准备对象需要的资源。
- [__toString()](https://secure.php.net/manual/zh/language.oop5.magic.php#object.tostring)

    __toString() 方法用于一个类被当成字符串时应怎样回应。
- [__invoke()](https://secure.php.net/manual/zh/language.oop5.magic.php#object.invoke)

    当尝试以调用函数的方式调用一个对象时，__invoke() 方法会被自动调用。
- [__set_state()](https://secure.php.net/manual/zh/language.oop5.magic.php#object.set-state)

    PHP 5.1.0 起当调用 var_export() 导出类时，此静态 方法会被调用。
- [__clone()](https://secure.php.net/manual/zh/language.oop5.cloning.php#object.clone)

    当复制完成时，如果定义了 __clone() 方法，则新创建的对象（复制生成的对象）中的 __clone() 方法会被调用，可用于修改属性的值（如果有必要的话）。    
- [__debugInfo()](https://secure.php.net/manual/zh/language.oop5.magic.php#object.debuginfo)
    
    当转储对象以获取应显示的属性时，此方法由var_dump（）调用。 如果方法未在对象上定义，那么将显示所有public，protected和private属性。
- [__autoload](https://secure.php.net/manual/zh/function.autoload.php)
    
    尝试加载未定义的类(该方法在PHP7.2中已经删除)

# 魔术常量
- [\_\_LINE\_\_](#魔术常量) 当前文件中的行号
- [\_\_FILE\_\_](#魔术常量) 返回文件的完整路径和文件名。如果用在包含文件中，则返回包含文件名。自 PHP 4.0.2 起，__FILE__ 总是包含一个绝对路径，而在此之前的版本有时会包含一个相对路径。
- [\_\_FUNCTION\_\_](#魔术常量) 返回文件的完整路径和文件名。如果用在包含文件中，则返回包含文件名。自 PHP 4.0.2 起，__FILE__ 总是包含一个绝对路径，而在此之前的版本有时会包含一个相对路径。
- [\_\_CLASS\_\_](#魔术常量) 返回类的名称（PHP 4.3.0 新加）。自 PHP 5 起本常量返回该类被定义时的名字（区分大小写）。在 PHP 4 中该值总是小写字母的。
- [\_\_METHOD\_\_](#魔术常量) 返回类的方法名（PHP 5.0.0 新加）。返回该方法被定义时的名字（区分大小写）

# COOKE和SESSION

#### COOKIE
```
/**
 * 设置cookie
 * @param string $name  cookie 名称
 * @param string $value  cookie 值
 * @param int      $expire cookie 保存时间
 * @param string $path  cookie 有效目录
 * @param string $domain cookie 保存域名
 * @param bool   $secure   cookie  是否通过HTTPS协议传输
 * @param bool   $httponly cookie 不允许客户端修改
 */
bool setcookie(string $name[, string $value = ''[, int $expire = 0[, string $path = '' [, string $domain = '' [, bool $secure = false[, bool $httponly = false ]]]]]])
```
#### SESSION
- 设置相关
1) session.save_handler = file
     用于读取/回写session数据的方式，默认是files
2) session.save_path = "/var/lib/php/session"
     指定保存session文件的目录，可以指定到别的目录，但是指定目录必须要有httpd守护进程属主(比如apache或www等)写权限，否则无法回存session数据
3) session.auto_start = 0
     如果启用该选项，用户的每次请求都会初始化session。我们推荐不启用该设置，最好通过session_start()显示地初始化session

- 相关函数

session_start:                          初始 session。

session_destroy:                     结束 session。

session_unset:                        释放session内存。

session_name:                        存取目前 session 名称。

session_module_name:           存取目前 session 模块。

session_save_path:                 存取目前 session 路径。

session_id:                              存取目前 session 代号。

session_register:                     注册新的变量。

session_unregister:                 删除已注册变量。

session_is_registered:             检查变量是否注册。

session_decode:                     Session 资料解码。

session_encode:                     Session 资料编码。

1. session_start()
     函数session_start会初始化session，也标识着session生命周期的开始。要使用session，必须初始化一个session环境。有点类似于OOP概念中调用构造函数构创建对象实例一样。
session初始化操作，声明一个全局数组$_SESSION，映射寄存在内存的session数据。如果session文件已经存在，并且保存有session数据，session_start()则会读取session数据，填入$_SESSION中，开始一个新的session生命周期
2. $_SESSION
     它是一个全局变量，类型是Array,映射了session生命周期的session数据，寄存在内存中。在session初始化的时候，从session文件中读取数据，填入该变量中。在session生命周期结束时，将$_SESSION数据写回session文件。
3. session_register()
     在session生命周期内，使用全局变量名称将注全局变量注册到当前session中。所谓注册，就是将变量填入$_SESSION中，值为NULL。它不会对session文件进行任何IO操作，只是影响$_SESSION变量。注意，它的正确写法是session_register(‘varname’)，而不是session_register($varname)
4. session_unregister()
     与session_register操作正好相反，即在session生命周期,从当前session注销指定变量。同样只影响$_SESSION，并不进行任何IO操作
5. session_unset()
     在session生命周期，从当前session中注销全部session数据，让$_SESSION成为一个空数组。它与unset($_SESSION)的区别在于:unset直接删除$_SESSION变量，释放内存资源;另一个区别在于，session_unset()仅在session生命周期能够操作$_SESSION数组，而unset()则在整个页面(page)生命周期都能操作$_SESSION数组。session_unset()同样不进行任何IO操作，只影响$_SESSION数组
6. session_destroy()
      如果说session_start()初始化一个session的话，而它则注销一个session。意味着session生命周期结束了。在session生命周期结整后，session_register, session_unset, session_register都将不能操作$_SESSION数组，而$_SESSION数组依然可以被unset()等函数操作。这时，session意味着是未定义的，而$_SESSION依然是一个全局变量，他们脱离了关映射关系。 通过session_destroy()注销session,除了结束session生命周期外，它还会删除sesion文件，但不会影响当前$_SESSION变量。即它会产生一个IO操作
7. session_regenerate_id()
     调用它，会给当前用户重新分配一个新的session id。并且在结束当前页面生命周期的时候，将当前session数据写入session文件。前提是，调用此函数之前，当前session生命周期没有被终止（参考第9点）。它会产生一个IO操作，创建一个新的session文件，创建新的session文件的是在session结束之前，而不是调用此函数就立即创建新的session文件。
8. session_commit()
     session_commit()函数是session_write_close()函数的别名。它会结束当前session的生命周期，并且将session数据立即强制写入session文件。不推荐通过session_commit()来手工写入session数据，因为PHP会在页面生命周期结束的时候，自动结束当前没有终止的session生命周期。它会产生一个IO写操作
9. session_end()
     结束session，默认是在页面生命周期结束的之前，PHP会自动结束当前没有终止的session。但是还可以通过session_commit()与session_destroy()二个函数提前结束session。不管是哪种方式，结束session都会产生IO操作，分别不一样。默认情况，产生一个IO写操作，将当前session数据写回session文件。session_commit()则是调用该函数那刻，产生一个IO写操作，将session数据写回session文件。而session_destroy()不一样在于，它不会将数据写回session文件，而是直接删除当前session文件。有趣的是，不管是session_commit()，还是session_destroy()都不会清空$_SESSION数组，更不会删除$_SESSION数组，只是所有session_*函数不能再操作session数据，因为当前的session生命周期终止了，即不能操作一个未定义对象。

# 设计模式
- 一、单例设计模式

单例模式的要点有三个：

1. 一是某个类只能有一个实例

2. 二是它必须自行创建这个实例

3. 三是它必须自行向整个系统提供这个实例

为什么要使用PHP单例模式

php的应用主要在于数据库应用, 一个应用中会存在大量的数据库操作, 在使用面向对象的方式开发时, 如果使用单例模式, 则可以避免大量的new 操作消耗的资源,还可以减少数据库连接这样就不容易出现 too many connections情况。

一个类来全局控制某些配置信息

```
<?php

namespace lib\mode;

/**
 * Class Man
 * @package lib\mode
 * @desc 设计模式单例模式
 * 1 私有化构造函数和克隆函数
 * 2 提供方法实例化对象
 */
class Man
{
    // 保存实例化对象属性
    private static $instance;

    /**
     * Man constructor.
     * @desc 私有化构造函数
     */
    private function __construct()
    {

    }

    /**
     * __clone() 私有化克隆函数
     */
    private function __clone()
    {
        return self::$instance;
    }

    /**
     * getInstance() 提供公共方法获取对象
     * @return Man
     */
    public static function getInstance()
    {
        if (!self::$instance) {
            self::$instance = new self();
        }

        return self::$instance;
    }

    /**
     * 其他方法
     */
    public function test()
    {
        echo '单例设计模式';
    }
}
```

- 二、简单工厂模式

1. 抽象基类：类中定义抽象一些方法，用以在子类中实现
2. 继承自抽象基类的子类：实现基类中的抽象方法

    - 工厂类：用以实例化所有相对应的子类

```
<?php

namespace lib\mode;

/**
 * Class Factory
 * @package lib\mode
 * @desc 设计模式 简单工厂模式
 * 1、提供一个工厂类，生产出指定类
 * 2、提供一个抽象类，设计出需要抽象的方法，让子类实现
 * 3、子类实现抽象基类
 */
class Factory
{
    /**
     * createObject() 工厂提供创建对象方法
     * @param  string      $name    需要创建的类名
     * @param  mixed|null  $config  配置信息
     * @return bool
     */
    public static function createObject($name, $config = null)
    {
        $mixReturn = false;
        if ($name) {
            $name = '\\lib\\mode\\'.$name;
            if(class_exists($name)) {
                try {
                  $mixReturn = new $name($config);
                } catch (\Exception $e) {
                    $mixReturn = false;
                }
            }
        }

        return $mixReturn;
    }
}

/**
 * Class Operation
 * @package lib\mode
 * @desc 抽象基类， 提供抽象方法
 */
abstract class Operation
{
    /**
     * getValue() 提供抽象方法，计算两个值的操作后的值
     * @param $value1
     * @param $value2
     * @return mixed
     */
    abstract public function getValue($value1, $value2);
}

/**
 * Class Add
 * @package lib\mode
 * @desc 加法类
 */
class Add extends Operation
{
    public function getValue($value1, $value2)
    {
        return $value1 + $value2;
    }
}

/**
 * Class Subtract
 * @package lib\mode
 * @desc 减法类
 */
class Subtract extends Operation
{
    public function getValue($value1, $value2)
    {
        return $value1 - $value2;
    }
}

// 使用 加法类
$obj = Factory::createObject('Add');
var_dump($obj->getValue(8, 13));

// 使用 减法类
$obj = Factory::createObject('Subtract');
var_dump($obj->getValue(8, 13));

```


- 三、观察者模式
      定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新【GOF95】 又称为发布-订阅（Publish-Subscribe）模式、模型-视图（Model-View）模式、源-监听（Source-Listener）模式、或从属者(Dependents)模式

1. 抽象主题（Subject）角色：主题角色将所有对观察者对象的引用保存在一个集合中，每个主题可以有任意多个观察者。 抽象主题提供了增加和删除观察者对象的接口。

2. 抽象观察者（Observer）角色：为所有的具体观察者定义一个接口，在观察的主题发生改变时更新自己。

3. 具体主题（ConcreteSubject）角色：存储相关状态到具体观察者对象，当具体主题的内部状态改变时，给所有登记过的观察者发出通知。具体主题角色通常用一个具体子类实现。

4. 具体观察者（ConcretedObserver）角色：存储一个具体主题对象，存储相关状态，实现抽象观察者角色所要求的更新接口，以使得其自身状态和主题的状态保持一致


```
<?php
namespace lib\mode;

/**
 * Class Observe
 * @package lib\mode
 * @desc 设计模式 观察者模式
 * 1、监听器
 * 2、被观察者
 */

/**
 * Class Subject
 * @package lib\mode
 * @desc 抽象主题角色
 */
class Subject implements \SplSubject
{
    private $observers;
    private $name;

    public function __construct($name)
    {
        $this->observers = new \SplObjectStorage();
        $this->name = $name;
    }

    /**
     * attach() 添加一个新的观察者对象
     * @param \SplObserver $observer
     */
    public function attach(\SplObserver $observer)
    {
        $this->observers->attach($observer);
    }

    /**
     * detach() 删除一个已经注册过的观察者
     * @param \SplObserver $observer
     */
    public function detach(\SplObserver $observer)
    {
        $this->observers->detach($observer);
    }

    public function notify()
    {
        foreach ($this->observers as $observer) {
            $observer->update($this);
        }
    }

    public function getName()
    {
        return $this->name;
    }
}

/**
 * Class Observe
 * @package lib\mode
 * @desc 观察者角色信息类
 */
class Observe implements \SplObserver
{
    public function update(\SplSubject $subject)
    {
        var_dump(__CLASS__. '-' . $subject->getName());
    }
}

/**
 * Class Observe
 * @package lib\mode
 * @desc 观察者角色信息类1
 */
class Observe1 implements \SplObserver
{
    public function update(\SplSubject $subject)
    {
        var_dump(__CLASS__. '-' . $subject->getName());
    }
}

// 使用说明
$observer1 = new Observe();
$observer2 = new Observe1();
$subject = new Subject("test");

$subject->attach($observer1);
$subject->attach($observer2);

$subject->notify();
```

# 排序算法
- 冒泡排序

    思路分析：在要排序的一组数中，对当前还未排好的序列，从前往后对相邻的两个数依次进行比较和调整，让较大的数往下沉，较小的往上冒。即，每当两相邻的数比较后发现它们的排序与排序要求相反时，就将它们互换。
```

/**
 * 冒泡排序
 * @param array $array
 * @return array
 */
function bubbleSort($array)
{
    $length = count($array);
    if ($length > 1) {
        for ($i = 0; $i < $length; $i++) {
            for ($x = 0; $x < ($length - $i - 1); $x++) {
                if ($array[$x] > $array[$x + 1]) {
                    $tmp = $array[$x + 1];
                    $array[$x + 1] = $array[$x];
                    $array[$x] = $tmp;
                }
            }
        }
    }

    return $array;
}

```
- 快速排序

    找到当前数组中的任意一个元素（一般选择第一个元素），作为标准，新建两个空数组，遍历整个数组元素，
如果遍历到的元素比当前的元素要小，那么就放到左边的数组，否则放到右面的数组，然后再对新数组进行同样的操作，
不难发现，这里符合递归的原理，所以我们可以用递归来实现。

使用递归，则需要找到递归点和递归出口：
递归点：如果数组的元素大于1，就需要再进行分解，所以我们的递归点就是新构造的数组元素个数大于1

递归出口：我们什么时候不需要再对新数组不进行排序了呢？就是当数组元素个数变成1的时候，所以这就是我们的出口。

```
/**
 * 快速排序
 * @param array $array
 * @return array
 */
function quickSort(array $array)
{
    if ($array && is_array($array)) {
        $length = count($array);
        if ($length > 1) {
            $left = $right = [];
            for ($i = 1; $i < $length; $i ++) {
                // 判断当前元素的大小和第一个元素比较
                if ($array[$i] < $array[0]) {
                    $left[] = $array[$i];
                } else {
                    $right[] = $array[$i];
                }
            }

            // 递归调用
            if ($left) {
                $left = quickSort($left);
            }

            if ($right) {
                $right = quickSort($right);
            }

            return array_merge($left, [$array[0]], $right);
        }
    }

    return $array;
}
```

- 选择排序

    思路分析：在要排序的一组数中，选出最小的一个数与第一个位置的数交换。然后在剩下的数当中再找最小的与第二个位置的数交换，如此循环到倒数第二个数和最后一个数比较为止。
    
```
/**
 * 选择排序
 * 在要排序的一组数中，选出最小的一个数与第一个位置的数交换。然后在剩下的数当中再找最小的与第二个位置的数交换，如此循环到倒数第二个数和最后一个数比较为止。
 * @param array $array
 * @return array
 */
function selectSort(array $array)
{
    $length = count($array);
    if ($length > 1) {

        for ($i = 0; $i < $length - 1; $i ++) {
            $index = $i;
            for ($x = $i + 1; $x < $length; $x ++) {
                // 默认$array[$index] 为最小值
                if ($array[$index] > $array[$x]) {
                    // 记录最小值
                    $index = $x;
                }
            }

            // 已经确定最小值的位置，保持到$index中;如果发现最小值的位置与当前假设的位置$i不同，则位置互换即可。
            if ($index != $i) {
                $tmp = $array[$index];
                $array[$index] = $array[$i];
                $array[$i] = $tmp;
            }
        }
    }

    return $array;
}
```

- 插入排序

    在要排序的一组数中，假设前面的数已经是排好顺序的，现在要把第n个数插到前面的有序数中，使得这n个数也是排好顺序的。如此反复循环，直到全部排好顺序。
```

/**
 * 插入排序
 * 在要排序的一组数中，假设前面的数已经是排好顺序的，现在要把第n个数插到前面的有序数中，使得这n个数也是排好顺序的。如此反复循环，直到全部排好顺序。
 * @param array $array
 * @return array
 */
function insertSort(array $array)
{
    $length = count($array);
    if ($length > 0) {
        for ($i = 1; $i < $length; $i ++) {
            $tmp = $array[$i];

            // 内层循环，比较插入
            for ($x = $i - 1; $x >= 0; $x --) {
                if ($tmp < $array[$x]) {
                    // 发现插入的元素要小，交换位置，将后边的元素与前面的元素互换
                    $array[$x + 1] = $array[$x];
                    $array[$x] = $tmp;
                } else {
                    break;
                }
            }
        }
    }

    return $array;
}

```
