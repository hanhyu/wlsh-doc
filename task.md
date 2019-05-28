#### 任务

由于使用协程处理I/O事件，其task现在使用的意义跟以前不太一样：

 * http协议中使用task方法,只限用于在worker操作方法中调用task时不依赖task方法返回的结果,如:redis,mysql等插入操作且不需返回插入后的状态或
日志记录等不依赖上下文的操作流程。

 * websocket协议中用task方法,可直接在task方法中调用push方法返回数据给客户端,
这样swoole服务模式就变为worker中方法是异步到task方法中同步+协程的执行模式,worker中可更多地处理请求以提高websocket服务器性能.