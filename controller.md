#### 控制器

ControllersTrait中是设置全局服务对象、设置全局请求对象、设置全局响应对象、获取请求参数、过滤验证请求参数等操作

 * 每个控制器的开始需要引入公共的trait类：use \App\Library\ControllersTrait
 
 * 每个控制器需要在init初始化时调用beforeInit方法
 
 * 如果需要接收请求参数，还需在请求方法首行调用validator方法进行接受、过滤、验证客户端传入的参数
 
 * http_response方法是对返回的数据格式进行封装
 
 * 支持手动抛出异常并返回接口数据，但必须指定异常状态码;否则将视为系统异常，只做日志记录，不会把异常内容返回接口。
 示例：throw new \ProgramException('参数错误', 400);
 
 * 其他流程操作与语法都需按照php8和swoole提供的原生语法操作
 
示例：

```php
<?php
declare(strict_types=1);

namespace App\Modules\System\controllers;

use App\Domain\System\UserDomain;
use App\Library\ControllersTrait;
use App\Library\ProgramException;
use App\Library\Router;
use App\Models\Forms\SystemUserForms;
use App\Library\ValidateException;
use JsonException;

class UserController
{
    use \ControllersTrait;

     /**
     * @var UserDomain
     */
    protected UserDomain $user;

    public function init()
    {
        $this->beforeInit();
        $this->user = new UserDomain();
    }
    
    /**
     * 创建用户
     * @throws ProgramException
     * @throws ValidateException|JsonException
     */
    #[Router(method: 'POST', auth: true)]
    public function setUserAction(): string
    {
        $data = $this->validator(SystemUserForms::$userLogin);
        $info = $this->user->existName($data['name']);
        if (!empty($info)) {
            return http_response(400, '该用户名已存在');
        }

        $res = $this->user->setUser($data);
        if ($res) {
            return http_response(200, $data['name'] . '注册成功');
        }

        return http_response(400, $data['name'] . '注册失败');
    }

    /**
     * 用户列表
     * @throws ProgramException
     * @throws ValidateException|JsonException
     */
    #[Router(method: 'GET', auth: true)]
    public function getUserListAction(): string
    {
        $data = $this->validator(SystemUserForms::$getUserList);
        $res  = $this->user->getInfoList($data);
        return http_response(data: $res);
    }
    
}
```
