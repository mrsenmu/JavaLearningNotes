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

Settings -> **File Encodings** -> Global Encoding、Project Encoding、Properties Files设置为UTF-8；勾选 "Transparent native-to-ascii conversion".

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



## 3、构建Rest微服务工程模块

### Ⅰ 步骤

1. 建module
2. 改pom
3. 写yml
4. 主启动类
5. 业务类

### Ⅱ 热部署Devtools



### Ⅲ 工程重构

# 四、Eureka服务注册与发现



# 五、Zookeeper服务注册与发现



# 六、Consul服务注册与发现



# 七、Ribbon负载均衡服务调用



# 八、OpenFeign服务接口调用



# 九、Hystrix断路由



# 十、zuul路由网关





