#### redis连接池

> 目录application/library/RedisPool.php 该类是基于swoole的协程channel实现保存redis连接对象池功能。

* 该协程连接池功能只能在wlsh框架中使用。

* 此功能依赖于redis的timeout参数值，用来释放连接池中长时间一直没使用过的连接对象。
