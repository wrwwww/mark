### 概述

Spring的英文翻译为春天，可以说是给Java程序员带来了春天，因为它极大的简化了开发。我得出一个公式：Spring = 春天 = Java程序员的春天 = 简化开发。最后的简化开发正是Spring框架带来的最大好处。

Spring是一个开放源代码的设计层面框架，它是于2003 年兴起的一个轻量级的Java 开发框架。由Rod Johnson创建，其前身为Interface21框架，后改为了Spring并且正式发布。

### 核心

Spring 的理念：不去重新发明轮子。其核心是控制反转（IOC）和面向切面（AOP）

### spring配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    
</beans>
```

通过set方式注入

```xml
<!-- 通过空的构造方法创建类，然后通过set方法的方式注入-->
<bean id="add" class="com.manager.pojo.Address">
    <!--set注入 -->
      <property name="address" value="北京"/>
	<!--构造方法注入-->
    <constructor-arg index="0" value="太原"/>

</bean>
```

##### 复杂类型的注入

```xml
  <bean id="s1" class="com.manager.pojo.Student">
<!--        普通类型-->
        <property name="id" value="01"/>
<!--        对于引用类型-->
        <property name="address" ref="add"/>
        <property name="name" value="李沧海"/>
<!--        list-->
        <property name="hobby">
            <list>
                <value>看书</value>
            </list>
        </property>
<!--map-->
        <property name="book">
            <map>
                <entry value="且听风吟" key="村上春树"/>
            </map>
        </property>
<!--数组-->
        <property name="score">
            <array>
                <value>12</value>
            </array>
        </property>
     <!--空值--> 
<property name="email" value=""/>
       <null/>
    </bean>
```

p name space 以使用另外的set注入标签

```xml
xmlns:p="http://www.springframework.org/schema/p"
<!-- 普通的set注入-->
<bean name="classic" class="com.example.ExampleBean">
        <property name="email" value="someone@somewhere.com"/>
    </bean>
<!-- p- name space set注入-->
    <bean name="p-namespace" class="com.example.ExampleBean"
        p:email="someone@somewhere.com"/>
```

c name space 

```xml
```

#### scope 作用域

```xml
<!--
    scope 作用域
    Singletons 单例模式 从容器中得到的对象是同一个对象
    prototype  原型模式 从容器中得到的对象是不同的对象
-->

@Scope("prototype")
<bean id="s1" class="com.manager.pojo.Student" scope="prototype">
```

#### 自动装箱

- 使用@Autowired关键字 spring会自动注入依赖类型

![image-20220515194628908](C:\Users\wrw\AppData\Roaming\Typora\typora-user-images\image-20220515194628908.png)

```xml
<!--    这是手动装箱-->
    <bean id="people" class="com.manager.pojo.People">
        <property name="name" value="风吹麦浪"/>
        <property name="cat" ref="cat"/>
        <property name="dog" ref="dog"/>
    </bean>
<!--    自动装箱-->
    <bean id="people" class="com.manager.pojo.People" autowire="byName">
        <property name="name" value="风吹麦浪"/>
    </bean>
```

- 通过id名称匹配，通过类型匹配

```xml
autowire="byName"
autowire="byType"
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

</beans>
```

可以为属性为没有配置的bean为null

```java
@Autowired(required = false)

@Autowired
public void setMovieFinder(@Nullable MovieFinder movieFinder) {
        ...
}
```

##### @Qualifier(value = "dog12")

##### 配合@autowired实现按名称匹配

```xml
<bean id="dog2" class="com.manager.pojo.Dog">
        <qualifier value="12"/>
</bean>
<bean id="people" class="com.manager.pojo.People" autowire="byName">
    <!-- a-->
    <property name="name" value="风吹麦浪"/>
</bean>
```

### 使用java配置工具类进行配置

创建配置工具类Myconfig使用@Configuration注解标记工具类

```java
@Configuration
@Import(MyConfig2.class)  //导入合并其他配置类，类似于配置文件中的 inculde 标签
public class MyUtils1 {

    @Bean
    public User user(){
        return new User();
    }

}
```

```java
ApplicationContext context = new AnnotationConfigApplicationContext(MyUtils1.class);
User user = context.getBean("user", User.class);
System.out.println(user.name);
```

### spring整合mybatis

1. ##### 之前的mybatis使用

   maven导入mybatis依赖

   创建mybatis的xml配置

   idea连接mysql

   创建实体类

   写操作数据库持久层接口,写接口同名的映射xml文件,在xml文件中写sql

   将xml文件在mybatis中注册绑定

   写mybatis工具类,通过SqlSessionFactoryBuilder().build(inputStream);创建sessionFastory工厂,通过工厂便可以生产sqlsession

   ```java
   SqlSessionFactory sqlSessionFactory;
   String url="mybatis-config.xml";
   InputStream inputStream = Resources.getResourceAsStream(url);
   sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
   ```

   sqlsession便可以通过getMapper(AccountMapper.class)方法得到mapper去执行sql

   2. #### 整合后创建使用mybatis

      一种通过spring配置文件创建mybatis的方法

      导入spring-mybatis的依赖

      在bean上下文中配置mybatis的bean

      1. 这是数据源,即数据库的基本信息,驱动密码

      ```xml
        <!--配置数据源：数据源有非常多，可以使用第三方的，也可使使用Spring的-->
          <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
              <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
              <property name="url" value="jdbc:mysql://localhost:3306/shop?useSSL=true&amp;useUnicode=true&amp;characterEncoding=utf8"/>
              <property name="username" value="root"/>
              <property name="password" value="123"/>
          </bean>
      ```

      2. 这是sqlsessionfactorybean类似mybatis中的sqlsessionfactory用来创建sqlsession,它的参数分别是数据源,mybatis的配置,mapper文件

      ```xml
      <!--配置SqlSessionFactory-->
      <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
          <property name="dataSource" ref="dataSource"/>
          <!--关联Mybatis-->
          <property name="configLocation" value="classpath:mybatis-config.xml"/>
          <property name="mapperLocations" value="classpath*:com/manager/**/*.xml" />
      </bean>
      ```

   3. 使用线程安全的SqlSessionTemplate的类创建session,他的构建器需要sqlsessionfactory来创建

      ```xml
      <!--注册sqlSessionTemplate , 关联sqlSessionFactory-->
      <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
          <!--利用构造器注入-->
          <constructor-arg index="0" ref="sqlSessionFactory"/>
      </bean>
      ```

这样通过spring就把mybatis配置写好了,接下来需要实体类,需要dao接口,接口映射xml文件和接口实现类,在接口实现类中绑定一个sqlsession来调用接口方法实现,通过容器创建

```xml
   <bean id="accountDao" class="com.manager.dao.AccountMapperImpl">
        <property name="sqlSession" ref="sqlSession"/>
    </bean>
```


## 自动配置

这里拿RabbitMq来看，首先导入在pom.xml中导入依赖

```
<!-- https://mvnrepository.com/artifact/org.springframework.amqp/spring-rabbit -->
<dependency>
    <groupId>org.springframework.amqp</groupId>
    <artifactId>spring-rabbit</artifactId>
    <version>3.1.6</version>
</dependency>
```

查看yaml配置文件

```
spring:
  rabbitmq:
    addresses: localhost
```

通过idea的crtl+鼠标左键可以快速定位到spring auto configuration的位置

![](学习笔记/Attachments/1721309273966-c7fd66e3-b3b9-40dc-912a-14e468abcf68.png)

这就是springboot auto configuration的rabbitMq这也是为什么只要导入依赖就可以使用rabbitmq，spring给我们提供了默认的RabbotMq的配置实现，

`@ConfigurationProperties(prefix = "spring.rabbitmq")`该注解会从配置文件中加载spring.rabbitmq下的配置文件，从而注入到内部字段，覆盖配置。

![](学习笔记/Attachments/1721309507627-d2a2ac2c-34c6-44e2-bac5-857c879ae653.png)

同时可以看到该文件是在springboot auto configuration包下，上面的文件是用来配置基础配置，RabbitMq的连接池的初始化等工作都是在这个包下的amqp路径下进行的。一般我们操作rabbotmq是通过Springboot提供给我们的rabbitmqTemplat来操作

我们可以通过idea的自动装配快速定位到该对象的注入所在代码文件

![](学习笔记/Attachments/1721309800768-f691879e-1cf0-4f80-878e-e90c0f468574.png)

定位到

![](学习笔记/Attachments/1721309818499-993f1cd2-5f4f-41e3-9367-f740de8ab66c.png)

### 注解

#### ConditionalOnMissingBean

**ConditionalOnMissingBean是Spring Framework（特别是Spring Boot）提供的一个条件注解**。它的主要作用是在Spring应用上下文中没有找到指定类型的Bean时，才会实例化被注解的类或者方法。这个注解为Spring的自动配置和模块化设计提供了很大的灵活性，允许开发者根据条件动态地决定是否创建和注册Bean。

### 主要功能和特点

1. **条件化Bean创建**：

- 当Spring容器启动时，会检查当前上下文中是否已经存在某种类型的Bean。
- 如果不存在，才会根据@ConditionalOnMissingBean的配置来决定是否创建一个新的Bean实例并加入到容器中。

2. **使用场景**：

- **默认配置**：为应用程序提供默认的Bean配置，允许用户在需要时覆盖。
- **模块化设计**：在模块化应用程序中，根据实际情况有选择地加载Bean。
- **自定义扩展**：允许用户在不修改原始配置的情况下，自定义和扩展应用程序。

3. **参数和属性**：

- **value/name**：指定要检查的Bean类型或名称。可以是一个Class类型，也可以是一个字符串数组，指定Bean的名称。
- **annotation**：指定要检查的Bean是否标注了特定的注解。
- **match**：是否考虑Bean的派生类。默认为true，表示检查是否存在该类型及其子类的Bean。

4. **位置**：

- 可以用在类上或者方法上。
- 类级别：标注在类上时，表示如果容器中不存在该类的Bean实例，则创建该Bean。
- 方法级别：标注在方法上时，表示如果容器中不存在该方法返回类型的Bean实例，则调用该方法创建并注册一个Bean。

5. **与其他注解组合**：

- 可以与其他@Conditional注解组合使用，以实现更复杂的条件逻辑。

### 使用示例

```
@Configuration
public class MyConfiguration {

    @Bean
    @ConditionalOnMissingBean
    public MyService myService() {
        return new DefaultMyService();
    }

    @Bean
    public AnotherService anotherService() {
        return new AnotherService();
    }
}
```

在上面的示例中，`MyConfiguration` 类中定义了两个Bean，分别是 `myService` 和 `anotherService`。使用 `@ConditionalOnMissingBean` 注解修饰的 `myService` 方法，在容器中不存在 `MyService` 类型的Bean时才会被实例化，否则不会实例化。而 `anotherService` 方法则不受影响，始终会被实例化。

### 总结

#### `ConditionalOnMissingBean`

`ConditionalOnMissingBean` 是Spring Boot提供的一个非常有用的条件注解，它可以根据具体的条件动态地决定是否创建和注册Bean，从而实现更灵活和可定制的Bean管理和装配策略。这个注解广泛应用于Spring Boot应用程序中，特别是在自动配置和模块化设计中。正确使用这个注解可以提高应用配置的可维护性和可读性，但也需要注意Bean加载顺序和条件组合逻辑的复杂性。

`@ConditionalOnSingleCandidate`是Spring框架中的一个条件注解，它用于控制Bean的创建和注入。具体来说，当Spring容器中存在且**仅存在一个**指定的Bean时，才会注入该Bean。这个注解通常用于确保某个Bean的唯一性，避免因为存在多个相同类型的Bean而导致的注入冲突。

### 主要特点

- **唯一性检查**：它检查Spring容器中是否存在且仅存在一个特定类型的Bean。
- **条件性注入**：如果条件满足（即存在且仅存在一个Bean），则允许Bean的注入；如果不满足条件，则不注入该Bean。

### 使用场景

`@ConditionalOnSingleCandidate`注解通常用于以下几种场景：

1. **确保唯一性**：当应用程序中某个组件或功能需要依赖一个特定类型的Bean，并且这个Bean必须是唯一的时，可以使用`@ConditionalOnSingleCandidate`来确保这一点。
2. **避免冲突**：在大型项目中，可能存在多个相同类型的Bean，这可能会导致注入时的冲突。使用`@ConditionalOnSingleCandidate`可以避免这种冲突。
3. **条件性配置**：在某些情况下，可能需要根据容器中Bean的存在情况来条件性地配置某些组件或功能。`@ConditionalOnSingleCandidate`提供了这种条件判断的能力。

### 示例

假设有一个`MyService`接口，并且有两个实现类`MyServiceImpl1`和`MyServiceImpl2`，但在某个配置类中，我们只想注入一个实现类，并且这个实现类必须是唯一的。这时，我们可以使用`@ConditionalOnSingleCandidate`来确保这一点：

```
@Configuration
public class MyConfig {

    @Bean
    @ConditionalOnSingleCandidate(MyService.class)
    public MyService myService(MyService myService) {
        return myService;
    }

    // 其他Bean定义...
}
```

然而，需要注意的是，上述示例中的`@ConditionalOnSingleCandidate`用法实际上并不常见，因为它通常不会直接用于`@Bean`方法的定义上。更常见的用法是在其他注解（如`@Component`、`@Service`等）或配置类上，通过间接的方式来控制Bean的创建和注入。

在实际应用中，如果需要在Spring容器中存在且仅存在一个指定类型的Bean时才进行注入，可能会结合`@Autowired`注解的`required`属性（设置为`true`）或其他机制来实现。而`@ConditionalOnSingleCandidate`更多地是作为Spring Boot自动配置中的一部分，用于在自动配置类中根据条件来决定是否创建和注入Bean。

### 结论

`@ConditionalOnSingleCandidate`是Spring框架中一个重要的条件注解，它用于控制Bean的创建和注入，确保在Spring容器中存在且仅存在一个特定类型的Bean时才进行注入。这个注解在Spring Boot的自动配置中得到了广泛应用，为开发者提供了灵活的条件判断能力。

### 被修饰对象的自动注入

是的，你完全正确。在Spring框架中，当定义`@Bean`方法时，如果方法的参数与Spring容器中存在的Bean类型相匹配，Spring会自动将这些Bean注入到方法参数中。这个过程是Spring依赖注入（DI）功能的一部分，它确保了Bean之间的依赖关系能够自动地被解析和满足。

这种自动注入机制极大地简化了Bean之间的依赖管理，使得开发者不需要手动编写大量的代码来查找和注入依赖项。相反，开发者只需要在`@Bean`方法的参数中声明所需的依赖项，Spring就会负责在调用该方法之前找到并注入这些依赖项。

这种机制不仅适用于配置类中的`@Bean`方法，也适用于普通的Spring组件（即被`@Component`、`@Service`、`@Repository`等注解标注的类）中的字段和方法参数。在这些情况下，如果字段或方法参数上使用了`@Autowired`（虽然它现在是可选的，但可以作为文档或显式声明的手段）或其他类似的注入注解（如`@Inject`），Spring也会尝试自动注入匹配的Bean。

然而，在`@Bean`方法的情况下，由于Spring知道它是在处理Bean的创建过程，因此它可以更智能地处理依赖注入，而不需要显式的`@Autowired`注解（尽管加上它也不会有问题）。这意呀着，只要`@Bean`方法的参数类型与Spring容器中的某个Bean类型相匹配，Spring就会自动注入该Bean，而无需开发者进行任何额外的配置或编码。

在Spring框架中，`@Configuration`注解用于定义配置类，这些类包含了通过`@Bean`注解声明的方法，这些方法会返回对象，这些对象会被注册为Spring应用上下文中的Bean。从Spring 5.2开始，`@Configuration`注解引入了一个新的属性`proxyBeanMethods`，默认值为`true`，但你可以将其设置为`false`以优化性能。

### `proxyBeanMethods = true`（默认值）

当`proxyBeanMethods`设置为`true`时，Spring容器会为每个`@Configuration`类创建一个CGLIB代理。这个代理确保了`@Bean`方法之间的调用会被拦截，从而允许Spring容器利用Bean的生命周期回调（如`@PostConstruct`）和AOP（面向切面编程）功能。具体来说，即使`@Bean`方法之间的调用是在配置类内部进行的，Spring也会确保这些调用通过容器来解析，从而返回容器中的Bean实例，而不是简单地调用另一个方法。

### `proxyBeanMethods = false`

当`proxyBeanMethods`设置为`false`时，Spring容器将不会为`@Configuration`类创建CGLIB代理。这意味着`@Bean`方法之间的直接调用将不会通过Spring容器进行解析，而是像普通Java方法调用一样直接执行。这可以带来性能上的提升，特别是当配置类中包含了大量的`@Bean`方法，并且这些方法之间频繁地相互调用时。然而，需要注意的是，如果`@Bean`方法之间的调用依赖于Spring容器的特定行为（如AOP增强、Bean的生命周期回调等），那么将`proxyBeanMethods`设置为`false`可能会导致意外的行为。

### 使用建议

- 如果你的配置类中的`@Bean`方法之间没有相互调用，或者即使它们相互调用，你也不依赖于Spring容器的特定行为（如AOP增强、Bean的生命周期回调等），那么将`proxyBeanMethods`设置为`false`可能是个好主意，因为它可以提高性能。
- 如果你的配置类中的`@Bean`方法之间确实存在相互调用，并且这些调用依赖于Spring容器的特定行为，那么你应该保持`proxyBeanMethods`的默认值`true`，以确保这些行为能够按预期工作。

### 示例

```
@Configuration(proxyBeanMethods = false)
public class MyConfig {

    @Bean
    public MyBean1 myBean1() {
        return new MyBean1();
    }

    @Bean
    public MyBean2 myBean2() {
        // 注意：这里直接调用了myBean1()方法，而不是注入myBean1的Bean实例
        // 如果proxyBeanMethods=false，这将直接执行myBean1()方法体，而不是通过Spring容器获取Bean
        MyBean1 myBean1 = myBean1();
        return new MyBean2(myBean1);
    }
}
```

在这个例子中，由于`proxyBeanMethods`被设置为`false`，`myBean2()`方法中的`myBean1()`调用将直接执行`myBean1()`方法体，而不是通过Spring容器获取`MyBean1`的Bean实例。这可能会导致`MyBean2`接收到的是一个尚未完全初始化或配置的`MyBean1`实例。

`@PostConstruct` 注解是 Java EE 5 引入的，用于标记在依赖关系注入完成之后需要执行的方法，以执行初始化代码。它通常用在类的方法上，该方法在对象创建并且依赖注入完成后自动被容器（如Spring或EJB容器）调用。这个注解不属于Spring框架本身，而是Java EE的一部分，但Spring框架完全支持它。

在Spring中，当一个Bean被创建并且其所有必需的属性都通过依赖注入（DI）被设置之后，`@PostConstruct`注解的方法将被自动调用。这允许你执行一些初始化操作，比如资源的初始化、配置的验证等，这些操作在Bean完全准备好被使用之前必须完成。

### 使用示例

```
import javax.annotation.PostConstruct;

@Component
public class MyBean {

    @Autowired
    private AnotherBean anotherBean;

    public MyBean() {
        // 构造函数调用，此时依赖可能还未注入
    }

    @PostConstruct
    public void init() {
        // 依赖注入完成后执行的方法
        // 你可以在这里安全地使用anotherBean
        System.out.println("MyBean has been initialized with AnotherBean = " + anotherBean);
    }

    // 其他方法...
}
```

在上面的例子中，`MyBean`类有一个被`@Autowired`注解的`AnotherBean`类型的字段。当Spring容器创建`MyBean`的实例时，它首先会调用`MyBean`的构造函数。然而，在构造函数调用时，依赖注入可能还没有完成，因此你不能在构造函数中安全地使用`anotherBean`。但是，一旦`MyBean`的实例被创建并且所有依赖都被注入，Spring容器就会调用用`@PostConstruct`注解标记的`init`方法。在这个方法中，你可以安全地使用`anotherBean`，因为此时它已经被注入了。

### 注意事项

- `@PostConstruct`注解的方法不能有返回类型（即必须是`void`）。
- `@PostConstruct`注解的方法不能抛出已检查的异常（即它必须声明抛出`RuntimeException`或`Error`）。
- 在Spring框架中，虽然`@PostConstruct`是Java EE的一部分，但Spring提供了自己的初始化回调接口（如`InitializingBean`接口和`@Bean`注解的`initMethod`属性），它们也可以用于执行初始化代码。然而，使用`@PostConstruct`注解通常更为简洁和方便。
- 如果你正在使用Spring Boot和自动配置，那么`@PostConstruct`是一个很好的选择来执行需要在Bean完全初始化后运行的代码。然而，请注意，在Spring Boot的某些生命周期事件中（如`CommandLineRunner`或`ApplicationRunner`），可能需要在`@PostConstruct`之后执行代码。在这些情况下，你应该考虑使用Spring Boot的生命周期回调而不是`@PostConstruct`。

## Spring 环境属性

Spring 环境属性是Spring框架中用于管理和访问应用程序运行时环境信息的核心组件。这些属性可以来源于多种不同的源，包括但不限于JVM系统参数、命令行参数、环境变量、属性文件等。Spring环境属性通过`Environment`接口进行抽象和表示，该接口为应用程序提供了一个访问这些属性的统一方式。以下是对Spring环境属性的详细分析：

### 1. Environment接口

- **定义**：`org.springframework.core.env.Environment`接口是Spring框架中表示应用程序运行环境的抽象。它提供了访问属性（如系统属性、环境变量、配置属性等）和配置文件的方法。
- **功能**：

- 访问系统属性（System Properties）。
- 访问环境变量（Environment Variables）。
- 访问配置属性（Configuration Properties），这些属性可以来自多个源，如属性文件、命令行参数等。
- 访问活动的profiles，这些profiles用于在不同的环境下（如开发、测试、生产）切换配置。

### 2. 属性源（Property Sources）

- **定义**：属性源是Spring环境属性的来源，它们以键值对的形式存储属性。
- **常见属性源**：

- **系统属性（System Properties）**：Java虚拟机启动时通过`-D`选项指定的属性。
- **环境变量（Environment Variables）**：操作系统级别的属性。
- **应用配置文件（Application Configuration Files）**：如`application.properties`或`application.yml`文件，以及特定于profiles的配置文件（如`application-dev.properties`）。
- **命令行参数（Command Line Arguments）**：在启动应用程序时指定的参数。

### 3. Profiles

- **定义**：Profiles是Spring中用于区分不同环境（如开发、测试、生产）的配置集合。每个profile可以定义一组特定的bean和配置属性。
- **使用**：

- 通过`@Profile`注解可以指定某个类或bean属于哪个profile。
- 通过`spring.profiles.active`属性可以设置激活的profile，该属性可以在多个地方设置，如命令行参数、`application.properties`文件或Spring Boot的启动参数中。

### 4. 属性解析

- **PropertyResolver**：是Spring中用于解析属性的接口，它提供了一系列方法来检查属性是否存在、获取属性值等。
- **ConfigurablePropertyResolver**：是`PropertyResolver`的子接口，提供了更多配置选项，如设置属性转换服务（`ConversionService`）、占位符前缀和后缀等。

### 5. 属性优先级

Spring在解析属性时，会根据属性源的优先级进行。一般来说，优先级从高到低如下：

1. 命令行参数
2. 来自`spring-boot:run`的启动参数
3. `--spring-application.arguments`属性
4. 程序内通过`SpringApplication.setAdditionalProfiles(...)`或`setAdditionalArguments(...)`指定的参数
5. 通过`SPRING_APPLICATION_JSON`（内嵌在环境变量或系统属性中的JSON）指定的配置
6. ServletConfig初始化参数
7. ServletContext初始化参数
8. JNDI属性（从`java:comp/env`获取）
9. Java系统属性（`System.getProperties()`）
10. 操作系统环境变量
11. 随机生成的`random.*`属性
12. 应用程序配置文件（`application.properties`或`application.yml`文件，位于`spring.config.location`指定的位置）
13. 打包在jar中的默认属性文件（`application.properties`或`application.yml`）
14. 在`@Configuration`类中通过`@PropertySource`注解指定的属性文件
15. 默认属性（通过`SpringApplication.setDefaultProperties`指定的属性）

### 6. 访问环境属性

在Spring应用程序中，可以通过多种方式访问环境属性：

- **Environment对象**：通过`ApplicationContext`的`getEnvironment()`方法获取`Environment`对象，然后调用其方法访问属性。
- **@Value注解**：直接在字段、方法参数或构造函数参数上使用`@Value("${propertyName}")`注解来注入属性值。
- **@ConfigurationProperties**：将一组属性绑定到Java Bean上，提供更强的类型安全和配置能力。

### 7. 结论

Spring环境属性是Spring框架中非常重要的一部分，它们为应用程序提供了灵活的配置方式。通过合理使用属性源、profiles和属性解析器，可以轻松地管理应用程序在不同环境下的配置，从而提高应用程序的可移植性和可维护性。

### application.properties 和 application.yml 哪个优先级高?

当 `application.properties` 和 `application.yml` 同时存在，同样的参数，**最终生效的是** `**application.properties**` **中的配置。**

所以如果项目里因为一些“逆天”原因，导致同时存在这两个配置，那么就要小心覆盖问题了！当然，移除其中一个配置文件才是最佳处理方案！

再扩展下 Spring Boot 在启动时加载配置属性的顺序，可参考如下的官方文档：

![](学习笔记/Attachments/1722302227940-d5fce520-06b9-4018-91db-698373637e1c.webp)

**从上到下，优先级逐渐降低**，即下面的配置，同样的参数会被上面的配置所覆盖。

简单优先级：

命令行参数 > JAR包外面的 `application-{profile}.properties` > JAR包内的 `application-{profile}.properties` > JAR包外的 `application.properties` > JAR包内的 `application.properties`

### **扩展 bootstrap 和 application 配置文件知识：**

boostrap 由父 ApplicationContext 加载，比 applicaton 优先加载，且它里面的内容不会被覆盖！

比如一些配置中心的配置，需要填写在 boostrap 中，父上下文先去配置中心获取额外的一些配置，然后再启动 SpringBoot 上下文。

## xxs攻击

### 什么是xxs

Cross-Site Scripting（跨站脚本攻击）简称 XSS，是一种代码注入攻击。攻击者通过在目标网站上注入恶意脚本，使之在用户的浏览器上运行。利用这些恶意脚本，攻击者可获取用户的敏感信息如 Cookie、SessionID 等，进而危害数据安全。为了和 CSS 区分，这里把攻击的第一个字母改成了 X，于是叫做 XSS。

XSS 的本质是：恶意代码未经过滤，与网站正常的代码混在一起；浏览器无法分辨哪些脚本是可信的，导致恶意脚本被执行。

而由于直接在用户的终端执行，**恶意代码能够直接获取用户的信息**，或者利用这些信息冒充用户向网站发起攻击者定义的请求。

在部分情况下，由于输入的限制，注入的恶意脚本比较短。但可以通过引入外部的脚本，并由浏览器执行，来完成比较复杂的攻击策略。

简单理解就是说，XSS是指攻击者往Web页面里插入恶意Script代码，当用户浏览该页之时，嵌入其中Web里面的Script代码会被解析执行，从而达到恶意攻击用户的目的。XSS攻击人群是针对用户层面的攻击！

### 防御之法

  

## logging

### SLF4J

Java的简单日志外观（SLF4J）作为各种日志框架的简单外观或抽象，例如java. util.log、log4j 1.x、reload4j和logback。SLF4J允许最终用户在部署时插入所需的日志框架。请注意，SLF4J启用您的库/应用程序意味着只添加一个强制依赖项，即slf4j-api-2.0.13.jar。

我的理解是它提供一套抽象得接口，它有一些实现者，Slf4j需要配合实现来食用，在我们得项目中引入SLF4J和它得实现者后我们调用slf4j提供得api，最终日志的格式、记录级别、输出方式等通过绑定具体的日志系统来实现。这样在项目需要变更其他日志实现得时候只需要更换实现得依赖即可，降低了日志实现同我们系统得耦合。

### pom依赖

```
  <!-- SLF4J API -->
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>2.0.13</version>
</dependency>

<!-- Logback 实现 -->
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>1.5.6</version>
</dependency>
```

#### logback配置文件

spring配置文件加载得顺序：

```
logback.xml	—>	application.properties	—>	logback-spring.xml.
```

如果使用的lobback配置文件的名称是logback.xml 会先出现先找不到设置的属性，然后项目启动起来才会找到，

#### springProfile

不同开发环境使用不同得配置

```
<springProfile name="staging">

</springProfile>

<springProfile name="dev | staging">

</springProfile>

<springProfile name="!production">

</springProfile>
```

#### Environment Properties

在xml中使用yml中的配置信息

```
<springProperty scope="context" name="fluentHost" source="myapp.fluentd.host"
		defaultValue="localhost"/>
<appender name="FLUENT" class="ch.qos.logback.more.appenders.DataFluentAppender">
	<remoteHost>${fluentHost}</remoteHost>
	...
</appender>
```

#### 简单配置

`**<property name="FILE_LOG_THRESHOLD" value="${FILE_LOG_THRESHOLD:-TRACE}"/>**` ${}是从spring环境中获取变量，FILE_LOG_THRESHOLD，如果没能获取到就使用默认值：TRACE

```
<configuration>

    <conversionRule conversionWord="applicationName" converterClass="org.springframework.boot.logging.logback.ApplicationNameConverter" />
    <conversionRule conversionWord="clr" converterClass="org.springframework.boot.logging.logback.ColorConverter" />
    <conversionRule conversionWord="correlationId" converterClass="org.springframework.boot.logging.logback.CorrelationIdConverter" />
    <conversionRule conversionWord="wex" converterClass="org.springframework.boot.logging.logback.WhitespaceThrowableProxyConverter" />
    <conversionRule conversionWord="wEx" converterClass="org.springframework.boot.logging.logback.ExtendedWhitespaceThrowableProxyConverter" />

    <property name="LOG_FILE" value="${LOG_FILE:-${LOG_PATH:-${LOG_TEMP:-${java.io.tmpdir:-/tmp}}}/emos.log}"/>
    <property name="CONSOLE_LOG_PATTERN" value="${CONSOLE_LOG_PATTERN:-%clr(%d{${LOG_DATEFORMAT_PATTERN:-yyyy-MM-dd'T'HH:mm:ss.SSSXXX}}){faint} %clr(${LOG_LEVEL_PATTERN:-%5p}) %clr(${PID:- }){magenta} %clr(---){faint} %clr(%applicationName[%15.15t]){faint} %clr(${LOG_CORRELATION_PATTERN:-}){faint}%clr(%-40.40logger{39}){cyan} %clr(:){faint} %m%n${LOG_EXCEPTION_CONVERSION_WORD:-%wEx}}"/>
    <property name="CONSOLE_LOG_CHARSET" value="${CONSOLE_LOG_CHARSET:-${file.encoding:-UTF-8}}"/>
    <property name="CONSOLE_LOG_THRESHOLD" value="${CONSOLE_LOG_THRESHOLD:-TRACE}"/>
    <property name="FILE_LOG_PATTERN" value="${FILE_LOG_PATTERN:-%d{${LOG_DATEFORMAT_PATTERN:-yyyy-MM-dd'T'HH:mm:ss.SSSXXX}} ${LOG_LEVEL_PATTERN:-%5p} ${PID:- } --- %applicationName[%t] ${LOG_CORRELATION_PATTERN:-}%-40.40logger{39} : %m%n${LOG_EXCEPTION_CONVERSION_WORD:-%wEx}}"/>
    <property name="FILE_LOG_CHARSET" value="${FILE_LOG_CHARSET:-${file.encoding:-UTF-8}}"/>
    <property name="FILE_LOG_THRESHOLD" value="${FILE_LOG_THRESHOLD:-TRACE}"/>

    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <charset>${CONSOLE_LOG_CHARSET}</charset>
            <pattern>${CONSOLE_LOG_PATTERN}</pattern>
        </encoder>
    </appender>


    <root level="info">
        <appender-ref ref="STDOUT" />

        <appender-ref ref="FILE" />
    </root>
<!--    <logger name="com.wrw.emos" level="debug" >-->
<!--        <appender-ref ref="STDOUT" />-->
<!--    </logger>-->
<!--    写入到文件-->
    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
            <level>${FILE_LOG_THRESHOLD}</level>
        </filter>
        <encoder>
            <pattern>${FILE_LOG_PATTERN}</pattern>
            <charset>${FILE_LOG_CHARSET}</charset>
        </encoder>
<!--        日志名称-->
<!--        <fileNamePattern>：定义了滚动后文件的命名模式。${LOGBACK_ROLLINGPOLICY_FILE_NAME_PATTERN:-${LOG_FILE}.%d{yyyy-MM-dd}.%i.gz}中的%d{yyyy-MM-dd}表示日期，%i是一个递增的索引（当文件因大小限制而滚动时），.gz表示压缩文件。如果LOGBACK_ROLLINGPOLICY_FILE_NAME_PATTERN变量未设置，则使用默认值${LOG_FILE}.%d{yyyy-MM-dd}.%i.gz。-->
<!--        <cleanHistoryOnStart>：是否在启动时清理历史记录，默认为false。-->
<!--        <maxFileSize>：定义了触发滚动的单个文件最大大小，默认为10MB。-->
<!--        <totalSizeCap>：所有日志文件总大小的上限，默认为0表示没有限制。-->
<!--        <maxHistory>：保留日志文件的最大天数，默认为7天。-->
        <file>${LOG_FILE}</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">
            <fileNamePattern>${LOGBACK_ROLLINGPOLICY_FILE_NAME_PATTERN:-${LOG_FILE}.%d{yyyy-MM-dd}.%i.gz}</fileNamePattern>
            <cleanHistoryOnStart>${LOGBACK_ROLLINGPOLICY_CLEAN_HISTORY_ON_START:-false}</cleanHistoryOnStart>
            <maxFileSize>${LOGBACK_ROLLINGPOLICY_MAX_FILE_SIZE:-10MB}</maxFileSize>

            <totalSizeCap>${LOGBACK_ROLLINGPOLICY_TOTAL_SIZE_CAP:-0}</totalSizeCap>
<!--            保存期限-->
            <maxHistory>${LOGBACK_ROLLINGPOLICY_MAX_HISTORY:-7}</maxHistory>
        </rollingPolicy>
    </appender>
</configuration>
```

## swagger

```
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger2</artifactId>
            <version>2.9.2</version>
        </dependency>
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger-ui</artifactId>
            <version>2.8.0</version>
        </dependency>
```

```
import io.swagger.annotations.Api;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;
 
/**
 * Swagger使用的配置文件
 */
@Configuration
@EnableSwagger2
public class Swagger2Configuration {
    @Bean
    public Docket createRestApi(){
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.withClassAnnotation(Api.class))
                .paths(PathSelectors.any())
                .build();
    }
 
    //基本信息的配置，信息会在api文档上显示
    private ApiInfo apiInfo(){
        return new ApiInfoBuilder()
                .title("zg测试的接口文档")
                .description("xx相关接口的文档")
                .termsOfServiceUrl("http://localhost:8080/hello")
                .version("1.0")
                .build();
    }
}
```

![](学习笔记/Attachments/1721955440083-49da0a63-a97c-4870-af5a-7e74b08a7ccc.png)

## cos跨域

```
@Configuration
public class CorsConfig {
    /**
     * 跨域配置
     * @return
     */
    @Bean
    public CorsWebFilter corsWebFilter(){
        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        CorsConfiguration corsConfiguration = new CorsConfiguration();

        // 允许哪些头部请求跨域：全部
        corsConfiguration.addAllowedHeader("*");
        // 允许哪些请求方式跨域：所有
        corsConfiguration.addAllowedMethod("*");
        // 允许哪些请求来源跨域：任意
        corsConfiguration.addAllowedOrigin("*");
        // 是否允许携带cookie跨域：是
        corsConfiguration.setAllowCredentials(true);

        source.registerCorsConfiguration("/**",corsConfiguration);
        return new CorsWebFilter(source);
    }
```

## openFeign

```
@EnableFeignClients

//调用的注册中心的名称
@FeignClient("xxxx")
public interface XxxFeign {

}
```

## gateway

[官方教程](https://cloud.spring.io/spring-cloud-gateway/reference/html/)

配置工程，类似与rabbitmq的topic

```
spring:
  cloud:
    gateway:
      routes:
      // id名称
      - id: query_route
    // 跳转的url
        uri: https://example.org
  // 规则
        predicates:
        // 携带名称为green的请求参数
        // 匹配到localhost:8080？green=* 定向到 上面的url
        - Query=green
```

### 跨域

```
@Configuration
public class CorsFilterConfig {

    @Bean
    public CorsWebFilter corsFilter() {
        CorsConfiguration config = new CorsConfiguration();
        config.addAllowedMethod("*");
        // springboot升级成2.4.0以上时对AllowedOrigin设置发生了改变，不能有”*“,可以替换成AllowedOriginPattern
        config.addAllowedOrigin("http://localhost:8081");

//        config.addAllowedOriginPattern("http://localhost:8081");
        config.addAllowedHeader("*");
        config.setAllowCredentials(true);
        // 必须是reactive包下的UrlBasedCorsConfigurationSource
        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource(new PathPatternParser());
        source.registerCorsConfiguration("/**", config);
        return new CorsWebFilter(source);
    }
}
```

## Long序列化String

在项目开发过程中，当后端返回给前端的类型为 `Long` 类型时，如果值超过了前端 `js` 显示的长度范围的话会导致数字精度丢失，但我们又不想变更字段的类型，此时我们可以在序列化返回时将 `Long` 类型转换成 `String` 类型。

##### `@JsonSerialize`

```
import com.fasterxml.jackson.databind.annotation.JsonSerialize;
import com.fasterxml.jackson.databind.ser.std.ToStringSerializer;

public class Demo {

	@JsonSerialize(using = ToStringSerializer.class)
    private Long uid;
    
}
```

webmvcConfig

```
import com.fasterxml.jackson.core.JsonGenerator;
import com.fasterxml.jackson.databind.JsonSerializer;
import com.fasterxml.jackson.databind.SerializerProvider;
import com.fasterxml.jackson.databind.module.SimpleModule;
import org.springframework.context.annotation.Configuration;
import org.springframework.http.converter.HttpMessageConverter;
import org.springframework.http.converter.json.MappingJackson2HttpMessageConverter;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

import java.io.IOException;
import java.util.List;

/**
 * @author 王廷云
 */
@Configuration
public class ShopWebConfig implements WebMvcConfigurer {

    /**
     * 注册自定义类型转换器
     */
    @Override
    public void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
        // 获取Json转换器
        MappingJackson2HttpMessageConverter converter = (MappingJackson2HttpMessageConverter) converters.get(0);
        // 转换Long和long类型
        SimpleModule module = new SimpleModule();
        module.addSerializer(Long.class, new LongToStringSerializer());
        module.addSerializer(long.class, new LongToStringSerializer());
        // 将自定义序列化器注册进Json转换器中
        converter.getObjectMapper().registerModule(module);
    }

    /**
     * 自定义LongToString序列化器
     */
    public static class LongToStringSerializer extends JsonSerializer<Long> {
        @Override
        public void serialize(Long value, JsonGenerator gen, SerializerProvider serializers) throws IOException {
            if (null != value) {
                gen.writeString(value.toString());
            } else {
                gen.writeNull();
            }
        }
    }
}
```

## ThreadPoolExcutor

```
   private final AtomicInteger ctl = new AtomicInteger(ctlOf(RUNNING, 0));
    // 32-3=29
    private static final int COUNT_BITS = Integer.SIZE - 3;
  
    // 0111_1111_1111_1111_1111_1111_1111
    private static final int COUNT_MASK = (1 << COUNT_BITS) - 1;

    // runState is stored in the high-order bits
    // 1010_0000_0000_0000_0000_0000_0000_0000
    private static final int RUNNING    = -1 << COUNT_BITS;
    private static final int SHUTDOWN   =  0 << COUNT_BITS;
    private static final int STOP       =  1 << COUNT_BITS;
    private static final int TIDYING    =  2 << COUNT_BITS;
    private static final int TERMINATED =  3 << COUNT_BITS;

    // Packing and unpacking ctl
    private static int runStateOf(int c)     { return c & ~COUNT_MASK; }
    private static int workerCountOf(int c)  { return c & COUNT_MASK; }
    private static int ctlOf(int rs, int wc) { return rs | wc; }

    /*
     * Bit field accessors that don't require unpacking ctl.
     * These depend on the bit layout and on workerCount being never negative.
     */

    private static boolean runStateLessThan(int c, int s) {
        return c < s;
    }

    private static boolean runStateAtLeast(int c, int s) {
        return c >= s;
    }

    private static boolean isRunning(int c) {
        return c < SHUTDOWN;
    }
```

```
   public void execute(Runnable command) {
        if (command == null)
            throw new NullPointerException();
   
        int c = ctl.get();
        // 当前工作线程数小于核心线程数
        if (workerCountOf(c) < corePoolSize) {
            if (addWorker(command, true))
                return;
            c = ctl.get();
        }
        if (isRunning(c) && workQueue.offer(command)) {
            int recheck = ctl.get();
            if (! isRunning(recheck) && remove(command))
                reject(command);
            else if (workerCountOf(recheck) == 0)
                addWorker(null, false);
        }
        else if (!addWorker(command, false))
            reject(command);
    }
```

```
// core是不是核心线程
private boolean addWorker(Runnable firstTask, boolean core) {
        retry:
        for (int c = ctl.get();;) {
            // Check if queue empty only if necessary.
            if (c>=SHUTDOWN
                && c>=STOP
                    || firstTask != null
                    || workQueue.isEmpty()))
                return false;

            for (;;) {
                // 当前工作线程大于核心线程数吗
                if (workerCountOf(c)
                    >= ((core ? corePoolSize : maximumPoolSize) & COUNT_MASK))
                    return false;
                // 线程数加1
                if (compareAndIncrementWorkerCount(c))
                    // 加1成功，就退出循环
                    break retry;
                // 重试
                c = ctl.get();  // Re-read ctl
                if (c>= SHUTDOWN)
                    continue retry;
                // else CAS failed due to workerCount change; retry inner loop
            }
        }

        boolean workerStarted = false;
        boolean workerAdded = false;
        Worker w = null;
        try {
            w = new Worker(firstTask);
            final Thread t = w.thread;
            if (t != null) {
                final ReentrantLock mainLock = this.mainLock;
                mainLock.lock();
                try {
                    // Recheck while holding lock.
                    // Back out on ThreadFactory failure or if
                    // shut down before lock acquired.
                    int c = ctl.get();

                    if (isRunning(c) ||
                        (runStateLessThan(c, STOP) && firstTask == null)) {
                        if (t.getState() != Thread.State.NEW)
                            throw new IllegalThreadStateException();
                        workers.add(w);
                        workerAdded = true;
                        int s = workers.size();
                        if (s > largestPoolSize)
                            largestPoolSize = s;
                    }
                } finally {
                    mainLock.unlock();
                }
                if (workerAdded) {
                    t.start();
                    workerStarted = true;
                }
            }
        } finally {
            if (! workerStarted)
                addWorkerFailed(w);
        }
        return workerStarted;
    }
```

## maven

### Rescourse标签

```xml
 <resources>
            <resource>
                <directory>src/main/resources</directory>
                <!-- 关闭过滤 -->
                <filtering>false</filtering>
            </resource>
            <resource>
                <directory>src/main/resources</directory>
                <!-- 引入所有 匹配文件进行过滤 -->
                <includes>
                    <include>application*</include>
                    <include>bootstrap*</include>
                    <include>banner*</include>
                </includes>
                <!-- 启用过滤 即该资源中的变量将会被过滤器中的值替换 -->
                <filtering>true</filtering>
            </resource>
</resources>
```

我的理解是maven是打包构建工具,只有在这里使用rescourse标签书写的资源才会被打包进jar包中,即使SpringBoot的配置加载有自己的加载方法,但是如果最终打包成jar包可能会出现配置文件丢失的情况,所以还是需要在maven中添加resource标签.

上面的内容首先将resource目录添加进来,然后对所有的资源关闭过滤,**过滤**的意思是可是使用maven中配置的变量去替换资源文件中的内容.

接着对需要开启过滤的资源配置标签进行过滤

### 项目单独配置镜像

```xml
  <repositories>
        <repository>
            <id>public</id>
            <name>huawei nexus</name>
            <url>https://mirrors.huaweicloud.com/repository/maven/</url>
            <releases>
                <enabled>true</enabled>
            </releases>
        </repository>
    </repositories>

    <pluginRepositories>
        <pluginRepository>
            <id>public</id>
            <name>huawei nexus</name>
            <url>https://mirrors.huaweicloud.com/repository/maven/</url>
            <releases>
                <enabled>true</enabled>
            </releases>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
        </pluginRepository>
    </pluginRepositories>
```

### 环境配置

```xml
  <profiles>
        <profile>
            <id>local</id>
            <properties>
                <!-- 环境标识，需要与配置文件的名称相对应 -->
                <profiles.active>local</profiles.active>
                <logging.level>info</logging.level>
            </properties>
        </profile>
        <profile>
            <id>dev</id>
            <properties>
                <!-- 环境标识，需要与配置文件的名称相对应 -->
                <profiles.active>dev</profiles.active>
                <logging.level>info</logging.level>
            </properties>
            <activation>
                <!-- 默认环境 -->
                <activeByDefault>true</activeByDefault>
            </activation>
        </profile>
        <profile>
            <id>prod</id>
            <properties>
                <profiles.active>prod</profiles.active>
                <logging.level>warn</logging.level>
            </properties>
        </profile>
    </profiles>
```

定义了三个环境配置文件：local、dev和prod。这些配置文件可以用于为不同的环境（如开发、测试和生产）设置不同的属性。

`<activation>`标签来设置默认激活的环境配置。`<activeByDefault>`标签设置为"true"，表示这个配置文件默认是激活的。

maven在这里负责的就是将给application.yaml配置中的profile赋值,这样spring就会加载相应的配置文件

##  配置加载

```yml
#application.yml

myApp:
	name: 'name'
```

```java
//demo.java
@Component
public class App{
    @Value("${myApp.name}")
    String name;
}
```

> 参数引用

```yml
#application.yml
myApp:
  name: "小程序"
  age: 18
  output: ${myApp.name} xxx ${myApp.name}
```

```java
// demo.java
@Component
@Data
public class App {
    @Value("${myApp.name}")
    public String name;
    @Value("${myApp.age}")
    public Integer age;
    @Value("${myApp.output}")
    public String output;
}
```

> 通过注解批量导入参数

```java
// demo.java
@Component
@Data
// 在配置中名称为myApp,注解这里需要命名规范,使用中划线命名
@ConfigurationProperties(prefix = "my-app")
public class App {
//    @Value("${myApp.name}")
    public String name;
//    @Value("${myApp.age}")
    public Integer age;
//    @Value("${myApp.output}")
    public String output;
}
```

### 命令行加载参数

```shell
java -jar xxx.jar --server.port=8888
```

上面的等同与下面这种情况,但是优先级比下面的要高

```yml
#application.yml
spring:
	port: 8888
```

### 多环境配置

在Spring Boot中多环境配置文件名需要满足`application-{profile}.properties`的格式，其中`{profile}`对应你的环境标识，比如：

- `application-dev.properties`：开发环境
- `application-test.properties`：测试环境
- `application-prod.properties`：生产环境

至于哪个具体的配置文件会被加载，需要在`application.properties`文件中通过`spring.profiles.active`属性来设置，其值对应配置文件中的`{profile}`值。如：`spring.profiles.active=test`就会加载`application-test.properties`配置文件内容。

> 通过命令行直接加载指定配置

```shell
java -jar xxx.jar --spring.profiles.active=dev
```

其中application.yml与application-dev.yml同时存在时,会先加载application.yml中的内容

```yml
#application.yml
spring:
  profiles:
    active: @profiles.active@

myApp:
  name: "小程序"
  age: 26
  output: ${myApp.age} xxx ${myApp.name}
```

```yml
#application-dev.yml
spring:
  profiles:
    active: @profiles.active@
```

```java
//demo.java
@Component
@Data
@ConfigurationProperties(prefix = "my-app")
public class App {
//    @Value("${myApp.name}")
    public String name;
//    @Value("${myApp.age}")
    public Integer age;
//    @Value("${myApp.output}")
    public String output;
}
```

> 通过自动装配打印内容

```
App(name=小程序, age=26, output=26 xxx 小程序)
```

### 配置加载顺序

Spring Boot为了能够更合理的重写各属性的值，使用了下面这种较为特别的属性加载顺序：

1. 命令行中传入的参数。
2. `SPRING_APPLICATION_JSON`中的属性。`SPRING_APPLICATION_JSON`是以JSON格式配置在系统环境变量中的内容。
3. `java:comp/env`中的`JNDI`属性。
4. Java的系统属性，可以通过`System.getProperties()`获得的内容。
5. 操作系统的环境变量
6. 通过`random.*`配置的随机属性
7. 位于当前应用jar包之外，针对不同`{profile}`环境的配置文件内容，例如：`application-{profile}.properties`或是`YAML`定义的配置文件
8. 位于当前应用jar包之内，针对不同`{profile}`环境的配置文件内容，例如：`application-{profile}.properties`或是`YAML`定义的配置文件
9. 位于当前应用jar包之外的`application.properties`和`YAML`配置内容
10. 位于当前应用jar包之内的`application.properties`和`YAML`配置内容
11. 在`@Configuration`注解修改的类中，通过`@PropertySource`注解定义的属性
12. 应用默认属性，使用`SpringApplication.setDefaultProperties`定义的内容

**优先级按上面的顺序有高到低，数字越小优先级越高。**

可以看到，其中第7项和第9项都是从应用jar包之外读取配置文件，所以，实现外部化配置的原理就是从此切入，为其指定外部配置文件的加载位置来取代jar包之内的配置内容。通过这样的实现，我们的工程在配置中就变的非常干净，我们只需要在本地放置开发需要的配置即可，而其他环境的配置就可以不用关心，由其对应环境的负责人去维护即可。

### 配置元数据

ide在我们将鼠标移动到配置key上会有属性描述提示,而我们的没有,这是配置内容有配置元数据,一般在包的META-INF包中的json

```json
  {
      "name": "debug",
      "type": "java.lang.Boolean",
      "description": "Enable debug logs.",
      "sourceType": "org.springframework.boot.context.logging.LoggingApplicationListener",
      "defaultValue": false
    },
    {
      "name": "logging.charset.console",
      "type": "java.nio.charset.Charset",
      "description": "Charset to use for console output."
    },
```

就是这些内容完成了,配置描述提示

#### 生成配置元数据

1. 完成注释描述

   ```java
   @Component
   @Data
   @ConfigurationProperties(prefix = "my-app")
   public class App {
   //    @Value("${myApp.name}")
       /**
        * 这是name
        */
       public String name;
   //    @Value("${myApp.age}")
       /**
        * 这是age
        */
       public Integer age;
   //    @Value("${myApp.output}")
       /**
        * 这是output
        */
       public String output;
   }
   
   ```

2. 添加依赖

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-configuration-processor</artifactId>
   </dependency>
   
   ```

3. ```bash
   mvn install
   ```

   生成的元数据

   ```json
   {
     "groups": [
       {
         "name": "my-app",
         "type": "com.example.demo.entity.App",
         "sourceType": "com.example.demo.entity.App"
       }
     ],
     "properties": [
       {
         "name": "my-app.age",
         "type": "java.lang.Integer",
         "description": "这是age",
         "sourceType": "com.example.demo.entity.App"
       },
       {
         "name": "my-app.name",
         "type": "java.lang.String",
         "description": "这是name",
         "sourceType": "com.example.demo.entity.App"
       },
       {
         "name": "my-app.output",
         "type": "java.lang.String",
         "description": "这是output",
         "sourceType": "com.example.demo.entity.App"
       }
     ],
     "hints": []
   }
   
   ```

4. 会发现ide已经会提示我们的自定义字段了

## 整合swagger(2.x)

1. 导入maven依赖

   ```xml
   <!-- pom.xml -->
   <dependency>
       <groupId>com.spring4all</groupId>
       <artifactId>swagger-spring-boot-starter</artifactId>
       <version>1.9.0.RELEASE</version>
   </dependency>
   
   ```

   

2. 添加注解在启动类上

   ```java	
   @SpringBootApplication
   @EnableSwagger2Doc
   public class DemoApplication {
       public static void main(String[] args) {
           SpringApplication.run(DemoApplication.class, args);
       }
   
   }
   ```

3. 配置文件

   ```yml
   # application.yml
   swagger:
     enabled: true
     title: "SpringBoot swagger"
     description: "描述"
     contact:
       url: "联系人url"
       name: "name"
       email: "2435xxxx@qq.com"
     base-path: "/**"
     base-package: "com.example"
   ```

   

4. 启动应用

   [地址](localhost:8080/swagger-ui.html)

## 整合SpringDoc

### 导入依赖

```xml
<!-- pom.xml -->
<dependency>
    <groupId>org.springdoc</groupId>
    <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
    <version>2.0.2</version>
</dependency>
<!-- 官方建议是springdoc替代springfox-->
<dependency>
    <groupId>org.springdoc</groupId>
    <artifactId>springdoc-openapi-starter-webmvc-api</artifactId>
    <version>2.0.2</version>
</dependency>
```

### 配置配置Bean

```java
// SwaggerConfig
@Configuration
public class SwaggerConfig {
  @Bean
  public OpenAPI springShopOpenAPI() {
      return new OpenAPI()
              .info(new Info().title("SpringShop API")
              .description("Spring shop sample application")
              .version("v0.0.1")
              .license(new License().name("Apache 2.0").url("http://springdoc.org")))
              .externalDocs(new ExternalDocumentation()
              .description("SpringShop Wiki Documentation")
              .url("https://springshop.wiki.github.org/docs"));
  }



}

```

### 注解装饰方法

```java
// HelloController.java

@RestController
@Tag(name = "hello")
public class HelloController {
    @Autowired
    App app;

    @GetMapping("/hello")
    @Operation(summary = "获取用户列表")
    public String hello() {
        return app.toString();
    }

}

```

## JSR-303参数校验

### 依赖引入

> 是否需要引入依赖查看项目中是否包含`hibernate-validator`,没有的话引入下面的依赖

[相关链接](https://www.didispace.com/spring-boot-2/3-3-jsr303.html#jsr-303)

```xml
<!-- pom.xml -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>

```











