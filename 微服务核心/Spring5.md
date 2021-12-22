[TOC]

# 一、Spring概念

1. ##### Spring是轻量级的开源的JavaEE框架。

2. ##### Spring可以解决企业应用开发的复杂性。

3. ##### Spring有两个核心部分，IOC和Aop:

   - ##### IOC:控制反转，把创建对象过程交给Spring进行管理。

   - ##### Aop:面向切面编程，不修改源代码进行功能增强。

4. **Spring特点（使用Spring的好处）**

   - **轻量：**Spring 是轻量的，基本的版本大约2MB

   - **控制反转：**Spring通过控制反转实现了松散耦合，对象们给出它们的依赖，而不是创建或查找依赖的对象们。方便解耦，简化开发。

   - **面向切面的编程(AOP)：**Spring支持面向切面的编程，并且把应用业务逻辑和系统服务分开。

   - **容器：**Spring 包含并管理应用中对象的生命周期和配置。

   - **MVC框架：**Spring的WEB框架是个精心设计的框架，是Web框架的一个很好的替代品。

   - **方便事务管理：**Spring 提供一个持续的事务管理接口，可以扩展到上至本地事务下至全局事务（JTA）

   - **异常处理：**Spring 提供方便的API把具体技术相关的异常（比如由JDBC，Hibernate or JDO抛出的）转化为一致的unchecked 异常。

5. [入门案例](https://www.cnblogs.com/zwwhnly/p/10448434.html)

# 二、IOC容器

## 1、IOC底层原理

### Ⅰ 什么是IOC

1. 控制反转（Inversion of Control，缩写为**IoC**），把对象创建和对象之间的调用过程交给Spring进行管理。**Spring 框架的核心是 Spring 容器。容器创建对象，将它们装配在一起，配置它们并管理它们的完整生命周期。**Spring 容器使用**依赖注入**来管理组成应用程序的组件。容器通过读取提供的配置元数据来接收对象进行实例化，配置和组装的指令。该元数据可以通过 XML，Java 注解或 Java 代码提供。
2. 使用目的：为了降低耦合度。
3. 涉及技术：xml解析、工厂模式、反射。

## 2、IOC接口（BeanFactory）

1. IOC思想基于IOC容器完成，IOC容器底层就是对象工厂。
2. Spring提供IOC容器实现两种方式：（两个接口）
   - BeanFactory:
     - IOC容器基本实现，是Spring内部的使用接口，不提供开发人员进行使用。
     - 加载配置文件时不会创建对象，在获取对象（使用）时采取创建对象。
   - ApplicationContext:
     - BeanFactory接口的子接口，提供更多更强大的功能，一般由开发人员进行使用。
     - 加载配置文件时就把配置文件对象进行创建。

3. ApplicationContext两个重要实现类：
   - FileSystemXmlApplicationContext：路径包含盘符的全路径。
   - ClassPathXmlApplicationContext：src目录下。

## 3、IOC操作Bean管理（基于XML）

### Ⅰ 什么是Bean管理

1. Spring创建对象。
2. Spring注入属性。

### Ⅱ Bean管理操作有两个操作

1. 基于xml配置文件方式实现。
2. 基于注解方式实现。

### Ⅲ 基于xml

1. **创建对象**

   - 在spring配置文件中，使用**bean标签**，标签里添加对应属性，就可以实现对象创建。

   - bean标签中的常用属性：

     - id：唯一标识。

     - class：类全路径（包、类路径）。
     - name：标识名称。
     - ref：可用于注入外部bean（类为成员变量时）。

   - **创建对象时候，默认也是执行无参构造器。**

2. **注入属性**

   - DI：依赖注入，就是注入属性。通常可通过构造器、setter、接口实现。
   - spring框架中，**仅使用有参构造器和 setter 注入**。
   - 注入属性两种常用方法，xml配置文件中，在<bean>标签中：
     - <property>标签**，创建对象时，直接配置对应类的属性值，name=属性名称，value=注入值。**
     - <constructor-arg>标签**，配置对应类的有参构造器配置。**
   - 注入特殊类型属性：
     - **注入外部bean：标签中使用 ref** 属性通过外部bean的**id**引入。常用于类中包含其他类类型的属性时。
     - 内部bean：使用情况与外部bean注入相同，使用方法是直接在属性**<property>标签**中嵌套bean标签。不常用，因为通过引用的方式，写在外层的bean，可复用。
     - 级联赋值：就是bean标签中**调用**其他bean，且**对其属性赋值**。
     - 注入集合属性：由于有多项数据，不能直接value赋值。
       - 对于**数组等**单列数据，可用<list>或<array>标签包裹多个<value>
       - 对于**set**，<set><value>...</value>...</set>
       - 对于**map**等双列数据，<map><entry key=" " value=" "></entry>...</map>
       - 可以<util:list id="bookList"><value>...</util>等的方式配置一个全局集合，可重复引用。
   - 当value值中包含特殊符号（如<>）：
     - 用转义字符如&lt，标签语言可查。
     - 数据写进CDATA中：PCDATA 指的是**被解析的字符数据**。 CDATA 指的是**不应由 XML 解析器进行解析的文本数据**。CDATA 部分如 **<![CDATA[** 包含转义字符<>,不应被解析的文本数据 **]]>**。

3. 工厂bean
   - spring中有两种bean，一种是普通的，一种是工厂bean（FactoryBean）
   - 两种bean区别：
     - 普通bean：在xml配置文件中定义bean类型就是返回类型。
     - 工厂bean：定义类型和返回类型可以不一样。
   - 工厂bean的使用：
     - 创建**FactoryBean接口**的**实现类，**作为工厂bean。
     - 实现三个方法，getObject(), getObjectType(), isSingleton()。返回类型由重写的方法 getObject() 中定义。

4. bean的作用域（设置单例、多例）

   - 默认bean为单实例对象，在类中创建，赋值多个对象，对象**地址**也一样。
   - scope属性值：
     - 默认singleton：单例。加载配置文件时就会创建单例对象。
     - prototype：多实例对象。调用getBean()方法时创建对象。
     - request：放在请求域中。
     - session：放在会话域中。

5. bean的**生命周期**

   - 加载配置文件：

   - ```java
     ApplicationContext context
     new ClassPathXmlApplicationContext bean4.xml;
     ClassPathXmlApplicationContext context =
     new ClassPathXmlApplicationContext ("bean4.xml");
     ```

   - 执行无参构造器创造实例。

   - 设置bean的属性值和其他bean的引用（调用setter）。

   - **将bean实例传递给bean后置处理器的**postProcess**Before**Initialization() **方法。（加后置处理器时，在调用初始化步骤前后）**

   - **自动调用**bean的**初始化方法**（需要在bean标签中配置**init-method="initMethod()"属性**和在类中定义初始化initMethod()方法）

   - **将bean实例传递给bean后置处理器的**postProcess**After**Initialization() **方法。（加后置处理器时）**

   - 使用bean，通过调用 context.getBean() 方法时获取对象。

   - 当容器关闭时，((ClassPathxmlApplicationContext) context).close ()；**手动调用**bean的销毁的方法（需要在bean标签中配置**destroy-method="destroyMethod()"属性**和在类中定义**destroyMethod()**销毁方法）.

6. 后置处理器

   - 若存在，则在执行时期在生命周期中调用初始化方法的前后。
   - 创建实现类实现BeanPostProcessor接口中的postProcess**Before**Initialization（Object bean，String beanName）和 postProcess**After**Initialization（Object bean，String beanName）方法。
   - xml中配置：与配置普通的bean相同。

7. xml的自动装配

   - 根据指定装配规则（属性名或属性类型），Spring自动将匹配的属性值进行注入。
   - 自动装配的过程：<bean>标签中属性autowire="..."。可设值：byName：根据名称自动装配。byType：根据属性类型自动注入。
   - 不常用，一般通过注解自动装配。

8. 外部属性文件（常用于配置连接池）

   - 直接配置：与配置bean类似，将驱动器、url、username、password通过<property>设置value属性值。
   - [外部引用](https://blog.csdn.net/qq_41865229/article/details/120758434)：第一步:引入context命名空间。第二步:引入外部属性文件。第三步:引用外部属性文件中的属性值。

## 4、IOC操作Bean管理（基于注解）

### Ⅰ 什么是注解

1. 注解是代码的特殊标记，格式：@注解名称(属性名=属性值，...)
2. 使用注解，定义在类、方法、属性上。
3. 目的：简化xml配置。

### Ⅱ Spring针对Bean管理中的创建对象提供注解

1. @**Controller** 控制器（注入服务）用于标注控制层，相当于struts中的action层。
2. @**Service** 服务（注入dao）用于标注服务层，主要用来进行业务的逻辑处理。
3. @**Rrepository**（实现dao访问）用于标注数据访问层，也可以说用于标注数据访问组件，即DAO组件
4. @**Component** （把普通pojo实例化到spring容器中，相当于配置文件中的)泛指各种组件，就是说当我们的类不属于各种归类的时候（不属于@Controller、@Services等的时候），我们就可以使用@Component来标注这个类。
5. 前四个注解**都可以用来创建bean实例**。

### Ⅲ 基于注解方式实现对象创建

1. 第一步：引入依赖（spring-aop-5.2.9.RELEASE.jar）。
2. 第二部：[开启组件扫描](https://blog.csdn.net/weixin_43883917/article/details/112747184)。先引入context命名空间。再配置扫描范围（<context:component-scan base-package="包名"> ... </context:component-scan>）。**默认的情况就是都扫描,可配置扫描的文件列表和不扫描的文件列表**。
3. 第三步：使用注解修饰代码结构。

### Ⅳ 基于注解方法实现属性注入

1. @**Autowired：**自动根据类型注入，自动装配常用注解。
2. @**Qualifier(value="名称")**：指定自动注入的id名称，**与上个注解一起使用**，适用于接口有多个实现类时，指定具体实现类id。
3. @Resource(“名称”)：默认无属性根据类型注入，加属性name="类id"，可根据名称注入。区别，非spring框架包中的，而是javax中的。
4. @Value(value=...)：注入普通类型属性，注入实际值。使用：直接在类属性上修饰。使用后就不用在xml中配置注入了。

### Ⅴ 完全注解开发

1. 创建配置类，替代xml文件。

   ```java
   @Configuration//作为配置类，替代xml配置文件
   @ComponentScan(basePackages com.atguigu }
   public class SpringConfig
   ```

2. 编写测试类：加载配置时，获取的是AnnotationConfigApplicationContext(配置类.class)实例，且不是加载xml文件。

# 三、AOP（面向切面编程）

## 1、概念

- 面向切面编程，利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的**耦合度降低**，提高程序的可复用性，同时提高了开发的效率。
- 简介的说：**AOP允许不通过修改源代码方式，在主干功能里添加新功能。**
- 举例（登录功能）：
  - 常规基本流程：输入用户名密码 -> 数据库查询 -> 验证 -- 成功 -->主页面（否则回到登陆页面）
  - 若需要添加权限判断功能，原始方式需要修改源代码。而AOP可不通过修改源代码方式添加新的功能（添加秦安判断模块，配置进去）

## 2、AOP的底层原理

### Ⅰ AOP底层使用动态代理

1. 有两种情况的动态代理：
   - **有接口**情况，使用JDK动态代理，创建被代理类共同接口实现类的代理类。
   - **没有接口**情况，使用CGLIB动态代理：创建继承 被代理类 的父类的代理类。
   - CGLIB（Code Genneration Library）是一个功能强大，高性能的代码生成包。它为没有实现接口的类提供代理，为JDK的动态代理提供了很好的补充。通常可以使用Java的动态代理创建代理，但当要代理的类没有实现接口或者为了更好的性能，CGLIB是一个好的选择。

## 3、JDK动态代理

1. 使用JDK动态代理，使用java.lang.reflect.Proxy类里面的方法创建代理对象。

   - 调用newProxyInstance方法

   - | static object | newProxyInstance(ClassLoader loader,类<？>[]interfaces，InvocationHandler h) | 返回指定接口的代理实例，该接口将方法调用分派给指定的调用处理程序。 |
     | ------------- | :----------------------------------------------------------- | ------------------------------------------------------------ |

     

   - 参数一：类加载器

   - 参数二：增强方法所爱类，这个类实现的接口，支持多个接口。

   - 参数三： 实现这个接口Invocationhander，创建代理对象，写增强的方法。可用匿名实现类方式，也可单独写个实现类实现Invocationhandler接口。

2. [编写动态代理代码](https://www.jianshu.com/p/269afd0a52e6)。

   - 属性：被代理类对象和返回被代理类的方法。
   - 实现invoke方法：public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {}

## 4、AOP术语

1. 连接点：类里面那些方法**可以被增强**，这些方法称为连接点。
2. 切入点：**实际被真正增强**的方法，称为切入点。
3. 通知（增强）：
   - 实际增强的逻辑部分曾为通知（增强）。
   - 通知有多种类型：
     - 前置通知：被增强方法之前执行。
     - 后置通知：之后执行。
     - 环绕通知：前后都执行。
     - 异常通知：出现异常时执行。
     - 最终通知：类似finally，无论有无异常，都会执行。
4. 切面：是动作过程，把通知应用到切入点的过程。

## 5、 AOP操作（准备）

### Ⅰ什么是AspectJ

1. Spring框架一般都是基于AspectJ实现AOP操作。
2. AspectJ不是Spring组成部分，独立AOP框架，一般AspectJ和Spring框架一起使用，进行AOP操作。

### Ⅱ 基于AspectJ实现AOP操作

1. 基于**xml配置**文件实现：

   - 创建两个类，增强类和被增强类，创建方法。

   - 在spring配置文件中创建两个类对象。

   - 在spring配置文件中配置AOP增强，利用<aop:config>:

   - ```xml
     <!--配置aop增强-->
     <aop:config>
     	<!--切入点-->
     	<aop:pointcut id="p"expression="execution(*com.atguigu.spring5.aopxml.Book.buy(..))"/>
     	<!--配置切面-->
     	<aop:aspect ref="bookProxy">
     		<!--增强作用在具体的方法上-->
     		<aop:before method="before" pointcut-ref="p"/>
     	</aop:aspect>
     </aop:config>
     ```

     

2. 基于**注解方式**实现（使用）：

   - 创建被增强类类，在类里定义方法。
   - 创建增强类（编写增强逻辑）在增强类里，创建方法，让不同方法代表不同通知类型，如before()代表前置通知。
   - 进行配置通知
     - 在spring配置文件中，开启注解扫描。
     - 使用注解创建被代理类和代理类对象(如@Comment)。
     - 在增强类上面注解@Aspect。
     - 在spring配置文件中开启生成代理对象（标签：<aop:aspectj-autoproxy></aop......>）。
   - 配置**不同类型的通知**
     - 在增强类的里面，在作为通知方法上面添加通知类型注解，使用切入点表达式配置。
     - 如配置前置通：@Before(value="execution(*com.senmu.dao.BookDao.add(..))")*
     - **通知注解类型**：@**After**：不管有无异常都执行，可称为**最终通知**；@Around：环绕；@AfterThrowing：异常；@**AfterReturning**：返回值之后执行，可称为**后置通知，一般在After之后执行**。
     - **相同切入点抽取**：可用@pointcut(value="excution(通用部分)") 注解 切入点抽取方法（如pointCut()），返回注解中相同的切入点表达式。则通知注解就可写成@Before(value="pointCut()")
     - 若有多个增强类增强同一个方法，可**设置优先级**。@Order（数字型值），越小优先级越高。
     - **可实现完全注解开发，可在配置类中配置AspectJ。**

3. 在项目工程里引入依赖

4. 切入点表达式：

   - 切入点表达式作用：知道对哪个类里面的哪个方法进行增强。
   - 语法结构：
     - execution([权限修饰符] [返回类型] [类全路径] [方法名称] [参数列表])
     - 案例：对com.senmu.dao.BookDao类里的add()进行增强：execution(*com.senmu.dao.BookDao.add(..))

# 四、JdbcTemplate

## 1、什么是JdbcTeamplate

1. Spring框架对JDBC进行封装，使用JdbcTempate方便实现对数据库操作。
2. [准备工作](https://www.cnblogs.com/caoyc/p/5630622.html)
   - 引入相关jar包
   - 在Spring配置文件**配置数据库连接池**
   - 配置JdbcTemlate对象，注入DataSource。
   - 创建service类，注入dao，创建dao类，在dao中注入JdbcTeamplate。

## 2、JdbcTemplate操作数据库

1.  对应数据库创建实体类（entity）:放在entity下的beans。
2.  编写service和dao:在dao中进行数据库操作。

# 五、事物管理



# 六、Spring5新特性
