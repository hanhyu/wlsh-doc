#### 服务

application/services中目录结构与modules模块目录结构保持一致性

 * 每个php文件需要开启严格模式：declare(strict_types=1);
 
 * 可以并行执行的流程必须使用协程处理
 

```
<?php
declare(strict_types=1);

namespace App\Services\System;

use App\Models\Mysql\{
    SystemUser as User,
    SystemUserLog as UserLog,
    UserLogView as UserV,
};

class UserServices
{
    /**
     * @var User
     */
    private $user;
    /**
     * @var UserLog
     */
    private $user_log;
    /**
     * @var UserV
     */
    private $user_v;

    public function __construct()
    {
        $this->user     = new User();
        $this->user_log = new UserLog();
        $this->user_v   = new UserV();
    }

    /**
     * 获取用户列表数据
     *
     * @param array $data
     *
     * @return array|null
     */
    public function getInfoList(array $data): ?array
    {
        $res = [];
        if ($data['curr_page'] > 0) {
            $data['curr_data'] = ($data['curr_page'] - 1) * $data['page_size'];
        } else {
            $data['curr_data'] = 0;
        }
        $data['where'] = [];

        $chan = new \Swoole\Coroutine\Channel(2);
        go(function () use ($chan) { //获取总数
            try {
                $count = $this->user->getListCount();
                $chan->push(['count' => $count]);
            } catch (\Throwable $e) {
                $chan->push(['500' => $e->getMessage()]);
            }
        });
        go(function () use ($chan, $data) { //获取列表数据
            try {
                $list = $this->user->getUserList($data);
                $chan->push(['list' => $list]);
            } catch (\Throwable $e) {
                $chan->push(['500' => $e->getMessage()]);
            }
        });

        for ($i = 0; $i < 2; $i++) {
            $res += $chan->pop(7);
            if (isset($res['500'])) {
                co_log(['exception' => $res['500']], 'getUserListAction mysql异常');
                return null;
            }
        }

        return $res;
    }
    
}
```