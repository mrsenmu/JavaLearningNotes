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

- 后跟 **-DarchetypeCatalog=internal** 指令加快创建速度。表示命令执行时指定的archetype-catalog.xml文件获取路径，可选值为remote、internal、local。默认remote即从中央仓库获取，可能会导致卡住。internal会让它找本地仓库中的插件进行创建项目。

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
package com.atguigu.maven;
	
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



## 6、测试依赖范围

## 7、测试依赖传递性

## 8、测试依赖的排除

## 9、继承

## 10、聚合

# 四、使用Maven：IDEA环境

## 1、创建父工程

## 2、配置Maven信息

## 3、创建Java模块工程

## 4、创建Web模块工程

## 5、其他操作

# 五、其他核心概念

## 1、生命周期

## 2、插件和目标

## 3、仓库

# 六、单一架构案例

## 1、创建工程，引入依赖

## 2、搭建环境：持久化层

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
