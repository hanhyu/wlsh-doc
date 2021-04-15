#### 框架启动

wlsh启动所需的php扩展版本最低要求：

> 必须已安装php8、swoole4.6、redis-5.0、mysql-8.0、inotify扩展，建议直接用docker环境版本

## 启动预检

> php swoole.php start 每次启动服务前使用该命令做预检

## 生产环境

> php swoole.php start -d 以守护进程方式启动，生产环境使用

## 开发环境

> php swoole.php start dev 开启debug模式，本地开发环境使用

## 测试环境

> php swoole.php start dev -d 以守护进程方式启动debug模式，线上开发环境与测试环境使用

## 停止服务

> php swoole.php stop

## 重载工作进程

> php swoole.php reload


?> 注意：所有命令都是在根目录下执行；支持代码自动热加载。
