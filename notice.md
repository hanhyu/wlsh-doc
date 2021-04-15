#### 注意事项

> 请小心地在协程框架中使用原fpm模式下的一些技巧与设计模式，一不留心就会踩坑。

```
本框架是协程服务框架，使用中注意swoole的协程特性给代码会来什么样的运行逻辑效果。
```

1、需要日志记录时用task_log()方法记录日志，该方法是协程方式处理日志。

2、task_log方法使用swoole的task扩展进行异步非阻塞方式记录日志，该方法是处理耗时的日志任务，如：发送邮件。

3、 修改swoole.php、application/library/Server.php与AutoReload.php三个文件需要手动重启框架服务，其他文件修改时框架会自动热加载。

（忽略）以下是框架自带的system模块约定：

4、暴露给前端的字段都不含表名前缀; 如： insert、update、delete:表名是base_config,在前端传name、content等字段，后端接口在model层把传入的参数做一一对应转换（加表前缀）
configName、configContent。 select:表名是base_config,后端接口在model层把查询的字段做一一对应转换（去掉表前缀）configName as name、configContent as
content，再传给前端。

5、由于在服务核心文件中使用了Throwable捕获异常，所以在程序运行中没有catch住的异常都会在最上层捕获，在关闭调试模式下会返回500服务异常提示。

6、按照约定：变量、公共函数、接口数据字段名等都使用下划线，类中方法使用小写驼峰，类名、模块名使用大写驼峰。

7、控制器层(controller目录)、服务领域层(domain目录)、模型层(model目录)三者只能从左到右调用，不能同级调用，也不能从右向左调用。

8、services目录存放对外接口的服务（调用外部接口（RPC、HTTP等）服务代码、对外提供接口服务代码），此服务层可以单向调用内部domain层代码，也可以直接调用model层代码；与domain层的区别：程序内部服务业务逻辑代码在domain层，第三方外部服务业务逻辑代码在services层。

**更多文档内容等抽空一一说明**

> workerProcess与taskProcess可以理解为ADM模式的A与D层的关系，同时也有点像传统的controller与service层的关系,但又有本质的区别： 不同进程之间的调度关系，
