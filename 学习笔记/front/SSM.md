# SSM(Spring+Spring MVC+MyBatis)框架整合

## 前戏:熟悉三个框架

### 1. mybatis

#### mybatis的了解,及用idea创建项目

- MyBatis 是一款优秀的**持久层框架**
- MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集的过程
- MyBatis 可以使用简单的 XML 或注解来配置和映射原生信息，将接口和 Java 的 实体类 【Plain Old Java Objects,普通的 Java对象】映射成数据库中的记录。
- MyBatis 本是apache的一个开源项目ibatis, 2010年这个项目由apache 迁移到了google code，并且改名为MyBatis 。
- 2013年11月迁移到**Github** .
- Mybatis官方文档 : http://www.mybatis.org/mybatis-3/zh/index.html
- GitHub : https://github.com/mybatis/mybatis-3

#### 引入mybatis的maven包,以及相关依赖包

分别是mysql的驱动,junit包可以用来测试代码,以及mybatis的包

```
      <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
          <version>8.0.29</version>
      </dependency>
      <dependency>
          <groupId>junit</groupId>
          <artifactId>junit</artifactId>
          <version>4.13.2</version>
          <scope>test</scope>
      </dependency>
      <dependency>
          <groupId>org.mybatis</groupId>
          <artifactId>mybatis</artifactId>
          <version>3.5.9</version>
      </dependency>
```

#### idea连接数据库

![](学习笔记/Attachments/1656129189878-aca39b2a-7c12-40d0-b5c5-be600817ceb4.png "输入数据库用户名和密码")

![](学习笔记/Attachments/1656129116213-bfa75dcb-94e9-4933-84ef-c77b2beec86d.png "连接后")

#### 创建项目的基本结构

实体类包 pojo

持久层包 dao

工具类包 utils

![](学习笔记/Attachments/1656129558753-e9f61e43-3e3f-4035-baf5-df30a394b3a7.png "mybatis项目基本结构")

#### 创建实体类(pojo)

根据需要的表创建实体类

![](学习笔记/Attachments/1656129660332-28991710-bb7b-4e85-a6c9-ec28e94b8d4e.png "需要的表")

##### 创建student,teacher类

```
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Student {
    private int id;
    private String name;
    private Teacher teacher;
}
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Teacher {
    private int id;
    private String name;
    private List<Student> students;
}
```

![](学习笔记/Attachments/1656129680769-34ff1e00-efaf-4eff-8b30-96b21fc3dc1a.png "创建的实体类")

其中实体类的字段需要对应表中得字段

#### 创建持久层接口

```
package com.manager.dao;
import com.manager.pojo.Student;
import java.util.List;
public interface StudentMapper {
   List<Student> studentList();
}
```

```
package com.manager.dao;
import com.manager.pojo.Teacher;
public interface TeacherMapper {
    Teacher getTeacher(int tno);
}
```

#### 在rescources中创建xml映射文件

![](学习笔记/Attachments/1656130244789-93e414dc-89c6-4a39-bc4e-25700569449a.png "目录")

之后我们的对数据库的操作将在这里编写

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.manager.dao.StudentMapper">

</mapper>
```

结果集映射:

1. 一个类中包含其他类
2. 一个类中包含其他类的集合

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.manager.dao.StudentMapper">
    <select id="studentList" resultMap="StudentTeacher">
        select s.id as s_id, s.name as s_name,t.id as t_id, t.name as t_name
        from mybatis_study.student s,
             teacher t
        where s.tid = t.id;
    </select>
    <!--column 是数据库中的列名或者别名 property是在实体类中的名字-->
    <!-- 结果集映射 -->
    <resultMap id="StudentTeacher" type="com.manager.pojo.Student">
        <id property="id" column="s_id"/>
        <result property="name" column="s_name"/>
        <association  property="teacher" javaType="Teacher">
            <id column="t_id" property="id"/>
            <result column="t_name" property="name"/>
        </association>
    </resultMap>
</mapper>
```

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.manager.dao.TeacherMapper">

    <select id="getTeacher" resultMap="TeacherStudent">
        select student.id s_id , student.name s_name, teacher.name t_name , tid t_id
        from mybatis_study.teacher,mybatis_study.student where teacher.id=student.tid and
                tid=#{id};
    </select>
    <resultMap id="TeacherStudent" type="Teacher">
        <id property="id" column="t_id" />
        <result property="name" column="t_name" />
        <collection property="students" ofType="Student">
            <result property="id" column="s_id" />
            <result property="name" column="s_name" />
<!--            <result property="tid" column="t_id" />-->
        </collection>
    </resultMap>
</mapper>
```

至此,我们已近实现了持久层对数据库的操作

#### 创建数据库的配置文件

其中包含数据库的用户名,密码数据库的地址,以及使用的驱动,为编写mybatis做准备

```
username=root
password=123
url=jdbc:mysql://localhost:3306/mybatis_study?useSSL=true&useUnicode=true&characterEncoding=UTF-8
driver=com.mysql.jdbc.Driver
```

#### 编写mybatis的配置

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

  <!--       导入数据库配置-->
    <properties resource="dp.properties"/>

<!--    这个标签可以把实体类的名称作为类型别名  方便在映射中使用-->
<typeAliases>
    <package name="com.manager.pojo"/>
</typeAliases>

    <!--环境标签-->
    <environments default="development">
        <!--可以配置多个环境-->
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="password"/>
            </dataSource>
        </environment>

        <environment id="test">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
<!--    映射持久层接口-->
    <mappers>
<!--        <mapper resource="com/manager/dao/userMapper.xml"/>-->
<!--        <mapper class="com.manager.dao.TeacherMapper"/>-->
<!--        <mapper class="com.manager.dao.StudentMapper"/>-->
        <package name="com.manager.dao"/>
    </mappers>
</configuration>
```

#### 编写工具类

```
package com.manager.utils;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import java.io.IOException;
import java.io.InputStream;

public class MyBatisUtils {
   static SqlSessionFactory sqlSessionFactory;

    static {
        try {
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

    public static SqlSession getMybatisUtils() {
        return sqlSessionFactory.openSession();
    }
}
```

#### 最后编写测试类测试

```
 @Test
    public void getTeacherTest(){
        SqlSession sqlSession = MyBatisUtils.getMybatisUtils();
        TeacherMapper mapper = sqlSession.getMapper(TeacherMapper.class);
        Teacher teacher = mapper.getTeacher(1);
        System.out.println(teacher);
        sqlSession.close();
    }
```

如果需要添加日志需要添加以下代码

```
    <settings>
<!--        标准日志输出-->
<!--        <setting name="logImpl" value="STDOUT_LOGGING"/>-->
<!--        logj4-->
        <setting name="logImpl" value="LOG4J"/>
    </settings>
```

如果是log4j还需要添加配置

导入依赖

```
<dependency>
   <groupId>log4j</groupId>
   <artifactId>log4j</artifactId>
   <version>1.2.17</version>
</dependency>
```

```
 log4j.rootLogger=DEBUG,console,file
 
log4j.appender.console = org.apache.log4j.ConsoleAppender
log4j.appender.console.Target = System.out
log4j.appender.console.Threshold=DEBUG
log4j.appender.console.layout = org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=[%c]-%m%n

log4j.appender.file = org.apache.log4j.RollingFileAppender
log4j.appender.file.File=./log/debug.log
log4j.appender.file.MaxFileSize=10mb
log4j.appender.file.Threshold=DEBUG
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=[%p][%d{yy-MM-dd}][%c]%m%n

log4j.logger.org.mybatis=DEBUG
log4j.logger.java.sql=DEBUG
log4j.logger.java.sql.Statement=DEBUG
log4j.logger.java.sql.ResultSet=DEBUG
log4j.logger.java.sq1.PreparedStatement=DEBUG
```

如果报错还有可能是资源过滤问题

```
 <build>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
        </resources>
    </build>
```

---

**学习知识点**

- 所有的增删改操作都需要提交事务！
-   `session.commit();`提交事务,重点!不写的话不会提交到数据库
- 接口所有的普通参数，尽量都写上@Param参数，尤其是多个参数时，必须写上！
- `User selectUserByNP(@Param("username") String username);`
- 有时候根据业务的需求，可以考虑使用map传递参数！
- 为了规范操作，在SQL的配置文件中，我们尽量将Parameter参数和resultType都写上！

**核心配置文件**

- mybatis-config.xml 系统核心配置文件
- MyBatis 的配置文件包含了会深深影响 MyBatis 行为的设置和属性信息。
- 能配置的内容如下：

```
configuration（配置）
properties（属性）
settings（设置）
typeAliases（类型别名）
typeHandlers（类型处理器）
objectFactory（对象工厂）
plugins（插件）
environments（环境配置）
environment（环境变量）
transactionManager（事务管理器）
dataSource（数据源）
databaseIdProvider（数据库厂商标识）
mappers（映射器）
<!-- 注意元素节点的顺序！顺序不对会报错 -->
```

**environments元素**

```
<environments default="development">
 <environment id="development">
   <transactionManager type="JDBC">
     <property name="..." value="..."/>
   </transactionManager>
   <dataSource type="POOLED">
     <property name="driver" value="${driver}"/>
     <property name="url" value="${url}"/>
     <property name="username" value="${username}"/>
     <property name="password" value="${password}"/>
   </dataSource>
 </environment>
</environments>
```

- 配置MyBatis的多套运行环境，将SQL映射到多个不同的数据库上，必须指定其中一个为默认运行环境（通过default指定）
- 子元素节点：**environment**

- dataSource 元素使用标准的 JDBC 数据源接口来配置 JDBC 连接对象的资源。
- 数据源是必须配置的。
- 有三种内建的数据源类型type="[UNPOOLED|POOLED|JNDI]"）
- unpooled：这个数据源的实现只是每次被请求时打开和关闭连接。
- **pooled**：这种数据源的实现利用“池”的概念将 JDBC 连接对象组织起来 , 这是一种使得并发 Web 应用快速响应请求的流行处理方式。
- jndi：这个数据源的实现是为了能在如 Spring 或应用服务器这类容器中使用，容器可以集中或在外部配置数据源，然后放置一个 JNDI 上下文的引用。
- 数据源也有很多第三方的实现，比如dbcp，c3p0，druid等等....
- 详情：点击查看官方文档
- 这两种事务管理器类型都不需要设置任何属性。
- 具体的一套环境，通过设置id进行区别，id保证唯一！
- 子元素节点：transactionManager - [ 事务管理器 ]<!-- 语法 -->  
    <transactionManager type="[ JDBC | MANAGED ]"/>
- 子元素节点：**数据源（dataSource）**

**执行流程**

1. sqlSessionFactoryBuilder通过配置文件生成sqlSessionFactory
2. SQLSessionFactory生成SQLSession
3. sqlSession生成sqlMapper
4. sqlMapper进行操作
5. 结束

映射文件

mybatis会根据这些查询的列名(会将列名转化为小写,数据库不区分大小写) , 去对应的实体类中查找相应列名的set方法设值 , 由于找不到setPwd() , 所以password返回null ; 【自动映射】

当结果集中字段名称不同时

1. 为列名指定别名 , 别名和java实体类的属性名一致
2. 使用结果集映射->ResultMap 【推荐】

```
<resultMap id="UserMap" type="User">
   <!-- id为主键 -->
   <id column="id" property="id"/>
   <!-- column是数据库表的列名 , property是对应实体类的属性名 -->
   <result column="name" property="name"/>
   <result column="pwd" property="password"/>
</resultMap>

<select id="selectUserById" resultMap="UserMap">
  select id , name , pwd from user where id = #{id}
</select>
```

**注解开发(sql语句写在注解中)**

利用注解开发就不需要mapper.xml映射文件了

```
//根据id查询用户
@Select("select * from user where id = #{id}")
User selectUserById(@Param("id") int id);
```

```
<!--使用class绑定接口-->
<mappers>
   <mapper class="com.kuang.mapper.UserMapper"/>
</mappers>
```

```
public interface deptMapper {
    @Select("select * from dept;")
    List<dept> deptList();

    @Select("select * from dept where dept.deptno=#{id};")
    dept getDept(@Param("id") int id);
}
```

**param注解**

- 在方法只接受一个参数的情况下，可以不使用@Param。
- 在方法接受多个参数的情况下，建议一定要使用@Param注解给参数命名。
- 如果参数是 JavaBean ， 则不能使用@Param。
- 不使用@Param注解时，参数只能有一个，并且是Javabean。

$与#

- #{} 的作用主要是替换预编译语句(PrepareStatement)中的占位符? (推荐)
- ${} 的作用是直接进行字符串替换

**结果集映射**

```
<select id="deptList" resultMap="map1">
    select deptno as d1, dname as d2, loc as d3 from dept;
</select>
    <resultMap id="map1" type="dept">
        <!--column数据库中的字段，property实体类中的属性-->
        <result column="d1" property="deptno"/>
        <result column="d2" property="dname"/>
        <result column="d3" property="loc"/>
    </resultMap>

    <select id="getDept" resultMap="map1">
        select deptno as d1, dname as d2, loc as d3 from dept where deptno=#{id};
    </select>
```

**结果集映射(按照结果)**

使用结果集映射在部门实体类中添加员工的集合`_private_ _List_<_emp_> emps;`

`autoMapping="true"`自动映射其他字段,联表查询,得到员工及部门的信息,通过结果集映射去实现1对n的关系

```
    <select id="deptList" resultMap="list1">
        select dept.deptno d1, dname , loc ,emp.empno eno, ename, job, mgr, hiredate, sal, comm,  sex  from dept,emp where dept.deptno=emp.deptno;
    </select>
    <resultMap id="list1" type="dept" autoMapping="true">
      
      <result column="d1" property="deptno"/>

        <collection property="emps" ofType="emp" autoMapping="true">
            <result column="eno" property="empno"/>
            <result column="d1" property="deptno"/>
        </collection>
      
    </resultMap>
```

**结果集映射(嵌套查询)**

```
<select id="deptList" resultMap="map1">
    select * from dept;
</select>

<resultMap id="map1" type="dept">
        <result column="deptno" property="deptno"/>
        <collection property="emps" ofType="emp" column="deptno" select="s1"/>
</resultMap>

<select id="s1"  resultType="emp" >
    select * from emp where emp.deptno=#{deptno};
</select>
```

`<collection property="emps" ofType="emp" column="deptno" select="s1"/>` 中一定要有column,这个参数是传递给下一个查询使用的

**结果集映射(一对一)**

```
    <select id="s1" resultMap="map1">
        select *
        from emp,
             dept;
    </select>

    <resultMap id="map1" type="emp">
        <result column="deptno" property="deptno"/>
        <association property="dept1" column="deptno" javaType="dept" select="s2"/>
    </resultMap>


    <select id="s2" resultType="dept">
        select *
        from dept
        where deptno = #{deptno};
    </select>
```

**小节**

1、关联-association(一个元素)

2、集合-collection(好多元素变成集合)

3、所以association是用于一对一和多对一，而collection是用于一对多的关系

4、JavaType和ofType都是用来指定对象类型的

- JavaType是用来指定pojo中属性的类型
- ofType指定的是映射到list集合属性中pojo的类型。

**动态sql**

```
<select id="getEmp" resultType="emp">
    select * from emp where
    <if test="id == 1">
        empno = 7369
    </if>
    <if test="id!= 1">
         empno = 7521
    </if>
</select>
```

```
 <!--注意set是用的逗号隔开-->
    <update id="updateEmp" parameterType="map">
        update emp
        <set>
            <if test=" job != null">
                job = #{job},
            </if>
            <if test=" ename != null">
               ename =#{ename}
            </if>
        </set>
        where empno = #{sid};
    </update>
```

`<where`标签可以去除多余的and和or

**缓存**

**一级缓存**是mybatis自己的,一个sqlsession一个缓存,在sqlsession查询一次后会留下缓存,

数据库更行后缓存失效需要改变,sqlsession关闭后缓存清理,也可以手动清理缓存 `session.clearCache();`手动清除缓存

**二级缓存**同一个namespace用一个缓存,一级缓存中的数据被保存到二级缓存中.

mybatis-config.xml中添加设置`<setting name="cacheEnabled" value="true"/>   `

```
<cache
 eviction="FIFO"
 flushInterval="60000"
 size="512"
 readOnly="true"/>
<!-- 创建了一个 FIFO 缓存，每隔 60 秒刷新，
最多可以存储结果对象或列表的 512 个引用，
而且返回的对象被认为是只读的，
因此对它们进行修改可能会在不同线程中的调用者产生冲突。-->
```

```
 @org.junit.Test
    public void cacheTest(){
        // 创建两个会话
        SqlSession utils1 = MybatisUtils.getMybatisUtils();
        SqlSession utils2 = MybatisUtils.getMybatisUtils();
        // 查询数据
        deptMapper mapper = utils1.getMapper(deptMapper.class);
        List<dept> list = mapper.deptList();
        // 关闭会话一
        utils1.close();
        // 查询相同的数据
        deptMapper mapper2 = utils2.getMapper(deptMapper.class);
        List<dept> list2 = mapper2.deptList();
        // 比较两次的数据,可知是同一个数据
        System.out.println(list==list2);
        // 关闭会话二
        utils2.close();
    }
```

**结论**

- 只要开启了二级缓存，我们在同一个Mapper中的查询，可以在二级缓存中拿到数据
- 查出的数据都会被默认先放在一级缓存中
- 只有会话提交或者关闭以后，一级缓存中的数据才会转到二级缓存中

![](学习笔记/Attachments/1656237499482-32d46a31-2b67-425b-a111-94c0f4f145ce.png "缓存原理")

学习参考狂神博客:

[https://blog.csdn.net/qq_33369905/article/details/105828924?spm=1001.2014.3001.5501](https://blog.csdn.net/qq_33369905/article/details/105828924?spm=1001.2014.3001.5501)

### 2. spring

#### 概述

Spring的英文翻译为春天，可以说是给Java程序员带来了春天，因为它极大的简化了开发。我得出一个公式：Spring = 春天 = Java程序员的春天 = 简化开发。最后的简化开发正是Spring框架带来的最大好处。

  

Spring是一个开放源代码的设计层面框架，它是于2003 年兴起的一个轻量级的Java 开发框架。由Rod Johnson创建，其前身为Interface21框架，后改为了Spring并且正式发布。

#### 核心

Spring 的理念：不去重新发明轮子。其核心是控制反转（IOC）和面向切面（AOP）

##### IOC(控制反转)

对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方，个人认为所谓控制反转就是：获得依赖对象的方式反转了。

![](学习笔记/Attachments/1656290576384-c15b797c-2473-4f85-88e0-c73f902096a5.png "包结构")

其中dao包下面UserMapper是抽象类,其余是两个实现类

```
 @org.junit.Test
    public void Test01(){
        UserServiceImpl service = new UserServiceImpl();
        service.setUserMapper(new UserMapper11Impl());
        service.fun1();
        service.setUserMapper(new UserMapperImpl());
        service.fun1();
    }
```

可见一个对象两次调用结果不同,创建一个service层对象,我们需要使用哪种实现就使用set方法注入,不需要创建两个不同的service对象,每次变动 , 不需要需要修改大量代码 . 这种设计的耦合性降低.

#### 快速开始spring(hello,spring)

导入依赖

```
<dependency>
   <groupId>org.springframework</groupId>
   <artifactId>spring-webmvc</artifactId>
   <version>5.1.10.RELEASE</version>
</dependency>
```

创建hello类

```
public class hello {
 @Autowired
    private String name;
    public void fun(){
        System.out.println("hello,spring"+name);
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
}
```

在rescources创建spring配置文件application.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean class="com.manager.dao.hello" id="hello">
       <!--注意: 这里的name并不是属性 , 而是set方法后面的那部分 , 首字母小写-->
       <!--引用另外一个bean , 不是用value 而是用 ref-->
        <property name="name" value="哈哈哈"/>
    </bean>

</beans>
```

测试

```
 @org.junit.Test
    public void Test01(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("application.xml");
        hello bean = applicationContext.getBean("hello", hello.class);
        bean.fun();
    }
```

![](学习笔记/Attachments/1656292672720-dd098afd-32d3-405d-a8ce-8ae94ade18b0.png "运行结果")

整个过程就new了applicationContext,没有主动new hello类却传递了参数,调用了hello的方法

所谓的IoC,一句话搞定 : 对象由Spring 来创建 , 管理 , 装配 !

##### ioc创建的对象

```
   @org.junit.Test
    public void Test01(){

        System.out.println("0");
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("application.xml");
        System.out.println("1");
        hello bean = 
        System.out.println("2");
        bean.fun();
    }
```

```
    <bean class="com.manager.dao.hello" id="hello" autowire="byName">
       <property name="name" value="哈哈哈"/>

    </bean>
```

![](学习笔记/Attachments/1656293154729-35be233d-de95-41dd-9028-ce960eb479c3.png "运行结果")

可见,在配置文件加载的时候。其中管理的对象都已经初始化了！

上面的是使用无参构造加set注入参数

下面使用有参构造注入

```
    <bean class="com.manager.dao.hello" id="hello" autowire="byName">
      <!-- set方法-->
      <!-- <property name="name" value="哈哈哈"/>-->
      
      <!-- 根据参数名字构造方法-->
      <!-- <constructor-arg name="name" value="你好啊"/>-->
      
      <!-- 根据类型的构造方法-->
        <constructor-arg type="java.lang.String" value="你好啊kj "/>
    </bean>
```

```
<!--设置别名：在获取Bean的时候可以使用别名获取-->
<alias name="hello" alias="hello4"/>
<!-- 可以有多个别名,可以用id-->
<bean class="com.manager.dao.hello" id="hello" name="hello2,hello3">
```

![](学习笔记/Attachments/1656295992840-dd4ac45d-e32e-445c-8e2c-6ec5765b4ce4.png "通过不同别名得到bean")

  

#### spring配置文件

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    
</beans>
```

导入多个bean.xml`<import resource="{path}/beans.xml"/>`

依赖注入（Dependency Injection,DI

- 依赖 : 指Bean对象的创建依赖于容器 . Bean对象的依赖资源 .
- 注入 : 指Bean对象所依赖的资源 , 由容器来设置和装配 .

##### 通过set方式注入

```
<!-- 通过空的构造方法创建类，然后通过set方法的方式注入-->
<bean id="add" class="com.manager.pojo.Address">
    <!--set注入 -->
      <property name="address" value="北京"/>
	<!--构造方法注入-->
    <constructor-arg index="0" value="太原"/>

</bean>
```

##### 复杂类型的注入

```
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
    <!--set--> 
    <property name="set1">
    <set>
      <value>LOL</value>
         <value>BOB</value>
         <value>COC</value>
      </set>
    </property>
    
     <!--properties注入--> 
     <property name="info">
     <props>
         <prop key="学号">20190604</prop>
         <prop key="性别">男</prop>
         <prop key="姓名">小明</prop>
     </props>
 </property>
    
    
    <!--空值--> 
<property name="email" value=""/>
       <null/>
    </bean>
```

```
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
```

###### p name space

```
xmlns:p="http://www.springframework.org/schema/p"
<!-- 普通的set注入-->
<bean name="classic" class="com.example.ExampleBean">
  
        <property name="email" value="someone@somewhere.com"/>
  
    </bean>
<!-- p- name space set注入-->
    <bean name="p-namespace" class="com.example.ExampleBean"
          
        p:email="someone@somewhere.com"/>
```

###### c name space

```
<!-- 普通的有参注入-->
<bean class="com.manager.dao.hello" id="hello" name="hello2,hello3">
            <constructor-arg name="name" value=""/>
</bean>

<!-- c命名空间-->   
<bean class="com.manager.dao.hello" id="hello" name="hello2,hello3" c:name="c命名空间">
    </bean>
```

注同一种注入方式不能同时使用命名空间或者普通的方式

```
 <bean class="com.manager.dao.hello" id="hello" name="hello2,hello3" c:name="c命名空间">
<!--        <constructor-arg name="name" value="asdfasdfasdf"/>-->
   </bean>
```

  

##### scope 作用域

```
<!--
    scope 作用域
    Singletons 单例模式 从容器中得到的对象是同一个对象
    prototype  原型模式 从容器中得到的对象是不同的对象
-->

@Scope("prototype")
<bean id="s1" class="com.manager.pojo.Student" scope="prototype">
```

同一个类不同id创建的对象是不同的

![](学习笔记/Attachments/1656297915295-d21e8798-2d2e-4b16-a3ea-6d25fed112cc.png "容器")

![](学习笔记/Attachments/1656297944443-976765d5-f1ab-43b6-9ecd-5d62d5f8fc09.png "测试")

![](学习笔记/Attachments/1656297969456-d52f6ae6-c992-4de2-8254-68e84f6977b5.png "结果")

同一个id获取的默认是同一个对象(即singleton单例模式)

默认为

`<bean id="ServiceImpl" class="com.manager.hello" scope="singleton">`

要使得每次都是新的对象实例改变scope为prototype(多例模式)或者 `singleton="false"`

```
 <bean class="com.manager.dao.hello" id="h1"  p:name="p命名空间">
    </bean>

    <bean class="com.manager.dao.hello" id="h2" c:name="c命名空间" scope="prototype">
    </bean>
```

##### 自动装箱(autowire)

- 使用Autowired标签 spring会自动注入依赖类型通过**set**注入的方式

```
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

```
autowire="byName"
autowire="byType"
```

##### 通过@Autowire注解去自动装配

```
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

```
//如果允许对象为null，设置required = false,默认为true
@Autowired(required = false)
@Autowired
public void setMovieFinder(@Nullable MovieFinder movieFinder) {
        ...
}
```

##### @Autowire的注入方式

```
public class dog extends animal{
    @Autowired(required = false)
    private cat c1;
}
```

```
 @Autowired
    dog(String name ,cat c1){
        this(name);
        this.c1=c1;
    }
```

```
@Autowired
    public void setC1(cat c1) {
        System.out.println("set注入");
        this.c1 = c1;
    }
```

- @Autowired注入成员变量，利用field反射注入，要等类加载完了才注入bean；
- @Autowired注入构造方法中，利用构造器注入，有先后依赖关系；
- @Autowired是根据类型自动装配的，加上@Qualifier则可以根据byName的方式自动装配
- @Qualifier不能单独使用。

##### 配合@autowired实现按名称匹配

```
    <bean  name="c2" class="com.manager.cat" id="c1">
        <constructor-arg name="name" value="猫"/>
    </bean>

    <bean id="dog" class="com.manager.dog" autowire="byName">
        <qualifier value="c2"/>
        <constructor-arg name="name" value="狗"/>
    </bean>
```

注 :实测被匹配bean只有name不行,需要有id

```
public class dog extends animal{
//    @Autowired(required = false)
    @Autowired
    @Qualifier(value = "c2")
    private cat c1;
}
```

##### 注解开发

<!--指定注解扫描包-->

`<context:component-scan base-package="com.manager"/>`

`<context:annotation-config/>`

```
@Data
@AllArgsConstructor
@NoArgsConstructor
@Component("s1")
// 相当于配置文件中 <bean id="s1" class="当前注解的类"/>
public class student {
    String name;
    int sid;
}
```

```
public class student {
    @Value(value = "风吹麦浪")
     // 相当于配置文件中 <property name="name" value="风吹麦浪"/>
    String name;
    @Value(value = "1")
    int sid;
}
```

```
@org.junit.Test
    public void test1(){
         ApplicationContext applicationContext = new ClassPathXmlApplicationContext("application.xml");
        student s1 = applicationContext.getBean("s1", student.class);
        System.out.println(s1.toString());
        System.out.println(s1.getSid()==1);
    }
```

结论:

- @Value写在属性上通过反射注入,写在set上通过set方法注入

##### 衍生注解

@Component三个衍生注解

为了更好的进行分层，Spring可以使用其它三个注解，功能一样，目前使用哪一个功能都一样。

@Controller：web层

@Service：service层

@Repository：dao层

##### 作用域@scope

```
    @Controller("s1")
    @Scope("prototype")
    public class student {
        @Value("风吹麦浪")
        public String name;
    }
```

#### 使用java配置工具类进行配置

创建配置工具类Myconfig使用@Configuration注解标记工具类

```
@Configuration
@Import(MyConfig2.class)  //导入合并其他配置类，类似于配置文件中的 inculde 标签
public class Application {

    @Bean //通过方法注册一个bean，这里的返回值就Bean的类型，方法名就是bean的id！
    public student s1(){
        return new student();
    }
    @Bean //通过方法注册一个bean，这里的返回值就Bean的类型，方法名就是bean的id！
    public teacher t1(){
        return new teacher();
    }
}
```

注:方法名就是bean的id 不需要再在类的上面写@Controller等注解去注册bean不生效

```
public class student {
    @Value(value = "风吹麦浪")
    public
    String name;
    @Value(value = "1")
    public
    int sid;
}
```

```
public class teacher {
    @Value("10086")
    int tid;
    @Value("老师")
    String name;
    @Autowired
     @Qualifier("s1")
    public student s12;
}
```

```
    @org.junit.Test
    public void test1(){
        ApplicationContext applicationContext =
                new AnnotationConfigApplicationContext(Application.class);
        student s1 = applicationContext.getBean("s1", student.class);
        System.out.println(s1.toString());
        System.out.println(s1.sid==1);

    }
    @org.junit.Test
    public void test2(){
        ApplicationContext applicationContext =
                new AnnotationConfigApplicationContext(Application.class);
        teacher t1 = applicationContext.getBean("t1", teacher.class);
        System.out.println(t1.s12.name);
    }ApplicationContext context = new AnnotationConfigApplicationContext(MyUtils1.class);
User user = context.getBean("user", User.class);
System.out.println(user.name);
```

##### 代理

![](学习笔记/Attachments/1656378476042-53da108c-b1c9-4c3f-8cff-4eaeb07721cb.png)

即我们不能直接访问我们需要的对象,而是通过一个代理类去访问,这个代理类帮助我们给这个对象打交道,并且赋予这个类不具备的功能

###### 静态代理

例子

```
public interface UserService {
    // 对用户的操作
    void addUser();
    void deleteUser();
    void updateUser();
    void queryUser();
}
```

```
public class UserServiceImpl implements UserService{
    @Override
    public void addUser() {
        System.out.println("添加用户");
    }
    @Override
    public void deleteUser() {
        System.out.println("删除用户");
    }
    @Override
    public void updateUser() {
        System.out.println("更新用户");
    }
    @Override
    public void queryUser() {
        System.out.println("查询用户");
    }
}
```

```
public class UserProxy implements UserService{
     // 被代理的对象
    UserServiceImpl userService;
    public UserProxy(){
        userService=new UserServiceImpl();
    }
    // 增加了被代理类不具备的日志功能
    @Override
    public void addUser() {
        userService.addUser();
        System.out.println("log------addUser");
    }
    @Override
    public void deleteUser() {
        userService.deleteUser();
        System.out.println("log------deleteUser");

    }
    @Override
    public void updateUser() {
        userService.updateUser();
        System.out.println("log------updateUser");
    }
    @Override
    public void queryUser() {
        userService.queryUser();
        System.out.println("log------queryUser");
    }
}
```

优点:

- 在不改变原来的类的基础上增加了新的功能,不需要去重构原来的代码

###### 动态代理

利用反射实现动态代理,利用`**InvocationHandler**`接口,代理类实现这个接口,`**Proxy.newProxyInstance()**`方法

```
public class UserProxy implements InvocationHandler {

    UserServiceImpl userService;
    public UserProxy(){
        userService=new UserServiceImpl();
    }
    //生成代理类，重点是第二个参数，获取要代理的抽象角色！之前都是一个角色，现在可以代理一类角色
    public Object getProxy(){
        return Proxy.newProxyInstance(this.getClass().getClassLoader(),
                userService.getClass().getInterfaces(),this);
    }
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        // 被代理类执行方法前
        Object o = method.invoke(userService, args);
        // 执行后
        return o;
    }
}
```

  

##### aop编程

AOP（Aspect Oriented Programming）意为：面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。

```
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.4</version>
</dependency>
```

![](学习笔记/Attachments/1656382153378-2986dae3-78fd-4ac6-8c22-c5eed125ab68.png)

  

实现以上需要的接口

```
@Component("log")
public class log implements MethodBeforeAdvice {
    @Override
    public void before(Method method, Object[] args, Object target) throws Throwable {
        System.out.println("执行"+method.getName());
    }
}
```

```
    <context:annotation-config/>

    <context:component-scan base-package="com.manager"/>

    <!--aop的配置-->
    <aop:config>
        <!--切入点  expression:表达式匹配要执行的方法-->
        <aop:pointcut id="pointcut" expression="execution(* com.manager.dao.UserServiceImpl.*(..))"/>
        <!--执行环绕; advice-ref执行方法 . pointcut-ref切入点-->
        <aop:advisor advice-ref="log" pointcut-ref="pointcut"/>
    </aop:config>
```

`expression=`后是匹配类的方法

```
 @org.junit.Test
    public void Test1(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("application.xml");
        UserService user = applicationContext.getBean("user", UserService.class);
        user.addUser();
    }
```

![](学习笔记/Attachments/1656384548381-853415ee-517b-45a9-a3c5-ec5847179755.png "运行结果")

  

#### spring整合mybatis

##### 整合前的mybatis

1. 持久层接口
2. 在xml映射中实现接口
3. 创建mybatis的配置文件
4. 在mybatis-config.xml中配置数据源等信息
5. 创建工具类MybatisUtils在其中创建sqlsessionFactorybuild构建sqlsessionFactory,并用sqlsessionFactory生产sqlsession
6. 通过sqlsession得到接口的实现类,通过其进行操作

##### 整合后创建使用mybatis

###### 导入依赖

```
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>2.0.2</version>
    </dependency>
```

###### mybatis的基本配置

1. pojo 实体类的配置
2. dao 接口
3. xml 接口映射文件的实现
4. mybatis-config的配置文件

###### spring的基本配置

1. spring的配置文件
2. `**ClassPathXmlApplicationContext("application.xml")**`

###### 整合

1. spring-mybatis整合的配置
2. 在spring配置中引入spring-mybatis配置
3. 数据源等配置在spring-mybatis中
4. 使用容器创建sqlsession
5. 业务层封装sqlsession来实现业务操作

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <!--标准输出日志-->
<settings>
    <setting name="logImpl" value="STDOUT_LOGGING"/>
</settings>
<!--    这个标签可以把实体类的名称作为类型别名  方便在映射中使用-->
<typeAliases>
    <package name="com.manager.pojo"/>
</typeAliases>

</configuration>
```

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">
<!--导入整合mybatis的配置-->
<import resource="spring-mybatis.xml"/>
 <!--注解扫描-->   
 <context:annotation-config/>
</beans>
```

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--配置数据源：数据源有非常多，可以使用第三方的，也可使使用Spring的-->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/homework?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8" />
        <property name="username" value="root"/>
        <property name="password" value="123"/>
    </bean>

  <!--不在通过sqlsessionfactorybuild生成sqlsessionFactory,通过sqlsesssionfactorybean-->
  <!--关联MyBatis-->
    <bean id="sessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <!--关联Mybatis-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <property name="mapperLocations" value="classpath*:com/manager/dao/*.xml"/>
    </bean>
<!--*-->

    <!-- 使用线程更安全的SqlSessionTemplate,替代sqlsession,通过构造器注入-->
    <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
        <constructor-arg index="0" ref="sessionFactory" />
    </bean>

<!-- 业务层bean,可以通过xml装配,也可以通过注解装配,这里通过注解装配-->
    <bean id="deptService" class="com.manager.service.deptServiceImpl">
    </bean>
    <bean id="empService" class="com.manager.service.empServiceImpl">
    </bean>
</beans>
```

```
// dept
public interface deptService {
    List<dept> deptList();


    dept getDept(int id);
}

// emp
public interface empService {
    List<emp> empList();


    emp getEmp(int id);
}
```

```
// dept
public class deptServiceImpl implements deptService{

    SqlSession sqlSession;

    public SqlSession getSqlSession() {
        return sqlSession;
    }

// 自动装配sqlsession
    @Autowired
    @Qualifier("sqlSession")
    public void setSqlSession(SqlSession sqlSession) {
        this.sqlSession = sqlSession;
    }

    @Override
    public List<dept> deptList() {
        deptMapper mapper = sqlSession.getMapper(deptMapper.class);
        return mapper.deptList();
    }

    @Override
    public dept getDept(int id) {
        deptMapper mapper = sqlSession.getMapper(deptMapper.class);
        return mapper.getDept(id);
    }
}
// emp
public class empServiceImpl implements empService{

    SqlSession sqlSession;

    public SqlSession getSqlSession() {
        return sqlSession;
    }
// 自动装配sqlsession
    @Autowired
    @Qualifier("sqlSession")
    public void setSqlSession(SqlSession sqlSession) {
        this.sqlSession = sqlSession;
    }

    @Override
    public List<emp> empList() {
        empMapper mapper = sqlSession.getMapper(empMapper.class);
        return mapper.empList();


    }

    @Override
    public emp getEmp(int id) {
        empMapper mapper = sqlSession.getMapper(empMapper.class);
        return mapper.getEmp(id);
    }
}
```

```
@org.junit.Test
    public void Test1(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("application.xml");
        empService bean = applicationContext.getBean("empService", empService.class);
        List<emp> list = bean.empList();
        list.forEach(System.out::println);

        deptService bean1 = applicationContext.getBean("deptService", deptService.class);
        List<dept> list1 = bean1.deptList();
        list1.forEach(System.out::println);
    }
```

结果成功输出！整合成功!

![](学习笔记/Attachments/1656408547012-afed4175-ae6d-468a-a495-845c45aca670.png "项目结构")

  

###### spring管理mybatis的事务

事务的的属性:原子性,持久性,一致性,隔离性

接口deptMapper增加

```
    int deleteDept(int id);
    int insertDept(dept d1);
```

```
    <delete id="deleteDept">
        deletes from dept where deptno=#{id};
    </delete>
    <insert id="insertDept">
        insert into dept values (#{deptno},#{dname},#{loc});
    </insert>
```

其中delete故意写错

业务层调用实现接口,并且添加一个增加用户删除用户的业务test()

```
 @Override
    public int deleteDept(int id) {
        deptMapper mapper = sqlSession.getMapper(deptMapper.class);

        return mapper.deleteDept(id);
    }

    @Override
    public int insertDept(dept d1) {
        deptMapper mapper = sqlSession.getMapper(deptMapper.class);

        return mapper.insertDept(d1);
    }
        @Override
    public void test(int id, dept d1) {
        deptMapper mapper = sqlSession.getMapper(deptMapper.class);
        mapper.insertDept(d1);
        mapper.deleteDept(id);
        System.out.println("succeed");
        System.out.println("succeed");
        System.out.println("succeed");
        System.out.println("succeed");
        System.out.println("succeed");
        System.out.println("succeed");
    }
```

测试

```
  @org.junit.Test
    public void Test2(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("application.xml");
        deptService bean1 = applicationContext.getBean("deptService", deptService.class);
        bean1.test(1,new dept(100,"麦浪","太原"));
    }
```

结果报错,但是发现用户添加进入数据库中,我们希望数据都成功才成功,有一个失败,就都失败,我们就应该需要事务

在配置中添加事务管理器

```
  <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource" />
    </bean>

    <!--配置事务通知-->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <tx:attributes>
            <!--配置哪些方法使用什么样的事务,配置事务的传播特性-->
            <tx:method name="add" propagation="REQUIRED"/>
            <tx:method name="delete" propagation="REQUIRED"/>
            <tx:method name="update" propagation="REQUIRED"/>
            <tx:method name="search*" propagation="REQUIRED"/>
            <tx:method name="get" read-only="true"/>
            <tx:method name="*" propagation="REQUIRED"/>
        </tx:attributes>
    </tx:advice>
    <!--配置aop织入事务-->
    <aop:config>
        <aop:pointcut id="txPointcut" expression="execution(* com.manager.service.*.*(..))"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointcut"/>
    </aop:config>
```

### 3.spring mvc

### SpringMVC

#### 什么是mvc

M : model (模型)实现系统的业务逻辑 javabean 负责

V : view (视图)负责与用户交互，即在界面上展示数据对象给用户 jsp负责

C : Controller (控制)Model与View之间沟通的桥梁 servlet负责

使用MVC的目的是将M和V的实现[代码](https://baike.baidu.com/item/%E4%BB%A3%E7%A0%81/86048)分离，从而使同一个程序可以使用不同的表现形式。其中，View的定义比较清晰，就是用户界面。它们各自处理自己的任务。最典型的MVC就是JSP + servlet + javabean的模式。

![](学习笔记/Attachments/1656486508505-ee36a959-0ec9-437b-964c-c22339494e8a.webp "MVC")

![](学习笔记/Attachments/1656487062951-242690d5-cfb5-4936-a667-eff9c469137c.png)

1. 用户发请求
2. Servlet接收请求数据，并调用对应的业务逻辑方法
3. 业务处理完毕，返回更新后的数据给servlet
4. servlet转向到JSP，由JSP来渲染页面
5. 响应给前端更新后的页面

#### Javaweb servlet

##### 新建maven项目

###### 导入依赖

```
<dependencies>
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.3.20</version>
  </dependency>
  <dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.28</version>
  </dependency>
  <dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.13.2</version>
    <scope>test</scope>
  </dependency>
  <dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.9</version>
  </dependency>
  <dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
  </dependency>
  <dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.9.1</version>
  </dependency>
  
  <dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.24</version>
  </dependency>
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.3.20</version>
  </dependency>
  <dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>2.0.7</version>
  </dependency>
  
</dependencies>
<build>
  <resources>
    <resource>
      <directory>src/main/resources</directory>
      <includes>
        <include>**/*.properties</include>
        <include>**/*.xml</include>
      </includes>
      <filtering>true</filtering>
    </resource>
    <resource>
      <directory>src/main/java</directory>
      <includes>
        <include>**/*.properties</include>
        <include>**/*.xml</include>
      </includes>
      <filtering>true</filtering>
    </resource>
  </resources>
    </build>
```

导入servlet,jsp依赖

```
 <dependency>
       <groupId>javax.servlet</groupId>
       <artifactId>servlet-api</artifactId>
       <version>2.5</version>
   </dependency>
   <dependency>
       <groupId>javax.servlet.jsp</groupId>
       <artifactId>jsp-api</artifactId>
       <version>2.2</version>
   </dependency>
   <dependency>
       <groupId>javax.servlet</groupId>
       <artifactId>jstl</artifactId>
       <version>1.2</version>
   </dependency>
```

##### 添加web框架支持

![](学习笔记/Attachments/1656475246356-cf2192fe-8598-4596-88cd-754fd3ca1293.png "模块右键添加框架")

添加一个servlet,hello

1. 新建hello类继承httpServlet重写get和post方法
2. 在web.xml中注册servlet

```
 @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        PrintWriter writer = resp.getWriter();
        System.out.println("----------------------------");
        System.out.println("----------------------------");
        System.out.println("----------------------------");
        System.out.println("----------------------------");
        writer.println("---------------------------------");
        writer.println("---------------------------------");
        writer.println("---------------------------------");
        writer.println("---------------------------------");
        writer.println("---------------------------------");
        req.getRequestDispatcher("/WEB-INF/jsp/hello.jsp").forward(req,resp);
        //System.out.println(req.getContextPath());
        //resp.sendRedirect(req.getContextPath()+"/templates/home.html");//重定向
        //resp.sendRedirect(req.getContextPath()+"/jsp/hello.jsp");//重定向  
    }
```

```
    <servlet>
        <servlet-name>hello</servlet-name>
        <servlet-class>com.manager.servlet.hello</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>hello</servlet-name>
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>
```

在web.xml目录中新建hello.jsp

配置tomcat并运行

浏览器运行[http://127.0.0.1:8080/hello](http://127.0.0.1:8080/hello)跳转到我们写好的hello视图中

一个请求要跳转到指定路径先经过web.xml,在其中找到地址所对应的servlet然后找到所对应的servlet,servlet对网页进行重定向或者转发

#### SpringMVC

  

![](26185156_co9w.jpg)

MVC用于将web（UI）层进行职责解耦

##### 配置springmvc

##### 通过xml配置版

首先创建普通的maven项目添加web.xml的框架支持,

创建目录lib添加jar到其中.在web.xml中注册DispatcherServlet的servlet并且配置参数和启动级别,它是spring提供给我们的.

```
<!--1.注册DispatcherServlet-->
<servlet>
    <servlet-name>springmvc</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <!--关联一个springmvc的配置文件:【servlet-name】-servlet.xml-->
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:springmvc-servlet.xml</param-value>
    </init-param>
    <!--启动级别-1-->
    <load-on-startup>1</load-on-startup>
</servlet>
            
    <!--/ 匹配所有的请求；（不包括.jsp）-->
    <!--/* 匹配所有的请求；（包括.jsp）-->
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
```

  

创建名为springmvc-servlet.xml的springmvc的上下文注册

SimpleControllerHandlerAdapter控制层适配器

BeanNameUrlHandlerMapping处理servlet映射

InternalResourceViewResolver视图解析器处理dipatcherservlet传递过来的modelandview

```
<bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
<bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
<!--视图解析器:DispatcherServlet给他的ModelAndView-->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
    <!--前缀-->
    <property name="prefix" value="/WEB-INF/jsp/"/>
    <!--后缀-->
    <property name="suffix" value=".jsp"/>
</bean>
```

控制器hello

```
public class helloController implements Controller {
    @Override
    public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {

        //ModelAndView 模型和视图
        ModelAndView mv = new ModelAndView();

        //封装对象，放在ModelAndView中。Model
        mv.addObject("msg","HelloSpringMVC!");
        //封装要跳转的视图，放在ModelAndView中
        mv.setViewName("hello"); //: /WEB-INF/jsp/hello.jsp
        return mv;
    }
}
```

注册控制层bean

```
<bean id="/hello" class="com.manager.controller.helloController"/>
```

地址栏输入/hello然后找到控制层

经过控制层处理返回要跳转的视图

- 实现接口Controller定义控制器是较老的办法
- 缺点是：一个控制器中只有一个方法，如果要多个方法则需要定义多个Controller；定义的方式比较麻烦；

###### 通过注解配置

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xmlns:context="http://www.springframework.org/schema/context"
      xmlns:mvc="http://www.springframework.org/schema/mvc"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/mvc
       https://www.springframework.org/schema/mvc/spring-mvc.xsd">
   <!-- 自动扫描包，让指定包下的注解生效,由IOC容器统一管理 -->
   <context:component-scan base-package="com.manager.controller"/>
   <!-- 让Spring MVC不处理静态资源 -->
   <mvc:default-servlet-handler />
   <!--
   支持mvc注解驱动
       在spring中一般采用@RequestMapping注解来完成映射关系
       要想使@RequestMapping注解生效
       必须向上下文中注册DefaultAnnotationHandlerMapping
       和一个AnnotationMethodHandlerAdapter实例
       这两个实例分别在类级别和方法级别处理。
       而annotation-driven配置帮助我们自动完成上述两个实例的注入。
    -->
   <mvc:annotation-driven />
   <!-- 视图解析器 -->
   <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
         id="internalResourceViewResolver">
       <!-- 前缀 -->
       <property name="prefix" value="/WEB-INF/jsp/" />
       <!-- 后缀 -->
       <property name="suffix" value=".jsp" />
   </bean>
</beans>
```

```
@Controller
@RequestMapping("/HelloController")
public class HelloController {

   //真实访问地址 : 项目名/HelloController/hello
   @RequestMapping("/hello")
    //这个hello为url中的地址,为控制器
   public String sayHello(Model model){
       //向模型中添加属性msg与值，可以在JSP页面中取出并渲染
       model.addAttribute("msg","hello,SpringMVC");
       //web-inf/jsp/hello.jsp
       return "hello";
       //返回的这个字符串为要跳转的视图
  }
}
```

在项目结构-工件中添加lib

运行tomcat测试成功[http://127.0.0.1:8080/hell0/h1](http://127.0.0.1:8080/hello/h1)进入视图

SpringMVC的原理如下图所示：

当发起请求时被前置的控制器拦截到请求，根据请求参数生成代理请求，找到请求对应的实际控制器，控制器处理请求，创建数据模型，访问数据库，将模型响应给中心控制器，控制器使用模型与视图渲染视图结果，将结果返回给中心控制器，再将结果返回给请求者。![](学习笔记/Attachments/640.png)

  

### SpringMVC执行原理

  

![](学习笔记/Attachments/640%201.png)

  

**简要分析执行流程**

  

1. DispatcherServlet表示前置控制器，是整个SpringMVC的控制中心。用户发出请求，DispatcherServlet接收请求并拦截请求。  
    我们假设请求的url为 : [http://localhost:8080/SpringMVC/hello](http://localhost:8080/SpringMVC/hello)  
    **如上url拆分成三部分：**  
    [http://localhost:8080](http://localhost:8080/)服务器域名  
    SpringMVC部署在服务器上的web站点  
    hello表示控制器在springmvc-servlet中注册

```
<!--Handler-->
<bean id="/hello" class="com.manager.controller.helloController"/>
```

  
通过分析，如上url表示为：请求位于服务器localhost:8080上的SpringMVC站点的hello控制器。

2. HandlerMapping为处理器映射。DispatcherServlet调用HandlerMapping,HandlerMapping根据请求url查找Handler。

```
<bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
<!-- 根据url查找处理器,比如从上面的连接中查找到需要的控制器hello-->
```

3. HandlerExecution表示具体的Handler,其主要作用是根据url查找控制器，如上url被查找控制器为：hello。
4. HandlerExecution将解析后的信息传递给DispatcherServlet,如解析控制器映射等。
5. HandlerAdapter表示处理器适配器，其按照特定的规则去执行Handler。
6. Handler让具体的Controller执行。

```
//实现接口controller处理业务及页面跳转重定向
public class helloController implements Controller {
    @Override
    public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
        ModelAndView mv = new ModelAndView();

        //封装对象，放在ModelAndView中。Model
        mv.addObject("msg","HelloSpringMVC!");
        //封装要跳转的视图，放在ModelAndView中
        mv.setViewName("hello"); //: /WEB-INF/jsp/hello.jsp
        return mv;
    }
}
```

7. Controller将具体的执行信息返回给HandlerAdapter,如ModelAndView。
8. HandlerAdapter将视图逻辑名或模型传递给DispatcherServlet。
9. DispatcherServlet调用视图解析器(ViewResolver)来解析HandlerAdapter传递的逻辑视图名。

```
    <!--视图解析器:DispatcherServlet给他的ModelAndView-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
        <!--前缀-->
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <!--后缀-->
        <property name="suffix" value=".jsp"/>
    </bean>
```

10. 视图解析器将解析的逻辑视图名传给DispatcherServlet。
11. DispatcherServlet根据视图解析器解析的视图结果，调用具体的视图。
12. 最终视图呈现给用户。

在地址栏后输入/hello或者其他网页跳转到hello就会通过控制器进行一系列操作

@RequestMapping是为了映射请求路径，这里因为类与方法上都有映射所以访问时应该是/HelloController/hello；  
方法中声明Model类型的参数是为了把Action中的数据带到视图中；  
方法返回的结果是视图的名称hello，加上配置文件中的前后缀变成WEB-INF/jsp/hello.jsp。

#### 使用springMVC必须配置的三大件：

处理器映射器、处理器适配器、视图解析器  
通常，我们只需要手动配置视图解析器，而处理器映射器和处理器适配器只需要开启注解驱动即可，而省去了大段的xml配置

#### 前后端数据的处理

[http://localhost:8080/hello?name=hello1](http://localhost:8080/hello?name=hello1)

从链接中得到参数

```
@RequestMapping("/hello")
public String hello(String name){
   System.out.println(name);
   return "hello";
}
```

  

输出hello1

  

[http://localhost:8080/hello?username=hello1](http://localhost:8080/hello?username=hello1)

  

如果参数名与链接中的参数名不同

  

```
@RequestMapping("/hello")
public String hello(@RequestParam("username") String name){
   System.out.println(name);
   return "hello";
}
```

  

输出hello1

  

[http://localhost:8080/mvc04/user?name=hello&id=1&age=15](http://localhost:8080/mvc04/user?name=hello&id=1&age=15)

  

提交的额为对象时候名字需要和类的属性名称相同

  

```
@RequestMapping("/user")
public String user(User user){
   System.out.println(user);
   return "hello";
}
```

  

#### 通过ModelAndView

  

```
public class ControllerTest1 implements Controller {

   public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
       //返回一个模型视图对象
       ModelAndView mv = new ModelAndView();
       mv.addObject("msg","ControllerTest1");
       mv.setViewName("test");
       return mv;
  }
}
```

  

#### 通过ModelMap

  

```
@RequestMapping("/hello")
public String hello(@RequestParam("username") String name, ModelMap model){
   //封装要显示到视图中的数据
   //相当于req.setAttribute("name",name);
   model.addAttribute("name",name);
   System.out.println(name);
   return "hello";
}
```

  

通过Model

  

```
@RequestMapping("/ct2/hello")
public String hello(@RequestParam("username") String name, Model model){
   //封装要显示到视图中的数据
   //相当于req.setAttribute("name",name);
   model.addAttribute("msg",name);
   System.out.println(name);
   return "test";
}
```

  

#### 解决网页乱码问题

  

在javaweb的时候通过添加一层过滤器来给每个请求响应设置统一编码

  

在SpringMVC中提供了一个过滤器

  

```
<filter>
   <filter-name>encoding</filter-name>
   <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
   <init-param>
       <param-name>encoding</param-name>
       <param-value>utf-8</param-value>
   </init-param>
</filter>
<filter-mapping>
   <filter-name>encoding</filter-name>
   <url-pattern>/*</url-pattern>
</filter-mapping>
```

  

有时候可能支持不是那么好

  

修改tamcat的配置

  

```
<Connector URIEncoding="utf-8" port="8080" protocol="HTTP/1.1"
          connectionTimeout="20000"
          redirectPort="8443" />
```

  

也可以添加自己的过滤器

  

和javaweb的时候一样添加过滤器类实现filter的接口实现方法doFilter

  

```
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws ServletException, IOException {
        request.setCharacterEncoding("utf-8");
        response.setCharacterEncoding("utf-8");
        response.setContentType("text/html;charset=UTF-8");
        chain.doFilter(request, response);
    }
```

  

最后在web.xml中配置

  

```

 <filter>
        <filter-name>GenericEncodingFilter</filter-name>
        <filter-class>com.manager.filter.GenericEncodingFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>GenericEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

  

## SSM的整合

### 创建空的maven项目

### 添加web框架

### 添加maven依赖

### idea连接数据库

### 编写实体类

### 编写dao接口

### 实现dao接口

### 编写service层接口

### 实现service层接口

### SSM整合的配置

### servlet添加,过滤器的添加

### jsp网页设计

### controller层的编写

### tomcat部署运行

### 使用json在前后端之间传递消息

导入依赖

```
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core -->
<dependency>
   <groupId>com.fasterxml.jackson.core</groupId>
   <artifactId>jackson-databind</artifactId>
   <version>2.9.8</version>
</dependency>
```

```
 @RequestMapping(value = "/h2",produces = "application/json;charset=utf-8")
    @ResponseBody
    public String s1() throws JsonProcessingException {
        ObjectMapper mapper = new ObjectMapper();
        return mapper.writeValueAsString(user1);
    }
```

通过`ObjectMapper`类来将对象转为json字符串

乱码问题的解决

```
<mvc:annotation-driven>
   <mvc:message-converters register-defaults="true">
       <bean class="org.springframework.http.converter.StringHttpMessageConverter">
           <constructor-arg value="UTF-8"/>
       </bean>
       <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
           <property name="objectMapper">
               <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                   <property name="failOnEmptyBeans" value="false"/>
               </bean>
           </property>
       </bean>
   </mvc:message-converters>
</mvc:annotation-driven>
```

**返回json字符串统一解决**

在类上直接使用 @RestController ，这样子，里面所有的方法都只会返回 json 字符串了，不用再每一个都添加@ResponseBody ！我们在前后端分离开发中，一般都使用 @RestController ，十分便捷！

  

## 验证码实现

导入依赖**kaptcha**

```
     <!--   验证码图片生成     -->
        <dependency>
            <groupId>com.github.penggle</groupId>
            <artifactId>kaptcha</artifactId>
            <version>2.3.2</version>
        </dependency>
```

#### 生成随机字母数字

方法一 : 使用随机数生成(26+10)范围的数字,前26的数字为字符a+随机数,后10个数为随机数-26

方法二 : 生成随机数后将其装换为36进制这样用36进制表示的数字既有字符又有数字

#### 设置响应类型

```
//设置页面不缓存
response.setHeader("Pragma", "no-cache");
response.setHeader("Cache-Control", "no-cache");
response.setDateHeader("Expires", 0);
//响应的内容类型
response.setContentType("image/jpeg");
```

#### 生成验证码图片

```
// verifyCode 随机字符  
BufferedImage image = Kaptcha.createImage(verifyCode);
// 通过ImageIO将内容写到响应输出流
ImageIO.write(image, "JPEG", response.getOutputStream());
```

## 拦截器

### 给予不同的用户不同的网页访问权限

定义拦截器实现 **HandlerInterceptor**的接口,接口有以下三个方法:

1. preHandle 拦截器处理前
2. postHandle 拦截器处理后
3. afterCompletion 清理

```
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        // 如果是登陆页面则放行
        System.out.println("uri: " + request.getRequestURI());
        if (!request.getRequestURI().contains("homePage")) {
            return true;
        }

        HttpSession session = request.getSession();

        // 如果用户已登陆也放行
        if(session.getAttribute("user") != null) {
            return true;
        }

        // 用户没有登陆跳转到登陆页面
        request.getRequestDispatcher("/WEB-INF/jsp/templates/home.jsp").forward(request, response);
        return false;
    }
```

配置拦截器的设置

```
<!--    拦截器-->
    <!--关于拦截器的配置-->
    <mvc:interceptors>
        <mvc:interceptor>
            <!--/** 包括路径及其子路径-->
            <!--/admin/* 拦截的是/admin/add等等这种 , /admin/add/user不会被拦截-->
            <!--/admin/** 拦截的是/admin/下的所有-->
            <mvc:mapping path="/home/**"/>
            <!--bean配置的就是拦截器-->
            <bean class="com.manager.filter.MyIntercept"/>
        </mvc:interceptor>
    </mvc:interceptors>
```

  

## SpringBoot

### 1. vueAdmin后台管理

#### 创建项目

#### 导入依赖

##### 1.2.1开发依赖

```
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
  		<!--devtools：项目的热加载重启插件-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
			<!--lombok：简化代码的工具-->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
```

##### 整合数据库

```

        <!--整合mybatis plus https://baomidou.com/-->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.4.1</version>
        </dependency><!--mp代码生成器-->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-generator</artifactId>
            <version>3.4.1</version>
        </dependency>
        <dependency>
            <groupId>org.freemarker</groupId>
            <artifactId>freemarker</artifactId>
            <version>2.3.30</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
```

  

### web容器

先排除tomcat,然后添加新的web容器

```
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
  <exclusions>
    <exclusion>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-tomcat</artifactId>
    </exclusion>
  </exclusions>
</dependency>
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-undertow</artifactId>
</dependency>
```

通过spring自动配置的jar包可以看到web容器有

![](学习笔记/Attachments/1688908846788-8a4824e8-ed00-4af5-96f5-729c3535c2c7.png)

当有相关依赖的时候就会自动创建相关的bean

### 登录流程

#### 1.4.1验证码的生成

##### 大概流程

1. 用户进入登录页面时,页面向服务器请求验证码图片
2. 后台收到请求
3. 随机生成验证码和使用uuid或者其它工具生成token
4. token与验证码分别作为key value保存到redis中等待后续验证时候使用
5. 生成验证码图片将图片编码后连同token发送给用户
6. 用户携带用户名密码验证码以及token到服务器
7. 服务器首先验证验证码的正确性
8. 通过token在redis中拿到正确的验证码,对比两个验证码
9. 之后就是验证用户名以及密码
10. 需要注意的是redis中的验证码应该是有TTL(标识数据包生命周期的字段)的,以及验证一次无论对错应该删掉保存的验证码更新新的

##### 思考

1. 如果用户输入的验证码错误，服务器应该返回一个新的Token和验证码，而不是旧的Token和新的验证码。

这是因为，如果服务器返回旧的Token和新的验证码，用户在输入新的验证码时可能会出现相同的错误，因为旧的Token可能已经过期或者被盗用。为了确保用户的安全和身份验证的准确性，服务器应该生成一个新的Token，并返回给用户，同时提供一个新的验证码，以便用户再次进行身份验证。

另外，服务器应该对连续的验证码错误进行计数，并在一定次数内限制用户的登录请求，以防止暴力破解和恶意攻击。

如果用户输入的验证码错误，服务器应该返回一个新的Token和验证码，而不是旧的Token和新的验证码。

这是因为，如果服务器返回旧的Token和新的验证码，用户在输入新的验证码时可能会出现相同的错误，因为旧的Token可能已经过期或者被盗用。为了确保用户的安全和身份验证的准确性，服务器应该生成一个新的Token，并返回给用户，同时提供一个新的验证码，以便用户再次进行身份验证。

另外，服务器应该对连续的验证码错误进行计数，并在一定次数内限制用户的登录请求，以防止暴力破解和恶意攻击。

##### 导入kaptcha jar包

```
<!--#验证码-->
<dependency>
  <groupId>com.github.penggle</groupId>
  <artifactId>kaptcha</artifactId>
  <version>2.3.2</version>
</dependency>
```

##### 配置kaptcha

配置生成的图片样式等

```
@Configuration
public class KaptchaConfig {
    @Bean
    public DefaultKaptcha getKaptchaBean()
    {
        DefaultKaptcha defaultKaptcha = new DefaultKaptcha();
        Properties properties = new Properties();
        // 是否有边框 默认为true 我们可以自己设置yes，no
        properties.setProperty(KAPTCHA_BORDER, "yes");
        // 验证码文本字符颜色 默认为Color.BLACK
        properties.setProperty(KAPTCHA_TEXTPRODUCER_FONT_COLOR, "black");
        // 验证码图片宽度 默认为200
        properties.setProperty(KAPTCHA_IMAGE_WIDTH, "160");
        // 验证码图片高度 默认为50
        properties.setProperty(KAPTCHA_IMAGE_HEIGHT, "60");
        // 验证码文本字符大小 默认为40
        properties.setProperty(KAPTCHA_TEXTPRODUCER_FONT_SIZE, "38");
        // KAPTCHA_SESSION_KEY
        properties.setProperty(KAPTCHA_SESSION_CONFIG_KEY, "kaptchaCode");
        // 验证码文本字符长度 默认为5
        properties.setProperty(KAPTCHA_TEXTPRODUCER_CHAR_LENGTH, "4");
        // 验证码文本字体样式 默认为new Font("Arial", 1, fontSize), new Font("Courier", 1, fontSize)
        properties.setProperty(KAPTCHA_TEXTPRODUCER_FONT_NAMES, "Arial,Courier");
        // 图片样式 水纹com.google.code.kaptcha.impl.WaterRipple 鱼眼com.google.code.kaptcha.impl.FishEyeGimpy 阴影com.google.code.kaptcha.impl.ShadowGimpy
        properties.setProperty(KAPTCHA_OBSCURIFICATOR_IMPL, "com.google.code.kaptcha.impl.ShadowGimpy");
        Config config = new Config(properties);
        defaultKaptcha.setConfig(config);
        return defaultKaptcha;
    }
}
```

##### 生成验证码并写为base64格式

```
@RestController
public class CaptchaController
{
    @Resource(name = "captchaProducer")
    private Producer captchaProducer;

 
  
  	@Autowired
    RedisUtil redisUtil;
    /**
     * 生成验证码
     */
    @GetMapping("/captchaImage")
    public AjaxResult getCode(HttpServletResponse response) throws IOException
    {
        BufferedImage image = null;
        String capStr = null;
        // 生成的验证码
        capStr = defaultKaptcha.createText();
        // 生成图片
        image = defaultKaptcha.createImage(capStr);
        // 生成token        
        String uuid = UUID.randomUUID().toString();
        String key = Const.KAPTCHA_CODE + uuid;
        // 存入redis
        redisUtil.set(key, capStr,120);
        FastByteArrayOutputStream outputStream = new FastByteArrayOutputStream();
        try{
            ImageIO.write(image, "jpg", outputStream);
        }catch (Exception e){
        }
        Base64.Encoder encoder = Base64.getEncoder();
        
        HashMap<String, String> map = new HashMap<>();
        map.put("token",uuid);
 	    map.put("imgStr",  "data:image/jpeg;base64,"+encoder.encodeToString(outputStream.toByteArray()));
        return map;     
    }
}
```

#### 验证用户信息

```
@RestController
public class SysLoginController
{
    @Autowired
    private SysLoginService loginService;
 

    /**
     * 登录方法
     * 
     * @param loginBody 登录信息
     * @return 结果
     */
    @PostMapping("/login")
    public AjaxResult login(@RequestBody LoginBody loginBody)
    {
        AjaxResult ajax = AjaxResult.success();
        
        // 验证 生成令牌
        String token = loginService.login(loginBody.getUsername(), loginBody.getPassword(), loginBody.getCode(),
                loginBody.getUuid());
        ajax.put(Constants.TOKEN, token);
        
        return ajax;
    }
}
```

```
@Component
public class SysLoginService
{
    @Autowired
    private TokenService tokenService;

    @Resource
    private AuthenticationManager authenticationManager;

    @Autowired
    private RedisCache redisCache;
    
    @Autowired
    private ISysUserService userService;

    @Autowired
    private ISysConfigService configService;

    /**
     * 登录验证
     * 
     * @param username 用户名
     * @param password 密码
     * @param code 验证码
     * @param uuid 唯一标识
     * @return 结果
     */
    public String login(String username, String password, String code, String uuid)
    {
        // 验证码校验
        validateCaptcha(username, code, uuid);
        // 登录前置校验
        loginPreCheck(username, password);
        // 用户验证
        Authentication authentication = null;
        try
        {
            UsernamePasswordAuthenticationToken authenticationToken = new UsernamePasswordAuthenticationToken(username, password);
            AuthenticationContextHolder.setContext(authenticationToken);
            // 该方法会去调用UserDetailsServiceImpl.loadUserByUsername
            authentication = authenticationManager.authenticate(authenticationToken);
        }
        catch (Exception e)
        {
            if (e instanceof BadCredentialsException)
            {
                AsyncManager.me().execute(AsyncFactory.recordLogininfor(username, Constants.LOGIN_FAIL, MessageUtils.message("user.password.not.match")));
                throw new UserPasswordNotMatchException();
            }
            else
            {
                AsyncManager.me().execute(AsyncFactory.recordLogininfor(username, Constants.LOGIN_FAIL, e.getMessage()));
                throw new ServiceException(e.getMessage());
            }
        }
        finally
        {
            AuthenticationContextHolder.clearContext();
        }
        AsyncManager.me().execute(AsyncFactory.recordLogininfor(username, Constants.LOGIN_SUCCESS, MessageUtils.message("user.login.success")));
        LoginUser loginUser = (LoginUser) authentication.getPrincipal();
        recordLoginInfo(loginUser.getUserId());
        // 生成token
        return tokenService.createToken(loginUser);
    }
}
```

```

    /**
     * 校验验证码
     * 
     * @param username 用户名
     * @param code 验证码
     * @param uuid 唯一标识
     * @return 结果
     */
    public void validateCaptcha(String username, String code, String uuid)
    {
        // 是否开启验证码验证
        boolean captchaEnabled = configService.selectCaptchaEnabled();
        if (captchaEnabled)
        {
            String verifyKey = CacheConstants.CAPTCHA_CODE_KEY + StringUtils.nvl(uuid, "");
            // 从redis中拿到正确的验证码
            String captcha = redisCache.getCacheObject(verifyKey);
            redisCache.deleteObject(verifyKey);
            if (captcha == null)
            {
                AsyncManager.me().execute(AsyncFactory.recordLogininfor(username, Constants.LOGIN_FAIL, MessageUtils.message("user.jcaptcha.expire")));
                throw new CaptchaExpireException();
            }
            if (!code.equalsIgnoreCase(captcha))
            {
                AsyncManager.me().execute(AsyncFactory.recordLogininfor(username, Constants.LOGIN_FAIL, MessageUtils.message("user.jcaptcha.error")));
                throw new CaptchaException();
            }
        }
    }

   
```

```
 /**
     * 登录前置校验
     * @param username 用户名
     * @param password 用户密码
     */
    public void loginPreCheck(String username, String password)
    {
        // 用户名或密码为空 错误
        if (StringUtils.isEmpty(username) || StringUtils.isEmpty(password))
        {
            AsyncManager.me().execute(AsyncFactory.recordLogininfor(username, Constants.LOGIN_FAIL, MessageUtils.message("not.null")));
            throw new UserNotExistsException();
        }
        // 密码如果不在指定范围内 错误
        if (password.length() < UserConstants.PASSWORD_MIN_LENGTH
                || password.length() > UserConstants.PASSWORD_MAX_LENGTH)
        {
            AsyncManager.me().execute(AsyncFactory.recordLogininfor(username, Constants.LOGIN_FAIL, MessageUtils.message("user.password.not.match")));
            throw new UserPasswordNotMatchException();
        }
        // 用户名不在指定范围内 错误
        if (username.length() < UserConstants.USERNAME_MIN_LENGTH
                || username.length() > UserConstants.USERNAME_MAX_LENGTH)
        {
            AsyncManager.me().execute(AsyncFactory.recordLogininfor(username, Constants.LOGIN_FAIL, MessageUtils.message("user.password.not.match")));
            throw new UserPasswordNotMatchException();
        }
        // IP黑名单校验
        String blackStr = configService.selectConfigByKey("sys.login.blackIPList");
        if (IpUtils.isMatchedIp(blackStr, IpUtils.getIpAddr()))
        {
            AsyncManager.me().execute(AsyncFactory.recordLogininfor(username, Constants.LOGIN_FAIL, MessageUtils.message("login.blocked")));
            throw new BlackListException();
        }
    }
```

```
    /**
     * 记录登录信息
     *
     * @param userId 用户ID
     */
    public void recordLoginInfo(Long userId)
    {
        SysUser sysUser = new SysUser();
        sysUser.setUserId(userId);
        sysUser.setLoginIp(IpUtils.getIpAddr());
        sysUser.setLoginDate(DateUtils.getNowDate());
        userService.updateUserProfile(sysUser);
    }
```

  

```
    public String createToken(LoginUser loginUser)
    {
        // 创建token
        String token = IdUtils.fastUUID();
		// 完善用户信息token等
        loginUser.setToken(token);
        setUserAgent(loginUser);
    	// 将其保存到redis并设置有效时间
        refreshToken(loginUser);
    	// 根据tokem生成令牌发送给前端
        Map<String, Object> claims = new HashMap<>();
        claims.put(Constants.LOGIN_USER_KEY, token);
        return createToken(claims);
    }
    public void refreshToken(LoginUser loginUser)
    {
        // 当前时间
        loginUser.setLoginTime(System.currentTimeMillis());
        // 过期时间
        loginUser.setExpireTime(loginUser.getLoginTime() + expireTime * MILLIS_MINUTE);
        // 根据uuid将loginUser缓存
        String userKey = getTokenKey(loginUser.getToken());
        redisCache.setCacheObject(userKey, loginUser, expireTime, TimeUnit.MINUTES);
    }
```

##### 过滤器中验证用户信息

```
@Component
public class JwtAuthenticationTokenFilter extends OncePerRequestFilter
{
    @Autowired
    private TokenService tokenService;

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain chain)
            throws ServletException, IOException
    {
        
        LoginUser loginUser = tokenService.getLoginUser(request);
        if (StringUtils.isNotNull(loginUser) && StringUtils.isNull(SecurityUtils.getAuthentication()))
        {
            // 用户的每次操作相差不足20分钟的话会刷新时间
            tokenService.verifyToken(loginUser);
            // 应该是当前security content 中没有 Authentication的话 进行添加
            UsernamePasswordAuthenticationToken authenticationToken = new UsernamePasswordAuthenticationToken(loginUser, null, loginUser.getAuthorities());
            authenticationToken.setDetails(new WebAuthenticationDetailsSource().buildDetails(request));
            SecurityContextHolder.getContext().setAuthentication(authenticationToken);
        }
        chain.doFilter(request, response);
    }
}
    public LoginUser getLoginUser(HttpServletRequest request)
    {
        // 获取请求携带的令牌
        String token = getToken(request);
        if (StringUtils.isNotEmpty(token))
        {
            try
            {
                // 从令牌解析token
                Claims claims = parseToken(token);
                // 解析对应的权限以及用户信息
                String uuid = (String) claims.get(Constants.LOGIN_USER_KEY);
                String userKey = getTokenKey(uuid);
                // 拿到redis中用户数据
                LoginUser user = redisCache.getCacheObject(userKey);
                return user;
            }
            catch (Exception e)
            {
            }
        }
        return null;
    }
```

### 获取用户信息

#### 拿到用户信息

```
    /**
     * 获取用户信息
     * 
     * @return 用户信息
     */
    @GetMapping("getInfo")
    public AjaxResult getInfo()
    {
        SysUser user = SecurityUtils.getLoginUser().getUser();
        // 角色集合
        Set<String> roles = permissionService.getRolePermission(user);
        // 权限集合
        Set<String> permissions = permissionService.getMenuPermission(user);
        AjaxResult ajax = AjaxResult.success();
        ajax.put("user", user);
        
        ajax.put("roles", roles);
        // 管理员 ["*:*:*"]
        // 其他user 类似于 
        // system:user:list
		// system:role:list
		// system:menu:list

        ajax.put("permissions", permissions);
        return ajax;
    }
// SecurityUtils.getLoginUser() 从SecurityContext中拿到用户信息
   public static Authentication getAuthentication()
    {
        return SecurityContextHolder.getContext().getAuthentication();
    }
```

#### 拿到用户路由

```

    /**
     * 获取路由信息
     * 
     * @return 路由信息
     */
    @GetMapping("getRouters")
    public AjaxResult getRouters()
    {
        Long userId = SecurityUtils.getUserId();
        List<SysMenu> menus = menuService.selectMenuTreeByUserId(userId);
        return AjaxResult.success(menuService.buildMenus(menus));
    }
```

```
select distinct m.menu_id,
                m.parent_id,
                m.menu_name,
                m.path,
                m.component,
                m.`query`,
                m.visible,
                m.status,
                ifnull(m.perms, '') as perms,
                m.is_frame,
                m.is_cache,
                m.menu_type,
                m.icon,
                m.order_num,
                m.create_time
from sys_menu m
         left join sys_role_menu rm on m.menu_id = rm.menu_id
         left join sys_user_role ur on rm.role_id = ur.role_id
         left join sys_role ro on ur.role_id = ro.role_id
         left join sys_user u on ur.user_id = u.user_id
where u.user_id = 2
  and m.menu_type in ('M', 'C')
  and m.status = 0
  AND ro.status = 0
order by m.parent_id, m.order_num
```

```
    getChildPerms(menus, 0);

/**
     * 根据父节点的ID获取所有子节点
     * 
     * @param list 分类表
     * @param parentId 传入的父节点ID
     * @return String
     */
    public List<SysMenu> getChildPerms(List<SysMenu> list, int parentId)
    {
        List<SysMenu> returnList = new ArrayList<SysMenu>();
        for (Iterator<SysMenu> iterator = list.iterator(); iterator.hasNext();)
        {
            SysMenu t = (SysMenu) iterator.next();
            // 一、根据传入的某个父节点ID,遍历该父节点的所有子节点
            if (t.getParentId() == parentId)
            {
                recursionFn(list, t);
                returnList.add(t);
            }
        }
        return returnList;
    }
```

### 拿到request的方法

### SpringSecurity

导入依赖

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

#### 架构

springSecurity的过滤器链

![](学习笔记/Attachments/1689603442800-cfc21950-8871-4657-a7e7-6282b033fe0a.png)

```
public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) {
	Filter delegate = getFilterBean(someBeanName); 
	delegate.doFilter(request, response); 
}
```

按照我的理解delegatingFilterProxy就是一个普通的servlet过滤器但是在这个过滤器中调用了springSecurity提供的过滤链 [SecurityFilterChain](https://docs.spring.io/spring-security/reference/servlet/architecture.html#servlet-securityfilterchain), Security Filters security 过滤器通过SecurityFilterChain中的api添加.

##### 过滤器调用顺序

- [ForceEagerSessionCreationFilter](https://docs.spring.io/spring-security/reference/servlet/authentication/session-management.html#session-mgmt-force-session-creation)
- ChannelProcessingFilter
- WebAsyncManagerIntegrationFilter
- SecurityContextPersistenceFilter
- HeaderWriterFilter
- CorsFilter
- CsrfFilter
- LogoutFilter
- OAuth2AuthorizationRequestRedirectFilter
- Saml2WebSsoAuthenticationRequestFilter
- X509AuthenticationFilter
- AbstractPreAuthenticatedProcessingFilter
- CasAuthenticationFilter
- OAuth2LoginAuthenticationFilter
- Saml2WebSsoAuthenticationFilter
- [UsernamePasswordAuthenticationFilter](https://docs.spring.io/spring-security/reference/servlet/authentication/passwords/form.html#servlet-authentication-usernamepasswordauthenticationfilter)
- DefaultLoginPageGeneratingFilter
- DefaultLogoutPageGeneratingFilter
- ConcurrentSessionFilter
- [DigestAuthenticationFilter](https://docs.spring.io/spring-security/reference/servlet/authentication/passwords/digest.html#servlet-authentication-digest)
- BearerTokenAuthenticationFilter
- [BasicAuthenticationFilter](https://docs.spring.io/spring-security/reference/servlet/authentication/passwords/basic.html#servlet-authentication-basic)
- [RequestCacheAwareFilter](https://docs.spring.io/spring-security/reference/servlet/architecture.html#requestcacheawarefilter)
- SecurityContextHolderAwareRequestFilter
- JaasApiIntegrationFilter
- RememberMeAuthenticationFilter
- AnonymousAuthenticationFilter
- OAuth2AuthorizationCodeGrantFilter
- SessionManagementFilter
- [ExceptionTranslationFilter](https://docs.spring.io/spring-security/reference/servlet/architecture.html#servlet-exceptiontranslationfilter)
- [AuthorizationFilter](https://docs.spring.io/spring-security/reference/servlet/authorization/authorize-http-requests.html)
- SwitchUserFilter

#### 认证

**认证:验证当前访问系统的是不是本系统的用户，并且要确认具体是哪个用户**

![](学习笔记/Attachments/1689592601995-10bc952c-7e57-4c1d-80a0-55f6ec6665e6.png)

本讨论对 [Servlet Security](https://springdoc.cn/spring-security/servlet/architecture.html#servlet-architecture) 进行了扩展。[架构图](https://springdoc.cn/spring-security/servlet/architecture.html#servlet-architecture) 阐述了 Spring Security 用于 Servlet 认证的主要架构组件。如果你需要具体的流程来解释这些部分是如何结合在一起的，请看 [认证机制](https://springdoc.cn/spring-security/servlet/authentication/index.html#servlet-authentication-mechanisms) 的具体章节。

- [SecurityContextHolder](https://springdoc.cn/spring-security/servlet/authentication/architecture.html#servlet-authentication-securitycontextholder) - SecurityContextHolder 是 Spring Security 存储 [认证](https://springdoc.cn/spring-security/features/authentication/index.html#authentication) 用户细节的地方。
- [SecurityContext](https://springdoc.cn/spring-security/servlet/authentication/architecture.html#servlet-authentication-securitycontext) - 是从 SecurityContextHolder 获得的，包含了当前认证用户的 Authentication （认证）。
- [Authentication](https://springdoc.cn/spring-security/servlet/authentication/architecture.html#servlet-authentication-authentication) - 可以是 AuthenticationManager 的输入，以提供用户提供的认证凭证或来自 SecurityContext 的当前用户。
- [GrantedAuthority](https://springdoc.cn/spring-security/servlet/authentication/architecture.html#servlet-authentication-granted-authority) - 在 Authentication （认证）中授予委托人的一种权限（即role、scope等）。
- [AuthenticationManager](https://springdoc.cn/spring-security/servlet/authentication/architecture.html#servlet-authentication-authenticationmanager) - 定义 Spring Security 的 Filter 如何执行 [认证](https://springdoc.cn/spring-security/features/authentication/index.html#authentication) 的API。
- [ProviderManager](https://springdoc.cn/spring-security/servlet/authentication/architecture.html#servlet-authentication-providermanager) - 最常见的 AuthenticationManager 的实现。
- [AuthenticationProvider](https://springdoc.cn/spring-security/servlet/authentication/architecture.html#servlet-authentication-authenticationprovider) - 由 ProviderManager 用于执行特定类型的认证。
- [用AuthenticationEntryPoint请求凭证](https://springdoc.cn/spring-security/servlet/authentication/architecture.html#servlet-authentication-authenticationentrypoint) - 用于从客户端请求凭证（即重定向到登录页面，发送 WWW-Authenticate 响应，等等）。
- [AbstractAuthenticationProcessingFilter](https://springdoc.cn/spring-security/servlet/authentication/architecture.html#servlet-authentication-abstractprocessingfilter)- 一个用于认证的基本 Filter。这也让我们很好地了解了认证的高层流程以及各部分是如何协作的。