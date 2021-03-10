# 配置开发环境

## 卸载JDK

1. 删除java安装的目录
2. 删除环境变量JAVA_HOME
3. 删除path下的所有关于java的目录

## 安装JDK

1. 安装java

2. 配置环境变量

   - 增加一个JAVA_HOME，值为java安装目录的根目录

   - 配置两个path

     - %JAVA_HOME%\bin
     - %JAVA_HOME%\jre\bin

   - 在cmd中用**java -version**命令测试是否成功

     ![image-20210212222532040](C:\Users\10645\Desktop\MarkDown\Java.assets\image-20210212222532040.png)







# HelloWorld

1. 随便打开一个文件夹存放代码
2. 新建一个java文件
   -  HelloWorld.java
3. 编写代码

```java
public class HelloWorld {
    public static void main(String[] args){
        String str="111";
        System.out.println(str);
    }
}
```

4. 编译使用命令**javac -文件名** 会生成一个.class文件

   运行使用命令**java 文件名** 运行的是.class文件

   ![image-20210212221938164](C:\Users\10645\Desktop\MarkDown\Java.assets\image-20210212221938164.png)





# 类、对象

类是对象的模板，是一类对象所具有的共同特征的抽象

类中包含**属性（或字段）**和**方法**

类的实质是一种**引用数据类型**，由于是一种**数据类型**，所以只有类被实例化为对象时才能被操作。

类具有三大特性：**封装、继承、多态**

## 构造器（构造方法）

每一个类中都有一个默认的**构造器（构造方法）**。默认的**构造器（构造方法）**的参数和方法体是空的，是一个**无参构造器**

可以在**构造器（构造方法）**中初始化对象的**属性（字段）**

若定义了一个有参构造器，则无参构造器必须显式定义。若定义了有参构造器而不定义无参构造器且创建对象时不给参数，则会报错。

匿名代码块也能赋初值



## 对象的创建

new 对象名();

创建子类对象时，会先调用父类的构造方法

若父类存在有参构造方法，则子类无法定义无参构造方法，除非子类在无参构造方法中显式的调用父类的有参构造方法。



## 继承

extends

继承父类所有子类可访问的方法和字段，包括静态字段和方法





## 多态

根据传入具体对象不同，调用不同的方法

方法中参数列表使用顶级父类，不同的子类对象传入时可以调用不同的方法



## 方法重写

静态方法和非静态方法有区别











## 抽象

public abstract XXX class{}

可以包含正常方法和属性

有默认构造方法





## 接口

方法默认 public abstract

属性默认 public static final

接口不能实例化，接口中没有构造方法

可以多继承接口





# 异常

![image-20210218225147470](C:\Users\10645\AppData\Roaming\Typora\typora-user-images\image-20210218225147470.png)





## 处理异常

### 主动抛出异常

#### 方法内抛出异常

``` java 
if(b==0){
    throw new Exception();
}
```

#### 方法上抛出异常

``` java
public void methodName(String[] args) throws Exception{}
```

### 捕获异常

``` java
try{
    
}catch(Exception e){
    
}finally{//不管有没有异常都会执行
    
}
```



### 异常处理五个关键字

try、catch、finally、throw、throws



### 自定义异常

继承Exception类就行



# 注解

在定义注解时，可以为注解添加属性以及对应的默认属性值，若没有给默认值，则在使用注解的时候需要给定特定的值。

注解和反射机制配合使用，可以通过反射提取出类，方法，属性上所使用的注解的值，根据其值来进行对应的操作

## 内置注解

## 元注解

![image-20210218232700705](C:\Users\10645\AppData\Roaming\Typora\typora-user-images\image-20210218232700705.png)

@Retention用来表示注解在什么时期有效，SOURCE源代码有效，CLASS被编译成Class文件后也有效，RUNTIME 运行时期也有效

## 自定义注解

![image-20210218233914494](C:\Users\10645\AppData\Roaming\Typora\typora-user-images\image-20210218233914494.png)



# 反射

反射用于在程序执行期间，借助Reflection API取得任何类的内部信息，并能直接操作任意对象的内部属性及方法。

在类被加载完之后，在堆中就会生成一个Class类型的对象（一个类只对应一个Class类型的对象），这个对象包含了完整的类的结构信息。我们可以通过这个对象看到类的结构。

## 反射提供的功能

1. 运行时判断对象所属的类
2. 运行时构造任意一个类的对象
3. 运行时判断任意一个类所具有的成员变量和方法
4. 运行时获取泛型信息
5. 运行时调用任意一个对象的成员变量和方法
6. 运行时处理注解
7. 动态代理

## 有哪些类型可以有Class对象

![image-20210219002549321](C:\Users\10645\AppData\Roaming\Typora\typora-user-images\image-20210219002549321.png)

``` java
Class c1 = 类名.class;
Class c1 = 接口名.class;//接口
Class c1 = String[].class;//一维数组
Class c1 = int[][].class;//二维数组
Class c1 = Enum.class;//枚举
Class c1 = Annotation.class;//注解
Class c1 = Integer.class;//基本数据类型
Class c1 = void.class;//void
Class c1 = Class.class;//Class
```



# 类加载

## 类加载过程

1. 类的加载，将类的class文件读入内存，并为其创建一个java.lang.Class对象。此过程由类加载器完成。
2. 类的链接，将类的二进制数据合并到JRE中
3. 类的初始化，JVM负责对类进行初始化

![image-20210219003413461](C:\Users\10645\AppData\Roaming\Typora\typora-user-images\image-20210219003413461.png)

类加载过程中，初始化阶段仅仅初始化静态成员变量以及静态代码块；

类加载过程中，如果发现父类没有被加载，则先加载父类；

### 什么时候会发生类的初始化

![image-20210219004707875](C:\Users\10645\AppData\Roaming\Typora\typora-user-images\image-20210219004707875.png)





# 字节流与字符流

字符流会有一个编码和解码的过程，字节流没有，某些任务上字节流效率更高

字节流是字符流的基础，字符流就是在字节流的基础上增加了编码和解码操作。

Writer会进行编码，Reader会进行解码；

解码时如果没有指定解码方式，则会使用系统默认的GBK解码方式，若读取的文件为UTF-8编码则会出现乱码。

字符流会用到缓冲区（内存）如果在写操作不调用close或者flush方法，写操作的内容不会输出到文件中；字节流不会用到缓冲区（内存），所有写操作都会直接在文件中体现出来。

猜想：字符流会用到缓冲区与编码有关。

文件拷贝功能使用字节流会比较有效率。

unicode代表一个字符集以及一个编码方式

utf-8代表一个字符集以及一个编码方式





# JAVA TIPS

1. web app中，maven会将src目录下的所有文件编译打包到WEB-INF文件下，src/main/resources文件夹会被删除，里面的文件统一打包到WEB-INF/classes文件夹下面，src/main/java下的目录结构会保留，并打包到WEB-INF/classes下。

   ![image-20210222182410936](C:\Users\10645\Desktop\MarkDown\Java.assets\image-20210222182410936.png)

   ![image-20210222182545326](C:\Users\10645\Desktop\MarkDown\Java.assets\image-20210222182545326.png)

   classes文件夹就是**常说的类路径**

   注意：

   - ​	java下的文件在没有POM设置的情况下，仅编译并打包.java文件，其余资源文件一概忽略；POM设置如下

     ![image-20210222182926327](C:\Users\10645\Desktop\MarkDown\Java.assets\image-20210222182926327.png)

   - 若要正常访问资源文件，最好全放在resources文件夹下面
   
2. 子类向上转型后

   - 调用子类父类共有的方法时，实际调用的是子类的方法；此时虚拟机进行了动态绑定。
   - 无法调用子类独有的方法，因为编译就过不去，因为类型是父类的类型，父类中没有子类独有的方法，所以无法编译。



# 提示翻译

1. **Result of 'String.replace()' is ignored** , 方法返回了一个值，而你没有把这它赋值给变量。



# 问题及解决方式

1. Maven父子项目，子项目POM文件中没有Parent结点，自己手动写上去就行了

   ``` xml
     <parent>
       <groupId>com.zcr</groupId>
       <artifactId>javaweb-02-servlet</artifactId>
       <version>1.0-SNAPSHOT</version>
     </parent>
   ```

2. idea中启动tomcat控制台乱码，将tomcat的conf/logging.properties文件中的所有UTF-8全部改为GBK即可

3. idea创建maven普通项目后，通过项目右键add Framework support中选择javaweb框架创建的maven java web的应用，在编译的时候不会编译source目录下的java文件，导致web请求无法处理，控制台提示找不到类，目前解决方案为不适用此方法创建web项目

