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

```xml
<!--User getUserById(@Param("id") Integer id);-->
<select id="getUserById" resultType="User">
    select * from t_user where id = #{id}
</select>
```

## 2、查询一个list集合

```xml
<!--List<User> getAllUser();-->
<select id="getAllUser" resultType="User">
    select * from t_user
</select>
```

## 3、查询单个数据

```xml
<!--Integer getCount();-->
<select id="getCount" resultType="_int">
    select count(*) from t_user
</select>
```

## 4、查询一条数据为map集合

key为字段名，value对应值。对应查询的实体类。

```xml
<!--Map<String, Object> getUserByIdToMap(@Param("id") Integer id);-->
<select id="getUserByIdToMap" resultType="map">
    select * from t_user where id = #{id}
</select>
```

## 5、查询多条数据为map集合方式

注解@MapKey("id")，指定每条数据的key；或者用list集合接收。

```xml
<!--Map<String, Object> getAllUserToMap();-->
<select id="getAllUserToMap" resultType="map">
    select * from t_user
</select>
```

# 六、特殊SQL的执行

## 1、模糊查询

```xml
<!--List<User> getUserByLike(@Param("username") String username);-->
<select id="getUserByLike" resultType="User">
    <!--select * from t_user where username like '%${username}%'-->
    <!--select * from t_user where username like concat('%',#{username},'%')-->
    select * from t_user where username like "%"#{username}"%"
</select>
```

## 2、批量删除

```xml
<!--ids = "1,2,3"-->
<!--int deleteMore(@Param("ids") String ids);-->
<delete id="deleteMore">
    delete from t_user where id in (${ids})
</delete>
```

## 3、动态设置表名

```xml
<!--List<User> getUserByTableName(@Param("tableName") String tableName);-->
<select id="getUserByTableName" resultType="User">
    select * from ${tableName}
</select>
```

## 4、添加功能获取自增的主键

```xml
<!--
    void insertUser(User user);
    useGeneratedKeys:设置当前标签中的sql使用了自增的主键
    keyProperty:将自增的主键的值赋值给传输到映射文件中参数的某个属性
 -->
<insert id="insertUser" useGeneratedKeys="true" keyProperty="id">
    insert into t_user values(null,#{username},#{password},#{age},#{sex},#{email})
</insert>
```

# 七、自定义映射resultMap

## 1、resultMap处理字段和属性的映射关系

若字段名和实体类中的属性名不一致，则可以通过resultMap设置自定义映射。

```xml
<!--
  resultMap：设置自定义映射
  属性：
  id：表示自定义映射的唯一标识
  type：查询的数据要映射的实体类的类型
  子标签：
  id：设置主键的映射关系
  result：设置普通字段的映射关系
  association：设置多对一的映射关系
  collection：设置一对多的映射关系
  属性：
  property：设置映射关系中实体类中的属性名
  column：设置映射关系中表中的字段名
-->
<resultMap type="SysUser" id="SysUserResult">
    <id property="userId" column="user_id"/>
    <result property="deptId" column="dept_id"/>
    <result property="userName" column="user_name"/>
    <result property="nickName" column="nick_name"/>
    <result property="userType" column="user_type"/>
</resultMap>
```

> 若字段名和实体类中的属性名不一致，但是字段名符合数据库的规则（使用 _ ），实体类中的属性名符合Java的规则（使用驼峰）。此时也可通过以下两种方式处理字段名和实体类中的属性的映射关系
>
> a>可以通过为字段起别名的方式，保证和实体类中的属性名保持一致
>
> b>可以在MyBatis的核心配置文件中设置一个全局配置信息mapUnderscoreToCamelCase，可
>
> 以在查询表中数据时，自动将_类型的字段名转换为驼峰。
>
> 例如：字段名user_name，设置了mapUnderscoreToCamelCase，此时字段名就会转换为
>
> userName

## 2、多对一映射处理

根据员工id查询员工信息以及员工所对应的部门信息。

- 级联方式处理映射关系

  ```xml
      <!--处理多对一映射关系方式一：级联属性赋值-->
      <resultMap id="empAndDeptResultMapOne" type="Emp">
          <id property="eid" column="eid"></id>
          <result property="empName" column="emp_name"></result>
          <result property="age" column="age"></result>
          <result property="sex" column="sex"></result>
          <result property="email" column="email"></result>
          <result property="dept.did" column="did"></result>
          <result property="dept.deptName" column="dept_name"></result>
      </resultMap>
  ```

- 使用association处理映射关系

  ```xml
  <!--处理多对一映射关系方式二：association-->
  <resultMap id="empAndDeptResultMapTwo" type="Emp">
      <id property="eid" column="eid"></id>
      <result property="empName" column="emp_name"></result>
      <result property="age" column="age"></result>
      <result property="sex" column="sex"></result>
      <result property="email" column="email"></result>
      <!--
              association:处理多对一的映射关系
              property:需要处理多对的映射关系的属性名
              javaType:该属性的类型
          -->
      <association property="dept" javaType="Dept">
          <id property="did" column="did"></id>
          <result property="deptName" column="dept_name"></result>
      </association>
  </resultMap>
  <!--Emp getEmpAndDept(@Param("eid") Integer eid);-->
  <select id="getEmpAndDept" resultMap="empAndDeptResultMapTwo">
      select * from t_emp left join t_dept on t_emp.did = t_dept .did where t_emp.eid = #{eid}
  </select>
  ```

- **分步查询**，分两步，分别调用了两个mapper中方法，员工mapper和部门mapper。**可以延迟加载，优化查询效率**。

  1. 查询员工信息,在resultMap中引入DeptMapper中查询方法

     ```xml
     <resultMap id="empAndDeptByStepResultMap" type="Emp">
         <id property="eid" column="eid"></id>
         <result property="empName" column="emp_name"></result>
         <result property="age" column="age"></result>
         <result property="sex" column="sex"></result>
         <result property="email" column="email"></result>
         <!--
                 select:设置分步查询的sql的唯一标识（namespace.SQLId或mapper接口的全类名.方法名）
                 column:设置分布查询的条件
                 fetchType:当开启了全局的延迟加载之后，可通过此属性手动控制延迟加载的效果
                 fetchType="lazy|eager":lazy表示延迟加载，eager表示立即加载
             -->
         <association property="dept"
                      select="com.atguigu.mybatis.mapper.DeptMapper.getEmpAndDeptByStepTwo"
                      column="did"
                      fetchType="eager"></association>
     </resultMap>
     <!--Emp getEmpAndDeptByStepOne(@Param("eid") Integer eid);-->
     <select id="getEmpAndDeptByStepOne" resultMap="empAndDeptByStepResultMap">
         select * from t_emp where eid = #{eid}
     </select>
     ```

  2. DeptMapper中，根据员工所对应的部门id查询部门信息

     ```xml
     <!--Dept getEmpAndDeptByStepTwo(@Param("did") Integer did);-->
     <select id="getEmpAndDeptByStepTwo" resultType="Dept">
         select * from t_dept where did = #{did}
     </select>
     ```

## 3、一对多映射处理

根据部门id查询部门中的员工信息

- **collection**

  ```xml
  <resultMap id="deptAndEmpResultMap" type="Dept">
      <id property="did" column="did"></id>
      <result property="deptName" column="dept_name"></result>
      <!--
              collection：处理一对多的映射关系
              ofType：表示该属性所对应的集合中存储数据的类型
          -->
      <collection property="emps" ofType="Emp">
          <id property="eid" column="eid"></id>
          <result property="empName" column="emp_name"></result>
          <result property="age" column="age"></result>
          <result property="sex" column="sex"></result>
          <result property="email" column="email"></result>
      </collection>
  </resultMap>
  <!--Dept getDeptAndEmp(@Param("did") Integer did);-->
  <select id="getDeptAndEmp" resultMap="deptAndEmpResultMap">
      select * from t_dept left join t_emp on t_dept.did = t_emp.did where t_dept.did = #{did}
  </select>
  ```

- **分步查询**

  1. 查询部门信息

  ```xml
  <resultMap id="deptAndEmpByStepResultMap" type="Dept">
      <id property="did" column="did"></id>
      <result property="deptName" column="dept_name"></result>
      <collection property="emps"
                  select="com.atguigu.mybatis.mapper.EmpMapper.getDeptAndEmpByStepTwo"
                  column="did" fetchType="eager"></collection>
  </resultMap>
  <!--Dept getDeptAndEmpByStepOne(@Param("did") Integer did);-->
  <select id="getDeptAndEmpByStepOne" resultMap="deptAndEmpByStepResultMap">
      select * from t_dept where did = #{did}
  </select>
  ```

  2. 根据部门id查询部门中的员工

  ```xml
  <!--List<Emp> getDeptAndEmpByStepTwo(@Param("did") Integer did);-->
  <select id="getDeptAndEmpByStepTwo" resultType="Emp">
      select * from t_emp where did = #{did}
  </select>
  ```

> 分步查询的优点：可以实现延迟加载，但是必须在核心配置文件中设置全局配置信息：
>
> ​	lazyLoadingEnabled：延迟加载的全局开关。当开启时，所有关联对象都会延迟加载
>
> ​	aggressiveLazyLoading：当开启时，任何方法的调用都会加载该对象的所有属性。否则，每个属性会按需加载
>
> 此时就可以实现按需加载，获取的数据是什么，就只会执行相应的sql (使用到设置为延迟加载的属性时，才会去查询相应数据，如上诉情况，当调用getEmpAndDeptByStepOne时不加载dept信息，通过返回对象调用，emp.getDept()时，才查询dept)。此时可通过association和collection中的fetchType属性设置当前的分步查询是否使用延迟加载，fetchType="lazy(延迟加载)|eager(立即加载)"

# 八、动态SQL

Mybatis框架的动态SQL技术是一种根据特定条件动态拼装SQL语句的功能，它存在的意义是为了解决

拼接SQL语句字符串时的痛点问题。

## 1、if

> if标签可通过test属性的表达式进行判断，若表达式的结果为true，则标签中的内容会执行；反之标签中
>
> 的内容不会执行

```xml
<select id="getEmpByConditionOne" resultType="Emp">
    select * from t_emp where 1=1
    <if test="empName != null and empName != ''">
        emp_name = #{empName}
    </if>
    <if test="age != null and age != ''">
        and age = #{age}
    </if>
    <if test="sex != null and sex != ''">
        and sex = #{sex}
    </if>
    <if test="email != null and email != ''">
        and email = #{email}
    </if>
</select>
```

## 2、where

> where和if一般结合使用：
>
> a>若where标签中的if条件都不满足，则where标签没有任何功能，即**不会添加where关键字**
>
> b>若where标签中的if条件满足，则where标签会自动添加where关键字，并**将条件最前方多余的and去掉**
>
> 注意：where标签不能去掉条件最后多余的and

```xml
<select id="getEmpByConditionTwo" resultType="Emp">
    select * from t_emp
    <where>
        <if test="empName != null and empName != ''">
            emp_name = #{empName}
        </if>
        <if test="age != null and age != ''">
            and age = #{age}
        </if>
        <if test="sex != null and sex != ''">
            or sex = #{sex}
        </if>
        <if test="email != null and email != ''">
            and email = #{email}
        </if>
    </where>
</select>
```

## 3、trim

> trim用于去掉或添加标签中的内容
>
> 常用属性：
>
> prefix：在trim标签中的内容的前面添加某些内容
>
> prefixOverrides：在trim标签中的内容的前面去掉某些内容
>
> suffix：在trim标签中的内容的后面添加某些内容
>
> suffixOverrides：在trim标签中的内容的后面去掉某些内容

```xml
<sql id="empColumns">eid,emp_name,age,sex,email</sql>

<!--List<Emp> getEmpByCondition(Emp emp);-->
<select id="getEmpByCondition" resultType="Emp">
    select <include refid="empColumns"></include> from t_emp
    <trim prefix="where" suffixOverrides="and|or">
        <if test="empName != null and empName != ''">
            emp_name = #{empName} and
        </if>
        <if test="age != null and age != ''">
            age = #{age} or
        </if>
        <if test="sex != null and sex != ''">
            sex = #{sex} and
        </if>
        <if test="email != null and email != ''">
            email = #{email}
        </if>
    </trim>
</select>
```

## 4、choose、when、otherwise

choose、when、otherwise相当于if...else

when至少有一个，otherwise至多一个。

```xml
<!--List<Emp> getEmpByChoose(Emp emp);-->
<select id="getEmpByChoose" resultType="Emp">
    select * from t_emp
    <where>
        <choose>
            <when test="empName != null and empName != ''">
                emp_name = #{empName}
            </when>
            <when test="age != null and age != ''">
                age = #{age}
            </when>
            <when test="sex != null and sex != ''">
                sex = #{sex}
            </when>
            <when test="email != null and email != ''">
                email = #{email}
            </when>
            <otherwise>
                did = 1
            </otherwise>
        </choose>
    </where>
</select>
```

## 5、foreach

> 属性：
>
> collection：设置要循环的数组或集合
>
> item：表示集合或数组中的每一个数据
>
> separator：设置循环体之间的分隔符
>
> open：设置foreach标签中的内容的开始符
>
> close：设置foreach标签中的内容的结束符

```xml
<!--int insertMoreByList(@Param("emps") List<Emp> emps);-->
<insert id="insertMoreByList">
    insert into t_emp values
    <foreach collection="emps" item="emp" separator=",">
        (null,#{emp.empName},#{emp.age},#{emp.sex},#{emp.email},null)
    </foreach>
</insert>

<!--int deleteMoreByArray(@Param("eids") Integer[] eids);-->
<delete id="deleteMoreByArray">
    delete from t_emp where
    <foreach collection="eids" item="eid" separator="or">
        eid = #{eid}
    </foreach>
    <!--
        delete from t_emp where eid in
        <foreach collection="eids" item="eid" separator="," open="(" close=")">
            #{eid}
       	</foreach>
     -->
</delete>
```

## 6、SQL片段

sql片段，可以记录一段公共sql片段，在使用的地方通过include标签进行引入

```xml
<sql id="empColumns">eid,emp_name,age,sex,email</sql>
```

# 九、MyBatis的缓存

## 1、MyBatis的一级缓存

一级缓存是SqlSession级别的，通过同一个SqlSession查询的数据会被缓存，下次查询相同的数据，就会从缓存中直接获取，不会从数据库重新访问

 使一级缓存失效的四种情况：

1) 不同的SqlSession对应不同的一级缓存 
2) 同一个SqlSession但是查询条件不同 
3) 同一个SqlSession两次查询期间执行了任何一次增删改操作
4) 同一个SqlSession两次查询期间手动清空了缓存

## 2、MyBatis的二级缓存

二级缓存是SqlSessionFactory级别，通过同一个SqlSessionFactory创建的SqlSession查询的结果会被缓存；此后若再次执行相同的查询语句，结果就会从缓存中获取

二级缓存开启的条件： 

​	a>在核心配置文件中，设置全局配置属性cacheEnabled="true"，默认为true，不需要设置 

​	b>在映射文件中设置标签

​	c>二级缓存必须在SqlSession关闭或提交之后有效 

​	d>查询的数据所转换的实体类类型必须实现序列化的接口 

使二级缓存失效的情况： 

两次查询之间执行了任意的增删改，会使一级和二级缓存同时失效

## 3、二级缓存的相关配置

## 4、MyBatis缓存查询的顺序

## 5、整合第三方缓存EHCache

# 十、MyBatis的逆向工程

## 1、创建逆向工程的步骤

## 2、QBC查询

# 十一、分页插件

## 1、分页插件使用步骤

## 2、分页插件的使用