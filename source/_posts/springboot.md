# springboot

## 1、hello,springboot

官方文档：https://docs.spring.io/spring-boot/docs/2.5.6/reference/htmlsingle/#getting-started

版本更新：https://github.com/spring-projects/spring-boot/wiki#release-notes

### 1.1系统要求

* [Java 8](https://www.java.com/) & 兼容java14 .
* Maven 3.3+
* idea 2019.1.2

### 1.2 Maven设置

```xml
<mirrors>
      <mirror>
        <id>nexus-aliyun</id>
        <mirrorOf>central</mirrorOf>
        <name>Nexus aliyun</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public</url>
      </mirror>
  </mirrors>

  <profiles>
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
  </profiles>
```

### 1.3引入依赖

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

### 1.4主程序

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

### 1.5编写业务

```java
@RestController
public class HelloController {
    @RequestMapping("/hello")
    public String handle01(){
        return "Hello, Spring Boot 2!";
    }
}
```

### 1.6测试

直接运行main()方法

### 1.7简化配置

application.properties

```xml
server.port=8888
```

### 1.8简化部署

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

注意点：

* 取消掉cmd的快速编辑模式,有时候光标放在命令行中就会等待编辑不会运行

## 2、自动配置

### 2.1依赖管理

* 父项目做依赖管理

```xml
<parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.4.RELEASE</version>
    </parent>

它的父项目
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.3.4.RELEASE</version>
  </parent>
```

* 开发导入starter场景启动器

```xml
1、见到很多 spring-boot-starter-* ： *就某种场景
2、只要引入starter，这个场景的所有常规需要的依赖我们都自动引入
3、SpringBoot所有支持的场景
https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-starter
4、见到的  *-spring-boot-starter： 第三方为我们提供的简化开发的场景启动器。
5、所有场景启动器最底层的依赖
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter</artifactId>
  <version>2.3.4.RELEASE</version>
  <scope>compile</scope>
</dependency>
```

* 无需关注版本号，自动版本仲裁

```xml
1、引入依赖默认都可以不写版本
2、引入非版本仲裁的jar,要写版本号
```

* 可以修改默认版本号

```xml
1、查看spring-boot-dependencies里面规定当前依赖的版本 用的 key。
2、在当前项目里面重写配置
    <properties>
        <mysql.version>5.1.43</mysql.version>
    </properties>
```

### 2.2 自动配置

* 自动配好Tomcat
  * 引入Tomcat依赖
  * 配置Tomcat

```
<dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-tomcat</artifactId>
      <version>2.3.4.RELEASE</version>
      <scope>compile</scope>
    </dependency>
```

* 自动配好SpringMVC
*
  * 引入SpringMVC全套组件
  * 自动配好SpringMVC常用组件（功能）
* 自动配好Web常见功能，如：字符编码问题
*
  * SpringBoot帮我们配置好了所有web开发的常见场景
* 默认的包结构
*
  * 主程序所在包及其下面的所有子包里面的组件都会被默认扫描进来
  * 无需以前的包扫描配置
*
  * 想要改变扫描路径，@SpringBootApplication(scanBasePackages=**"com.atguigu"**)
*
  *
    * 或者@ComponentScan 指定扫描路径

```java
@SpringBootApplication
等同于
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan("com.atguigu.boot")
```

* 各种配置拥有默认值
*
  * 默认配置最终都是映射到某个类上，如：MultipartProperties
  * 配置文件的值最终会绑定每个类上，这个类会在容器中创建对象
* 按需加载所有自动配置项
*
  * 非常多的starter
  * 引入了哪些场景这个场景的自动配置才会开启
*
  * SpringBoot所有的自动配置功能都在 spring-boot-autoconfigure 包里面
  *

## 3、容器功能

### 3.1 组件添加

#### 1、@Configuration

* 基本使用
* **Full模式与Lite模式**
*
  * 示例
  * 最佳实战
*
  *
    * 配置 类组件之间无依赖关系用Lite模式加速容器启动过程，减少判断
    * 配置类组件之间有依赖关系，方法会被调用得到之前单实例组件，用Full模式

```java
============myConfig==================
/**
 * 1、配置类里面使用@Bean标注在方法上给容器注册组件，默认也是单实例的
 * 2、配置类本身也是组件
 * 3、proxyBeanMethods：代理bean的方法
 *      Full(proxyBeanMethods = true)、【保证每个@Bean方法被调用多少次返回的组件都是单实例的】
 *      Lite(proxyBeanMethods = false)【每个@Bean方法被调用多少次返回的组件都是新创建的】
 *      组件依赖必须使用Full模式默认。其他默认是否Lite模式
 */
@Configuration(proxyBeanMethods = true)
public class MyConfig {

    @Bean("user01")
    public User getUser(){
        User zhangsan = new User("zhangsan", 18);
        //user组件依赖了Pet组件
        zhangsan.setPet(TomPet());
        return zhangsan;

    }

    @Bean("tom")
    public Pet TomPet(){
        return new Pet("小南");
    }
}
==================configuration使用==============
    @SpringBootApplication(scanBasePackages = "com.aimer.boot")
public class MainApplication {
    public static void main(String[] args) {
//        1、返回我们的IOC容器
        ConfigurableApplicationContext run = SpringApplication.run(MainApplication.class, args);
//        2、查看容器里面的组件
        String[] names = run.getBeanDefinitionNames();
        for (String name : names) {
            System.out.println(name);
        }

//        3、从容器中获取组件
        Pet tom01 = run.getBean("tom", Pet.class);

        Pet tom02 = run.getBean("tom", Pet.class);

        System.out.println("组件："+(tom01 == tom02));

//        4、com.aimer.boot.config.MyConfig$$EnhancerBySpringCGLIB$$a600d077@6884f0d9
//        spring的增强类，MyConfig添加了注解也变成了spring中的组件
        MyConfig config = run.getBean(MyConfig.class);
        System.out.println(config);

//      如果@Configuration(proxyBeanMethods = true)代理对象调用方法。SpringBoot总会检查这个组件是否在容器中有。
//      保持组件单实例
        User user01 = config.getUser();
        User user02 = config.getUser();
        System.out.println("用户："+(user01 == user02));

        User user1 = run.getBean("user01", User.class);
        Pet tom = run.getBean("tom", Pet.class);

        System.out.println("用户的宠物："+(user1.getPet() == tom));

    }
}
```

#### 2、@Bean、@Component、@Controller、@Service、@Repository

这些组件的用法都和SpringMVC中相同，只要被扫描到就可以使用

#### 3、@ComponentScan、@Import

@ComponentScan前面也已经说了，被主方法@SpringBootApplication兼并了

```java
@SpringBootApplication(scanBasePackages = "com.aimer.boot")
```

```java
@Import({User.class, DBHelper.class}) *      给容器中自动创建出这两个类型的组件、默认组件的名字就是全类名 
    String[] names1 = run.getBeanNamesForType(User.class);
        for (String s : names1) {
            System.out.println(s);
        }
        DBHelper bean = run.getBean(DBHelper.class);
        System.out.println(bean);

//输出：
//com.aimer.boot.bean.User
//user01
//ch.qos.logback.core.db.DBHelper@70cccd8f
//user01是myconfig里面定义的，其他两个都是导入的
```

### 4、@Conditional

条件装配：满足Conditional指定的条件，则进行组件注入

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1602835786727-28b6f936-62f5-4fd6-a6c5-ae690bd1e31d.png?x-oss-process=image%2Fwatermark%2Ctype\_d3F5LW1pY3JvaGVp%2Csize\_17%2Ctext\_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor\_FFFFFF%2Cshadow\_50%2Ct\_80%2Cg\_se%2Cx\_10%2Cy\_10)

```java
//@ConditionalOnBean(name = "tom")若存在name为tom的组件就执行装配下面的组件
@ConditionalOnMissingBean(name = "tom")//若不存在name为tom的组件就执行装配下面的组件
//这个方法放在类上面就是执行的装配是类中的全部方法
public class MyConfig {

    //若注解是放在方法上面就只对该方法生效
    @ConditionalOnBean(name="tom")
    @Bean("user01")
    public User getUser(){}

}
```

### 5、@ImportResource

```java
@ImportResource("classpath:beans.xml")
随意放在一个类上面就开源使用
可以导入spring配置文件，像spring一样使用xml配置
```

### 6、@configurationProperites

这个注解可以将写在配置文件里面的属性和类进行绑定

```
@Component
@ConfigurationProperties(prefix = "mycar")
public class Car {
    String brand;
    Integer price;
}
===========在application.properties文件里面写======
mycar.brand=byd
mycar.price=10000
```

```java
//在controller中写上
    @Autowired
    Car car;
    @RequestMapping("/car")
    public Car car(){
        return car;
    }
```

另一种方式：

```java
//在配置类上面使用注解绑定类,在配置类上面注解是因为myconfig是已经注册的组件，在主方法类上也是可以的
@EnableConfigurationProperties(Car.class)
public class MyConfig {

    //在类上面仍需要一个注解
    @ConfigurationProperties(prefix = "mycar")
    public class Car {    
```

## 4、自动配置原理

### 4.1 引导加载自动配置类

```java
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
public @interface SpringBootApplication {
```

#### 1、@SpringBootConfiguration

@Configuration.代表当前是一个配置类

```java
@Configuration
public @interface SpringBootConfiguration {
```

#### 2、@ComponentScan

指定扫描哪些，spring注解

#### 3、EnableAutoConfiguration

```java
@AutoConfigurationPackage
@Import({AutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {
  //
```

**1 @AutoConfigurationPackage**

自动配置包，指定了默认的包规则

```java
@Import({Registrar.class})
public @interface AutoConfigurationPackage {
    //利用Registrar给容器导入一系列组件
    //将指定的一个包下的所有组件导入进来，Main Application所在的包下
```

**2、@Import(AutoConfigurationImportSelector.class)**

AutoConfigurationImportSelector

```java
1、利用getAutoConfigurationEntry(annotationMetadata);给容器中批量导入一些组件
2、调用List<String> configurations = getCandidateConfigurations(annotationMetadata, attributes)获取到所有需要导入到容器中的配置类
3、利用工厂加载 Map<String, List<String>> loadSpringFactories(@Nullable ClassLoader classLoader)；得到所有的组件
4、从META-INF/spring.factories位置来加载一个文件。
    默认扫描我们当前系统里面所有META-INF/spring.factories位置的文件
    spring-boot-autoconfigure-2.3.4.RELEASE.jar包里面也有META-INF/spring.factories
```

![image-20211025163626715](C:%5CUsers%5Clilia%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20211025163626715.png)

```xml
文件里面写死了spring-boot一启动就要给容器中加载的所有配置类
spring-boot-autoconfigure-2.3.4.RELEASE.jar/META-INF/spring.factories
# Auto Configure
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
org.springframework.boot.autoconfigure.admin.SpringApplicationAdminJmxAutoConfiguration,\
org.springframework.boot.autoconfigure.aop.AopAutoConfiguration,\
org.springframework.boot.autoconfigure.amqp.RabbitAutoConfiguration,\
org.springframework.boot.autoconfigure.batch.BatchAutoConfiguration,\
org.springframework.boot.autoconfigure.cache.CacheAutoConfiguration,\
org.springframework.boot.autoconfigure.cassandra.CassandraAutoConfiguration,\
org.springframework.boot.autoconfigure.context.ConfigurationPropertiesAutoConfiguration,\
org.springframework.boot.autoconfigure.context.LifecycleAutoConfiguration,\
org.springframework.boot.autoconfigure.context.MessageSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.context.PropertyPlaceholderAutoConfiguration,\
org.springframework.boot.autoconfigure.couchbase.CouchbaseAutoConfiguration,\
org.springframework.boot.autoconfigure.dao.PersistenceExceptionTranslationAutoConfiguration,\
org.springframework.boot.autoconfigure.data.cassandra.CassandraDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.cassandra.CassandraReactiveDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.cassandra.CassandraReactiveRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.cassandra.CassandraRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.couchbase.CouchbaseDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.couchbase.CouchbaseReactiveDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.couchbase.CouchbaseReactiveRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.couchbase.CouchbaseRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.elasticsearch.ElasticsearchDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.elasticsearch.ElasticsearchRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.elasticsearch.ReactiveElasticsearchRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.elasticsearch.ReactiveElasticsearchRestClientAutoConfiguration,\
org.springframework.boot.autoconfigure.data.jdbc.JdbcRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.jpa.JpaRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.ldap.LdapRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.mongo.MongoDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.mongo.MongoReactiveDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.mongo.MongoReactiveRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.mongo.MongoRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.neo4j.Neo4jDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.neo4j.Neo4jRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.solr.SolrRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.r2dbc.R2dbcDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.r2dbc.R2dbcRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.r2dbc.R2dbcTransactionManagerAutoConfiguration,\
org.springframework.boot.autoconfigure.data.redis.RedisAutoConfiguration,\
org.springframework.boot.autoconfigure.data.redis.RedisReactiveAutoConfiguration,\
org.springframework.boot.autoconfigure.data.redis.RedisRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.rest.RepositoryRestMvcAutoConfiguration,\
org.springframework.boot.autoconfigure.data.web.SpringDataWebAutoConfiguration,\
org.springframework.boot.autoconfigure.elasticsearch.ElasticsearchRestClientAutoConfiguration,\
org.springframework.boot.autoconfigure.flyway.FlywayAutoConfiguration,\
org.springframework.boot.autoconfigure.freemarker.FreeMarkerAutoConfiguration,\
org.springframework.boot.autoconfigure.groovy.template.GroovyTemplateAutoConfiguration,\
org.springframework.boot.autoconfigure.gson.GsonAutoConfiguration,\
org.springframework.boot.autoconfigure.h2.H2ConsoleAutoConfiguration,\
org.springframework.boot.autoconfigure.hateoas.HypermediaAutoConfiguration,\
org.springframework.boot.autoconfigure.hazelcast.HazelcastAutoConfiguration,\
org.springframework.boot.autoconfigure.hazelcast.HazelcastJpaDependencyAutoConfiguration,\
org.springframework.boot.autoconfigure.http.HttpMessageConvertersAutoConfiguration,\
org.springframework.boot.autoconfigure.http.codec.CodecsAutoConfiguration,\
org.springframework.boot.autoconfigure.influx.InfluxDbAutoConfiguration,\
org.springframework.boot.autoconfigure.info.ProjectInfoAutoConfiguration,\
org.springframework.boot.autoconfigure.integration.IntegrationAutoConfiguration,\
org.springframework.boot.autoconfigure.jackson.JacksonAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.JdbcTemplateAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.JndiDataSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.XADataSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.DataSourceTransactionManagerAutoConfiguration,\
org.springframework.boot.autoconfigure.jms.JmsAutoConfiguration,\
org.springframework.boot.autoconfigure.jmx.JmxAutoConfiguration,\
org.springframework.boot.autoconfigure.jms.JndiConnectionFactoryAutoConfiguration,\
org.springframework.boot.autoconfigure.jms.activemq.ActiveMQAutoConfiguration,\
org.springframework.boot.autoconfigure.jms.artemis.ArtemisAutoConfiguration,\
org.springframework.boot.autoconfigure.jersey.JerseyAutoConfiguration,\
org.springframework.boot.autoconfigure.jooq.JooqAutoConfiguration,\
org.springframework.boot.autoconfigure.jsonb.JsonbAutoConfiguration,\
org.springframework.boot.autoconfigure.kafka.KafkaAutoConfiguration,\
org.springframework.boot.autoconfigure.availability.ApplicationAvailabilityAutoConfiguration,\
org.springframework.boot.autoconfigure.ldap.embedded.EmbeddedLdapAutoConfiguration,\
org.springframework.boot.autoconfigure.ldap.LdapAutoConfiguration,\
org.springframework.boot.autoconfigure.liquibase.LiquibaseAutoConfiguration,\
org.springframework.boot.autoconfigure.mail.MailSenderAutoConfiguration,\
org.springframework.boot.autoconfigure.mail.MailSenderValidatorAutoConfiguration,\
org.springframework.boot.autoconfigure.mongo.embedded.EmbeddedMongoAutoConfiguration,\
org.springframework.boot.autoconfigure.mongo.MongoAutoConfiguration,\
org.springframework.boot.autoconfigure.mongo.MongoReactiveAutoConfiguration,\
org.springframework.boot.autoconfigure.mustache.MustacheAutoConfiguration,\
org.springframework.boot.autoconfigure.orm.jpa.HibernateJpaAutoConfiguration,\
org.springframework.boot.autoconfigure.quartz.QuartzAutoConfiguration,\
org.springframework.boot.autoconfigure.r2dbc.R2dbcAutoConfiguration,\
org.springframework.boot.autoconfigure.rsocket.RSocketMessagingAutoConfiguration,\
org.springframework.boot.autoconfigure.rsocket.RSocketRequesterAutoConfiguration,\
org.springframework.boot.autoconfigure.rsocket.RSocketServerAutoConfiguration,\
org.springframework.boot.autoconfigure.rsocket.RSocketStrategiesAutoConfiguration,\
org.springframework.boot.autoconfigure.security.servlet.SecurityAutoConfiguration,\
org.springframework.boot.autoconfigure.security.servlet.UserDetailsServiceAutoConfiguration,\
org.springframework.boot.autoconfigure.security.servlet.SecurityFilterAutoConfiguration,\
org.springframework.boot.autoconfigure.security.reactive.ReactiveSecurityAutoConfiguration,\
org.springframework.boot.autoconfigure.security.reactive.ReactiveUserDetailsServiceAutoConfiguration,\
org.springframework.boot.autoconfigure.security.rsocket.RSocketSecurityAutoConfiguration,\
org.springframework.boot.autoconfigure.security.saml2.Saml2RelyingPartyAutoConfiguration,\
org.springframework.boot.autoconfigure.sendgrid.SendGridAutoConfiguration,\
org.springframework.boot.autoconfigure.session.SessionAutoConfiguration,\
org.springframework.boot.autoconfigure.security.oauth2.client.servlet.OAuth2ClientAutoConfiguration,\
org.springframework.boot.autoconfigure.security.oauth2.client.reactive.ReactiveOAuth2ClientAutoConfiguration,\
org.springframework.boot.autoconfigure.security.oauth2.resource.servlet.OAuth2ResourceServerAutoConfiguration,\
org.springframework.boot.autoconfigure.security.oauth2.resource.reactive.ReactiveOAuth2ResourceServerAutoConfiguration,\
org.springframework.boot.autoconfigure.solr.SolrAutoConfiguration,\
org.springframework.boot.autoconfigure.task.TaskExecutionAutoConfiguration,\
org.springframework.boot.autoconfigure.task.TaskSchedulingAutoConfiguration,\
org.springframework.boot.autoconfigure.thymeleaf.ThymeleafAutoConfiguration,\
org.springframework.boot.autoconfigure.transaction.TransactionAutoConfiguration,\
org.springframework.boot.autoconfigure.transaction.jta.JtaAutoConfiguration,\
org.springframework.boot.autoconfigure.validation.ValidationAutoConfiguration,\
org.springframework.boot.autoconfigure.web.client.RestTemplateAutoConfiguration,\
org.springframework.boot.autoconfigure.web.embedded.EmbeddedWebServerFactoryCustomizerAutoConfiguration,\
org.springframework.boot.autoconfigure.web.reactive.HttpHandlerAutoConfiguration,\
org.springframework.boot.autoconfigure.web.reactive.ReactiveWebServerFactoryAutoConfiguration,\
org.springframework.boot.autoconfigure.web.reactive.WebFluxAutoConfiguration,\
org.springframework.boot.autoconfigure.web.reactive.error.ErrorWebFluxAutoConfiguration,\
org.springframework.boot.autoconfigure.web.reactive.function.client.ClientHttpConnectorAutoConfiguration,\
org.springframework.boot.autoconfigure.web.reactive.function.client.WebClientAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.DispatcherServletAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.ServletWebServerFactoryAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.error.ErrorMvcAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.HttpEncodingAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.MultipartAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.WebMvcAutoConfiguration,\
org.springframework.boot.autoconfigure.websocket.reactive.WebSocketReactiveAutoConfiguration,\
org.springframework.boot.autoconfigure.websocket.servlet.WebSocketServletAutoConfiguration,\
org.springframework.boot.autoconfigure.websocket.servlet.WebSocketMessagingAutoConfiguration,\
org.springframework.boot.autoconfigure.webservices.WebServicesAutoConfiguration,\
org.springframework.boot.autoconfigure.webservices.client.WebServiceTemplateAutoConfiguration
```

### 4.2 按需开启自动装配

```java
虽然我们127个场景的所有自动配置启动的时候默认全部加载。xxxxAutoConfiguration
按照条件装配规则（@Conditional），最终会按需配置。
```

### 4.3 修改默认配置

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
给容器中加入了文件上传解析器；
```

**SpringBoot默认会在底层配好所有的组件。但是如果用户自己配置了以用户的优先**

```java
@Bean
    @ConditionalOnMissingBean
    public CharacterEncodingFilter characterEncodingFilter() {
    }
//springboot也配置了字符编码过滤器，但是你自己要定义了就会使用你的
```

总结：

* SpringBoot先加载所有的自动配置类 xxxxxAutoConfiguration
* 每个自动配置类按照条件进行生效，默认都会绑定配置文件指定的值。xxxxProperties里面拿。xxxProperties和配置文件进行了绑定
* 生效的配置类就会给容器中装配很多组件
* 只要容器中有这些组件，相当于这些功能就有了
* 定制化配置
*
  * 用户直接自己@Bean替换底层的组件
  * 用户去看这个组件是获取的配置文件什么值就去修改。

**xxxxxAutoConfiguration ---> 组件 --->** **xxxxProperties里面拿值 ----> application.properties**

### 4.4、最佳实践

* 引入场景依赖
*
  * https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-starter
* 查看自动配置了哪些（选做）
*
  * 自己分析，引入场景对应的自动配置一般都生效了
  * 配置文件中debug=true开启自动配置报告。Negative（不生效）\Positive（生效）
* 是否需要修改
*
  * 参照文档修改配置项
*
  *
    * https://docs.spring.io/spring-boot/docs/current/reference/html/appendix-application-properties.html#common-application-properties
    * 自己分析。xxxxProperties绑定了配置文件的哪些。
*
  * 自定义加入或者替换组件
*
  *
    * @Bean、@Component。。。
*
  * 自定义器 **XXXXXCustomizer**；

## 5、开发小技巧

### 5.1、LomBok

简化JavaBean开发

```java
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>

idea中搜索安装lombok插件
```

```java
===============================简化JavaBean开发===================================
@NoArgsConstructor
//@AllArgsConstructor
@Data
@ToString
@EqualsAndHashCode
public class User {

    private String name;
    private Integer age;

    private Pet pet;

    public User(String name,Integer age){
        this.name = name;
        this.age = age;
    }
}

================================简化日志开发===================================
@Slf4j
@RestController
public class HelloController {
    @RequestMapping("/hello")
    public String handle01(@RequestParam("name") String name){

        log.info("请求进来了....");

        return "Hello, Spring Boot 2!"+"你好："+name;
    }
}
```

### 5.2、dev-tools

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <optional>true</optional>
        </dependency>
```

ctrl+F9 热加载（其实是重新启动，基本和idea里面点击重启一样，在后面静态页面会便捷一些直接更新）

### 5.3、spring initializr(项目初始化向导）

#### 1、选择我们需要的开发场景

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1354552/1602922147241-73fb2496-e795-4b5a-b909-a18c6011a028.png?x-oss-process=image%2Fwatermark%2Ctype\_d3F5LW1pY3JvaGVp%2Csize\_37%2Ctext\_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor\_FFFFFF%2Cshadow\_50%2Ct\_80%2Cg\_se%2Cx\_10%2Cy\_10)

#### 2、自动创建项目结构![image.png](https://cdn.nlark.com/yuque/0/2020/png/1354552/1602921758313-5099fe18-4c7b-4417-bf6f-2f40b9028296.png?x-oss-process=image%2Fwatermark%2Ctype\_d3F5LW1pY3JvaGVp%2Csize\_18%2Ctext\_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor\_FFFFFF%2Cshadow\_50%2Ct\_80%2Cg\_se%2Cx\_10%2Cy\_10)

#### 3、自动编写好主配置类

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1354552/1602922039074-79e98aad-8158-4113-a7e7-305b57b0a6bf.png?x-oss-process=image%2Fwatermark%2Ctype\_d3F5LW1pY3JvaGVp%2Csize\_29%2Ctext\_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor\_FFFFFF%2Cshadow\_50%2Ct\_80%2Cg\_se%2Cx\_10%2Cy\_10)

**以后我们写spring boot项目直接使用这个创建，我们专注于业务就可以了**

## 6、配置文件

### 1、文件类型

#### 1.1 properties

#### 1.2 yaml

#### 1.2.1、简介

YAML 是 "YAML Ain't Markup Language"（YAML 不是一种标记语言）的递归缩写。在开发的这种语言时，YAML 的意思其实是："Yet Another Markup Language"（仍是一种标记语言）。

非常适合用来做以数据为中心的配置文件

#### 1.2.2、基本语法

* key: value；kv之间有空格
* 大小写敏感
* 使用缩进表示层级关系
* 缩进不允许使用tab，只允许空格
* 缩进的空格数不重要，只要相同层级的元素左对齐即可
* '#'表示注释
* 字符串无需加引号，如果要加，''与""表示字符串内容 会被 转义/不转义

#### 1.2.3、数据类型

* 字面量：单个的、不可再分的值。date、boolean、string、number、null

```yaml
k: v
```

* 对象：键值对的集合。map、hash、set、object

```yaml
行内写法：  k: {k1:v1,k2:v2,k3:v3}
#或
k: 
  k1: v1
  k2: v2
  k3: v3
```

* 数组：一组按次序排列的值。array、list、queue

```yam
行内写法：  k: [v1,v2,v3]
#或者
k:
 - v1
 - v2
 - v3
```

#### 1.2.4、示例

```java
@Data
public class Person {

    private String userName;
    private Boolean boss;
    private Date birth;
    private Integer age;
    private Pet pet;
    private String[] interests;
    private List<String> animal;
    private Map<String, Object> score;
    private Set<Double> salarys;
    private Map<String, List<Pet>> allPets;
}

@Data
public class Pet {
    private String name;
    private Double weight;
}
```

```yaml
# yaml表示以上对象
person:
  userName: "zhang \n san"
  #单引号会将\n作为字符串输出， 双引号会将\n作为换行符输出
  #单引号会转义，双引号不会转移，\n本身就是个转义字符它的含义就是换行
  boss: false
  birth: 2019/12/12 20:12:33
  age: 18
  pet: 
    name: tomcat
    weight: 23.4
  interests: [篮球,游泳]
  animal: 
    - jerry
    - mario
  score:
    english: 
      first: 30
      second: 40
      third: 50
    math: [131,140,148]
    chinese: {first: 128,second: 136}
  salarys: [3999,4999.98,5999.99]
  allPets:
    sick:
      - {name: tom}
      - {name: jerry,weight: 47}
      -
        name: QAQ
        weight: 990
    health: [{name: mario,weight: 47}]
```

### 2、配置提示

自定义的类和配置文件绑定一般没有提示。

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>

<!--设置打包的时候不要把这个组件打包，因为这个组件只是在开发的时候方便我们查看，对用户没有用，防止JVM启动的时候加载太多无用的东西所有我们排除这个组件-->
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

## 7、Web开发

### 1、springMVC自动配置概览

Spring Boot provides auto-configuration for Spring MVC that **works well with most applications.(大多场景我们都无需自定义配置)**

The auto-configuration adds the following features on top of Spring’s defaults:

* Inclusion of `ContentNegotiatingViewResolver` and `BeanNameViewResolver` beans.
*
  * 内容协商视图解析器和BeanName视图解析器
* Support for serving static resources, including support for WebJars (covered [later in this document](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-spring-mvc-static-content))).
*
  * 静态资源（包括webjars）
* Automatic registration of `Converter`, `GenericConverter`, and `Formatter` beans.
*
  * 自动注册 `Converter，GenericConverter，Formatter`
* Support for `HttpMessageConverters` (covered [later in this document](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-spring-mvc-message-converters)).
*
  * 支持 `HttpMessageConverters` （后来我们配合内容协商理解原理）
* Automatic registration of `MessageCodesResolver` (covered [later in this document](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-spring-message-codes)).
*
  * 自动注册 `MessageCodesResolver` （国际化用）
* Static `index.html` support.
*
  * 静态index.html 页支持
* Custom `Favicon` support (covered [later in this document](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-spring-mvc-favicon)).
*
  * 自定义 `Favicon`
* Automatic use of a `ConfigurableWebBindingInitializer` bean (covered [later in this document](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-spring-mvc-web-binding-initializer)).
*
  * 自动使用 `ConfigurableWebBindingInitializer` ，（DataBinder负责将请求数据绑定到JavaBean上）

If you want to keep those Spring Boot MVC customizations and make more [MVC customizations](https://docs.spring.io/spring/docs/5.2.9.RELEASE/spring-framework-reference/web.html#mvc) (interceptors, formatters, view controllers, and other features), you can add your own `@Configuration` class of type `WebMvcConfigurer` but **without** `@EnableWebMvc`.

**不用@EnableWebMvc注解。使用** `**@Configuration**` **+** `**WebMvcConfigurer**` **自定义规则**

If you want to provide custom instances of `RequestMappingHandlerMapping`, `RequestMappingHandlerAdapter`, or `ExceptionHandlerExceptionResolver`, and still keep the Spring Boot MVC customizations, you can declare a bean of type `WebMvcRegistrations` and use it to provide custom instances of those components.

**声明** `**WebMvcRegistrations**` **改变默认底层组件**

If you want to take complete control of Spring MVC, you can add your own `@Configuration` annotated with `@EnableWebMvc`, or alternatively add your own `@Configuration`-annotated `DelegatingWebMvcConfiguration` as described in the Javadoc of `@EnableWebMvc`.

**使用** `**@EnableWebMvc+@Configuration+DelegatingWebMvcConfiguration 全面接管SpringMVC**`

### 2、简单功能分析

#### 2.1、静态资源访问

**1、静态资源目录**

只要静态资源在类路径下：called `/static` (or `/public` or `/resources` or `/META-INF/resources`

访问 ： **当前项目根路径/ + 静态资源名**

原理：静态映射/\*\*

请求进来，先去找Controller看能不能处理，不难处理的所有请求又都交给静态资源处理器。静态资源也找不到则响应404页面

改变默认静态资源啊路径

```yaml
spring:
  mvc:
    static-path-pattern: /res/**

  resources:
    static-locations: [classpath:/haha/]
    #这里不需要空格它是一个String，并不是键值对
```

**2、静态资源访问前缀**

默认无前缀

```yaml
spring:
  mvc:
    static-path-pattern: /res/**
```

当前项目+static-path-pattern +静态资源名 = 静态资源文件夹下找

**3、webjar**

自动映射/webjar/\*\*

https://www.webjars.org/

```xml
        <dependency>
            <groupId>org.webjars</groupId>
            <artifactId>jquery</artifactId>
            <version>3.5.1</version>
        </dependency>
```

访问地址：[http://localhost:8080/webjars/**jquery/3.5.1/jquery.js**](http://localhost:8080/webjars/jquery/3.5.1/jquery.js) 后面地址要按照依赖里面的包路径

webjar是自动映射所有静态资源访问前缀对它无影响，当然加上也没有影响

#### 2.2、欢迎页支持

* 静态资源路径下 index.html
*
  * 可以配置静态资源路径
  * 但是不可以配置静态资源的访问前缀。否则导致 index.html不能被默认访问

```yaml
spring:
#  mvc:
#    static-path-pattern: /res/**   这个会导致welcome page功能失效

  resources:
    static-locations: [classpath:/haha/]
```

* Controller能处理/index

spring boot中设置有限查找controller请求中的/index，controller处理不了再去静态资源中找

#### 2.3、自定义Favicon

favicon.ico 放在静态资源目录下即可。spring boot会自动查找

```yaml
spring:
#  mvc:
#    static-path-pattern: /res/**   这个会导致 Favicon 功能失效
#springboot底层都是写死的不会自动映射，所以不能添加前缀
```

#### 2.4、静态资源配置原理

* SpringBoot启动默认加载 xxxAutoConfiguration 类（自动配置类）
* SpringMVC功能的自动配置类 WebMvcAutoConfiguration，生效

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

* 给容器中配了什么。

```java
    @Configuration(proxyBeanMethods = false)
    @Import(EnableWebMvcConfiguration.class)
    @EnableConfigurationProperties({ WebMvcProperties.class, ResourceProperties.class })
    @Order(0)
    public static class WebMvcAutoConfigurationAdapter implements WebMvcConfigurer {}
```

* 配置文件的相关属性和xxx进行了绑定。WebMvcProperties==**spring.mvc**、ResourceProperties==**spring.resources**

**1、配置类只有一个有参构造器**

```java
    //有参构造器所有参数的值都会从容器中确定
//ResourceProperties resourceProperties；获取和spring.resources绑定的所有的值的对象
//WebMvcProperties mvcProperties 获取和spring.mvc绑定的所有的值的对象
//ListableBeanFactory beanFactory Spring的beanFactory
//HttpMessageConverters 找到所有的HttpMessageConverters
//ResourceHandlerRegistrationCustomizer 找到 资源处理器的自定义器。=========
//DispatcherServletPath  
//ServletRegistrationBean   给应用注册Servlet、Filter....
    public WebMvcAutoConfigurationAdapter(ResourceProperties resourceProperties, WebMvcProperties mvcProperties,
                ListableBeanFactory beanFactory, ObjectProvider<HttpMessageConverters> messageConvertersProvider,
                ObjectProvider<ResourceHandlerRegistrationCustomizer> resourceHandlerRegistrationCustomizerProvider,
                ObjectProvider<DispatcherServletPath> dispatcherServletPath,
                ObjectProvider<ServletRegistrationBean<?>> servletRegistrations) {
            this.resourceProperties = resourceProperties;
            this.mvcProperties = mvcProperties;
            this.beanFactory = beanFactory;
            this.messageConvertersProvider = messageConvertersProvider;
            this.resourceHandlerRegistrationCustomizer = resourceHandlerRegistrationCustomizerProvider.getIfAvailable();
            this.dispatcherServletPath = dispatcherServletPath;
            this.servletRegistrations = servletRegistrations;
        }
```

**2、资源处理的默认规则**

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

**3、欢迎页的处理规则**

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

**4、favicon**

### 3、请求参数处理

#### 1、请求映射

**1.1、Rest风格使用与原理**

* @xxxMapping；
* Rest风格支持（_使用**HTTP**请求方式动词来表示对资源的操作_）
*
  * _以前：\*\*/getUser_ _获取用户_ _/deleteUser_ _删除用户_ _/editUser_ _修改用户_ _/saveUser_ _保存用户_
  * _现在： /user_ \*GET-\*_获取用户_ \*DELETE-\*_删除用户_ \*PUT-\*_修改用户_ \*POST-\*_保存用户_
*
  * 核心Filter；HiddenHttpMethodFilter
*
  *
    * 用法： 表单method=post，隐藏域 \_method=put
    * SpringBoot中手动开启
*
  * 扩展：如何把\_method 这个名字换成我们自己喜欢的。

```java
    @RequestMapping(value = "/user",method = RequestMethod.GET)
    public String getUser(){
        return "GET-张三";
    }

    @RequestMapping(value = "/user",method = RequestMethod.POST)
    public String saveUser(){
        return "POST-张三";
    }


    @RequestMapping(value = "/user",method = RequestMethod.PUT)
    public String putUser(){
        return "PUT-张三";
    }

    @RequestMapping(value = "/user",method = RequestMethod.DELETE)
    public String deleteUser(){
        return "DELETE-张三";
    }

//关于使用rest的组件
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

Rest原理（表单提交要使用REST的时候）

* 表单提交会带上**\_method=PUT**
* **请求过来被**HiddenHttpMethodFilter拦截
*
  * 请求是否正常，并且是POST
*
  *
    * 获取到**\_method**的值。
    * 兼容以下请求；**PUT**.**DELETE**.**PATCH**
*
  *
    * **原生request（post），包装模式requesWrapper重写了getMethod方法，返回的是传入的值。**
    * **过滤器链放行的时候用wrapper。以后的方法调用getMethod是调用\*\*\*\*requesWrapper的。**

如果不是兼容的请求就默认使用自带的GET/POST

**Rest使用客户端工具，**

* 如PostMan直接发送Put、delete等方式请求，无需Filter。
* boot后面做微服务直接提供接口即可，不一定需要页面的交互

```yaml
spring:
  mvc:
    hiddenmethod:
      filter:
        enabled: true   #开启页面表单的Rest功能
```

**１.２、请求映射原理**

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1603181171918-b8acfb93-4914-4208-9943-b37610e93864.png?x-oss-process=image%2Fwatermark%2Ctype\_d3F5LW1pY3JvaGVp%2Csize\_27%2Ctext\_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor\_FFFFFF%2Cshadow\_50%2Ct\_80%2Cg\_se%2Cx\_10%2Cy\_10)

SpringMVC功能分析都从 org.springframework.web.servlet.DispatcherServlet-》doDispatch（）

最后由doDispatch完成

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

![image-20211028154149670](C:%5CUsers%5Clilia%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20211028154149670.png)

**RequestMappingHandlerMapping**：保存了所有@RequestMapping 和handler的映射规则。

![image-20211028154348699](C:%5CUsers%5Clilia%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20211028154348699.png)

所有的请求映射都在HandlerMapping中

* SpringBoot自动配置欢迎页的 WelcomePageHandlerMapping 。访问 /能访问到index.html；
* SpringBoot自动配置了默认 的 RequestMappingHandlerMapping
* 请求进来，挨个尝试所有的HandlerMapping看是否有请求信息。
*
  * 如果有就找到这个请求对应的handler
  * 如果没有就是下一个 HandlerMapping
* 我们需要一些自定义的映射处理，我们也可以自己给容器中放**HandlerMapping**。自定义 **HandlerMapping**

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

### 5、视图解析与模板引擎

#### 1、视图解析

1、目标方法处理的过程中，所有数据都会被放在 **ModelAndViewContainer 里面。包括数据和视图地址**

**2、方法的参数是一个自定义类型对象（从请求参数中确定的），把他重新放在** **ModelAndViewContainer**

**3、任何目标方法执行完成以后都会返回 ModelAndView（数据和视图地址）。**

\*\*4、\*\***processDispatchResult 处理派发结果（页面改如何响应）**

* 1、**render**(**mv**, request, response); 进行页面渲染逻辑
*
  * 1、根据方法的String返回值得到 **View** 对象【定义了页面的渲染逻辑】
*
  *
    * 1、所有的视图解析器尝试是否能根据当前返回值得到**View**对象
    * 2、得到了 **redirect:/main.html** --> Thymeleaf new **RedirectView**()
*
  *
    * 3、ContentNegotiationViewResolver 里面包含了下面所有的视图解析器，内部还是利用下面所有视图解析器得到视图对象。
    * 4、view.render(mv.getModelInternal(), request, response); 视图对象调用自定义的render进行页面渲染工作
*
  *
    *
      * **RedirectView 如何渲染【重定向到一个页面】**
      * **1、获取目标url地址**
*
  *
    *
      * **2、response.sendRedirect(encodedURL);**

**视图解析：**

*
  * **返回值以 forward: 开始： new InternalResourceView(forwardUrl); --> 转发request.getRequestDispatcher(path).forward(request, response);**
  * **返回值以** **redirect: 开始：** **new RedirectView() --》 render就是重定向**
*
  * **返回值是普通字符串： new ThymeleafView（）--->**

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605680247945-088b0f17-185c-490b-8889-103e8b4d8c07.png?x-oss-process=image%2Fwatermark%2Ctype\_d3F5LW1pY3JvaGVp%2Csize\_16%2Ctext\_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor\_FFFFFF%2Cshadow\_50%2Ct\_80%2Cg\_se%2Cx\_10%2Cy\_10)

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605679959020-54b96fe7-f2fc-4b4d-a392-426e1d5413de.png?x-oss-process=image%2Fwatermark%2Ctype\_d3F5LW1pY3JvaGVp%2Csize\_23%2Ctext\_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor\_FFFFFF%2Cshadow\_50%2Ct\_80%2Cg\_se%2Cx\_10%2Cy\_10)

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605679471537-7db702dc-b165-4dc6-b64a-26459ee5fd6c.png?x-oss-process=image%2Fwatermark%2Ctype\_d3F5LW1pY3JvaGVp%2Csize\_17%2Ctext\_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor\_FFFFFF%2Cshadow\_50%2Ct\_80%2Cg\_se%2Cx\_10%2Cy\_10)

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605679913592-151a616a-c754-4da3-a2c1-91dc0230a48d.png?x-oss-process=image%2Fwatermark%2Ctype\_d3F5LW1pY3JvaGVp%2Csize\_22%2Ctext\_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor\_FFFFFF%2Cshadow\_50%2Ct\_80%2Cg\_se%2Cx\_10%2Cy\_10)

#### 2、模板引擎-Thymeleaf

**1、thymeleaf简介**

Thymeleaf is a modern server-side Java template engine for both web and standalone environments, capable of processing HTML, XML, JavaScript, CSS and even plain text.

**现代化、服务端Java模板引擎**

**2、基本语法**

**1、表达式**

| 表达式名称 | 语法   | 用途                     |
| ----- | ---- | ---------------------- |
| 变量取值  | ${}  | 获取请求域、session域、对象等值    |
| 选择变量  | \*{} | 获取上下文对象值               |
| 消息    | #{}  | 获取国际化对象值               |
| 链接    | @{}  | 生成链接                   |
| 片段表达式 | \~{} | JSP:include作用，引入公共页面片段 |

**2、字面量**

文本值: **'one text'** **,** **'Another one!'** \*\*,…\*\*数字: **0** **,** **34** **,** **3.0** **,** **12.3** \*\*,…\*\*布尔值: **true** **,** **false**

空值: **null**

变量： one，two，.... 变量不能有空格

**3、文本操作**

字符串拼接: **+**

变量替换: **|The name is ${name}|**

**4、数学运算**

运算符: + , - , \* , / , %

**5、布尔运算**

运算符: **and** **,** **or**

一元运算: **!** **,** **not**

**6、比较运算**

比较: **>** **,** **<** **,** **>=** **,** **<=** **(** **gt** **,** **lt** **,** **ge** **,** **le** \*\*)\*\*等式: **==** **,** **!=** **(** **eq** **,** **ne** **)**

**7、条件运算**

If-then: **(if) ? (then)**

If-then-else: **(if) ? (then) : (else)**

Default: (value) **?: (defaultvalue)**

**8、特殊操作**

无操作： \_

**3、设置属性值-th:attr**

设置单个值

```html
<form action="subscribe.html" th:attr="action=@{/subscribe}">
  <fieldset>
    <input type="text" name="email" />
    <input type="submit" value="Subscribe!" th:attr="value=#{subscribe.submit}"/>
  </fieldset>
</form>
```

设置多个值

```html
<img src="../../images/gtvglogo.png"  th:attr="src=@{/images/gtvglogo.png},title=#{logo},alt=#{logo}" />
```

以上两个的代替写法 th:xxxx

```html
<input type="submit" value="Subscribe!" th:value="#{subscribe.submit}"/>
<form action="subscribe.html" th:action="@{/subscribe}">
```

所有h5兼容的标签写法

https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#setting-value-to-specific-attributes

**4、迭代**

```html
<tr th:each="prod : ${prods}">
        <td th:text="${prod.name}">Onions</td>
        <td th:text="${prod.price}">2.41</td>
        <td th:text="${prod.inStock}? #{true} : #{false}">yes</td>
</tr>
```

```html
<tr th:each="prod,iterStat : ${prods}" th:class="${iterStat.odd}? 'odd'">
  <td th:text="${prod.name}">Onions</td>
  <td th:text="${prod.price}">2.41</td>
  <td th:text="${prod.inStock}? #{true} : #{false}">yes</td>
</tr>
```

**5、条件运算**

```html
<a href="comments.html"
th:href="@{/product/comments(prodId=${prod.id})}"
th:if="${not #lists.isEmpty(prod.comments)}">view</a>
```

```html
<div th:switch="${user.role}">
  <p th:case="'admin'">User is an administrator</p>
  <p th:case="#{roles.manager}">User is a manager</p>
  <p th:case="*">User is some other thing</p>
</div>
```

**6、属性优先级**

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605498132699-4fae6085-a207-456c-89fa-e571ff1663da.png?x-oss-process=image%2Fwatermark%2Ctype\_d3F5LW1pY3JvaGVp%2Csize\_44%2Ctext\_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor\_FFFFFF%2Cshadow\_50%2Ct\_80%2Cg\_se%2Cx\_10%2Cy\_10)

#### 3、thymeleaf使用

**1、引入thymeleaf**

```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </dependency>
```

**2、自动配置好了thymeleaf**

```java
@Configuration(proxyBeanMethods = false)
@EnableConfigurationProperties(ThymeleafProperties.class)
@ConditionalOnClass({ TemplateMode.class, SpringTemplateEngine.class })
@AutoConfigureAfter({ WebMvcAutoConfiguration.class, WebFluxAutoConfiguration.class })
public class ThymeleafAutoConfiguration { }
```

自动配好的策略

* 1、所有thymeleaf的配置值都在 ThymeleafProperties
* 2、配置好了 **SpringTemplateEngine**
* **3、配好了** **ThymeleafViewResolver**
* 4、我们只需要直接开发页面

```java
public static final String DEFAULT_PREFIX = "classpath:/templates/";

public static final String DEFAULT_SUFFIX = ".html";  //xxx.html
```

**3、页面开发**

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title >Title</title>
</head>
<body>
<h1 th:text="${msg}">msg</h1>
<h2>
    <a href="https://www.wolai.com/aimer" th:href="${link}">wolai</a> </br>
    <a href="https://www.wolai.com/aimer" th:href="@{/link}">wolai</a>
</h2>
</body>
</html>
```

@{}使用链接，当你添加前缀时会自动添加进路径
