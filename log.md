#### 日志

默认提供了两个记录日志的方法：

* co_log

日志记录等级critica、alert、emergency都是发送到绑定邮箱，其他等级的日志是通过mongodb存储，如果mongodb存储失败则使用普通文件记录。

* task_log

使用task异步任务记录日志

> 所有日志记录都是协程操作