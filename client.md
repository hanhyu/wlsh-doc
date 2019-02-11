## 测试

wlsh-framework集成了swoole client, swoole http client, swoole websocket client连接服务器测试方法

```
/**
 * 测试文件使用方法
 * 进入tests目录
 * 在命令行中执行： php client.php  TestClient  websocket  index/index  进行websocket客户端测试
 * 在命令行中执行： php client.php  TestClient  http       index/index  进行http客户端测试
 * 执行命令参数说明：第1个为文件路径  第2个为类名  第3个为方法  第4个为路由URL
 * 在其他目录下执行,第一个文件路径参数必须为绝对目录. 如: php /var/www/wlsh-framework/tests/client.php TestClient http index/index
 */
 
测试数据库请自行按修改yaf中的数据库连接配置信息，项目根目录下自带了一份备份测试数据，可以导入进自己的mysql库中使用。
```
