#### pdo连接池

> 目录application/library/PdoPool.php 该类是基于swoole的协程channel实现保存pdo连接对象池功能。

* 默认配置加载mysql与pgsql扩展做为连接对象，支持mysql、mssql、oracle、sqlite、pgsql等pdo扩展。

* mysql扩展依赖wait_timeout与interactive_timeout两个参数值，用来释放连接池中长时间一直没使用过的连接对象。