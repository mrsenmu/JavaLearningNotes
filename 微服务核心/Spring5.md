[TOC]

# 一、Spring概念

1. Spring是轻量级的开源的JavaEE框架。
2. Spring可以解决企业应用开发的复杂性。
3. Spring有两个核心部分，IOC和Aop:
   - IOC:控制反转，把创建对象过程交给Spring进行管理。
   - Aop:面向切面编程，不修改源代码进行功能增强。
4. **Spring特点**（使用Spring的好处）
   - **轻量**:Spring 是轻量的 ，基本的版本大约2MB
   - **控制反转**：Spring通过控制反转实现了松散耦合，对象们给出它们的依赖，而不是创建或查找依赖的对象们。方便解耦，简化开发。
   - **面向切面的编程(AOP)**：Spring支持面向切面的编程，并且把应用业务逻辑和系统服务分开。
   - **容器**：Spring 包含并管理应用中对象的生命周期和配置。
   - **MVC框架**:Spring的WEB框架是个精心设计的框架，是Web框架的一个很好的替代品。
   - **方便事务管理**：Spring 提供一个持续的事务管理接口，可以扩展到上至本地事务下至全局事务（JTA）
   - **异常处理**：Spring 提供方便的API把具体技术相关的异常（比如由JDBC，Hibernate or JDO抛出的）转化为一致的unchecked 异常。
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

1. 基于**xml配置**文件方式实现。
2. 基于**注解**方式实现。

### Ⅲ 基于xml

1. **创建对象**

   - 在spring配置文件中，使用**bean标签**，标签里添加对应属性，就可以实现对象创建。

     ```xml
     <bean id="user" class "com.xxx.spring5.User"></bean>
     ```

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
       - 对于**列表或数组等**单列数据，可用<list>或<array>标签包裹多个<value>
       - 对于**set**，<set><value>...</value>...</set>
       - 对于**map**等双列数据，<map><entry key=" " value=" "></entry>...</map>
       - 可以<util:list id="bookList"><value>...</util>等的方式配置一个全局集合，可重复引用。
   - 当value值中包含特殊符号（如<>）：
     - 用转义字符如&lt，标签语言可查。
     - 数据写进CDATA中：PCDATA 指的是**被解析的字符数据**。 CDATA 指的是**不应由 XML 解析器进行解析的文本数据**。CDATA 部分如 **<![CDATA[** 包含转义字符<>,不应被解析的文本数据 **]]>**。

3. 工厂bean
   - spring中有两种bean，一种是普通的，一种是工厂bean**（FactoryBean）**
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

5. bean的**生命周期**(正常5个步骤，有前置、后置处理时7个步骤)

   1. 执行无参构造器创造实例。

   2. 设置bean的属性值和对其他bean的引用（调用setter）。

   3. 将bean实例传递给bean**前置处理器**的postProcessBeforeInitialization() 方法。（存在前置处理器时，在调用初始化步骤前后）

   4. 自动调用bean的**初始化**方法（需要在bean标签中配置init-method="initMethod()"属性和在类中定义初始化initMethod()方法）

   5. 将bean实例传递给bean**后置处理器**的postProcessAfterInitialization() 方法。（存在后置处理器时）

   6. 使用bean，通过调用 context.getBean() 方法时获取对象。

   7. 当容器关闭时，((ClassPathxmlApplicationContext) context).close ()；手动调用bean的销毁的方法（需要在bean标签中配置destroy-method="destroyMethod()"属性和在类中定义destroyMethod()销毁方法).

6. 生命周期演示

   ```xml
   <!--配置Bean-->
   <bean id="orders" class="com.atguigu.spring5.bean.Orders" init-method="initMethod" destroy-method="destroyMethod">
        <property name="oname" value="手机"></property>
   </bean>
       
   <!--配置后置处理器--> 
   <bean id="myBeanPost" class="com.atguigu.spring5.bean.MyBeanPost"></bean>
   ```

   ```java
   public class Orders {
        //无参数构造
        public Orders() {
        	System.out.println("第一步 执行无参数构造创建 bean 实例");
        }
        private String oname;
        public void setOname(String oname) {
            this.oname = oname;
            System.out.println("第二步 调用 set 方法设置属性值");
        }
        //创建执行的初始化的方法
        public void initMethod() {
        	System.out.println("第三步 执行初始化的方法");
        }
        //创建执行的销毁的方法
        public void destroyMethod() {
        	System.out.println("第五步 执行销毁的方法");
        }
   }
   
   //   创建类，实现接口 BeanPostProcessor，创建后置处理器
   public class MyBeanPost implements BeanPostProcessor {
    	@Override
   	public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
            System.out.println("在初始化之前执行的方法");
            return bean;
    	}
   	@Override
   	public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
            System.out.println("在初始化之后执行的方法");
            return bean;
   	} 
   }
   
   @Test
   public void testBean() {
   // 	ApplicationContext context =
   // 			new ClassPathXmlApplicationContext("bean.xml");
       ClassPathXmlApplicationContext context =
           	new ClassPathXmlApplicationContext("bean.xml");
       Orders orders = context.getBean("orders", Orders.class);
       System.out.println("第四步 获取创建 bean 实例对象");
       System.out.println(orders);
       //手动让 bean 实例销毁
       context.close();
   }
   ```

7. xml的自动装配

   - 根据指定装配规则（属性名或属性类型），Spring自动将匹配的属性值进行注入。
   - 自动装配的过程：<bean>标签中属性autowire="..."。可设值：byName：根据名称自动装配。byType：根据属性类型自动注入。
   - 不常用，一般通过注解自动装配。
   - ```xml
     <!--根据属性名自动装配，还可以byType-->
     <bean id="emp" class="com.atguigu.spring5.autowire.Emp" autowire="byName">
     ```

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
5. 上述四个注解**都可以用来创建bean实例**。

### Ⅲ 基于注解方式实现对象创建

1. 第一步：引入依赖（如spring-aop-5.2.9.RELEASE.jar）。
2. 第二部：[开启组件扫描](https://blog.csdn.net/weixin_43883917/article/details/112747184)。先引入context命名空间。再配置扫描范围（<context:component-scan base-package="包名"> ... </context:component-scan>）。**默认的情况就是都扫描,可配置扫描的文件列表和不扫描的文件列表**。

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
   
       <!--开启组件扫描
       1、如果要扫描多个包，多个包用逗号隔开
       2、直接写到上层目录
   
       所有类所有注解都扫描
       -->
       <context:component-scan base-package="com.xxx"></context:component-scan>
   
       <!--示例 1
        use-default-filters="false" 表示现在不使用默认 filter，自己配置 filter
        context:include-filter ，设置扫描哪些内容
       -->
        <context:component-scan base-package="com.atguigu" use-default-filters="false">
           <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
       </context:component-scan>
   </beans>
   
   ```
3. 第三步：使用注解修饰代码结构。

   ```java
   @Component(value = "userService") //<bean id="userService" class=".."/>
   public class UserService {
   	public void add() {
    		System.out.println("service add.......");
   	}
   }
   ```

### Ⅳ 基于注解方法实现属性注入

1. @**Autowired：**自动根据**类型**注入，自动装配常用注解。
2. @**Qualifier(value="名称")**：指定自动注入的id名称，**与上个注解一起使用**，适用于接口有多个实现类时，指定具体实现类id。
3. @Resource(“名称”)：默认无属性根据类型注入，加属性name="类id"，可根据名称注入。区别，非spring框架包中的，而是javax中的。
4. @Value(value=...)：注入普通类型属性，注入实际值。使用：直接在类属性上修饰。使用后就不用在xml中配置注入了。

### Ⅴ 完全注解开发

1. 创建配置类，替代xml文件。

   ```java
   @Configuration//作为配置类，替代xml配置文件
   @ComponentScan(basePackages = {"com.atguigu"})
   public class SpringConfig
   ```

2. 编写测试类：加载配置时，获取的是AnnotationConfigApplicationContext(配置类.class)实例，且不是加载xml文件。

# 三、AOP（面向切面编程）

## 1、概念

- 面向切面编程，利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的**耦合度降低**，提高程序的可复用性，同时提高了开发的效率。
- 简介的说：**AOP允许不通过修改源代码方式，在主干功能里添加新功能。**
- 举例（登录功能）：
  - 常规基本流程：输入用户名密码 -> 数据库查询 -> 验证 -- 成功 -->主页面（否则回到登陆页面）
  - 若需要添加权限判断功能，原始方式需要修改源代码。而AOP可不通过修改源代码方式添加新的功能（添加权限判断模块，配置进去）

## 2、AOP的底层原理

### Ⅰ AOP底层使用动态代理

1. 有两种情况的动态代理：
   - **有接口**情况，使用JDK动态代理，创建接口实现类代理对象，增强类的方法。
   
   - **没有接口**情况，使用CGLIB动态代理：创建子类的代理对象，增强类的方法。
   
     ![图4](D:\BaiduNetdiskDownload\Java全套教程_尚硅谷\微服务核心\Spring5\笔记\笔记\分析图\图4.png)
   
   - **CGLIB**（Code Genneration Library）是一个功能强大，高性能的代码生成包。它为没有实现接口的类提供代理，为JDK的动态代理提供了很好的补充。通常可以使用Java的动态代理创建代理，但当要代理的类没有实现接口或者为了更好的性能，CGLIB是一个好的选择。

## 3、JDK动态代理

1. 使用JDK动态代理，使用java.lang.reflect.Proxy类里面的方法创建代理对象。

   - 调用newProxyInstance方法

     | static object | newProxyInstance(ClassLoader loader,类<？>[]interfaces，InvocationHandler h) | 返回指定接口的代理实例，该接口将方法调用分派给指定的调用处理程序。 |
     | ------------- | :----------------------------------------------------------- | ------------------------------------------------------------ |

   - 参数一：类加载器

   - 参数二：增强方法所在的类，这个类实现的接口，支持多个接口。

   - 参数三： 实现这个接口InvocationHander，创建代理对象，写增强的方法。可用匿名实现类方式，也可单独写个实现类实现InvocationHandler接口。

2. [编写动态代理代码](https://www.jianshu.com/p/269afd0a52e6)。

   1. 创建接口，定义方法
   
      ```java
      public interface UserDao {
          public int add(int a,int b);
          public String update(String id);
      }
      ```
   
   2. 创建接口实现类，实现方法
   
      ```java
      public class UserDaoImpl implements UserDao {
          @Override
          public int add(int a, int b) {
              return a+b;
          }
          @Override
          public String update(String id) {
              return id;
          }
      }
      ```
   
   3. 创建代理对象代码
   
      ```java
      //创建代理对象代码
      class UserDaoProxy implements InvocationHandler {
          //把创建的是谁的代理对象，把谁传递过来
          //有参数构造传递
          private Object obj;
          public UserDaoProxy(Object obj) {
              this.obj = obj;
          }
          //增强的逻辑
          @Override
          public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
              //方法之前
              System.out.println("方法之前执行...."+method.getName()+" :传递的参数..."+ Arrays.toString(args));
              //被增强的方法执行
              Object res = method.invoke(obj, args);
              //方法之后
              System.out.println("方法之后执行...."+obj);
              return res;
        	}
      }
      ```
   
   4. 使用Proxy类创建接口代理对象
   
      ```java
      public class JDKProxy {
          public static void main(String[] args) {
              //创建接口实现类代理对象
              Class[] interfaces = {UserDao.class};
              UserDaoImpl userDao = new UserDaoImpl();
              UserDao dao = (UserDao)Proxy.newProxyInstance(JDKProxy.class.getClassLoader(), interfaces, new UserDaoProxy(userDao));
              int result = dao.add(1, 2);
              System.out.println("result:"+result);
          }
      }
      ```

## 4、AOP术语

1. **连接点**：类里面那些方法**可以被增强**，这些方法称为连接点。
2. **切入点**：**实际被真正增强**的方法，称为切入点。
3. **通知（增强）**：
   - 实际增强的逻辑部分曾为通知（增强）。
   - 通知有多种类型：
     - 前置通知：被增强方法之前执行。
     - 后置通知：之后执行。
     - 环绕通知：前后都执行。
     - 异常通知：出现异常时执行。
     - 最终通知：类似finally，无论有无异常，都会执行。
4. **切面**：是动作过程，把通知应用到切入点的过程。

## 5、 AOP操作（准备工作）

### Ⅰ什么是AspectJ

1. Spring框架一般都是基于AspectJ实现AOP操作。
2. AspectJ不是Spring组成部分，独立AOP框架，一般AspectJ和Spring框架一起使用，进行AOP操作。

### Ⅱ 基于AspectJ实现AOP操作

1. 基于xml**配置文件**实现

2. 基于**注解**方式实现

### Ⅲ 引入AOP依赖包

```
com.springsource.net.sf.cglib-2.2.0.jar
com.springsource.org.aopalliance-1.0.0.jar
com.springsource.org.aspectj.weaver-1.6.8.RELEASE.jar
```

### Ⅳ 切入点表达式：

   1. 切入点表达式作用：知道对哪个类里面的哪个方法进行增强。

   2. 语法结构：**execution([权限修饰符] [返回类型] [类全路径] [方法名称] [参数列表])**

      - 案例1：对com.senmu.dao.BookDao**类里的某个方法**add()进行增强：execution(*com.senmu.dao.BookDao.add(..))

      - 案例2：对 com.atguigu.dao.BookDao **类里面的所有的方法**进行增强execution(* com.atguigu.dao.BookDao.* (..))

      - 案例3：对 com.atguigu.dao **包里面所有类，类里面所有方法**进行增强

        execution(* com.atguigu.dao.*.* (..))

## 6、AOP操作（实现）

### Ⅰ **AspectJ** **配置文件**

1. 创建两个类，增强类BookProxy和被增强类Book，创建方法buy()。

2. 在spring配置文件中创建两个类对象。

   ```xml
   <!--创建对象--> 
   <bean id="book" class="com.atguigu.spring5.aopxml.Book"></bean>
   <bean id="bookProxy" class="com.atguigu.spring5.aopxml.BookProxy"></bean>
   ```

3. 在spring配置文件中配置AOP增强，利用<aop:config>:

   ```xml
   <!--配置aop增强-->
   <aop:config>
   	<!--切入点-->
   	<aop:pointcut id="p"expression="execution(*com.xxx.spring5.aopxml.Book.buy(..))"/>
   	<!--配置切面-->
   	<aop:aspect ref="bookProxy">
   		<!--增强作用在具体的方法上-->
   		<aop:before method="before" pointcut-ref="p"/>
   	</aop:aspect>
   </aop:config>
   ```

### Ⅱ **AspectJ** **注解**

1. 创建被增强类类，在类里定义方法。

   ```java
   public class User {
       public void add() {
           System.out.println("add.......");
       }
   }
   ```

2. 创建增强类（编写增强逻辑）在增强类里，创建方法，让不同方法代表不同通知类型，如before()代表前置通知。

   ```java
   //增强的类
   public class UserProxy {
       public void before() {//前置通知
           System.out.println("before......");
       }
   }
   ```

3. 进行增强的配置

     - 在spring配置文件中，开启注解扫描。

       ```xml
       <context:component-scan base-package="com.xxx.spring5.aopanno"></context:component-scan>
       ```

     - 使用**注解(如@Comment)**创建**被增强类**和**增强的类**。

     - 在增强的类上面再加上注解@Aspect，生成代理对象。

     - 在spring配置文件中开启生成代理对象。

       ```xml
       <!-- 开启 Aspect 生成代理对象-->
       <aop:aspectj-autoproxy> </aop:aspectj-autoproxy
       ```

4. 配置**不同类型的通知**

   - 在增强类的里面，在作为通知方法上面添加通知类型注解，使用切入点表达式配置。

   - 如配置前置通知：

     ```java
     @Component
     @Aspect //生成代理对象
     public class UserProxy {
         //@Before 注解表示作为前置通知
         @Before(value = "execution(* com.xxx.spring5.aopanno.User.add(..))")
         public void before() {
             System.out.println("before.........");
         }
     }
     ```

   - **通知注解类型**：
     - @Before：前置通知；
     - @Around：环绕；
     - @AfterThrowing：异常；
     - @**After**：不管有无异常都执行，可称为**最终通知**；
     - @**AfterReturning**：返回值之后执行，可称为**后置通知**。

5. **相同切入点抽取**：可用@pointcut(value="excution(通用部分)") 注解 切入点抽取方法（如pointCut()），返回注解中相同的切入点表达式。则通知注解就可写成@Before(value="pointCut()")

   ```java
   //相同切入点抽取
   @Pointcut(value = "execution(* com.xxx.spring5.aopanno.User.add(..))")
   public void pointdemo() {}
   //前置通知
   //@Before 注解表示作为前置通知
   @Before(value = "pointdemo()")
   public void before() {
       System.out.println("before.........");
   }
   ```

6. 若有多个增强类增强同一个方法，可**设置优先级**。@Order（数字型值），**越小优先级越高**。

7. **可实现完全注解开发，可在配置类中配置AspectJ。**

     ```java
     @Configuration
     @ComponentScan(basePackages = {"com.xxx"})
     @EnableAspectJAutoProxy(proxyTargetClass = true)
     public class ConfigAop {
     }
     ```


# 四、JdbcTemplate

## 1、什么是JdbcTeamplate

​	Spring框架对JDBC进行封装，使用JdbcTempate方便实现对数据库操作。

## 2、JdbcTemplate操作数据库

### Ⅰ [准备工作](https://www.cnblogs.com/caoyc/p/5630622.html)

1. 引入相关jar包

   <img src="https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/202207121620758.png" alt="image-20220712162014073" style="zoom:67%;" />
2. 在Spring配置文件**配置数据库连接池**

   ```xml
   <!-- 数据库连接池 --> 
   <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" destroy method="close">
       <property name="url" value="jdbc:mysql:///user_db" />
       <property name="username" value="root" />
       <property name="password" value="root" />
       <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
   </bean>
   ```
3. 配置JdbcTemlate对象，注入DataSource。

   ```xml
   <!-- JdbcTemplate 对象 --> 
   <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
    <!--注入 dataSource-->
    <property name="dataSource" ref="dataSource"></property>
   </bean>
   ```
4. 创建service类，注入dao，创建dao类，在dao中注入JdbcTeamplate。

   ```xml
   <!-- 组件扫描 --> 
   <context:component-scan base-package="com.atguigu"></context:component-scan>
   ```

   ```java
   @Service
   public class BookService {
       //注入 dao
       @Autowired
       private BookDao bookDao;
   }
   
   
   @Repository
   public class BookDaoImpl implements BookDao {
       //注入 JdbcTemplate
       @Autowired
       private JdbcTemplate jdbcTemplate;
   }
   ```

### Ⅲ 添加操作

1. 对应数据库创建实体类（entity）:放在entity下的beans。
2. 编写service和dao：

   1.  在dao中进行数据库操作。
   2.  调用 JdbcTemplate 对象里面 update 方法实现添加操作。

   ```java
   update(String sql, Object... args)
   参数1：sql语句
   参数2：可变参数，设置sql语句值
   ```

   ```java
   @Repository
   public class BookDaoImpl implements BookDao {
       //注入 JdbcTemplate
       @Autowired
       private JdbcTemplate jdbcTemplate;
       //添加的方法
       @Override
       public void add(Book book) {
           //1 创建 sql 语句
           String sql = "insert into t_book values(?,?,?)";
           //2 调用方法实现
           Object[] args = {book.getUserId(), book.getUsername(), 
                            book.getUstatus()};
           int update = jdbcTemplate.update(sql,args);
           System.out.println(update);
       } 
    }
   ```
3. 测试

   ```java
   @Test
   public void testJdbcTemplate() {
       ApplicationContext context =
           new ClassPathXmlApplicationContext("bean1.xml");
       BookService bookService = context.getBean("bookService", 
                                                 BookService.class);
       Book book = new Book();
       book.setUserId("1");
       book.setUsername("java");
       book.setUstatus("a");
       bookService.addBook(book)
   }
   ```

### Ⅳ 删、改、查操作

 调用JdbcTemplate 对象中的不同方法。

# 五、事物管理

## 1、概念

### Ⅰ 什么是事物

1. 事务是**数据库操作最基本单元**，逻辑上的一组操作，要么都成功，如果有一个失败所有操作都失败。
2. 典型场景：银行转账，lucy 转账 100 元 给 mary，lucy 少 100，mary 多 100。

### Ⅱ 事物的四个特性（ACID）

1. **原子性**：**原子性**就是将事物进行的操作捆绑成一个不可分割的单元，事物中进行的数据操作要么全部成功，要么全部失败（回滚）。
2. **一致性**：**一致性是指事物使数据库从一个一致性状态转换到另一个一致性状态。也就是数据库前后必须处于一致性状态。**拿转账来说，假设用户A和用户B两者的钱加起来一共是5000，那么不管A和B之间如何转账，转几次账，事务结束后两个用户的钱相加起来应该还得是5000，这就是事务的一致性。
3. **隔离性**：**多个用户并发访问数据库，数据库为每个用户开启一个事物，每个事物相互独立，互不干扰。**事务的隔离性主要规定了各个事务之间相互影响的程度。隔离性概念主要面向对数据资源的并发访问(Concurrency)，并兼顾影响事务的一致性。当两个事务或者更多事务同时访问同一数据资源的时候，不同的隔离级别决定了各个事务对该数据资源访问的不同行为。事务的隔离级别，隔离程度按照从弱到强分别为“Read Uncommitted (未提交读)”，“Read Committed（提交读）”，“Repeatable Read（可重复读）”和“Serializable（序列化）”。
4. **持久性**：**持久性**是指一个事务一旦被提交了，那么对数据库中的数据的改变就是永久性的，即便是在数据库系统遇到故障的情况下也不会丢失提交事务的操作。

## 2、搭建事物操作环境

![图6](https://cdn.jsdelivr.net/gh/mrsenmu/JavaLearningNotes/images/202207121654473.png)

1. 创建数据库表，添加记录

2. 创建 **service**，搭建**dao**，完成对象创建和注入关系。

   - service 注入 dao

     ```java
     @Service
     public class UserService {
         //注入 dao
         @Autowired
         private UserDao userDao;
     }
     ```

   - 在 dao 注入 JdbcTemplate

     ```java
     @Repository
     public class UserDaoImpl implements UserDao {
         @Autowired
         private JdbcTemplate jdbcTemplate;
     }
     ```

   - 在 JdbcTemplate 注入 DataSource

3. 方法实现：

   - 在 dao 创建两个方法：多钱和少钱的方法。

     ```java
     @Repository
     public class UserDaoImpl implements UserDao {
         @Autowired
         private JdbcTemplate jdbcTemplate;
         //lucy 转账 100 给 mary
         //少钱
         @Override
         public void reduceMoney() {
             String sql = "update t_account set money=money-? where username=?";
             jdbcTemplate.update(sql,100,"lucy");
         }
         //多钱
         @Override
         public void addMoney() {
             String sql = "update t_account set money=money+? where username=?";
             jdbcTemplate.update(sql,100,"mary");
         } 
     }
     ```

   - 在 service 创建方法（转账的方法）。

     ```java
     @Service
     public class UserService {
         //注入 dao
         @Autowired
         private UserDao userDao;
         //转账的方法
         public void accountMoney() {
             try {
                 //第一步 开启事物
                 
                 //第二部 进行业务操作
                 //lucy 少 100
                 userDao.reduceMoney();
                 
                 //模拟可能出现的异常
                 
                 //mary 多 100
                 userDao.addMoney();
                 
                 //第三步 没有发生异常，提交事物
             }catch(Exception e){
                 //第四步 出现异常，事物回滚
             }
         } 
     }
     ```

## 3、Spring事物管理介绍

1. 事务添加到 JavaEE 三层结构里面 **Service 层（业务逻辑层）**

2. 在 Spring 进行**事务管理操作**(两种): 编程式事务管理和**声明式事务管理**（使用）

3. 声明式事物管理：

   - **基于注解的方式（使用）**
   - 基于xml配置文件方式

4. **在 Spring 进行声明式事务管理，底层使用 AOP 原理**

5. Spring事物管理API：

   ```java
   PlatformTransactionManager(org.springframework.transaction)
   ```

## 4、基于注解的声明式事物管理

1. spring配置文件中配置事物管理器

   ```xml
   <!--创建事务管理器--> 
   <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    	<!--注入数据源-->
       <property name="dataSource" ref="dataSource"></property>
   </bean>
   ```

2. 在 spring 配置文件，开启事务注解

   - 引入命名空间tx

   - 开启事物注解

     ```xml
     <!--开启事务注解--> 
     <tx:annotation-driven transaction-manager="transactionManager"></tx:annotation-driven>
     ```

3. 在 service 类上面（或者 service 类里面方法上面）添加事务注解

   - **@Transactional**，这个注解添加到类上面，也可以添加方法上面
   - 如果把这个注解添加**类**上面，这个类里面**所有的方法**都添加事务
   - 如果把这个注解添加**方法**上面，为这个方法添加事务

## 5、声明式事物管理参数配置

1. 在 service 类上面添加注解@Transactional，在这个**注解里面可以配置事务相关参数**，可设置的参数如下。

2. 事务传播行为(**propagation**)：指的就是当一个事务方法被另一个事务方法调用时，这个事务方法应该如何进行。

   - @Transactional(propagation = Propagation.**REQUIRED**)
   - 事物的传播行为由传播属性指定。spring的定义了7种类传播行为。

   | 传播属性                  | 描述                                                         |
   | ------------------------- | ------------------------------------------------------------ |
   | PROPAGATION_REQUIRED      | 表示当前方法必须运行在事务中。如果当前事务存在，方法将会在该事务中运行。否则，会启动一个新的事务 |
   | PROPAGATION_REQUIRED_NEW  | 表示当前方法B必须运行在它自己的事务B中。一个新的事务B将被启动。如果存在当前事务A，在该方法B执行期间，当前事务A会被挂起。相当于A为外层事物，B为内层事物。如果使用JTATransactionManager的话，则需要访问TransactionManager |
   | PROPAGATION_SUPPORTS      | 表示当前方法不需要事务上下文，但是如果存在当前事务的话，那么该方法会在这个事务中运行 |
   | PROPAGATION_NOT_SUPPORTED | 当前事务将被挂起。如果使用JTATransactionManager的话，则需要访问TransactionManager |
   | PROPAGATION_MANDATORY     | 表示该方法必须在事务中运行，如果当前事务不存在，则会抛出—个异常 |
   | PROPAGATION_NEVER         | 表示当前方法不应该运行在事务上下文中。如果当前正有一个事务在运行，则会抛出异常 |
   | PROPAGATION_NESTED        | 其行为与PROPAGATION_REQUIRED一样。注意各厂商对这种传播行为的支持是有所差异的。可以参考资源管理器的文档来确认它们是否支持嵌套事务 |

3. **ioslation：事务隔离级别**：

   1. 事务有特性成为隔离性，多事务操作之间不会产生影响。不考虑**隔离性产生很多问题**。

   2. 有三个读问题：脏读、不可重复读、虚（幻）读

      - **脏读：**一个未提交事务读取到另一个未提交事务(操作后回滚前数据)的数据。
      - **不可重复读：**一个未提交事务读取到另一提交事务修改数据，两个事物同时进行时，事物A读取到事物B提交修改后的数据。
      - **幻读：**一个未提交事务读取到另一提交事务添加数据。

   3. 通过设置隔离级别来解决问题。

      | 隔离级别                           | 脏读 | 不可重复读 | 幻读 |
      | ---------------------------------- | ---- | ---------- | ---- |
      | READ UNCOMMITTED<br />（读未提交） | 有   | 有         | 有   |
      | READ COMMITTED<br />（读已提交）   | 无   | 有         | 有   |
      | REPEATABLE READ<br />（可重复读）  | 无   | 无         | 有   |
      | SERIALIZABLE<br />（串行化）       | 无   | 无         | 无   |

      注意：MySQL默认的隔离级别是REPEATABLE READ。

4. **timeout：超时时间**

   - 事物需要再一定时间内进行提交，如果不提交进行回滚。
   - 默认值是-1，设置时间以秒单位进行计算。

5. **readOnly：是否只读**，默认false。

6. **rollbackFor：回滚**，设置出现哪些异常进行事物回滚。

7. **noRollbackFor：不回滚**，设置出现哪些异常不进行事物回滚。

## 6、基于XML声明式的事物管理

在spring配置文件中进行配置

```xml
<!--1 创建事务管理器--> 
<bean id="transactionManager"            class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <!--注入数据源-->
    <property name="dataSource" ref="dataSource"></property>
</bean>

<!--2 配置通知--> 
<tx:advice id="txadvice">
    <!--配置事务参数-->
    <tx:attributes>
        <!--指定哪种规则的方法上面添加事务-->
        <tx:method name="accountMoney" propagation="REQUIRED"/>
        <!--<tx:method name="account*"/>-->
    </tx:attributes>
</tx:advice>

<!--3 配置切入点和切面--> 
<aop:config>
    <!--配置切入点-->
    <aop:pointcut id="pt" expression="execution(*com.atguigu.spring5.service.UserService.*(..))"/>
    <!--配置切面-->
    <aop:advisor advice-ref="txadvice" pointcut-ref="pt"/>
</aop:config>
```

## 7、完全注解声明式事物管理

**创建配置类，使用配置类替代 xml 配置文件**

```java
@Configuration //配置类
@ComponentScan(basePackages = "com.xxx") //组件扫描
@EnableTransactionManagement //开启事务
public class TxConfig {
    //创建数据库连接池
    @Bean
    public DruidDataSource getDruidDataSource() {
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql:///user_db");
        dataSource.setUsername("root");
        dataSource.setPassword("root");
        return dataSource;
    }
    //创建 JdbcTemplate 对象
    @Bean
    public JdbcTemplate getJdbcTemplate(DataSource dataSource) {
        //到 ioc 容器中根据类型找到 dataSource
        JdbcTemplate jdbcTemplate = new JdbcTemplate();
        //注入 dataSource
        jdbcTemplate.setDataSource(dataSource);
        return jdbcTemplate;
    }
    //创建事务管理器
    @Bean
    public DataSourceTransactionManager 
        getDataSourceTransactionManager(DataSource dataSource) {
        DataSourceTransactionManager transactionManager = new 
            DataSourceTransactionManager();
        transactionManager.setDataSource(dataSource);
        return transactionManager;
    }
}
```

# 六、Spring5新特性

## 1、新的支持

1. 整个 Spring5 框架的代码基于 Java8，**运行时兼容 JDK9**，许多不建议使用的类和方法在代码库中删除
2. Spring 5.0 框架自带了通用的**日志封装**
   - Spring5 已经移除 Log4jConfigListener，官方建议使用 **Log4j2**
   - Spring5 框架整合 Log4j2
3. Spring5 框架核心容器支持**@Nullable** 注解
   - @Nullable 注解可以使用在方法上面，属性上面，参数上面，表示方法返回可以为空，属性值可以为空，参数值可以为空

4. Spring5 核心容器支持**函数式风格** GenericApplicationContext

   ```java
   //函数式风格创建对象，交给 spring 进行管理
   @Test
   public void testGenericApplicationContext() {
       //1 创建 GenericApplicationContext 对象
       GenericApplicationContext context = new GenericApplicationContext();
       //2 调用 context 的方法对象注册
       context.refresh();
       context.registerBean("user1",User.class,() -> new User());
       //3 获取在 spring 注册的对象
       // User user = (User)context.getBean("com.atguigu.spring5.test.User");
       User user = (User)context.getBean("user1");
       System.out.println(user);
   }
   ```

5. Spring5 支持整合 **JUnit5**

## 2、Webflux

### Ⅰ 介绍

1. 是 Spring5 添加新的模块，用于 web 开发的，功能和 SpringMVC 类似的，Webflux 使用当前一种比较流程响应式编程出现的框架。

2. 使用传统 web 框架，比如 SpringMVC，这些基于 Servlet 容器，Webflux 是一种**异步非阻塞的框架**，异步非阻塞的框架在 Servlet3.1 以后才支持，核心是基于 Reactor 的相关 API 实现的。

3. 关于异步非阻塞

   - 异步和同步，非阻塞和阻塞，都是针对的对象不一样。

   - **异步和同步针对调用者**，调用者发送请求，如果等着对方回应之后才去做其他事情就是同步，如果发送请求之后不等着对方回应就去做其他事情就是异步。

   - **阻塞和非阻塞针对被调用者**，被调用者受到请求之后，做完请求任务之后才给出反馈就是阻

     塞，受到请求之后马上给出反馈然后再去做事情就是非阻塞。

4. **特点：**

   - **非阻塞式：**在有限资源下，提高系统吞吐量和伸缩性，以 Reactor 为基础实现响应式编程。
   - **函数式编程：**Spring5 框架基于 java8，Webflux 使用 Java8 函数式编程方式实现路由请求。

5. 相较于SpringMVC

   - 两个框架都可以使用注解方式，都运行在 Tomet 等容器中。
   - SpringMVC 采用命令式编程，Webflux 采用异步响应式编程。

### Ⅱ  响应式编程(Java实现)

1. 概述

   响应式编程是一种**面向数据流和变化传播的编程范式**。这意味着可以在编程语言中很方便

   地表达静态或动态的数据流，而相关的计算模型会自动将变化的值通过数据流进行传播。

   电子表格程序就是响应式编程的一个例子。单元格可以包含字面值或类似"=B1+C1"的公

   式，而包含公式的单元格的值会依据其他单元格的值的变化而变化。

2. 实现（Java8及之前版本）

   ```java
   //提供的观察者模式两个类 Observer 和 Observable
   public class ObserverDemo extends Observable {
       public static void main(String[] args) {
           ObserverDemo observer = new ObserverDemo();
           //添加观察者
           observer.addObserver((o,arg)->{
               System.out.println("发生变化");
           });
           observer.addObserver((o,arg)->{
               System.out.println("手动被观察者通知，准备改变");
           });
           observer.setChanged(); //数据变化
           observer.notifyObservers(); //通知
       } 
   }
   ```

### Ⅲ 响应式编程（Reactor实现）

1. 响应式编程操作中，Reactor 是满足 Reactive 规范框架.

2. Reactor 有两个核心类，Mono 和 Flux，这两个类实现接口 Publisher，提供丰富操作符。Flux 对象实现发布者，返回 N 个元素；Mono 实现发布者，返回 0 或者 1 个元素。

3. Flux 和 Mono 都是数据流的发布者，使用 Flux 和 Mono 都可以发出三种数据信号：**元素值，错误信号，完成信号**，错误信号和完成信号都代表终止信号，终止信号用于告诉订阅者数据流结束了，错误信号终止数据流同时把错误信息传递给订阅者。

4. 代码演示Flux和Mono

   - 第一步：引入依赖

     ```xml
     <dependency>
         <groupId>io.projectreactor</groupId>
         <artifactId>reactor-core</artifactId>
         <version>3.1.5.RELEASE</version>
     </dependency>
     ```

   - 第二步：实现代码

     ```java
     public static void main(String[] args) {
         //just 方法直接声明
         Flux.just(1,2,3,4);
         Mono.just(1);
         //其他的方法
         Integer[] array = {1,2,3,4};
         Flux.fromArray(array);
     
         List<Integer> list = Arrays.asList(array);
         Flux.fromIterable(list);
         Stream<Integer> stream = list.stream();
         Flux.fromStream(stream);
     }
     ```

5. 三种信号的特点

   - 错误信号和完成信号都是终止信号，不能共存的。
   - 如果没有发送任何元素值，而是直接发送错误或者完成信号，表示是空数据流。
   - 如果没有错误信号，没有完成信号，表示是无限数据流。

6. 调用 just 或者其他方法只是声明数据流，数据流并没有发出，只有进行订阅之后才会触发数据流，不订阅（subscribe方法）什么都不会发生的。

