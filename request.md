#### 请求

* 请求的参数默认支持json与x-www-form-urlencoded两种格式

* 获取请求参数值时，请直接用swoole提供的request方法，不要使用yaf与php自带的获取请求方式

> 通过核心文件yaf路由转接到控制器分发时，后面的参数对象都由swoole服务接管，所以wlsh框架只是利用了yaf的路由与生命周期，
其他功能都由swoole完成操作。