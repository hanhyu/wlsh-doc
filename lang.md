#### 接口国际化

客户端请求接口时只需在请求头Headers中加上Language参数，即可返回对应的参数国际化语言。

> 服务端配置：在library目录下Language.php文件中修改相关的国际化提示码。

```
默认返回简体中文：

{
    "code": 400,
    "msg": "用户名或密码错误",
    "data": [ ]
}
```
```
在请求头Headers中设置了Language的值为：en-us

{
    "code": 400,
    "msg": "User name or password error",
    "data": [ ]
}
```