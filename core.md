#### 两个核心

## 核心一

> 目录application/library/Server.php 该类是项目的核心所在，基于swoole的服务器，可同时处理websocket、http、tcp、udp等协议

* 默认开启websocket与http(含https、http2.0)两种协议

* 默认隔离两种协议的请求接口通信互不干扰

* 默认开启ssl通信，证书是用框架自带的一个。

* 默认开启task进程可以使用协程

* 默认开启记录swoole日志

* 默认开启异步安全重启服务，且设置重启等待时间为7秒。

* 默认设置了一个table共享内存表

* 默认设置了一个原子计数器

* 默认使用inotify监听框架application与conf目录中文件变化，自动安全重启工作进程。

* 默认响应数据内容为json格式

* 默认开启了请求头的Authorization与cookie属性

* 默认设置了请求头额外的Timestamp与Sign属性

* 默认设置了允许跨域访问的域名从配置文件中读取

* 默认过滤掉了固定的几个模块不能在外部http直接访问：ws、task、tcp、close、finish模块。

* 默认开启了http协议的OPTIONS预检功能，此请求不做任何处理直接返回。

* 默认设置了yaf路由通过自定义路由功能过滤

* 默认在响应数据的顶层捕获手动抛出的异常和系统自动抛出的异常，且开启了协程记录日志

* 默认设置了使用task异步任务时，需要序列化数据serialize后进行投递。

## 核心二

> 目录application/Bootstrap.php 该类处理项目的引导、配置注册加载、路由设置等

* 默认关闭yaf视图渲染功能，wlsh框架只做api接口操作。

* 默认注册了配置文件、路由过滤、邮件配置。

* 默认注册了redis、mysql、postgreSQL、协程mysql连接池对象。

?> 如需增加其他协议还可以同时开启监听多个协议端口,实现只需启动一个入口文件就可以处理多种协议状态；框架路由部分转交给yaf框架完成操作。
