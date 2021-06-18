# Spring

## 一、



# SpringMVC

## 一、SpringMvc的执行流程







# Mybatis

## 一、Mybatis如何支持懒加载

Mybatis配置有一个属性：LazyLoadingEnable = true

Myabtis有几种加载方式：

- 直接加载：执行完对主加载对象的select语句，马上执行相关联的select语句
- 侵入式延迟加载：执行对主加载对象的查询时，不会马上执行相关联对象的select语句，需要访问主加载对象的详情信息时，才会执行相关联对象的select语句
- 深度延迟加载：不会马上执行相关联对象的select语句，需要访问主加载对象相关信息时也不会执行相关语句，而是需要相关联对象的详细信息的时候才会执行关联的selecr语句

如何开启侵入式延迟和深度延迟：

```xml
<!--全局参数设置-->
<settings>
    <!--延迟加载总开关-->
    <setting name="lazyLoadingEnabled" value="true"/>
    <!--3.4.1版本之前默认是true，之后默认是false-->
    <!--侵入式延迟加载开关，为true时就是侵入式延迟-->
    <setting name="aggressiveLazyLoading" value="true"/>
    <!--侵入式延迟加载开关，为false就是深度延迟-->
    <setting name="aggressiveLazyLoading" value="false"/>
</settings>

```

注：（只能嵌套查询，而不能嵌套结果）延迟加载要求，对关联对象的查询与主加载对象的查询必须分别进行的select语句，不能是使用多表连接所进行的select查询。因为多表连接查询，其实质是对一张表的查询，对由多个表连接后形成的一张表的查询。会一次性将多张表的所有信息查询出来。Mybatis中对于延迟加载设置，只对于resultMap中的collection和association起作用，可以应用到一对一、一对多、多对一、多对多的所有关联查询中

## 二、#{ } 和 ${ }的区别

#{ }是预编译加载，${ }是字符串替换。预编译加载会对sql语句中的#{ }使用？占位符替换，然后使用PreparedStatement的set方式进行赋值。这种方式的可以避免sql注入，保证了安全。

## 三、Mybatis有几种分页方法（逻辑分页、物理分页）

逻辑分页：使用Mybatis自带的RowBounds进行分页，他是一次性查询出多个数据，然后把数据放入内存中，在内存中进行的分页

物理分页：使用sql语句的limit进行分页，去数据库查询指定的条数

注：

1. Mybatis自带的分页方法RowBounds并不是一次性全部把所有数据存放进内存，而是由一定的条数限制。这个限制是配置在jdbc驱动中的FetchSize配置来控制一次性查询的最大数据。
2. 逻辑分页的弊端：消耗大量内存、有内存溢出的风险、对数据库压力大

## 三、Mybatis的一级缓存和二级缓存



## 四、Mybatis有哪些执行器（Executor [ɪɡˈzekjətə(r)] ）

1. SimpleExecutor：每执行一次update或select就开启一个Statement对象，用完就关
2. ReuseExecutor：执行update或select后，以sql语句作为key查找Statement对象，如果有，则使用，如果没有则创建。执行完毕后Statement对象不关闭，而是保存在Map中供下一次使用
3. BatchExecutor：执行update（没有select，select没有批处理），将所有update的sql语句添加到批处理中（addBatch()），等待统一处理（executorBatch()）。它缓存了多个Statement对象，每一个Statement对象都是添加到批处理后创建的（addBatch()），等待统一处理（executorBatch()）。

## 五、Mybatis分页插件实现分页查询的原理

原理是分页插件在插件内部实现拦截方法拦截待执行的SQL语句。然后在SQL语句末尾添加上limit实现分页。

## 六、Mybatis实现自定义插件



七、



























