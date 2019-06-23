#### 模型

数据模型都继承各自的父类，在父类中执行了从协程连接池中获取连接对象、使用连接对象、释放连接对象操作。

 * 为什么使用medoo?
 
> 1. 为什么不使用数据库orm框架，作者做过大量测试，使用orm时运行的性能非常低下，且会增加PHPer学习成本，所以推荐使用基本的pdo扩展就足够用了。
> 2. medoo扩展是基于pdo开发的，且测试性能基本与原生pdo性能一致。
> 3. 使用medoo容易做协程连接池扩展，可最大化减少连接池相关问题。
 
 ## mysql模型
 
```
<?php
declare(strict_types=1);

namespace App\Models\Mysql;

use Exception;

class SystemUser extends AbstractMysql
{
    prodected $table = 'frame_system_user';

    /**
     * @param array $data
     *
     * @return array
     * @throws Exception
     */
    public function getUserList(array $data): array
    {
        if (!empty($data['where'])) {
            $wheres = [
                'AND'   => $data['where'],
                'ORDER' => ['id' => 'DESC'],
                'LIMIT' => [$data['curr_data'], $data['page_size']],
            ];
        } else {
            $wheres = [
                'ORDER' => ['id' => 'DESC'],
                'LIMIT' => [$data['curr_data'], $data['page_size']],
            ];
        }
        
        $datas = $this->db->select($this->table, '*', $wheres);
        
        if ($datas == false) throw new Exception($this->db->last());
        
        return $datas;
    }

    /**
     * @return int
     * @throws Exception
     */
    public function getListCount(): int
    {
        $datas = $this->db->count($this->table);
        if ($datas == false) throw new Exception($this->db->last());
        return $datas;
    }
    
}
```

> 在domain服务中直接使用，如：
```
$list = MysqlFactory::systemUser()->getUserList($data);
```

## redis模型

```
<?php
declare(strict_types=1);

namespace App\Models\Redis;

class Login extends AbstractRedis
{
    /**
     * 此处使用静态延迟绑定，实现选择不同的数据库,如不设置默认为0
     * @var int
     */
    protected static $dbindex = 1;

    /**
     *
     * @param string $key
     *
     * @return string|null
     */
    public function getValue(string $key): ?string
    {
        $datas = $this->db->get($key);
        if ($datas == false) $datas = null;
        return $datas;
    }

}
```

> 在domain服务中直接使用，如：
```
$value = RedisFactory::login()->getValue($key);
```

## mongo模型

```
<?php
declare(strict_types=1);

namespace App\Models\Mongo;

class Monolog extends AbstractMongo
{
    /**
     * @param array $data
     *
     * @return array
     * @throws \MongoDB\Driver\Exception\Exception
     */
    public function getMongoList(array $data): array
    {
        /*$filter = [
              //'level'=> 200,
          ];*/
        $options = [
            'sort'  => ['datetime' => -1],
            'skip'  => $data['curr_data'],
            'limit' => (int)$data['page_size'],
            //'projection' => ['_id'=>0],
        ];
        $res     = $this->col->find($data['where'], $options);
        return $res->toArray();
    }

    /**
     *
     * @param array $where
     *
     * @return int
     * @throws \MongoDB\Driver\Exception\Exception
     */
    public function getMongoCount(array $where): int
    {
        return $this->col->countDocuments($where);
    }

    /**
     * @param string $id
     *
     * @return array
     * @throws \MongoDB\Driver\Exception\Exception
     */
    public function getMongoInfo(string $id): array
    {
        $id = new \MongoDB\BSON\ObjectId($id);
        return $this->col->findOne(['_id' => $id])->toArray();
    }

}
```

> 使用时需指定数据库与实例，如：
```
$monolog = MongoFactory::monolog('baseFrame', 'monolog');
$list = $monolog->getMongoList($data);
```