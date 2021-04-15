#### 注解路由

路由参数说明：

* method值是请求的http方法，支持：GET、POST、PUT、DELETE

* auth值是需要使用authorization进行token认证的路由，false不需要，true需要

* rate_limit值是接口服务限流参数，此处功能相当于设置该接口最大可以接受的Qps|Tps值

* before值是请求方法之前执行,一般是权限检查动作，用户登录日志，重要数据查询日志，数据删除日志，重要数据变更日志 （如密码变更，权限变更，数据修改等）

* after值是请求方法之后执行

```php
#[Router(method: 'GET', auth: false, before: 'rule', after: 'responseLog')]
#[Router(method: 'POST', auth: true)]
#[Router(method: 'PUT', auth: true)]
#[Router(method: 'DELETE', auth: true)]
#[Router(method: 'Cli', auth: false)]
```
