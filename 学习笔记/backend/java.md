# 1. 安装

在linux上安装jdk

## 1.1. 解压

```
tar -xzvf jdk 
```

  

## 1.2. 配置环境变量

```
export $JAVA_HOME=/jdk/bin/
```

  

# 2. jvm内存模型

## 2.1.

# 3. 字符串

## 3.1. 字面量

```
String str="xxxxxx"
```

形如String s = "xxx";定义String的时候，xxx被我们称之为字面量，这种字面量在编译之后会以常量的形式进入到Class常量池

### 3.1.1. 编译期字符串长度

String的构造函数指定的长度是可以支持2147483647(2^31 - 1)

常量字符串过长

```
String s = "11111...1111";//其中有10万个字符"1"
```

字符串的长度是有要求的不是等于int的长度

```
private void checkStringConstant(DiagnosticPosition var1, Object var2) {

    if (this.nerrs == 0 && var2 != null && var2 instanceof String 
        && ((String)var2).length() >= 65535) {

        this.log.error(var1, "limit.string", new Object[0]);

        ++this.nerrs;

    }

}
```

代码中可以看出，当参数类型为String，并且长度大于等于65535的时候，就会导致编译失败。

这个地方大家可以尝试着debug一下javac的编译过程（视频中有对java的编译过程进行debug的方法），也可以发现这个地方会报错。

如果我们尝试以65534个字符定义字符串，则会发现可以正常编译。

## 3.2. 运行期限制

上面提到的这种String长度的限制是编译期的限制，也就是使用String s= “”;这种字面值方式定义的时候才会有的限制。

这里指的是在运行期通过+ ,等手段添加的字符串

运行期最大字符串长度不超过int表示的范围

```
2^31-1 =2147483647 个 16-bit Unicodecharacter

2147483647 * 16 = 34359738352 位

34359738352 / 8 = 4294967294 (Byte)

4294967294 / 1024 = 4194303.998046875 (KB)

4194303.998046875 / 1024 = 4095.9999980926513671875 (MB)

4095.9999980926513671875 / 1024 = 3.99999999813735485076904296875 (GB)
```

## 3.3. 字符串长度限制

字符串有长度限制，在编译期，要求字符串常量池中的常量不能超过65535，并且在javac执行过程中控制了最大值为65534。

在运行期，长度不能超过Int的范围，否则会抛异常。

# 4. java工具

## 4.1. maven

Maven 是一个项目管理和自动化构建工具，它基于项目对象模型（POM，Project Object Model）的概念。Maven 简化了项目的构建、依赖管理和项目信息管理。通过使用 Maven，开发者可以定义项目的构建过程、依赖关系、插件等，并通过简单的命令来执行项目的编译、测试、打包、部署等任务。

### 4.1.1. 导入本地jar到项目

#### 4.1.1.1. 通过mvn导入jar包

使用`mvn install:install-file`命令Maven提供了一个`install:install-file`的goal，允许你将本地jar包安装到本地仓库中。你需要提供jar包的位置、groupId、artifactId、version以及（可选的）packaging类型（通常是jar）。

- `<path-to-file>`：你的jar包文件的完整路径。
- `<group-id>`：你的项目的groupId，通常是一个组织或公司的反向域名。
- `<artifact-id>`：你的项目的artifactId，通常是项目的名称或模块名称。
- `<version>`：你的项目的版本号。
- `<packaging>`：打包类型，对于jar包来说，通常是`jar`。

```
mvn install:install-file -Dfile=<path-to-file> -DgroupId=<group-id> -DartifactId=<artifact-id> -Dversion=<version> -Dpackaging=<packaging>
```

```
mvn install:install-file -Dfile=path/to/my-library.jar -DgroupId=com.example -DartifactId=my-library -Dversion=1.0-SNAPSHOT -Dpackaging=jar
```

#### 4.1.1.2. 将jar包添加到项目中，然后再pom文件里指定所加入到项目得jar包路径

先是创建了lib文件夹在main目录下，在把jar包copy过来，

```
<dependency>
            <groupId>e-iceblue</groupId>
            <artifactId>spire.xls.free</artifactId>
            <version>5.1.0</version>
            <systemPath>${project.basedir}/src/main/lib/xxxx.jar</systemPath>
            <scope>system</scope>
        </dependency>
```

# 5. 安全检测

## 5.1. SQL注入：MyBatis

SQL 注入(SQL)是一种注入攻击，可以执行恶意 SQL 语句。这些语句控制 web 应用程序后面的数据库服务器。攻击者可以利用 SQL 注入漏洞绕过应用程序安全措施。他们可以绕过网页或 Web 应用程序的身份验证和授权，并检索整个 SQL 数据库的内容。他们还可以使用 SQL 注入来添加、修改和删除数据库中的记录。

在 SQL 注入攻击中，用户可以直接向数据库提交 SQL 查询,无需提供适当的凭据即可获得访问权限.然后攻击者可以查看、导出、修改和刑除机密信息:更改密码和其他身份验证信息:并可能访问网终中的其他系统。这是最常被利用的漏洞类别之一，但可以通过良好的编码实践在很大程度上避免。变量名称周围的 #字符表示 iBatis 将使用 setName 变量创建参数化查询。但是,iBatis 还允许使用 $字符将变量直接连接到 SQL 指令，使其易受 SQL injection 攻击。

## 5.2. 代码注入

  

## 5.3. 系统信息泄露

你这句可能没写完，应该是想问：  
**Spring Boot 中 @Valid 注解 与 @Validated 的区别** 对吧？

我先直接给你解释一下这两个概念，如果你问的是别的组合（例如与 @NotNull/@NotBlank 的区别等），你可以补充。

---

## 5.4. ✔ @Valid vs @Validated

### 5.4.1. 1）@Valid —— 来自 JSR 规范（javax.validation）

- 用于触发 Java Bean Validation
- 作用对象可以是：

- 方法参数
- 方法返回值
- 属性字段

- 支持嵌套验证（很重要！子对象也会继续验证）

示例：

```
public Response createUser(@Valid @RequestBody UserDTO dto)
```

如果 UserDTO 里还有 Address 对象：

```
class UserDTO {
    @NotBlank
    private String name;

    @Valid  // 继续对子对象做验证
    private Address address;
}
```

---

### 5.4.2. 2）@Validated —— Spring 提供

- 用于开启 Spring 的校验机制
- 支持 **分组验证（Groups）**
- 不支持嵌套验证！

示例：

```
@Validated
public Response addUser(@RequestBody UserDTO dto)
```

如果你有验证分组：

```
public interface Create {}
public interface Update {}
```

```
class UserDTO {
    @NotBlank(groups = Create.class)
    private String username;

    @NotNull(groups = Update.class)
    private Long id;
}
```

使用：

```
public Response create(@Validated(Create.class) @RequestBody UserDTO dto)
public Response update(@Validated(Update.class) @RequestBody UserDTO dto)
```

---

## 5.5. ⚠️ 超重要区别总结

|特性|@Valid|@Validated|
|---|---|---|
|来源|原生 JSR 规范|Spring|
|分组验证|❌ 不支持|✔ 支持|
|嵌套对象验证|✔ 支持|❌ 不支持|
|使用场景|常规简单校验|复杂业务规则、创建/更新区分|

---

## 5.6. ❗最容易踩的坑

### 5.6.1. 1. 嵌套验证失败，因为用了 @Validated

```
@Validated
class UserDTO {
    @Valid   // ❗ 这个在 @Validated 上不起作用！
    private Address address;
}
```

结果 → address 的字段不会被校验！

---

### 5.6.2. 2. 在 Controller 方法的入参上

你可以这样写：

✔ 推荐写法：

```
public Response test(@Valid @RequestBody UserDTO dto)
```

✔ 使用分组时：

```
public Response test(@Validated(Create.class) @RequestBody UserDTO dto)
```

---

## 5.7. 哪个应该用？

### 5.7.1. 如果你不需要分组验证：

✔ 用 @Valid

### 5.7.2. 如果你需要根据场景设置不同校验策略：

✔ 用 @Validated + Groups

---

## 5.8. 最后

如果你其实想问的是：

- @Valid 与 @NotNull、@NotBlank 的区别
- @Valid 和 @NotEmpty 的区别
- @Valid 和 Hibernate Validator 的关系
- 请求体校验和方法参数校验如何触发
- 校验失败输出统一 JSON
- 校验异常如何通过 @ControllerAdvice 处理

你继续说一声，我都可以帮你讲清楚，并给你直接可运行的示例代码。