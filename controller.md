#### 控制器

ControllersTrait中是设置全局服务对象、设置全局请求对象、设置全局响应对象、获取请求参数、过滤验证请求参数等操作

 * 每个控制器的开始需要引入公共的trait类：use \App\Library\ControllersTrait;
 
 * 并在init初始化时调用beforeInit方法
 
 * 如果请求的方法中需要接受参数，还需在方法首行调用validator方法
 
 * http_response方法是对返回的数据格式进行封装
 
 * 其他流程操作与语法使用都是按照php及yaf和swoole提供的原生操作
 
 > 请勿使用静态类与静态方法

```
<?php
declare(strict_types=1);

use \App\Services\System\UserServices;

class UserController extends Yaf\Controller_Abstract
{
    use \App\Library\ControllersTrait;

    /**
     * @var UserServices
     */
    private $user;

    public function init()
    {
        $this->beforeInit();
        $this->user = new UserServices();
    }
    
    /**
     * 用户列表
     * @throws Exception
     */
    public function getUserListAction(): void
    {
        $data = $this->validator('SystemUserForms', 'getUserList');
        $res  = $this->user->getInfoList($data);
        if ($res) {
            $this->response->end(http_response(200, $res));
        } else {
            $this->response->end(http_response(500, '查询失败'));
        }
    }
    
}
```