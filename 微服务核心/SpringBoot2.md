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



**e> 引导加载自动配置类**

@**SpringBootApplication**注解类上标注的注解

```java
...
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication{...}
```

- @SpringBootConfiguration：@Configuration。代表当前是一个配置类

- @ComponentScan：指定扫描哪些，Spring注解；

- **@EnableAutoConfiguration**：

  ```java
  ...
  @AutoConfigurationPackage
  @Import(AutoConfigurationImportSelector.class)
  public @interface EnableAutoConfiguration {...}
  ```

  - @AutoConfigurationPackage：自动配置包。

    ```java
    @Import(AutoConfigurationPackages.Registrar.class)  //给容器中导入一个组件
    public @interface AutoConfigurationPackage {}
    
    //利用Registrar给容器中导入一系列组件
    //将指定的一个包下的所有组件导入进MainApplication 所在包下。
    ```
    
  - @Import(AutoConfigurationImportSelector.class)
  
    ```java
    1、利用getAutoConfigurationEntry(annotationMetadata);给容器中批量导入一些组件
        
    2、调用List<String> configurations = getCandidateConfigurations(annotationMetadata, attributes)获取到所有需要导入到容器中的配置类
        
    3、利用工厂加载 Map<String, List<String>> loadSpringFactories(@Nullable ClassLoader classLoader)；得到所有的组件
        
    4、从META-INF/spring.factories位置来加载一个文件。
    	默认扫描我们当前系统里面所有META-INF/spring.factories位置的文件
        spring-boot-autoconfigure-2.3.4.RELEASE.jar包里面也有META-INF/spring.factories
    ```
  

**f> 按需开启自动配置项**

虽然我们127个场景的所有自动配置启动的时候默认全部加载。xxxxAutoConfiguration
按照**条件装配**规则（@Conditional），最终会**按需配置**。



**g> 修改默认配置**

```java
    @Bean
	@ConditionalOnBean(MultipartResolver.class)  //容器中有这个类型组件
	@ConditionalOnMissingBean(name = DispatcherServlet.MULTIPART_RESOLVER_BEAN_NAME) //容器中没有这个名字 multipartResolver 的组件
	public MultipartResolver multipartResolver(MultipartResolver resolver) {
        //给@Bean标注的方法传入了对象参数，这个参数的值就会从容器中找。
        //SpringMVC multipartResolver。防止有些用户配置的文件上传解析器不符合规范
		// Detect if the user has created a MultipartResolver but named it incorrectly
		return resolver;
	}
```
SpringBoot默认会在底层配好所有的组件。但是如果用户自己配置了以用户的优先

```java
// 自定义字符编码器
@Bean
@ConditionalOnMissingBean
public CharacterEncodingFilter characterEncodingFilter() {
}
```

**h> 总结**

- SpringBoot先加载所有的自动配置类  xxxxxAutoConfiguration
- 每个自动配置类按照条件进行生效，默认都会绑定配置文件指定的值。xxxxProperties里面拿。xxxProperties和配置文件进行了绑定
- 生效的配置类就会给容器中装配很多组件
- 只要容器中有这些组件，相当于这些功能就有了
- 定制化配置

- - 用户直接自己@Bean替换底层的组件
  - 用户去看这个组件是获取的配置文件什么值就去修改。

**xxxxxAutoConfiguration ---> 组件  --->** **xxxxProperties里面拿值  ---> application.properties**



### Ⅴ  开发技巧

- Lombok：省略java类getter、setter、构造器等方法

  1. IDEA安装lombok插件

  2. 引入依赖

     ```xml
     <dependency>
         <groupId>org.projectlombok</groupId>
         <artifactId>lombok</artifactId>
     </dependency>
     ```

     

- dev-tools：项目或者**页面**修改以后：Ctrl+F9；

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-devtools</artifactId>
      <optional>true</optional>
  </dependency>
  ```

- Spring Initailizr（IDEA项目初始化向导）

## 2、SpringBoot2核心功能

### Ⅰ 配置文件

**a> 文件类型**

- properties：配置方式不变

- yaml：

  1. 简介：

     YAML 是 "YAML Ain't Markup Language"（YAML 不是一种标记语言）的递归缩写。在开发的这种语言时，YAML 的意思其实是："Yet Another Markup Language"（仍是一种标记语言）。 

     非常适合用来做以数据为中心的配置文件

  2. 语法

     - key: value；kv之间有空格
     - 大小写敏感
     - 使用缩进表示层级关系
     - 缩进不允许使用tab，只允许空格
     - 缩进的空格数不重要，只要相同层级的元素左对齐即可
     - '#'表示注释
     - 字符串无需加引号，如果要加，'' 与 "" 表示字符串内容 会被 转义/不转义

  3. 数据类型

     - 字面量：单个的、不可再分的值。date、boolean、string、number、null

       ```yaml
       K: v
       ```

     - 对象：键值对集合。map、hash、set、object

       ```yaml
       行内写法：  k: {k1:v1,k2:v2,k3:v3}
       #或
       k: 
         k1: v1
         k2: v2
         k3: v3
       ```

     - 数组：一组按次序排列的值。array、list、queue

       ```
       行内写法：  k: [v1,v2,v3]
       #或者
       k:
        - v1
        - v2
        - v3
       ```

       

**b> 配置提示**

自定义的类和配置文件绑定一般没有提示。

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>


<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <excludes>
                    <exclude>
                        <groupId>org.springframework.boot</groupId>
                        <artifactId>spring-boot-configuration-processor</artifactId>
                    </exclude>
                </excludes>
            </configuration>
        </plugin>
    </plugins>
</build>
```



### Ⅱ web开发

**a> SpringMVC 自动配置概览**

-   内容协商视图解析器(ContentNegotiatingViewResolver) 和 BeanName视图解析器(BeanNameViewResol)
-   静态资源（包括webjars）
-   自动注册 Converter，GenericConverter，Formatter 
-   支持 HttpMessageConverters （配合内容协商理解原理）
-   自动注册 MessageCodesResolver （国际化用）
- 静态index.html 页支持
- 自定义 Favicon
- 自动使用 ConfigurableWebBindingInitializer ，（DataBinder负责将请求数据绑定到JavaBean上）



> **不用@EnableWebMvc注解。使用** `@Configuration` **+** `WebMvcConfigurer` **自定义规则**
>
> **声明** `WebMvcRegistrations` **改变默认底层组件**
>
> **使用** `@EnableWebMvc+@Configuration+DelegatingWebMvcConfiguration 全面接管SpringMVC`

**b> 简单功能分析**

- **静态资源访问**

  1. 静态资源目录

     只要静态资源放在类路径下： called `/static` (or `/public` or `/resources` or `/META-INF/resources`

     访问 ： 当前项目根路径/ + 静态资源名 

     原理： 静态映射 /**。

     请求进来，先去找Controller看能不能处理。不能处理的所有请求又都交给静态资源处理器。静态资源也找不到则响应404页面

     改变默认的静态资源路径

     ```yaml
     spring:
       mvc:
         static-path-pattern: /res/**
     
       resources:
         static-locations: [classpath:/haha/]
     ```

  2. **静态资源访问前缀**

     默认无前缀

     当前项目 + **static-path-pattern** +  静态资源名 = 静态资源文件夹下找

  3. webjar

     自动映射 /[webjars](http://localhost:8080/webjars/jquery/3.5.1/jquery.js)/**

     https://www.webjars.org/

     ```xml
     <dependency>
         <groupId>org.webjars</groupId>
         <artifactId>jquery</artifactId>
         <version>3.5.1</version>
     </dependency>
     ```

     访问地址：[http://localhost:8080/webjars/**jquery/3.5.1/jquery.js**](http://localhost:8080/webjars/jquery/3.5.1/jquery.js)   后面地址要按照依赖里面的包路径

- **欢迎页支持**

  1. 静态资源路径下  index.html
     - 可以配置静态资源路径（resources: static-locations: [classpath:/haha/]）
     - 但是不可以配置静态资源的访问前缀（static-path-pattern）。否则导致 index.html不能被默认访问
  2. controller能处理/index
  
- 自定义 Favcon

  favicon.ico 放在静态资源目录下即可。但是不可以配置静态资源的访问前缀。

- **静态资源配置原理**

  - SpringBoot启动默认加载  xxxAutoConfiguration 类（自动配置类）

  - SpringMVC功能的自动配置类 WebMvcAutoConfiguration，生效

    ```java
    @Configuration(proxyBeanMethods = false)
    @ConditionalOnWebApplication(type = Type.SERVLET)
    @ConditionalOnClass({ Servlet.class, DispatcherServlet.class, WebMvcConfigurer.class })
    @ConditionalOnMissingBean(WebMvcConfigurationSupport.class)
    @AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE + 10)
    @AutoConfigureAfter({ DispatcherServletAutoConfiguration.class, TaskExecutionAutoConfiguration.class,
    		ValidationAutoConfiguration.class })
    public class WebMvcAutoConfiguration {}
    ```

  - 给容器中配了什么

    ```java
    @Configuration(proxyBeanMethods = false)
    @Import(EnableWebMvcConfiguration.class)
    @EnableConfigurationProperties({ WebMvcProperties.class, ResourceProperties.class })
    @Order(0)
    public static class WebMvcAutoConfigurationAdapter implements WebMvcConfigurer {}
    ```

  - 配置文件的相关属性和xxx进行了绑定。WebMvcProperties==**spring.mvc**、ResourceProperties==**spring.resources**

  - 配置类只有一个有参构造器

    ```java
    //ResourceProperties resourceProperties；获取和spring.resources绑定的所有的值的对象
    //WebMvcProperties mvcProperties 获取和spring.mvc绑定的所有的值的对象
    //ListableBeanFactory beanFactory Spring的beanFactory
    //HttpMessageConverters 找到所有的HttpMessageConverters
    //ResourceHandlerRegistrationCustomizer 找到 资源处理器的自定义器。=========
    //DispatcherServletPath  
    //ServletRegistrationBean   给应用注册Servlet、Filter....
    public WebMvcAutoConfigurationAdapter(ResourceProperties resourceProperties, WebMvcProperties mvcProperties,
                                          ListableBeanFactory beanFactory, ObjectProvider<HttpMessageConverters> messageConvertersProvider, ObjectProvider<ResourceHandlerRegistrationCustomizer> resourceHandlerRegistrationCustomizerProvider, ObjectProvider<DispatcherServletPath> dispatcherServletPath, ObjectProvider<ServletRegistrationBean<?>> servletRegistrations) {
        this.resourceProperties = resourceProperties;
        this.mvcProperties = mvcProperties;
        this.beanFactory = beanFactory;
        this.messageConvertersProvider = messageConvertersProvider;
        this.resourceHandlerRegistrationCustomizer = resourceHandlerRegistrationCustomizerProvider.getIfAvailable();
        this.dispatcherServletPath = dispatcherServletPath;
        this.servletRegistrations = servletRegistrations;
    }
    ```

  - 资源处理的默认规则

    ```java
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        if (!this.resourceProperties.isAddMappings()) {
            logger.debug("Default resource handling disabled");
            return;
        }
        Duration cachePeriod = this.resourceProperties.getCache().getPeriod();
        CacheControl cacheControl = this.resourceProperties.getCache().getCachecontrol().toHttpCacheControl();
        //webjars的规则
        if (!registry.hasMappingForPattern("/webjars/**")) {
            customizeResourceHandlerRegistration(registry.addResourceHandler("/webjars/**")
                                                 .addResourceLocations("classpath:/META-INF/resources/webjars/")
                                                 .setCachePeriod(getSeconds(cachePeriod)).setCacheControl(cacheControl));
        }
    
        //
        String staticPathPattern = this.mvcProperties.getStaticPathPattern();
        if (!registry.hasMappingForPattern(staticPathPattern)) {
            customizeResourceHandlerRegistration(registry.addResourceHandler(staticPathPattern)
                                                 .addResourceLocations(getResourceLocations(this.resourceProperties.getStaticLocations()))
                                                 .setCachePeriod(getSeconds(cachePeriod)).setCacheControl(cacheControl));
        }
    }
    ```

    ```yaml
    spring:
    #  mvc:
    #    static-path-pattern: /res/**
    
      resources:
        add-mappings: false   禁用所有静态资源规则
    ```

    ```java
    @ConfigurationProperties(prefix = "spring.resources", ignoreUnknownFields = false)
    public class ResourceProperties {
    
    	private static final String[] CLASSPATH_RESOURCE_LOCATIONS = { "classpath:/META-INF/resources/",
    			"classpath:/resources/", "classpath:/static/", "classpath:/public/" };
    
    	/**
    	 * Locations of static resources. Defaults to classpath:[/META-INF/resources/,
    	 * /resources/, /static/, /public/].
    	 */
    	private String[] staticLocations = CLASSPATH_RESOURCE_LOCATIONS;
    ```

  - index.html欢迎页 处理规则

    ```java
    HandlerMapping：处理器映射。保存了每一个Handler能处理哪些请求。	
    
        @Bean
        public WelcomePageHandlerMapping welcomePageHandlerMapping(ApplicationContext applicationContext,
                                                                   FormattingConversionService mvcConversionService, ResourceUrlProvider mvcResourceUrlProvider) {
        WelcomePageHandlerMapping welcomePageHandlerMapping = new WelcomePageHandlerMapping(
            new TemplateAvailabilityProviders(applicationContext), applicationContext, getWelcomePage(),
            this.mvcProperties.getStaticPathPattern());
        welcomePageHandlerMapping.setInterceptors(getInterceptors(mvcConversionService, mvcResourceUrlProvider));
        welcomePageHandlerMapping.setCorsConfigurations(getCorsConfigurations());
        return welcomePageHandlerMapping;
    }
    
    WelcomePageHandlerMapping(TemplateAvailabilityProviders templateAvailabilityProviders,
                              ApplicationContext applicationContext, Optional<Resource> welcomePage, String staticPathPattern) {
        if (welcomePage.isPresent() && "/**".equals(staticPathPattern)) {
            //要用欢迎页功能，必须是/**
            logger.info("Adding welcome page: " + welcomePage.get());
            setRootViewName("forward:index.html");
        }
        else if (welcomeTemplateExists(templateAvailabilityProviders, applicationContext)) {
            // 调用Controller  /index
            logger.info("Adding welcome page template: index");
            setRootViewName("index");
        }
    }
    ```

    


**c> 请求参数处理**

1. **请求映射**

   - **rest使用与原理**

     1. @xxxMapping；

     2. Rest风格支持（*使用**HTTP**请求方式动词来表示对资源的操作*）

        | 业务 | 传统        | Rest (method + /user) |
        | ---- | ----------- | --------------------- |
        | 查询 | /getUser    | GET                   |
        | 保存 | /saveUser   | POST                  |
        | 修改 | /editUser   | PUT                   |
        | 删除 | /deleteUser | DELETE                |

   	   ```java
        @Bean
        @ConditionalOnMissingBean(HiddenHttpMethodFilter.class)
        @ConditionalOnProperty(prefix = "spring.mvc.hiddenmethod.filter", name = "enabled", matchIfMissing = false)
        public OrderedHiddenHttpMethodFilter hiddenHttpMethodFilter() {
            return new OrderedHiddenHttpMethodFilter();
        }
        
        
        //自定义filter
        @Bean
        public HiddenHttpMethodFilter hiddenHttpMethodFilter(){
            HiddenHttpMethodFilter methodFilter = new HiddenHttpMethodFilter();
            methodFilter.setMethodParam("_m");
            return methodFilter;
        }
        ```
     
     3. **Rest原理（表单提交要使用REST的时候）**
     
        > 表单提交会带上**_method=PUT**
        >
        > **请求过来被**HiddenHttpMethodFilter拦截
        >
        > ​		请求是否正常，并且是POST
        >
        > ​			获取到**_method**的值。
        >
        > ​			兼容以下请求；**PUT**.**DELETE**.**PATCH**
        >
        > ​			**原生request（post），包装模式requesWrapper重写了getMethod方法，返回的是传入的值。**
        >
        > ​			**过滤器链放行的时候用wrapper。以后的方法调用getMethod是调requesWrapper的。**
        >
        > Rest使用客户端工具
        >
        > ​		如PostMan直接发送 put、delete等方式请求，无需Filter
        
        ```yaml
        spring:
          mvc:
            hiddenmethod:
              filter:
                enabled: true   #开启页面表单的Rest功能
        ```
     
   - **请求映射原理**

     SpringMVC功能分析都从 org.springframework.web.servlet.DispatcherServlet —> doDispatch（）

     ```java
     protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
     		HttpServletRequest processedRequest = request;
     		HandlerExecutionChain mappedHandler = null;
     		boolean multipartRequestParsed = false;
     
     		WebAsyncManager asyncManager = WebAsyncUtils.getAsyncManager(request);
     
     		try {
     			ModelAndView mv = null;
     			Exception dispatchException = null;
     
     			try {
     				processedRequest = checkMultipart(request);
     				multipartRequestParsed = (processedRequest != request);
     
     				// 找到当前请求使用哪个Handler（Controller的方法）处理
     				mappedHandler = getHandler(processedRequest);
                     
                     //HandlerMapping：处理器映射。/xxx->>xxxx
     ```

     **RequestMappingHandlerMapping**：保存了所有@RequestMapping 和handler的映射规则。

     所有的请求映射都在HamdlerMapping中。

     - SpringBoot自动配置 欢迎页 的 WelcomePageHandlermapping。访问 / 能访问到index.html
     - SpringBoot自动配置了默认的RequestMappingHandlerMapping
     - 请求进来会挨个尝试**所有的 handlerMapping** 看是否有请求信息，找到就返回handler，没有就试下一个
     - 我们需要一些自定义的映射处理，我们也可以自己给容器中放handlerMapping。自定义HandlerMapping。

     ```java
     protected HandlerExecutionChain getHandler(HttpServletRequest request) throws Exception {
         if (this.handlerMappings != null) {
             for (HandlerMapping mapping : this.handlerMappings) {
                 HandlerExecutionChain handler = mapping.getHandler(request);
                 if (handler != null) {
                     return handler;
                 }
             }
         }
         return null;
     }
     ```

     

2. **普通参数与基本注解**

   - 注解

     | 注解            | 说明                                                         | 案例                                                         |
     | --------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
     | @PathVariable   | 映射 URL 绑定的**占位符**                                    | /user/{id} --> @PathVariable("id") Long id                   |
     | @RequestHeader  | 映射请求头中的参数                                           | @RequestHeader(value="User-Agent") String userAgent          |
     | @ModelAttribute | 映射Model对象中的参数                                        | 可应用在方法和参数上                                         |
     | @RequestParam   | 映射 URL 上拼接的参数值                                      | @RequestParam("name") String name                            |
     | @MatrixVariable | 优化映射 URL 上拼接的参数值(可拼接多值参数)                  | */Demo1/color="red","blue"* --> @MatrixVariable String[] color |
     | @CookieValue    | 用来获取Cookie中的值                                         | @CookieValue("LY_TOKEN") String token                        |
     | @RequestBody    | 处理content-type**不是**默认的application/x-www-form-urlcoded编码的内容，一般是application/json | @requestBody User user                                       |

   - Servlet API

     | API                            | 说明 | 案例 |
     | ------------------------------ | ---- | ---- |
     | WebRequest                     |      |      |
     | ServletRequest                 |      |      |
     | MultipartRequest               |      |      |
     | HttpSession                    |      |      |
     | javax.servlet.http.PushBuilder |      |      |
     | Principal                      |      |      |
     | InputStream                    |      |      |
     | Reader                         |      |      |
     | HttpMethod                     |      |      |
     | Locale                         |      |      |
     | TimeZone                       |      |      |
     | ZoneId                         |      |      |

     

   - 复杂参数

     ```
     Map、Model（map、model里面的数据会被放在request的请求域  request.setAttribute）、Errors/BindingResult、RedirectAttributes（ 重定向携带数据）、ServletResponse（response）、SessionStatus、UriComponentsBuilder、ServletUriComponentsBuilder
     ```

     

   - 自定义对象参数

3. **POJO封装过程**

   - 

4. **参数处理原理**

   - 

**d> 数据响应与内容协商**



**e> 视图解析与模板引擎**



**f> 拦截器**



**g> 异常处理**



**h> 原生Servlet组件**



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



