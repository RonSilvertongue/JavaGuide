# Mybatis（ORM，对象关系映射框架）

环境：

- JDK1.8
- Mysql 5.7.19
- Maven 3.6.2



## 1 .1如何获取Mybatis

- maven仓库

``` xml
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis</artifactId>
  <version>x.x.x</version>
</dependency>
```

## 1.2 持久化

数据持久化

## 1.3 持久层

负责数据持久化工作的代码



# 2 第一个Mybatis程序

``` xml
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.6</version>
        </dependency>
```

## 2.1 编写Mybatis的配置文件

``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <environments default="development">
    <environment id="development">
      <transactionManager type="JDBC"/>
      <dataSource type="POOLED">
        <property name="driver" value="${driver}"/>
        <property name="url" value="${url}"/>
        <property name="username" value="${username}"/>
        <property name="password" value="${password}"/>
      </dataSource>
    </environment>
  </environments>
  <mappers>
    <mapper resource="org/mybatis/example/BlogMapper.xml"/>
  </mappers>
</configuration>
```

## 2.1 编写Mybatis工具类获取sqlsession

``` java
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

//用于获得sqlsessionFactory
public class MybatisUtil {
    private static SqlSessionFactory sqlSessionFactory;
    static{
        String resource = "mybatis-config.xml";
        try {
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static SqlSession getSqlSession(){
        return sqlSessionFactory.openSession();
    }
}
```



## 2.3 编写代码

- 实体类 User

- DAO接口 UserDAO

  ``` java
  public interface UserDAO {
      List<User> getUserList();
  
      User getUserById(int id);
  
      int addUser(User user);
  
      int updateUser(User user);
  
      int deleteUser(int id);
  }
  ```

  

- 接口实现类 由UserMapper.xml实现

  ``` xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE mapper
          PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  <!--namespace绑定一个DAO或者mapper-->
  <mapper namespace="com.zcr.dao.UserDAO">
  <!--    虽然返回类型是一个List，但是这边只要写List中的类型即可,id是接口中的方法名 要一一对应-->
      <select id="getUserList" resultType="com.zcr.pojo.User">
          select * from mybatis.user;
      </select>
  
      <select id="getUserById" parameterType="int" resultType="com.zcr.pojo.User">
          select * from user where id=#{id};
      </select>
      
  <!--插入删除更新等修改数据的操作一定要commit才会生效-->
      <insert id="addUser" parameterType="com.zcr.pojo.User">
          insert into mybatis.user(id,name,pwd) values(#{id},#{name},#{pwd});
      </insert>
  
      <update id="updateUser" parameterType="com.zcr.pojo.User">
          update mybatis.user set name=#{name},pwd=#{pwd} where id=#{id};
      </update>
  
      <delete id="deleteUser" parameterType="int">
          delete from mybatis.user where id=#{id};
      </delete>
  </mapper>
  ```

  



## 2.4 Junit测试

注意点：

1. 每一个Mapper.xml都需要再配置的xml中注册

   ``` xml
       <mappers>
           <mapper resource="UserMapper.xml"/>
       </mappers>
   ```

2. resource中的文件如果不在resources文件夹下且是maven项目，需要在maven中配置编译的相关设置，不然不在resources的文件是会被忽略的

3. 如果出现了MapperRegistry错误，就是mapper.xml没有注册，看点1



## 2.5 总结

Mybatis工作路径：

1. 编写mybatis-config.xml配置文件

2. 编写XXXMapper接口

3. 编写XXXMapper.xml配置文件，其中的namespace要指向XXXMapper接口

4. 将XXXMapper.xml配置文件在mybatis-config.xml配置文件中注册

5. 使用：

   - 通过SqlSessionFactoryBuilder创建SqlSessionFactory对象

     - SqlSessionFactoryBuilder需要mybatis-config.xml配置文件作为参数

   - 再通过SqlSessionFactory对象创建SqlSession对象

   - 关于SqlSession对象

     - 此对象可以通过给定的Mapper接口的类对象创建接口的实现类，通过调用实现类来操作数据库

       ``` java
       UserDAO mapper = sqlSession.getMapper(UserDAO.class);
       ```

     - 还可以通过传递Mapper接口的全限定名来直接执行数据库操作

       ``` java
       List<User> users = sqlSession.selectList("com.zcr.dao.UserDAO.getUserList");
       ```

6. 关于各个对象的Scope

   - SqlSessionFactoryBuilder对象用完就可以销毁，Scope定在方法中
   - SqlSessionFactory对象Scope为Singleton，为每一个请求创建SqlSession对象
   - SqlSession对象Scope为方法，为每一个请求创建一个SqlSession对象

