# 一、MyBatis简介

## 1、MyBatis历史

MyBatis最初是Apache的一个开源项目**iBatis**, 2010年6月这个项目由Apache Software Foundation迁移到了Google Code。随着开发团队转投Google Code旗下， iBatis3.x正式更名为MyBatis。代码于 2013年11月迁移到Github。

iBatis一词来源于“internet”和“abatis”的组合，是一个基于Java的持久层框架。 iBatis提供的持久层框架包括SQL Maps和Data Access Objects（DAO）。

## 2、MyBatis特性

1） MyBatis 是支持定制化 SQL、存储过程以及高级映射的优秀的持久层框架

2） MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集

3） MyBatis可以使用**简单的XML**或注解用于配置和原始映射，将接口和Java的POJO（Plain Old Java Objects，普通的Java对象）映射成数据库中的记录

4） MyBatis 是一个 半自动的ORM（Object Relation Mapping）框架

## 3、MyBatis下载

MyBatis下载地址：[https://](https://github.com/mybatis/mybatis-3)[g](https://github.com/mybatis/mybatis-3)[ithub.com/m](https://github.com/mybatis/mybatis-3)[y](https://github.com/mybatis/mybatis-3)[batis/m](https://github.com/mybatis/mybatis-3)[y](https://github.com/mybatis/mybatis-3)[batis-3](https://github.com/mybatis/mybatis-3)

## 4、和其它持久化层技术对比

- JDBC
  - SQL夹杂在Java代码中耦合度高，导致硬编码(写死)内伤。
  - 维护不易且实际开发需求中SQL有变化，平凡修改的情况多见。
  - 代码融创，开发效率低
- Hibernate和JPA
  - 操作简便，开发效率高
  - 程序中的长难复杂SQL需要绕过框架
  - 内部自动生成的SQL，不容易做特殊优化
  - 基于全映射的全自动框架，大量字段的POJO进行部分映射时比较困难
  - 反射操作太多，导致数据库性能下降
- MyBatis
  - 轻量级，性能出色
  - SQL和Java编码分开，功能边界清晰，Java代码专注业务、SQL语句专注数据
  - 开发效率稍逊于Hibernate，但是完全能够接受

# 二、搭建MyBatis

## 1、开发环境

## 2、创建maven工程

1. 打包方式jar
2. 引入依赖

## 3、创建MyBatis的核心配置文件

配置连接数据库环境**mybatis-config.xml**，主要用于配置连接数据库的环境以及MyBatis的全局配置信息。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!--
        MyBatis核心配置文件中，标签的顺序：
        properties?,settings?,typeAliases?,typeHandlers?,
        objectFactory?,objectWrapperFactory?,reflectorFactory?,
        plugins?,environments?,databaseIdProvider?,mappers?
    -->

    <!--引入properties文件-->
    <properties resource="jdbc.properties" />

    <!--设置类型别名-->
    <typeAliases>
        <!--
            typeAlias：设置某个类型的别名
            属性：
                type：设置需要设置别名的类型
                alias：设置某个类型的别名，若不设置该属性，那么该类型拥有默认的别名，即类名且不区分大小写
        -->
        <!--<typeAlias type="com.atguigu.mybatis.pojo.User"></typeAlias>-->
        <!--以包为单位，将包下所有的类型设置默认的类型别名，即类名且不区分大小写-->
        <package name="com.atguigu.mybatis.pojo"/>
    </typeAliases>

    <!--
        environments：配置多个连接数据库的环境
        属性：
            default：设置默认使用的环境的id
    -->
    <environments default="development">
        <!--
            environment：配置某个具体的环境
            属性：
                id：表示连接数据库的环境的唯一标识，不能重复
        -->
        <environment id="development">
            <!--
                transactionManager：设置事务管理方式
                属性：
                    type="JDBC|MANAGED"
                    JDBC：表示当前环境中，执行SQL时，使用的是JDBC中原生的事务管理方式，事务的提交或回滚需要手动处理
                    MANAGED：被管理，例如Spring
            -->
            <transactionManager type="JDBC"/>
            <!--
                dataSource：配置数据源
                属性：
                    type：设置数据源的类型
                    type="POOLED|UNPOOLED|JNDI"
                    POOLED：表示使用数据库连接池缓存数据库连接
                    UNPOOLED：表示不使用数据库连接池
                    JNDI：表示使用上下文中的数据源
            -->
            <dataSource type="POOLED">
                <!--设置连接数据库的驱动-->
                <property name="driver" value="${jdbc.driver}"/>
                <!--设置连接数据库的连接地址-->
                <property name="url" value="${jdbc.url}"/>
                <!--设置连接数据库的用户名-->
                <property name="username" value="${jdbc.username}"/>
                <!--设置连接数据库的密码-->
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>

        <environment id="test">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>
    <!--引入映射文件-->
    <mappers>
        <!--<mapper resource="mappers/UserMapper.xml"/>-->
        <!--
            以包为单位引入映射文件
            要求：
            1、mapper接口所在的包要和映射文件所在的包一致
            2、mapper接口要和映射文件的名字一致
        -->
        <package name="com.atguigu.mybatis.mapper"/>
    </mappers>
</configuration>
```

## 4、创建mapper接口

MyBatis中的mapper接口相当于以前的dao，区别在于mapper仅仅是接口，不需要实现类。

```java
public interface UserMapper {

    /**
     * MyBatis面向接口编程的两个一致：
     * 1、映射文件的namespace要和mapper接口的全类名保持一致
     * 2、映射文件中SQL语句的id要和mapper接口中的方法名一致
     *
     * 表--实体类--mapper接口--映射文件
     */

    /**
     * 添加用户信息
     */
    int insertUser();

    /**
     * 修改用户信息
     */
    void updateUser();

    /**
     * 删除用户信息
     */
    void deleteUser();

    /**
     * 根据id查询用户信息
     */
    User getUserById();

    /**
     * 查询所有的用户信息
     */
    List<User> getAllUser();

}
```

## 5、创建MyBatis的映射文件

相关概念：**ORM**（**O**bject **R**elationship **M**apping）对象关系映射。

1. 映射文件命名规则：实体类名+Mapper.xml。一个映射文件对应一个实体类，对应一张表操作。
2. 映射文件用于编写SQL访问以及操作表数据
3. MyBatis中可以面向接口操作数据，要保证两个一致：
   - mapper接口全类名的映射文件的命名空间(namespace)保持一致
   - mapper接口中方法的方法名和映射文件中编写SQL的标签的id属性一致

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.atguigu.mybatis.mapper.UserMapper">

    <!--int insertUser();-->
    <insert id="insertUser">
        insert into t_user values(null,'admin','123456',23,'男','12345@qq.com')
    </insert>

    <!--void updateUser();-->
    <update id="updateUser">
        update t_user set username = '张三' where id = 4
    </update>

    <!--void deleteUser();-->
    <delete id="deleteUser">
        delete from t_user where id = 5
    </delete>

    <!--User getUserById();-->
    <!--
        查询功能的标签必须设置resultType或resultMap
        resultType：设置默认的映射关系
        resultMap：设置自定义的映射关系
    -->
    <select id="getUserById" resultType="com.atguigu.mybatis.pojo.User">
        select * from t_user where id = 3
    </select>

    <!--List<User> getAllUser();-->
    <select id="getAllUser" resultType="User">
        select * from t_user
    </select>

</mapper>
```

## 6、通过junit测试功能

- SqlSession：代表java程序和数据库之间的会话。（HttpSession是Java程序和浏览器之间的会话）
- SqlSessionFactory：是 “生产” SqlSession的 “工厂” 
- 工厂模式：如果创建莫一个对象，使用的过程基本固定，那么我们就可以把创建这个对象的相关代码封装到一个 “工厂类” 中，以后都使用这个工厂类来 “生产” 我们需要的对象。 

```java
public class MyBatisTest {

    /**
     * SqlSession默认不自动提交事务，若需要自动提交事务
     * 可以使用SqlSessionFactory.openSession(true);
     */


    @Test
    public void testMyBatis() throws IOException {
        //加载核心配置文件
        InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
        //获取SqlSessionFactoryBuilder
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
        //获取sqlSessionFactory
        SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(is);
        //获取SqlSession
        SqlSession sqlSession = sqlSessionFactory.openSession(true);//true 自动提交事物
        //获取mapper接口对象
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        //测试功能
        int result = mapper.insertUser();
        //提交事务
        //sqlSession.commit();
        System.out.println("result:"+result);
    }

    @Test
    public void testCRUD() throws IOException {
        InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
        SqlSession sqlSession = sqlSessionFactory.openSession(true);
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        //mapper.updateUser();
        //mapper.deleteUser();
        /*User user = mapper.getUserById();
        System.out.println(user);*/
        List<User> list = mapper.getAllUser();
        list.forEach(user -> System.out.println(user));
	}
}

```

## 7、加入log4j日志功能

1. 加入依赖

2. 配置文件log4j.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
   
   <log4j:configuration xmlns:log4j="http://jakarta.apache.org/log4j/">
   
       <appender name="STDOUT" class="org.apache.log4j.ConsoleAppender">
           <param name="Encoding" value="UTF-8" />
           <layout class="org.apache.log4j.PatternLayout">
               <param name="ConversionPattern" value="%-5p %d{MM-dd HH:mm:ss,SSS} %m  (%F:%L) \n" />
           </layout>
       </appender>
       <logger name="java.sql">
           <level value="debug" />
       </logger>
       <logger name="org.apache.ibatis">
           <level value="info" />
       </logger>
       <root>
           <level value="debug" />
           <appender-ref ref="STDOUT" />
       </root>
   </log4j:configuration>
   ```

   > ## 日志的级别
   >
   > FATAL(致命) > ERROR(错误) > WARM(警告) > INFO(信息) > DEBUG(调试)
   >
   > 从左到右打印的内容越来越详细

# 三、MyBatis的增删改查

> 注意：
>
> 1、查询的标签select必须设置属性resultType或resultMap 用于设置实体类和数据库表的映射关系
>
> ​	resultType 自动映射，用于属性名和表中字段名一致的情况
>
> ​	resultMap 自定义映射，用于一对多或多对一或字段名和属性名不一致的情况
>
> 2、当查询的数据为多条时，不能使用实体类作为返回值，只能使用集合，否则会抛出异常TooManyResultsException，但是若查询的数据只有一条，可以使用实体类或集合作为返回值。

# 四、MyBatis获取参数值的两种方式（重点）

MyBatis获取参数值的两种方式：**${}**和**#{}**

${}的本质就是字符串拼接，#{}的本质就是占位符赋值

${}使用字符串拼接的方式拼接sql，若为字符串类型或日期类型的字段进行赋值时，需要**手动加单引号**；但是#{}使用占位符赋值的方式拼接sql，此时为字符串类型或日期类型的字段进行赋值时，可以**自动添加单引号**

## 1、单个字面量类型的参数

若mapper接口中的方法参数为单个的字面量类型

此时可以使用${}和#{}以任意的名称获取参数的值，注意${}需要手动加单引号

## 2、多个字面量类型的参数

若mapper接口中的方法参数为多个时

此时MyBatis会自动将这些参数放在一个map集合中，以arg0,arg1...为键，以参数为值或以

param1,param2...为键，以参数为值；因此只需要通过${}和#{}访问map集合的键就可以获取相对应的值，注意${}需要手动加单引号

```xml
<!--User checkLogin(String username, String password);-->
<select id="checkLogin" resultType="User">
    <!--select * from t_user where username = #{arg0} and password = #{arg1}-->
    select * from t_user where username = '${param1}' and password = '${param2}'
</select>
```

## 3、map集合类型的参数

若mapper接口中的方法需要的参数为多个时，此时可以手动创建map集合，将这些数据放在map中

只需要通过${}和#{}访问map集合的键就可以获取相对应的值，注意${}需要手动加单引号

```xml
<!--User checkLoginByMap(Map<String, Object> map);-->
<select id="checkLoginByMap" resultType="User">
    select * from t_user where username = #{username} and password = #{password}
</select>
```

## 4、实体类类型的参数

若mapper接口中的方法参数为实体类对象时

此时可以使用${}和#{}，通过访问实体类对象中的属性名获取属性值，注意${}需要手动加单引号

```xml
<!--int insertUser(User user);-->
<insert id="insertUser">
    insert into t_user values(null,#{username},#{password},#{age},#{sex},#{email})
</insert>
```

## 5、使用@Param标识参数

可以通过@Param注解标识mapper接口中的方法参数

此时，会将这些参数放在map集合中，以@Param注解的value属性值为键，以参数为值；以 param1,param2...为键，以参数为值；只需要通过${}和#{}访问map集合的键就可以获取相对应的值，注意${}需要手动加单引号

```xml
<!--User checkLoginByParam(@Param("username") String username, @Param("password") String password);-->
<select id="checkLoginByParam" resultType="User">
    select * from t_user where username = #{username} and password = #{password}
</select>
```

# 五、MyBatis的各种查询功能

## 1、查询一个实体类对象

## 2、查询一个list集合

## 3、查询单个数据

## 4、查询一条数据为map集合

## 5、查询多条数据为map集合方式

# 六、特殊SQL的执行

## 1、模糊查询

## 2、批量删除

## 3、动态设置表名

## 4、添加功能获取自增的主键

# 七、自定义映射resultMap

## 1、resultMap处理字段和属性的映射关系

## 2、多对一映射处理

## 3、一对多映射处理

# 八、动态SQL

## 1、if

## 2、where

## 3、trim

## 4、choose、when、otherwise

## 5、foreach

## 6、SQL片段

# 九、MyBatis的缓存

## 1、MyBatis的一级缓存

## 2、MyBatis的二级缓存

## 3、二级缓存的相关配置

## 4、MyBatis缓存查询的顺序

## 5、整合第三方缓存EHCache

# 十、MyBatis的逆向工程

## 1、创建逆向工程的步骤

## 2、QBC查询

# 十一、分页插件

## 1、分页插件使用步骤

## 2、分页插件的使用