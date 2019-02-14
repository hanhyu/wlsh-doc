#### 控制器

ControllersTrait中是设置全局服务对象、设置全局请求对象、设置全局响应对象、获取请求参数、过滤验证请求参数等操作

 * 每个控制器的开始需要引入公共的trait类：use \App\Library\ControllersTrait
 
 * 每个控制器需要在init初始化时调用beforeInit方法
 
 * 如果请求方法中需要接收参数，还需在方法首行调用validator方法进行接受、过滤、验证操作
 
 * http_response方法是对返回的数据格式进行封装
 
 * 支持手动抛出异常并返回接口数据，但必须指定异常状态码;否则将视为系统异常，只做日志记录，不会把异常内容返回接口。
 示例：throw new \Exception('参数错误', 400);
 
 * 其他流程操作与语法都需按照php及yaf和swoole提供的原生语法操作
 
 > 请勿使用静态类与静态方法，也不要过度封装与过度使用继承。

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