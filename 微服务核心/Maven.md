# 一、Maven概述

## 1、为什么学Maven

### Ⅰ Maven 作为依赖管理工具

**a> 罐包的规模**

随着我们使用越来越多的框架，或者框架封装程度越来越高，项目中使用的jar包也越来越多。项目中，一个模块里面用到上百个jar包是非常正常的。

比如下面的例子，我们只用到 SpringBoot、SpringCloud 框架中的三个功能：Nacos 服务注册发现、Web 框架环境、图模板技术 Thymeleaf。最终却导入了 106 个 jar 包。

而如果使用 Maven 来引入这些 jar 包只需要配置三个『依赖』：

```xml
    <!-- Nacos 服务注册发现启动器 -->
    <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
    </dependency>

    <!-- web启动器依赖 -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- 视图模板技术 thymeleaf -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </dependency>
```

**b> 罐包的来源**

- 这个jar包所属技术的官网。官网通常是英文界面，网站的结构又不尽相同，甚至找到下载链接还发现需要通过特殊的工具下载。
- 第三方网站提供下载。问题是不规范，在使用过程中会出现各种问题。
  - jar包的名称
  - jar包的版本
  - jar包内的具体细节
- 而使用 Maven 后，依赖对应的 jar 包能够**自动下载**，方便、快捷又规范。

**c> jar 包之间的依赖关系**

框架中使用的 jar 包，不仅数量庞大，而且彼此之间存在错综复杂的依赖关系。依赖关系的复杂程度，已经上升到了完全不能靠人力手动解决的程度。另外，jar 包之间有可能产生冲突。进一步增加了我们在 jar 包使用过程中的难度。

而实际上 jar 包之间的依赖关系是普遍存在的，如果要由程序员手动梳理无疑会增加极高的学习成本，而这些工作又对实现业务功能毫无帮助。

而使用 Maven 则几乎不需要管理这些关系，极个别的地方调整一下即可，极大的减轻了我们的工作量。

### Ⅱ Maven作为构建管理工具

**a> 透明的构建过程**

你可以不使用 Maven，但是构建必须要做。当我们使用 IDEA 进行开发时，构建是 IDEA 替我们做的。

**b> 脱离 IDE 环境仍需构建**

![图像](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/202208101409649.png)

### Ⅲ 结论

- **管理规模庞大的 jar 包，需要专门工具。**
- **脱离 IDE 环境执行构建操作，需要专门工具。**

## 2、什么是Maven

### Ⅰ 构建

Java 项目开发过程中，构建指的是使用**『原材料生产产品』**的过程。

**a> 原材料**

- Java 源代码

- 基于 HTML 的 Thymeleaf 文件

- 图片

- 配置文件

- ###### .......

**b> 产品**

- 一个可以在服务器上运行的项目

**c> 构建过程包含的主要的环节：**

- **清理(clean)**：删除上一次构建的结果，为下一次构建做好准备
- **编译(compile)**：Java 源程序编译成 *.class 字节码文件
- **测试(test)**：运行提前准备好的测试程序
- **报告**：针对刚才测试的结果生成一个全面的信息
- **打包(package)**
  - Java工程：jar包
  - Web工程：war包
- **安装(install)**：把一个 Maven 工程经过打包操作生成的 jar 包或 war 包存入 Maven 仓库
- **部署(deploy)**
  - 部署 jar 包：把一个 jar 包部署到 Nexus 私服服务器上
  - 部署 war 包：借助相关 Maven 插件（例如 cargo），将 war 包部署到 Tomcat 服务器上

### Ⅱ 依赖

如果 A 工程里面用到了 B 工程的类、接口、配置文件等等这样的资源，那么我们就可以说 A 依赖 B。例如：

- junit-4.12 依赖 hamcrest-core-1.3
- thymeleaf-3.0.12.依赖 ognl-3.1.26
  - ognl-3.1.26 依赖 javassist-3.20.0-GA
- thymeleaf-3.0.12.RELEASE 依赖 attoparser-2.0.5.RELEASE
- thymeleaf-3.0.12.RELEASE 依赖 unbescape-1.1.6.RELEASE
- thymeleaf-3.0.12.RELEASE 依赖 slf4j-api-1.7.26



依赖管理中要解决的具体问题：

- jar 包的下载：使用 Maven 之后，jar 包会从规范的远程仓库下载到本地
- jar 包之间的依赖：通过依赖的传递性自动完成
- jar 包之间的冲突：通过对依赖的配置进行调整，让某些jar包不会被导入

### Ⅲ Maven的工作机制

![img](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/202208101737363.png)

# 二、Maven核心程序解压和配置

## 1、Maven核心程序解压和配置

### Ⅰ Maven官网地址

首页：

[Maven – Welcome to Apache Maven(opens new window)](https://maven.apache.org/)

下载页面：

[Maven – Download Apache Maven](https://maven.apache.org/download.cgi)

下载链接：

![images](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/202208110951526.png)

### Ⅱ 解压Maven核心程序

核心程序压缩包：apache-maven-3.8.4-bin.zip，解压到**非中文、没有空格**的目录。

![image-20220811095323861](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/202208110953780.png)

在解压目录中，我们需要着重关注 Maven 的核心配置文件：**conf/settings.xml**

### Ⅲ 指定本地仓库

本地仓库默认值：用户家目录/.m2/repository。由于本地仓库的默认位置是在用户的家目录下，而家目录往往是在 C 盘，也就是系统盘。将来 Maven 仓库中 jar 包越来越多，仓库体积越来越大，可能会拖慢 C 盘运行速度，影响系统性能。所以建议将 Maven 的本地仓库放在其他盘符下。配置方式如下：

```xml
<!-- localRepository
| The path to the local repository maven will use to store artifacts.
|
| Default: ${user.home}/.m2/repository
<localRepository>/path/to/local/repo</localRepository>
-->
<localRepository>D:\maven-repository</localRepository>
```

本地仓库这个目录，我们手动创建一个空的目录即可。

**记住**：一定要把 localRepository 标签**从注释中拿出来**。

**注意**：本地仓库本身也需要使用一个**非中文、没有空格**的目录。

### Ⅳ 配置阿里云提供的镜像仓库

Maven 下载 jar 包默认访问境外的中央仓库，而国外网站速度很慢。改成阿里云提供的镜像仓库，**访问国内网站**，可以让 Maven 下载 jar 包的时候速度更快。配置的方式是：

**a> 将原有的例子配置注释掉**

```xml
<!-- <mirror>
  <id>maven-default-http-blocker</id>
  <mirrorOf>external:http:*</mirrorOf>
  <name>Pseudo repository to mirror external repositories initially using HTTP.</name>
  <url>http://0.0.0.0/</url>
  <blocked>true</blocked>
</mirror> -->
```

**b> 加入镜像配置**

将下面 mirror 标签整体复制到 settings.xml 文件的 mirrors 标签的内部。

```xml
	<mirror>
		<id>nexus-aliyun</id>
		<mirrorOf>central</mirrorOf>
		<name>Nexus aliyun</name>
		<url>http://maven.aliyun.com/nexus/content/groups/public</url>
	</mirror>
```

### Ⅴ 配置Maven工程的基础 **JDK** 版本

如果按照默认配置运行，Java 工程使用的默认 JDK 版本是 1.5，而我们熟悉和常用的是 JDK 1.8 版本。修改配置的方式是：将 profile 标签整个复制到 settings.xml 文件的 profiles 标签内。

```xml
	<profile>
	  <id>jdk-1.8</id>
	  <activation>
		<activeByDefault>true</activeByDefault>
		<jdk>1.8</jdk>
	  </activation>
	  <properties>
		<maven.compiler.source>1.8</maven.compiler.source>
		<maven.compiler.target>1.8</maven.compiler.target>
		<maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
	  </properties>
	</profile>
```

## 2、配置环境变量

### Ⅰ 检查JAVA_HOME配置是否正确

Maven 是一个用 Java 语言开发的程序，它必须基于 JDK 来运行，需要通过 JAVA_HOME 来找到 JDK 的安装位置。

### Ⅱ 配置Maven

1. 配置MAVEN_HOME，配置到bin目录上一级
2. 配置path，配置到bin目录
3. 验证，指令：mvn -v

# 三、使用Maven：命令行环境

## 1、根据左边创建Maven工程

### Ⅰ Maven核心概念：坐标

**a> 数学中的坐标**

 使用 x、y、z 三个**『向量』**作为空间的坐标系，可以在**『空间』**中唯一的定位到一个**『点』**。

**b> Maven中的坐标**

1. 向量说明

   使用 x、y、z 三个**『向量』**作为空间的坐标系，可以在**『空间』**中唯一的定位到一个**『点』**。

   - **groupId**：公司或组织的 id
   - **artifactId**：一个项目或者是项目中的一个模块的 id
   - **version**：版本号

2. 三个向量的取值方式

   - groupId：公司或组织域名的倒序，通常也会加上项目名称
     - 例如：com.atguigu.maven
   - artifactId：模块的名称，将来作为 Maven 工程的工程名
   - version：模块的版本号，根据自己的需要设定
     - 例如：SNAPSHOT 表示快照版本，正在迭代过程中，不稳定的版本
     - 例如：RELEASE 表示正式版本

**c> 坐标和仓库中 jar 包的存储路径之间的对应关系**

坐标：

```xml
  <groupId>javax.servlet</groupId>
  <artifactId>servlet-api</artifactId>
  <version>2.5</version>
```

上面坐标对应的 jar 包在 Maven 本地仓库中的位置：

```shell
Maven本地仓库根目录\javax\servlet\servlet-api\2.5\servlet-api-2.5.jar
```

### Ⅱ 实验操作

**a> 创建目录作为后面操作的工作空间**

例如：D:\maven-workspace\space201026

**b> 在工作空间目录下打开命令行窗口**

**c> 使用命令生成Maven工程**

![images](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/202208111054771.png)

运行 **mvn archetype:generate** 命令

- 后跟 **-DarchetypeCatalog=internal** 指令加快创建速度。表示命令执行时指定的archetype-catalog.xml文件获取路径，可选值为remote、internal、local。默认remote即从中央仓库获取，可能会导致卡住。internal会让它找本地仓库中的插件进行创建项目。设置了镜像源的话不需要。

根据下面提示操作

> Choose a number or apply filter (format: [groupId:]artifactId, case sensitive contains): 7:【直接回车，使用默认值】
>
> Define value for property 'groupId': com.atguigu.maven
>
> Define value for property 'artifactId': pro01-maven-java
>
> Define value for property 'version' 1.0-SNAPSHOT: :【直接回车，使用默认值】
>
> Define value for property 'package' com.atguigu.maven: :【直接回车，使用默认值】
>
> Confirm properties configuration: groupId: com.atguigu.maven artifactId: pro01-maven-java version: 1.0-SNAPSHOT package: com.atguigu.maven Y: :【直接回车，表示确认。如果前面有输入错误，想要重新输入，则输入 N 再回车。】

**d> 调整**

Maven 默认生成的工程，对 junit 依赖的是较低的 3.8.1 版本，我们可以改成较适合的 4.12 版本。

自动生成的 App.java 和 AppTest.java 可以删除。

```xml
<!-- 依赖信息配置 -->
<!-- dependencies复数标签：里面包含dependency单数标签 -->
<dependencies>
	<!-- dependency单数标签：配置一个具体的依赖 -->
	<dependency>
		<!-- 通过坐标来依赖其他jar包 -->
		<groupId>junit</groupId>
		<artifactId>junit</artifactId>
		<version>4.12</version>
		
		<!-- 依赖的范围 -->
		<scope>test</scope>
	</dependency>
</dependencies>
```

**e> 自动生成的 pom.xml 解读**

```xml
  <!-- 当前Maven工程的坐标 -->
  <groupId>com.atguigu.maven</groupId>
  <artifactId>pro01-maven-java</artifactId>
  <version>1.0-SNAPSHOT</version>
  
  <!-- 当前Maven工程的打包方式，可选值有下面三种： -->
  <!-- jar：表示这个工程是一个Java工程  -->
  <!-- war：表示这个工程是一个Web工程 -->
  <!-- pom：表示这个工程是“管理其他工程”的工程 -->
  <packaging>jar</packaging>

  <name>pro01-maven-java</name>
  <url>http://maven.apache.org</url>

  <properties>
	<!-- 工程构建过程中读取源码时使用的字符集 -->
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>

  <!-- 当前工程所依赖的jar包 -->
  <dependencies>
	<!-- 使用dependency配置一个具体的依赖 -->
    <dependency>
	
	  <!-- 在dependency标签内使用具体的坐标依赖我们需要的一个jar包 -->
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
	  
	  <!-- scope标签配置依赖的范围 -->
      <scope>test</scope>
    </dependency>
  </dependencies>
```

### Ⅲ Maven核心概念：POM

**a> 含义**

POM：**P**roject **O**bject **M**odel，项目对象模型。和 POM 类似的是：DOM（Document Object Model），文档对象模型。它们都是模型化思想的具体体现。

**b> 模型化思想**

POM 表示将工程抽象为一个模型，再用程序中的对象来描述这个模型。这样我们就可以用程序来管理项目了。我们在开发过程中，最基本的做法就是将现实生活中的事物抽象为模型，然后封装模型相关的数据作为一个对象，这样就可以在程序中计算与现实事物相关的数据。

**c> 对应的配置文件**

POM 理念集中体现在 Maven 工程根目录下 **pom.xml** 这个配置文件中。所以这个 pom.xml 配置文件就是 Maven 工程的核心配置文件。其实学习 Maven 就是学这个文件怎么配置，各个配置有什么用。

### Ⅳ Maven核心概念：约定的目录结构

**a> 各个目录的作用**

![./images](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/202208111445611.png)

另外还有一个 **target** 目录专门存放**构建操作输出**的结果。

**c> 约定目录结构的意义**

Maven 为了让构建过程能够尽可能自动化完成，所以必须约定目录结构的作用。例如：Maven 执行编译操作，必须先去 Java 源程序目录读取 Java 源代码，然后执行编译，最后把编译结果存放在 target 目录。

**d> 约定大于配置**

Maven 对于目录结构这个问题，没有采用配置的方式，而是基于约定。这样会让我们在开发过程中非常方便。如果每次创建 Maven 工程后，还需要针对各个目录的位置进行详细的配置，那肯定非常麻烦。

目前开发领域的技术发展趋势就是：**约定大于配置，配置大于编码**。

## 2、在Maven工程中编写代码

### Ⅰ 主体程序

![./images](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/202208111453883.png)

主体程序指的是被测试的程序，同时也是将来在项目中真正要使用的程序。

```java
package com.atguigu.maven;
	
public class Calculator {
	
	public int sum(int i, int j){
		return i + j;
	}
	
}
```

### Ⅱ 测试程序

![./images](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/202208111454367.png)

```java
package com.atguigu.maven;
	
import org.junit.Test;
import com.atguigu.maven.Calculator;
	
// 静态导入的效果是将Assert类中的静态资源导入当前类
// 这样一来，在当前类中就可以直接使用Assert类中的静态资源，不需要写类名
import static org.junit.Assert.*;
	
public class CalculatorTest{
	
	@Test
	public void testSum(){
		
		// 1.创建Calculator对象
		Calculator calculator = new Calculator();
		
		// 2.调用Calculator对象的方法，获取到程序运行实际的结果
		int actualResult = calculator.sum(5, 3);
		
		// 3.声明一个变量，表示程序运行期待的结果
		int expectedResult = 8;
		
		// 4.使用断言来判断实际结果和期待结果是否一致
		// 如果一致：测试通过，不会抛出异常
		// 如果不一致：抛出异常，测试失败
		assertEquals(expectedResult, actualResult);
		
	}
	
}
```

## 3、执行Maven的构建命令

### Ⅰ 要求

运行 Maven 中和构建操作相关的命令时，必须进入到 pom.xml 所在的目录。如果没有在 pom.xml 所在的目录运行 Maven 的构建命令，那么会看到下面的错误信息：

```java
The goal you specified requires a project to execute but there is no POM in this directory
```

> tip
>
> mvn -v 命令和构建操作无关，只要正确配置了 PATH，在任何目录下执行都可以。而构建相关的命令要在 pom.xml 所在目录下运行——操作哪个工程，就进入这个工程的 pom.xml 目录。

### Ⅱ 清理操作

mvn clean

效果：删除 target 目录

### Ⅲ 编译操作

主程序编译：mvn compile

测试程序编译：mvn test-compile

主体程序编译结果存放的目录：target/classes

测试程序编译结果存放的目录：target/test-classes

### Ⅳ 测试操作

mvn test

测试的报告存放的目录：target/surefire-reports

### Ⅴ 打包操作

mvn package

打包的结果——jar 包，存放的目录：target

### Ⅵ 安装操作

mvn install

```log
[INFO] Installing D:\maven-workspace\space201026\pro01-maven-java\target\pro01-maven-java-1.0-SNAPSHOT.jar to D:\maven-rep1026\com\atguigu\maven\pro01-maven-java\1.0-SNAPSHOT\pro01-maven-java-1.0-SNAPSHOT.jar
[INFO] Installing D:\maven-workspace\space201026\pro01-maven-java\pom.xml to D:\maven-rep1026\com\atguigu\maven\pro01-maven-java\1.0-SNAPSHOT\pro01-maven-java-1.0-SNAPSHOT.pom
```

安装的效果是将本地构建过程中生成的 jar 包存入 Maven 本地仓库。这个 jar 包在 Maven 仓库中的路径是根据它的坐标生成的。

坐标信息如下：

```xml
  <groupId>com.atguigu.maven</groupId>
  <artifactId>pro01-maven-java</artifactId>
  <version>1.0-SNAPSHOT</version>
```

在 Maven 仓库中生成的路径如下：

```log
D:\maven-rep1026\com\atguigu\maven\pro01-maven-java\1.0-SNAPSHOT\pro01-maven-java-1.0-SNAPSHOT.jar
```

另外，安装操作还会将 pom.xml 文件转换为 XXX.pom 文件一起存入本地仓库。所以我们在 Maven 的本地仓库中想看一个 jar 包原始的 pom.xml 文件时，查看对应 XXX.pom 文件即可，它们是名字发生了改变，本质上是同一个文件。

## 4、创建Maven版的Web工程

### Ⅰ 说明

使用 mvn archetype:generate 命令生成 Web 工程时，需要使用一个专门的 archetype。这个专门生成 Web 工程骨架的 archetype 可以参照官网看到它的用法：

![./images](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/202208111520766.png)

参数 archetypeGroupId、archetypeArtifactId、archetypeVersion 用来指定现在使用的 maven-archetype-webapp 的坐标。

### Ⅱ 操作

注意：如果在上一个工程的目录下执行 mvn archetype:generate 命令，那么 Maven 会报错：不能在一个非 pom 的工程下再创建其他工程。所以不要再刚才创建的工程里再创建新的工程，**请回到工作空间根目录**来操作。

然后运行生成工程的命令：

```bash
mvn archetype:generate -DarchetypeGroupId=org.apache.maven.archetypes -DarchetypeArtifactId=maven-archetype-webapp -DarchetypeVersion=1.4
```

> tip
>
> Define value for property 'groupId': com.atguigu.maven 
>
> Define value for property 'artifactId': pro02-maven-web 
>
> Define value for property 'version' 1.0-SNAPSHOT: :【直接回车，使用默认值】
>
> Define value for property 'package' com.atguigu.maven: :【直接回车，使用默认值】
>
>  Confirm properties configuration: 
>
> groupId: com.atguigu.maven 
>
> artifactId: pro02-maven-web 
>
> version: 1.0-SNAPSHOT package: com.atguigu.maven 
>
> Y: :【直接回车，表示确认】

### Ⅲ 生成的pom.xml

确认打包的方式是war包形式

```xml
<packaging>war</packaging>
```

### Ⅳ 生成的Web工程的目录结构

![image-20220811161716750](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/202208111617706.png)

webapp 目录下有 index.jsp

WEB-INF 目录下有 web.xml

### Ⅴ 创建Servlet

**a> 在 main 目录下创建 java 目录** 

**b> 在java目录下创建Servlet类所在的包的目录**

- com.xxx.maven

**c> 在包下创建Servlet类**

```java
package com.senmu.maven;
	
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.ServletException;
import java.io.IOException;
	
public class HelloServlet extends HttpServlet{
	
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		response.getWriter().write("hello maven web");
		
	}
	
}
```

**d> 在 web.xml 中注册 Servlet**

```xml
  <servlet>
	<servlet-name>helloServlet</servlet-name>
	<servlet-class>com.atguigu.maven.HelloServlet</servlet-class>
  </servlet>
  <servlet-mapping>
	<servlet-name>helloServlet</servlet-name>
	<url-pattern>/helloServlet</url-pattern>
  </servlet-mapping>
```

### Ⅵ 在index.jsp页面编写超链接

```html
<html>
<body>
<h2>Hello World!</h2>
<a href="helloServlet">Access Servlet</a>
</body>
</html>
```

> tip
>
> JSP全称是 Java Server Page，和 Thymeleaf 一样，是服务器端页面渲染技术。这里我们不必关心 JSP 语法细节，编写一个超链接标签即可。

### Ⅶ 编译

此时直接执行 mvn compile 命令出错：

> DANGER
>
> 程序包 javax.servlet.http 不存在
>
> 程序包 javax.servlet 不存在
>
> 找不到符号
>
> 符号: 类 HttpServlet
>
> ……

上面的错误信息说明：我们的 Web 工程用到了 HttpServlet 这个类，而 HttpServlet 这个类属于 servlet-api.jar 这个 jar 包。此时我们说，Web 工程需要依赖 servlet-api.jar 包。

![./images](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/202208111627327.png)

### Ⅷ 配置对servlet-api.jar包的依赖

对于不知道详细信息的依赖可以到https://mvnrepository.com/网站查询。使用关键词搜索，然后在搜索结果列表中选择适合的使用。

![./images](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/202208111629910.png)

比如，我们找到的 servlet-api 的依赖信息：

```xml
<!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.1.0</version>
    <scope>provided</scope>
</dependency>
```

这样就可以把上面的信息加入 pom.xml。重新执行 mvn compile 命令。

### Ⅸ 将Web工程打包为war包

运行 mvn package 命令，生成 war 包的位置如下图所示：

![image-20220811163444641](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/202208111634905.png)

### Ⅹ 将war包部署道Tomcat上运行

c将 war 包复制到 Tomcat/webapps 目录下

![image-20220811163614894](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/202208111636070.png)

启动 Tomcat：

![image-20220811163656183](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/202208111636576.png)

![image-20220811163827182](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/202208111638688.png)

通过浏览器尝试访问：http://localhost:8080/pro02-maven-web/index.jsp

## 5、让Web工程依赖Java工程

### Ⅰ 观念

明确一个意识：从来只有 Web 工程依赖 Java 工程，没有反过来 Java 工程依赖 Web 工程。本质上来说，Web 工程依赖的 Java 工程其实就是 Web 工程里导入的 jar 包。最终 Java 工程会变成 jar 包，放在 Web 工程的 WEB-INF/lib 目录下。

### Ⅱ 操作

在 maven-web 工程的 pom.xml 中，找到 dependencies 标签，在 dependencies 标签中做如下配置：

```xml
<!-- 配置对Java工程pro01-maven-java的依赖 -->
<!-- 具体的配置方式：在dependency标签内使用坐标实现依赖 -->
<dependency>
	<groupId>com.senmu.maven</groupId>
	<artifactId>pro01-maven-java</artifactId>
	<version>1.0-SNAPSHOT</version>
</dependency>
```

### Ⅲ 在Web工程中，编写测试代码

**a> 补充创建目录**

pro02-maven-web**\src\test\java\com\senmu\maven**

**b> 确认Web工程依赖了junit**

```xml
  <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
    </dependency>
```

**c> 创建测试类**

把 Java 工程的 CalculatorTest.java 类复制到 pro02-maven-wb**\src\test\java\com\senmu\maven** 目录下

### Ⅳ 执行Maven命令

**a> 测试命令**

mvn test

说明：测试操作中会提前自动执行编译操作，测试成功就说明编译也是成功的。

**b> 打包命令**

mvn package

![./images](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/202208121535348.png)

通过查看 war 包内的结构，我们看到被 Web 工程依赖的 Java 工程确实是会变成 Web 工程的 WEB-INF/lib 目录下的 jar 包。

![image-20220812153622389](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/202208121536560.png)

**c> 查看当前 Web 工程所依赖的 jar 包的列表**

mvn dependency:list

```bash
[INFO] The following files have been resolved:
[INFO] org.hamcrest:hamcrest-core:jar:1.3:test
[INFO] javax.servlet:javax.servlet-api:jar:3.1.0:provided
[INFO] com.senmu.maven:pro01-maven-java:jar:1.0-SNAPSHOT:compile
[INFO] junit:junit:jar:4.12:test
```

说明：javax.servlet:javax.servlet-api:jar:3.1.0:provided 格式显示的是一个 jar 包的坐标信息。**格式**是：

```
groupId:artifactId:打包方式:version:依赖的范围
```

这样的格式虽然和我们 XML 配置文件中坐标的格式不同，但是本质上还是坐标信息，大家需要能够认识这样的格式，将来从 Maven 命令的日志或错误信息中看到这样格式的信息，就能够识别出来这是坐标。进而根据坐标到Maven 仓库找到对应的jar包，用这样的方式解决我们遇到的报错的情况。

**d> 以树形结构查看当前 Web 工程的依赖信息**

mvn dependency:tree

```bash
[INFO] com.senmu.maven:pro02-maven-web:war:1.0-SNAPSHOT
[INFO] +- junit:junit:jar:4.11:test
[INFO] |  \- org.hamcrest:hamcrest-core:jar:1.3:test
[INFO] +- javax.servlet:javax.servlet-api:jar:3.1.0:provided
[INFO] \- com.senmu.maven:pro01-maven-java:jar:1.0-SNAPSHOT:compile
```

我们在 pom.xml 中并没有依赖 hamcrest-core，但是它却被加入了我们依赖的列表。原因是：junit 依赖了hamcrest-core，然后基于依赖的传递性，hamcrest-core 被传递到我们的工程了。

## 6、测试依赖范围

### Ⅰ 依赖范围

标签的位置：dependencies/dependency/**scope**

标签的可选值：**compile**/**test**/**provided**/system/runtime/**import**

**a> compile和test对比**

|         | main目录（空间） | test目录（空间） | 开发过程（时间） | 部署到服务器（时间） |
| ------- | ---------------- | ---------------- | ---------------- | -------------------- |
| compile | 有效             | 有效             | 有效             | 有效                 |
| test    | 无效             | 有效             | 有效             | 无效                 |

**b> compile 和 provided 对比**

|          | main目录（空间） | test目录（空间） | 开发过程（时间） | 部署到服务器（时间） |
| -------- | ---------------- | ---------------- | ---------------- | -------------------- |
| compile  | 有效             | 有效             | 有效             | 有效                 |
| provided | 有效             | 有效             | 有效             | 无效                 |

**c> 结论**

compile：通常使用的第三方框架的 jar 包这样在项目实际运行时真正要用到的 jar 包都是以 compile 范围进行依赖的。比如 SSM 框架所需jar包。

test：测试过程中使用的 jar 包，以 test 范围依赖进来。比如 junit。

provided：在开发过程中需要用到的“服务器上的 jar 包”通常以 provided 范围依赖进来。比如 servlet-api、jsp-api。而这个范围的 jar 包之所以不参与部署、不放进 war 包，就是避免和服务器上已有的同类 jar 包产生冲突，同时减轻服务器的负担。说白了就是：“**服务器上已经有了，你就别带啦！**”

### Ⅱ 测试

**a> 验证 compile 范围对 main 目录有效**

> tip
>
> main目录下的类：HelloServlet 使用compile范围导入的依赖：pro01-senmu-maven
>
> 验证：使用compile范围导入的依赖对main目录下的类来说是有效的
>
> 有效：HelloServlet 能够使用 pro01-senmu-maven 工程中的 Calculator 类
>
> 验证方式：在 HelloServlet 类中导入 Calculator 类，然后编译就说明有效。

**b> 验证test范围对main目录无效**

测试方式：在主体程序中导入org.junit.Test这个注解，然后执行编译。

具体操作：在pro01-maven-java\src\main\java\com\atguigu\maven目录下修改Calculator.java

```java
package com.atguigu.maven;

import org.junit.Test;

public class Calculator {
	
	public int sum(int i, int j){
		return i + j;
	}
	
}
```

执行Maven编译命令：

```java
[ERROR] /D:/maven-workspace/space201026/pro01-maven-java/src/main/java/com/senmu/maven/Calculator.java:[3,17] 程序包org.junit不存在
```

**c> 验证test和provided范围不参与服务器部署**

其实就是验证：通过compile范围依赖的jar包会放入war包，通过test范围依赖的jar包不会放入war包。

**d> 验证provided范围对测试程序有效**

测试方式是在pro02-maven-web的测试程序中加入servlet-api.jar包中的类。

修改：**pro02-maven-web**\src\\**test**\java\com\senmu\maven\\**CalculatorTest.java**

```java
package com.senmu.maven;

import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.ServletException;

import org.junit.Test;
import com.senmu.maven.Calculator;

// 静态导入的效果是将Assert类中的静态资源导入当前类
// 这样一来，在当前类中就可以直接使用Assert类中的静态资源，不需要写类名
import static org.junit.Assert.*;

public class CalculatorTest{
	
	@Test
	public void testSum(){
		
		// 1.创建Calculator对象
		Calculator calculator = new Calculator();
		
		// 2.调用Calculator对象的方法，获取到程序运行实际的结果
		int actualResult = calculator.sum(5, 3);
		
		// 3.声明一个变量，表示程序运行期待的结果
		int expectedResult = 8;
		
		// 4.使用断言来判断实际结果和期待结果是否一致
		// 如果一致：测试通过，不会抛出异常
		// 如果不一致：抛出异常，测试失败
		assertEquals(expectedResult, actualResult);
		
	}
	
}
```

然后运行Maven的编译命令：mvn compile

然后看到编译成功。

## 7、测试依赖传递性

### Ⅰ 依赖的传递性

**a> 概念**

A 依赖 B，B 依赖 C，那么在 A 没有配置对 C 的依赖的情况下，A 里面能不能直接使用 C？

**b> 传递的原则**

在 A 依赖 B，B 依赖 C 的前提下，C 是否能够传递到 A，取决于 B 依赖 C 时使用的依赖范围。

- B 依赖 C 时使用 compile 范围：可以传递
- B 依赖 C 时使用 test 或 provided 范围：不能传递，所以需要这样的 jar 包时，就必须在需要的地方明确配置依赖才可以。

### Ⅱ 使用compile范围依赖spring-core

测试方式：让 pro01-maven-java 工程依赖 spring-core

具体操作：编辑 pro01-maven-java 工程根目录下 pom.xml

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->
<dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-core</artifactId>
	<version>4.0.0.RELEASE</version>
</dependency>
```

使用 mvn dependency:tree 命令查看效果：

> tip
>
> [INFO] com.senmu.maven:pro01-maven-java:jar:1.0-SNAPSHOT
> [INFO] +- junit:junit:jar:4.12:test
> [INFO] | \- org.hamcrest:hamcrest-core:jar:1.3:test
> [INFO] \- org.springframework:spring-core:jar:4.0.0.RELEASE:compile
> [INFO] \- commons-logging:commons-logging:jar:1.1.1:compile

还可以在 Web 工程中，使用 mvn dependency:tree 命令查看效果（需要重新将 pro01-maven-java 安装到仓库）：

> tip
>
> [INFO] com.senmu.maven:pro02-maven-web:war:1.0-SNAPSHOT
> [INFO] +- junit:junit:jar:4.12:test
> [INFO] | \- org.hamcrest:hamcrest-core:jar:1.3:test
> [INFO] +- javax.servlet:javax.servlet-api:jar:3.1.0:provided
> [INFO] \- com.senmu.maven:pro01-maven-java:jar:1.0-SNAPSHOT:compile
> [INFO] \- org.springframework:spring-core:jar:4.0.0.RELEASE:compile
> [INFO] \- commons-logging:commons-logging:jar:1.1.1:compile

### Ⅲ 验证test和provided范围不能传递

从上面的例子已经能够看到，pro01-maven-java 依赖了 junit，但是在 pro02-maven-web 工程中查看依赖树的时候并没有看到 junit。

要验证 provided 范围不能传递，可以在 pro01-maven-java 工程中加入 servlet-api 的依赖。

```xml
<dependency>
	<groupId>javax.servlet</groupId>
	<artifactId>javax.servlet-api</artifactId>
	<version>3.1.0</version>
	<scope>provided</scope>
</dependency>
```

效果还是和之前一样,没有scope为provided的javax.servlet-api：

> tip
>
> [INFO] com.senmu.maven:pro02-maven-web:war:1.0-SNAPSHOT
> [INFO] +- junit:junit:jar:4.12:test
> [INFO] |\ \- org.hamcrest:hamcrest-core:jar:1.3:test
> [INFO] +- javax.servlet:javax.servlet-api:jar:3.1.0:provided
> [INFO]\ \- com.senmu.maven:pro01-maven-java:jar:1.0-SNAPSHOT:compile
> [INFO]\ \- org.springframework:spring-core:jar:4.0.0.RELEASE:compile
> [INFO]\ \- commons-logging:commons-logging:jar:1.1.1:compile

## 8、测试依赖的排除

### Ⅰ 概念

当 A 依赖 B，B 依赖 C 而且 C 可以传递到 A 的时候，A 不想要 C，需要在 A 里面把 C 排除掉。而往往这种情况都是为了避免 jar 包之间的冲突。

![./images](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/202208121631831.png)

所以配置依赖的排除其实就是阻止某些 jar 包的传递。因为这样的 jar 包传递过来会和其他 jar 包冲突。

### Ⅱ 配置方式

```xml
<dependency>
	<groupId>com.senmu.maven</groupId>
	<artifactId>pro01-maven-java</artifactId>
	<version>1.0-SNAPSHOT</version>
	<scope>compile</scope>
	<!-- 使用excludes标签配置依赖的排除	-->
	<exclusions>
		<!-- 在exclude标签中配置一个具体的排除 -->
		<exclusion>
			<!-- 指定要排除的依赖的坐标（不需要写version） -->
			<groupId>commons-logging</groupId>
			<artifactId>commons-logging</artifactId>
		</exclusion>
	</exclusions>
</dependency>
```

### Ⅲ 测试

测试的方式：在 pro02-maven-web 工程中配置对 commons-logging 的排除

```xml
<dependency>
	<groupId>com.senmu.maven</groupId>
	<artifactId>pro01-maven-java</artifactId>
	<version>1.0-SNAPSHOT</version>
	<scope>compile</scope>
	<!-- 使用excludes标签配置依赖的排除	-->
	<exclusions>
		<!-- 在exclude标签中配置一个具体的排除 -->
		<exclusion>
			<!-- 指定要排除的依赖的坐标（不需要写version） -->
			<groupId>commons-logging</groupId>
			<artifactId>commons-logging</artifactId>
		</exclusion>
	</exclusions>
</dependency>
```

运行 mvn dependency:tree 命令查看效果：

> tip
>
> [INFO] com.atguigu.maven:pro02-maven-web:war:1.0-SNAPSHOT
> [INFO] +- junit:junit:jar:4.12:test
> [INFO] |\ \- org.hamcrest:hamcrest-core:jar:1.3:test
> [INFO] +- javax.servlet:javax.servlet-api:jar:3.1.0:provided
> [INFO] \\- com.senmu.maven:pro01-maven-java:jar:1.0-SNAPSHOT:compile
> [INFO] \\- org.springframework:spring-core:jar:4.0.0.RELEASE:compile

发现在 spring-core 下面就没有 commons-logging 了。

## 9、继承

### Ⅰ 概念

Maven工程之间，A 工程继承 B 工程

- B 工程：父工程
- A 工程：子工程

本质上是 A 工程的 pom.xml 中的配置继承了 B 工程中 pom.xml 的配置。

### Ⅱ 作用

在父工程中统一管理项目中的依赖信息，具体来说是管理依赖信息的版本。

它的背景是：

- 对一个比较大型的项目进行了模块拆分。
- 一个 project 下面，创建了很多个 module。
- 每一个 module 都需要配置自己的依赖信息。

它背后的需求是：

- 在每一个 module 中各自维护各自的依赖信息很容易发生出入，不易统一管理。
- 使用同一个框架内的不同 jar 包，它们应该是同一个版本，所以整个项目中使用的框架版本需要统一。
- 使用框架时所需要的 jar 包组合（或者说依赖信息组合）需要经过长期摸索和反复调试，最终确定一个可用组合。这个耗费很大精力总结出来的方案不应该在新的项目中重新摸索。

通过在父工程中为整个项目维护依赖信息的组合既**保证了整个项目使用规范、准确的 jar 包**；又能够将**以往的经验沉淀**下来，节约时间和精力。

### Ⅲ 举例

在一个工程中依赖多个 Spring 的 jar 包

> tip
>
> [INFO] +- org.springframework:**spring-core**:jar:**4.0.0**.RELEASE:compile
> [INFO] |\ \- commons-logging:commons-logging:jar:1.1.1:compile
> [INFO] +- org.springframework:**spring-beans**:jar:**4.0.0**.RELEASE:compile
> [INFO] +- org.springframework:**spring-context**:jar:**4.0.0**.RELEASE:compile
> [INFO] +- org.springframework:**spring-expression**:jar:4.0.0.RELEASE:compile
> [INFO] +- org.springframework:**spring-aop**:jar:**4.0.0**.RELEASE:compile
> [INFO] | \\- aopalliance:aopalliance:jar:1.0:compile

使用 Spring 时要求所有 Spring 自己的 jar 包版本必须一致。为了能够对这些 jar 包的版本进行统一管理，我们使用继承这个机制，将所有版本信息统一在父工程中进行管理。

### Ⅳ 操作

**a> 创建父工程**

创建的过程和前面创建 pro01-maven-java 一样。

工程名称：pro03-maven-parent

工程创建好之后，要修改它的打包方式：

```xml
  <groupId>com.senmu.maven</groupId>
  <artifactId>pro03-maven-parent</artifactId>
  <version>1.0-SNAPSHOT</version>

  <!-- 当前工程作为父工程，它要去管理子工程，所以打包方式必须是 pom -->
  <packaging>pom</packaging>
```

只有打包方式为 pom 的 Maven 工程能够管理其他 Maven 工程。打包方式为 pom 的 Maven 工程中不写业务代码，它是专门管理其他 Maven 工程的工程。

**b> 创建模块工程**

模块工程类似于 IDEA 中的 module，所以需要**进入 pro03-maven-parent 工程的根目录**，然后运行 mvn archetype:generate 命令来创建模块工程。

假设，我们创建三个模块工程：

![下载 (8)](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/202208121709875.png)

**c> 查看被添加新内容的父工程 pom.xml**

下面 modules 和 module 标签是聚合功能的配置

```xml
<modules>  
	<module>pro04-maven-module</module>
	<module>pro05-maven-module</module>
	<module>pro06-maven-module</module>
</modules>
```

**d> 解读子工程的pom.xml**

```xml
<!-- 使用parent标签指定当前工程的父工程 -->
<parent>
	<!-- 父工程的坐标 -->
	<groupId>com.senmu.maven</groupId>
	<artifactId>pro03-maven-parent</artifactId>
	<version>1.0-SNAPSHOT</version>
</parent>

<!-- 子工程的坐标 -->
<!-- 如果子工程坐标中的groupId和version与父工程一致，那么可以省略 -->
<!-- <groupId>com.senmu.maven</groupId> -->
<artifactId>pro04-maven-module</artifactId>
<!-- <version>1.0-SNAPSHOT</version> -->
```

**e> 在父工程中配置依赖的统一管理**

```xml
<!-- 使用dependencyManagement标签配置对依赖的管理 -->
<!-- 被管理的依赖并没有真正被引入到工程 -->
<dependencyManagement>
	<dependencies>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-core</artifactId>
			<version>4.0.0.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-beans</artifactId>
			<version>4.0.0.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>4.0.0.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-expression</artifactId>
			<version>4.0.0.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-aop</artifactId>
			<version>4.0.0.RELEASE</version>
		</dependency>
	</dependencies>
</dependencyManagement>
```

**f> 子工程中引用那些被父工程管理的依赖**

**关键点：省略版本号**

```xml
<!-- 子工程引用父工程中的依赖信息时，可以把版本号去掉。	-->
<!-- 把版本号去掉就表示子工程中这个依赖的版本由父工程决定。 -->
<!-- 具体来说是由父工程的dependencyManagement来决定。 -->
<dependencies>
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-core</artifactId>
	</dependency>
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-beans</artifactId>
	</dependency>
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-context</artifactId>
	</dependency>
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-expression</artifactId>
	</dependency>
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-aop</artifactId>
	</dependency>
</dependencies>
```

**g> 在父工程中升级依赖信息的版本**

```xml
……
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-beans</artifactId>
        <version>4.1.4.RELEASE</version>
    </dependency>
……
```

然后在子工程中运行mvn dependency:list，效果如下：

```bash
[INFO] org.springframework:spring-aop:jar:4.1.4.RELEASE:compile
[INFO] org.springframework:spring-core:jar:4.1.4.RELEASE:compile
[INFO] org.springframework:spring-context:jar:4.1.4.RELEASE:compile
[INFO] org.springframework:spring-beans:jar:4.1.4.RELEASE:compile
[INFO] org.springframework:spring-expression:jar:4.1.4.RELEASE:compile
```

**h> 在父工程中声明自定义属性**

```xml
<!-- 通过自定义属性，统一指定Spring的版本 -->
<properties>
	<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
	
	<!-- 自定义标签，维护Spring版本数据 -->
	<senmu.spring.version>4.3.6.RELEASE</senmu.spring.version>
</properties>
```

在需要的地方使用${}的形式来引用自定义的属性名：

```xml
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>${senmu.spring.version}</version>
    </dependency>
```

真正实现“一处修改，处处生效”。

### Ⅴ 实际意义

![./images](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/202208121722333.jpeg)

编写一套符合要求、开发各种功能都能正常工作的依赖组合并不容易。如果公司里已经有人总结了成熟的组合方案，那么再开发新项目时，如果不使用原有的积累，而是重新摸索，会浪费大量的时间。为了提高效率，我们可以使用工程继承的机制，让成熟的依赖组合方案能够保留下来。

如上图所示，公司级的父工程中管理的就是成熟的依赖组合方案，各个新项目、子系统各取所需即可。

## 10、聚合

### Ⅰ 聚合本身的含义

部分组成整体

### Ⅱ Maven中的聚合

使用一个“总工程”将各个“模块工程”汇集起来，作为一个整体对应完整的项目。

- 项目：整体
- 模块：部分

> TIP
>
> 概念的对应关系：
>
> 从继承关系角度来看：
>
> - 父工程
> - 子工程
>
> 从聚合关系角度来看：
>
> - 总工程
> - 模块工程

### Ⅲ 好处

- 一键执行 Maven 命令：很多构建命令都可以在“总工程”中一键执行。

  以 mvn install 命令为例：Maven 要求有父工程时先安装父工程；有依赖的工程时，先安装被依赖的工程。我们自己考虑这些规则会很麻烦。但是工程聚合之后，在总工程执行 mvn install 可以一键完成安装，而且会自动按照正确的顺序执行。

- 配置聚合之后，各个模块工程会在总工程中展示一个列表，让项目中的各个模块一目了然。

### Ⅳ 聚合的配置

在总工程中配置 modules 即可：

```xml
	<modules>  
		<module>pro04-maven-module</module>
		<module>pro05-maven-module</module>
		<module>pro06-maven-module</module>
	</modules>
```

### Ⅴ 依赖循环问题

如果 A 工程依赖 B 工程，B 工程依赖 C 工程，C 工程又反过来依赖 A 工程，那么在执行构建操作时会报下面的错误：

> DANGER
>
> [ERROR] [ERROR] The projects in the reactor contain a cyclic reference:

这个错误的含义是：循环引用。

# 四、使用Maven：IDEA环境

## 1、创建父工程

### Ⅰ 创建现目

当前IDEA版本2020.3

idea创建新项目，选择Maven项目 ip01-maven-parent

### Ⅱ 开启自动导入

自动导入**一定要开启**，因为 Project、Module 新创建或 pom.xml 每次修改时都应该让 IDEA 重新加载 Maven 信息。这对 Maven 目录结构认定、Java 源程序编译、依赖 jar 包的导入都有非常关键的影响。

方法（适用于IDEA2020.3版本以前）：Settings -> Build, Execution,Deployment -> Maven -> Importing -> Import Maven projects automatically 选项

在2020.3版本中，没有该选项，但更改pom文件后，可通过点击右上角maven刷新图标更新依赖。

## 2、配置Maven信息

每次创建 Project 后都需要设置 Maven 家目录位置，否则 IDEA 将使用内置的 Maven 核心程序（不稳定）并使用默认的本地仓库位置。这样一来，我们在命令行操作过程中已下载好的 jar 包就白下载了，默认的本地仓库通常在 C 盘，还影响系统运行。

配置之后，IDEA 会根据我们在这里指定的 Maven 家目录自动识别到我们在 settings.xml 配置文件中指定的本地仓库。

![image-20220815104929813](C:\Users\senmu\AppData\Roaming\Typora\typora-user-images\image-20220815104929813.png)

## 3、创建Java模块工程

在创建的父工程ip01-maven-parent中，通过创建新module选项创建module01-java，选择当前工程的父工程（Parent：ip01-maven-parent）。

![image-20220815112124949](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/202208151121429.png)

## 4、创建Web模块工程

### Ⅰ 创建模块

按照前面的同样操作创建模块，**此时**这个模块其实还是一个**Java模块**。

### Ⅱ 修改打包方式

Web 模块将来打包当然应该是 **war** 包。

```xml
<packaging>war</packaging>
```

### Ⅲ Web设定

首先打开项目结构菜单(Project Structure)，然后到 Facets 下查看 IDEA 是否已经帮我们自动生成了 Web 设定。正常来说只要我们确实设置了打包方式为 war，那么 IDEA 就会自动生成 Web 设定。IDEA2018及之前需要自己新建。

![image-20220815113004092](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/202208151130675.png)

### Ⅳ 借助IDEA生成web.xml

![image-20220815113416594](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/202208151134865.png)

### Ⅴ 设置Web资源的根目录

结合 Maven 的目录结构，Web 资源的根目录需要设置为 src/main/webapp 目录。

![image-20220815113647958](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/202208151136870.png)

## 5、其他操作

### Ⅰ 在IDEA中执行Maven命令

**a> 直接执行**

<img src="https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/202208151140111.png" alt="image-20220815114017878" style="zoom:80%;" />

**b> 手动执行**

<img src="https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/202208151143457.png" alt="image-20220815114353901" style="zoom:80%;" />

### Ⅱ 在IDEA中查看某个模块的依赖信息

Dependencies下

### Ⅲ 工程导入

Maven工程除了自己创建的，还有很多情况是别人创建的。而为了参与开发或者是参考学习，我们都需要导入到 IDEA 中。下面我们分几种不同情况来说明：

**a> 来自版本控制系统(Version Control)**

目前我们通常使用的都是 Git（本地库） + 码云（远程库）的版本控制系统。

**b> 来自工程目录**

直接使用 IDEA 打开工程目录即可。

- **工程压缩包：**假设别人发给我们一个 Maven 工程的 zip 压缩包：maven-rest-demo.zip。从码云或GitHub上也可以以 ZIP 压缩格式对项目代码打包下载。
- **解压：**如果你的所有 IDEA 工程有一个专门的目录来存放，而不是散落各处，那么首先我们就把 ZIP 包解压到这个指定目录中。
- **打开：**只要我们确认在解压目录下可以直接看到 pom.xml，那就能证明这个解压目录就是我们的工程目录。那么接下来让 IDEA 打开这个目录就可以了。
- **设置 Maven 核心程序位置：**打开一个新的 Maven 工程，和新创建一个 Maven 工程是一样的，此时 IDEA 的 settings 配置中关于 Maven 仍然是默认值。所以我们还是需要像新建 Maven 工程那样，指定一下 Maven 核心程序位置（Maven home directory：..\\..\\apache-maven-3.x.x）。

### Ⅳ 模块导入

1. 复制源工程目录下的源模块到目标工程下

2. IDEA导入复制的文件

   ![image-20220815141017938](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/202208151410750.png)

3. 修改 pom.xml，如坐标等。

4. （web模块）删除多余的、不正确的 web.xml 设置。

# 五、其他核心概念

## 1、生命周期

### Ⅰ 作用

为了让构建过程自动化完成，Maven 设定了三个生命周期，生命周期中的每一个环节对应构建过程中的一个操作。

### Ⅱ 三个生命周期

| 生命周期名称 | 作用         | 各个环节                                                     |
| ------------ | ------------ | ------------------------------------------------------------ |
| Clean        | 清理操作相关 | pre-clean<br />clean<br />post-clean                         |
| Site         | 生成站点相关 | pre-site <br />site <br />post-site <br />deploy-site        |
| Default      | 主要构建过程 | validate<br />generate-sources<br />process-sources<br />generate-resources<br />process-resources 复制并处理资源文件，至目标目录，准备打包。 <br />compile 编译项目 main 目录下的源代码。 <br />process-classes <br />generate-test-sources <br />process-test-sources <br />generate-test-resources <br />process-test-resources 复制并处理资源文件，至目标测试目录。 <br />test-compile 编译测试源代码。 <br />process-test-classes <br />test 使用合适的单元测试框架运行测试。这些测试代码不会被打包或部署。 <br />prepare-package <br />package 接受编译好的代码，打包成可发布的格式，如JAR。 <br />pre-integration-test <br />integration-test <br />post-integration-test <br />verify install将包安装至本地仓库，以让其它项目依赖。 deploy将最终的包复制到远程的仓库，以让其它开发人员共享；或者部署到服务器上运行（需借助插件，例如：cargo）。 |

### Ⅲ 特点

- 前面三个生命周期彼此是独立的。
- 在任何一个生命周期内部，执行任何一个具体环节的操作，都是从本周期最初的位置开始执行，直到指定的地方。

Maven 之所以这么设计其实就是为了提高构建过程的自动化程度：让使用者只关心最终要干的即可，过程中的各个环节是自动执行的。

## 2、插件和目标

### Ⅰ 插件

Maven 的核心程序仅仅负责宏观调度，不做具体工作。具体工作都是由 Maven 插件完成的。例如：编译就是由 maven-compiler-plugin-x.x.jar 插件来执行的。

### Ⅱ 目标

一个插件可以对应多个目标，而每一个目标都和生命周期中的某一个环节对应。

Default 生命周期中有 compile 和 test-compile 两个和编译相关的环节，这两个环节对应 compile 和 test-compile 两个目标，而这两个目标都是由 maven-compiler-plugin-x.x.jar 插件来执行的。

## 3、仓库

- 本地仓库：在当前电脑上，为电脑上所有 Maven 工程服务
- 远程仓库：需要联网
  - 局域网：我们自己搭建的 Maven 私服，例如使用 Nexus 技术。
  - Internet
    - 中央仓库
    - 镜像仓库：内容和中央仓库保持一致，但是能够分担中央仓库的负载，同时让用户能够就近访问提高下载速度，例如：Nexus aliyun

建议：不要中央仓库和阿里云镜像混用，否则 jar 包来源不纯，彼此冲突。

专门搜索 Maven 依赖信息的网站：https://mvnrepository.com/

# 六、单一架构案例

## 1、创建工程，引入依赖

### Ⅰ 架构

**a> 架构的概念**

『架构』其实就是『项目的**结构**』，只是因为架构是一个更大的词，通常用来形容比较大规模事物的结构。

**b> 单一架构**

单一架构也叫『all-in-one』结构，就是所有代码、配置文件、各种资源都在同一个工程。

- 一个项目包含一个工程
- 导出一个 war 包
- 放在一个 Tomcat 上运行

### Ⅱ 创建工程

### Ⅲ 引入依赖

**a> 搜索依赖信息的网站**

- 网站：https://mvnrepository.com/
- 如何选择：
  - 确定技术选型：确定我们项目中要使用哪些技术
  - 到 mvnrepository 网站搜索具体技术对应的具体依赖信息
  - 确定这个技术使用哪个版本的依赖，根据所用技术是否要求进行选择，如无硬性要求，则选择较高版本或下载量大的版本
  - 在实际使用中检验所有依赖信息是否都正常可用

> TIP
>
> 确定技术选型、组建依赖列表、项目划分模块……等等这些操作其实都属于**架构设计**的范畴。
>
> - 项目本身所属行业的基本特点
> - 项目具体的功能需求
> - 项目预计访问压力程度
> - 项目预计将来需要扩展的功能
> - 设计项目总体的体系结构

**b> 持久化层所需依赖**

- mysql:mysql-connector-java:8.0.23**(需要与数据库版本配对)**
- com.alibaba:druid:1.2.8
- commons-dbutils:commons-dbutils:1.6

**c> 表述层所需依赖**

- javax.servlet:javax.servlet-api:3.1.0
- org.thymeleaf:thymeleaf:3.0.11.RELEASE

**d> 辅助功能所需依赖**

- junit:junit:4.12
- ch.qos.logback:logback-classic:1.2.3

**e> 最终完整依赖信息**

```xml
<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.23</version>
</dependency>
<!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.2.8</version>
</dependency>
<!-- https://mvnrepository.com/artifact/commons-dbutils/commons-dbutils -->
<dependency>
    <groupId>commons-dbutils</groupId>
    <artifactId>commons-dbutils</artifactId>
    <version>1.6</version>
</dependency>
<!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.1.0</version>
    <scope>provided</scope>
</dependency>
<!-- https://mvnrepository.com/artifact/org.thymeleaf/thymeleaf -->
<dependency>
    <groupId>org.thymeleaf</groupId>
    <artifactId>thymeleaf</artifactId>
    <version>3.0.11.RELEASE</version>
</dependency>
<!-- https://mvnrepository.com/artifact/junit/junit -->
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
</dependency>
<!-- https://mvnrepository.com/artifact/ch.qos.logback/logback-classic -->
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>1.2.3</version>
    <scope>test</scope>
</dependency>
```

### Ⅳ 建包

| package 功能          | package 名称                   |
| --------------------- | ------------------------------ |
| 主包                  | com.senmu.maven                |
| 子包[实体类]          | com.senmu.maven.entity         |
| 子包[Servlet基类包]   | com.senmu.maven.servlet.base   |
| 子包[Servlet模块包]   | com.senmu.maven.servlet.module |
| 子包[Service接口包]   | com.senmu.maven.service.api    |
| 子包[Service实现类包] | com.senmu.maven.service.impl   |
| 子包[Dao接口包]       | com.senmu.maven.dao.api        |
| 子包[Dao实现类包]     | com.senmu.maven.dao.impl       |
| 子包[Filter]          | com.senmu.maven.filter         |
| 子包[异常类包]        | com.senmu.maven.exception      |
| 子包[工具类]          | com.senmu.maven.util           |
| 子包[测试类]          | com.senmu.maven.test           |

![image-20220815164902815](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/202208151649287.png)

## 2、搭建环境：持久化层

### Ⅰ 数据建模

**a> 物理建模**

```sql
create database db_maven_all_in_one;

use db_maven_all_in_one;

-- ----------------------------
-- 1、部门表
-- ----------------------------
drop table if exists sys_dept;
create table sys_dept (
  dept_id           bigint(20)      not null auto_increment    comment '部门id',
  parent_id         bigint(20)      default 0                  comment '父部门id',
  ancestors         varchar(50)     default ''                 comment '祖级列表',
  dept_name         varchar(30)     default ''                 comment '部门名称',
  order_num         int(4)          default 0                  comment '显示顺序',
  leader            varchar(20)     default null               comment '负责人',
  phone             varchar(11)     default null               comment '联系电话',
  email             varchar(50)     default null               comment '邮箱',
  status            char(1)         default '0'                comment '部门状态（0正常 1停用）',
  del_flag          char(1)         default '0'                comment '删除标志（0代表存在 2代表删除）',
  create_by         varchar(64)     default ''                 comment '创建者',
  create_time       datetime                                   comment '创建时间',
  primary key (dept_id)
) engine=innodb auto_increment=200 comment = '部门表';

-- ----------------------------
-- 初始化-部门表数据
-- ----------------------------
insert into sys_dept values(100,  0,   '0',          '森木有限公司',   0, '森木', '15888888888', 'ry@qq.com', '0', '0', 'admin', sysdate(), '', null);
insert into sys_dept values(101,  100, '0,100',      '研发部门', 1, '森木', '15888888888', 'ry@qq.com', '0', '0', 'admin', sysdate(), '', null);
insert into sys_dept values(102,  100, '0,100',      '测试部门', 2, '森木', '15888888888', 'ry@qq.com', '0', '0', 'admin', sysdate(), '', null);


-- ----------------------------
-- 2、用户信息表
-- ----------------------------
drop table if exists sys_user;
create table sys_user (
  user_id           bigint(20)      not null auto_increment    comment '用户ID',
  dept_id           bigint(20)      default null               comment '部门ID',
  user_name         varchar(30)     not null                   comment '用户账号',
  email             varchar(50)     default ''                 comment '用户邮箱',
  phonenumber       varchar(11)     default ''                 comment '手机号码',
  sex               char(1)         default '0'                comment '用户性别（0男 1女 2未知）',
  password          varchar(100)    default ''                 comment '密码',
  del_flag          char(1)         default '0'                comment '删除标志（0代表存在 2代表删除）',
  create_by         varchar(64)     default ''                 comment '创建者',
  create_time       datetime                                   comment '创建时间',
  remark            varchar(500)    default null               comment '备注',
  primary key (user_id)
) engine=innodb auto_increment=100 comment = '用户信息表';

-- ----------------------------
-- 初始化-用户信息表数据
-- ----------------------------
insert into sys_user values(1,  100, 'admin', '超级管理员', 'sys_user', 'admin@163.com', '15888888888', '1', '', '$2a$10$7JB720yubVSZvUI0rEqK/.VqGOZTH.ulu33dHOiBE8ByOhJIrdAu2', '0', '0', '127.0.0.1', sysdate(), 'admin', sysdate(), '', null, '管理员');
insert into sys_user values(2,  101, 'senmu', '森木', 'sys_user', 'senmu@qq.com',  '15666666666', '1', '', '$2a$10$7JB720yubVSZvUI0rEqK/.VqGOZTH.ulu33dHOiBE8ByOhJIrdAu2', '0', '0', '127.0.0.1', sysdate(), 'admin', sysdate(), '', null, '研发人员');
```

### Ⅱ 数据库连接信息

**a> SysDept实体类**

```java

```

**b> SysUser 实体类**

```java
public class SysUser{

    /**
     * 用户id
     */
    private Long userId;

    /**
     * 部门id
     */
    private Long deptId;

    /**
     * 用户账号
     */
    private String userName;

    /**
     * 用户昵称
     */
    private String nickName;

    /**
     * 用户类型（sys_user系统用户）
     */
    private String userType;

    /**
     * 用户邮箱
     */
    private String email;

    /**
     * 手机号码
     */
    private String phonenumber;

    /**
     * 用户性别（0男 1女 2未知）
     */
    private String sex;

    /**
     * 头像地址
     */
    private String avatar;

    /**
     * 密码
     */
    private String password;

    /**
     * 帐号状态（0正常 1停用）
     */
    private String status;

    /**
     * 删除标志（0代表存在 2代表删除）
     */
    private String delFlag;

    /**
     * 最后登录ip
     */
    private String loginIp;

    /**
     * 最后登录时间
     */
    private DateTime loginDate;

    /**
     * 创建者
     */
    private String createBy;

    /**
     * 创建时间
     */
    private DateTime createTime;

    /**
     * 更新者
     */
    private String updateBy;

    /**
     * 更新时间
     */
    private DateTime updateTime;

    /**
     * 备注
     */
    private String remark;

    public SysUser() {}
}
```



### Ⅲ 获取数据库连接



### Ⅳ BaseDao



### Ⅴ 子类Dao



## 3、搭建环境：事务控制



## 4、搭建环境：表述层



## 5、搭建环境：辅助功能



## 6、业务功能：登录



## 7、业务功能：显示奏折列表



## 8、业务功能：显示奏折详情



## 9、业务功能：批复奏折



## 10、业务功能：登陆检查



## 11、打包部署



# 七、SSM整合伪分布式案例

## 1、创建工程，引入依赖

## 2、 搭建环境：持久化层

## 3、 搭建环境：事务控制

## 4、 搭建环境：表述层

## 5、 搭建环境：辅助功能

## 6、 业务功能：登录

# 八、微服务架构案例

## 1、 创建工程

## 2、 父工程管理依赖

## 3、 打基础

## 4、 用户登录认证服务：提供端

## 5、 用户登录认证服务：消费端

## 6、 部署运行

# 九、POM深入与强化

## 1、 重新认识 Maven

## 2、 POM 的四个层次

## 3、 属性的声明与引用

## 4、 build 标签详解

## 5、 依赖配置补充

## 6、 Maven 自定义插件

## 7、 profile 详解

# 十、生产实践

## 1、 搭建 Maven 私服：Nexus

## 2、 jar 包冲突问题

## 3、 体系外 jar 包引入
