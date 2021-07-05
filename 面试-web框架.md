https://blog.csdn.net/weixin_45266712/article/details/105867592?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-2.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-2.control

# Spring

## 一、SpringFramework和SpringBoot的区别

SpringFramework就是一个提供了很多基础功能整合的框架，包括JDBC、MVC、AOP、IOC等，开发者可以直接调用Spring提供的模块实现功能。SpringBoot就是在Spring的基础上，对SpringFramework又再一次进行了封装的框架，SpringBoot的特征就是“约定大于配置”。SpringBoot消除了Spring和其它框架整合以及他本身自己需要的很多xml配置文件，就非常的方便。

## 二、SpringBean的作用域（引出单例，饿汉懒汉、Session、HTTP、Portlet）

|     类别      |                             描述                             |
| :-----------: | :----------------------------------------------------------: |
|   singleton   | 在SpringIoc容器中仅存在一个Bean实例，Bean以单例方式存在，默认值 |
|   prototype   | 每次从容器调用Bean时都返回一个新的实例，即每次调用getBean()时，都会创建一个新的Bean |
|    request    | 每次HTTP请求都会创建一个新的Bean。该作用域仅适用于WebApplicationContext环境 |
|    session    | 同一个HTTP Session共享一个Bean，不同的Session使用不同Bena。仅适用于WebApplicationContext环境 |
| globalSession | 一般用于Portlet应用环境（门户系统）。该作用域仅适用于WebApplicationContext环境 |

## 三、Spring事务中的隔离级别（也就是Mysql的隔离级别）

TransactionDefinition 接口中定义了五个表示隔离级别的常量：

1. TransactionDefinition.ISOLATION_DEFAULT

   使用后端数据库默认的隔离级别，Mysql 默认采用的 REPEATABLE_READ隔离级别 Oracle 默认采用的 READ_COMMITTED隔离级别. 

2. TransactionDefinition.ISOLATION_READ_UNCOMMITTED

   最低的隔离级别，允许读取尚未提交的数据变更，可能会导致脏读、幻读或不可重复读 

3. TransactionDefinition.ISOLATION_READ_COMMITTED

   允许读取并发事务已经提交的数据，可以阻止脏读，但是幻读或不可重复读仍有可能发生 

4. TransactionDefinition.ISOLATION_REPEATABLE_READ

   对同一字段的多次读取结果都是一致的，除非数据是被本身事务自己所修改，可以阻止脏读和不可重复读，但幻读仍有可能发生。（也就是说即使在多次查询之间有新增的数据满足该查询，这些新增的记录也会被忽略）

5. TransactionDefinition.ISOLATION_SERIALIZABLE

   最高的隔离级别，完全服从ACID的隔离级别。所有的事务依次逐个执行，这样事务之间就完全不可能产生干扰，也就是说，该级别可以防止脏读、不可重复读以及幻读。但是这将严重影响程序的性能。通常情况下也不会用到该级别

## 四、Spring中事务的传播行为

所谓事务的传播行为是指，如果在开始当前事务之前，一个事务上下文已经存在，此时有若干选项可以指定一个事务性方法的执行行为。

支持当前事务的情况： 

1. TransactionDefinition.PROPAGATION_REQUIRED

   如果当前存在事务，则加入该事务；		如果当前没有事务，则创建一个新的事务

2. TransactionDefinition.PROPAGATION_SUPPORTS

   如果当前存在事务，则加入该事务；		如果当前没有事务，则以非事务的方式继续运行

3. TransactionDefinition.PROPAGATION_MANDATORY

    如果当前存在事务，则加入该事务；		如果当前没有事务，则抛出异常。（mandatory：强制性） 

不支持当前事务的情况： 

1. TransactionDefinition.PROPAGATION_REQUIRES_NEW

   创建一个新的事务，			如果当前存在事务，则把当前事务挂起

2. TransactionDefinition.PROPAGATION_NOT_SUPPORTED

   以非事务方式运行，			如果当前存在事务，则把当前事务挂起

3. TransactionDefinition.PROPAGATION_NEVER

   以非事务方式运行，			如果当前存在事务，则抛出异常

其他情况： 

1. TransactionDefinition.PROPAGATION_NESTED（嵌套事务是什么?）

   如果当前存在事务，则创建一个事务作为当前事务的嵌套事务来运行；如果当前没有事务，则该取值等价于TransactionDefinition.PROPAGATION_REQUIRED

## 四、解释一下IOC是什么？以及实现原理

IOC（Inversionof Control），控制反转。这种控制权不由当前对象管理了，由其他（类,第三方容器）来管理。它解决了以前应用程序开发的一个最主要的问题：将组件的创建+配置与组件的使用相分离，并且，由IoC容器负责管理组件的生命周期。

![image-20210628172437711](../../../../AppData/Roaming/Typora/typora-user-images/image-20210628172437711.png)

## 五、解释一下AOP原理是什么？

AOP（Aspect Oriented Program），面向切面编程。

## 六、Spring AOP和AspectJ AOP有什么区别？

Spring AOP是属于运行时增强，而AspectJ是编译时增强。Spring AOP基于代理（Proxying），而AspectJ基于字节码操作（Bytecode Manipulation）。

Spring AOP已经集成了AspectJ，AspectJ应该算得上是Java生态系统中最完整的AOP框架了。AspectJ相比于Spring AOP功能更加强大，但是Spring AOP相对来说更简单。

如果我们的切面比较少，那么两者性能差异不大。但是，当切面太多的话，最好选择AspectJ，它比SpringAOP快很多。

## 七、DI有哪些类型

构造器依赖注入（通过构造器参数注入）：触发一个类的构造函数，通过不同构造函数的参数，实现注入不同依赖的Bean

Setter方法注入：调用无参构造函数构建Bean之后，调用该Bean的Set方法注入

## 八、手写饿汉式、懒汉式（Bean的单例模式引出）

```java
// 很重要：在这里的“线程安全”的意思是指“无法重复实例化相同的一个对象”
// 饿汉式
public class SingletonEH {
    /**
     *是否 Lazy 初始化：否
     *是否多线程安全：是
     *实现难度：易
     *描述：这种方式比较常用，但容易产生垃圾对象。
     *优点：没有加锁，执行效率会提高。
     *缺点：类加载时就初始化，浪费内存。
     *它基于 classloder 机制避免了多线程的同步问题，
     * 不过，instance 在类装载时就实例化，虽然导致类装载的原因有很多种，
    * 在单例模式中大多数都是调用 getInstance 方法，
     * 但是也不能确定有其他的方式（或者其他的静态方法）导致类装载，
     * 这时候初始化 instance 显然没有达到 lazy loading 的效果。
     */
    private static SingletonEH instance = new SingletonEH();
    private SingletonEH (){}
    public static SingletonEH getInstance() {
        System.out.println("instance:"+instance);
        System.out.println("加载饿汉式....");
        return instance;
    }
}
 
// 懒汉式
public class SingletonLH {
    /**
     *是否 Lazy 初始化：是
     *是否多线程安全：否
     *实现难度：易
     *描述：这种方式是最基本的实现方式，这种实现最大的问题就是不支持多线程。因为没有加锁 synchronized，所以严格意义上它并不算单例模式。
     *这种方式 lazy loading 很明显，不要求线程安全，在多线程不能正常工作。
     */
    private static SingletonLH instance;
    private SingletonLH (){}
 
    public static SingletonLH getInstance() {
        if (instance == null) {
            instance = new SingletonLH();
        }
        return instance;
    }
}

	/**
     	注：
		饿汉式天生就是线程安全的，可以直接用于多线程而不会出现问题，
		懒汉式本身是非线程安全的，为了实现线程安全有几种写法。
	**/
// 实现了线程安全的懒汉式
public class SingletonLHsyn {
    /**
     *是否 Lazy 初始化：是
     *是否多线程安全：是
     *实现难度：易
     *描述：这种方式具备很好的 lazy loading，能够在多线程中很好的工作，但是，效率很低，99% 情况下不需要同步。
     *优点：第一次调用才初始化，避免内存浪费。
     *缺点：必须加锁 synchronized 才能保证单例，但加锁会影响效率。
     *getInstance() 的性能对应用程序不是很关键（该方法使用不太频繁）。
     */
    private static SingletonLHsyn instance;
    private SingletonLHsyn (){}
    public static synchronized SingletonLHsyn getInstance() {
        if (instance == null) {
            instance = new SingletonLHsyn();
        }
        return instance;
    }
}
```



## 九、什么是依赖注入（DI）？

依赖注入是IOC的一个功能吧。DI的意思就是开发者不用手动编写创建对象的代码，而只需要描述如何创建对象就行，之后IOC容器会把这些Bean对象创建并管理起来。

## 十、Bean的生命周期

![image-20210627153703495](../../../../AppData/Roaming/Typora/typora-user-images/image-20210627153703495.png)

Bean的生命周期大致分为4个阶段：

1. 实例化 Instantiation
2. 属性赋值 Populate
3. 初始化 Initialization
4. 销毁 Destruction

主要逻辑都在org\springframework\beans\factory\support\AbstractAutowireCapableBeanFactory#doCreate()方法中

```java
// 忽略了无关代码
protected Object doCreateBean(final String beanName, final RootBeanDefinition mbd, final @Nullable Object[] args) throws BeanCreationException {

   // Instantiate the bean.
   BeanWrapper instanceWrapper = null;
   if (instanceWrapper == null) {
      // 实例化阶段！
      instanceWrapper = createBeanInstance(beanName, mbd, args);
   }

   // Initialize the bean instance.
   Object exposedObject = bean;
   try {
      // 属性赋值阶段！
      populateBean(beanName, mbd, instanceWrapper);
      // 初始化阶段！
      exposedObject = initializeBean(beanName, exposedObject, mbd);
   }
}
```

而在实例化阶段和初始化阶段的前后，有两个接口发挥了扩展的作用

- BeanPostProcessor
- InstantiationAwareBeanPostProcessor

![image-20210627161602348](../../../../AppData/Roaming/Typora/typora-user-images/image-20210627161602348.png)

- postProcessBeforeInstantiation调用点

```java
@Override
protected Object createBean(String beanName, RootBeanDefinition mbd, @Nullable Object[] args)
        throws BeanCreationException {

    try {
        // Give BeanPostProcessors a chance to return a proxy instead of the target bean instance.
        // postProcessBeforeInstantiation方法调用点，这里就不跟进了，
        // 有兴趣的同学可以自己看下，就是for循环调用所有的InstantiationAwareBeanPostProcessor
        Object bean = resolveBeforeInstantiation(beanName, mbdToUse);
        if (bean != null) {
            return bean;
        }
    }

    try {
        // 上文提到的doCreateBean方法，可以看到
        // postProcessBeforeInstantiation方法在创建Bean之前调用
        Object beanInstance = doCreateBean(beanName, mbdToUse, args);
        if (logger.isTraceEnabled()) {
            logger.trace("Finished creating instance of bean '" + beanName + "'");
        }
        return beanInstance;
    }        
}
```

可以看到resolveBeforeInstantiation方法，也就是postProcessBeforeInstantiation方法的调用是在doCreateBean方法之前的。

- postProcessAfterInstantiation调用点（在属性赋值之前执行）

```java
protected void populateBean(String beanName, RootBeanDefinition mbd, @Nullable BeanWrapper bw) {

   // Give any InstantiationAwareBeanPostProcessors the opportunity to modify the
   // state of the bean before properties are set. This can be used, for example,
   // to support styles of field injection.
   boolean continueWithPropertyPopulation = true;
    // InstantiationAwareBeanPostProcessor#postProcessAfterInstantiation()
    // 方法作为属性赋值的前置检查条件，在属性赋值之前执行，能够影响是否进行属性赋值！
   if (!mbd.isSynthetic() && hasInstantiationAwareBeanPostProcessors()) {
      for (BeanPostProcessor bp : getBeanPostProcessors()) {
         if (bp instanceof InstantiationAwareBeanPostProcessor) {
            InstantiationAwareBeanPostProcessor ibp = (InstantiationAwareBeanPostProcessor) bp;
            if (!ibp.postProcessAfterInstantiation(bw.getWrappedInstance(), beanName)) {
               continueWithPropertyPopulation = false;
               break;
            }
         }
      }
   }
   // 忽略后续的属性赋值操作代码
```

面试回答：

Bean的生命周期大致分为4个阶段：

1. 实例化 Instantiation（1）实例化前后会调用postProcessBeforeInstantiation和postProcessAfterInstantiation
2. 属性赋值 Populate（2、3、4、5）populateBean方法内会先执行postProcessAfterInstantiation，然后再赋值
3. 初始化 Initialization（6、7、8）初始化前后会调用postProcessBeforeInitialization和postProcessAfterInitialization。调用初始化的操作是invokeInitMethods(beanName, wrappedBean, mbd);
4. 销毁 Destruction（10）调用DisposableBean接口的destory()方法
1. **Spring启动，查找并加载需要被Spring管理的bean，进行Bean的实例化**
2. **Bean实例化后对将Bean的引入和值注入到Bean的属性中**
3. **如果Bean实现了BeanNameAware接口的话，Spring将Bean的Id传递给setBeanName()方法**
4. **如果Bean实现了BeanFactoryAware接口的话，Spring将调用setBeanFactory()方法，将BeanFactory容器实例传入**
5. **如果Bean实现了ApplicationContextAware接口的话，Spring将调用Bean的setApplicationContext()方法，将bean所在应用上下文引用传入进来**
6. **如果Bean实现了BeanPostProcessor接口，Spring就将调用他们的postProcessBeforeInitialization()方法。**
7. **如果Bean 实现了InitializingBean接口，Spring将调用他们的afterPropertiesSet()方法。类似的，如果bean使用init-method声明了初始化方法，该方法也会被调用**
8. **如果Bean 实现了BeanPostProcessor接口，Spring就将调用他们的postProcessAfterInitialization()方法。**
9. **此时，Bean已经准备就绪，可以被应用程序使用了。他们将一直驻留在应用上下文中，直到应用上下文被销毁。**
10. **如果bean实现了DisposableBean接口，Spring将调用它的destory()接口方法，同样，如果bean使用了destory-method 声明销毁方法，该方法也会被调用。**

## 十（二）、Bean的生命周期（推荐）

![image-20210628175501501](../../../../AppData/Roaming/Typora/typora-user-images/image-20210628175501501.png)

1.Bean容器找到配置文件中Spring Bean的定义。

2.Bean容器利用Java Reflection API创建一个Bean的实例。

3.如果涉及到一些属性值，利用set()方法设置一些属性值。

4.如果Bean实现了BeanNameAware接口，调用setBeanName()方法，传入Bean的名字。

5.如果Bean实现了BeanClassLoaderAware接口，调用setBeanClassLoader()方法，传入ClassLoader对象的实例。

6.如果Bean实现了BeanFactoryAware接口，调用setBeanClassFacotory()方法，传入ClassLoader对象的实例。

7.与上面的类似，如果实现了其他*Aware接口，就调用相应的方法。

8.如果有和加载这个Bean的Spring容器相关的BeanPostProcessor对象，执行postProcessBeforeInitialization()方法。

9.如果Bean实现了InitializingBean接口，执行afeterPropertiesSet()方法。

10.如果Bean在配置文件中的定义包含init-method属性，执行指定的方法。

11.如果有和加载这个Bean的Spring容器相关的BeanPostProcess对象，执行postProcessAfterInitialization()方法。

12.当要销毁Bean的时候，如果Bean实现了DisposableBean接口，执行destroy()方法。

13.当要销毁Bean的时候，如果Bean在配置文件中的定义包含destroy-method属性，执行指定的方法。

## 十一、Bean装配（过程是如何实现的）

Spring容器把bean组装到一起，前提是容器需要知道bean的依赖关系，如何通过依赖注入来把它们装配到一起

## 十二、Bean自动装配（过程是如何实现的）

Spring能自动装配相互合作的bean，这意味着容器不需要<constructor-arg>和<property>配置，能通过Bean工厂自动处理bean之间的协作

## 十三、有哪些自动装配方式

五种自动装配方式，用于指导Spring容器用自动装配方式进行依赖注入。

1. no：默认方式是不进行自动装配，而是显示设置ref属性来进行装配
2. byName：通过参数名自动装配。通过配置bean的autowire属性为byname，之后容器就会装配和bean属性名称相同的bean
3. byType：通过参数类型自动装配。通过配置bean的autowire属性为bytype，之后容器就会装配和bean属性名称相同的bean。如果有多个相同类型的bean，则抛出错误
4. constructor：通过配置bean的autowire属性为constructor。这个方式类似于byType，但是要提供给构造器参数，如果没有确定的带参数的构造器参数类型，将会抛出异常
5. autodetect：首先尝试使用constructor自动装配，如果无法工作，则使用byType的方式

## 十四、注解





## 十五、你知道Spring的事务是怎么实现的么？（基于AOP、动态代理）

都知道啦，动态代理嘛有两种方式。第一种就是jdk的动态代理，第二种呢就是常用的CGLIB了。这两种方式有什么区别呢？CGLIB的原理其实是生成代理类的子类，

然后对代理类的方法进行重写，在重写的方法上添加上需要的额外功能（就有点类似于编程式事务）。

动态代理主要就是实现一个接口InvocationHandler，然后重写invoke方法。然后在invoke方法内通过反射的机制执行代理类的方法。然后在反射执行代理类方法前后添加额外的功能实现。

```java
public class DynaProxyHello implements InvocationHandler{
    private Object target;//目标对象
    
    /**
     * 通过反射来实例化目标对象
     * @param object
     * @return
     */
    public Object bind(Object object){
        this.target = object;
        return Proxy.newProxyInstance(this.target.getClass().getClassLoader(),this.target.getClass().getInterfaces(), this);
    }
    
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        Object result = null;
        //---------------添加额外的方法-------------------
        System.out.println("start"+str);
        //----------通过反射机制来运行目标对象的方法-----------
        result = method.invoke(this.target,args);
        System.out.println("end"+str);
        return result;
    }
}
```





## 十六、AOP切面（Aspect）的组成（涉及代理对象）

1. 连接点（Joinpoint）：被拦截到的点，该连接点可以是被拦截到的方法、字段或者构造器；
2. 切入点（Pointcut）：指要对哪些连接点进行拦截，即被拦截的连接点；
3. 通知（Advice）：指拦截到连接点之后要执行的代码，通知分为前置(**before**)、后置(**after**)、异常(**after-throwing**)、最终(**after-returning**)、环绕通知(**around**)；
4. 目标（Target）：代理的目标对象；
5. 织入（Weaving）：把增强的代码应用到目标上，生成代理对象的过程；
6. 切面（Aspect）：切入点和通知的集合。

```xml
<!-- 在Spring中配置AOP-->
<!-- 事务管理器,依赖于数据源(用于控制回滚等操作) -->
<bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"/>
</bean>

<!-- 编写通知：对事务进行增强，需要对切入点和具体执行事务细节 -->
<tx:advice id="txAdvice" transaction-manager="txManager">
    <tx:attributes>
        <!-- <tx:method> 给切入点添加事务详情
                 name:方法名称, *表示任意方法, do* 表示以do开头的方法
                 propagation:设置传播行为
                 isolation:隔离级别
                 read-only:是否只读 -->
        <tx:method name="*" propagation="REQUIRED" isolation="DEFAULT" read-only="false"/>
    </tx:attributes>
</tx:advice>

<!-- aop编写,让Spring自动对目标进行代理，需要使用AspectJ的表达式 -->
<aop:config>
    <!-- 切入点 -->
    <aop:pointcut expression="execution(* com.jdbc.AccountServiceImpl.*(..))" id="txPointCut"/>
    <!-- 切面:将切入点和通知整合 -->
    <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointCut"/>
</aop:config> 

<!-- 配置日志打印类（上面是没有实现日志的方式，下面是实现了日志的方式） --> 
<bean id="logHandler" class="com.jdbc.LogHandler"/>
<aop:config>
    <!-- order属性表示横切关注点的顺序，当有多个时，序号依次增加 -->
    <aop:aspect id="log" ref="logHandler" order="1">
        <!--  切入点为AccountServiceImpl类下的transfer方法 -->
        <aop:pointcut id="logTime" expression="execution(* com.jdbc.AccountServiceImpl.transfer(..))"/>
        <aop:before method="LogBefore" pointcut-ref="logTime"/>
        <aop:after method="LogAfter" pointcut-ref="logTime"/>
    </aop:aspect>
</aop:config>

```

## 十七、Spring中单例bean的成员变量如何解决线程安全问题

问题：多个线程操作同一个对象的时候，对这个对象的非静态成员变量的写操作会存在线程安全问题

解决方案

1、在bean中尽量避免定义可变的成员变量（不太现实）

2、在类中定义一个ThreadLocal成员变量，将需要的可变成员变量保存在ThreadLocal变量中（推荐）

## 十八、Spring中使用了哪些设计模式（大概率会问源码）

- 工厂设计模式：通过BeanFactory和ApplicationContext创建bean对象
- 代理设计模式：Spring AOP功能的实现（动态代理、反射）
- 单例设计模式：Spring中的bean默认都是单例的
- 模板方法模式：Spring中的jdbcTemplate、hibernateTemplate等以Template结尾的对数据库操作的类都使用了模板模式
- 包装器设计模式：项目需要连接多种不同的数据库。这种模式可以根据客户需求动态更换不同的数据源
- 观察者模式：Spring事件驱动模型（？？？）
- 适配器模式：Spring AOP的增强或通知（Advice）使用到了适配器模式，SpringMVC使用适配器模式（HandlerAdapter）适配Controller

## 十九、@Component和@Bean注册bean的方式的不同

1. 作用对象：component只作用于类，而Bean作用于方法
2. 实现：component通过**类路径扫描**来自动侦测类并把类装配到Spring容器中，而Bean注解是把方法返回的实体类作为bean注入到容器中的

## 二十、Spring的启动流程

https://blog.csdn.net/moshenglv/article/details/53517343

总结：

1. web.xml中配置contextLoaderListener。在web容器启动时，这个监听器会监听到容器被初始化，于是就会调用contextInitialized方法（完成对上下文的初始化）（关闭项目时会触发contextDestroyed）
2. 在这个方法中其实也只做了一件事，那就是：通过父类的contextLoader的initWebApplicationContext方法创建Sping上下文对象
3. initWebApplicationContext做了三件事①创建WebApplicationContext②加载对应的Spring文件创建里面的bean③将WebApplicationContext放入ServletContext（java Web的全局变量）
   1. 创建Web应用上下文会调用createWebApplicationContext方法来创建上下文对象（如果需要自定义，需要继承ConfigurableWebApplicationContext，而Spring MVC默认使用ConfigurableWebApplicationContext作为ApplicationContext（它仅仅是一个接口）的实现）
   2. configureAndRefreshWebApplicationContext方法用于封装ApplicationContext数据并且初始化所有相关Bean对象。它会从web.xml中读取名为 contextConfigLocation的配置，这就是spring xml数据源设置，然后放到ApplicationContext中，最后调用传说中的refresh方法执行所有Java对象的创建。
   3. 完成ApplicationContext创建之后就是将其放入ServletContext中，注意它存储的key值常量
4. 完成了web上下文对象得创建之后，开始创建servlet，首先创建的就是DispatcherServlet。创建DispatcherServlet是根据xml配置文件中的<servlet>标签指定servlet-name、servlet-class、init-param





# SpringMVC

## 一、SpringMvc的执行流程（问底层？）

![image-20210629210106610](../../../../AppData/Roaming/Typora/typora-user-images/image-20210629210106610.png)

## 二、DispatcherServlet的重要性

Spring的MVC框架是围绕DispatcherServlet来设计的，它用来处理所有的HTTP请求和响应

## 三、WebApplicationContext介绍

WebApplicationContext继承了ApplicationContext并增加了一些WEB应用必备的特有功能，它不同于一般的ApplicationContext，因为它能处理主题，并找到被关联的servlet

## 四、@Controller注解（控制器）

该注解表明该类扮演控制器的角色，Spring不需要你继承任何其他控制器基类或引用ServletAPI。

五、









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



# SpringBoot

https://zhuanlan.zhihu.com/p/163685081（SpringBoot自动装配原理）

一、SpringBoot的启动流程

# SpringCloud

一、



# Nacos

一、















