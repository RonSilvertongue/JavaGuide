# Spring Framework Core Technologies

## 1 The IoC Container

### 1.1 Introduction to the Spring IoC Container and Beans

- 本节讲述Inversion of Control (IoC)原则的实现（即Spring是如何实现IoC的）
- IoC也被认知为dependency injection (DI)
- 对比传统的由客户端负责调用Bean的构造器来添加依赖，由容器去注入对象所需要依赖的过程，就是一个控制权移交的过程
- Spring IoC 的两个基础包 `org.springframework.beans`  and   `org.springframework.context` 
- Spring中的两个接口： [`BeanFactory`](https://docs.spring.io/spring-framework/docs/5.3.5/javadoc-api/org/springframework/beans/factory/BeanFactory.html) 和 [`ApplicationContext`](https://docs.spring.io/spring-framework/docs/5.3.5/javadoc-api/org/springframework/context/ApplicationContext.html) 
  -  `BeanFactory` 提供了一个配置框架和基本的功能
  - ` ApplicationContext` 是 `BeanFactory` 的一个超集，它提供了更多的企业级功能
- 在Spring中，所有托管给Spring IoC 容器的对象统一称作Bean
- 在Spring中，所有被托管的Bean都在配置文件中体现

### 1.2 Container Overview

-  `org.springframework.context.ApplicationContext` 接口代表了Spring IoC 容器并且它负责将Bean初始化，配置，装配。这个接口从配置信息中获取需要被初始化，配置，装配的Bean。可以用XML文件、Java注解或者Java代码来撰写配置信息（即配置的三种方法）。

- 有多种实现` ApplicationContext` 接口的方式，其中普遍的使用 [`ClassPathXmlApplicationContext`](https://docs.spring.io/spring-framework/docs/5.3.5/javadoc-api/org/springframework/context/support/ClassPathXmlApplicationContext.html) or [`FileSystemXmlApplicationContext`](https://docs.spring.io/spring-framework/docs/5.3.5/javadoc-api/org/springframework/context/support/FileSystemXmlApplicationContext.html) 的实例，他们都是` ApplicationContext` 的实现类。除了以传统的XML文件形式来描述配置信息，还可以让容器使用由Java注解或Java代码描述的配置信息，想要除了XML文件形式以外的两种配置形式，只需要在XML文件中进行少量配置来启动对另外两种配置形式的支持

- 在大多数应用场景下，并不需要手动的实例化一个或者多个容器。比如说，在Web应用的场景下，只需要在web.xml中写上简单的八行配置就足够了。

- 下面的图描述了Spring的IoC容器如何工作。配置文件中包含了所有类的信息，当容器被创建并初始化了之后，所有Bean都已经被实例化了。

  ![container magic](Spring Framework Core Technologies.assets/container-magic.png)

#### 1.2.1. Configuration Metadata





