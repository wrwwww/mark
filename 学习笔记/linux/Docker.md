# 简介

## 什么是docker

# 安装

## 包管理工具

```
yum install docker -y
```

## 离线安装

  

## 通过压缩包安装

压缩包下载地址[下载地址](https://download.docker.com/linux/static/stable/x86_64/)

```
#!/bin/bash
FILE_NAME="docker-27.2.0.tgz"

DOWNLOAD_URL="https://download.docker.com/linux/static/stable/x86_64/docker-27.2.0.tgz"

# 检查文件是否存在  
if [ ! -f "$FILE_NAME" ]; then  
    echo "文件 $FILE_NAME 不存在，正在下载..."  
    # 使用 wget 或 curl 下载文件  
    # wget 示例  
    wget "$DOWNLOAD_URL"  
    # curl 示例  
    # curl -O "$DOWNLOAD_URL"  
    echo "文件 $FILE_NAME 已下载。"  
else  
    echo "文件 $FILE_NAME 已存在。"  
fi


# 解压docker安装包
tar zxvf $FILE_NAME

# 移动文件到指定路径
cp docker/* /usr/bin/



echo "[Unit]
Description=Docker Application Container Engine
Documentation=https://docs.docker.com
After=network-online.target firewalld.service
Wants=network-online.target

[Service]
Type=notify
# the default is not to use systemd for cgroups because the delegate issues still
# exists and systemd currently does not support the cgroup feature set required
# for containers run by docker
ExecStart=/usr/bin/dockerd
ExecReload=/bin/kill -s HUP $MAINPID
# Having non-zero Limit*s causes performance problems due to accounting overhead
# in the kernel. We recommend using cgroups to do container-local accounting.
LimitNOFILE=infinity
LimitNPROC=infinity
LimitCORE=infinity
# Uncomment TasksMax if your systemd version supports it.
# Only systemd 226 and above support this version.
#TasksMax=infinity
TimeoutStartSec=0
# set delegate yes so that systemd does not reset the cgroups of docker containers
Delegate=yes
# kill only the docker process, not all processes in the cgroup
KillMode=process
# restart the docker process if it exits prematurely
Restart=on-failure
StartLimitBurst=3
StartLimitInterval=60s

[Install]
WantedBy=multi-user.target" > /etc/systemd/system/docker.service



# 在/etc/systemd/system/下创建docker.service文件
echo 在/etc/systemd/system/下创建docker.service文件

# 赋权：
chmod +x /etc/systemd/system/docker.service

# 使配置生效、启动docker服务并查看服务状态：
systemctl daemon-reload   
systemctl start docker.service     
systemctl status docker.service

# 设置服务自启动
systemctl enable docker.service

```

## 卸载

```shell
#!/bin/bash

echo 关闭docker中运行的容器
docker stop $(docker ps -a -q)
echo 删除docker容器
docker rm $(docker ps -a -q)
echo 删除docker中的镜像
docker rmi $(docker images -a -q)

echo 关闭docker服务
systemctl disable docker.service
systemctl stop docker.service
systemctl daemon-reload   

rm -rf /etc/systemd/system/docker.service
# 移除该目录下的docker文件
rm -rf  /usr/bin/containerd
rm -rf  /usr/bin/containerd-shim
rm -rf  /usr/bin/ctr
rm -rf  /usr/bin/docker
rm -rf  /usr/bin/docker-init
rm -rf  /usr/bin/docker-proxy
rm -rf  /usr/bin/runc
```

## 镜像加速

[镜像加速文章](https://developer.aliyun.com/article/1131834)

```
https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors
```

```
// -y 说明yes 因为它会自动确认所有提示，这在某些情况下可能会导致意外的结果，特别是当你进行系统更新或删除软件包时。
yum  install docker -y 
// 启动docker
service docker start
// 关闭docker
service docker stop
// 删除镜像
docker rm 镜像名称
// 查看镜像
docker image
-d 是以后台的形式运行容器
-- name 设置名称
```

```
docker run -it -d --name mysql_4 -p 12004:3306 \
--net mynet --ip 172.18.0.5 \
-m 400m -v /root/mysql_4/data:/var/lib/mysql \
-v /root/mysql_4/config:/etc/mysql/conf.d \
-e MYSQL_ROOT_PASSWORD=1234 \
-e TZ=Asia/Shanghai --privileged=true \
mysql:8.0.23  \
--lower_case_table_names=1
```

  

### 拉取镜像

```
docker pull openjdk:17.0.1-jdk
```

### 查看已经启动的docker程序

```
docker ps
# 启用未启用都可以
docker ps -a 
```

### 查看本地的docker镜像

```
docker images
```

### 导入本地镜像

```
docker load < your_image.tar
```

### 启动一个新的docker程序

docker run 这是Docker的基本命令之一，用于运行一个新的容器实例。

```
docker run: 这是Docker的基本命令之一，用于运行一个新的容器实例。
-it: 这两个参数通常一起使用。-i（或--interactive）保持STDIN开放，即使没有附加任何东西；-t（或--tty）分配一个伪终端。然而，在这个命令中，由于-d的存在，-it可能不会按预期工作（通常-it用于交互式会话，而-d用于在后台运行容器）。
-d: 以守护进程模式运行容器，在后台运行。
--name mysql_4: 为容器指定一个名称，这里是mysql_4。
-p 12004:3306: 将容器的3306端口映射到宿主机的12004端口上，允许从宿主机通过12004端口访问MySQL服务。
--net mynet --ip 172.18.0.5: 指定容器使用名为mynet的自定义网络，并为容器分配一个静态IP地址172.18.0.5。
-m 400m: 限制容器可以使用的最大内存为400MB。
-v /root/mysql_4/data:/var/lib/mysql: 创建一个卷，将宿主机的/root/mysql_4/data目录挂载到容器的/var/lib/mysql目录，用于存储MySQL数据库文件。
-v /root/mysql_4/config:/etc/mysql/conf.d: 另一个卷挂载，将宿主机的/root/mysql_4/config目录挂载到容器的/etc/mysql/conf.d目录，允许你放置自定义的MySQL配置文件。
-e MYSQL_ROOT_PASSWORD=1234: 设置环境变量MYSQL_ROOT_PASSWORD，MySQL容器启动时会自动使用这个值来设置root用户的密码。
-e TZ=Asia/Shanghai: 设置环境变量TZ，用于指定容器的时区为中国上海时区。
--privileged=true: 给容器额外的权限，相当于在容器内运行root权限的命令。这通常不是必需的，除非有特定需求（如访问宿主机的设备）。
mysql:8.0.23: 指定要运行的镜像名称和标签，这里是MySQL 8.0.23版本的官方镜像。
--lower_case_table_names=1: 这不是Docker命令的一部分，而是MySQL的启动参数，应该放在MySQL的配置文件（如my.cnf）中，或者通过环境变量或其他方式传递给MySQL服务。然而，在这个命令中，它直接放在了Docker命令的最后，这通常不会按预期工作，因为Docker命令本身不会直接将这些参数传递给MySQL服务。如果你需要设置lower_case_table_names=1，你应该在你的/root/mysql_4/config目录下的某个配置文件中设置它。
--rm参数表示在容器停止后自动删除容器，这样可以避免在主机上留下无用的容器。
```

```
docker run -it -d --name mysql_4 -p 12004:3306 \
--net mynet --ip 172.18.0.5 \
-m 400m -v /root/mysql_4/data:/var/lib/mysql \
-v /root/mysql_4/config:/etc/mysql/conf.d \
-e MYSQL_ROOT_PASSWORD=1234 \
-e TZ=Asia/Shanghai --privileged=true \
mysql:8.0.23  \
--lower_case_table_names=1
```

```
docker run -it -d --name test -p 12345:8080 \
--net mynet --ip 172.18.0.33 \
-e TZ=Asia/Shanghai --privileged=true \
docker_test  \
--lower_case_table_names=1
```

### 启动程序

```
docker start redis
```

### 停止程序

```
docker stop redis
```

### 查看日志

```
docker logs id
```

### 删除镜像

```
docker rmi <image_id>
```

### 删除部署的镜像

```
docker rm name
```

# 常用命令

## docker volume（数据卷管理）

### **1. 基本操作**

```bash
# 查看所有数据卷
docker volume ls

# 创建数据卷
docker volume create my-volume

# 查看数据卷详情
docker volume inspect my-volume
# 输出：挂载点、驱动、标签等信息

# 删除数据卷
docker volume rm my-volume
# 强制删除（即使有容器在使用）
docker volume rm -f my-volume

# 删除所有未使用的数据卷
docker volume prune
```

### **2. 数据卷使用示例**

```bash
# 创建并使用数据卷
docker run -d \
  --name mysql \
  -v mysql-data:/var/lib/mysql \  # 使用命名卷
  mysql:8.0

# 挂载宿主机目录
docker run -d \
  --name nginx \
  -v /host/path:/container/path:ro \  # 只读挂载
  -v /host/path2:/container/path2:rw \ # 读写挂载（默认）
  nginx

# 临时文件系统（tmpfs）
docker run -d \
  --name tmpfs-test \
  --tmpfs /tmp:size=100m,mode=1777 \
  alpine

# 使用多个数据卷
docker run -d \
  --name app \
  -v config:/app/config \
  -v data:/app/data \
  -v logs:/app/logs \
  myapp
```

### **3. 高级卷操作**

```
# 从容器复制数据到卷
docker run --rm -v my-volume:/data alpine cp /etc/hosts /data/

# 备份数据卷
docker run --rm \
  -v my-volume:/source \
  -v $(pwd):/backup \
  alpine tar czf /backup/backup.tar.gz -C /source .

# 恢复数据卷
docker run --rm \
  -v my-volume:/target \
  -v $(pwd):/backup \
  alpine tar xzf /backup/backup.tar.gz -C /target

# 卷之间复制数据
docker run --rm \
  -v source-volume:/source \
  -v target-volume:/target \
  alpine cp -a /source/. /target/
```

### **4. 卷驱动插件**

```bash

# 查看可用驱动
docker volume ls --filter driver=local

# 使用不同驱动创建卷
docker volume create \
  --driver local \
  --opt type=nfs \
  --opt o=addr=192.168.1.100,rw \
  --opt device=:/path/to/share \
  nfs-volume
```

## docker network（网络管理）

### 基本操作

```bash
# 查看所有网络
docker network ls
# 输出：网络ID、名称、驱动、作用域

# 查看网络详情
docker network inspect bridge
# 输出：子网、网关、连接的容器等

# 创建网络
docker network create my-network
# 指定驱动和子网
docker network create \
  --driver bridge \
  --subnet 172.20.0.0/16 \
  --gateway 172.20.0.1 \
  custom-network

# 删除网络
docker network rm my-network
# 强制删除（即使有容器连接）
docker network rm -f my-network

# 删除所有未使用的网络
docker network prune

```

### 网络类型

```bash
# 1. bridge网络（默认）
docker network create --driver bridge isolated-net

# 2. host网络（共享宿主机网络栈）
docker run -d --network host nginx

# 3. none网络（无网络）
docker run -d --network none alpine

# 4. overlay网络（Swarm集群）
docker network create --driver overlay swarm-net

# 5. macvlan网络（物理网络直接访问）
docker network create \
  --driver macvlan \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  -o parent=eth0 \
  macvlan-net

```

### 容器网络操作

```bash
# 运行容器时连接网络
docker run -d \
  --name web \
  --network my-network \
  nginx

# 连接容器到网络
docker network connect my-network existing-container

# 断开网络连接
docker network disconnect my-network container

# 查看容器网络信息
docker inspect container-name | grep -A 10 "Networks"

# 为容器设置别名
docker run -d \
  --name app \
  --network my-network \
  --network-alias app-server \
  myapp

# 连接多个网络
docker network connect net1 container
docker network connect net2 container
```

### 网络配置示例

```bash
# 创建用于微服务的网络
docker network create \
  --driver bridge \
  --subnet 10.10.0.0/24 \
  --gateway 10.10.0.1 \
  --ip-range 10.10.0.128/25 \
  --label env=production \
  backend-net

# 运行服务
docker run -d \
  --name api \
  --network backend-net \
  --ip 10.10.0.10 \
  api-service

docker run -d \
  --name db \
  --network backend-net \
  --ip 10.10.0.20 \
  postgres

# 测试网络连通性
docker exec api ping db  # 通过容器名访问
docker exec api ping 10.10.0.20  # 通过IP访问
```

## docker inspect（详细信息查看）

### 基本用法

```bash
# 查看容器详细信息
docker inspect container-name
docker inspect container-id

# 查看镜像详细信息
docker inspect image-name:tag
docker inspect image-id

# 查看网络/卷/其他对象
docker inspect network-name
docker inspect volume-name
```

### 格式化输出

```bash
# 仅获取特定信息
docker inspect --format='{{.Name}}' container
docker inspect --format='{{.NetworkSettings.IPAddress}}' container

# 获取JSON格式的特定部分
docker inspect --format='{{json .NetworkSettings}}' container

# 获取多个信息
docker inspect --format='容器: {{.Name}} IP: {{.NetworkSettings.IPAddress}}' container
```

### 常用查询路径

```bash
# 容器状态信息
docker inspect --format='{{.State.Status}}' container  # 运行状态
docker inspect --format='{{.State.Pid}}' container     # 进程ID
docker inspect --format='{{.State.StartedAt}}' container  # 启动时间

# 网络信息
docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container
docker inspect --format='{{.NetworkSettings.Ports}}' container  # 端口映射

# 配置信息
docker inspect --format='{{.Config.Image}}' container  # 使用的镜像
docker inspect --format='{{.Config.Cmd}}' container    # 启动命令
docker inspect --format='{{.Config.Entrypoint}}' container  # 入口点
docker inspect --format='{{.Config.Env}}' container    # 环境变量

# 挂载点信息
docker inspect --format='{{.Mounts}}' container
docker inspect --format='{{json .Mounts}}' container | jq  # 使用jq格式化

# 资源限制
docker inspect --format='{{.HostConfig.Memory}}' container  # 内存限制
docker inspect --format='{{.HostConfig.NanoCpus}}' container  # CPU限制
```
### 高级查询

```bash
# 遍历数组/映射
docker inspect --format='{{range .Mounts}}{{.Source}} -> {{.Destination}}{{"\n"}}{{end}}' container

# 条件判断
docker inspect --format='{{if .State.Running}}Running{{else}}Stopped{{end}}' container

# 索引访问
docker inspect --format='{{index .Config.Env 0}}' container  # 第一个环境变量

# 大小写转换
docker inspect --format='{{upper .Name}}' container
docker inspect --format='{{lower .Config.Image}}' container

# 切片操作
docker inspect --format='{{slice .Id 0 12}}' container  # 获取短ID
```

### 使用脚本示例

```shell
#!/bin/bash
# 获取所有运行中容器的基本信息
for container in $(docker ps -q); do
  name=$(docker inspect --format='{{.Name}}' $container | sed 's/\///')
  ip=$(docker inspect --format='{{.NetworkSettings.IPAddress}}' $container)
  image=$(docker inspect --format='{{.Config.Image}}' $container)
  status=$(docker inspect --format='{{.State.Status}}' $container)
  echo "容器: $name | IP: $ip | 镜像: $image | 状态: $status"
done

# 输出示例：
# 容器: web | IP: 172.17.0.2 | 镜像: nginx:alpine | 状态: running
```

```shell
#!/bin/bash
# 检查容器的资源使用情况
container=$1

echo "=== 容器: $(docker inspect --format='{{.Name}}' $container) ==="
echo "内存限制: $(docker inspect --format='{{.HostConfig.Memory}}' $container) bytes"
echo "CPU限制: $(docker inspect --format='{{.HostConfig.NanoCpus}}' $container) nano CPUs"
echo "重启策略: $(docker inspect --format='{{.HostConfig.RestartPolicy.Name}}' $container)"
echo "挂载点:"
docker inspect --format='{{range .Mounts}}{{printf "  %s -> %s\n" .Source .Destination}}{{end}}' $container
```

# compose

1. 上线

```
docker compose up -d 
```

## docker compose常用命令

### 启动服务

docker-compose up -d

### 停止服务

docker-compose down

### 列出所有运行容器

docker-compose ps

### 查看服务日志

docker-compose logs

### 构建或

docker-compose build者重新构建服务

### 启动服务

docker-compose start

### 停止已运行的服务

docker-compose stop

### 重启服务

docker-compose restart

## 本地虚拟机部署项目

#### 打包jar程序

```
mvn clean package
```

写DockerFile

```
# 运行的基础环境
FROM openjdk:17.0.1-jdk

# 工作目录

WORKDIR /opt/docker/images/docker_test/

ADD docker_test-0.0.1-SNAPSHOT.jar docker_test.jar

LABEL authors="wrw"
# 暴露8080端口
EXPOSE 8080
EXPOSE 8081

ENTRYPOINT ["top", "-b"]

# 容器运行时候执行的命令
CMD java -jar docker_test.jar --server.port=8081

```

#### 代理

```
export http_proxy="http://192.168.1.124:7890"
export https_proxy="http://192.168.1.124:7890"

export all_proxy="socks5://192.168.1.124:7890"
```

### docker配置网络

在Docker中，`docker network create` 命令用于创建一个新的网络。这个命令允许你自定义网络的许多方面，包括网络驱动（如bridge、overlay等）、子网、网关等。在你给出的命令中：

```
bash复制代码



docker network create --subnet=172.18.0.0/18 mynet
```

这个命令创建了一个名为`mynet`的新网络，并指定了该网络的子网为`172.18.0.0/18`。

- `--subnet=172.18.0.0/18`：这个参数指定了网络的子网和子网掩码的长度。`172.18.0.0/18`表示子网是从`172.18.0.0`开始，子网掩码是`/18`，这意味着前18位是网络地址，剩下的14位（32-18=14）是主机地址。因此，这个子网可以包含从`172.18.0.0`到`172.18.63.255`的IP地址（总共有2^14=16384个IP地址，但是网络地址和广播地址不可用，所以实际可用的IP地址数量会少一些）。
- `mynet`：这是你创建的网络的名称。

创建了这个网络之后，你可以在启动容器时通过`--network=mynet`参数将容器连接到这个网络。这样做的好处包括：

1. **隔离性**：不同网络中的容器默认是相互隔离的，不能直接通信，这有助于提升安全性。
2. **自定义网络设置**：你可以为网络指定自定义的IP地址范围、网关等，这对于需要精细控制网络通信的场景非常有用。
3. **容器间通信**：容器可以通过自定义网络轻松地进行通信，而无需担心底层的网络配置。

总之，`docker network create --subnet=172.18.0.0/18 mynet`命令是Docker中创建自定义网络的一个实用示例，它为Docker容器提供了灵活的网络通信能力。

```
docker network create --subnet=172.18.0.0/18 mynet
```

### 安装redis

```
docker run -it -d --name redis -p 6379:6379 \
--net mynet --ip 172.18.0.1 \
-m 200m -v /root/redis/conf:/usr/local/etc/redis \
-e TZ=Asia/Shanghai --privileged=true  \
redis /usr/local/etc/redis/redis.conf
```

### 安装nginx

```
# 创建nginx，复制配置文件等目录
# 生成容器
docker run --name nginx -p 9001:80 -d nginx
# 将容器nginx.conf文件复制到宿主机
docker cp nginx:/etc/nginx/nginx.conf /home/nginx/conf/nginx.conf
# 将容器conf.d文件夹下内容复制到宿主机
docker cp nginx:/etc/nginx/conf.d /home/nginx/conf/conf.d
# 将容器中的html文件夹复制到宿主机
docker cp nginx:/usr/share/nginx/html /home/nginx/
# 删除nginx
docker stop nginx && docker rm nginx
# 重新创建nginx，并指定文件映射
```

```
docker run -it -d --name nginx1 -p 8080:80 \
--net mynet --ip 172.18.0.2 \
-v /root/nginx_test/nginx1/conf/nginx.conf:/etc/nginx/nginx.conf \
-v /root/nginx_test/nginx1/conf/conf.d:/etc/nginx/conf.d \
-v /root/nginx_test/nginx1/log:/var/log/nginx \
-v /root/nginx_test/nginx1/html:/usr/share/nginx/html \
-m 200m \
-e TZ=Asia/Shanghai --privileged=true  \
nginx
```

```
docker run -it -d --name nginx2 -p 8081:80 \
--net mynet --ip 172.18.0.3 \
-v /root/nginx_test/nginx2/conf/nginx.conf:/etc/nginx/nginx.conf \
-v /root/nginx_test/nginx2/conf/conf.d:/etc/nginx/conf.d \
-v /root/nginx_test/nginx2/log:/var/log/nginx \
-v /root/nginx_test/nginx2/html:/usr/share/nginx/html \
-m 200m \
-e TZ=Asia/Shanghai --privileged=true  \
nginx
```

#### 权限问题

##### docker安装nginx 启动提示日志权限问题

[权限问题13](https://blog.csdn.net/weixin_41157200/article/details/127127775)

### docker导入导出镜像

[https://www.hangge.com/blog/cache/detail_2411.html](https://www.hangge.com/blog/cache/detail_2411.html)

## 报错

### library initialization failed - unable to allocate file descriptor table - out of memory

[LimitNOFILE=infinity 虽然是不限制，但是在systemctl版本小于234的时候不生效，查看systemctl版本：systemctl --version](https://www.cnblogs.com/Leonardo-li/p/17047308.html)