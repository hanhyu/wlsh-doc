#### 协程mysql

> 目录application/library/CoMysqlPool.php 该类是基于swoole的协程channel与协程mysql客户端实现的连接池功能。

* mysql扩展依赖wait_timeout与interactive_timeout两个参数值，用来释放连接池中长时间一直没使用过的连接对象。