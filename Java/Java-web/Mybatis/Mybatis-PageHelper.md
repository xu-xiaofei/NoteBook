### 1. Mybatis配置分页
    

### 2. 关于分页插件的执行顺序

 [官网链接](https://github.com/pagehelper/Mybatis-PageHelper/blob/master/wikis/zh/Interceptor.md)

 ### 3. 数据库乐观锁
   更新某条数据时候希望此数据没有动过
   - 1.查询出去的时候就带着相关的锁字段
   - 2.保存的时候对比相应的锁字段
