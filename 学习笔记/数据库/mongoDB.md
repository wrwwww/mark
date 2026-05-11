# mongoDB的安装

  

## 配置文件mongod.cfg

  

```
 
storage:
	数据库的路径
  dbPath: D:\software\mongodb-win32-x86_64-windows-6.0.4\data\db
  journal:
    enabled: true
 
systemLog:
  destination: file
  logAppend: true
	  日志的路径
  path:  D:\software\mongodb-win32-x86_64-windows-6.0.4\log\mongod.log
net:
  port: 27017
  bindIp: 127.0.0.1
```

  

目录的创建  
创建数据库路径和log路径

  

```
mongod --dbpath 上面的数据库路径
刷入MongoDB服务
mongod --config '绝对路径的配置文件' --install 
net start MongoDB 开启服务
net stop MongoDB 关闭服务
mongod --remove 删除服务
```

  

## MongoDB的使用

  

```
全部数据库
show dbs/databases
当前数据库
db
创建和使用数据库
use db 
当前数据库中的表
show collocation
查看指定表中的数据
db.getCollection("指定表").find({name:"sss"}).pretty()
等同于
db.users.find({}).pretty()
pretty()一行打印

更新数据库
db.users.update({"查询的条件"},{$set{'更新的内容'}})
db.users.update({username:'admin'},{$set:{ isAdmin : true }})

插入数据
db.users.insert({}) //如果主键重复抛出异常,提示主键重复,数据不被保存
db.users.save({}) // 如果主键存在更新数据,否则插入数据,已废弃可以使用 insertOne()或者replaceOne()替换

删除数据
db.users.deleteOne({条件})
删除所有
db.users.deleteMany({})
查看所有
db.users.find()

逻辑运算符 与或非
and 的话就是多个条件
db.users.find({条件1,条件2})

or
db.users.find($or:[{条件1},{条件2}])

and or
dn.users.find({like:'ss',$or:[{条件1},{条件2}]})
```

使用docker安装mongodb

```
podman run -d --name mongodb -p 27017:27017 -v /home/wrw/mongodb/db:/data/db:Z --restart=always mongo
```

```
# 进入mongdb命令行
podman exec  -it mongodb mongosh
# 切换到 admin 数据库，这是创建具有管理权限的用户的地方。
use admin
# 创建管理员
db.createUser({user:"admin",pwd:"admin123",roles:[{role:"root",db:"admin"}]})
# 创建存ai对话数据的数据库
use ai_agent
# 创建供ai使用的用户
db.createUser({user:"ai_agent",roles:[role:"readWrite",db:"ai_agent"]})
# 重启mongodb镜像刷新权限
podman  restart mongodb
```

## MongoDB 用户认证问题分析

### **用户信息概述**

你遇到的连接问题是由于 MongoDB 中的两个用户配置不同引起的。以下是这两个用户的关键区别：

#### **用户1：**`ai_agent.ai_agent`

- **数据库**：`ai_agent`
- **认证数据库**：`ai_agent`
- **权限**：只能在 `ai_agent` 数据库中进行读写操作。
- **连接字符串**：需要指定 `authSource=ai_agent`，例如：

```
mongodb://ai_agent:password@localhost:27017/ai_agent?authSource=ai_agent
```

#### **用户2：**`admin.ai_agent`

- **数据库**：`admin`
- **认证数据库**：`admin`
- **权限**：在 `admin` 数据库中具有只读权限，在 `ai_agent` 数据库中具有读写权限。
- **连接字符串**：可以省略 `authSource`，默认为 `admin`，例如：

```
mongodb://ai_agent:password@localhost:27017/ai_agent
```

### **错误分析**

从你的错误日志看到：

```
"error": "UserNotFound: Could not find user \"ai_agent\" for db \"admin\""
```

这表示 **MongoDB 默认在** `admin` **数据库中查找用户**，但第一个用户是在 `ai_agent` 数据库中创建的。

### **解决方案**

#### **方案1：使用第二个用户（推荐）**

使用 `admin.ai_agent` 用户，它具有跨数据库的访问权限。在连接字符串中设置 `authSource=admin`：

```
mongodb://ai_agent:password@localhost:27017/ai_agent?authSource=admin
```

#### **方案2：修改连接使用第一个用户**

如果你想继续使用第一个用户 `ai_agent.ai_agent`，则需要在连接字符串中指定 `authSource=ai_agent`：

```
mongodb://ai_agent:password@localhost:27017/ai_agent?authSource=ai_agent
```

### **详细解释**

- **用户1：**`ai_agent.ai_agent`

- 仅能在 `ai_agent` 数据库中进行认证。
- 需要显式设置 `authSource=ai_agent`。

- **用户2：**`admin.ai_agent`

- 具有 `admin` 和 `ai_agent` 数据库的权限。
- 适合用于跨数据库操作，且可以省略 `authSource`。

### **测试连接**

- **用户1（会失败的情况）**

```
mongosh "mongodb://ai_agent:password@localhost:27017/ai_agent"
# 错误：UserNotFound in admin database

# 正确：指定 authSource
mongosh "mongodb://ai_agent:password@localhost:27017/ai_agent?authSource=ai_agent"
```

- **用户2（推荐）**

```
mongosh "mongodb://ai_agent:password@localhost:27017/ai_agent"
# 或明确指定
mongosh "mongodb://ai_agent:password@localhost:27017/ai_agent?authSource=admin"
```

### **最佳实践建议**

对于生产环境，建议将管理用户创建在 `admin` 数据库中，集中管理用户权限，并避免在业务数据库中创建用户。

### **如何清理和简化用户配置**

如果不需要两个用户，可以删除不需要的用户：

#### **删除第一个用户（在** `ai_agent` **数据库）**

```
use ai_agent
db.dropUser("ai_agent")
```

#### **保留第二个用户（在** `admin` **数据库）**

```
use admin
db.getUser("ai_agent")
```

并可以为用户添加更多权限：

```
db.grantRolesToUser("ai_agent", [
  {role: "readWrite", db: "ai_agent"},
  {role: "dbAdmin", db: "ai_agent"}
])
```

### **总结**

- **问题原因**：MongoDB 默认在 `admin` 数据库寻找用户进行认证，而你的第一个用户在 `ai_agent` 数据库。
- **解决方法**：

1. 使用第二个用户（推荐），并在连接字符串中设置 `authSource=admin`。
2. 或者删除多余的用户，保持一个统一的认证源。

---

这样整理后就专注于 MongoDB 用户认证的问题，如果有其他问题，欢迎继续提问！