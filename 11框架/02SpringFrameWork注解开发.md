## Spring注解开发

![image-20211113163632002](C:/Users/Kevin/Desktop/image-20211113163632002.png)

```xml
<!--扫描com.itheima包及其子包下的类中注解-->
<context:component-scan base-package="com.itheima"/>
```

- 在类上使用@Component注解定义Bean
  - 如果@Component注解没有使用参数指定Bean的名称，那么类名首字母小写就是Bean在IOC容器中的默认名称。例如：BookServiceImpl对象在IOC容器中的名称是bookServiceImpl。
  - Spring提供**`@Component`**注解的三个衍生注解
    - **`@Controller`**：用于表现层bean定义
    - **`@Service`**：用于业务层bean定义
    - `@Repository`：用于数据层bean定义



### 纯注解开发

Spring3.0开启了纯注解开发模式，使用Java类替代配置文件

```java
@Configuration //用于设定当前类为配置类
@ComponentScan({service,dao}) //用于设定扫描路径，此注解只能添加一次，多个数据用数组格式
@PropertySource({"classpath:jdbc.properties"}) //加载配置文件
@Import({JdbcConfig.class}) //导入其他配置类
public class SpringConfig{}
```

```java
//加载配置文件初始化容器
ApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
```



#### bean作用范围注解配置

```java
@Scope("singleton")
```

#### bean生命周期注解配置

使用`@PostConstruct`、`@PreDestroy`定义bean生命周期

@PostConstruct和@PreDestroy注解是jdk中提供的注解，从jdk9开始，jdk中的javax.annotation包被移除,需要额外导依赖

```xml
<dependency>
  <groupId>javax.annotation</groupId>
  <artifactId>javax.annotation-api</artifactId>
  <version>1.3.2</version>
</dependency>
```

#### 依赖注入

```java
@Autowired //依赖注入，自动装配(按类型)
@Qualifier("dao") //自动装配bean时按bean名称装配
@Value("${name}") //简单类型注入
```



