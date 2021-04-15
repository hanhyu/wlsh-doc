#### 服务领域

application/domain中目录结构与modules模块目录结构保持一致性

 * 每个php文件需要开启严格模式：declare(strict_types=1)

```php
<?php
declare(strict_types=1);

namespace App\Domain\System;

use App\Library\ProgramException;
use App\Models\Mysql\SystemUserLogMysql;
use App\Models\Mysql\SystemUserMysql;
use App\Models\Mysql\UserLogViewMysql;
use Swoole\Coroutine\Channel;

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

        $res['count'] = SystemUserMysql::getInstance()->getListCount();
        $res['list']  = SystemUserMysql::getInstance()->getUserList($data);
        return $res;
    }
    
    /**
     *
     * @param array $data
     *
     * @return int
     * @throws ProgramException
     * @throws \Envms\FluentPDO\Exception
     */
    public function editUser(array $data): int
    {
        return SystemUserMysql::getInstance()->editUser($data);
    }

    public function setLoginLog(array $data): void
    {
        SystemUserLogMysql::getInstance()->setLoginLog($data);
    }
    
}
```
