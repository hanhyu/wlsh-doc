## 两个核心

目录application/library/Server.php 该类是项目的核心所在，基于swoole的服务器，可同时处理websocket、http、tcp、udp等协议,
如需增加其他协议还可以同时开启监听多个协议端口,实现只需启动一个入口文件就可以处理多种协议状态；框架路由部分转交给yaf框架完成操作。
