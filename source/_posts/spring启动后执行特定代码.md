# 1、PostConstruct注解

PostConstruct是Java5提供的一个注解，@PostConstruct该注解被用来修饰一个非静态的void（）方法。被@PostConstruct修饰的方法会在服务器加载Servlet的时候运行，并且只会被服务器执行一次。PostConstruct在构造函数之后执行，init（）方法之前执行

```JavaScript
@SpringBootApplication
public class AlipayDemoApplication implements ApplicationRunner, CommandLineRunner, InitializingBean {

    public static void main(String[] args) {
        System.out.println("main方法中run前面执行");
        SpringApplication.run(AlipayDemoApplication.class, args);
        System.out.println("main方法中run后面执行");
    }

    @PostConstruct
    public void init() {
        System.out.println("PostConstruct方法执行");
    }
}
```

# 2、@Async:标注方法为异步方法,在独立线程中运行

这个要在主启动类加上`@EnableAsync` ，这个方法必须在别处调用，否则不会自动运行

注意@Async需要使用到自定义线程池，不能使用默认线程池

```JavaScript
@Async
public void doSomethingAsync() {
    // ...
}
```

# 3、ApplicationRunner

实现接口的方法,在Spring Boot启动后自动运行

```JavaScript
@SpringBootApplication
public class AlipayDemoApplication implements ApplicationRunner, CommandLineRunner {

    public static void main(String[] args) {
        System.out.println("main方法中run前面执行");
        SpringApplication.run(AlipayDemoApplication.class, args);
        System.out.println("main方法中run后面执行");
    }

    @Override
    public void run(ApplicationArguments args) throws Exception {
        System.out.println("ApplicationRunner方法执行");
    }
}
```

# 4、CommandLineRunner

```JavaScript
@SpringBootApplication
public class AlipayDemoApplication implements ApplicationRunner, CommandLineRunner {

    public static void main(String[] args) {
        System.out.println("main方法中run前面执行");
        SpringApplication.run(AlipayDemoApplication.class, args);
        System.out.println("main方法中run后面执行");
    }

    @Override
    public void run(String... args) throws Exception {
        System.out.println("CommandLineRunner方法执行");
    }
}
```

# 5、在main方法中

```JavaScript
public static void main(String[] args) {
        System.out.println("main方法中run前面执行");
        SpringApplication.run(AlipayDemoApplication.class, args);
        System.out.println("main方法中run后面执行");
    }
```

# 6、使用@Bean注解定义初始化方法

@Bean注解是Spring Framework中的一个注解，它用于告诉Spring容器，这个方法将返回一个对象，该对象应该被注册为在Spring应用程序上下文中的bean。使用@Bean注解的方法可以在@Configuration类中声明，也可以在@Component类中声明。如果在@Configuration类中声明，那么@Bean方法将被CGLIB代理，以便能够拦截对该bean的调用并添加所需的事务管理、安全性检查等。如果在@Component类中声明，则不会进行代理。

```JavaScript
@Component
public class MyComponent {
    public void init() {
        // Startup code here  
    }
}

@Configuration
public class MyConfig {
    @Bean(initMethod = "init")
    public MyComponent myComponent() {
        return new MyComponent();
    }
}
```

`MyComponent `是一个普通的Java类，它有一个名为`init()`的初始化方法。当Spring容器创建该bean时，它将调用该方法

# 7、InitializingBean

```JavaScript
@SpringBootApplication
public class AlipayDemoApplication implements ApplicationRunner, CommandLineRunner, InitializingBean {

    public static void main(String[] args) {
        System.out.println("main方法中run前面执行");
        SpringApplication.run(AlipayDemoApplication.class, args);
        System.out.println("main方法中run后面执行");
    }

    @Override
    public void afterPropertiesSet() throws Exception {
        System.out.println("InitializingBean方法执行");
    }
}
```

# 8、Spring Boot监听器

Spring Boot提供了以下类型的监听器：

1. `ApplicationStartingEvent`：在应用程序启动开始时触发。
2. `ApplicationEnvironmentPreparedEvent`：当ApplicationContext准备好Environment时触发。
3. `ApplicationPreparedEvent`：在ApplicationContext准备刷新时触发，但在bean定义加载之后。
4. `ApplicationStartedEvent`：在ApplicationContext刷新并启动后，但在调用CommandLineRunner和ApplicationRunner之前触发。
5. `AvailabilityChangeEvent`：当应用程序状态更改为已启动、已停止或已暂停时触发。
6. `ApplicationFailedEvent`：当应用程序启动失败时触发。

例如，以下代码片段演示了如何使用ApplicationListener接口：

```JavaScript
@Component
public class applictionListener  implements ApplicationListener<ApplicationStartedEvent> {
    @Override
    public void onApplicationEvent(ApplicationStartedEvent event) {
        System.out.println("ApplicationStartedEvent 事件执行");
    }
}
@Component
public class applicationReadyEvent implements ApplicationListener<ApplicationReadyEvent> {
    @Override
    public void onApplicationEvent(ApplicationReadyEvent event) {
        System.out.println("ApplicationReadyEvent 事件执行");
    }
}
```

# 总结

上述几个方法的启动顺序：

main方法中run前面执行→

PostConstruct方法执行→ InitializingBean方法执行→

ApplicationStartedEvent 事件执行→ ApplicationRunner方法执行→ CommandLineRunner方法执行→ ApplicationReadyEvent 事件执行→ main方法中run后面执行