# 一、SpringBoot2核心技术

## 1、SpringBoot2基础入门

### Ⅰ Spring 能做什么

- web开发
- 数据访问
- 安全控制
- 分布式
- 消息服务
- 移动开发
- 批处理
- ......

### Ⅱ 什么是SpringBoot

**a> SpringBoot优点**

- 创建独立的Spring应用
- 内嵌web服务器
- 自动starter依赖，简化构建配置
- 自动配置Spring以及第三方功能
- 提供生产级别的监控、健康检查及外部化配置
- 无代码生成、无需编写XML

> TIP
>
> SpringBoot是整合Spring技术栈的一站式框架
>
> SpringBoot是简化Spring技术栈的快速开发脚手架



**b> SpringBoot缺点**

- 迭代快，需要时刻关注变化
- 封装太深，内部原理复杂，不容易精通



**c> 时代背景**

对于微服务、分布式、云原生的应用

### Ⅲ 创建SpringBoot2项目

**a> 系统要求**

- java 8 & 兼容java14
- Maven 3.3+

**b> 创建maven项目**

**c> 引入依赖**

```xml
	<parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.4.RELEASE</version>
    </parent>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
```

**d> 创建主程序**

```java
/**
 * 主程序类
 * @SpringBootApplication：这是一个SpringBoot应用
 */
@SpringBootApplication
public class MainApplication {

    public static void main(String[] args) {
        SpringApplication.run(MainApplication.class,args);
    }
}
```

**e> 编写业务**


```java
@RequestMapping("/hello")
public String handle01(){
    return "Hello, Spring Boot 2!";
}}
```

**f> 测试**

直接运行main方法

**g> 简化配置**

```xml
server.port=8888
```

**h> 简化部署**

```xml
 <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```

把项目打成jar包，直接在目标服务器执行即可。

```shell
D:\spring-boot-hello-world\target>java -jar spring-boot-hello-world-1.0-SNAPSHOT.jar
```

注意点：

- 取消掉cmd的快速编辑模式

### Ⅳ 了解自动配置原理

**a> 依赖管理**

- 父项目做依赖管理

  ```xml
  <!--依赖管理-->
  <parent>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-parent</artifactId>
          <version>2.3.4.RELEASE</version>
  </parent>
  
  
  <!--他的父项目-->
   <parent>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-dependencies</artifactId>
      <version>2.3.4.RELEASE</version>
    </parent>
  
  <!--父项目几乎声明了所有开发中常用的依赖的版本号,自动版本仲裁机制-->
  ```

- 开发导入starter场景启动器

  - 官方启动器：spring-boot-starter-*，[SpringBoot所有支持的场景](https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-starter)
  - 第三方启动器：*-spring-boot-starter
  - 所有启动器pom引入的基本依赖

  ```xml
  <!-- spring-boot-starter-web-2.3.4.RELEASE.pom -->
  
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
    <version>2.3.4.RELEASE</version>
    <scope>compile</scope>
  </dependency>
  ```

- 无需关注版本号，自动版本仲裁

  1、引入依赖默认都可以不写版本
  2、引入非版本仲裁的jar，要写版本号。

- 可以修改默认版本号

  ```xml
  <!--
  1、查看spring-boot-dependencies里面规定当前依赖的版本 用的 key。
  2、在当前项目里面重写配置
  -->
      <properties>
          <mysql.version>5.1.43</mysql.version>
      </properties>
  ```



**b> 自动配置**

- 自动配好Tomcat

  - 引入Tomcat依赖
  - 自动配置Tomcat

- 自动配好SpringMVC

  - 引入SpringMVC全套组件
  - 自动配好SpringMVC常用组件（功能）

- 自动配好Web常见功能，如：字符编码问题等

- 默认包结构

  - **主程序所在包**及其下面的所有**子包**里面的组件都会被默认扫描进来

  - 可通过配置覆盖默认

    ```java
    @SpringBootApplication(scanBasePackages="com.xxx.xxx")
    等同于
    @SpringBootConfiguration
    @EnableAutoConfiguration
    @ComponentScan("com.xxx.xxx")
    3个合用
    ```

- 各种配置拥有默认值

  - 默认配置最终都是映射到某个类上，如：MultipartProperties
  - 配置文件的值最终会绑定每个类上，这个类会在容器中创建对象

- 按需加载所有自动配置项

  - 非常多starter
  - 引入哪些场景这个场景的自动配置才会开启
  - Springboot所有的自动配置功能都在**spring-boot-autoconfigure**包里面

**c> 容器功能**

- @Configuration: 标注配置类。

  - Full 和 Lite 模式配置方式
    - 配置类 组件之间无依赖关系用Lite模式加速容器启动过程，减少判断
    - 配置类 组件之间有依赖关系，方法会被调用，得到之前单实例组件，用Full模式

  ```java
  // Configuration使用示例
  /**
   * 1、配置类里面使用@Bean标注在方法上给容器注册组件，默认也是单实例的
   * 2、配置类本身也是组件
   * 3、proxyBeanMethods：代理bean的方法
   *      Full(proxyBeanMethods = true)、【保证每个@Bean方法被调用多少次返回的组件都是单实例的】
   *      Lite(proxyBeanMethods = false)【每个@Bean方法被调用多少次返回的组件都是新创建的】
   *      组件依赖必须使用Full模式默认。其他默认是否Lite模式
   */
  @Configuration(proxyBeanMethods = true) //告诉SpringBoot这是一个配置类 == 配置文件
  public class MyConfig {
  
      /**
       * Full:外部无论对配置类中的这个组件注册方法调用多少次获取的都是之前注册容器中的单实例对象
       * @return
       */
      @Bean //给容器中添加组件。以方法名作为组件的id。返回类型就是组件类型。返回的值，就是组件在容器中的实例
      public User getUser(){
          return new User(1, "senmu", 18, getDept());
      }
  
      @Bean
      public Dept getDept(){
          return new Dept(1, "研发部");
      }
  }
  ```

- @Import: 标注配置类、方法。

  ```java
  @Import({User.class, DBHelper.class}) // 给容器中自动创建出这两个类型的组件、默认组件的名字就是全类名
  @Configuration(proxyBeanMethods = false) 
  public class MyConfig {
  }
  ```

- @Conditional：标注配置类，表示条件装配：满足Conditional指定的条件，则进行组件注入。包含许多功能的派生注解。

- @ImportResource：用于xml原生配置文件引入。

**d> 配置绑定**

- 应用场景：使用java读取到properties文件中的内容。并将它封装到javaBean中。
- 方法：
  - @Component + @ConfigurationProperties  标注Bean类
  - @EnableConfigurationProperties标注类配置类 + @ConfigurationProperties 标注Bean类

## 2、SpringBoot2核心功能

### Ⅰ 配置文件



### Ⅱ web开发



### Ⅲ 数据访问



### Ⅳ JUnit5单元测试



### Ⅴ生产指标监控



### Ⅵ SpringBoot核心原理解析



## 3、SpringBoot2场景整合

### Ⅰ 虚拟化技术



### Ⅱ 安全控制



### Ⅲ 缓存技术



### Ⅳ 消息中间件



### Ⅴ 分布式入门



# 二、SpringBoot2响应式编程

## 1、响应式编程基础

### Ⅰ 响应式编程模型



### Ⅱ 使用Reactor开发



## 2、Webflux开发web应用

### Ⅰ Webflux构建RESTful应用



### Ⅱ 函数时构建RESTfu应用



### Ⅲ RestTemplate与WebClient



## 3、响应式访问持久层

### Ⅰ 响应式访问MySQL



### Ⅱ 响应式访问Redis



## 4、响应式安全开发

### Ⅰ Spring Security Reactive构建安全服务



## 5、响应式原理

### Ⅰ 几种IO模型



### Ⅱ Netty-Reactor



### Ⅲ 数据流处理原理



