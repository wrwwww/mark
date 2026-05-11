### 定位
Zabbix 是一款**企业级开源分布式监控解决方案**，旨在为 IT 基础设施、应用服务和网络设备提供统一的**可用性与性能监控**。

其核心定位在于：

+ **全栈可观测性**：覆盖从硬件服务器、操作系统、虚拟化平台到数据库、中间件、云原生应用的全链路监控。
+ **一体化平台**：集数据采集、存储、可视化、告警通知于一体，无需拼凑多个工具。
+ **企业级特性**：支持分布式架构、高可用部署、细粒度权限控制和 API 自动化。
+ **灵活扩展**：通过模板、自定义脚本、插件机制快速适配各类监控场景。

### 功能架构与组件详解
Zabbix 采用模块化设计，各组件职责明确、协同工作。其整体功能架构如下：

![](https://cdn.nlark.com/yuque/__mermaid_v3/d869b5802978fc23f47270709d65e6cf.svg)

| 组件 | 核心职责 | 技术说明 |
| :--- | :--- | :--- |
| **Zabbix Server** | 监控中枢 | 核心引擎，负责配置管理、轮询采集、触发器计算和告警动作执行。所有配置和运营数据均存储于数据库中。 |
| **Database** | 数据存储 | 存储配置信息、历史监控数据（原始值）和趋势数据（小时级聚合）。数据库性能是整套系统的瓶颈所在，推荐 SSD 存储和独立部署。 |
| **Web Frontend** | 交互界面 | 基于 PHP 开发的可视化操作界面，用于仪表盘展示、主机配置、报表生成和用户管理。通常与 Server 同机部署，也可分离。 |
| **Zabbix Agent** | 数据采集器 | 部署在被监控主机上的轻量级守护进程，负责采集本地指标（CPU、内存、磁盘、日志等）。支持 C 语言开发的轻量 Agent 和 Go 语言开发、易扩展的 Agent 2。 |
| **Zabbix Proxy** | 分布式代理 | 可选组件，部署在分支机构或网络隔离区域，代替 Server 收集数据并缓存后统一上报。可大幅降低中心 Server 的 I/O 和计算压力。 |


### 2.3 工作原理：三种监控模式深度解析
Zabbix 的数据采集基于 JSON 通信协议，根据连接发起方和数据流向分为以下三种模式。

#### 2.3.1 被动模式
**通信流程：Server → Agent:10050**

1. Server 主动向 Agent 的 **10050/TCP** 端口发起连接。
2. Server 发送监控项键值请求（如 `system.cpu.load`）。
3. Agent 执行本地采集，返回结果数据。
4. 连接关闭。

**特点与配置**：

+ 配置简单，是默认工作模式。
+ Agent 配置文件中的 `Server` 参数指定允许连接的 Server 或 Proxy 地址。
+ `StartAgents` 参数控制被动模式的监听进程数，设置为 0 则禁用被动模式。
+ 适用于网络互通的小规模环境。

#### 2.3.2 主动模式
**通信流程：Agent → Server:10051**

1. Agent 主动向 Server 的 **10051/TCP** 端口发起连接。
2. Agent 首先请求自己需要监控的 Items 列表（JSON 格式）。
3. Server 返回属于该主机的所有主动式监控项（key、delay 等）。
4. Agent 按周期独立采集数据，再次主动连接 Server 批量推送结果。
5. 连接关闭。

**特点与配置**：

+ 大幅减少 Server 的连接开销，适合大规模监控。
+ Agent 配置文件中的 `ServerActive` 参数指定目标 Server 地址。
+ `RefreshActiveChecks` 参数控制拉取 Items 列表的间隔（默认 120 秒）。
+ 如果 Agent 位于防火墙后，主动模式可避免防火墙入站规则的修改。

#### 2.3.3 代理模式
**通信流程：Agent → Proxy → Server**

1. Agent 将数据发送给 Proxy（主动或被动均可）。
2. Proxy 将数据暂存于本地数据库（SQLite/MySQL/PostgreSQL）。
3. Proxy 定期与 Server 建立连接，批量同步采集数据。

**特点与配置**：

+ 分布式监控的关键组件，适用于跨机房、跨网段场景。
+ Proxy 断开与 Server 的连接时，采集不中断，数据本地缓存，恢复后自动补齐。
+ 建议主机数量超过 **500 台**时引入 Proxy 分担负载。

### 2.4 高可用方案
Zabbix 的高可用需分层实现，核心思路是消除各层单点故障。

| 层级 | HA 方案 | 实现原理 |
| :--- | :--- | :--- |
| **Zabbix Server** | **原生 Active-Passive** | Zabbix 6.0+ 支持多 Server 节点通过竞争数据库锁实现主备切换。主节点故障时，备节点可在数分钟内自动接管，监控不中断。 |
| **Database** | **主从复制 + 自动故障转移** | 数据库是 HA 的基础。可使用 **PostgreSQL + pg_auto_failover**（轻量级自动切换） 或 **MariaDB Galera Cluster**（多主同步复制，所有节点可读写） 实现数据库层高可用。 |
| **Web Frontend** | **负载均衡器 + 虚拟IP** | 部署多套 Web 前端，通过 **Keepalived + HAProxy** 或 Nginx 反向代理提供统一的虚拟 IP（VIP）。健康检查失败时自动将流量切换到健康节点。 |
| **Zabbix Proxy** | **无状态 + 本地缓存** | Proxy 无官方 HA 方案，但其设计为无状态节点，单点故障不影响已采集数据的本地缓存。可部署多 Proxy 实现采集端冗余，或快速重新部署恢复。 |


> **架构说明**：完整的高可用部署通常包含 **2 台 Zabbix Server**（主备）、**3 台数据库节点**（Galera 集群需满足法定人数）、**2+ 台 Web 前端**及负载均衡器，确保任意单节点故障时监控服务仍可正常访问。
>

### 2.5 环境要求
#### 2.5.1 硬件配置参考
以下为官方推荐的生产环境硬件配置（以 Linux/Unix 平台为例），监控规模以 **NVPS（每秒处理的新值数量）** 为核心衡量指标。

| 监控规模 | 监控项/触发器/图表 | CPU 核心 | 内存 | 数据库类型 | 存储建议 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **小型** | ~1,000 项 | 2 vCPU | 8 GB | MySQL/PostgreSQL | 500GB SSD |
| **中型** | ~10,000 项 | 4 vCPU | 16 GB | MySQL/PostgreSQL | 1TB SSD (RAID 10) |
| **大型** | ~100,000 项 | 16 vCPU | 64 GB | PostgreSQL（推荐） | 2TB+ SSD + HDD 分级存储 |
| **超大型** | ~1,000,000 项 | 32 vCPU | 96 GB+ | PostgreSQL + TimescaleDB | 企业级 SAN/NAS |


> **注意**：数据库性能是决定性瓶颈，建议数据库服务器使用 **SSD 存储**并独立部署。内存越大，数据库缓存命中率越高，系统响应越快。
>

#### 2.5.2 软件环境要求
| 组件 | 要求 | 说明 |
| :--- | :--- | :--- |
| **操作系统** | RHEL/CentOS 8+、AlmaLinux 9、Ubuntu 20.04/22.04、Debian 11+ | Server 建议部署在 Linux 平台；Agent 支持 Linux、Windows、AIX、Solaris 等。 |
| **Web 服务器** | Apache 1.3.12+ 或 Nginx 1.20+ | 前端基于 PHP，需配置 PHP-FPM。 |
| **PHP** | 7.2.5 - 8.3 | 需编译 GD、bcmath、ctype、libXML、mbstring、gettext 等扩展。 |
| **数据库** | MySQL 8.0+ / MariaDB 10.5+ / PostgreSQL 13+ | 推荐 **PostgreSQL + TimescaleDB**（时序数据优化）或 **MySQL 8.0**（InnoDB 引擎）。 |
| **浏览器** | Chrome/Firefox/Edge/Safari 最新稳定版 | 前端需启用 Cookie 和 JavaScript，最小屏幕宽度 1200px。 |


#### 2.5.3 核心依赖库
**Server 端**：libpcre（正则）、libevent（IPMI/高并发）、libcurl（Web 监控/VMware）、net-snmp（SNMP 监控）、fping（ICMP 检测）。

**Agent 端**：libpcre（正则）、OpenSSL/GnuTLS（加密传输可选）。

### 2.6 监控对象与能力范围
Zabbix 支持丰富的监控对象和采集协议：

| 类别 | 监控对象 | 采集方式 |
| :--- | :--- | :--- |
| **操作系统** | Linux/Unix/Windows 性能指标 | Zabbix Agent |
| **网络设备** | 路由器、交换机、防火墙 | SNMP (v1/v2c/v3) |
| **硬件带外** | 服务器硬件状态（温度、风扇、电源） | IPMI |
| **数据库** | MySQL、PostgreSQL、Oracle、SQL Server | Agent + 自定义查询 |
| **中间件** | Nginx、Apache、Tomcat、Redis、Kafka | Agent + 状态页解析 |
| **虚拟化** | VMware vSphere、KVM、Xen | Agent + SDK/API |
| **容器云** | Docker、Kubernetes | Agent + 脚本采集 |
| **应用服务** | HTTP/HTTPS、DNS、FTP 可用性 | Server 端轮询 |
| **日志文件** | 错误日志、访问日志 | Agent 主动监控 |


### 2.7 核心优势与局限性
**优势**：

+ **完全开源**：无许可证成本，社区活跃，二次开发灵活。
+ **统一监控**：一套系统覆盖从底层硬件到上层应用的全栈监控。
+ **模板驱动**：开箱即用的大量模板，自动化监控配置。
+ **强扩展性**：支持自定义脚本（UserParameter）、API 对接、插件开发。
+ **分布式架构**：原生支持 Proxy 代理，轻松实现跨地域集中监控。

**局限性**：

+ **数据库压力大**：所有数据存储于关系型数据库，大规模场景下数据库易成为瓶颈，需配合 **TimescaleDB** 或分区表优化。
+ **学习曲线较陡**：配置项多，深层次需求需要深入理解其设计逻辑，二次开发有一定门槛。
+ **批量操作不便**：通过 Web 界面进行大批量主机的修改操作不够灵活，建议使用 API 脚本配合完成。
+ **告警配置复杂**：精细化的阈值和依赖关系配置较为繁琐，不筛选时易产生告警风暴。

## Almx使用agent被动监控
> 编辑仓库配置
>

```bash
# /etc/yum.repos.d/epel.repo 
# 将epel仓库源中排除zabbix的程序，防止版本冲突
[epel]
...
excludepkgs=zabbix*
```

---



> 下载zabbix官方仓库,并清理缓存
>

```bash
rpm -Uvh https://repo.zabbix.com/zabbix/7.4/release/alma/9/noarch/zabbix-release-latest-7.4.el9.noarch.rpm
dnf clean all
```

---



> 如何卸载这个软件仓库
>

`rpm -qa | grep zabbix-release` 找到软件仓库源的包

`rpm -e $(rpm -qa | grep zabbix-release)` 删除软件仓库

---



> server端按需安装需要的程序 <font style="color:rgb(37, 40, 47);">Zabbix server，Web前端，agent</font>
>

```bash
dnf install zabbix-server-mysql zabbix-web-mysql zabbix-nginx-conf zabbix-sql-scripts zabbix-selinux-policy zabbix-agent
```

---



> 数据库建库并初始化
>

```bash
mysql> create database zabbix character set utf8mb4 collate utf8mb4_bin;
Query OK, 1 row affected (0.01 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| zabbix             |
+--------------------+
5 rows in set (0.00 sec)

mysql> create user zabbix@localhost identified by "8910jqkA@";
Query OK, 0 rows affected (0.02 sec)

mysql> grant all privileges on zabbix.* to zabbix@localhost;
Query OK, 0 rows affected (0.01 sec)

mysql> set global log_bin_trust_function_creators = 1;
Query OK, 0 rows affected, 1 warning (0.00 sec)
```

`**zcat /usr/share/zabbix/sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix**`

---



> 编辑server端配置,数据库端口，用户名以及密码
>

```bash
# /etc/zabbix/zabbix_server.conf
ListenPort=10051
LogFile=/var/log/zabbix/zabbix_server.log
LogFileSize=0
PidFile=/run/zabbix/zabbix_server.pid
SocketDir=/run/zabbix
DBHost=localhost
DBName=zabbix
DBUser=zabbix
DBPassword=8910jqkA@
SNMPTrapperFile=/var/log/snmptrap/snmptrap.log
Timeout=4
LogSlowQueries=3000
StatsAllowedIP=127.0.0.1
EnableGlobalScripts=0
Include=/etc/zabbix/zabbix_server.d/*.conf
```

---



> 重启zabbix-server.service
>

`**systemctl restart zabbix-server**`**  **重启service

`**systemctl enable zabbix-server**`**   **设置service自启动

---



> 配置nginx
>

```bash
#  取消/etc/nginx/conf.d/zabbix.conf中的注释
#  将通过8080端口访问zabbix前端页面
listen 8080;
server_name example.com;
```

---



> 编辑agent的配置
>

```bash
#  编辑Hostname , server(被动模式)，ServerActive（主动模式）
PidFile=/run/zabbix/zabbix_agentd.pid
LogFile=/var/log/zabbix/zabbix_agentd.log
LogFileSize=0
Server=127.0.0.1
ServerActive=127.0.0.1
Hostname=master
Include=/etc/zabbix/zabbix_agentd.d/*.conf
```

---



> ##### <font style="color:rgb(37, 40, 47);">启动Zabbix server和agent进程</font>

```bash
systemctl restart zabbix-agent zabbix-server nginx zabbix php-fpm
```

---



> 访问ip:8080 通过web端添加对agent的监控
>



## 使用zabbix_get监控agent的指标
> 安装`zabbix-get`
>

`sudo dnf install zabbix-get` 直接通过包管理工具安装

`zabbix_get -V` 验证版本

---



> 使用`zabbix_agentd`查看所有的监控项
>

`zabbix-agentd -p` 查看监控的项

<img src="https://cdn.nlark.com/yuque/0/2026/png/23135285/1776160285742-86c90f7b-88f8-469a-a15f-a357ccfded7e.png" width="332" title="" crop="0,0,1,1" id="udc422b7b" class="ne-image">

---



> 通过`zabbix_get` 查看该agent的具体的指标项
>

`zabbix_get -s <host> -p <port> -k <key>`

`zabbix_get -s 127.0.0.1 -k system.cpu.num` 打印当前机器上的cpu核心数

Agent监听关键点：

1. 默认配置ListenIP=127.0.0.1仅允许本地访问；若需远程zabbix_get调用，须将ListenIP改为本机实际IP（如10.0.0.13或10.0.0.16）；
2. 修改后需重启zabbix-agent服务并验证netstat -tnulp | grep 10050。
3. Zabbix Server Web界面中必须已添加对应主机条目，否则即使网络连通、Agent正常，zabbix_get也无法获取数据。

## 自定义监控项
1. 编写定制化监控脚本（如Shell/Python）；
2. 在zabbix_agentd.conf中通过UserParameter注册该脚本为新监控项；
3. 使用zabbix_get在Server端验证脚本能返回预期值；
4. 在Web界面中为该监控项创建图形、触发器与告警。
