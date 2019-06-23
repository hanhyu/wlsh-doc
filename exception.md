#### 异常

* 核心文件server.php所有事件回调中设置了捕获异常信息

* 分三种类别捕获：

> 1. 验证类异常ValidateException
> 2. 程序手动抛出的异常ProgramException
> 3. 其他php抛出的异常用顶级Throwable捕获
