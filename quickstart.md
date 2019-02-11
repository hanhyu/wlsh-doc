## 启动

* 启动服务（根目录下执行命令）： php swoole.php start         （每次启动服务前使用该命令做预检）

* 启动服务（根目录下执行命令）： php swoole.php start -d      （以守护进程方式启动，生产环境使用）

* 启动服务（根目录下执行命令）： php swoole.php start dev     （开启debug模式，本地开发环境使用）

* 启动服务（根目录下执行命令）： php swoole.php start dev -d  （以守护进程方式启动debug模式，线上开发环境与测试环境使用）

* 停止服务（根目录下执行命令）： php swoole.php stop