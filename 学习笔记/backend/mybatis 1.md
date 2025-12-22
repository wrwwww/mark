# 1. java反射

## 1.1. Class.newInstance()和Constructor.newInstance()

都是Java反射API的一部分，但它们在用法和功能上存在一些区别。

1. Class.newInstance()

Class.newInstance()方法只能调用无参的构造函数，即默认的构造函数。这意味着，如果你要实例化一个类，并且该类只有一个无参的构造函数，那么可以使用Class.newInstance()方法。然而，如果类中定义了带有参数的构造函数，那么Class.newInstance()将不能正确地创建类的实例。

Class.newInstance()方法在调用构造函数时，会抛出所有由被调用构造函数抛出的异常。这就要求被调用的构造函数必须是可见的，也就是说，必须是public类型的构造函数。如果尝试调用非public类型的构造函数，那么会抛出java.lang.ClassNotFoundException异常。

2. Constructor.newInstance()

Constructor.newInstance()方法可以根据传入的参数，调用任意构造构造函数。这意味着，只要你提供正确的参数类型和数量，Constructor.newInstance()就可以调用对应的构造函数来创建类的实例。

在某些情况下，Constructor.newInstance()可以调用private类型的构造函数。这是Class.newInstance()所无法做到的。但是，如果尝试调用不存在的构造函数，或者调用参数类型不匹配的构造函数，将会抛出java.lang.NoSuchMethodException或java.lang.InstantiationException等异常。

总结起来，Class.newInstance()和Constructor.newInstance()的主要区别在于：Class.newInstance()只能调用无参的public构造函数，而Constructor.newInstance()可以根据传入的参数调用任意类型的构造函数。

## 1.2. 通过反射创建对象

通过反射调用指定的构造方法来创建对象

```
public class User{
    String name;
    public User(){
    }
    public User(String name){
        this.name=name;
    }
}
public void Test(){
@Test
    void test01(){
        Class<?> clazz=User.class;
        // 通过反射拿到构造方法
        Constructor constructor=clazz.getConstructor(String.class);
        // 调用构造方法并传递参数
        User user=(User)constructor.newInstance("张三");
	}
}
```

## 1.3. 获取指定类上指定注解中的内容

```
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface Tx {
    String[] value();
}

@Tx({"John","df"})
public class User{
    String name;
    public User(){
    }
    public User(String name){
        this.name=name;
    }
}

Tx annotation = User.class.getAnnotation(Tx.class);
if (annotation!=null){
    for (String s : annotation.value()) {
    	System.out.println(s);
    }
}
// 输出
// John
// df
```

## 1.4. getDeclaredConstructor与getConstructor

前者会获取全部的构造方法public ,private,protected

后者只获public

# 2. mybatis初始化

## 2.1. 加载mybatis配置

### 2.1.1. 配置文件的准备

mybatis-config.xml的配置文件

```
<?xml version="1.0" encoding="UTF-8" ?>
<!--

Copyright 2009-2022 the original author or authors.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

https://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

-->
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"https://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>

  <properties resource="org/apache/ibatis/databases/blog/blog-derby.properties"/>

  <settings>
    <setting name="cacheEnabled" value="true"/>
    <setting name="lazyLoadingEnabled" value="false"/>
    <setting name="multipleResultSetsEnabled" value="true"/>
    <setting name="useColumnLabel" value="true"/>
    <setting name="useGeneratedKeys" value="false"/>
    <setting name="defaultExecutorType" value="SIMPLE"/>
    <setting name="defaultStatementTimeout" value="25"/>
  </settings>

  <typeAliases>
    <typeAlias alias="Author" type="org.apache.ibatis.domain.blog.Author"/>
    <typeAlias alias="Blog" type="org.apache.ibatis.domain.blog.Blog"/>
    <typeAlias alias="Comment" type="org.apache.ibatis.domain.blog.Comment"/>
    <typeAlias alias="Post" type="org.apache.ibatis.domain.blog.Post"/>
    <typeAlias alias="Section" type="org.apache.ibatis.domain.blog.Section"/>
    <typeAlias alias="Tag" type="org.apache.ibatis.domain.blog.Tag"/>
  </typeAliases>

  <typeHandlers>
    <typeHandler javaType="String" jdbcType="VARCHAR" handler="org.apache.ibatis.builder.CustomStringTypeHandler"/>
  </typeHandlers>

  <objectFactory type="org.apache.ibatis.builder.ExampleObjectFactory">
    <property name="objectFactoryProperty" value="100"/>
  </objectFactory>

  <plugins>
    <plugin interceptor="org.apache.ibatis.builder.ExamplePlugin">
      <property name="pluginProperty" value="100"/>
    </plugin>
  </plugins>

  <environments default="development">
    <environment id="development">
      <transactionManager type="JDBC">
        <property name="" value=""/>
      </transactionManager>
      <dataSource type="UNPOOLED">
        <property name="driver" value="${driver}"/>
        <property name="url" value="${url}"/>
        <property name="username" value="${username}"/>
        <property name="password" value="${password}"/>
      </dataSource>
    </environment>
  </environments>

  <mappers>
    <mapper resource="org/apache/ibatis/builder/AuthorMapper.xml"/>
    <mapper resource="org/apache/ibatis/builder/BlogMapper.xml"/>
    <mapper resource="org/apache/ibatis/builder/CachedAuthorMapper.xml"/>
    <mapper resource="org/apache/ibatis/builder/PostMapper.xml"/>
    <mapper resource="org/apache/ibatis/builder/NestedBlogMapper.xml"/>
  </mappers>

</configuration>
```

properties文件

```
driver=org.apache.derby.jdbc.EmbeddedDriver
url=jdbc:derby:ibderby;create=true
username=
password=
```

mapper.xml文件

```
<?xml version="1.0" encoding="UTF-8" ?>
<!--

Copyright 2009-2023 the original author or authors.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

https://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

-->
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"https://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="org.apache.ibatis.domain.blog.mappers.AuthorMapper">

  <parameterMap id="selectAuthor" type="org.apache.ibatis.domain.blog.Author">
    <parameter property="id" />
  </parameterMap>

  <resultMap id="selectAuthor" type="org.apache.ibatis.domain.blog.Author">
    <id column="id" property="id" />
    <result property="username" column="username" />
    <result property="password" column="password" />
    <result property="email" column="email" />
    <result property="bio" column="bio" />
    <result property="favouriteSection" column="favourite_section" />
  </resultMap>

  <resultMap id="selectImmutableAuthor" type="org.apache.ibatis.domain.blog.ImmutableAuthor">
    <constructor>
      <idArg column="id" javaType="_int" />
      <arg column="username" javaType="string" />
      <arg column="password" javaType="string" />
      <arg column="email" javaType="string" />
      <arg column="bio" javaType="string" />
      <arg column="favourite_section" javaType="org.apache.ibatis.domain.blog.Section" />
    </constructor>
  </resultMap>

  <resultMap id="complexAuthorId" type="org.apache.ibatis.domain.blog.ComplexImmutableAuthorId">
    <constructor>
      <idArg column="id" javaType="_int" />
      <idArg column="username" javaType="string" />
      <idArg column="password" javaType="string" />
      <idArg column="email" javaType="string" />
    </constructor>
  </resultMap>

  <resultMap id="selectComplexImmutableAuthor" type="org.apache.ibatis.domain.blog.ComplexImmutableAuthor">
    <constructor>
      <idArg javaType="org.apache.ibatis.domain.blog.ComplexImmutableAuthorId"
        resultMap="complexAuthorId" />
      <arg column="bio" javaType="string" />
      <arg column="favourite_section" javaType="org.apache.ibatis.domain.blog.Section" />
    </constructor>
  </resultMap>

  <select id="selectAllAuthors" resultType="org.apache.ibatis.domain.blog.Author">
    select * from author
  </select>

  <select id="selectAllAuthorsSet" resultType="org.apache.ibatis.domain.blog.Author">
    select * from author
  </select>

  <select id="selectAllAuthorsVector" resultType="org.apache.ibatis.domain.blog.Author">
    select * from author
  </select>

  <select id="selectAllAuthorsLinkedList" resultType="org.apache.ibatis.domain.blog.Author">
    select * from author
  </select>

  <select id="selectAllAuthorsArray" resultType="org.apache.ibatis.domain.blog.Author">
    select * from author
  </select>

  <select id="selectComplexAuthors" resultMap="selectComplexImmutableAuthor">
    select * from author
  </select>

  <select id="selectAuthorLinkedHashMap" resultType="java.util.LinkedHashMap">
    select id, username from author where id = #{value}
  </select>

  <select id="selectAuthor" parameterMap="selectAuthor" resultMap="selectAuthor">
    select id, username, password, email, bio, favourite_section
    from author where id = ?
  </select>

  <select id="selectImmutableAuthor" parameterMap="selectAuthor"
    resultMap="selectImmutableAuthor">
    select id, username, password, email, bio, favourite_section
    from author where id = ?
  </select>

  <select id="selectAuthorWithInlineParams" parameterType="int"
    resultType="org.apache.ibatis.domain.blog.Author">
    select * from author where id = #{id}
  </select>

  <insert id="insertAuthor" parameterType="org.apache.ibatis.domain.blog.Author">
    insert into Author (id,username,password,email,bio)
    values (#{id},#{username},#{password},#{email},#{bio})
  </insert>

  <update id="updateAuthor" parameterType="org.apache.ibatis.domain.blog.Author">
    update Author
    set username=#{username,
    javaType=String},
    password=#{password},
    email=#{email},
    bio=#{bio}
    where id=#{id}
  </update>

  <delete id="deleteAuthor" parameterType="int">
    delete from Author where id = #{id}
  </delete>


  <update id="updateAuthorIfNecessary" parameterType="org.apache.ibatis.domain.blog.Author">
    update Author
    <set>
      <if test="username != null">username=#{username},</if>
      <if test="password != null">password=#{password},</if>
      <if test="email != null">email=#{email},</if>
      <if test="bio != null">bio=#{bio}</if>
    </set>
    where id=#{id}
  </update>

  <select id="selectWithOptions" resultType="org.apache.ibatis.domain.blog.Author"
    fetchSize="200" timeout="10" statementType="PREPARED" resultSetType="SCROLL_SENSITIVE" flushCache="false" useCache="false">
    select * from author
  </select>

</mapper>
```

### 2.1.2. 加载配置文件

```
// mybatis配置文件的路径
final String resource = "org/apache/ibatis/builder/MapperConfig.xml";
// 通过mybatis的工具将配置文件读为Reader
final Reader reader = Resources.getResourceAsReader(resource);
// 使用建造者模式构建sqlSessionFactory
SqlSessionFactory sqlMapper = new SqlSessionFactoryBuilder().build(reader);
```

SqlSessionFactoryBuilder().build(reader);通过传入的配置文件去构建SqlSessionFactory

为了方便查看代码,只保留了核心代码

```
public SqlSessionFactory build(Reader reader, String environment, Properties properties) {
    try {
        // XMLConfigBuilder内部通过封装的XpathParser解析mybatis-config.xml文件和mapper文件
        // 初始化Configuration类,Xpathparsed类
        XMLConfigBuilder parser = new XMLConfigBuilder(reader, environment, properties);
        // parser.parse()返回Configuration类
        // parser.parse()开始调用reader去解析配置文件
        return build(parser.parse());
    }
}
public SqlSessionFactory build(Configuration config) {
	// 通过传入的Configuration类去构建默认的DefaultSqlSessionFactory,可以发现SqlSessionFactory接口的
	// 默认实现是DefaultSqlSessionFactory
	return new DefaultSqlSessionFactory(config);
}
```

```
public DefaultSqlSessionFactory(Configuration configuration) {
	this.configuration = configuration;
}
```

# 3. 顶层设计

![](学习笔记/Attachments/1694852055781-38c5d486-aa22-4475-a7ea-a79c3a70721e.png)

# 4. mybatis的核心对象

## 4.1. 核心对象及作用

### 4.1.1. SqlSession

SqlSession是顶层的接口设计,具体的功能需要实现类

sqlSession有两个实现类分别是DefaultSqlSession和SqlSessionManager

#### 4.1.1.1. 创建

请注意，SqlSession对象是线程不安全的，每次使用之前需要创建一个新的对象，并在使用完成后及时关闭。

```
// 加载mybatis-config文件
InputStream resourceAsStream = Resources.getResourceAsStream("/mybatis-config.xml");
// 通过建造者模式创建sqlSessionFactory对象
SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
SqlSessionFactory build = sqlSessionFactoryBuilder.build(resourceAsStream);
// 打开sqlSession
SqlSession sqlSession = build.openSession();
```

### 4.1.2. DefaultSqlSession

在 MyBatis 中，SqlSession 的默认实现类是 DefaultSqlSession，这个类实现了 SqlSession 接口，并提供了操作数据库的具体实现。

DefaultSqlSession 中维护了一个 Connection 连接对象，通过这个连接对象来执行 SQL 语句。同时，DefaultSqlSession 还提供了事务管理功能，可以通过 commit() 和 rollback() 方法来提交或回滚事务。

除此之外，DefaultSqlSession 还提供了一些其他的功能，如缓存管理、自动映射等。在使用 MyBatis 时，我们通常会通过 SqlSessionFactory 来获取 SqlSession 对象，而 SqlSessionFactory 默认创建的就是 DefaultSqlSession 对象。

### 4.1.3. SqlSessionManager

SqlSessionManager 是 MyBatis 中用于管理 SqlSession 的类，它实现了 SqlSession 的接口，并提供了对 SqlSession 的管理功能。

SqlSessionManager 中维护了一个 SqlSession 对象池，可以通过 getSqlSession() 方法从池中获取 SqlSession 对象，使用完成后可以通过 closeSqlSession() 方法将 SqlSession 对象归还到池中。

SqlSessionManager 还提供了事务管理功能，可以通过 commit() 和 rollback() 方法来提交或回滚事务。同时，SqlSessionManager 还提供了一些其他的功能，如缓存管理、自动映射等。

需要注意的是，SqlSessionManager 是线程安全的，可以在多个线程中共享使用

### 4.1.4. Executor

#### 4.1.4.1. 作用

1. 执行SQL语句：可以执行各种类型的SQL语句，如查询、插入、更新、删除等。
2. 提供事务管理：为数据库操作提供事务管理功能，保证数据库操作的原子性、一致性和持久性。
3. 缓存管理：管理一级缓存和二级缓存，提高数据库操作的性能。

Executor 类是 SqlSession 的内部实现类之一，它主要负责执行 SQL 语句并返回结果

在Executor中对数据库的操作分为Query和update,其中update不是单纯的update而是insert,update,delete的集合

策略模式是一种行为设计模式，它允许在运行时根据上下文切换不同的算法或策略。在MyBatis中，通过配置文件可以指定使用不同的Executor实现类，这样可以在运行时根据需要选择不同的执行策略。

```
public enum ExecutorType {
  // 易于理解的
  SIMPLE,
  // 复用的
  REUSE,
  // 批量的
  BATCH
}
```

```
// 可以知道simple是excutor的默认方式
protected ExecutorType defaultExecutorType = ExecutorType.SIMPLE;
```

```
@Override
public SqlSession openSession() {
return openSessionFromDataSource(configuration.getDefaultExecutorType(), null, false);
}

private SqlSession openSessionFromDataSource(ExecutorType execType, TransactionIsolationLevel level,
  boolean autoCommit) {
    Transaction tx = null;
    try {
      final Environment environment = configuration.getEnvironment();
      final TransactionFactory transactionFactory = getTransactionFactoryFromEnvironment(environment);
      tx = transactionFactory.newTransaction(environment.getDataSource(), level, autoCommit);
      // 根据配置中的ExecutorType创建不同的Executor
     // 这种设计模式是策略模式（Strategy Pattern）。策略模式是一种行为设计模式，
    //它允许在运行时根据上下文切换不同的算法或策略。在MyBatis中，通过配置文件可以指定使用不同的Executor实现类，
    // 这样可以在运行时根据需要选择不同的执行策略。
      final Executor executor = configuration.newExecutor(tx, execType);
      return new DefaultSqlSession(configuration, executor, autoCommit);
    } catch (Exception e) {
      closeTransaction(tx); // may have fetched a connection so lets call close()
      throw ExceptionFactory.wrapException("Error opening session.  Cause: " + e, e);
    } finally {
      ErrorContext.instance().reset();
    }
}
```

#### 4.1.4.2. 创建

通过ExecutorType创建不同的Excutor

```
public Executor newExecutor(Transaction transaction, ExecutorType executorType) {
	// 策略模式
    executorType = executorType == null ? defaultExecutorType : executorType;
    Executor executor;
    if (ExecutorType.BATCH == executorType) {
      executor = new BatchExecutor(this, transaction);
    } else if (ExecutorType.REUSE == executorType) {
      executor = new ReuseExecutor(this, transaction);
    } else {
      executor = new SimpleExecutor(this, transaction);
    }
    if (cacheEnabled) {
      executor = new CachingExecutor(executor);
    }
    return (Executor) interceptorChain.pluginAll(executor);
}
```

### 4.1.5. StateMentHandler

mybatis中对jdbc 的statement的封装

### 4.1.6. ParameterHandler

对mybatis中写的sql中的@Params("") -> #{} -> ?的处理

### 4.1.7. ResultHandler

对jdbc的ResultSet的处理

### 4.1.8. TypeHandler

java类型与jdbc类型的处理

  

## 4.2. Statement

STATEMENT、PREPARED和CALLABLE是关于JDBC中三种类型的Statement对象。

1. Statement对象：这是JDBC中用于执行SQL语句的最基本的对象。它不支持参数化查询，也就是说，你在执行SQL语句时，如果有需要更新的参数，你需要在SQL语句中直接写入值。这种做法可能引发SQL注入问题，且每次执行相同的SQL语句时，都会重新编译，这会降低执行效率。尽管它可以执行批量更新和删除操作，但并不支持批量查询。
2. PreparedStatement对象：这是一种特殊的Statement对象，用于执行参数化的SQL查询。也就是说，你可以在SQL语句中预留一些占位符，在真正执行SQL语句时，再通过setXXX()方法填充具体的值。这种对象预编译了SQL语句，所以无论是执行多次相同的SQL语句，还是使用不同的参数执行SQL语句，都无需再次编译，这大大提高了执行效率，且能有效防止SQL注入。与Statement对象一样，也可以执行批量更新和删除操作，但对于批量查询，需要通过设置fetchSize来优化性能。
3. CallableStatement对象：这是一种特殊的PreparedStatement对象，用于执行存储过程。你可以通过它调用数据库中的存储过程，且可以传递参数并获取返回值。对于带参数的SQL查询和存储过程调用，CallableStatement对象的效率高于Statement和PreparedStatement。

  

所以，在选择使用哪种类型的Statement对象时，可以根据具体情况来定：如果你需要执行大量的、相同的、不涉及参数的SQL查询，且对效率没有太高要求，可以选择使用Statement；如果你需要执行参数化的SQL查询，对效率有较高要求，且希望有效防止SQL注入问题，应选择PreparedStatement；如果你需要执行存储过程，应选择CallableStatement。

### 4.2.1. 在xml中修改statement的类型

```
<insert id="insertAuthorInvalidInsert" statementType="CALLABLE">
  <selectKey keyProperty="id" resultType="int" order="BEFORE">
    select CAST(RANDOM()*1000000 as INTEGER) a from SYSIBM.SYSDUMMY1
  </selectKey>
  insert into Author (id,username,password,email,bio,favourite_section_xyz)
  values(
  #{id}, #{username}, #{password}, #{email}, #{bio}, #{favouriteSection:VARCHAR}
  )
</insert>
```

## 4.3. SqlSession操作的两种方式

### 4.3.1. 通过mapper实现类

该方法是我们常用的,他是mybatis通过代理模式对第二种方式的封装

```
SqlSession sqlSession = sqlMapper.openSession();
// mybatis 通过动态代理实现了我们传入的接口
AuthorMapper mapper = sqlSession.getMapper(AuthorMapper.class);
List<Author> authors = mapper.selectAllAuthors();
for (Author author : authors) {
  System.out.println(author.toString());
}
```

### 4.3.2. 通过statementId

```
// 上面的方式底层使用的也是对下面这个方法的封装
List<Author> objects = sqlSession.selectList("org.apache.ibatis.domain.blog.mappers.AuthorMapper.selectAllAuthors");
for (Author author : authors) {
  System.out.println(author.toString());
}
```

### 4.3.3. 实现简单的mybatis动态代理

实现java的拦截器接口

```
class InvocationHandler implements java.lang.reflect.InvocationHandler {
    private SqlSession sqlSession;
    private Class<?> clazz;

    InvocationHandler(SqlSession sqlSession,Class<?> clazz) {
      this.sqlSession = sqlSession;
      this.clazz=clazz;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
      // id = namespace+.+函数名
      String id = clazz.getName()+"." + method.getName();
      // 实现指定接口
      return sqlSession.selectList(id);
    }
  }
```

对要实现的Mapper进行代理

```
InvocationHandler invocationHandler = new InvocationHandler(sqlSession,AuthorMapper.class);
AuthorMapper o = (AuthorMapper) Proxy.newProxyInstance(AuthorMapper.class.getClassLoader(), new Class[]{AuthorMapper.class}, invocationHandler);
List<Author> authors1 = o.selectAllAuthors();
for (Author oi : authors1) {
  System.out.println(oi.toString());
}
```

可以看到我们也实现了类似的功能,invocation类需要sqlSession来执行sql,需要authorMapper.class来拼接statementId,而我们使用mybatis的时候同样是调用 sqlSession.getMapper(),同样需要sqlSession,而且在getMapper()的时候同样需要我们传入authorMapper.class

我们当前只是针对单独的statement,在mybatis中的实现更为复杂.