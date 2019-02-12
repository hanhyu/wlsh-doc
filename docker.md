#### 组件容器

## 启动说明

1. 启动框架：docker start base_frame

2. 启动缓存：docker start base_redis_5.0

3. 启动数据库：docker start base_mysql_8.0

## 默认设置

1. 宿主机redis的6380映射base_redis_5.0中的6379

2. 宿主机mysql的3307映射base_mysql_8.0中的3306

> 稍后会放出组件扩展的整套docker服务运行环境