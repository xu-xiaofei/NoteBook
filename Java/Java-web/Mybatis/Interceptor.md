
### 1. 拦截范围

    Mybatis拦截器只能拦截Executor、ParameterHandler、StatementHandler、ResultSetHandler四个对象里面的方法。而且一旦拦截器加载就是针对全局的所有sql 都生效，暂时没有找到办法只针对某个接口或者sql生效，interceptor是有执行顺序的


### 2. 