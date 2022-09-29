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

![image-20220819143744846](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208101409649.png)

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

![image-20220819143851202](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208101737363.png)

# 二、Maven核心程序解压和配置

## 1、Maven核心程序解压和配置

### Ⅰ Maven官网地址

首页：

[Maven – Welcome to Apache Maven(opens new window)](https://maven.apache.org/)

下载页面：

[Maven – Download Apache Maven](https://maven.apache.org/download.cgi)

下载链接：

![images](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208110951526.png)

### Ⅱ 解压Maven核心程序

核心程序压缩包：apache-maven-3.8.4-bin.zip，解压到**非中文、没有空格**的目录。

![image-20220811095323861](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208110953780.png)

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

![images](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208111054771.png)

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

![./images](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208111445611.png)

另外还有一个 **target** 目录专门存放**构建操作输出**的结果。

**c> 约定目录结构的意义**

Maven 为了让构建过程能够尽可能自动化完成，所以必须约定目录结构的作用。例如：Maven 执行编译操作，必须先去 Java 源程序目录读取 Java 源代码，然后执行编译，最后把编译结果存放在 target 目录。

**d> 约定大于配置**

Maven 对于目录结构这个问题，没有采用配置的方式，而是基于约定。这样会让我们在开发过程中非常方便。如果每次创建 Maven 工程后，还需要针对各个目录的位置进行详细的配置，那肯定非常麻烦。

目前开发领域的技术发展趋势就是：**约定大于配置，配置大于编码**。

## 2、在Maven工程中编写代码

### Ⅰ 主体程序

![./images](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208111453883.png)

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

![./images](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208111454367.png)

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

> TIP
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

![./images](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208111520766.png)

参数 archetypeGroupId、archetypeArtifactId、archetypeVersion 用来指定现在使用的 maven-archetype-webapp 的坐标。

### Ⅱ 操作

注意：如果在上一个工程的目录下执行 mvn archetype:generate 命令，那么 Maven 会报错：不能在一个非 pom 的工程下再创建其他工程。所以不要再刚才创建的工程里再创建新的工程，**请回到工作空间根目录**来操作。

然后运行生成工程的命令：

```bash
mvn archetype:generate -DarchetypeGroupId=org.apache.maven.archetypes -DarchetypeArtifactId=maven-archetype-webapp -DarchetypeVersion=1.4
```

> TIP
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

![image-20220811161716750](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208111617706.png)

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

> TIP
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

![./images](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208111627327.png)

### Ⅷ 配置对servlet-api.jar包的依赖

对于不知道详细信息的依赖可以到https://mvnrepository.com/网站查询。使用关键词搜索，然后在搜索结果列表中选择适合的使用。

![./images](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208111629910.png)

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

![image-20220811163444641](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208111634905.png)

### Ⅹ 将war包部署道Tomcat上运行

c将 war 包复制到 Tomcat/webapps 目录下

![image-20220811163614894](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208111636070.png)

启动 Tomcat：

![image-20220811163656183](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208111636576.png)

![image-20220811163827182](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208111638688.png)

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

![./images](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208121535348.png)

通过查看 war 包内的结构，我们看到被 Web 工程依赖的 Java 工程确实是会变成 Web 工程的 WEB-INF/lib 目录下的 jar 包。

![image-20220812153622389](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208121536560.png)

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

> TIP
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
package com.senmu.maven;

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
- B 依赖 C 时使用 **test 或 provided** 范围：**不能传递**，所以需要这样的 jar 包时，就必须在需要的地方明确配置依赖才可以。

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

> TIP
>
> [INFO] com.senmu.maven:pro01-maven-java:jar:1.0-SNAPSHOT
> [INFO] +- junit:junit:jar:4.12:test
> [INFO] | \- org.hamcrest:hamcrest-core:jar:1.3:test
> [INFO] \- org.springframework:spring-core:jar:4.0.0.RELEASE:compile
> [INFO] \- commons-logging:commons-logging:jar:1.1.1:compile

还可以在 Web 工程中，使用 mvn dependency:tree 命令查看效果（需要重新将 pro01-maven-java 安装到仓库）：

> TIP
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

> TIP
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

![./images](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208121631831.png)

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

> TIP
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

> TIP
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

![下载 (8)](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208121709875.png)

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

![./images](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208121722333.jpeg)

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

![image-20220815112124949](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208151121429.png)

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

![image-20220815113004092](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208151130675.png)

### Ⅳ 借助IDEA生成web.xml

![image-20220815113416594](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208151134865.png)

### Ⅴ 设置Web资源的根目录

结合 Maven 的目录结构，Web 资源的根目录需要设置为 src/main/webapp 目录。

![image-20220815113647958](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208151136870.png)

## 5、其他操作

### Ⅰ 在IDEA中执行Maven命令

**a> 直接执行**

<img src="https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208151140111.png" alt="image-20220815114017878" style="zoom:80%;" />

**b> 手动执行**

<img src="https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208151143457.png" alt="image-20220815114353901" style="zoom:80%;" />

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

   ![image-20220815141017938](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208151410750.png)

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

![image-20220815164902815](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208151649287.png)

## 2、搭建环境：持久化层

### Ⅰ 数据建模

**a> 物理建模**

```sql
-- ----------------------------
-- maven学习
-- ----------------------------
drop database if exists maven;
create database maven;

use maven;

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
  phone             varchar(11)     default null               comment '联系电话',
  email             varchar(50)     default null               comment '邮箱',
  del_flag          char(1)         default '0'                comment '删除标志（0代表存在 2代表删除）',
  create_by         varchar(64)     default ''                 comment '创建者',
  create_time       datetime                                   comment '创建时间',
  primary key (dept_id)
) engine=innodb auto_increment=200 comment = '部门表';

-- ----------------------------
-- 初始化-部门表数据
-- ----------------------------
insert into sys_dept values(100,  0,   '0', '森木有限公司', 0, '15888888888', 'ry@qq.com', '0', 'admin', sysdate());
insert into sys_dept values(101,  100, '0,100', '研发部门', 1, '15888888888', 'ry@qq.com', '0', 'admin', sysdate());
insert into sys_dept values(102,  100, '0,100', '测试部门', 2, '15888888888', 'ry@qq.com', '0', 'admin', sysdate());


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
  primary key (user_id)
) engine=innodb auto_increment=100 comment = '用户信息表';

-- ----------------------------
-- 初始化-用户信息表数据
-- ----------------------------
insert into sys_user values(1,  100, 'admin', 'admin@163.com', '15888888888', '2', '$2a$10$7JB720yubVSZvUI0rEqK/.VqGOZTH.ulu33dHOiBE8ByOhJIrdAu2', '0', 'admin',sysdate());
insert into sys_user values(2,  101, 'senmu', 'senmu@qq.com',  '15666666666', '0', '$2a$10$7JB720yubVSZvUI0rEqK/.VqGOZTH.ulu33dHOiBE8ByOhJIrdAu2', '0','admin', sysdate());
```

**b> 逻辑建模**

sql在线转java  [JAVA代码生成平台 (bejson.com)](http://java.bejson.com/generator/)

1. **SysDept**实体类

```java
/**
 * @description sys_dept
 * @author senmu
 * @date 2022-08-16
 */
@Data
public class SysDept implements Serializable {

    private static final long serialVersionUID = 1L;
    
    /**
    * 部门id
    */
    private Long deptId;

    /**
    * 父部门id
    */
    private Long parentId;

    /**
    * 祖级列表
    */
    private String ancestors;

    /**
    * 部门名称
    */
    private String deptName;

    /**
    * 显示顺序
    */
    private Integer orderNum;

    /**
    * 联系电话
    */
    private String phone;

    /**
    * 邮箱
    */
    private String email;

    /**
    * 删除标志（0代表存在 2代表删除）
    */
    private String delFlag;

    /**
    * 创建者
    */
    private String createBy;

    /**
    * 创建时间
    */
    private LocalDateTime createTime;

    public SysDept() {}
}
```

2. **SysUser** 实体类

```java
/**
 * @description sys_user
 * @author senmu
 * @date 2022-08-16
 */
@Data
public class SysUser implements Serializable {

    private static final long serialVersionUID = 1L;

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
    * 密码
    */
    private String password;

    /**
    * 删除标志（0代表存在 2代表删除）
    */
    private String delFlag;

    /**
    * 创建者
    */
    private String createBy;

    /**
    * 创建时间
    */
    private LocalDateTime createTime;

    public SysUser() {}
}
```

### Ⅱ 数据库连接信息

说明：这是我们第一次用到 Maven 约定目录结构中的 resources 目录，这个目录存放各种配置文件。

在**resources**目录下创建**jdbc.properties**

```properties
driverClassName=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://localhost:3306/maven
username=root
password=123456
initialSize=10
maxActive=20
maxWait=10000
```

### Ⅳ 获取数据库连接

**a> 创建 JDBCUtils工具类**

路径：src/main/java/com/senmu/maven/util/JDBCUtils.java

```java
/**
 * 功能1：从数据源获取数据库连接
 * 功能2：从数据库获取到数据库连接后，绑定到本地线程（借助 ThreadLocal）
 * 功能3：释放线程时和本地线程解除绑定
 */
public class JDBCUtils {

}
```

**b> 创建javax.sql.DataSource 对象**

```java
// 将数据源对象设置为静态属性，保证大对象的单一实例
private static DataSource dataSource;

static {

    // 1.创建一个用于存储外部属性文件信息的Properties对象
    Properties properties = new Properties();

    // 2.使用当前类的类加载器加载外部属性文件：jdbc.properties
    InputStream inputStream = JDBCUtils.class.getClassLoader().getResourceAsStream("jdbc.properties");

    try {

        // 3.将外部属性文件jdbc.properties中的数据加载到properties对象中
        properties.load(inputStream);

        // 4.创建数据源对象
        dataSource = DruidDataSourceFactory.createDataSource(properties);

    } catch (Exception e) {
        e.printStackTrace();
        
        // 为了避免在真正抛出异常后，catch 块捕获到异常从而掩盖问题，
        // 这里将所捕获到的异常封装为运行时异常继续抛出
        throw new RuntimeException(e);
    }

}
```

**c> 创建 ThreadLocal 对象**

1. **提出需求**

   - **在一个方法内控制事务**

     如果在每一个 Service 方法中都写下面代码，那么代码重复性就太高了：

     ```java
     try{
     
     	// 1、获取数据库连接
     	// 重要：要保证参与事务的多个数据库操作（SQL 语句）使用的是同一个数据库连接
     	Connection conn = JDBCUtils.getConnection();
     	
     	// 2、核心操作
     	// ...
     	
     	// 3、核心操作成功结束，可以提交事务
     	conn.commit();
     
     }catch(Exception e){
     
     	// 4、核心操作抛出异常，必须回滚事务
     	conn.rollBack();
     
     }finally{
     
     	// 5、释放数据库连接
     	JDBCUtils.releaseConnection(conn);
     	
     }
     ```

   - **将重复代码抽取到 Filter**

     所谓『当前请求覆盖的 Servlet 方法、Service 方法、Dao 方法』其实就是 chain.doFilter(request, response) **间接调用**的方法。

     ```java
     public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain){
     
     	try{
     
     		// 1、获取数据库连接
     		// 重要：要保证参与事务的多个数据库操作（SQL 语句）使用的是同一个数据库连接
     		Connection conn = JDBCUtils.getConnection();
             
             // 重要操作：关闭自动提交功能
             connection.setAutoCommit(false);
     		
     		// 2、核心操作：通过 chain 对象放行当前请求
     		// 这样就可以保证当前请求覆盖的 Servlet 方法、Service 方法、Dao 方法都在同一个事务中。
     		// 同时各个请求都经过这个 Filter，所以当前事务控制的代码在这里只写一遍就行了，
     		// 避免了代码的冗余。
     		chain.doFilter(request, response);
     		
     		// 3、核心操作成功结束，可以提交事务
     		conn.commit();
     
     	}catch(Exception e){
     
     		// 4、核心操作抛出异常，必须回滚事务
     		conn.rollBack();
     
     	}finally{
     
     		// 5、释放数据库连接
     		JDBCUtils.releaseConnection(conn);
     		
     	}
     
     }
     ```

     

   - **数据的跨方法传递**

     通过 JDBCUtils 工具类获取到的 Connection 对象需要传递给 Dao 方法，让事务涉及到的所有 Dao 方法用的都是同一个 Connection 对象。

     但是 Connection 对象无法通过 chain.doFilter() 方法以参数的形式传递过去。

     所以从获取到 Connection 对象到使用 Connection 对象中间隔着很多不是我们自己声明的方法——我们无法决定它们的参数。

     ![images](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208161616886.png)

2. **ThreadLocal 对象的功能**

   - 使上图中的所有方法调用的过程都在同一个线程内
   - 全类名：java.lang.ThreadLocal<T>
   - 泛型 T：要绑定到当前线程的数据的类型
   - 具体三个主要的方法：

   | 方法名       | 功能                       |
   | ------------ | -------------------------- |
   | set(T value) | 将数据绑定到当前线程       |
   | get()        | 从当前线程获取已绑定的数据 |
   | remove()     | 将数据从当前线程移除       |

3. **Java代码实现**

   ```java
   // 由于 ThreadLocal 对象需要作为绑定数据时 k-v 对中的 key，所以要保证唯一性
   // 加 static 声明为静态资源即可保证唯一性
   private static ThreadLocal<Connection> threadLocal = new ThreadLocal<>();
   ```

**d> 声明方法：获取数据库连接**

```java
/**
 * 工具方法：获取数据库连接并返回
 * @return
 */
public static Connection getConnection() {

    Connection connection = null;

    try {
        // 1、尝试从当前线程检查是否存在已经绑定的 Connection 对象
        connection = threadLocal.get();

        // 2、检查 Connection 对象是否为 null
        if (connection == null) {
            
            // 3、如果为 null，则从数据源获取数据库连接
            connection = dataSource.getConnection();

            // 4、获取到数据库连接后绑定到当前线程
            threadLocal.set(connection);
            
        }
    } catch (SQLException e) {
        e.printStackTrace();
        
        // 为了调用工具方法方便，编译时异常不往外抛
        // 为了不掩盖问题，捕获到的编译时异常封装为运行时异常抛出
        throw new RuntimeException(e);
    }

    return connection;
}
```

**e> 声明方法：释放数据库连接**

```java
/**
 * 释放数据库连接
 */
public static void releaseConnection(Connection connection) {

    if (connection != null) {

        try {
            // 在数据库连接池中将当前连接对象标记为空闲
            connection.close();

            // 将当前数据库连接从当前线程上移除
            threadLocal.remove();

        } catch (SQLException e) {
            e.printStackTrace();
            throw new RuntimeException(e);
        }

    }

}
```

**f> 初步测试**

创建测试类 src/test/java/com/senmu/maven/test01.java

```java
public class test01 {

    @Test
    public void testGetConnection() {

        Connection connection = JDBCUtils.getConnection();
        System.out.println("connection = " + connection);

        JDBCUtils.releaseConnection(connection);

    }

}
```

### Ⅳ BaseDao

**a> 创建数据库基本操作对象**

路径：src/main/java/com/senmu/maven/dao/BaseDao.java

```java
//需要声明泛型，代表实体类类型
public class BaseDao<T> {

}
```

**b> 创建QueryRunner 对象**

```java
   // org.apache.commons.dbutils 工具包提供的数据库操作对象
    private QueryRunner runner = new QueryRunner();
```

**c> 通用增删改方法**

**特别说明**：在 BaseDao 方法中获取数据库连接但是不做释放，因为我们要在控制事务的 Filter 中统一释放。

```java
/**
 * 通用的增删改方法，insert、delete、update 操作都可以用这个方法
 * @param sql 执行操作的 SQL 语句
 * @param parameters SQL 语句的参数
 * @return 受影响的行数
 */
public int update(String sql, Object ... parameters) {

    try {
        Connection connection = JDBCUtils.getConnection();

        int affectedRowNumbers = runner.update(connection, sql, parameters);
        
        return affectedRowNumbers;
    } catch (SQLException e) {
        e.printStackTrace();

        // 如果真的抛出异常，则将编译时异常封装为运行时异常抛出
        new RuntimeException(e);
        
        return 0;
    }

}
```

**d> 查询单个对象**

```java
/**
 * 查询单个对象
 * @param sql 执行查询的 SQL 语句
 * @param entityClass 实体类对应的 Class 对象
 * @param parameters 传给 SQL 语句的参数
 * @return 查询到的实体类对象
 */
public T getSingleBean(String sql, Class<T> entityClass, Object ... parameters) {

    try {
        // 获取数据库连接
        Connection connection = JDBCUtils.getConnection();

        return runner.query(connection, sql, new BeanHandler<>(entityClass), parameters);

    } catch (SQLException e) {
        e.printStackTrace();

        // 如果真的抛出异常，则将编译时异常封装为运行时异常抛出
        new RuntimeException(e);
    }

    return null;
}
```

**e> 查询多个对象**

```java
/**
 * 查询返回多个对象的方法
 * @param sql 执行查询操作的 SQL 语句
 * @param entityClass 实体类的 Class 对象
 * @param parameters SQL 语句的参数
 * @return 查询结果
 */
public List<T> getBeanList(String sql, Class<T> entityClass, Object ... parameters) {
    try {
        // 获取数据库连接
        Connection connection = JDBCUtils.getConnection();

        return runner.query(connection, sql, new BeanListHandler<>(entityClass), parameters);

    } catch (SQLException e) {
        e.printStackTrace();

        // 如果真的抛出异常，则将编译时异常封装为运行时异常抛出
        new RuntimeException(e);
    }

    return null;
}
```

**f> 测试**

test01.java

```java
    private BaseDao<SysUser> baseDao = new BaseDao<>();

    @Test
    public void testGetSingleBean() {

        //需要字段取别名与实体类字段名匹配
        String sql = "SELECT user_id userId, dept_id deptId, user_name userName, email, phonenumber, sex, password, del_flag delFlag, create_by createBy, create_time createTime" +
                " FROM sys_user WHERE user_id=?";

        SysUser user = baseDao.getSingleBean(sql, SysUser.class, 1);

        System.out.println("user = " + user);

    }

    @Test
    public void testGetBeanList() {

        String sql = "SELECT user_id userId, dept_id deptId, user_name userName, email, phonenumber, sex, password, del_flag delFlag, create_by createBy, create_time createTime" +
                " FROM sys_user";

        List<SysUser> userList = baseDao.getBeanList(sql, SysUser.class);

        for (SysUser user: userList) {
            System.out.println("user = " + user);
        }

    }

    @Test
    public void testUpdate() {

        String sql = "update sys_user set create_time=? where user_id=?";

        Date createTime = new Date();
        Integer userId = 2;

        int affectedRowNumber = baseDao.update(sql, createTime, userId);

        System.out.println("affectedRowNumber = " + affectedRowNumber);

    }
```

### Ⅴ 子类Dao

![image-20220816173758854](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208161738844.png)

## 3、搭建环境：事务控制

### Ⅰ 总体思路

![images](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208170850649.png)

### Ⅱ TransactionFilter

**a> 创建Filter**

src/main/java/com/senmu/maven/filter/TransactionFilter.java

**b> TransactionFilter 完整代码**

```java
public class TransactionFilter implements Filter {

    // 声明集合保存静态资源扩展名
    private static Set<String> staticResourceExtNameSet;

    static {
        staticResourceExtNameSet = new HashSet<>();
        staticResourceExtNameSet.add(".png");
        staticResourceExtNameSet.add(".jpg");
        staticResourceExtNameSet.add(".css");
        staticResourceExtNameSet.add(".js");
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {

        // 前置操作：排除静态资源
        HttpServletRequest request = (HttpServletRequest) servletRequest;
        String servletPath = request.getServletPath();
        if (servletPath.contains(".")) {
            String extName = servletPath.substring(servletPath.lastIndexOf("."));

            if (staticResourceExtNameSet.contains(extName)) {

                // 如果检测到当前请求确实是静态资源，则直接放行，不做事务操作
                filterChain.doFilter(servletRequest, servletResponse);

                // 当前方法立即返回
                return ;
            }

        }

        Connection connection = null;

        try{

            // 1、获取数据库连接
            connection = JDBCUtils.getConnection();

            // 重要操作：关闭自动提交功能
            connection.setAutoCommit(false);

            // 2、核心操作
            filterChain.doFilter(servletRequest, servletResponse);

            // 3、提交事务
            connection.commit();

        }catch (Exception e) {

            try {
                // 4、回滚事务
                connection.rollback();
            } catch (SQLException ex) {
                ex.printStackTrace();
            }

            // 页面显示：将这里捕获到的异常发送到指定页面显示
            // 获取异常信息
            String message = e.getMessage();

            // 将异常信息存入请求域
            request.setAttribute("systemMessage", message);

            // 将请求转发到指定页面
            request.getRequestDispatcher("/").forward(request, servletResponse);

        }finally {

            // 5、释放数据库连接
            JDBCUtils.releaseConnection(connection);

        }

    }

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {}
    @Override
    public void destroy() {}
}
```

**c> 配置web.xml**

**注意**：需要首先将当前工程改成 Web 工程。

![image-20220817091253582](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208170912168.png)

```xml
<filter>
    <filter-name>txFilter</filter-name>
    <filter-class>com.senmu.maven.filter.TransactionFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>txFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

**d> 注意点**

1. **确保异常回滚**

   在程序执行的过程中，必须让所有 catch 块都把编译时异常转换为运行时异常抛出；如果不这么做，在 TransactionFilter 中 catch 就无法捕获到底层抛出的异常，那么该回滚的时候就无法回滚。

2. **谨防数据库连接提前释放**

   由于诸多操作都是在使用同一个数据库连接，那么中间任何一个环节释放数据库连接都会导致后续操作无法正常完成。

## 4、搭建环境：表述层

### Ⅰ 视图模板技术Thymeleaf

**a> 服务器端渲染**

![images](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208170945417.png)

**b> hymeleaf 简要工作机制**

1. **初始化阶段**

   - 目标：创建 TemplateEngine 对象
   - 封装：因为对每一个请求来说，TemplateEngine 对象使用的都是同一个，所以在初始化阶段准备好

   ![image-20220817094728322](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208170947100.png)

2.  请求处理阶段

   ![images](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208170948848.png)

**c> 逻辑视图与物理视图**

假设有下列页面地址：

> /WEB-INF/pages/apple.html
> /WEB-INF/pages/banana.html
> /WEB-INF/pages/orange.html
> /WEB-INF/pages/grape.html
> /WEB-INF/pages/egg.html

这样的地址可以**直接访问**到页面本身，我们称之为：**物理视图**。而将物理视图中前面、后面的固定内容抽取出来，让每次请求指定中间变化部分即可，那么**中间变化**部分就叫：**逻辑视图**。

![images](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208171002498.png)

**d> ViewBaseServlet 完整代码**

为了简化视图页面处理过程，我们将 Thymeleaf 模板引擎的初始化和请求处理过程封装到一个 Servlet 基类中：ViewBaseServlet。以后负责具体模块业务功能的 Servlet 继承该基类即可直接使用。

**特别提醒**：这个类**不需要掌握**，因为以后都被框架封装了，我们现在只是暂时用一下。

src/main/java/com/senmu/maven/**servlet/base/ViewBaseServlet.java**

```java
public class ViewBaseServlet extends HttpServlet {

    private TemplateEngine templateEngine;

    @Override
    public void init() throws ServletException {

        // 1.获取ServletContext对象
        ServletContext servletContext = this.getServletContext();

        // 2.创建Thymeleaf解析器对象
        ServletContextTemplateResolver templateResolver = new ServletContextTemplateResolver(servletContext);

        // 3.给解析器对象设置参数
        // ①HTML是默认模式，明确设置是为了代码更容易理解
        templateResolver.setTemplateMode(TemplateMode.HTML);

        // ②设置前缀
        String viewPrefix = servletContext.getInitParameter("view-prefix");

        templateResolver.setPrefix(viewPrefix);

        // ③设置后缀
        String viewSuffix = servletContext.getInitParameter("view-suffix");

        templateResolver.setSuffix(viewSuffix);

        // ④设置缓存过期时间（毫秒）
        templateResolver.setCacheTTLMs(60000L);

        // ⑤设置是否缓存
        templateResolver.setCacheable(true);

        // ⑥设置服务器端编码方式
        templateResolver.setCharacterEncoding("utf-8");

        // 4.创建模板引擎对象
        templateEngine = new TemplateEngine();

        // 5.给模板引擎对象设置模板解析器
        templateEngine.setTemplateResolver(templateResolver);

    }

    protected void processTemplate(String templateName, HttpServletRequest req, HttpServletResponse resp) throws IOException {
        // 1.设置响应体内容类型和字符集
        resp.setContentType("text/html;charset=UTF-8");

        // 2.创建WebContext对象
        WebContext webContext = new WebContext(req, resp, getServletContext());

        // 3.处理模板数据
        templateEngine.process(templateName, webContext, resp.getWriter());
    }
}
```

**e> 声明初始化参数**

配置web.xml

```xml
<!-- 配置 Web 应用初始化参数指定视图前缀、后缀 -->
<!-- 
    物理视图举例：/WEB-INF/pages/index.html
    对应逻辑视图：index
-->
<context-param>
    <param-name>view-prefix</param-name>
    <param-value>/WEB-INF/pages/</param-value>
</context-param>
<context-param>
    <param-name>view-suffix</param-name>
    <param-value>.html</param-value>
</context-param>
```

### Ⅱ ModelBaseServlet

**a> 提出问题**

- 需求

  ![image-20220817101217514](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208171012974.png)

- HttpServlet的局限

  - doGet()方法：处理GET请求
  - doPost()方法：处理POST请求

**b> 解决方案**

- 每个请求附带一个**请求参数**，表明自己要调用的目标方法
- Servlet 根据目标方法名通过**反射**调用目标方法

**c> ModelBaseServlet完整代码**

src/main/java/com/senmu/maven/**servlet/base/ModelBaseServlet.java**

**特别提醒**：为了配合 TransactionFilter 实现事务控制，捕获的异常必须抛出。

```java
public class ModelBaseServlet extends ViewBaseServlet {
    
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        // 在doGet()方法中调用doPost()方法，这样就可以在doPost()方法中集中处理所有请求
        doPost(request, response);
    }
    
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        // 1.在所有request.getParameter()前面设置解析请求体的字符集
        request.setCharacterEncoding("UTF-8");

        // 2.从请求参数中获取method对应的数据
        String method = request.getParameter("method");

        // 3.通过反射调用method对应的方法
        // ①获取Class对象
        Class<? extends ModelBaseServlet> clazz = this.getClass();

        try {
            // ②获取method对应的Method对象
            Method methodObject = clazz.getDeclaredMethod(method, HttpServletRequest.class, HttpServletResponse.class);

            // ③打开访问权限
            methodObject.setAccessible(true);

            // ④通过Method对象调用目标方法
            methodObject.invoke(this, request, response);
        } catch (Exception e) {
            e.printStackTrace();

            // 重要提醒：为了配合 TransactionFilter 实现事务控制，捕获的异常必须抛出。
            throw new RuntimeException(e);
        }
    }

}
```

**d> 继承关系**

![images](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202208171044003.png)

## 5、搭建环境：辅助功能

### Ⅰ 常量类

src/main/java/com/senmu/maven/**util/CommConstants.java**

```java
public class CommConstants {

    public static final String LOGIN_FAILED_MESSAGE = "账号或密码错误！";

    public static final String ACCESS_DENIED_MESSAGE = "拒绝访问！";
}
```

### Ⅱ MD5加密工具方法

src/main/java/com/senmu/maven/**util/MD5Util.java**

```java
public class MD5Util {

    /**
     * 针对明文字符串执行MD5加密
     * @param source
     * @return
     */
    public static String encode(String source) {

        // 1.判断明文字符串是否有效
        if (source == null || "".equals(source)) {
            throw new RuntimeException("用于加密的明文不可为空");
        }

        // 2.声明算法名称
        String algorithm = "md5";

        // 3.获取MessageDigest对象
        MessageDigest messageDigest = null;
        try {
            messageDigest = MessageDigest.getInstance(algorithm);
        } catch (NoSuchAlgorithmException e) {
            e.printStackTrace();
        }

        // 4.获取明文字符串对应的字节数组
        byte[] input = source.getBytes();

        // 5.执行加密
        byte[] output = messageDigest.digest(input);

        // 6.创建BigInteger对象
        int signum = 1;
        BigInteger bigInteger = new BigInteger(signum, output);

        // 7.按照16进制将bigInteger的值转换为字符串
        int radix = 16;
        String encoded = bigInteger.toString(radix).toUpperCase();

        return encoded;
    }

}
```

### Ⅲ 日志配置文件

src/main/**resources/logback.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration debug="true">
    <!-- 指定日志输出的位置 -->
    <appender name="STDOUT"
              class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <!-- 日志输出的格式 -->
            <!-- 按照顺序分别是：时间、日志级别、线程名称、打印日志的类、日志主体内容、换行 -->
            <pattern>[%d{HH:mm:ss.SSS}] [%-5level] [%thread] [%logger] [%msg]%n</pattern>
            <charset>UTF-8</charset>
        </encoder>
    </appender>

    <!-- 设置全局日志级别。日志级别按顺序分别是：DEBUG、INFO、WARN、ERROR -->
    <!-- 指定任何一个日志级别都只打印当前级别和后面级别的日志。 -->
    <root level="INFO">
        <!-- 指定打印日志的appender，这里通过“STDOUT”引用了前面配置的appender -->
        <appender-ref ref="STDOUT" />
    </root>

    <!-- 专门给某一个包指定日志级别 -->
    <logger name="com.senmu" level="DEBUG" additivity="false">
        <appender-ref ref="STDOUT" />
    </logger>

</configuration>
```

## 6、业务功能：登录

### Ⅰ 显示首页

**a> 流程**

http://localhost:8080/all_in_one/  → PortalServlet → templateName: index → 物理视图: index.html → 解析 → 浏览器index页面

**b> 创建ProtalServlet**

1. 创建Java类

   路径：src/main/java/com/senmu/maven/**servlet/module/PortalServlet.java**

   ```java
   public class PortalServlet extends ViewBaseServlet {
   
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           doPost(req, resp);
       }
   
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           // 声明要访问的首页的逻辑视图
           String templateName = "index";
           
           // 调用父类的方法根据逻辑视图名称渲染视图
           processTemplate(templateName, req, resp);
       }
   }
   ```

2. 配置web.xml

   ```xml
   <servlet>
       <servlet-name>portalServlet</servlet-name>
       <servlet-class>com.senmu.maven.servlet.module.PortalServlet</servlet-class>
   </servlet>
   <servlet-mapping>
       <servlet-name>portalServlet</servlet-name>
       <url-pattern>/</url-pattern>
   </servlet-mapping>
   ```

**c> 在index.html 中编写登陆表单**

路径：src/main/**webapp/WEB-INF/pages/index.html**

```html
<!DOCTYPE html>
<html lang="en" xml:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<!-- @{/auth} 解析后：/demo/auth -->
<form th:action="@{/auth}" method="post">
    <!-- 传递 method 请求参数，目的是为了让当前请求调用 AuthServlet 中的 login() 方法 -->
    <input type="hidden" name="method" value="login" />

    <!-- th:text 解析表达式后会替换标签体 -->
    <!-- ${attrName} 从请求域获取属性名为 attrName 的属性值 -->
    <p th:text="${message}"></p>
    <p th:text="${systemMessage}"></p>
    
    账号：<input type="text" name="loginAccount"/><br/>
    密码：<input type="password" name="loginPassword"><br/>
    <button type="submit">登录</button>
</form>

</body>
</html>
```

### Ⅱ 登陆操作

**a> 正常情况的基本流程**

1. 页面登录按钮，提交请求参数：登录账户、密码
2. 调用AuthServlet.login()
3. 调用**userService**.getUserByLoginAccount(loginAccount, loginPassword)
4. 调用**userDao**.selectUserByLoginAccount(loginAccount, encodeLoginPassword)
5. 登录用户信息跳转至temp页面

**b> 创建 用户 业务层**

- **用户业务层：**src/main/java/com/senmu/maven/**service/api/ISysUserService.java**

- **用户业务层处理：**src/main/java/com/senmu/maven/**service/impl/SysUserServiceImpl.java**

**c> 创建登录失败异常**

src/main/java/com/senmu/maven/**exception/LoginFailedException.java**

```java
public class LoginFailedException extends RuntimeException {

    public LoginFailedException() {
    }

    public LoginFailedException(String message) {
        super(message);
    }

    public LoginFailedException(String message, Throwable cause) {
        super(message, cause);
    }

    public LoginFailedException(Throwable cause) {
        super(cause);
    }

    public LoginFailedException(String message, Throwable cause, boolean enableSuppression, boolean writableStackTrace) {
        super(message, cause, enableSuppression, writableStackTrace);
    }
}
```

**d> 增加常量声明**

src/main/java/com/senmu/maven/**util/CommConstants.java**

```java
public class CommConstants {

    public static final String LOGIN_FAILED_MESSAGE = "账号或密码错误！";

    public static final String ACCESS_DENIED_MESSAGE = "拒绝访问！";

    public static final String LOGIN_ATTR_NAME = "loginInfo";
}
```

**e> 创建 AuthServlet**

1. 创建java类

```java
package com.senmu.maven.servlet.module;

public class AuthServlet extends ModelBaseServlet {

    private ISysUserService userService = new SysUserServiceImpl();

    protected void login(
            HttpServletRequest request,
            HttpServletResponse response)
            throws ServletException, IOException{
        try {
            // 1、获取请求参数
            String loginAccount = request.getParameter("loginAccount");
            String loginPassword = request.getParameter("loginPassword");

            // 2、调用  userService 方法执行登录逻辑
            SysUser user = userService.getUserByLoginAccount(loginAccount, loginPassword);

            // 3、通过 request 获取 HttpSession 对象
            HttpSession session = request.getSession();

            // 3、通过 request 获取 HttpSession 对象
            session.setAttribute(CommConstants.LOGIN_ATTR_NAME, user);

            // 5、前往指定页面视图
            String templateName = "temp";

            processTemplate(templateName, request, response);
        } catch (Exception e){
            e.printStackTrace();

            // 6、判断此处捕获到的异常是否是登录失败异常
            if (e instanceof LoginFailedException){

                // ①将异常信息存入请求域
                request.setAttribute("message", e.getMessage());

                // ②处理视图：index
                processTemplate("index", request, response);
            }else {

                // 8、如果不是登录异常则封装为运行时异常继续抛出
                throw new RuntimeException(e);
            }
        }
    }
}
```

2. 配置web.xml

```xml
<servlet>
    <servlet-name>authServlet</servlet-name>
    <servlet-class>com.senmu.maven.servlet.module.AuthServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>authServlet</servlet-name>
    <url-pattern>/auth</url-pattern>
</servlet-mapping>
```

**f> SysUserServiceImpl 中实现方法**

1. src/main/java/com/senmu/maven/**service/api/ISysUserService.java**

   ```java
   public interface ISysUserService {
   
       /**
        * 获取登录用户信息
        * @param loginAccount
        * @param loginPassword
        * @return
        */
       public SysUser getUserByLoginAccount(String loginAccount, String loginPassword);
   }
   ```

2. src/main/java/com/senmu/maven/**service/api/SysUserServiceImpl.java**

   ```java
   public class SysUserServiceImpl implements ISysUserService {
   
       private ISysUserDao userDao = new SysUserDaoImpl();
   
       /**
        * 获取登录用户信息
        *
        * @param loginAccount
        * @param loginPassword
        * @return
        */
       @Override
       public SysUser getUserByLoginAccount(String loginAccount, String loginPassword) {
   
           //1、对密码执行加密
           String encodeLoginPassword = MD5Util.encode(loginPassword);
   
           //2、根据账户和加密密码查询数据库
           SysUser user = userDao.selectUserByLoginAccount(loginAccount, loginPassword);
   
           //3、检查user对象是否为null
           if (user != null){
               // ①不为 null：返回 user
               return user;
           } else {
               // ②为 null：抛登录失败异常
               throw new LoginFailedException(CommConstants.LOGIN_FAILED_MESSAGE);
           }
       }
   }
   ```

**g> SysUserDaoImpl 中实现方法**

```java
public class SysUserDaoImpl extends BaseDao<SysUser> implements ISysUserDao {

    @Override
    public SysUser selectUserByLoginAccount(String loginAccount, String loginPassword) {

        // 编写sql
        String sql = "SELECT user_id userId," +
                "user_name userName," +
                "password password " +
                "FROM sys_user WHERE user_name=? AND password=?";


        // 调用BaseDao查询单个对象方法
        return super.getSingleBean(sql, SysUser.class, loginAccount, loginPassword);
    }
}
```

**h> 临时页面**

src/main/webapp/WEB-INF/pages/**temp.html**

```html
<!DOCTYPE html>
<html lang="en" xml:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>临时</title>
</head>
<body>

    <p th:text="${session.loginInfo}"></p>

</body>
</html>
```

### Ⅲ 退出登录

**a> 在临时页面编写超链接**

```html
<a th:href="@{/auth?method=logout}">注销</a>
```

**b> 在 AuthServlet 编写退出逻辑**

```java
protected  void logout(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException{

    //1、通过 request 对象获取 HttpSession 对象
    HttpSession session = request.getSession();

    //2、将 HttpSession 对象强制失效
    session.invalidate();

    // 3、回到首页
    String templateName = "index";
    processTemplate(templateName, request, response);
}
```

## 7、业务功能：显示部门列表

流程：

1. /dept?method=list
2. DeptServlet: List<SysDept>结果存入请求域
3. List<SysDept> ISysDeptService.getAllDepts()
4. List<SysDept> ISysDeptDao.selectAllDept()

## 8、业务功能：详情、修改、删除

略

## 9、业务功能：登陆检查

1. 创建LoginFilter类：com/senmu/maven/**filter**/LoginFilter.java

   ```java
   public class LoginFilter implements Filter {
   
       @Override
       public void init(FilterConfig filterConfig) throws ServletException {}
   
       @Override
       public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
           // 1、获取HttpSession 对象
           HttpServletRequest request = (HttpServletRequest) servletRequest;
   
           HttpSession session = request.getSession();
   
           // 2、尝试从 Session 域获取已登录的对象
           Object loginUser = session.getAttribute(CommConstants.LOGIN_ATTR_NAME);
   
           // 3、判断 loginUser 是否为空
           if (loginUser != null){
   
               // 4、若不为空说明当前请求已登录，直接放行
               filterChain.doFilter(request, servletResponse);
               return;
           }
   
           // 5、否则说明尚未登录，回到登录页面
           request.setAttribute("systemMessage", CommConstants.ACCESS_DENIED_MESSAGE);
           request.getRequestDispatcher("/").forward(request,servletResponse);
       }
   
       @Override
       public void destroy() {}
   }
   ```

2. 注册：

   ```xml
       <filter>
           <filter-name>loginFilter</filter-name>
           <filter-class>com.senmu.maven.filter.LoginFilter</filter-class>
       </filter>
       <filter-mapping>
           <filter-name>loginFilter</filter-name>
           <url-pattern>/dept</url-pattern>
       </filter-mapping>
   ```

   注意：把 LoginFilter 放在 TransactionFilter 前面声明，原因是：如果登录检查失败不放行，直接跳转到页面，此时将不必执行 TransactionFilter 中的事务操作，可以节约性能。

## 10、打包部署

### Ⅰ 适配部署环境

MySQL 连接信息中，IP 地址部分需要改成 localhost。

src/main/resources/jdbc.properties

```properties
url=jdbc:mysql://localhost:3306/maven
```

### Ⅱ 跳过测试打包

```sh
mvn clean package -Dmaven.test.skip=true
```

指定war包名称(可默认)

```xml
    <!-- 对构建过程进行自己的定制 -->
    <build>
        <!-- 当前工程在构建过程中使用的最终名称 -->
        <finalName>demo-me</finalName>
    </build>
```

### Ⅲ 部署执行

1. 上传war包：[见上文第三章第4节](#Ⅹ 将war包部署道Tomcat上运行)
2. 启动Tomcat: startup.bat(Windows)或startup.sh(Linux)

# 七、SSM整合伪分布式案例

## 1、创建工程，引入依赖

### Ⅰ 创建工程

**a> 工程清单**

| 工程名               | 地位   | 说明                 |
| -------------------- | ------ | -------------------- |
| maven-ssm            | 父工程 | 总体管理各个子工程   |
| module01-web         | 子工程 | 唯一的 war 包工程    |
| module02-component   | 子工程 | 管理项目中的各种组件 |
| module03-entity      | 子工程 | 管理项目中的实体类   |
| module04-util        | 子工程 | 管理项目中的工具类   |
| module05-environment | 子工程 | 框架环境所需依赖     |
| module06-generate    | 子工程 | Mybatis 逆向工程     |

**b> 工程间关系**

依赖关系：module01-web —>依赖—> module02-component —>依赖—> {module03-entity，module04-util，module05-environment}

独立存在：module06-generate

### Ⅱ 各工程 POM 配置

**a> 父工程**

maven-ssm模块下pom.xml

各子工程创建好之后就会有下面配置，不需要手动编辑：

```xml
    <groupId>com.senmu</groupId>
    <artifactId>maven-ssm</artifactId>
    <packaging>pom</packaging>
    <version>1.0-SNAPSHOT</version>
    <modules>
        <module>module01-web</module>
        <module>module02-component</module>
        <module>module03-entity</module>
        <module>module04-util</module>
        <module>module05-environment</module>
        <module>module06-generate</module>
    </modules>
```

**b> Mybatis 逆向工程**

module06-generate/pom.xml

```xml
    <!-- 依赖MyBatis核心包 -->
    <dependencies>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.7</version>
        </dependency>
    </dependencies>

    <!-- 控制Maven在构建过程中相关配置 -->
    <build>

        <!-- 构建过程中用到的插件 -->
        <plugins>

            <!-- 具体插件，逆向工程的操作是以构建过程中插件形式出现的 -->
            <plugin>
                <groupId>org.mybatis.generator</groupId>
                <artifactId>mybatis-generator-maven-plugin</artifactId>
                <version>1.3.0</version>

                <!-- 插件的依赖 -->
                <dependencies>

                    <!-- 逆向工程的核心依赖 -->
                    <dependency>
                        <groupId>org.mybatis.generator</groupId>
                        <artifactId>mybatis-generator-core</artifactId>
                        <version>1.3.2</version>
                    </dependency>

                    <!-- 数据库连接池 -->
                    <dependency>
                        <groupId>com.mchange</groupId>
                        <artifactId>c3p0</artifactId>
                        <version>0.9.2</version>
                    </dependency>

                    <!-- MySQL驱动 -->
                    <dependency>
                        <groupId>mysql</groupId>
                        <artifactId>mysql-connector-java</artifactId>
                        <version>8.0.28</version>
                    </dependency>
                </dependencies>
            </plugin>
        </plugins>
    </build>
```

**c> 环境依赖工程**

module05-environment/pom.xml

```xml
<dependencies>
    <!-- SpringMVC -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.3.1</version>
    </dependency>

    <!-- Spring 持久化层所需依赖 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-orm</artifactId>
        <version>5.3.1</version>
    </dependency>

    <!-- 日志 -->
    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-classic</artifactId>
        <version>1.2.3</version>
    </dependency>

    <!-- Spring5和Thymeleaf整合包 -->
    <dependency>
        <groupId>org.thymeleaf</groupId>
        <artifactId>thymeleaf-spring5</artifactId>
        <version>3.0.12.RELEASE</version>
    </dependency>

    <!-- Mybatis 和 Spring 的整合包 -->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>2.0.6</version>
    </dependency>

    <!-- Mybatis核心 -->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.7</version>
    </dependency>

    <!-- MySQL驱动 -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.28</version>
    </dependency>

    <!-- 数据源 -->
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.0.31</version>
    </dependency>
</dependencies>
```

**d> 工具类工程**

无配置

**e> 实体类工程**

无配置

**f> 组件工程**

module02-component/pom.xml

```xml
<dependencies>
    <dependency>
        <groupId>com.senmu</groupId>
        <artifactId>module03-entity</artifactId>
        <version>1.0-SNAPSHOT</version>
    </dependency>
    <dependency>
        <groupId>com.senmu</groupId>
        <artifactId>module04-util</artifactId>
        <version>1.0-SNAPSHOT</version>
    </dependency>
    <dependency>
        <groupId>com.senmu</groupId>
        <artifactId>module05-environment</artifactId>
        <version>1.0-SNAPSHOT</version>
    </dependency>

    <!-- ServletAPI -->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>3.1.0</version>
        <scope>provided</scope>
    </dependency>
</dependencies>
```

**g> Web 工程**

module01-web/pom.xml

```xml
<dependencies>
    <dependency>
        <groupId>com.senmu</groupId>
        <artifactId>module02-component</artifactId>
        <version>1.0-SNAPSHOT</version>
    </dependency>

    <!-- junit5 -->
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter-api</artifactId>
        <version>5.7.0</version>
        <scope>test</scope>
    </dependency>

    <!-- Spring 的测试功能 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-test</artifactId>
        <version>5.3.1</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

## 2、 搭建环境：持久化层

### Ⅰ 物理建模

数据库表创建

### Ⅱ MyBatis逆向工程

**a> 配置文件** 

src/main/resources/**generatorConfig.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN" "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
    <!--
            targetRuntime: 执行生成的逆向工程的版本
                    MyBatis3Simple: 生成基本的CRUD（清新简洁版）
                    MyBatis3: 生成带条件的CRUD（奢华尊享版）
     -->
    <context id="DB2Tables" targetRuntime="MyBatis3">
        <!-- 数据库的连接信息 -->
        <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/maven"
                        userId="root"
                        password="123456">
        </jdbcConnection>
        <!-- javaBean的生成策略-->
        <javaModelGenerator targetPackage="com.senmu.maven.entity" targetProject=".\src\main\java">
            <property name="enableSubPackages" value="true" />
            <property name="trimStrings" value="true" />
        </javaModelGenerator>
        <!-- SQL映射文件的生成策略 -->
        <sqlMapGenerator targetPackage="com.senmu.maven.mapper"  targetProject=".\src\main\java">
            <property name="enableSubPackages" value="true" />
        </sqlMapGenerator>
        <!-- Mapper接口的生成策略 -->
        <javaClientGenerator type="XMLMAPPER" targetPackage="com.senmu.maven.mapper"  targetProject=".\src\main\java">
            <property name="enableSubPackages" value="true" />
        </javaClientGenerator>
        <!-- 逆向分析的表 -->
        <!-- tableName设置为*号，可以对应所有表，此时不写domainObjectName -->
        <!-- domainObjectName属性指定生成出来的实体类的类名 -->
        <table tableName="sys_user" domainObjectName="SysUser"/>
        <table tableName="sys_dept" domainObjectName="SysDept"/>
    </context>
</generatorConfiguration>
```

**b> 执行逆向生成**

idea -> Maven ->  module06-generate -> Plugins -> mybatis-generator -> mybatis-generator:generate

**c> 将生成文件放置对应位置**

- **Mapper.xml 配置文件**：移至**module01-web**/src/main/**resources/mapper**
- **Mapper接口**：移至**module02-component**/src/main/java/com/senmu/maven/**mapper**
- **实体类**：移至**module03-entity**src/main/java/com/senmu/maven/**entity**

### Ⅲ 建立数据库连接

**a> 配置 数据库连接信息 module01-web\src\main\resources\jdbc.properties**

```properties
dev.driverClassName=com.mysql.cj.jdbc.Driver
dev.url=jdbc:mysql://localhost:3306/maven
dev.username=root
dev.password=123456
dev.initialSize=10
dev.maxActive=20
dev.maxWait=10000
```

**b> 配置数据源 module01-web\src\main\resources\spring-persist.xml**

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <context:property-placeholder location="classpath:jdbc.properties"/>

    <bean id="druidDataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="username" value="${dev.username}"/>
        <property name="password" value="${dev.password}"/>
        <property name="url" value="${dev.url}"/>
        <property name="driverClassName" value="${dev.driverClassName}"/>
        <property name="initialSize" value="${dev.initialSize}"/>
        <property name="maxActive" value="${dev.maxActive}"/>
        <property name="maxWait" value="${dev.maxWait}"/>
    </bean>
</beans>
```

**c> 测试**

```java
@ExtendWith(SpringExtension.class)
@ContextConfiguration(value = "classpath:spring-persist.xml")
public class dbConnectTest {

    @Autowired
    private DataSource dataSource;

    private final Logger logger = LoggerFactory.getLogger((dbConnectTest.class));

    @Test
    public void testConnection() throws SQLException{
        Connection connection = dataSource.getConnection();
        logger.debug(connection.toString());
    }
}
```



> TIP
>
> 配置文件为什么要放到 Web 工程里面？
>
> - Web 工程将来生成 war 包。
> - war 包直接部署到 Tomcat 运行。
> - Tomcat 从 war 包（解压目录）查找配置文件最直接。
> - 如果不是把配置文件放在 Web 工程，而是放在 Java 工程，那就等于将配置文件放在了 war 包内的 jar 包中。
> - 配置文件在 jar 包中读取相对困难。

### Ⅳ Spring 整合Mybatis

**a> 配置SqlSessionFactoryBean**

目的：

1. 装配数据源
2. 指定Mapper配置文件的位置

位置：src/main/resources/**spring-persist.xml**

```xml
<!-- 配置 SqlSessionFactoryBean -->
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">

    <!-- 装配数据源 -->
    <property name="dataSource" ref="druidDataSource"/>

    <!-- 指定 Mapper 配置文件的位置 -->
    <property name="mapperLocations" value="classpath:mapper/*Mapper.xml"/>
</bean>
```

**b> 扫描 Mapper 接口**

```xml
<mybatis:scan base-package="com.senmu.maven.mapper"/>
```

**c> 测试**

```java
@Autowired
private SysUserMapper userMapper;

@Test
public void testUserMapper(){
    List<SysUser> userList = userMapper.selectByExample(new SysUserExample());
    for (SysUser user: userList){
        System.out.println("user = " + user.getUserName());
    }
}
```

## 3、 搭建环境：事务控制

### Ⅰ 声明式事务注解

src/main/resources/**spring-persist.xml**

```xml
<!-- 配置事务管理器 -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!-- 装配数据源 -->
        <property name="dataSource" ref="druidDataSource"/>
    </bean>

    <!-- 配置事务的注解驱动，开启基于注解的声明式事务功能 -->
    <tx:annotation-driven transaction-manager="transactionManager"/>

    <!-- 配置对 Service 所在包的自动扫描 -->
    <context:component-scan base-package="com.senmu.maven.service"/>
```

### Ⅱ 注解写法

**a> 查询操作**

```java
@Transactional(readOnly = true)
```

**b> 增删改操作**

```java
@Transactional(propagation = Propagation.REQUIRES_NEW, readOnly = false)
```

> TIP
>
> 在具体代码开发中可能会将相同设置的 @Transactional 注解提取到 Service 类上。

## 4、 搭建环境：表述层

### Ⅰ 设定 Web 工程

[步骤如前文所示](#4、创建Web模块工程)

### Ⅱ web.xml 配置

**a> 配置 ContextLoaderListener**

```xml
<!-- 第一部分：加载 spring-persist.xml 配置文件 -->
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:spring-persist.xml</param-value>
</context-param>
<listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```

**b> 配置 DispatcherServlet**

```xml
<!-- 第二部分：加载 spring-mvc.xml 配置文件 -->
<servlet>
    <servlet-name>dispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:spring-mvc.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>dispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

**c> 配置 CharacterEncodingFilter**

```xml
<!-- 第三部分：设置字符集的 Filter -->
<filter>
    <filter-name>characterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
    <init-param>
        <param-name>forceRequestEncoding</param-name>
        <param-value>true</param-value>
    </init-param>
    <init-param>
        <param-name>forceResponseEncoding</param-name>
        <param-value>true</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>characterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

**d> 配置 HiddenHttpMethodFilter**

```xml
<!-- 第四部分：按照 RESTFul 风格负责转换请求方式的 Filter -->
<filter>
    <filter-name>hiddenHttpMethodFilter</filter-name>
    <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>hiddenHttpMethodFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

### Ⅲ 显示首页

**a> 配置 SpringMVC**

**module01-web**/src/main/resources/**spring-mvc.xml**

1. 基本配置

   ```xml
   <!-- 开启 SpringMVC 的注解驱动功能 -->
   <mvn:annotation-driven />
   
   <!-- 让 SpringMVC 对没有 @RequestMapping 的请求直接放行 -->
   <mvc:default-servlet-handler />
   ```

2. 配置视图解析器

   ```xml
   <!-- 配置视图解析器 -->
   <bean id="thymeleafViewResolver" class="org.thymeleaf.spring5.view.ThymeleafViewResolver">
   	<property name="order" value="1"/>
   	<property name="characterEncoding" value="UTF-8"/>
   	<property name="templateEngine">
   		<bean class="org.thymeleaf.spring5.SpringTemplateEngine">
   			<property name="templateResolver">
   				<bean class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">
   					<property name="prefix" value="/WEB-INF/templates/"/>
   					<property name="suffix" value=".html"/>
   					<property name="characterEncoding" value="UTF-8"/>
   					<property name="templateMode" value="HTML5"/>
   				</bean>
   			</property>
   		</bean>
   	</property>
   </bean>
   ```

3. 配置自动扫描的包

   ```xml
   <!-- 配置自动扫描的包 -->
   <context:component-scan base-package="com.senmu.maven.controller"/>
   ```

**b> 配置 view-controller 访问首页**

```xml
<!-- 配置 view-controller -->
<mvc:view-controller path="/" view-name="index" />
```

**c> 创建首页模板文件**

**module01-web**/webapp/WEB-INF/**templates/index.html**

```html
<!DOCTYPE html>
<html lang="en" xml:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>首页</title>
</head>
<body>

<!-- @{/auth} 解析后：/demo/auth -->
<form th:action="@{/auth/login}" method="post">

    <!-- th:text 解析表达式后会替换标签体 -->
    <!-- ${attrName} 从请求域获取属性名为 attrName 的属性值 -->
    <p style="color: red;font-weight: bold;" th:text="${message}"></p>
    <p style="color: red;font-weight: bold;" th:text="${systemMessage}"></p>

    账号：<input type="text" name="loginAccount"/><br/>
    密码：<input type="password" name="loginPassword"><br/>
    <button type="submit">登录</button>
</form>

</body>
</html>
```

## 5、 搭建环境：辅助功能

### Ⅰ 登录失败异常

**module04-util**/src/main/java/com/senmu/maven/**exception/LoginFailedException.java**

```java
public class LoginFailedException extends RuntimeException {

    public LoginFailedException() {
    }

    public LoginFailedException(String message) {
        super(message);
    }

    public LoginFailedException(String message, Throwable cause) {
        super(message, cause);
    }

    public LoginFailedException(Throwable cause) {
        super(cause);
    }

    public LoginFailedException(String message, Throwable cause, boolean enableSuppression, boolean writableStackTrace) {
        super(message, cause, enableSuppression, writableStackTrace);
    }
}
```

### Ⅱ 常量类

**module04-util**/src/main/java/com/senmu/maven/**util/ComConstant.java**

```java
public class ComConstant {

    public static final String LOGIN_FAILED_MESSAGE = "账号、密码错误!";
    public static final String ACCESS_DENIED_MESSAGE = "访问失败！";
    public static final String LOGIN_USER_ATTR_NAME = "loginInfo";

}
```

### Ⅲ MD5 工具

**module04-util**/src/main/java/com/senmu/maven/**util/MD5Util.java**

```java
public class MD5Util {
    /**
     * 针对明文字符串执行MD5加密
     * @param source
     * @return
     */
    public static String encode(String source) {

        // 1.判断明文字符串是否有效
        if (source == null || "".equals(source)) {
            throw new RuntimeException("用于加密的明文不可为空");
        }

        // 2.声明算法名称
        String algorithm = "md5";

        // 3.获取MessageDigest对象
        MessageDigest messageDigest = null;
        try {
            messageDigest = MessageDigest.getInstance(algorithm);
        } catch (NoSuchAlgorithmException e) {
            e.printStackTrace();
        }

        // 4.获取明文字符串对应的字节数组
        byte[] input = source.getBytes();

        // 5.执行加密
        byte[] output = messageDigest.digest(input);

        // 6.创建BigInteger对象
        int signum = 1;
        BigInteger bigInteger = new BigInteger(signum, output);

        // 7.按照16进制将bigInteger的值转换为字符串
        int radix = 16;
        String encoded = bigInteger.toString(radix).toUpperCase();

        return encoded;
    }
}
```

### Ⅳ 日志配置文件

**module01-web**/src/**resources/logback.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration debug="true">
    <!-- 指定日志输出的位置 -->
    <appender name="STDOUT"
              class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <!-- 日志输出的格式 -->
            <!-- 按照顺序分别是：时间、日志级别、线程名称、打印日志的类、日志主体内容、换行 -->
            <pattern>[%d{HH:mm:ss.SSS}] [%-5level] [%thread] [%logger] [%msg]%n</pattern>
            <charset>UTF-8</charset>
        </encoder>
    </appender>

    <!-- 设置全局日志级别。日志级别按顺序分别是：DEBUG、INFO、WARN、ERROR -->
    <!-- 指定任何一个日志级别都只打印当前级别和后面级别的日志。 -->
    <root level="INFO">
        <!-- 指定打印日志的appender，这里通过“STDOUT”引用了前面配置的appender -->
        <appender-ref ref="STDOUT" />
    </root>

    <!-- 专门给某一个包指定日志级别 -->
    <logger name="com.atguigu" level="DEBUG" additivity="false">
        <appender-ref ref="STDOUT" />
    </logger>

</configuration>
```

## 6、 业务功能：登录

### Ⅰ AuthController

**module02-component**/src/main/java/com/senmu/maven/**controller/AuthController.java**

```java
@Controller
public class AuthController {

    @Autowired
    private ISysUserService userService;

    @RequestMapping("/auth/login")
    public String login(
            @RequestParam("loginAccount") String loginAccount,
            @RequestParam("loginPassword") String loginPassword,
            HttpSession session,
            Model model
    ){
        // 1、尝试查询登录信息
        SysUser user = userService.getLoginUserInfo(loginAccount, loginPassword);

        // 2、判断登录是否成功
        if (user == null){

            // 3、登陆失败，返回登录失败信息
            model.addAttribute("message", ComConstant.LOGIN_FAILED_MESSAGE);

            return "index";
        }else {

            // 4、登录成功，返回登录用户信息
            session.setAttribute("loginInfo", user);

            return "target";
        }
    }
}
```

### Ⅱ SysUserServiceImpl

**module02-component**/src/main/java/com/senmu/maven/**service/impl/SysUserServiceImpl.java**

```java
@Service
@Transactional(readOnly = true)
public class SysUserServiceImpl implements ISysUserService {

    @Autowired
    private SysUserMapper userMapper;

    @Override
    public SysUser getLoginUserInfo(String loginAccount, String loginPassword) {

        // 1、密码加密
        String encodedLoginPassword = MD5Util.encode(loginPassword);

        // 2、通过 QBC 查询方式封装查询条件
        SysUserExample example = new SysUserExample();

        SysUserExample.Criteria criteria = example.createCriteria();

        criteria.andUserNameEqualTo(loginAccount).andPasswordEqualTo(encodedLoginPassword);

        List<SysUser> userList = userMapper.selectByExample(example);

        if (userList != null && userList.size() > 0){

            // 3、返回查询结果
            return userList.get(0);
        }

        return null;
    }
}
```

### Ⅲ target.html

**module01-web**/webapp/WEB-INF/**templates/target.html**

```html
<!DOCTYPE html>
<html lang="en" xml:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

    <p th:text="${session.loginInfo}"></p>

</body>
</html>
```

# 八、微服务架构案例

## 1、 创建工程

### Ⅰ 工程模块

| 工程名                           | 地位   | 说明               |
| -------------------------------- | ------ | ------------------ |
| **maven-ms**                     | 父工程 | 总体管理各个子工程 |
| module01-gateway                 | 子工程 | 网关               |
| **module02-user-auth-center**    | 子工程 | 用户中心           |
| module03-user-manager-center     | 子工程 | 用户数据维护中心   |
| module04-dept-manager-center     | 子工程 | 部门数据维护中心   |
| module05-work-manager-center     | 子工程 | 业务操作中心       |
| **module06-mysql-data-provider** | 子工程 | MySQL 数据提供者   |
| module07-redis-data-provider     | 子工程 | Redis 数据提供者   |
| **module08-base-api**            | 子工程 | 声明 Feign 接口    |
| **module09-base-entity**         | 子工程 | 实体类             |
| **module10-base-util**           | 子工程 | 工具类             |

### Ⅱ  工程间依赖关系

- **module02-user-auth-center** —>依赖 —> **module08-base-api**；
- **module08-base-api** —>依赖 —> **module09-base-entity**、**module10-base-util**；
- **module06-mysql-data-provider **—>依赖 —> **module09-base-entity**、**module10-base-util**；

## 2、 父工程管理依赖

maven-ms/pom.xml

```xml
<dependencyManagement>
    <dependencies>

        <!-- SpringCloud 依赖导入 -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>Hoxton.SR9</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>

        <!-- SpringCloud Alibaba 依赖导入 -->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-alibaba-dependencies</artifactId>
            <version>2.2.6.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>

        <!-- SpringBoot 依赖导入 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-dependencies</artifactId>
            <version>2.3.6.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>

        <!-- 通用 Mapper 依赖 -->
        <dependency>
            <groupId>tk.mybatis</groupId>
            <artifactId>mapper-spring-boot-starter</artifactId>
            <version>2.1.5</version>
        </dependency>

        <!-- Druid 数据源依赖 -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.10</version>
        </dependency>

        <!-- JPA 依赖 -->
        <dependency>
            <groupId>javax.persistence</groupId>
            <artifactId>persistence-api</artifactId>
            <version>1.0</version>
        </dependency>
    </dependencies>
</dependencyManagement>
```

> TIP
>
> 此处可能出现父工程爆红却无法导入的情况
>
> 解决方案：可先将爆红的依赖放入其他工程中进行导入。

## 3、 打基础

### Ⅰ module10-base-util

**a> 常量类**

```java
public class ComConstant {

    public static final String LOGIN_FAILED_MESSAGE = "账号、密码错误！";
    public static final String ACCESS_DENIED_MESSAGE = "拒绝访问！";
    public static final String LOGIN_USER_ATTR_NAME = "loginInfo";

}
```

**b> 字符串加密工具类**

```
public class MD5Util {
    /**
     * 针对明文字符串执行MD5加密
     * @param source
     * @return
     */
    public static String encode(String source) {

        // 1.判断明文字符串是否有效
        if (source == null || "".equals(source)) {
            throw new RuntimeException("用于加密的明文不可为空");
        }

        // 2.声明算法名称
        String algorithm = "md5";

        // 3.获取MessageDigest对象
        MessageDigest messageDigest = null;
        try {
            messageDigest = MessageDigest.getInstance(algorithm);
        } catch (NoSuchAlgorithmException e) {
            e.printStackTrace();
        }

        // 4.获取明文字符串对应的字节数组
        byte[] input = source.getBytes();

        // 5.执行加密
        byte[] output = messageDigest.digest(input);

        // 6.创建BigInteger对象
        int signum = 1;
        BigInteger bigInteger = new BigInteger(signum, output);

        // 7.按照16进制将bigInteger的值转换为字符串
        int radix = 16;
        String encoded = bigInteger.toString(radix).toUpperCase();

        return encoded;
    }
}
```

**c> 登陆失败异常**

```java
public class LoginFailedException extends RuntimeException {

    public LoginFailedException() {
    }

    public LoginFailedException(String message) {
        super(message);
    }

    public LoginFailedException(String message, Throwable cause) {
        super(message, cause);
    }

    public LoginFailedException(Throwable cause) {
        super(cause);
    }

    public LoginFailedException(String message, Throwable cause, boolean enableSuppression, boolean writableStackTrace) {
        super(message, cause, enableSuppression, writableStackTrace);
    }
}
```

**d> 远程方法调用统一返回结果**

```java
public class ResultEntity<T> {

    public static final String SUCCESS = "SUCCESS";
    public static final String FAILED = "FAILED";

    // 封装当前请求处理的结果是成功还是失败
    private String result;

    // 请求处理失败时返回的错误消息
    private String message;

    // 要返回的数据
    private T data;

    /**
     * 请求处理成功且不需要返回数据时使用的工具方法
     * @param <Type>
     * @return
     */
    public static <Type> ResultEntity<Type> successWithoutData(){
        return new ResultEntity<Type>(SUCCESS, null, null);
    }

    /**
     * 请求处理成功且需要返回数据时使用的工具方法
     * @param data
     * @param <Type>
     * @return
     */
    public static <Type> ResultEntity<Type> successWithData(Type data){
        return new ResultEntity<Type>(SUCCESS, null, data);
    }

    public static <Type> ResultEntity<Type> faild(String message){
        return new ResultEntity<Type>(FAILED, message, null);
    }

    public ResultEntity() {

    }

    public ResultEntity(String result, String message, T data) {
        super();
        this.result = result;
        this.message = message;
        this.data = data;
    }

    @Override
    public String toString() {
        return "ResultEntity [result=" + result + ", message=" + message + ", data=" + data + "]";
    }

    public String getResult() {
        return result;
    }

    public void setResult(String result) {
        this.result = result;
    }

    public String getMessage() {
        return message;
    }

    public void setMessage(String message) {
        this.message = message;
    }

    public T getData() {
        return data;
    }

    public void setData(T data) {
        this.data = data;
    }
}
```

### Ⅱ module09-base-entity

**a> 引入依赖**

```xml
<dependency>
    <groupId>javax.persistence</groupId>
    <artifactId>persistence-api</artifactId>
</dependency>
```

**b> 创建实体类**

```java
@Table(name = "sys_user")
public class SysUser implements Serializable {

    private Long userId;

    private Long deptId;

    private String userName;

    private String email;

    private String phonenumber;

    private String sex;

    private String password;

    private String delFlag;

    private String createBy;

    private Date createTime;

    public Long getUserId() {
        return userId;
    }

    public void setUserId(Long userId) {
        this.userId = userId;
    }

    public Long getDeptId() {
        return deptId;
    }

    public void setDeptId(Long deptId) {
        this.deptId = deptId;
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName == null ? null : userName.trim();
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email == null ? null : email.trim();
    }

    public String getPhonenumber() {
        return phonenumber;
    }

    public void setPhonenumber(String phonenumber) {
        this.phonenumber = phonenumber == null ? null : phonenumber.trim();
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex == null ? null : sex.trim();
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password == null ? null : password.trim();
    }

    public String getDelFlag() {
        return delFlag;
    }

    public void setDelFlag(String delFlag) {
        this.delFlag = delFlag == null ? null : delFlag.trim();
    }

    public String getCreateBy() {
        return createBy;
    }

    public void setCreateBy(String createBy) {
        this.createBy = createBy == null ? null : createBy.trim();
    }

    public Date getCreateTime() {
        return createTime;
    }

    public void setCreateTime(Date createTime) {
        this.createTime = createTime;
    }
}
```

## 4、 用户登录认证服务：提供端

### Ⅰ 总体分析

![总体分析](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202209281509242.png)

### Ⅱ 注册中心

在本地启动nacos注册中心

```sh
startup.cmd -m standalone
```

[Nacos官方文档](https://nacos.io/zh-cn/docs/quick-start.html)

### Ⅲ 声明接口，暴露服务

**a> 接口文档**

[参考案例](http://heavy_code_industry.gitee.io/code_heavy_industry/pro002-maven/chapter08/api.html)

**b> Feign 接口代码**

1. **位置**：

   **module08-base-api**/src/main/java/com/senmu/maven/**api/MySQLProvider.java**

2. **引入依赖**

   ```xml
   <!-- OpenFeign 专用依赖 -->
   <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-openfeign</artifactId>
   </dependency>
   ```

3. **接口代码**

   **注意**：@FeignClient 注解中指定的是提供服务的微服务名称，要和注册中心注册的一致

   ```java
   // @FeignClient 注解将当前接口标记为服务暴露接口
   // name 属性：指定被暴露服务的微服务名称
   @FeignClient(name = "module06-mysql-data-provider")
   public interface MySQLProvider {
   
       @RequestMapping("/remote/get/login/info")
       ResultEntity<SysUser> getLoginUserInfo(
   
               // @RequestParam 无论如何不能省略
               @RequestParam("loginAccount") String loginAccount,
               @RequestParam("loginPassword") String loginPassword);
   }
   ```

### Ⅳ 实现接口

**a> 所属工程模块**

module06-mysql-data-provider

**b> 引入依赖**

```xml
<!-- Nacos 服务注册发现启动器 -->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>

<!--通用mapper启动器依赖-->
<dependency>
    <groupId>tk.mybatis</groupId>
    <artifactId>mapper-spring-boot-starter</artifactId>
</dependency>

<!--mysql驱动-->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>

<!--druid启动器依赖-->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
</dependency>

<!--web启动器依赖-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>

<!--编码工具包-->
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-lang3</artifactId>
</dependency>

<!--单元测试启动器-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>

<!--热部署 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <scope>runtime</scope>
    <optional>true</optional>
</dependency>
```

**c> 代码实现**

1.  总体内容：实现controller、mapper、service、主启动类、YAML配置文件

2. SysUserMapper

   继承 tk.mybatis.mapper.common.Mapper 后就可以使用通用 Mapper 提供的常规代码实现。除非有非常规需求，否则我们自己什么都不用写。

   ```java
   public interface SysUserMapper extends Mapper<SysUser> {
   }
   ```

3. service接口

   ```java
   SysUser getLoginUserInfo(String loginAccount, String loginPassword);
   ```

4. service实现

    @Autowired
       private SysUserMapper userMapper;

   ```java
   @Override
   public SysUser getLoginUserInfo(String loginAccount, String loginPassword) {
   
       String encodedLoginPassword = MD5Util.encode(loginPassword);
   
       Example example = new Example(SysUser.class);
   
       Example.Criteria criteria = example.createCriteria();
   
       criteria.andUserNameEqualTo(loginAccount).andPasswordEqualTo(encodedLoginPassword);
   
       List<SysUser> userList = userMapper.selectByExample(example);
   
       if (userList != null && userList.size() > 0){
   
           return userList.get(0);
       }
   
       return null;
   }
   ```

5. SysUserController

   ```java
   @RestController
   public class SysUserController {
   
       @Autowired
       private ISysUserService userService;
   
       @RequestMapping("/remote/get/login/info")
       ResultEntity<SysUser> getLoginUserInfo(
   
               // @RequestParam 无论如何不能省略
               @RequestParam("loginAccount") String loginAccount,
               @RequestParam("loginPassword") String loginPassword){
           try {
               SysUser userInfo = userService.getLoginUserInfo(loginAccount, loginPassword);
   
               return ResultEntity.successWithData(userInfo);
           } catch (Exception e){
               e.printStackTrace();
               String message = e.getMessage();
               return ResultEntity.faild(message);
           }
       }
   }
   ```

6. 主启动类

   ```java
   package com.senmu.maven;
   
   // 为了让当前微服务对接（注册或发现服务）注册中心
   @EnableDiscoveryClient
   
   // SpringBoot 标配注解
   @SpringBootApplication
   
   // 扫描通用 Mapper 的 Mapper 接口所在包
   // 这个注解全类名：tk.mybatis.spring.annotation.MapperScan
   // basePackage 属性：指定要扫描的 Mapper 接口所在的包
   @MapperScan(basePackages = "com.senmu.maven.mapper")
   public class MainType {
   
       public static void main(String[] args) {
           SpringApplication.run(MainType.class, args);
       }
   }
   ```

7. YAML 配置文件：src/main/resources/**application.yml**

   ```yml
   server:
     port: 10001
   spring:
     datasource:
       driver-class-name: com.mysql.cj.jdbc.Driver
       url: jdbc:mysql://127.0.0.1:3306/maven
       username: root
       password: 123456
       type: com.alibaba.druid.pool.DruidDataSource
     application:
       name: module06-mysql-data-provider
     cloud:
       nacos:
         discovery:
           server-addr: localhost:8848
   ```

## 5、 用户登录认证服务：消费端

### Ⅰ 所属工程模块

**module02-user-auth-center**

### Ⅱ 引入依赖

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

### Ⅲ YAML 配置文件

src/main/resources/application.yml

```yml
server:
  port: 10002
spring:
  application:
    name: module02-user-auth-center
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848
```

> TIP
>
> 就 Thymeleaf 而言，有两个常用属性，但我们全部都使用的是默认值，所以可以省略。



### Ⅳ 显示首页

**a> 配置 view-controller**

com/senmu/maven/**config/DemoConfig.java**

```java
@SpringBootConfiguration
public class DemoConfig implements WebMvcConfigurer {

    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/").setViewName("index");
    }
}
```

**b> Thymeleaf 视图模板页面**

src/main/resources/**templates/index.html**

```html
<!DOCTYPE html>
<html lang="en" xml:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>首页</title>
</head>
<body>

<form th:action="@{/consumer/do/login}" method="post">
    <p style="color: red;font-weight: bold;" th:if="${not #strings.isEmpty(authMessage)}" th:text="${authMessage}">
        这里根据条件显示登录失败消息</p>
    <p style="color: red;font-weight: bold;" th:if="${not #strings.isEmpty(systemMessage)}" th:text="${systemMessage}">
        这里根据条件显示系统消息</p>
    账号：<input type="text" name="loginAccount"/><br/>
    密码：<input type="password" name="loginPassword"><br/>
    <button type="submit">登录</button>
</form>
</body>
</html>
```

### Ⅴ 登录验证

**a> 主要流程**

![无标题](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202209290937987.png)

**b> 主启动类**

**注意**：一定要标记 @EnableFeignClients 注解。

```java
@EnableFeignClients
@EnableDiscoveryClient
@SpringBootApplication
public class MainType {

    public static void main(String[] args) {
        SpringApplication.run(MainType.class, args);
    }

}
```

**c> AuthController**

com/senmu/maven/**controller/AuthController.java**

```java
@Controller
public class AuthController {

    // 装配远程接口
    // 1、本地使用 @Autowired 注解装配远程接口类型即可实现方法的远程调用，
    // 看起来就像是调用本地方法一样，我们管这种特性叫方法的声明式远程调用。
    // 2、凭啥通过 @Autowired 注解就能够导入远程接口对应的 bean
    //      ①当前环境包含 Feign 相关 jar 包。
    //      ②当前微服务的主启动类上标记 @EnableFeignClients
    //      ③符合 SpringBoot 自动扫描包的约定规则：默认情况下主启动类所在的包、以及主启动类所在包的子包都会被自动扫描
    //          主启动类所在包：     com.senmu.maven
    //          被扫描的接口所在的包：com.senmu.maven.api
    @Autowired
    private MySQLProvider mySQLProvider;

    /**
     * 执行登录方法
     * @param loginAccount
     * @param loginPassword
     * @param session
     * @param model
     * @return
     */
    @RequestMapping("/consumer/do/login")
    public String doLogin(@RequestParam("loginAccount") String loginAccount,
                          @RequestParam("loginPassword") String loginPassword, HttpSession session, Model model) {

        // 1、调用远程接口根据登录账号、密码查询 SysUser 对象
        ResultEntity<SysUser> resultEntity = mySQLProvider.getLoginUserInfo(loginAccount, loginPassword);

        // 2、验证远程接口调用是否成功
        String result = resultEntity.getResult();

        if ("SUCCESS".equals(result)) {

            // 3、从 ResultEntity 中获取查询得到的 SysUser 对象
            SysUser user = resultEntity.getData();

            // 4、将 user 对象存入 Session 域
            session.setAttribute("loginInfo", user);

            // 5、前往 target 页面
            return "target";
        } else {

            // 6、获取失败消息
            String message = resultEntity.getMessage();

            // 7、将失败消息存入模型
            model.addAttribute("message", message);

            // 8、回到登录页面
            return "index";

        }
    }
}
```

## 6、 部署运行

### Ⅰ 目标

![最终目标](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202209291002310.png)

### Ⅱ 微服务打包

**a> 修改MySQL 连接信息**

module06-mysql-data-provider/src/main/resources/application.yml

当前微服务和 MySQL 位于同一个服务器上，需要把访问地址改成 localhost

**b> 在父工程执行 install 命令**

```sh
mvn clean install -Dmaven.test.skip=true
```

### Ⅲ 生成微服务可运行 jar 包

1. **应用微服务打包插件**

   可以以 SpringBoot 微服务形式直接运行的 jar 包包括：

   - 当前微服务本身代码
   - 当前微服务所依赖的 jar 包
   - 内置 Tomcat（Servlet 容器）
   - 与 jar 包可以通过 java -jar 方式直接启动相关的配置

   要加入额外的资源、相关配置等等，仅靠 Maven 自身的构建能力是不够的，所以要通过 build 标签引入下面的插件。包括**user-auth-center和mysql-data-provider**模块.

   ```xml
   <!-- build 标签：用来配置对构建过程的定制 -->
   <build>
       <!-- plugins 标签：定制化构建过程中所使用到的插件 -->
   	<plugins>
           <!-- plugin 标签：一个具体插件 -->
   		<plugin>
   			<groupId>org.springframework.boot</groupId>
   			<artifactId>spring-boot-maven-plugin</artifactId>
   		</plugin>
   	</plugins>
   </build>
   ```

2. **执行插件目标**

   请对 module02-user-auth-center 和 module06-mysql-data-provider 都执行下面的命令：

   - clean 子命令：清理之前构建的结果
   - package 子命令：我们真正要调用的 spring-boot:repackage 要求必须将当前微服务本身的 jar 包提前准备好，所以必须在它之前执行 package 子命令。
   - spring-boot:repackage 子命令：调用 spring-boot 插件的 repackage 目标
   - -Dmaven.test.skip=true 参数：跳过测试

   ```sh
   mvn clean package spring-boot:repackage -Dmaven.test.skip=true
   ```

### Ⅲ 执行部署

**a> 启动 Nacos**

```sh
startup.cmd -m standalone
```

**b> 上传 jar 包**

**c> 启动微服务**

# 九、POM深入与强化

## 1、 重新认识 Maven

### Ⅰ Maven 的完整功能

在入门的时候我们介绍说 Maven 是一款『**构建**管理』和『**依赖**管理』的工具。但事实上这只是 Maven 的一部分功能。Maven 本身的产品定位是一款『**项目**管理工具』。

### Ⅱ 项目管理功能的具体体现

下面是 spring-boot-starter 的 POM 文件，可以看到：除了我们熟悉的坐标标签、dependencies 标签，还有 description、url、organization、licenses、developers、scm、issueManagement 等这些描述项目信息的标签。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd" xmlns="http://maven.apache.org/POM/4.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <!-- This module was also published with a richer model, Gradle metadata,  -->
  <!-- which should be used instead. Do not delete the following line which  -->
  <!-- is to indicate to Gradle or any Gradle module metadata file consumer  -->
  <!-- that they should prefer consuming it instead. -->
  <!-- do_not_remove: published-with-gradle-metadata -->
  <modelVersion>4.0.0</modelVersion>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter</artifactId>
  <version>2.5.6</version>
  <name>spring-boot-starter</name>
  <description>Core starter, including auto-configuration support, logging and YAML</description>
  <url>https://spring.io/projects/spring-boot</url>
  <organization>
    <name>Pivotal Software, Inc.</name>
    <url>https://spring.io</url>
  </organization>
  <licenses>
    <license>
      <name>Apache License, Version 2.0</name>
      <url>https://www.apache.org/licenses/LICENSE-2.0</url>
    </license>
  </licenses>
  <developers>
    <developer>
      <name>Pivotal</name>
      <email>info@pivotal.io</email>
      <organization>Pivotal Software, Inc.</organization>
      <organizationUrl>https://www.spring.io</organizationUrl>
    </developer>
  </developers>
  <scm>
    <connection>scm:git:git://github.com/spring-projects/spring-boot.git</connection>
    <developerConnection>scm:git:ssh://git@github.com/spring-projects/spring-boot.git</developerConnection>
    <url>https://github.com/spring-projects/spring-boot</url>
  </scm>
  <issueManagement>
    <system>GitHub</system>
    <url>https://github.com/spring-projects/spring-boot/issues</url>
  </issueManagement>

  <dependencies>
    <dependency>
      ……
    </dependency>
  </dependencies>
</project>
```

所以从『项目管理』的角度来看，Maven 提供了如下这些功能：

- 项目对象模型（POM）：将整个项目本身抽象、封装为应用程序中的一个对象，以便于管理和操作。
- 全局性构建逻辑重用：Maven 对整个构建过程进行封装之后，程序员只需要指定配置信息即可完成构建。让构建过程从 Ant 的『编程式』升级到了 Maven 的『声明式』。
- 构件的标准集合：在 Maven 提供的标准框架体系内，所有的构件都可以按照统一的规范生成和使用。
- 构件关系定义：Maven 定义了构件之间的三种基本关系，让大型应用系统可以使用 Maven 来进行管理
  - 继承关系：通过从上到下的继承关系，将各个子构件中的重复信息提取到父构件中统一管理
  - 聚合关系：将多个构件聚合为一个整体，便于统一操作
  - 依赖关系：Maven 定义了依赖的范围、依赖的传递、依赖的排除、版本仲裁机制等一系列规范和标准，让大型项目可以有序容纳数百甚至更多依赖
- 插件目标系统：Maven 核心程序定义抽象的生命周期，然后将插件的目标绑定到生命周期中的特定阶段，实现了标准和具体实现解耦合，让 Maven 程序极具扩展性
- 项目描述信息的维护：我们不仅可以在 POM 中声明项目描述信息，更可以将整个项目相关信息收集起来生成 HTML 页面组成的一个可以直接访问的站点。这些项目描述信息包括：
  - 公司或组织信息
  - 项目许可证
  - 开发成员信息
  - issue 管理信息
  - SCM 信息

## 2、 POM 的四个层次

### Ⅰ 超级 POM

经过我们前面的学习，我们看到 Maven 在构建过程中有很多默认的设定。例如：源文件存放的目录、测试源文件存放的目录、构建输出的目录……等等。但是其实这些要素也都是被 Maven 定义过的。定义的位置就是：**超级 POM**。

关于超级 POM，Maven 官网是这样介绍的：

> The Super POM is Maven's default POM. All POMs extend the Super POM unless explicitly set, meaning the configuration specified in the Super POM is inherited by the POMs you created for your projects.
>
> 译文：Super POM 是 Maven 的默认 POM。除非明确设置，否则所有 POM 都扩展 Super POM，这意味着 Super POM 中指定的配置由您为项目创建的 POM 继承。

所以我们自己的 POM 即使没有明确指定一个父工程（父 POM），其实也默认继承了超级 POM。就好比一个 Java 类默认继承了 Object 类。

### Ⅱ 父 POM

和 Java 类一样，POM 之间其实也是**单继承**的。如果我们给一个 POM 指定了父 POM，那么继承关系就是,  超级 POM <—继承— 父 POM <—继承— 当前 POM。

### Ⅲ 有效 POM

**a> 概念**

有效 POM 英文翻译为 effective POM，它的概念是这样的——在 POM 的继承关系中，子 POM 可以覆盖父 POM 中的配置；如果子 POM 没有覆盖，那么父 POM 中的配置将会被继承。按照这个规则，继承关系中的所有 POM 叠加到一起，就得到了一个最终生效的 POM。显然 Maven 实际运行过程中，执行构建操作就是按照这个最终生效的 POM 来运行的。这个最终生效的 POM 就是**有效 POM**，英文叫**effective POM**。

**b> 查看有效 POM**

```
mvn help:effective-pom
```

### Ⅳ 小结

综上所述，平时我们使用和配置的 POM 其实大致是由四个层次组成的：

- 超级 POM：所有 POM 默认继承，只是有直接和间接之分。
- 父 POM：这一层可能没有，可能有一层，也可能有很多层。
- 当前 pom.xml 配置的 POM：我们最多关注和最多使用的一层。
- 有效 POM：隐含的一层，但是实际上真正生效的一层。

## 3、 属性的声明与引用

### Ⅰ help 插件的各个目标

官网说明地址：https://maven.apache.org/plugins/maven-help-plugin

| 目标                    | 说明                                              |
| ----------------------- | ------------------------------------------------- |
| help:active-profiles    | 列出当前已激活的 profile                          |
| help:all-profiles       | 列出当前工程所有可用 profile                      |
| help:describe           | 描述一个插件和/或 Mojo 的属性                     |
| help:effective-pom      | 以 XML 格式展示有效 POM                           |
| help:effective-settings | 为当前工程以 XML 格式展示计算得到的 settings 配置 |
| **help:evaluate**       | 计算用户在交互模式下给出的 Maven 表达式           |
| help:system             | 显示平台详细信息列表，如系统属性和环境变量        |

### Ⅱ 用途

- 在当前 pom.xml 文件中引用属性
- 资源过滤功能：在非 Maven 配置文件中引用属性，由 Maven 在处理资源时将引用属性的表达式替换为属性值

## 4、 build 标签详解

### Ⅰ 概述

实际适用Maven过程中，build标签可以省略，但通过有效 POM 我们能够看到，build 标签的相关配置其实一直都在，只是在我们需要定制构建过程的时候才会通过配置 build 标签覆盖默认值或补充配置。这一点我们可以通过打印有效 POM 来看到。

所以**本质**上来说：我们配置的 build 标签都是对**超级 POM 配置**的**叠加**。在默认配置无法满足需求的时候**定制构建过程**。

### Ⅱ build标签的组成

**a> 定义约定的目录结构**

```xml
<sourceDirectory>D:\maven-ssm\src\main\java</sourceDirectory>
<scriptSourceDirectory>D:\maven-ssm\src\main\scripts</scriptSourceDirectory>
<testSourceDirectory>D:\maven-ssm\src\test\java</testSourceDirectory>
<outputDirectory>D:\maven-ssm\target\classes</outputDirectory>
<testOutputDirectory>D:\maven-ssm\target\test-classes</testOutputDirectory>
<resources>
    <resource>
        <directory>D:\maven-ssm\src\main\resources</directory>
    </resource>
</resources>
<testResources>
    <testResource>
        <directory>D:\maven-ssm\src\test\resources</directory>
    </testResource>
</testResources>
<directory>D:\maven-ssm\target</directory>
```

| 目录名                | 作用                       |
| --------------------- | -------------------------- |
| sourceDirectory       | 主体源程序存放目录         |
| scriptSourceDirectory | 脚本源程序存放目录         |
| testSourceDirectory   | 测试源程序存放目录         |
| outputDirectory       | 主体源程序编译结果输出目录 |
| testOutputDirectory   | 测试源程序编译结果输出目录 |
| resources             | 主体资源文件存放目录       |
| testResources         | 测试资源文件存放目录       |
| directory             | 构建结果输出目录           |

**b> 备用插件管理**

pluginManagement 标签存放着几个极少用到的插件：

- maven-antrun-plugin
- maven-assembly-plugin
- maven-dependency-plugin
- maven-release-plugin

通过 pluginManagement 标签管理起来的插件就像 dependencyManagement 一样，子工程使用时可以省略版本号，起到在父工程中统一管理版本的效果。起到在父工程中统一管理版本的效果。

- 被 spring-boot-dependencies 管理的插件信息：

```xml
<plugin>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-maven-plugin</artifactId>
	<version>2.6.2</version>
</plugin>
```

- 子工程使用的插件信息：

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

**c> 生命周期插件**

plugins 标签存放的是默认生命周期中实际会用到的插件：

```xml
<plugin>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.1</version>
    <executions>
        <execution>
            <id>default-compile</id>
            <phase>compile</phase>
            <goals>
                <goal>compile</goal>
            </goals>
        </execution>
        <execution>
            <id>default-testCompile</id>
            <phase>test-compile</phase>
            <goals>
                <goal>testCompile</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

1. 坐标部分

   artifactId 和 version 标签定义了插件的坐标，作为 Maven 的自带插件这里省略了 groupId。

2. 执行部分

   executions 标签内可以配置多个 execution 标签，execution 标签内：

   - id：指定唯一标识
   - phase：关联的生命周期阶段
   - goals/goal：关联指定生命周期的目标

   另外，插件目标的执行过程可以进行配置，例如 maven-site-plugin 插件的 site 目标：

   ```xml
   <execution>
       <id>default-site</id>
       <phase>site</phase>
       <goals>
           <goal>site</goal>
       </goals>
       <configuration>
           <outputDirectory>D:\maven-test-prepare\target\site</outputDirectory>
           <reportPlugins>
               <reportPlugin>
                   <groupId>org.apache.maven.plugins</groupId>
                   <artifactId>maven-project-info-reports-plugin</artifactId>
               </reportPlugin>
           </reportPlugins>
       </configuration>
   </execution>
   ```

   configuration 标签内进行配置时使用的标签是插件本身定义的。就以 maven-site-plugin 插件为例，它的核心类是 org.apache.maven.plugins.site.render.SiteMojo，在这个类中可以看到 **outputDirectory** 属性。

   SiteMojo 的父类是：AbstractSiteRenderingMojo，在父类中可以看到 reportPlugins 属性。

   因此：每个插件能够做哪些设置都是各个插件自己规定的。

### Ⅲ 典形应用：指定 JDK 版本

**a> 需求**

在 settings.xml 中配置了 JDK 版本，那么将来把 Maven 工程部署都服务器上，脱离了 settings.xml 配置，如何保证程序正常运行？

可以直接把 JDK 版本信息告诉负责编译操作的 maven-compiler-plugin 插件，让它在构建过程中，按照我们指定的信息工作。

**b> 暂时取消 settings.xml 配置**

为了测试对 maven-compiler-plugin 插件进行配置的效果，我们暂时取消 settings.xml 中的 profile 配置。

```xml
<!-- 配置Maven工程的默认JDK版本 -->
<!-- <profile>
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
</profile> -->
```

**c> 配置构建过程**

```xml
<!-- build 标签：意思是告诉 Maven，你的构建行为，我要开始定制了！ -->
<build>
    <!-- plugins 标签：Maven 你给我听好了，你给我构建的时候要用到这些插件！ -->
    <plugins>
        <!-- plugin 标签：这是我要指定的一个具体的插件 -->
        <plugin>
            <!-- 插件的坐标。此处引用的 maven-compiler-plugin 插件不是第三方的，是一个 Maven 自带的插件。 -->
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.1</version>
            
            <!-- configuration 标签：配置 maven-compiler-plugin 插件 -->
            <configuration>
                <!-- 具体配置信息会因为插件不同、需求不同而有所差异 -->
                <source>1.8</source>
                <target>1.8</target>
                <encoding>UTF-8</encoding>
            </configuration>
        </plugin>
    </plugins>
</build>
```

| source | 提供与指定发行版的源兼容性 |
| ------ | -------------------------- |
| target | 生成特点 VM 版本的类文件   |

相较于settings.xml配置的优势是：无论在哪个环境执行编译等构建操作都有效。settings.xml 中配置仅在本地生效。

### Ⅳ 典型应用：SpringBoot 定制化打包

**a> 需求**

spring-boot-maven-plugin 并不是 Maven 自带的插件，而是 SpringBoot 提供的，用来改变 Maven 默认的构建行为。具体来说是改变打包的行为。默认情况下 Maven 调用 maven-jar-plugin 插件的 jar 目标，生成普通的 jar 包。

普通 jar 包没法使用 java -jar xxx.jar 这样的命令来启动、运行，但是 SpringBoot 的设计理念就是每一个『微服务』导出为一个 jar 包，这个 jar 包可以使用 java -jar xxx.jar 这样的命令直接启动运行。

这样一来，打包的方式肯定要进行调整。所以 SpringBoot 提供了 spring-boot-maven-plugin 这个插件来定制打包行为。

**b> 配置**

```xml
<build>
	<plugins>
		<plugin>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-maven-plugin</artifactId>
			<version>2.5.5</version>
		</plugin>
	</plugins>
</build>
```

**c> 插件的七个目标**

| 目标名称                  | 作用                                                         |
| ------------------------- | ------------------------------------------------------------ |
| spring-boot:build-image   | 使用构建包将应用程序打包到 OCI image 中。                    |
| spring-boot:build-info    | 根据当前 MavenProject 的内容生成一个内部版本信息属性文件(build-info.properties)。 |
| spring-boot:help          | 显示有关 spring-boot-maven-plugin 的帮助信息. 调用 mvn spring-boot:help -Ddetail=true -Dgoal=<goal-name> 以显示参数详细信息。 |
| **spring-boot:repackage** | 重新打包现有的 JAR 和 WAR 归档文件，以便可以使用 java -jar 从命令行执行它们。使用 layout=NONE 也可以简单地用于打包具有嵌套依赖项的 JAR（并且没有主类，因此不可执行）。 |
| spring-boot:run           | Run an application in place.                                 |
| spring-boot:start         | 启动 Spring 应用。与运行目标相反，这不会阻塞并允许其他目标在应用程序上运行。此目标通常用于集成测试方案，其中应用程序在测试套件之前启动，在测试套件之后停止。 |
| spring-boot:stop          | 停止已由“启动”目标启动的应用程序。通常在测试套件完成后调用。 |



### Ⅴ 典型应用：Mybatis 逆向工程

使用 Mybatis 的逆向工程需要使用如下配置，MBG 插件的特点是需要提供插件所需的依赖：

```xml
<!-- 控制 Maven 在构建过程中相关配置 -->
<build>
		
	<!-- 构建过程中用到的插件 -->
	<plugins>
		
		<!-- 具体插件，逆向工程的操作是以构建过程中插件形式出现的 -->
		<plugin>
			<groupId>org.mybatis.generator</groupId>
			<artifactId>mybatis-generator-maven-plugin</artifactId>
			<version>1.3.0</version>
	
			<!-- 插件的依赖 -->
			<dependencies>
				
				<!-- 逆向工程的核心依赖 -->
				<dependency>
					<groupId>org.mybatis.generator</groupId>
					<artifactId>mybatis-generator-core</artifactId>
					<version>1.3.2</version>
				</dependency>
					
				<!-- 数据库连接池 -->
				<dependency>
					<groupId>com.mchange</groupId>
					<artifactId>c3p0</artifactId>
					<version>0.9.2</version>
				</dependency>
					
				<!-- MySQL驱动 -->
				<dependency>
					<groupId>mysql</groupId>
					<artifactId>mysql-connector-java</artifactId>
					<version>5.1.8</version>
				</dependency>
			</dependencies>
		</plugin>
	</plugins>
</build>
```

## **5、 依赖配置补充**

### Ⅰ 依赖范围

**a> import**

管理依赖最基本的办法是继承父工程，但是和 Java 类一样，Maven 也是单继承的。如果不同体系的依赖信息封装在不同 POM 中了，没办法继承多个父工程怎么办？这时就可以使用 import 依赖范围。

典型案例就是在项目中引入 SpringBoot、SpringCloud 依赖：

```xml
<dependencyManagement>
    <dependencies>

        <!-- SpringCloud 依赖导入 -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>Hoxton.SR9</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>

        <!-- SpringCloud Alibaba 依赖导入 -->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-alibaba-dependencies</artifactId>
            <version>2.2.6.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>

        <!-- SpringBoot 依赖导入 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-dependencies</artifactId>
            <version>2.3.6.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

import 依赖范围使用要求：

- 打包类型必须是 pom
- 必须放在 dependencyManagement 中

**b> system**

以 Windows 系统环境下开发为例，假设现在 D:\maven-test-aaa-1.0-SNAPSHOT.jar 想要引入到我们的项目中，此时就可以将依赖配置为 system 范围：

```xml
<dependency>
    <groupId>com.atguigu.maven</groupId>
    <artifactId>atguigu-maven-test-aaa</artifactId>
    <version>1.0-SNAPSHOT</version>
    <systemPath>D:\maven-test-aaa-1.0-SNAPSHOT.jar</systemPath>
    <scope>system</scope>
</dependency>
```

这样引入依赖完全不具有可移植性，所以**不建议使用**。

**c> runtime**

专门用于编译时不需要，但是运行时需要的 jar 包。比如：编译时我们根据接口调用方法，但是实际运行时需要的是接口的实现类。典型案例是：

```xml
<!--热部署 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <scope>runtime</scope>
    <optional>true</optional>
</dependency>
```

### Ⅱ 可选依赖

可选其实就是『可有可无』。如上述 热部署。用标签 <optional>true</optional> 控制

其核心含义是：Project X 依赖 Project A，A 中一部分 X 用不到的代码依赖了 B，那么对 X 来说 B 就是『可有可无』的。

### Ⅲ 版本仲裁

**a> 最短路径优先**

在下图的例子中，对模块 pro25-module-a 来说，Maven 会采纳 1.2.12 版本。

![image-20220929153925098](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202209291539994.png)

**b> 路径相同时先声明者优先**

![image-20220929154022460](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202209291540803.png)

此时 Maven 采纳哪个版本，取决于在 pro29-module-x 中，对 pro30-module-y 和 pro31-module-z 两个模块的依赖哪一个先声明。

**c> 总结**

其实 Maven 的版本仲裁机制只是在没有人为干预的情况下，自主决定 jar 包版本的一个办法。而实际上我们要使用具体的哪一个版本，还要取决于项目中的实际情况。所以在项目正常运行的情况下，jar 包版本可以由 Maven 仲裁，不必我们操心；而发生冲突时 Maven 仲裁决定的版本无法满足要求，此时就应该由程序员明确指定 jar 包版本。

## **6、 Maven 自定义插件**

只做了解，略

[Maven 自定义插件](http://heavy_code_industry.gitee.io/code_heavy_industry/pro002-maven/chapter09/verse06.html#_1、本节定位)

## **7、 profile 详解**

### Ⅰ profile概述

**a> 项目的不同运行环境**

![images](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/maven/202209291635873.png)

通常情况下，我们至少有三种运行环境：

- 开发环境：供不同开发工程师开发的各个模块之间互相调用、访问；内部使用

- 测试环境：供测试工程师对项目的各个模块进行功能测试；内部使用

- 生产环境：供最终用户访问——所以这是正式的运行环境，对外提供服务

- 而我们这里的『环境』仍然只是一个笼统的说法，实际工作中一整套运行环境会包含很多种不同服务器：

  - MySQL

  - Redis

  - ElasticSearch

  - RabbitMQ

  - FastDFS

  - Nginx

  - Tomcat

  - ……

就拿其中的 MySQL 来说，不同环境下的访问参数肯定完全不同：

| 开发环境                                                     | 测试环境                                                     | 生产环境                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
  | dev.driver=com.mysql.jdbc.Driver dev.url=jdbc:mysql://124.71.36.17:3306/db-sys dev.username=root dev.password=123456 | test.driver=com.mysql.jdbc.Driver test.url=jdbc:mysql://124.71.36.89:3306/db-sys test.username=dev-team test.password=123456 | product.driver=com.mysql.jdbc.Driver product.url=jdbc:mysql://39.107.88.164:3306/prod-db-sys product.username=root product.password=123456 |

可是代码只有一套。如果在 jdbc.properties 里面来回改，那就太麻烦了，而且很容易遗漏或写错，增加调试的难度和工作量。所以最好的办法就是把适用于各种不同环境的配置信息分别准备好，部署哪个环境就激活哪个配置。

在 Maven 中，使用 profile 机制来管理不同环境下的配置信息。但是解决同类问题的类似机制在其他框架中也有，而且从模块划分的角度来说，持久化层的信息放在构建工具中配置也违反了『高内聚，低耦合』的原则。

所以 Maven 的 profile 我们了解一下即可，不必深究。

**b> profile 声明和使用的基本逻辑**

- 首先为每一个环境声明一个 profile
  - 环境 A：profile A
  - 环境 B：profile B
  - 环境 C：profile C
  - ……
- 然后激活某一个 profile

**c> 默认 profile**

其实即使在 pom.xml 中不配置 profile 标签，也已经用到 profile。因为根标签 project 下所有标签相当于都是在设定默认的 profile。这样一来我们也就很容易理解下面这句话：project 标签下除了 modelVersion 和坐标标签之外，其它标签都可以配置到 profile 中。

### Ⅱ profile配置

**a> 外部视角：配置文件**

从外部视角来看，profile 可以在下面两种配置文件中配置：

- settings.xml：全局生效。其中我们最熟悉的就是配置 JDK 1.8。
- pom.xml：当前 POM 生效



**b> 内部实现：具体标签**

从内部视角来看，配置 profile 有如下语法要求：

1.  **profiles/profile 标签**

   - 由于 profile 天然代表众多可选配置中的一个所以由复数形式的 profiles 标签统一管理。
   - 由于 profile 标签覆盖了 pom.xml 中的默认配置，所以 profiles 标签通常是 pom.xml 中的最后一个标签。

2. **id 标签**

   每个 profile 都必须有一个 id 标签，指定该 profile 的唯一标识。这个 id 标签的值会在命令行调用 profile 时被用到。这个命令格式是：-D<profile id>。

3. **其它允许出现的标签**

   一个 profile 可以覆盖项目的最终名称、项目依赖、插件配置等各个方面以影响构建行为。

   - build
     - defaultGoal
     - finalName
     - resources
     - testResources
     - plugins
   - reporting
   - modules
   - dependencies
   - dependencyManagement
   - repositories
   - pluginRepositories
   - properties

### Ⅲ 激活

**a> 默认配置默认被激活**

POM 中没有在 profile 标签里的就是默认的 profile，当然默认被激活。

**b> 基于环境信息激活**

环境信息包含：JDK 版本、操作系统参数、文件、属性等各个方面。一个 profile 一旦被激活，那么它定义的所有配置都会覆盖原来 POM 中对应层次的元素。可以参考如下的标签结构：

```xml
<profile>
	<id>dev</id>
    <activation>
        <!-- 配置是否默认激活 -->
    	<activeByDefault>false</activeByDefault>
        <jdk>1.8</jdk>
        <os>
        	<name>Windows XP</name>
            <family>Windows</family>
            <arch>x86</arch>
            <version>5.1.2600</version>
        </os>
        <property>
        	<name>mavenVersion</name>
            <value>2.0.5</value>
        </property>
        <file>
        	<exists>file2.properties</exists>
            <missing>file1.properties</missing>
        </file>
    </activation>
</profile>
```

> TIP
>
> 若配置jdk版本为1.8，则以1.8开头的版本都符合条件



**c> 命令行激活**

1. **列出活动的 profile**

   ```sh
   # 列出所有激活的 profile，以及它们在哪里定义
   mvn help:active-profiles
   ```

2. **指定某个具体 profile**

   ```xml
   mvn compile -P<profile id>
   ```

### Ⅳ 资源属性过滤

**a> 简介**

Maven 为了能够通过 profile 实现各不同运行环境切换，提供了一种『资源属性过滤』的机制。通过属性替换实现不同环境使用不同的参数。

**b> 操作演示**

1. **配置 profile**

   ```xml
   <profiles>
       <profile>
           <id>devJDBCProfile</id>
           <properties>
               <dev.jdbc.user>root</dev.jdbc.user>
               <dev.jdbc.password>123456</dev.jdbc.password>
               <dev.jdbc.url>http://localhost:3306/maven</dev.jdbc.url>
               <dev.jdbc.driver>com.mysql.cj.jdbc.Driver</dev.jdbc.driver>
           </properties>
           <build>
               <resources>
                   <resource>
                       <!-- 表示为这里指定的目录开启资源过滤功能 -->
                       <directory>src/main/resources</directory>
   
                       <!-- 将资源过滤功能打开 -->
                       <filtering>true</filtering>
                   </resource>
               </resources>
           </build>
       </profile>
   </profiles>
   ```

2. **待处理的文件 jdbc.properties** 

   ```properties
   dev.user=${dev.jdbc.user}
   dev.password=${dev.jdbc.password}
   dev.url=${dev.jdbc.url}
   dev.driver=${dev.jdbc.driver}
   ```

3. **执行处理资源命令**

   ```sh
   mvn clean resources:resources -PdevJDBCProfile
   ```

4. **处理结果**

   target中生成的 **jdbc.properties** 将填入profile中配置内容

   ```properties
   dev.user=root
   dev.password=123456
   dev.url=http://localhost:3306/maven
   dev.driver=com.mysql.cj.jdbc.Driver
   ```

5. **延伸**

   使用includes和excludes锁定目标文件

   - includes：指定执行 resource 阶段时要包含到目标位置的资源
   - excludes：指定执行 resource 阶段时要排除的资源

# **十、生产实践**

## **1、 搭建 Maven 私服：Nexus**

## **2、 jar 包冲突问题**

## **3、 体系外 jar 包引入**
