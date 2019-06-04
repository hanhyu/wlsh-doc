#### 模型

数据模型都继承各自的父类，在父类中执行了从协程连接池中获取连接对象、使用连接对象、释放连接对象操作。

 * 为什么使用medoo?
 
> 1. 为什么不使用数据库orm框架，作者做过大量测试，使用orm时运行的性能非常低下，且会增加PHPer学习成本，所以推荐使用基本的pdo扩展就足够用了。
> 2. medoo扩展是基于pdo开发的，且测试性能基本与原生pdo性能一致。
> 3. 使用medoo容易做协程连接池扩展，可最大化减少连接池相关问题。
 
```
<?php
declare(strict_types=1);

namespace App\Models\Mysql;

class SystemUser extends AbstractMysql
{
    private $table = 'frame_system_user';

    /**
     * @param array $data
     *
     * @return array
     */
    protected function getUserList(array $data): array
    {
        $datas = [];
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

        try {
            $datas = $this->db->select($this->table, '*', $wheres);
        } catch (\Exception $e) {
            co_log($this->db->last(), "信息出错：{$e->getMessage()},,出错的sql：");
        } finally {
            if ($datas === false) {
                co_log($this->db->last(), '查询信息失败的sql：');
                $datas = [];
            }
        }
        return $datas;
    }

    protected function getListCount(): int
    {
        $datas = 1;
        try {
            $datas = $this->db->count($this->table);
        } catch (\Exception $e) {
            co_log($this->db->last(), "信息出错：{$e->getMessage()},,出错的sql：");
        }
        return $datas;
    }
    
}
```