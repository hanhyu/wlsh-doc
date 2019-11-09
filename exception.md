#### 异常
> 核心文件server.php所有事件回调中设置了程序(三种)顶级捕获异常信息

* 验证类异常`ValidateException`

ValidateException信息直接返回到接口调用方

* 程序手动抛出的异常`ProgramException`

ProgramException信息直接返回到接口调用方

* 其他php抛出的异常用顶级`Throwable`捕获，在启动服务时指定参数“dev”来表示是否开启debug模式：

| 区别 | 说明 |
| :------ | :------ |
| 开启debug模式 | 顶级`Throwable`捕获的所有信息（异常与错误）都会直接返回到接口调用方，并记录日志信息。
| 关闭debug模式 | 顶级`Throwable`捕获的所有信息（异常与错误）都会给接口调用方统一返回：“服务异常”信息，并记录日志信息。


?> 业务代码中程序逻辑主动抛出的异常都应该是`ProgramException`类型。
