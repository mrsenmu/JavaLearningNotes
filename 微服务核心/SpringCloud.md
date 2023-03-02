# 一、版本说明

## 1、版本号命名规则

采用伦敦地铁站命名，简写为A-Z依次类推迭代版本。如Angel、Brixton等，重要发行版"service releases"，简写SR，如Greenwich.SR2, 为G版第二个发行版。

## 2、版本选择

### Ⅰ SpringBoot版本选择

https://github.com/spring-projects/spring-boot/releases/

### Ⅱ SpringCloud版本选择

https://spring.io/projects/spring-cloud

### Ⅲ 详细版本依赖

https://start.spring.io/actuator/info

# 二、相关组件的的停更/升级/替换

## 1、服务注册中心

### Ⅰ Eureka

### Ⅱ Zookeeper

### Ⅲ Consul

### Ⅳ Nacos

## 2、服务调用

### Ⅰ Ribbon

### Ⅱ LoadBalancer

### Ⅲ Feign

### Ⅳ OpenFeign

## 3、服务降级

### Ⅰ HyStrix

### Ⅱ resilience4j

### Ⅲ sentienl

## 4、服务网关

### Ⅰ Zuul

### Ⅱ Zuul2

### Ⅲ gateway

## 5、服务配置

### Ⅰ Config

### Ⅱ Nacos

## 6、服务总线

### Ⅰ Bus

### Ⅱ Nacos

# 三、微服务架构编码构建(IDEA)

约定 > 配置 > 编码

## 1、微服务cloud整体聚合父工程Project

### Ⅰ 新建Maven工程项目

选择maven-archetype，**org.apache.maven.archetypes:maven-archetype-site**。

Maven不同archetype介绍：[Maven 的41种骨架功能介绍 ](https://www.cnblogs.com/iusmile/archive/2012/11/14/2770118.html)

### Ⅱ 字符编码

Settings -> Editor ->**File Encodings** -> Global Encoding、Project Encoding、Properties Files设置为UTF-8；勾选 "Transparent native-to-ascii conversion".

### Ⅲ 注解生效激活

Settings -> Build,Execution,Deployment -> Complier -> **Annotation Processors** -> Default -> 勾选:Enable annotation processing

### Ⅳ Java编译版本选择

Settings -> Build,Execution,Deployment -> Complier -> **Java Complier**

### Ⅴ File Type过滤

过滤项目中不常用文件显示(个人习惯)

Settings -> Editor -> File Types

## 2、父工程pom文件

1. 整合：利用<dependencyManagement>标签，约定子项目 中依赖版本号(父项目只声明，不实现引入)
2. 发布：mvn install，父工程创建完成执行mvn:install将父工程发布到仓库方便子工程继承

```xml
	<properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <mysql.version>8.0.28</mysql.version>
        <druid.version>1.1.22</druid.version>
        <mybatis.spring.boot.version>2.1.3</mybatis.spring.boot.version>
        <junit.version>4.12</junit.version>
        <log4j.version>1.2.17</log4j.version>
        <lombok.version>1.16.20</lombok.version>
    </properties>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>2.2.2.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>

            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Hoxton.SR1</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>

            <dependency>
                <groupId>com.alibaba.cloud</groupId>
                <artifactId>spring-cloud-alibaba-dependencies</artifactId>
                <version>2.1.0.RELEASE</version>
            </dependency>

            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>${mysql.version}</version>
            </dependency>

            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>druid</artifactId>
                <version>${druid.version}</version>
            </dependency>

            <dependency>
                <groupId>org.mybatis.spring.boot</groupId>
                <artifactId>mybatis-spring-boot-starter</artifactId>
                <version>${mybatis.spring.boot.version}</version>
            </dependency>

            <dependency>
                <groupId>junit</groupId>
                <artifactId>junit</artifactId>
                <version>${junit.version}</version>
                <scope>test</scope>
            </dependency>

            <dependency>
                <groupId>log4j</groupId>
                <artifactId>log4j</artifactId>
                <version>${log4j.version}</version>
            </dependency>

            <dependency>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
                <version>${lombok.version}</version>
            </dependency>

        </dependencies>
    </dependencyManagement>
```

## 3、构建Rest微服务工程模块

### Ⅰ 步骤

1. 建module

2. 改子工程pom

   ```xml
   	<dependencies>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
   
           <!--用于检测系统的健康情况、当前的Beans、系统的缓存等-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-actuator</artifactId>
           </dependency>
   
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-jdbc</artifactId>
           </dependency>
   
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-test</artifactId>
               <scope>test</scope>
           </dependency>
   
           <!--DevTools通过提供自动重启和LiveReload功能-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-devtools</artifactId>
           </dependency>
   
           <dependency>
               <groupId>org.mybatis.spring.boot</groupId>
               <artifactId>mybatis-spring-boot-starter</artifactId>
           </dependency>
   
           <dependency>
               <groupId>org.projectlombok</groupId>
               <artifactId>lombok</artifactId>
               <optional>true</optional>
           </dependency>
       </dependencies>
   ```

3. 写yml配置文件

4. 主启动类

5. 业务类

### Ⅱ 热部署Devtools

spring-boot-devtools是一个为开发者服务的一个模块，其中最重要的功能就是自动应用代码更改到最新的App上面去。原理是在发现代码有更改之后，重新启动应用，但是速度比手动停止后再启动还要更快，更快指的不是节省出来的手工操作的时间。

深层原理是使用了两个ClassLoader，一个Classloader加载那些不会改变的类（第三方Jar包），另一个ClassLoader加载会更改的类，称为restart ClassLoader，这样在有代码更改的时候，原来的restart ClassLoader 被丢弃，重新创建一个restart ClassLoader，由于需要加载的类相比较少，所以实现了较快的重启时间。

### Ⅲ 工程重构

# 四、Eureka服务注册与发现



# 五、Zookeeper服务注册与发现



# 六、Consul服务注册与发现



# 七、Ribbon负载均衡服务调用



# 八、OpenFeign服务接口调用



# 九、Hystrix断路由



# 十、zuul路由网关





