#### 服务领域

application/domain中目录结构与modules模块目录结构保持一致性

 * 每个php文件需要开启严格模式：declare(strict_types=1);
 
 * 可以并行执行的流程必须使用协程处理
 

```php
<?php
declare(strict_types=1);

namespace App\Domain\System;

use App\Models\MysqlFactory;
use Exception;

class User
{
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
                $count = MysqlFactory::systemUser()->getListCount();
                $chan->push(['count' => $count]);
            } catch (\Throwable $e) {
                $chan->push(['500' => $e->getMessage()]);
            }
        });
        go(function () use ($chan, $data) { //获取列表数据
            try {
                $list = MysqlFactory::systemUser()->getUserList($data);
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
    
    /**
     *
     * @param array $data
     *
     * @return int
     * @throws Exception
     */
    public function editUser(array $data): int
    {
        return MysqlFactory::systemUser()->editUser($data);
    }

    public function setLoginLog(array $data): void
    {
        go(function () use ($data) {
            MysqlFactory::systemUserLog()->setLoginLog($data);
        });
    }
    
}
```
