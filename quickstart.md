#### 框架启动

wlsh启动所需的php扩展版本最低要求：

> 必须已安装php 7.3、swoole 4.4.12、redis-5.0、mysql-8.0、inotify、yaf 13.1.1(支持命名空间的yaf版本，链接：https://github.com/gaohuia/yaf)

## 启动预检
> php swoole.php start         每次启动服务前使用该命令做预检

## 生产环境
> php swoole.php start -d      以守护进程方式启动，生产环境使用

## 开发环境
> php swoole.php start dev     开启debug模式，本地开发环境使用

## 测试环境
> php swoole.php start dev -d  以守护进程方式启动debug模式，线上开发环境与测试环境使用

## 停止服务
> php swoole.php stop

## 重载工作进程
> php swoole.php reload


?> 注意：所有命令都是在根目录下执行;不支持restart命令，可自行使用systemd管理swoole服务，实现故障重启、开机自启动等功能。
