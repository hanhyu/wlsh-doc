#### pdo连接池

> 目录application/library/PdoPool.php 该类是基于swoole的协程channel实现保存pdo连接对象池功能。

* 该协程连接池功能只能在wlsh框架中使用。

* 默认配置加载mysql与pgsql扩展做为连接对象，支持mysql、mssql、oracle、sqlite、pgsql等pdo扩展。

* mysql扩展依赖wait_timeout与interactive_timeout两个参数值，用来释放连接池中长时间一直没使用过的连接对象。

* 该连接池是被动地增加池子对象：初始启动时每个worker主进程连接池对象个数为5,运行中根据客户端并发的情况自动增加池子对象，来保证最大限度地处理连接。

* 连接池的最大连接数必须大于等于swoole启动的worker（含taskWorker）进程数.

* 每个mysql模型初始化时需要在mysql工厂类中使用协程ID（上下文）进行注册，防止在协程状况下出现共享连接池对象问题.
