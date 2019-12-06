---
layout: post
title: "nodejs redis 交互入门"
categories: nodejs
tags: nodejs redis
---

* content
{:toc}

简介和安装

redis简介： 开源高性能key-value存储；采用内存中（in-memory）数据集的方式，也可以采用磁盘存储方式（前者性能高，但数据可能丢失，后者正好相反） 支持字符串（strings）、哈希（hashes）、列表（lists）、集合（sets）和 有序集合（sorted sets）等；支持对复杂数据结构的高速操作。 特性多，支持主从同步、pub/sub等 支持多种客户端（http://redis.io/clients） ... 　　注：应用场景没有提到，暂时没有太多实际体会，不瞎说，以免误导人，但是从它的简介和特性来说，起码缓存场景是不错的！

　　Redis下载地址： https://github.com/dmajkic/redis/downloads

　　node.js客户端：node_redis https://github.com/mranney/node_redis/

redis安装(Windows平台) 　　 redis非常方便，直接下载解压就可以使用，因为开发环境是win7 64位，直接下载（示例下载的安装包：redis-2.4.5-win32-win64.zip）

redis运行 　　解压到后运行"64bit"文件夹下的redis-server.exe即可，但是这样运行会出现一个如下警告提示：

　　#Warning: no config file specified,using the default config. In order to specify a config file use ‘redis-server /path/to/redis.conf’

　　提示也比较明显，没有明确的配置文件，使用的是默认配置，请使用‘redis-server /path/to/redis.conf’指定明确的配置文件

　　 根据提示运行redis成功（如下图）

　　在redis-server.exe同级目录下可以看到一个redis.conf文件，这就是配置文件

node_redis安装 npm install redis 或者 npm install hiredis redis 　　我这里采用 npm install hiredis redis 安装

　　注：两种都可用，区别在于性能，hiredis是非阻塞的，而且速度更快；如果安装了hiredis，node_redis则会默认以它为解析器，没安装就会用纯javascript解释器，对于学习或者开发环境，用哪个都无所谓

redis.createClient()连接到redis服务器

　　环境都准备好了，就开始写一代简单的代码测试用nodejs连接一下服务器

示例源码 　　从上图中可以看到运行结果，输出ready，表示成功！　　

　　对代码还是讲一下:

　　redis.createClient()：返回的是一个RedisClient的对象，大家可以输出来看一下此对象的具体信息。

　　ready：Redis的Connection事件之一，当与redis服务器连接成功后会触发这个事件，此时表示已经准备好接收命令，当这个事件触发之前client命令会存在队列中，当一切准备就绪后按顺序调用

　　对于上面的几句代码就能连接成功redis服务器，原因是当前redis服务器在本地，如果不在本地，怎么连接呢？

示例源码 　　也是成功！这种方式和上一种在redis.createClient()时分别传入了端口号、服务器IP和设置项

　　这样就可以用于连接远程的redis服务器，或者利用第三个参数进行一些配置！

　　redis的默认端口：6379

认证 client.auth(password, callback)

　　上面试过了，连接到redis服务器，可以看出我们并没有输入密码进行验证的过程就成功连接到了服务器，因为redis服务器默认不需要密码，不过这不太安全，我们肯定要设置一下密码

　　打开redis.conf文件，找到requirepass，取消注释，设置密码为:porschev

requirepass porschev 　　然后重启redis服务器;再次利用上面的代码连接到redis服务器，出现错误提示（如下图）：ERR operation not permitted

　　那么如何连接到有密码的redis服务器呢？

　　简单的试了一下，有两种方法（可能有更多，没试，其实一种完全就够了，多了也没用^_^!）

　　方式一：通过设置redis.createClient()的第三个参数，也就是设置项来完成

示例源码 　　上图可以连接成功，通过设置连接设置项中的auth_pass来通过认证！

　　auth_pass：默认值为null，默认情况下客户端将不通过auth命令连接，如果设置了此项，客户端将调用auth命令连接

　　方式二：通过client.auth(password, callback)

示例源码 　　此方法也可以成功，第一个参数为密码，第二个为回调函数！

单值set和get

示例源码 　　从输出结果可以看出，set一个值和获取这个值都成功！

　　代码讲一下：

　　client.set(key,value,[callback])：设置单个key和value，回调函数可选

　　client.get(key,[callback])：得到key得到value，回调函数可选（虽然可选，但不写回调函数获取又有什么意义呢^_^!）

　　connect：Redis的Connection事件之一，在不设置client.options.no_ready_check的情况下，客户端触发connect同时它会发出ready，如果设置了client.options.no_ready_check，当这个stream被连接时会触发connect，

　　　　　　 这时候就可以自由尝试发命令

　　redis.print：简便的回调函数，测试时显示返回值（从示例的输出结果中可以看出）

　　其它补充说明：

  client.options.no_ready_check：默认值为false,当连接到一台redis服务器时，服务器也许正在从磁盘中加载数据库，当正在加载阶段，redis服务器不会响应任何命令，node_redis会发送一个“准备确认”的INFO命令，
　　　　　　　　　　　　　　　　　INFO命令得到响应表示此时服务器可以提供服务，这时node_redis会触发"ready"事件，如果该设置项设置为true，则不会有这种检查

　　client.set([key,value],callback)：与client.set(key,value,[callback]);效果一致（可以自行对上面示例源码进行修改进行测试）,必须要有回调函数

多值get和set

示例源码 　　从输出结果可以看出，set多值和获取都成功！

　　代码讲一下：

　　client.hmset(hash, obj, [callback])：赋值操作，第一个参数是hash名称；第二个参数是object对象，其中key1:value1。。,keyn:valuen形式；第三个参数是可选回调函数

　　client.hmset(hash, key1, val1, ... keyn, valn, [callback])：与上面做用一致，第2个参数到可选回调函数之前的参数都是key1, val1, ... keyn, valn形式；

　　client.hgetall(hash, [callback])：获取值操作，返回一个对象

　　其它补充说明：

　　console.dir()：用于显示一个对象所有的属性和方法

打包执行多个命令[事务]

示例源码 　　官方有个示例，我修改一下，可能更好理解一些，下面一步步说吧！

　 先了解一下API再看结果

　　client.multi([commands])：这个标记一个事务的开始，由Multi.exec原子性的执行；github上描述是可以理解为打包，把要执行的命令存放在队列中，redis服务器会原子性的执行所有命令，node_redis接口返回一个Multi对象

　　Multi.exec( callback )：执行事务内所有命令；github上描述是client.multi()返回一个Multi对象，它包含了所有命令，直到Multi.exec()被调用；

　　Multi.exec( callback )回调函数参数err：返回null或者Array，出错则返回对应命令序列链中发生错误的错误信息，这个数组中最后一个元素是源自exec本身的一个EXECABORT类型的错误

　　Multi.exec( callback )回调函数参数results：返回null或者Array，返回命令链中每个命令的返回信息

　　end：redis已建立的连接被关闭时触发

　　client.sadd(key,value1,...valuen,[callback])：集合操作，向集合key中添加N个元素，已存在元素的将忽略；redis2.4版本前只能添加一个值

　　sismember(key,value,[callback])：元素value是否存在于集合key中，存在返回1，不存在返回0

　　smembers(key,[callback])：返回集合 key 中的所有成员，不存在的集合key也不会报错，而是当作空集返回

　　client.quit()：与之对应的还有一个client.end()方法，相对比较暴力；client.quit方法会接收到所有响应后发送quit命令，而client.end则是直接关闭；都是触发end事件

　　再看结果应该就比较简单了，client.multi打包了sismember和smembers两个命令，执行exec方法后，回调函数得到两个回应，分别输出两个回应的结果！

其它...

　　redis.debug_mode：这个在开发中可能有用，大家自行设置试一下，设置为true后，看输出

　　Publish / Subscribe：这个官方示例比较简单清晰，大家运行起来看一下就能理解，深入的网上还有很多用它实现的聊天、监控示例，大家看一下，如果以后觉得有必要就再做个示例分享一下

　　client.monitor：监控，可能以后会用到，有需要的深入研究一下，入门可以略过

　　其它redis命令还有不少，能讲到的非常有限，深入练习可以对照：http://redis.readthedocs.org/en/latest/index.html