# Jenkins 是什么

- **定义**：Jenkins 是一个基于 Java 的 **开源自动化服务器**。
- **主要用途**：

- 持续集成（CI）：开发者提交代码后，自动编译、构建、测试。
- 持续交付/部署（CD）：自动将应用部署到测试环境/生产环境。
- 执行自动化任务（定时任务、脚本）。

- **特点**：

- 基于插件扩展（支持 Git、Maven、Docker、Kubernetes 等）。
- Web 界面操作，配置灵活。
- 社区活跃，文档丰富。

通俗讲：**你写好代码 → 提交到 Git → Jenkins 发现更新 → 自动帮你编译、测试、打包、部署 → 省去手工重复操作。**

---

# Jenkins 安装

Jenkins 是 Java 写的，所以前置依赖是 **JDK (推荐 Java 11/17)**。安装方式主要有 3 种：

### (1) Linux 系统安装（以 CentOS/RHEL 为例）

```
# 安装 Java
sudo yum install -y java-11-openjdk

# 添加 Jenkins 官方源
wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat/jenkins.repo
rpm --import https://pkg.jenkins.io/redhat/jenkins.io-2023.key

# 安装 Jenkins
sudo yum install -y jenkins

# 启动服务并开机自启
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

### (2) Ubuntu/Debian 安装

```
sudo apt update
sudo apt install -y openjdk-11-jdk

curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt update
sudo apt install -y jenkins
sudo systemctl enable jenkins
sudo systemctl start jenkins
```

### (3) Docker 安装（最推荐，方便）

```
docker run -d \
  -u root \
  -p 8080:8080 -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  jenkins/jenkins:lts
```

- 访问 `http://服务器IP:8080` 就能进入 Jenkins 界面。
- 初次登录需要密码：

```
cat /var/lib/jenkins/secrets/initialAdminPassword
# 或 docker 方式
docker exec -it <容器ID> cat /var/jenkins_home/secrets/initialAdminPassword
```

# Jenkins 使用

### (1) 首次登录配置

- 用上面的密码登录 Jenkins Web 界面。
- 安装推荐插件。
- 创建管理员账号。

### (2) 创建 Job（任务）

1. 新建任务（例如 **Freestyle Project** 或 **Pipeline**）。
2. 配置 **源码管理**（Git 地址）。
3. 配置 **构建触发器**（手动、定时、webhook）。
4. 配置 **构建步骤**（Maven 构建、Gradle、Shell 脚本等）。
5. 配置 **构建后操作**（部署、发送邮件、归档制品等）。

### (3) 使用场景举例

- **Java 项目**：用 Maven 构建，打包成 JAR/WAR，自动部署到 Tomcat/Docker。
- **前端项目**：拉取代码 → npm install → npm run build → 发布到 Nginx。
- **微服务**：结合 Docker + Kubernetes，实现 CI/CD。

---

✅ 总结：

- Jenkins = 自动化服务器（CI/CD 工具）。
- 安装方式：Linux 包管理器 / Docker / 手动运行 Java。
- 使用流程：**安装 → 登录 → 配置插件 → 新建任务 → 配置源码管理 + 构建步骤 → 执行构建 → 自动化交付。**

---