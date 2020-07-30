## SpringBoot注解
#### Test

SpringBoot 测试支持由两个模块提供
spring-boot-test 包含核心项目，而spring-boot-test-autoconfigure 支持测试的自动配置

pom引入 sping-boot-starter-test

```
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
```

Spring Boot 提供了一个@SpringBootTest注解，当你需要Spring Boot功能时，它可以用作标准Spring-test中@ContextConfiguration注释的替代方法，注解的工作原理是通过SpringApplication在测试中创建ApplicationContext


```
@RunWith(SpringRunner.class)
@SpringBootTest
public class test{
    
}

等价于---

@RunWith(SpringRunner.class)
@ContextConfiguration(locations={"classpath:spring-servlet.xml","classpath:spring-dao-test.xml"})
public class test{
    
}
```
当然，@SpringBootTest注解可以使用webEnvironment属性来进一步优化测试的运行方式：
- [MOCK] 加载一个WebApplicationContext并提供一个模拟servlet环境，嵌入式servlet容器在使用此注释时不会启动。如果servletAPI不在你的类路径上，这个模式将透明地回退到创建一个常规的非web应用程序上下文。可以与@AutoConfigureMockMvc结合使用，用于基于MockMvc的应用程序测试。
- [NONE] 使用SpringApplication加载ApplicationContext, 但不提供任何servlet环境
- [RANDOM_PORT] 加载一个EmbeddedWebApplicationContext并提供一个真正的servlet环境，嵌入式servlet容器启动并在随机端口上侦听。
- [DEFINED_PORT] 加载一个EmbeddedWebApplicationContext并提供一个真正的servlet环境，嵌入式servlet容器启动并监听定义的端口（即从application.properties或默认端口8080）。

```
> @RunWith()  一个运行器

> @RunWith(JUnit4.class)  指用JUnit4来运行

> @RunWith(SpringJUnit4ClassRunner.class)  让测试运行于Spring测试环境

```
##### 注意：

```
1、如果你的测试是@Transactional，默认情况下它会在每个测试方法结束时回滚事务，但是，由于使用RANDOM_PORT或者DEFINED_PORT这种安排隐式地提供一个真正的servlet环境，所以HTTP客户端和服务器将在不同的线程中运行，从而分离事务。在这种情况下，服务器上的任何事物都不会回滚。
2、不用忘记在测试中添加@RunWith(SpringRunner.class)，否则注释将被忽略。
```

### Java注解
```
> @Deprecated Java过期注解，当前版本不使用

> @SuppressWarings    用于抑制编译器产生的警告信息

> @SupperssWarings("ALL")  抑制所以类型的警告

> @SuppressWarnings("unchecked")  抑制单类型的警告

> @SuppressWarnings("unchecked", "rawtypes")  抑制多类型的警告
```
### Java 8 函数式注解

```
函数式接口：首先是一个接口，然后在这个接口里面只能有一个抽象方法。
这种类型的接口也成为SAM接口，即Single Abstract Method interface

@FunctionalInterface

函数式接口用途：主要用在Lambda表达式和方法引用上，如下：
@FunctionalInterface
interface TestService {
    void test(String msg);
}

那么就可以使用Lambda表达式来表示该接口的一个实现：
TestService testService = msg -> System.out.println("测试："+msg);

Java 8 为函数式引入一个新的注解，@FunctionalInterface，主要用于编译级错误检查，加不加这个注解并不影响该接口是否是函数式接口。
```
### Lombok注解

```
> @Data 注解在类上, 为类提供读写属性, 此外还提供了 equals()、hashCode()、toString() 方法

> @NoArgsConstructor 生成一个无参构造

> @AllArgsContructor 生成一个所有参构造

> RequiredArgsConstructor 生成一个包含常量和标识

> @NonNull 注解在参数上, 如果该类参数为 null , 就会报出异常,  throw new NullPointException(参数名)
 
>  Cleanup 注释在引用变量前, 自动回收资源 默认调用 close() 方法

> @Getter/@Setter 注解在类上, 为类提供读写属性

> @Getter(lazy=true)

> @ToString 注解在类上, 为类提供 toString() 方法

> @EqualsAndHashCode 注解在类上, 为类提供 equals() 和 hashCode() 方法
 
> @Builder 注解在类上, 为类提供一个内部的 Builder

> @Synchronized 注解在方法上, 为方法提供同步锁

> @Log4j/@Slf4j 注解在类上，为类提供属性名为log的日志对象
```
### Spring注解

```
> @CrossOrigin Spring框架用于解决跨域问题，可用于类上或者具体的方法上

> @Configuration 用于定义配置类，等同于xml文件中的<beans>,作用为配置spring容器(应用上下文)
注意：
    1.@Configuration不可是final类型
    2.@Configuration不可是匿名类
    3.嵌套的@Configuration必须是静态类
    
> @Bean 标注在方法上(返回某个实例的方法)，等价于xml中<bean>，作用为注册bean对象
拓展：  1.@Bean同时可以指定初始化和销毁方法，@Bean(name="test",initMethod="start",         destroyMethod="cleanUp")
        2.@Bean注解在返回实例的方法上，如果未通过@Bean指定bean的名称，则默认与标注的方法名相同
        3.@Bean注解默认作用域为单例singleton作用域，可通过@Scope(“prototype”)设置为原型作用域
        4.既然@Bean的作用是注册bean对象，那么完全可以使用@Component、@Controller、@Service、@Ripository等注解注册bean，当然需要配置@ComponentScan注解进行自动扫描   
    
> @ComponentScan  等同于xml文件中<context:component-scan base-package="com.ziding.demo">

> @EnableAsync 开启异步方法的支持

> @EnableScheduling 开启计划任务的支持

> @EnableWebMVC 开启Web MVC的配置支持

> @EnableCaching 开启注解式缓存的支持

> @EnableTransactionManagement 开启注解事务的支持

> @EnableConfigurationProperties 开启对 @ConfigurationProperties注解配置Bean的支持

> @EnableJpaRepositories 开启对Spring Data JPA Repostory 的支持

- 加载配置文件

> @Value 加载yml文件中的配置，@Value("${enterprise.token}")

> @PropertySource 加载指定的配置文件并将属性注入到配置类,可同时指定多个配置文件 @PropertySource(value={"classpath:config/jdbc-dev.properties"}, ignoreResourceNotFound=false, encoding="UTF-8",name="jdbc-dev.properties")

> @ConfigurationProperties 作用在类上，@ConfigurationProperties(prefix="spring.datasource.qr")
```

