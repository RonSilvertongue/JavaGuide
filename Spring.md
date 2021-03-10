# 1 Spring

## 1.1 IOC 控制反转

控制反转是一种设计原则，其中最常见的方式叫做依赖注入（DI，Dependency Injection），还有一种方式叫“依赖查找”（Dependency Lookup）。通过控制反转，对象A在被创建的时候，由一个调控系统内所有对象的外界实体将其所依赖的对象B的引用传递给它（对象A）。

理解，当一个对象A依赖于另一个对象B时，常见的操作是在A中new一个对象B，而控制反转则不需要在对象A中显示的new对象B，只需要提供set方法或者构造方法，由容器控制程序创建对象B，再由容器控制程序利用对象A的set或者构造方法将对象B注入对象A。配合接口使用可以大大提高程序的灵活性。

## 1.2 使用Spring提供的IOC容器

1.在resources文件夹下面创建 [自己取名].xml （resources文件夹下的东西被编译后统一进入类路径Classpath中），在xml文件中注册需要托管的类，这些类统称为Bean。xml文件内容如下：

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
<!--    使用spring创建对象，再spring中这些都称为bean-->
    <bean id="mysql" class="com.zcr.dao.UserDAOMysqlImpl"/>
    <bean id="oracle" class="com.zcr.dao.UserDAOOracleImpl"/>

    <bean id="UserServiceImpl" class="com.zcr.service.UserServiceImpl">
<!--        ref选择的是注册在beans.xml文件中的bean，并将其注入-->
<!--        value是设置具体的值，一般用于基本数据类型-->
        <property name="userDAO" ref="oracle"/>
    </bean>

</beans>
```

2.开始使用容器，使用时容器需要前面创建的xml配置文件；代码如下：

```java
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");
```

容器可以接收多个xml文件。

3.注意：如果被托管的类没有无参构造函数，在xml配置中，需要在bean结点中配置construct-arg结点，具体如下：

参数需要的对象在xml中配置了。

``` xml
<beans>
    <bean id="beanOne" class="x.y.ThingOne">
        <constructor-arg ref="beanTwo"/>
        <constructor-arg ref="beanThree"/>
    </bean>

    <bean id="beanTwo" class="x.y.ThingTwo"/>

    <bean id="beanThree" class="x.y.ThingThree"/>
</beans>
```

**Constructor argument type matching**

 the container can use type matching with simple types if you explicitly specify the type of the constructor argument by using the attribute. as the following example shows:`type`

``` xml
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg type="int" value="7500000"/>
    <constructor-arg type="java.lang.String" value="42"/>
</bean>
```

**Constructor argument index**

You can use the attribute to specify explicitly the index of constructor arguments, as the following example shows:`index`

``` xml
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg index="0" value="7500000"/>
    <constructor-arg index="1" value="42"/>
</bean>
```

**还可以通过name和value去赋值**

``` xml
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg name="..." value="..."/>
    <constructor-arg name="..." value="..."/>
</bean>
```



4.在xml配置文件中注册的bean，在xml文件被加载的时候，所有被注册bean都会被初始化，效率低，最好使用lazyload



## 1.3 Spring配置（context上下文）

``` xml
    <!--
    import结点
    resource：要导入的xml文件的路径-->
    <import resource="beans.xml"/>
    <!--
    bean结点
    id: bean的唯一标识符，用于取出对象
    class：bean对象的全限定名
    name：别名，可以取多个别名，空格或者逗号或者分号
    scope：
-->
    <bean id="hello" class="com.zcr.pojo.Hello" name="hello2,hello3">
        <property name="str" value="Spring"/>
    </bean>
```



## 1.4 依赖注入

### 1.4.1 构造器注入

### 1.4.2 set注入



### 1.4.3 扩展的注入方式

P命名空间和C命名空间，需要在xml结点配置命名空间



## 1.5 Bean Scopes



### 1.5.1 Singleton 单例模式

一个Spring IOC Container拥有一个唯一的实例。适合没有状态的Bean

### 1.5.2 Prototype 原型

每一次的注入都会注入全新的独占的实例，这种Scope适合有状态的Bean，即不同情况下使用该实例时可能会产生不同的状态，因此，针对每一种使用情况都应该拥有一个全新的实例，而不是像Singleton那样不管什么情况都是使用同一个实例。

> In contrast to the other scopes, Spring does not manage the complete lifecycle of a prototype bean.The container instantiates, configures, and otherwise assembles a prototype object and hands it to the client, with no further record of that prototype instance.Thus, although initialization lifecycle callback methods are called on all objects regardless of scope, in the case of prototypes, configured destruction lifecycle callbacks are not called. 

与其他Scope不同，Prototype-Scoped Bean的生命周期不完全由Spring托管。在容器创建，配置，组装完原型对象并交给客户端后，容器就不管Prototype-Scoped Bean了。虽然初始化生命周期回调方法不考虑Scope，但是销毁生命周期回调方法并不会在Prototype-Scoped Bean上调用。

注意：当你在Singleton-Scoped Bean中引用了Prototype-Scoped Bean，DI发生在初始化阶段，且对于这个SSB来说，这个PSB是唯一且不变的，如果想在运行阶段使SSB获得一个全新的PSB，需要让SSB实现**ApplicationContextAware** 接口，例子如下：

``` java
// a class that uses a stateful Command-style class to perform some processing
package fiona.apple;

// Spring-API imports
import org.springframework.beans.BeansException;
import org.springframework.context.ApplicationContext;
import org.springframework.context.ApplicationContextAware;

public class CommandManager implements ApplicationContextAware {

    private ApplicationContext applicationContext;

    public Object process(Map commandState) {
        // grab a new instance of the appropriate Command
        Command command = createCommand();
        // set the state on the (hopefully brand new) Command instance
        command.setState(commandState);
        return command.execute();
    }

    protected Command createCommand() {
        // notice the Spring API dependency!
        return this.applicationContext.getBean("command", Command.class);
    }

    public void setApplicationContext(
            ApplicationContext applicationContext) throws BeansException {
        this.applicationContext = applicationContext;
    }
}
```

### 1.5.3 Request, Session, Application, and WebSocket Scopes

## 1.6 Bean自动装配

- 自动装配式Spring满足bean依赖的一种方式
- Spring会在上下文中自动寻找，并自动给Bean装配属性



在Spring中有三种装配方式

1. 在xml中显示的配置
2. 在java中显示的配置
3. 隐式的自动装配【重要】

### 1.6.1 xml中自动装配

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="cat" class="com.zcr.pojo.Cat"/>
    <bean id="dog" class="com.zcr.pojo.Dog"/>

<!--    <bean id="people" class="com.zcr.pojo.People">-->
<!--        <property name="cat" ref="cat"/>-->
<!--        <property name="dog" ref="dog"/>-->
<!--        <property name="name" value="zcr"/>-->
<!--    </bean>-->

    <!-- ByName 如果属性对应的set方法名字为setXXX，ByName方式就会在容器中查找ID为XXX（忽略大小写）的Bean -->
<!--    <bean id="people" class="com.zcr.pojo.People" autowire="byName"/>-->

    <!-- ByType 在容器中找到相同类型的Bean，当相同类型的Bean不唯一时会报错 -->
    <bean id="people" class="com.zcr.pojo.People" autowire="byType"/>

</beans>
```

### 1.6.2 使用注解自动装配

使用注解需要：

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

</beans>
```

@AutoWired 默认by type，by name用的是变量名

@Qualifier（value=）指定Bean

@Resource default by name用的是变量名，by type



## 1.7 使用注解开发

在spring4之后使用注解开发，必须要导入aop的包

1. bean
   - @Component 注册一个bean
2. 属性如何注入
   - @Value 给属性赋值，简单赋值
3. 衍生的注解
   - 注册Bean的衍生注解
     - dao层  【@Repository】
     - service层 【@Service】
     - controller层 【@Controller】
4. 自动装配
5. 作用域
   - @Scope，与注册类的注解一起用可以设置其作用域
6. 小结
   - xml与注解
     - xml更加方便，适用于任何场合，维护简单方便
     - 注解开发需要依靠自动装配，维护相对复杂
   - xml与注解的最佳实践：
     - xml用来管理bean
     - 注解只负责完成属性的注入
     - 在使用的过程中，只需要注意一个问题：让注解生效，必须要开启注解的支持

## 1.8 基于Java的配置



JavaConfig是Spring的一个子项目，在Spring4之后它成为了核心功能

``` java
public class User {
    public String name;

    public String getName() {
        return name;
    }

    @Value("zcr")
    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                '}';
    }
}


@Configuration //这个注解会把JavaConfig类注册为Bean
public class JavaConfig {
    @Bean   //注册一个Bean，方法名就是Bean的名字
    public User getUser(){
        return new User();
    }
}



public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(JavaConfig.class);
        User u = context.getBean("getUser",User.class);
        System.out.println(u);
    }
}
```



@Configuration 注册一个配置类

@Import 导入其他配置类

@ConponentScan 指定扫描的包，指定后，会去指定包下扫描注解



## 1.9 代理模式

一种设计模式， 代理模式分类：

- 静态代理
- 动态代理

### 1.9.1 静态代理

角色分析：

- 抽象角色：一般会使用接口或者抽象类来解决
- 真实角色：被代理的角色
- 代理角色：代理真实角色，代理真实角色后，一般会做一些增量操作
- 客户：访问代理角色的角色



## 1.10 AOP

面向切面编程

### 1.10.1 AOP在Spring中的作用

提供声明式事务；允许用户自定义切面

相关名词解释：

- 横切关注点：跨越应用程序多个模块的方法或功能。即，与业务逻辑无关的，但需要关注的部分。如日志，安全，缓存，事务等等
- 切面Aspect ：横切关注点被模块化的特殊对象。即，它是一个类
- 通知Advice ：切面必须要完成的工作。即，它是类中的方法。有五种类型的Advice
  - 前置通知：方法执行前进行通知
  - 后置通知：方法执行后进行通知
  - 环绕通知：方法执行前后进行通知
  - 异常抛出通知：方法抛出异常时进行的通知
  - 引介通知：类中增加新的方法和属性时进行的通知
- 目标Target ：被通知的对象
- 代理Proxy ：向目标对象应用通知后创建的对象。
- 切入点PointCut ：切面通知执行的“地点”的定义
- 连接点JoinPoint ：与切入点匹配的执行点。即，进行通知的时机，条件

![image-20210308195328186](Spring.assets\image-20210308195328186.png)



理解：将“通知”织入“目标对象”生成“代理类”



### 1.10.2 使用Spring实现AOP（默认JDK，可选CGlib）

需要依赖包

``` xml
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.9.4</version>
        </dependency>
```

**实现方式1：使用Spring的接口**

![image-20210308204601852](Spring.assets\image-20210308204601852.png)

``` xml
    <aop:config>
        <!--切入点：expression 表达式,execution(要执行的位置，修饰词，返回值，类名，方法名，参数)-->
        <aop:pointcut id="pointcut"   expression="execution(* com.zcr.service.UserServiceImpl.*(..))"/>

        <!-- 执行环绕通知 -->
        <aop:advisor advice-ref="log" pointcut-ref="pointcut"/>
        <aop:advisor advice-ref="afterReturningLog" pointcut-ref="pointcut"/>

    </aop:config>
```

在编写好advisor类并注册为Bean后，整个流程可以总结为：

1. 定义切入点，即你想在那个方法执行通知
2. 将切面类与切入点绑定，这样切面类中的通知就会在切入点进行通知了



**实现方式2：自定义类来实现AOP**

1. 自定义一个类，并将其注册为Bean

2. 配置

   ``` xml
       <bean id="diy" class="com.zcr.diy.DiyPointCut"/>
       <aop:config>
           <aop:aspect ref="diy">
               <aop:pointcut id="point" expression="execution(* com.zcr.service.UserServiceImpl.*(..))"/>
               <aop:before method="before" pointcut-ref="point"/>
               <aop:after method="after" pointcut-ref="point"/>
           </aop:aspect>
       </aop:config>
   ```

3. 注意：如果想要将被通知方法的参数传递给通知方法，需要在aop:pointcut结点的expression属性中添加args（参数名字），参数必须和被通知方法的参数一致，要填全部参数，不能只填一部分，另外，通知方法中也必须含有这些参数。

4. 在方式2中，可以在通知方法中设置JoinPoint参数来获取被通知方法所在的类以及被通知方法的方法签名以及方法参数，设置JoinPoint参数不需要在xml文件中进行额外配置

理解：此时是定义了一个切面，与方式1不同的是，方式1直接指定的advisor，具体的通知类型根据advisor的接口来自动匹配，而方式2除了要指定advisor类外，还需要指定通知类型，并为特定的通知类型来指定特定的方法。

猜想：SpringAOP的具体代理方式大概是编写完advisor类后，将advisor类放入InvocationHandler中，在代理特定的方法时再通过通知类型来调用advisor类中的通知方法。

**实现方式3：注解实现**

``` xml
<aop:aspectj-autoproxy/> //使注解生效
```

定义切面类

@Aspect @Before @After @Around



总结：

一个切面包含了三个要素

1. advisor，通知类
2. advice，通知，即类中的方法
3. pointcut，切入点，即在哪个方法上进行通知

通知与切入点的结合生成了连接点JoinPoint，可以通过连接点来获取一些信息，如：被通知方法所属的类（目标对象）的信息；可以获得类名，被通知方法名等等。

# 待整理

jdk动态代理：

1. 代理类

   - 创建代理类需要的东西
     - InvocationHandler 调用处理器
     - ClassLoader 类加载器
     - Class[] interfaces 被代理的类实现的接口们
   - InvocationHandler 调用处理器需要的东西
     - 被代理的类
     - 需要实现Invoke方法，其参数为：
       - Proxy，代理类，这个参数的作用没有头绪，待研究
       - Method，方法
       - args，方法所需要的参数

2. 注意点：Method的invoke方法返回的是Method的返回类型，例如：

   ``` java
   public String getString(){
       return "1234565";
   }
   ```

   当使用反射的方式（invoke）调用上述的方法时，invoke返回的对象就是String，即“1234565”.

   如果方法返回类型时void，则invoke返回的时null

   在实现调用处理器中的invoke方法时，为了保持一至，需要使用Method.invoke的返回值作为返回值，不然一些操作会出错，例如在代理equals方法时，代理类中的代码如下：

   ``` java
   public final boolean equals(Object var1) throws  {
           try {
               return ((Boolean)super.h.invoke(this, m1, new Object[]{var1})).booleanValue();
           } catch (RuntimeException | Error var3) {
               throw var3;
           } catch (Throwable var4) {
               throw new UndeclaredThrowableException(var4);
           }
       }
   ```

   如果此时将调用处理器中的invoke方法的返回值主动设置为null或者非bool型变量，在使用代理的equals方法时就可能会出现空指针异常或者类型转换错误

   

