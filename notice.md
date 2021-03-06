#### 注意事项

> 请小心地在协程框架中使用原fpm模式下的一些技巧与设计模式，一不留心就会踩坑。

```
本框架是协程服务框架，使用中注意swoole的协程特性给代码会来什么样的运行逻辑效果。
```
1、需要日志记录时用co_log()方法记录日志，该方法是协程方式处理日志。

2、task_log方法使用swoole的task扩展进行异步非阻塞方式记录日志，该方法是处理耗时的日志任务，如：发送邮件。

3、数据模型中所有的Model需要继承各自文件中AbstractModel抽象类，该类中实现了从数据协程连接池中自动获取一个连接后使用，等使用完后立即自动返回池子中;
Model中的方法如有需要用到数据库连接对象时都必须设置为protected受保护类型，这样外层在正常调用数据方法操作时，会自动先请求AbstractModel中的
call魔术方法进行连接池操作的业务分流。

4、 修改swoole.php、application/library/Server.php与AutoReload.php三个文件需要手动重启框架服务，其他文件修改时框架会自动重载。

（忽略）以下是框架自带的system模块约定：

5、暴露给前端的字段都不含表名前缀;
如： insert、update、delete:表名是base_config,在前端传name、content等字段，后端接口在model层把传入的参数做一一对应转换（加表前缀）
configName、configContent。
select:表名是base_config,后端接口在model层把查询的字段做一一对应转换（去掉表前缀）configName as name、configContent as content，再传给前端。

6、由于在服务核心文件中使用了Throwable捕获异常，所以在程序运行中没有catch住的异常都会在最上层捕获，在关闭调试模式下会返回500服务异常提示。

7、按照约定：变量、公共函数、接口暴露的uri、接口数据字段名等都使用下划线，类中方法使用小写驼峰，类名、模块名使用大写驼峰。

8、如果不熟悉swoole的运行机制，请不要开启OPcache、Apc等php代码缓存功能，由于swoole本身就是常驻内存的，所以无须开启代码缓存功能。

9、控制器层(controller目录)、服务领域层(domain目录)、模型层(model目录)三者只能从左到右调用，不能同级调用，也不能从右向左调用。

10、由于使用yaf，需注意命名空间中controllers拼写必须与目录都是小写开头，其他需按照命名空间标准大驼峰写法。

**更多文档内容等抽空一一说明**

> workerProcess与taskProcess可以理解为ADM模式的A与D层的关系，同时也有点像传统的controller与service层的关系,但又有本质的区别：
不同进程之间的调度关系，
