---
title: PHP安全Web常见攻击
date: 2019-05-10 10:40:15
tags:
- php
- web
---

# SQL注入
- 说明    
攻击者把SQL命令插入到Web表单的输入域或页面请求的字符串，欺骗服务器执行恶意的SQL命令。在某些表单中，用户输入的内容直接用来构造（或者影响）动态SQL命令，或作为存储过程的输入参数，这类表单特别容易受到SQL注入式攻击

- 模拟
    ```
    // SQL
    $query = 'SELECT * FROM `user` WHERE `username` = ' . $username . ' AND `password` = '. $password;
    // 在没有过滤下 如果 $username 或者 $password 为 '1' OR 1 = 1 ,那么SQL 为
    // "SELECT * FROM `user` WHERE `username` = '1' OR 1 = 1 AND `password` = '1' OR 1 = 1";
    
    ```
- 危害
    - 查询数据库中敏感数据
    - 绕过认证
    - 添加、删除、修改服务器数据
    - 拒绝服务

- 防范
    - 检查变量的数据类型和格式
    - 过滤特殊符号
    - SQL组装的时候，对外部变量以及所有变量都进行过滤
    - 可以用sqlEscape、sqlImplode、sqlSingle、sqlMulti等函数过滤组装。过滤主要是一些’单引号这些可以破坏SQL组装的数据
    - 绑定变量,使用预处理语句

# 跨站脚本攻击
- 说明
    
    XSS跨站脚本;恶意攻击者往Web页面里插入恶意html代码，当用户浏览该页之时，嵌入其中Web里面的html代码会被执行，从而达到恶意用户的特殊目的.反射型跨站。GET或POST内容未过滤，可以提交js以及HTML等恶意代码。
- 模拟
    ```
    <?php echo $_GET['message'] ?>
    // 正常url
    index.php?message=处理成功
    // 带js的url
    index.php?message=<script>alert(1)</script>
    // 恶意跳转url
    index.php?message=<script>window.location.href=user.php</script>
    
    ```
- 危害
    - 获取到用户cookie信息
    - 跳转到钓鱼网站
    - 操作受害者的浏览器，查看受害者网页浏览信息等。
    - 蠕虫攻击。
- 防范
    - 使用htmlspecialchars函数将特殊字符转换成HTML编码，过滤输出的变量

# 跨站请求伪造
- 说明

    CSRF跨站请求伪造，也被称成为“one click attack”或者session riding，通常缩写为CSRF或者XSRF，是一种对网站的恶意利用。XSS利用站点内的信任用户，而CSRF则通过伪装来自受信任用户的请求来利用受信任的网站。与XSS攻击相比，CSRF攻击往往不大流行（因此对其进行防范的资源也相当稀少）和难以防范，所以被认为比XSS更具危险性。
- 模拟

    某个购物网站购买商品时，采用http://www.shop.com/buy.php?item=watch&num=100，item参数确定要购买什么物品，num参数确定要购买数量，如果攻击者以隐藏的方式发送给目标用户链接
，那么如果目标用户不小心访问以后，购买的数量就成了100个

- 危害

    - 强迫受害者的浏览器向一个易受攻击的Web应用程序发送请求,最后达到攻击者所需要的操作行为

- 防范

    - 检查网页来源
    - 检查内置的隐藏变量
    - 使用POST，不要使用GET，处理变量也不要直接使用$_REQUEST
    - 采用类似随即码或者令牌的形式，让用户操作唯一性。 （每次用户登录网站随机生成一token，存放在cookie中，用户的所有操作中都需要经过token验证）
    
# 文件上传漏洞
- 说明
    
    攻击者利用程序缺陷绕过系统对文件的验证与处理策略将恶意代码上传到服务器并获得执行服务器端命令的能力。
- 常用的攻击手段有
    - 上传Flash跨域策略文件crossdomain.xml，修改访问权限(其他策略文件利用方式类似)；
    - 上传病毒、木马文件，诱骗用户和管理员下载执行；
    - 上传包含脚本的图片，某些浏览器的低级版本会执行该脚本，用于钓鱼和欺诈。
    - 总的来说，利用的上传文件要么具备可执行能力(恶意代码)，要么具备影响服务器行为的能力(配置文件)。
    - 上传Web脚本代码，Web容器解释执行上传的恶意脚本

- 防范
    
    - 文件上传的目录设置为不可执行
    - 判断文件类型，设置白名单。对于图片的处理，可以使用压缩函数或者resize函数，在处理图片的同时破坏图片中可能包含的HTML代码
    - 使用随机数改写文件名和文件路径：一个是上传后无法访问；再来就是像shell、.php 、.rar和crossdomain.xml这种文件，都将因为重命名而无法攻击
    - 单独设置文件服务器的域名：由于浏览器同源策略的关系，一系列客户端攻击将失效，比如上传crossdomain.xml、上传包含Javascript的XSS利用等问题将得到解决

# SESSION 劫持
- 说明

    攻击者利用各种手段来获取目标用户的session id。一旦获取到session id，那么攻击者可以利用目标用户的身份来登录网站，获取目标用户的操作权限
- 攻击者获取目标用户session id的方法
    - 暴力破解:尝试各种session id，直到破解为止;
    - 计算:如果session id使用非随机的方式产生，那么就有可能计算出来;
    - 窃取:使用网络截获，xss攻击等方法获得
- 防范

    - 定期更改session id
    - 更改session的名称
    - 关闭透明化session id
    - 设置HttpOnly。通过设置Cookie的HttpOnly为true，可以防止客户端脚本访问这个Cookie，从而有效的防止XSS攻击

# 其他安全相关

1. X-Frame-Options防止网页放在iframe中

    X-Frame-Options是一个HTTP标头（header），用来告诉浏览器这个网页是否可以放在iFrame内
    
    X-Frame-Options 有三个可选的值
    - DENY：浏览器拒绝当前页面加载任何Frame页面
    - SAMEORIGIN：frame页面的地址只能为同源域名下的页面
    - ALLOW-FROM：origin为允许frame加载的页面地址
    
    可以直接通过meta标签来设置，不需要放在http头部请求中
    ```
    <meta http-equiv="X-Frame-Options" content="ALLOW-FROM http://optimizely.com">
    ```
    
#### 参考文档
1. [参考一](http://blog.csdn.net/erjian666/article/details/53289763)
2. [参考二](http://www.cnblogs.com/luyucheng/p/6234524.html)
