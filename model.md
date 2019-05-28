#### 模型

数据模型都继承各自的父类，在父类中执行了从协程连接池中获取连接对象、使用连接对象、释放连接对象操作。

 * 为什么使用medoo
 
 ?> 为什么不使用数据库orm框架，作者做过大量测试，使用orm时运行的性能非常低下，且会增加PHPer学习成本。所以推荐使用基本的pdo扩展就足够用了。
    找了很多扩展最后发现medoo扩展是基于pdo开发的，且测试性能基本与原生pdo性能一致。当然，更推荐直接使用sql语句加预处理开发，
    我们酷毙的码农不缺的就是时间，结合多年开发经验，使用orm快速开发省下来的时间跟带来的性能比较，时间是唯不足道的，一天上班8小时，
    使用orm写代码与使用sql写代码的时间对比可以忽略不计，且直接用sql后期维护迭代也比使用orm开发的快、稳、直观，
    当然对于那些写一个复杂点的sql就需要一整天时间的开发新人来说也不用气馁，每天加班都做什么？
    酷毙的码农只有敲代码时间不缺，我们缺的是陪伴家人的时间，当前，前提是要有RMB。
 
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