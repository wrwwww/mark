以下是根据你目前的学习内容整理的一篇完整的 Nginx 安装与 systemctl 管理的复盘文档，涵盖了从源码下载到编译、安装、配置服务的一整套流程。  
[Nginx学习笔记(三)：核心配置文件详解_nginx.pid-CSDN博客](https://blog.csdn.net/baidu_38300480/article/details/122351161)

---

# 🧾 Nginx 安装与 systemctl 管理复盘文档
> 本文档总结了如何通过源码方式安装 Nginx，并使用 `systemctl` 实现服务管理的完整流程，适用于 Ubuntu / Debian / CentOS 等主流 Linux 系统。
>

---

## 📌 目录
1. [准备工作](https://chatgpt.com/c/688094fa-c378-8002-b105-5a434315fb59#%E5%87%86%E5%A4%87%E5%B7%A5%E4%BD%9C)
2. [下载 Nginx 源码](https://chatgpt.com/c/688094fa-c378-8002-b105-5a434315fb59#%E4%B8%8B%E8%BD%BD-nginx-%E6%BA%90%E7%A0%81)
3. [安装依赖](https://chatgpt.com/c/688094fa-c378-8002-b105-5a434315fb59#%E5%AE%89%E8%A3%85%E4%BE%9D%E8%B5%96)
4. [编译与安装](https://chatgpt.com/c/688094fa-c378-8002-b105-5a434315fb59#%E7%BC%96%E8%AF%91%E4%B8%8E%E5%AE%89%E8%A3%85)
5. [创建 systemd 服务](https://chatgpt.com/c/688094fa-c378-8002-b105-5a434315fb59#%E5%88%9B%E5%BB%BA-systemd-%E6%9C%8D%E5%8A%A1)
6. [使用 systemctl 管理 nginx](https://chatgpt.com/c/688094fa-c378-8002-b105-5a434315fb59#%E4%BD%BF%E7%94%A8-systemctl-%E7%AE%A1%E7%90%86-nginx)
7. [常见问题与解决](https://chatgpt.com/c/688094fa-c378-8002-b105-5a434315fb59#%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98%E4%B8%8E%E8%A7%A3%E5%86%B3)
8. [总结](https://chatgpt.com/c/688094fa-c378-8002-b105-5a434315fb59#%E6%80%BB%E7%BB%93)



---

## ✅ 准备工作
+ 确保系统联网
+ 使用普通用户运行编译操作，使用 `sudo` 进行安装和配置系统服务



```bash
sudo apt update        # Debian/Ubuntu
sudo yum update        # CentOS/RHEL
```

---

## 📥 下载 Nginx 源码
1. 访问官网：[https://nginx.org/en/download.html](https://nginx.org/en/download.html)
2. 选择最新稳定版本，比如 `nginx-1.26.0.tar.gz`



下载并解压：

```bash
wget https://nginx.org/download/nginx-1.26.0.tar.gz
tar zxvf nginx-1.26.0.tar.gz
cd nginx-1.26.0
```

---

## 🧱 安装依赖
Nginx 源码编译依赖以下库：

### Ubuntu / Debian
```bash
sudo apt install build-essential libpcre3 libpcre3-dev zlib1g zlib1g-dev libssl-dev
```

### CentOS / RHEL
```bash
sudo yum groupinstall "Development Tools"
sudo yum install pcre pcre-devel zlib zlib-devel openssl-devel
```

---

## 🔧 编译与安装
> 创建用户
>

创建系统用户nginx（无登录权限）：`useradd -r -s /sbin/nologin nginx`

### 1. 配置
关键参数：

`--prefix=/data/server/nginx`：指定安装根目录。

`--user=nginx --group=nginx`：指定运行用户与组。

`--with-http_stub_status_module`：启用stub_status模块（用于监控）。

```bash
./configure \
  --prefix=/usr/local/nginx \
  --with-http_ssl_module \
  --with-http_v2_module \
  --with-http_gzip_static_module
```

### 2. 编译（多核加速）
```bash
make -j"$(nproc)"
```

### 3. 安装
```bash
sudo make install
```

此时 Nginx 安装在 `/usr/local/nginx` 目录下。

---

## 🧪 测试启动
```bash
/usr/local/nginx/sbin/nginx       # 启动
ps aux | grep nginx               # 查看进程
curl http://localhost             # 验证页面
```

---

## 🧰 创建 systemd 服务
```bash
sudo nano /etc/systemd/system/nginx.service
```

写入以下内容：

```properties
[Unit]
Description=Nginx Web Server (manual build)
After=network.target

[Service]
Type=forking
ExecStart=/usr/local/nginx/sbin/nginx
ExecReload=/usr/local/nginx/sbin/nginx -s reload
ExecStop=/usr/local/nginx/sbin/nginx -s quit
PIDFile=/usr/local/nginx/logs/nginx.pid
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

重载 systemd 配置：

```bash
sudo systemctl daemon-reexec   # 或 daemon-reload
```

---

## 🎛 使用 systemctl 管理 nginx
| 命令 | 功能 |
| --- | --- |
| `sudo systemctl start nginx` | 启动服务 |
| `sudo systemctl stop nginx` | 停止服务 |
| `sudo systemctl restart nginx` | 重启服务 |
| `sudo systemctl reload nginx` | 平滑重载配置 |
| `sudo systemctl enable nginx` | 设置开机自启 |
| `sudo systemctl disable nginx` | 禁止开机自启 |
| `systemctl status nginx` | 查看服务状态 |
| `journalctl -u nginx` | 查看运行日志 |


---

## 🛠 常见问题与解决
### ❌ 错误：缺少 PCRE 库
```plain
./configure: error: the HTTP rewrite module requires the PCRE library
```

✅ 解决：

```bash
sudo apt install libpcre3 libpcre3-dev     # Ubuntu
sudo yum install pcre pcre-devel           # CentOS
```

---

### ❌ 错误：不支持 HTTPS
```plain
OpenSSL library is not used
```

✅ 解决：

```bash
sudo apt install libssl-dev
./configure --with-http_ssl_module
```

---

### ❌ `systemctl` 找不到服务
```plain
Unit nginx.service could not be found.
```

✅ 解决：

```bash
sudo nano /etc/systemd/system/nginx.service   # 创建服务
sudo systemctl daemon-reload
```

### 防火墙与SELinux适配
Rocky需临时禁用SELinux：setenforce 0，或永久修改/etc/selinux/config中SELINUX=disabled后重启。

Ubuntu需关闭UFW：ufw disable（因其策略粒度较粗，影响较小）。

---

## 📦 目录结构简要
| 路径 | 含义 |
| --- | --- |
| `/usr/local/nginx/sbin/nginx` | 主程序 |
| `/usr/local/nginx/conf/nginx.conf` | 配置文件 |
| `/usr/local/nginx/logs/` | 日志目录 |
| `/usr/local/nginx/html/` | 默认站点目录 |


---



# 📝 Nginx 基础配置示例
可以通过nginx -t 来查看nginx使用哪个目录下的配置

路径一般是 `/usr/local/nginx/conf/nginx.conf` 或 `/etc/nginx/nginx.conf`，根据你的安装路径调整。

```nginx
# 用户和工作进程数
user  www-data;  # 或 nginx，根据系统实际用户
worker_processes  auto;

# 错误日志路径及级别
error_log  /usr/local/nginx/logs/error.log  warn;

# 进程文件
pid        /usr/local/nginx/logs/nginx.pid;

events {
    worker_connections  1024;  # 单个 worker 最大连接数
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    # 日志格式
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /usr/local/nginx/logs/access.log  main;

    # 连接超时设置
    sendfile        on;
    tcp_nopush      on;
    tcp_nodelay     on;
    keepalive_timeout  65;
    types_hash_max_size 2048;

    # Gzip 压缩配置
    gzip  on;
    gzip_disable "msie6";
    gzip_vary on;
    gzip_proxied any;
    gzip_comp_level 6;
    gzip_buffers 16 8k;
    gzip_http_version 1.1;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

    # 服务器配置
    server {
        listen       80;
        server_name  localhost;

        # 默认主页路径
        root   /usr/local/nginx/html;
        index  index.html index.htm;

        # 错误页面
        error_page 404 /404.html;
        location = /404.html {
            internal;
        }

        # 静态文件访问
        location / {
            try_files $uri $uri/ =404;
        }

        # 反向代理示例
        # location /api/ {
        #     proxy_pass http://127.0.0.1:8080/;
        #     proxy_set_header Host $host;
        #     proxy_set_header X-Real-IP $remote_addr;
        #     proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        # }

        # 其他配置，比如访问限制、缓存等可以继续补充
    }
}
```

---

## 配置说明
+ **worker_processes auto**：自动根据 CPU 核心数设置工作进程数
+ **error_log 和 access_log**：错误日志和访问日志路径及格式
+ **events**：设置单个进程最大连接数
+ **http**：HTTP 服务器配置，包含 MIME 类型、压缩、连接优化等
+ **gzip**：开启压缩，提高页面传输效率
+ **server**：
    - 监听 80 端口
    - `root` 指定网站根目录
    - `index` 指定默认首页
    - `location /` 用于访问静态文件
    - 可配置反向代理（注释部分）



---

## 启用配置并测试
1. **检测配置文件语法**



```bash
/usr/local/nginx/sbin/nginx -t
```

2. **重新加载配置**



```bash
sudo systemctl reload nginx
# 或者
/usr/local/nginx/sbin/nginx -s reload
```

---

## 扩展建议
+ 配置 SSL（HTTPS）
+ 配置反向代理和负载均衡
+ 配置缓存和限速
+ 配合 `systemctl` 管理服务



---

需要我帮你写一个 **支持 HTTPS 的 nginx.conf** 示例，或者包含反向代理、负载均衡的更复杂配置吗？

### 默认配置
```plain

#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}

```

### 配置https通过自签ssl
> 生成自签名得ssl证书
>

```nginx


#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    server {
        listen       80;
        server_name  localhost;

    return 301 https://$host$request_uri;
        #charset koi8-r;

        #access_log  logs/host.access.log  main;

#        location / {
 #           root   html;
  #          index  index.html index.htm;
       # }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
       # error_page   500 502 503 504  /50x.html;
        #location = /50x.html {
        #    root   html;
        #}

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    
    server {
        listen       443 ssl;
        server_name  localhost;

        ssl_certificate      /usr/local/nginx/example.com.crt;
        ssl_certificate_key  /usr/local/nginx/example.com.key;

        ssl_session_cache    shared:SSL:1m;
        ssl_session_timeout  5m;

        ssl_ciphers  HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers  on;

        location / {
            root   html;
            index  index.html index.htm;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }

}


```

好的，下面是完整地用 `openssl` 自签发 SSL 证书的步骤，非常适合你测试 Nginx 的 HTTPS 配置。

---

## ✅ 一、生成自签名 SSL 证书（`openssl`）
### 1. 创建一个私钥（Private Key）
```bash
openssl genrsa -out example.com.key 2048
```

+ 生成一个 2048 位的私钥
+ 文件名：`example.com.key`



---

### 2. 创建一个证书签名请求（CSR）
```bash
openssl req -new -key example.com.key -out example.com.csr
```

会要求输入一些信息，比如：

+ 国家代码（Country Name）：CN
+ 省份（State）：Beijing
+ 组织名（Organization Name）：Test Org
+ Common Name：**你的域名（如 example.com）或 IP 地址**



💡 **Common Name 非常重要，要填你访问时用的域名或 IP。**

---

### 3. 自签发证书（有效期 365 天）
```bash
openssl x509 -req -days 365 -in example.com.csr -signkey example.com.key -out example.com.crt
```

得到：

+ 公钥证书：`example.com.crt`
+ 私钥：`example.com.key`



你可以用这两个文件配置 Nginx 的 HTTPS。

---

## ✅ 二、Nginx 配置（示例）
```nginx
server {
    listen 443 ssl;
    server_name localhost;

    ssl_certificate     /etc/nginx/ssl/example.com.crt;
    ssl_certificate_key /etc/nginx/ssl/example.com.key;

    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;

    location / {
        root /usr/share/nginx/html;
        index index.html;
    }
}
```

你需要将 `.crt` 和 `.key` 文件放到合适的目录（比如 `/etc/nginx/ssl/`），并确保 Nginx 有权限访问它们。

---

## ✅ 三、测试自签证书
```bash
sudo nginx -t
sudo nginx -s reload
```

然后在浏览器中访问 `https://localhost/` 或 `https://your_ip/`

+ 浏览器会提示“不受信任的证书”，点击继续即可（因为是自签的，没有受信机构认证）。
+ 你可以用 `curl -k https://localhost` 测试（`-k` 表示忽略证书校验）。



---

## 源码安装与清理优势
### 1.1 源码安装的可清理性
源码编译安装的 Nginx 可实现彻底清理，步骤明确：

| 步骤 | 命令 | 说明 |
| --- | --- | --- |
| 停止服务 | `nginx -s stop` | 停止 Nginx 进程 |
| 禁用服务 | `systemctl disable nginx` | 取消开机自启 |
| 删除主程序 | `rm -f /usr/sbin/nginx` | 删除执行文件 |
| 删除安装目录 | `rm -rf /usr/local/nginx` | 删除全部安装文件 |
| 清理源码包 | `rm -f *.tar.gz` | 删除下载的源码包 |


### 1.2 二进制包清理的局限性
使用 APT/YUM 安装时存在以下问题：

+ 文件分散于多路径：`/usr/bin`、`/etc/nginx`、`/var/log/nginx` 等
+ `apt remove` 或 `yum remove` 会产生残留文件
+ 被进程占用的残留文件无法删除
+ 无依赖关系的文件亦会残留，干扰后续安装

---

## 第三方模块集成方式
### 2.1 ADD_MODULE（编译内嵌模式）
+ 模块源码直接编译进 `nginx` 执行文件
+ 启动后即生效，无需额外配置启用
+ 模块功能成为 Nginx 命令的原生能力

### 2.2 ADD_DYNAMIC_MODULE（动态加载模式）
+ 模块以独立 `.so` 文件存在，不编译进主程序
+ 需在配置文件中通过 `load_module` 指令显式加载
+ 未加载则模块不生效

---

## 三、常用第三方模块示例
| 模块名称 | 功能说明 |
| --- | --- |
| `ngx_http_echo_module` | 添加 `echo` 指令，在响应体中输出指定字符串 |
| `ngx_http_security_headers_module` | 提供 Web 安全头配置（X-Content-Type-Options、X-Frame-Options 等） |
| `ngx_http_redis_module` | 将请求代理至 Redis 服务器，实现键值存储交互 |
| `ngx_http_flv_module` | 支持 FLV 视频流媒体的渐进式播放与伪流式传输 |
| 一致性哈希模块 | 实现基于 key 的稳定后端节点分配，保障缓存命中率 |


---

## 四、模块扩展实操流程
### 4.1 版本与环境严格一致性要求
+ 新增模块必须基于**当前运行版本**的源码重新编译
+ `nginx -V` 输出的 `--configure arguments` 必须**完全复用**
+ 编译环境（gcc、perl-devel、openssl-devel 等）需与原始安装一致

### 4.2 模块源码准备与路径配置
```bash
# 下载对应版本源码
wget https://nginx.org/download/nginx-1.24.0.tar.gz

# 下载模块源码（以 echo 模块为例）
# 将模块目录移至自定义路径
mv echo-nginx-module /usr/local/nginx/modules/echo
```

### 4.3 编译参数追加与构建
```bash
# 在原始 ./configure 参数后追加模块
./configure [原始参数] --add-module=/usr/local/nginx/modules/echo

# 执行编译（严禁 make install）
make clean    # 清理旧构建状态
./configure ... # 重新配置
make          # 生成新 nginx 二进制文件
```

### 4.4 依赖库缺失处理
编译报错 `libperl.so` 缺失时的解决方案：

```bash
# 定位库文件
find /usr -name "libperl.so*"

# 创建符号链接
ln -s /usr/lib/x86_64-linux-gnu/libperl.so.5.38 /usr/lib/libperl.so
```

---

## 五、执行文件覆盖升级规范
### 5.1 覆盖操作步骤
```bash
# 备份旧执行文件
mv /usr/sbin/nginx /usr/sbin/nginx-1.24.0.bak

# 覆盖新执行文件
mv objs/nginx /usr/sbin/nginx
```

> ⚠️ **注意**：保留原配置文件不动，仅替换执行文件。
>

### 5.2 验证与测试
```bash
# 验证配置语法
nginx -t

# 确认版本及新增模块
nginx -V

# 测试 echo 模块
# 在 nginx.conf 的 server 块中添加：
location /test {
    echo "hello";
}
# 重启后访问验证响应
```

---

## 六、Nginx 配置文件结构
### 6.1 配置层级概览
```plain
全局配置段 (main)
├── events 配置段
├── http 配置段
│   ├── server 配置段
│   │   └── location 配置段
│   └── ...
├── stream 配置段
└── mail 配置段
```

### 6.2 各配置段详解
| 配置段 | 位置 | 关键指令 | 说明 |
| --- | --- | --- | --- |
| **全局段 (main)** | 最外层 | `user`、`worker_processes`、`pid`、`error_log`、`include` | 控制主进程行为，失效则服务无法启动 |
| **events 段** | 全局内 | `worker_connections 1024`、`use epoll`、`worker_cpu_affinity` | 定义事件驱动模型 |
| **http 段** | 全局内 | `sendfile on`、`tcp_nopush on`、`keepalive_timeout 65`、`gzip on`、`charset utf-8` | 处理 HTTP/HTTPS 协议 |
| **server 段** | http 内 | `listen 80`、`server_name`、`root`、`index` | 定义虚拟主机 |
| **location 段** | server 内 | URI 匹配规则（支持正则 `~ \.php$`） | 精细化资源路由与处理 |
| **stream 段** | 全局内 | TCP/UDP 代理配置 | 四层代理，需编译时启用 `--with-stream` |
| **mail 段** | 全局内 | 邮件代理配置 | 处理 POP3/IMAP/SMTP 协议 |


---

## 七、配置语法核心规则
### 7.1 指令格式
| 规则 | 示例 |
| --- | --- |
| 每条指令以分号 `;` 结尾 | `worker_processes auto;` |
| 指令块以大括号 `{}` 包裹 | `http { ... }` |


### 7.2 include 机制
```nginx
include /etc/nginx/conf.d/*.conf;
```

+ 将匹配文件内容导入当前配置段
+ `include` 位置决定导入作用域（全局、http、server 等）

### 7.3 变量与注释
| 符号 | 含义 | 示例 |
| --- | --- | --- |
| `$` | 变量前缀 | `$host`、`$request_uri` |
| `#` | 注释行 | `# 这是注释` |
| `set` | 自定义变量 | `set $var value;` |




# NGINX 核心配置结构详解
## 一、学习方法与理念
### 1.1 核心学习原则
| 原则 | 说明 |
| --- | --- |
| 动手实践 | 操作时产生的"误会"即是学习契机，异常发现能引出注意事项 |
| 聚焦高频 | 低频属性仅保留课件参考，教学聚焦高价值、高频场景 |
| 理解场景 | 命令与属性本身不重要，**使用场景与目的**才是关键 |


### 1.2 知识边界
+ 课时限制，大量低频属性不逐个讲解
+ 课件保留完整属性供参考查阅
+ 重点掌握核心内容的**使用逻辑**而非记忆所有参数

---

## 二、NGINX 配置文件核心结构
### 2.1 配置段层级关系
```plain
全局配置段 (global)
    │
    ▼
events 配置段
    │
    ▼
http 配置段
    │
    ├── server 配置段
    │       │
    │       ▼
    │   location 配置段
    │
    ├── server 配置段
    └── ...
```

> ⚠️ **重要**：层级顺序不可颠倒，这是理解配置生效范围的基础。
>

### 2.2 核心配置段功能定位
| 配置段 | 功能说明 |
| --- | --- |
| **global** | 影响整个 NGINX 进程的全局参数 |
| **events** | 定义连接处理模型（如 `worker_connections`） |
| **http** | 封装所有 HTTP 服务相关配置，是 server 的直接父容器 |
| **server** | 定义一个独立 Web 站点，对应监听地址（IP+端口）及域名 |
| **location** | 定义 server 内针对特定 URI 路径的处理规则，资源级精细化控制 |


---

## 三、server 配置段详解
### 3.1 server 核心作用
server 块代表一个**独立的 Web 站点**，本质是将网络流量按监听地址和域名路由到指定的静态或动态资源集合。

### 3.2 关键指令解析
| 指令 | 格式 | 说明 |
| --- | --- | --- |
| `listen` | `listen [address:]port [default_server] [ssl] [http2]` | 指定监听 IP 地址与端口 |
| `server_name` | `server_name domain1 domain2;` | 定义该 server 响应的域名列表 |
| `root` | `root /path/to/root;` | 设定 Web 根目录，URI 相对路径的基准 |
| `index` | `index index.html index.htm;` | 默认首页文件列表，按顺序尝试匹配 |


### 3.3 虚拟主机三种实现方式
#### 3.3.1 基于端口的虚拟主机
```nginx
server {
    listen 81;
    root /var/www/site81;
}

server {
    listen 82;
    root /var/www/site82;
}
```

**验证方法**：

```bash
nginx -t                              # 验证语法
systemctl restart nginx               # 重启服务
netstat -tunlp | grep nginx           # 确认多端口监听
```

#### 3.3.2 基于 IP 的虚拟主机
```nginx
server {
    listen 10.0.0.186:80;
    root /var/www/site186;
}

server {
    listen 10.0.0.187:80;
    root /var/www/site187;
}
```

> ⚠️ **注意**：需为网卡绑定多个 IP；在云主机等受限环境中实用性低，多用于内部测试。
>

#### 3.3.3 基于域名的虚拟主机
```nginx
server {
    listen 80;
    server_name www.a.com;
    root /var/www/a;
}

server {
    listen 80;
    server_name www.b.com;
    root /var/www/b;
}

server {
    listen 80 default_server;
    server_name _;
    root /var/www/default;
}
```

**关键机制**：

+ `default_server`：当无匹配 server 时的默认接收者，类比"嫡子"优先继承流量
+ 直接用 IP 访问的请求由 `default_server` 处理

**验证方法**：

```bash
# 方法一：curl 指定 Host 头
curl -H "Host: www.a.com" http://10.0.0.113

# 方法二：修改 /etc/hosts 模拟 DNS
echo "10.0.0.113 www.a.com" >> /etc/hosts
curl http://www.a.com
```

---

## 四、location 配置段详解
### 4.1 location 核心作用
location 块位于 server 内，用于匹配客户端请求的 **URI 路径**（即 `$uri`），并定义该路径下资源的处理逻辑。

### 4.2 匹配类型与优先级
| 匹配类型 | 语法 | 优先级 | 说明 |
| --- | --- | --- | --- |
| **精确匹配** | `location = /path` | 最高 | 仅当 URI 完全等于 `/path` 时触发 |
| **正则匹配** | `location ~ \.png$` | 中 | `~` 区分大小写，`~*` 不区分大小写 |
| **前缀匹配** | `location ^~ /path` | 较高 | 匹配后不再检查正则 |
| **普通前缀匹配** | `location /path` | 最低 | 匹配所有以 `/path` 开头的 URI |


### 4.3 匹配优先级规则
```plain
1. 精确匹配 (=)           → 命中即停止
2. 前缀匹配 (^~)          → 命中即停止，不再检查正则
3. 正则匹配 (~ / ~*)      → 按配置顺序，首个匹配成功即停止
4. 普通前缀匹配           → 选择最长匹配的 location
```

### 4.4 关键指令功能
| 指令 | 功能 | 示例 |
| --- | --- | --- |
| `alias` | 指定别名路径，URI 匹配部分被替换 | `location /images/ { alias /data/images/; }` |
| `try_files` | 按顺序检查文件/目录存在性，首个存在者返回 | `try_files $uri $uri/ =404;` |
| `proxy_pass` | 反向代理至后端服务器 | `proxy_pass http://backend;` |
| `fastcgi_pass` | 通过 FastCGI 协议转发动态请求 | `fastcgi_pass 127.0.0.1:9000;` |
| `allow` / `deny` | 基于 CIDR 网段控制访问权限 | `allow 192.168.1.0/24; deny all;` |


### 4.5 alias 与 root 的区别
```nginx
# 请求 /images/logo.png
location /images/ {
    root /data/;           # 实际路径: /data/images/logo.png
}

location /images/ {
    alias /data/images/;   # 实际路径: /data/images/logo.png
}
```

> **区别**：`alias` 会丢弃匹配的 URI 前缀，`root` 会追加完整的 URI 路径。
>

### 4.6 try_files 请求处理逻辑
```nginx
location / {
    try_files $uri $uri/ /index.php?$args;
}
```

**执行顺序**：

1. 尝试 `$uri` 作为文件
2. 尝试 `$uri/` 作为目录
3. 均失败则转发至 `/index.php?$args`

这是 NGINX 处理静态请求的**核心流程**。

### 4.7 allow/deny 执行顺序
```nginx
# ✅ 正确：先放行后拒绝
allow 192.168.1.0/24;
deny all;

# ❌ 错误：deny all 置于顶部会导致后续规则失效
deny all;
allow 192.168.1.0/24;  # 永远不会执行
```

> ⚠️ **原则**：自上而下匹配，需遵循"先放行后拒绝"。
>

---

## 五、跨系统配置差异
### 5.1 Ubuntu vs Rocky 配置组织
| 发行版 | 安装方式 | 配置组织特点 |
| --- | --- | --- |
| **Ubuntu** | APT | http 与 server 配置拆分：主配置仅含全局与 events，站点配置在 `/etc/nginx/sites-enabled/` |
| **Rocky** | YUM/RPM | 配置集中于 `/etc/nginx/conf.d/default.conf`，结构更紧凑 |


### 5.2 配置一致性保证
+ 所有 NGINX 配置指令的**语法、语义与生效逻辑完全一致**
+ 掌握核心配置段结构后，可在任意 Linux 发行版中无缝迁移操作

---

## 六、错误日志与问题定位
### 6.1 错误日志关键字
| 级别 | 说明 |
| --- | --- |
| `emerg` | 紧急，系统不可用 |
| `alert` | 告警，需立即处理 |
| `crit` | 严重错误 |
| `error` | **最常见**，直接关联配置语法或路径权限问题 |
| `warn` | 警告，不影响服务但建议关注 |


### 6.2 配置验证与热重载
```bash
# 验证配置语法
nginx -t

# 平滑重载（无需停机）
systemctl reload nginx
# 或
nginx -s reload
```

> 💡 **建议**：每次修改配置后先执行 `nginx -t` 验证，避免因错误配置导致服务中断。
>

# Nginx 核心运维配置原理与实践
## 一、reload 与 restart 命令的本质区别
### 1.1 核心定义对比
| 命令 | 机制 | 业务影响 | 适用场景 |
| --- | --- | --- | --- |
| **reload** | 进程持续运行，新配置加载进内存 | 无服务中断，请求不丢失 | 生产环境配置热更新 |
| **restart** | 先终止进程，再启动新进程 | 正在处理的请求被强制中断 | 首次部署或调试阶段 |


### 1.2 业务影响深度分析
#### restart 的风险场景
```plain
用户请求处理中 → restart 执行 → 进程终止 → 请求中断
```

+ 银行取款交易中途断开 → 数据不一致
+ 文件上传中途断开 → 文件损坏
+ 用户体验灾难

#### reload 的安全机制
+ Master 进程重新读取配置
+ 创建新 Worker 进程接管新请求
+ 旧 Worker 处理完存量请求后优雅退出

### 1.3 适用时机决策表
| 场景 | 推荐命令 | 说明 |
| --- | --- | --- |
| 首次部署 | restart | 无真实流量，可接受中断 |
| 业务运行中配置调整 | reload | 保障服务连续性 |
| 日志切割（维护窗口期） | restart | 凌晨操作，影响最小 |
| 业务高峰期 | **禁止 restart** | 必须 reload 且提前规划 |
| 多站点环境 | reload | 仅刷新当前配置，其他站点不受影响 |


### 1.4 高级限制说明
> ⚠️ **重要限制**：reload 仅对普通配置项生效
>

| 配置类型 | reload 生效 | 必须 restart |
| --- | --- | --- |
| location 块 | ✅ 生效 | - |
| root 指令 | ✅ 生效 | - |
| server 块增删 | ✅ 生效 | - |
| HTTP 段 `log_format` | ❌ 不生效 | ✅ 必须 restart |
| `error_log` 路径 | ❌ 不生效 | ✅ 必须 restart |


---

## 二、root 与 alias 路径映射机制详解
### 2.1 基础定义对比
| 特性 | root | alias |
| --- | --- | --- |
| **映射方式** | 相对路径拼接 | 绝对路径替换 |
| **URI 处理** | 完整 URI 追加至 root 路径 | 匹配部分被剥离，剩余部分追加 |
| **路径暴露** | 暴露真实目录结构 | 可隐藏内部路径，提升安全性 |
| **末尾斜杠** | 影响较小 | **至关重要**，行为差异大 |


### 2.2 路径拼接逻辑图解
#### root 示例
```nginx
location /images/ {
    root /data/www;
}
```

```plain
请求: /images/logo.png
     ↓
拼接: /data/www + /images/logo.png
     ↓
结果: /data/www/images/logo.png
```

#### alias 示例
```nginx
location /images/ {
    alias /data/assets/;
}
```

```plain
请求: /images/logo.png
     ↓
剥离: /images/ 被移除，剩余 logo.png
     ↓
拼接: /data/assets/ + logo.png
     ↓
结果: /data/assets/logo.png
```

### 2.3 关键易错点
#### alias 末尾斜杠的重要性
```nginx
# ✅ 正确：带斜杠
location /web2/ {
    alias /data/server/;      # 请求 /web2/index.html → /data/server/index.html
}

# ❌ 错误：不带斜杠
location /web2/ {
    alias /data/server;       # 请求 /web2/index.html → /data/serverindex.html（路径错位）
}
```

### 2.4 实践验证方法
| location 配置 | 请求 URI | 实际查找路径 |
| --- | --- | --- |
| `location /web2 { alias /data/server/; }` | `/web2/index.html` | `/data/server/index.html` |
| `location /web2 { root /data/www; }` | `/web2/index.html` | `/data/www/web2/index.html` |


> 💡 **学习建议**：此知识点"易学难精"，需通过多次手动配置-验证循环深化理解，每次成功后务必回溯路径拼接逻辑。
>

---

## 三、location 匹配优先级规则
### 3.1 优先级顺序（由高到低）
```plain
1. 精确匹配 (=)          location = /api/status
        ↓
2. 前缀匹配 (^~)         location ^~ /images/
        ↓
3. 正则匹配 (~ / ~*)     location ~ \.php$
        ↓
4. 普通前缀匹配          location /app/
        ↓
5. 通用匹配 (/)          location /
```

### 3.2 匹配类型详解
| 匹配类型 | 语法 | 优先级 | 说明 |
| --- | --- | --- | --- |
| **精确匹配** | `location = /` | 最高 | 仅匹配根路径，多一个字符即失效 |
| **正则匹配（区分大小写）** | `location ~ \.png$` | 中高 | PCRE 正则 |
| **正则匹配（不区分大小写）** | `location ~* \.(gif|jpg|jpeg)$` | 中 | 常用于静态资源 |
| **前缀匹配（优先）** | `location ^~ /images/` | 中 | 匹配后不再检查正则 |
| **普通前缀匹配** | `location /images/` | 较低 | 最长字符串匹配 |
| **通用匹配** | `location /` | 最低 | 兜底规则 |


### 3.3 匹配逻辑示例
```nginx
location /images/ {           # 前缀匹配
    root /data;
}

location ~ \.(gif|png)$ {     # 正则匹配
    root /data/static;
}

location / {                  # 通用匹配
    root /data/default;
}
```

```plain
请求: /images/test.png
     ↓
同时满足: /images/（前缀）、\.png$（正则）、/（通用）
     ↓
优先级判断: 正则 > 前缀 > 通用
     ↓
最终执行: 正则匹配规则
```

### 3.4 兜底规则必要性
> ⚠️ `/` 规则必须存在，用于处理未被显式规则覆盖的异常或未知请求，确保服务健壮性。
>

---

## 四、error_page 与内部重定向
### 4.1 核心机制
```nginx
error_page 404 /error;
```

+ 将 404 状态重定向至内部 URI `/error`
+ 触发 `location /error` 块处理
+ **内部重定向**：非 HTTP 302 跳转，客户端无感知，URL 栏不变

### 4.2 定制化错误页
```nginx
error_page 404 /custom_404;

location /custom_404 {
    return 404 "Custom Not Found - The requested resource does not exist.";
}
```

### 4.3 关键约束
| 约束项 | 说明 |
| --- | --- |
| 作用域 | server 或 location |
| 目标要求 | 内部重定向目标必须有对应 location 块定义 |
| 状态码范围 | 支持 4xx、5xx 等 HTTP 错误码 |


---

## 五、访问控制（allow/deny）
### 5.1 指令顺序强制性
> ⚠️ **关键原则**：allow 与 deny 按配置顺序**自上而下逐条检查**
>

```nginx
# ✅ 正确：先放行后拒绝
allow 192.168.1.0/24;
deny all;

# ❌ 错误：deny all 置于顶部会导致后续规则失效
deny all;
allow 192.168.1.0/24;  # 永远不会执行
```

### 5.2 典型配置示例
```nginx
# 仅允许内网访问
location /admin/ {
    allow 192.168.1.0/24;
    allow 10.0.0.0/8;
    deny all;
}

# 禁止特定 IP
location /api/ {
    deny 192.168.1.100;
    allow all;
}
```

---

## 六、日志定制体系
### 6.1 三级配置模型
```plain
全局格式定义 (http 段)
    ↓
Server 级路径与格式绑定
    ↓
Location 级覆盖（可选）
```

### 6.2 配置示例
```nginx
http {
    # 1. 全局格式定义
    log_format main '$remote_addr - $remote_user [$time_local] '
                    '"$request" $status $body_bytes_sent '
                    '"$http_referer" "$http_user_agent"';
    
    log_format json escape=json '{'
        '"time":"$time_iso8601",'
        '"remote_addr":"$remote_addr",'
        '"request":"$request",'
        '"status":$status'
        '}';
    
    server {
        # 2. Server 级日志
        access_log /var/log/nginx/access.log main;
        error_log /var/log/nginx/error.log warn;
        
        location /test/ {
            # 3. Location 级覆盖
            access_log off;                                    # 禁用日志
            # 或
            access_log /var/log/nginx/test.log json;           # 独立日志文件
        }
    }
}
```

### 6.3 日志格式变量参考
| 变量 | 含义 |
| --- | --- |
| `$remote_addr` | 客户端 IP 地址 |
| `$remote_user` | HTTP 认证用户名 |
| `$time_local` | 本地时间 |
| `$request` | 请求行（方法 + URI + 协议） |
| `$status` | 响应状态码 |
| `$body_bytes_sent` | 响应体字节数 |
| `$http_referer` | 来源页面 URL |
| `$http_user_agent` | 客户端浏览器信息 |


### 6.4 动态日志路径
```nginx
# 按域名自动分文件
access_log /var/log/nginx/$host-access.log main;
```

### 6.5 权限要求
> ⚠️ **关键**：Nginx Worker 进程必须对日志目录具备写权限
>

```bash
# 确保权限正确
chown -R nginx:nginx /var/log/nginx
chmod 755 /var/log/nginx
```

---

## 七、核心要点速查
| 知识点 | 关键要点 |
| --- | --- |
| **reload vs restart** | 生产环境用 reload，全局配置变更需 restart |
| **root vs alias** | root 拼接完整路径，alias 剥离匹配前缀后拼接 |
| **alias 斜杠** | 末尾斜杠至关重要，决定路径是否正确拼接 |
| **location 优先级** | 精确 > 前缀优先 > 正则 > 普通前缀 > 通用 |
| **error_page** | 内部重定向，客户端无感知，目标需有对应 location |
| **allow/deny** | 自上而下匹配，先放行后拒绝 |
| **log_format** | http 段定义，reload 不生效，需 restart |

