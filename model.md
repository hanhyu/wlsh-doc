#### 模型

数据模型都继承各自的父类，在父类中执行了从协程连接池中获取连接对象、使用连接对象、释放连接对象操作。

 ## mysql模型
 
```php
<?php
declare(strict_types=1);

namespace App\Models\Mysql;

use App\Library\AbstractPdo;
use App\Library\ProgramException;
use Envms\FluentPDO\Exception;

class SystemUserMysql extends AbstractPdo
{
    protected string $table = 'frame_system_user';

    public static function getPool(): string
    {
        return 'mysql_pool_obj';
    }

    /**
     * @param array $data
     *
     * @return array
     * @throws Exception|ProgramException
     */
    public function getUserList(array $data): array
    {
        $wheres = !empty($data['where']) ? $data['where'] : null;
        return self::getDb()->from($this->table)
            ->where($wheres)
            ->orderBy('id DESC')
            ->offset($data['curr_data'])
            ->limit($data['page_size'])
            ->fetchAll();
    }

    /**
     * @return int
     * @throws Exception|ProgramException
     * @todo mysql count 性能下降100倍
     */
    public function getListCount(): int
    {
        //return self::getDb()->from($this->table)->count();
        return (int)self::getDb()
            ->from('information_schema.`TABLES`')
            ->where('TABLE_NAME', $this->table)
            ->select('TABLE_ROWS', true)
            ->fetchColumn();
    }
    
}
```

## redis模型

```php
<?php
declare(strict_types=1);

namespace App\Models\Redis;

use App\Library\AbstractRedis;
use RedisException;

class LoginRedis extends AbstractRedis
{
    /**
     * 此处使用静态延迟绑定，实现选择不同的数据库,如不设置默认为0
     * @var int
     */
    protected static int $db_index = 1;

   /**
     * @param string $key
     *
     * @return bool|string
     * @throws RedisException
     */
    public function getKey(string $key): bool|string
    {
        return self::getDb()->get($key);
    }

}
```
