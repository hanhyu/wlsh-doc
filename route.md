#### 路由

允许外部访问的方法，二维数组格式：key=>value

 * key是请求的uri，value是转发路由相关参数的数组
 
value参数说明：
 
 * auth值是需要使用authorization进行token认证的路由，false不需要，true需要
 
 * method值是请求的协议方法
 
 * action值为** * **时代表路由直接转发请求的uri，当设定其他值时代表转发指定的路由，**指定格式：/modules/controller/action**
 
**注：uri与action值结尾不能有 / 字符**
 
> 目录conf/routerFilter.php 该文件是处理项目自定义路由信息

```
[
    /***************************************** 相关路由示例 *****************************************/
    '/login/test' => ['auth' => false, 'method' => 'GET', 'action' => '*'],

    '/login/get_redis'          => ['auth' => false, 'method' => 'GET',    'action' => '*'],
    '/system/process/set_msg'   => ['auth' => true,  'method' => 'POST',   'action' => '*'],
    '/system/menu/edit_menu'    => ['auth' => true,  'method' => 'PUT',    'action' => '*'],
    '/system/backup/del'        => ['auth' => true,  'method' => 'DELETE', 'action' => '*'],
    '/login/get_log_user_view'  => ['auth' => false, 'method' => 'GET',    'action' => '*'],
    '/finish/log/index'         => ['auth' => false, 'method' => 'Cli',    'action' => '/finish/flog/index'],
]
```