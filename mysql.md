#### 协程mysql

> 目录application/library/CoMysqlPool.php 该类是基于swoole的协程channel与协程mysql客户端实现的连接池功能。

* 该协程连接池功能只能在wlsh框架中使用。

* mysql扩展依赖wait_timeout与interactive_timeout两个参数值，用来释放连接池中长时间一直没使用过的连接对象。

* 连接池的最大连接数必须大于等于swoole启动的worker（含taskWorker）进程数.