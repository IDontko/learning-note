<a name="index">**Index**</a>

<a href="#0">Spring </a>  
&emsp;<a href="#1">Spring IOC & AOP</a>  
&emsp;&emsp;<a href="#2">Spring IOC</a>  
&emsp;&emsp;<a href="#3">AOP</a>  
&emsp;&emsp;&emsp;<a href="#4">aop切面的相关方法   </a>  
&emsp;&emsp;<a href="#5">Spring AOP 和 AspectJ AOP 有什么区别？</a>  
&emsp;<a href="#6">Spring 中的 bean 的作用域有哪些?</a>  
&emsp;<a href="#7">Spring 中的 bean 生命周期</a>  
&emsp;<a href="#8">Spring 循环依赖</a>  
&emsp;&emsp;<a href="#9">相关问题(加深理解)</a>  
&emsp;&emsp;<a href="#10">相关文章</a>  
&emsp;<a href="#11">Spring Transaction</a>  
&emsp;&emsp;<a href="#12">@Transactional 的声明式事务管理</a>  
&emsp;&emsp;<a href="#13">spring transaction的隔离级别</a>  
&emsp;&emsp;<a href="#14"> 同一个方法无事务的方法调用有事务的方法会出现什么情况？</a>  
&emsp;<a href="#15">Spring boot 自动配置的加载流程</a>  
&emsp;<a href="#16">Spring mvc 工作原理</a>  
&emsp;<a href="#17">@RestController vs @Controller</a>  
&emsp;<a href="#18">spring中的设计模式</a>  
&emsp;<a href="#19">零散的一些面试题</a>  
&emsp;&emsp;&emsp;<a href="#20">@Autowired和@Resource的区别是什么？</a>  
&emsp;&emsp;&emsp;<a href="#21">@PostConstruct和@PreDestroy</a>  
&emsp;&emsp;&emsp;<a href="#22">Spring 的异常处理</a>  
&emsp;&emsp;&emsp;<a href="#23"> json 数据处理</a>  
&emsp;&emsp;&emsp;<a href="#24"> @Component 和 @Bean 的区别是什么？</a>  
# <a name="0">Spring </a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>

## <a name="1">Spring IOC & AOP</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>
### <a name="2">Spring IOC</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>
- IOC 理解
  1. 控制反转：原来在程序中手动创建对象，现在需要什么对象由IOC提供，一个好处就是对象统一管理。
  2. 依赖注入：将对象之间的相互依赖关系交给 IoC 容器来管理，并由 IoC 容器完成对象的注入。简化开发及对象的创建。
  
### <a name="3">AOP</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>
AOP(Aspect-Oriented Programming:面向切面编程)
  - 能够将那些与业务无关，却为业务模块所共同调用的逻辑或责任（例如事务处理、日志管理、权限控制等）封装起来，便于减少系统的重复代码，降低模块间的耦合度，并有利于未来的可拓展性和可维护性。

Spring AOP就是基于动态代理的，如果要代理的对象，实现了某个接口，那么Spring AOP会使用JDK Proxy，去创建代理对象，而对于没有实现接口的对象，就无法使用 JDK Proxy 去进行代理了，这时候Spring AOP会使用Cglib 。
#### <a name="4">aop切面的相关方法   </a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>
https://www.cnblogs.com/zhangxufeng/p/9160869.html

### <a name="5">Spring AOP 和 AspectJ AOP 有什么区别？</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>
Spring AOP 属于运行时增强，而 AspectJ 是编译时增强。 Spring AOP 基于代理(Proxying)，而 AspectJ 基于字节码操作(Bytecode Manipulation)。
- 如果我们的切面比较少，那么两者性能差异不大。但是，当切面太多的话，最好选择 AspectJ ，基于字节码的修改实现代理，它比Spring AOP 快很多。
- @EnableAspectJAutoProxy + @Configuration 用于加载@AspectJ的类。但是在Spring Boot项目中，我们不必显式使用@EnableAspectJAutoProxy。 如果Aspect或Advice位于类路径中，则有一个专用的AopAutoConfiguration启用Spring的AOP支持。


## <a name="6">Spring 中的 bean 的作用域有哪些?</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>
- singleton : 唯一 bean 实例，Spring 中的 bean 默认都是单例的。
- prototype : 每次请求都会创建一个新的 bean 实例。
- request : 每一次HTTP请求都会产生一个新的bean，该bean仅在当前HTTP request内有效。
- session : 每一次HTTP请求都会产生一个新的 bean，该bean仅在当前 HTTP session 内有效。
- global-session： 全局session作用域，仅仅在基于portlet的web应用中才有意义，Spring5已经没有了。Portlet是能够生成语义代码(例如：HTML)片段的小型Java Web插件。它们基于portlet容器，可以像servlet一样处理HTTP请求。但是，与 servlet 不同，每个 portlet 都有不同的会话
   
## <a name="7">Spring 中的 bean 生命周期</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>
- 见声明周期详解

## <a name="8">Spring 循环依赖</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>
对于普通的循环依赖如A 依赖B， B依赖A。在初始化A的时候，会实例化B，实例化B发现需要A的引用，这时候通过缓存返回A的引用。虽然A还未初始化完毕，但是由于是对象的引用，所以最终初始化完成的时候，两个对象均是初始化完整的。
![avatar](https://github.com/rbmonster/learning-note/blob/master/src/main/java/com/four/picture/iocAutowire.png)

getSingleton(beanName, true)这个方法实际上就是到缓存中尝试去获取Bean，整个缓存分为三级
- singletonObjects，一级缓存，存储的是所有创建好了的单例Bean
- earlySingletonObjects，完成实例化，但是还未进行属性注入及初始化的对象
- singletonFactories，提前暴露的一个单例工厂（一个获取对象的函数表达式），二级缓存中存储的就是从这个工厂中获取到的对象。

循环依赖装载流程：
  - 条件1.出现循环依赖的Bean必须要是单例
  - 条件2.依赖注入的方式不能全是构造器注入的方式
  1. 实例化A对象，添加提前暴露的对象的方法到singletonFactory
  2. 填充对象，无循环引用，直接调用postProcessAfterInitialization方法。有循环引用，doGetBean循环引用的对象。
  3. 循环引用对象B实例化，在填充对象的时候，发现循环依赖A，从三级缓存中获取，并添加A到二级缓存。
  4. 若A为AOP代理，此时二级缓存中的对象为代理对象A。
  5. B初始化完成后，继续A的对象填充及初始化，填充完成后。从二级缓存中获取对象，若存在对象，说明发生了循环引用，返回二级缓存的对象。

### <a name="9">相关问题(加深理解)</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>
如果单单使用二级缓存，为解决循环引用问题，那么二级缓存存储的就是为进行属性注入的对象。这与Spring生命周期的设计相悖。
- Spring结合AOP跟Bean的生命周期本身就是通过AnnotationAwareAspectJAutoProxyCreator这个后置处理器来完成的，在这个后置处理的postProcessAfterInitialization方法中对初始化后的Bean完成AOP代理。
  - 如果出现了循环依赖，那没有办法，只有给Bean先创建代理。
  - 但是如果没有出现循环依赖的情况下，设计之初就是让Bean在生命周期的最后一步完成代理而不是在实例化后就立马完成代理。

三级缓存为什么要使用工厂而不是直接使用引用？换而言之，为什么需要这个三级缓存，直接通过二级缓存暴露一个引用不行吗？
- 这个工厂的目的在于**延迟对实例化阶段生成对象的代理**，只有真正发生循环依赖的时候，才去提前生成代理对象，否则只会创建一个工厂并将其放入到三级缓存中，但是不会去通过这个工厂去真正创建对象。
  
- 三级缓存引入对于对象延迟实例化的体现。
![avatar](https://github.com/rbmonster/learning-note/blob/master/src/main/java/com/four/picture/iocAutowire.jpg)

- 不同注入方式对于循环引用的影响
![avatar](https://github.com/rbmonster/learning-note/blob/master/src/main/java/com/four/picture/iocAutowire3.jpg)

### <a name="10">相关文章</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>
- https://mp.weixin.qq.com/s/kS0K5P4FdF3v-fiIjGIvvQ

## <a name="11">Spring Transaction</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>
Spring 框架中，事务管理相关最重要的 3 个接口如下：
- PlatformTransactionManager： （平台）事务管理器，Spring 事务策略的核心。
- TransactionDefinition： 事务定义信息(事务隔离级别、传播行为、超时、只读、回滚规则)。
- TransactionStatus： 事务运行状态。

1. 注解@EnableTransactionManagement 实现事务相关的Bean加载（现在自动配置使用AutoConfiguration实现）
2. TransactionInterceptor 主要的实现类，继承TransactionAspectSupport（定义了事务实现的方式）
3. 实现原理为使用AOP+ThreadLocal实现。

> 详细可见spring 源码部分

### <a name="12">@Transactional 的声明式事务管理</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>
- Propagation.REQUIRED：如果当前存在事务，则加入该事务，如果当前不存在事务，则创建一个新的事务。
- Propagation.SUPPORTS：如果当前存在事务，则加入该事务；如果当前不存在事务，则以非事务的方式继续运行。
- Propagation.MANDATORY：如果当前存在事务，则加入该事务；如果当前不存在事务，则抛出异常。

- Propagation.REQUIRES_NEW：重新创建一个新的事务，如果当前存在事务，延缓当前的事务。
- Propagation.NOT_SUPPORTED：以非事务的方式运行，如果当前存在事务，暂停当前的事务。
- Propagation.NEVER：以非事务的方式运行，如果当前存在事务，则抛出异常。
- Propagation.NESTED：如果没有，就新建一个事务；如果有，就在当前事务中嵌套其他事务。
  
### <a name="13">spring transaction的隔离级别</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>
- TransactionDefinition.ISOLATION_DEFAULT :使用后端数据库默认的隔离级别，MySQL 默认采用的 REPEATABLE_READ 隔离级别 Oracle 默认采用的 READ_COMMITTED 隔离级别.
- TransactionDefinition.ISOLATION_READ_UNCOMMITTED :最低的隔离级别，使用这个隔离级别很少，因为它允许读取尚未提交的数据变更，可能会导致脏读、幻读或不可重复读
- TransactionDefinition.ISOLATION_READ_COMMITTED : 允许读取并发事务已经提交的数据，可以阻止脏读，但是幻读或不可重复读仍有可能发生
- TransactionDefinition.ISOLATION_REPEATABLE_READ : 对同一字段的多次读取结果都是一致的，除非数据是被本身事务自己所修改，可以阻止脏读和不可重复读，但幻读仍有可能发生。
- TransactionDefinition.ISOLATION_SERIALIZABLE : 最高的隔离级别，完全服从 ACID 的隔离级别。所有的事务依次逐个执行，这样事务之间就完全不可能产生干扰，也就是说，该级别可以防止脏读、不可重复读以及幻读。但是这将严重影响程序的性能。通常情况下也不会用到该级别。

### <a name="14"> 同一个方法无事务的方法调用有事务的方法会出现什么情况？</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>
- 当这个方法被同一个类调用的时候，spring无法将这个方法加到事务管理中。只有在代理对象之间进行调用时，可以触发切面逻辑。
1. 使用 ApplicationContext 上下文对象获取该对象;
2. 使用 AopContext.currentProxy() 获取代理对象,但是需要配置exposeProxy=true



## <a name="15">Spring boot 自动配置的加载流程</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>
> 在上下文初始化中invoke**PostProcessor是有执行等级之分的。
> 自动配置加载主要在上下文初始化的invokeBeanFactoryPostProcessors(beanFactory);
1. Spring boot的配置自动加载主要通过@SpringBootApplication 中的 @EnableAutoConfiguration注解实现
2. 注解中@Import(AutoConfigurationImportSelector.class) 的类。借助@Import的支持，收集和注册特定场景相关的bean定义。
3. 该AutoConfigurationImportSelector类的getAutoConfigurationEntry方法会扫描所有包下spring-autoconfigure-metadata.properties的属性
4. 通过@ConditionOn的系列注解并对比过滤符合当前配置的配置项，重新进行config的注解扫描添加需要的bean配置到BenDefinition中
5. 再执行初始化方法。

## <a name="16">Spring mvc 工作原理</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>
流程说明（重要）：
1. 客户端（浏览器）发送请求，直接请求到 DispatcherServlet。
2. DispatcherServlet 根据请求信息调用 HandlerMapping，解析请求对应的 Handler。
3. 解析到对应的 Handler（也就是我们平常说的 Controller 控制器）后，开始由 HandlerAdapter 适配器处理。
4. HandlerAdapter 会根据 Handler 来调用真正的处理器来处理请求，并处理相应的业务逻辑。调用handler的时候，如果有继承HandlerInterceptor接口，就对应拦截处理。
5. 处理器处理完业务后，会返回一个 ModelAndView 对象，Model 是返回的数据对象，View 是个逻辑上的 View。
6. ViewResolver 会根据逻辑 View 查找实际的 View。
7. DispatcherServlet 把返回的 Model 传给 View Resolver（视图渲染）。
8. 把 View 返回给请求者（浏览器）



## <a name="17">@RestController vs @Controller</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>
@RestController
  - ```
    @Controller
    @ResponseBody
    public @interface RestController { ... }
    ```
> 单独使用 @Controller 不加 @ResponseBody的话返回一个视图，这种情况属于比较传统的Spring MVC 的应用
- @ResponseBody 注解的作用是将 Controller 的方法返回的对象通过适当的转换器转换为指定的格式之后，写入到HTTP 响应(Response)对象的 body 中，通常用来返回 JSON 或者 XML 数据，返回 JSON 数据的情况比较多。

## <a name="18">spring中的设计模式</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>
- 工厂设计模式 : Spring使用工厂模式通过 BeanFactory、ApplicationContext 创建 bean 对象。
- 代理设计模式 : Spring AOP 功能的实现。
- 单例设计模式 : Spring 中的 Bean 默认都是单例的。
- 模板方法模式 : Spring 中 jdbcTemplate、hibernateTemplate 等以 Template 结尾的对数据库操作的类，它们就使用到了模板模式。
- 包装器设计模式 : 我们的项目需要连接多个数据库，而且不同的客户在每次访问中根据需要会去访问不同的数据库。这种模式让我们可以根据客户的需求能够动态切换不同的数据源。
- 观察者模式: Spring 事件驱动模型就是观察者模式很经典的一个应用。
- 适配器模式 :Spring AOP 的增强或通知(Advice)使用到了适配器模式、spring MVC 中也是用到了适配器模式适配Controller。

## <a name="19">零散的一些面试题</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>

#### <a name="20">@Autowired和@Resource的区别是什么？</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>
1. @Autowired注解是按类型装配依赖对象，默认情况下它要求依赖对象必须存在，如果允许null值，可以设置它required属性为false。可以结合@Qualifier注解一起使用。
2. @Resource注解和@Autowired一样，也可以标注在字段或属性的setter方法上，但它默认按名称装配。默认按byName自动注入，也提供按照byType 注入；
3. @Resources按名字，是JDK的，@Autowired按类型，是Spring的。
4. 处理这2个注解的BeanPostProcessor不一样CommonAnnotationBeanPostProcessor是处理@Resource注解的，AutoWiredAnnotationBeanPostProcessor是处理@AutoWired注解的


#### <a name="21">@PostConstruct和@PreDestroy</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>
@PostConstruct和@PreDestroy 是两个作用于 Servlet 生命周期的注解，被这两个注解修饰的方法可以保证在整个 Servlet 生命周期只被执行一次，即使 Web 容器在其内部中多次实例化该方法所在的 bean。
  - @PostConstruct : 用来修饰方法，标记在项目启动的时候执行这个方法,一般用来执行某些初始化操作比如全局配置。PostConstruct 注解的方法会在构造函数之后执行,Servlet 的init()方法之前执行。
  - @PreDestroy : 当 bean 被 Web 容器的时候被调用，一般用来释放 bean 所持有的资源。。@PreDestroy 注解的方法会在Servlet 的destroy()方法之前执行。
  - 实现Spring 提供的 InitializingBean和 DisposableBean接口的效果和使用@PostConstruct和@PreDestroy 注解的效果一样
    - 建议您不要使用 InitializingBean回调接口，因为它不必要地将代码耦合到 Spring
- ```
  @Configuratio n
  public class MyConfiguration {
      public MyConfiguration() {
          System.out.println("构造方法被调用");
      }
  
      @PostConstruct
      private void init() {
          System.out.println("PostConstruct注解方法被调用");
      }
  
      @PreDestroy
      private void shutdown() {
          System.out.println("PreDestroy注解方法被调用");
      }
  }
  ```
#### <a name="22">Spring 的异常处理</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>
使用 @ControllerAdvice和@ExceptionHandler处理全局异常
  - ```
    @RestControllerAdvice(basePackages = {"com.design.apidesign.controller"}) 
    public class ExceptionControllerAdvice { 
         
          @ExceptionHandler(MethodArgumentNotValidException.class)
          public String MethodArgumentNotValidExceptionHandler(MethodArgumentNotValidException e) {
    
    }
    
    ```
@ResponseStatusException：通过 ResponseStatus注解简单处理异常的方法（将异常映射为状态码）。
  - ```
    @ResponseStatus(code = HttpStatus.NOT_FOUND)
    public class ResourseNotFoundException2 extends RuntimeException {
    
       public ResourseNotFoundException2() {
       }
    
       // status ： http status 
       // reason ：response 的消息内容
       // cause ： 抛出的异常
       public ResponseStatusException(HttpStatus status, @Nullable String reason, @Nullable Throwable cause) {
    		super(null, cause);
    		Assert.notNull(status, "HttpStatus is required");
    		this.status = status;
    		this.reason = reason;
       }
    
       public ResourseNotFoundException2(String message) {
           super(message);
       }
    }
    ```

#### <a name="23"> json 数据处理</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>
- @JsonIgnoreProperties 作用在类上用于过滤掉特定字段不返回或者不解析
- @JsonIgnore一般用于类的属性上，作用和上面的@JsonIgnoreProperties 一样。
- @JsonFormat一般用来格式化 json 数据。
- @JsonUnwrapped扁平化对象
  ```
  //before
  {
      "location": {
          "provinceName":"湖北",
          "countyName":"武汉"
      },
      "personInfo": {
          "userName": "coder1234",
          "fullName": "shaungkou"
      }
  }
  
  
  @Getter
  @Setter
  @ToString
  public class Account {
      @JsonUnwrapped
      private Location location;
      @JsonUnwrapped
      private PersonInfo personInfo;
      ......
  }
  
  
  //after 
  {
    "provinceName":"湖北",
    "countyName":"武汉",
    "userName": "coder1234",
    "fullName": "shaungkou"
  }
  ```
  
#### <a name="24"> @Component 和 @Bean 的区别是什么？</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>
作用对象不同: @Component 注解作用于类，而@Bean注解作用于方法。
  - @Component通常是通过类路径扫描来自动侦测以及自动装配到Spring容器中（我们可以使用 @ComponentScan 注解定义要扫描的路径从中找出标识了需要装配的类自动装配到 Spring 的 bean 容器中）。@Bean 注解通常是我们在标有该注解的方法中定义产生这个 bean,@Bean告诉了Spring这是某个类的示例，当我需要用它的时候还给我。
  - @Bean 注解比 Component 注解的自定义性更强，而且很多地方我们只能通过 @Bean 注解来注册bean。比如当我们引用第三方库中的类需要装配到 Spring容器时，则只能通过 @Bean来实现。