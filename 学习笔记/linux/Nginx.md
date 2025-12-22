以下是根据你目前的学习内容整理的一篇完整的 Nginx 安装与 systemctl 管理的复盘文档，涵盖了从源码下载到编译、安装、配置服务的一整套流程。  
[Nginx学习笔记(三)：核心配置文件详解_nginx.pid-CSDN博客](https://blog.csdn.net/baidu_38300480/article/details/122351161)

---

# 🧾 Nginx 安装与 systemctl 管理复盘文档

本文档总结了如何通过源码方式安装 Nginx，并使用 `systemctl` 实现服务管理的完整流程，适用于 Ubuntu / Debian / CentOS 等主流 Linux 系统。

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

- 确保系统联网

- 使用普通用户运行编译操作，使用 `sudo` 进行安装和配置系统服务

  

```
sudo apt update        # Debian/Ubuntu
sudo yum update        # CentOS/RHEL
```

---

## 📥 下载 Nginx 源码

1. 访问官网：[https://nginx.org/en/download.html](https://nginx.org/en/download.html)

2. 选择最新稳定版本，比如 `nginx-1.26.0.tar.gz`

  

下载并解压：

```
wget https://nginx.org/download/nginx-1.26.0.tar.gz
tar zxvf nginx-1.26.0.tar.gz
cd nginx-1.26.0
```

---

## 🧱 安装依赖

Nginx 源码编译依赖以下库：

### Ubuntu / Debian

```
sudo apt install build-essential libpcre3 libpcre3-dev zlib1g zlib1g-dev libssl-dev
```

### CentOS / RHEL

```
sudo yum groupinstall "Development Tools"
sudo yum install pcre pcre-devel zlib zlib-devel openssl-devel
```

---

## 🔧 编译与安装

### 1. 配置

```
./configure \
  --prefix=/usr/local/nginx \
  --with-http_ssl_module \
  --with-http_v2_module \
  --with-http_gzip_static_module
```

### 2. 编译（多核加速）

```
make -j"$(nproc)"
```

### 3. 安装

```
sudo make install
```

此时 Nginx 安装在 `/usr/local/nginx` 目录下。

---

## 🧪 测试启动

```
/usr/local/nginx/sbin/nginx       # 启动
ps aux | grep nginx               # 查看进程
curl http://localhost             # 验证页面
```

---

## 🧰 创建 systemd 服务

```
sudo nano /etc/systemd/system/nginx.service
```

写入以下内容：

```
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

```
sudo systemctl daemon-reexec   # 或 daemon-reload
```

---

## 🎛 使用 systemctl 管理 nginx

|命令|功能|
|---|---|
|`sudo systemctl start nginx`|启动服务|
|`sudo systemctl stop nginx`|停止服务|
|`sudo systemctl restart nginx`|重启服务|
|`sudo systemctl reload nginx`|平滑重载配置|
|`sudo systemctl enable nginx`|设置开机自启|
|`sudo systemctl disable nginx`|禁止开机自启|
|`systemctl status nginx`|查看服务状态|
|`journalctl -u nginx`|查看运行日志|

---

## 🛠 常见问题与解决

### ❌ 错误：缺少 PCRE 库

```
./configure: error: the HTTP rewrite module requires the PCRE library
```

✅ 解决：

```
sudo apt install libpcre3 libpcre3-dev     # Ubuntu
sudo yum install pcre pcre-devel           # CentOS
```

---

### ❌ 错误：不支持 HTTPS

```
OpenSSL library is not used
```

✅ 解决：

```
sudo apt install libssl-dev
./configure --with-http_ssl_module
```

---

### ❌ `systemctl` 找不到服务

```
Unit nginx.service could not be found.
```

✅ 解决：

```
sudo nano /etc/systemd/system/nginx.service   # 创建服务
sudo systemctl daemon-reload
```

---

## 📦 目录结构简要

|路径|含义|
|---|---|
|`/usr/local/nginx/sbin/nginx`|主程序|
|`/usr/local/nginx/conf/nginx.conf`|配置文件|
|`/usr/local/nginx/logs/`|日志目录|
|`/usr/local/nginx/html/`|默认站点目录|

---

## ✅ 总结

- 从源码编译 nginx 可以灵活控制模块（如 SSL、HTTP/2、gzip）

- 使用 `make -j$(nproc)` 可加速编译

- `make` 是编译，`make install` 是安装

- 建议通过 `systemd` 创建服务文件，实现标准的服务管理

- 配合 `systemctl` 可以轻松控制 nginx 的启动、重载和开机启动

  

---

如果你需要，我可以继续为你生成：

- 完整 nginx.conf 示例

- HTTPS + 自签证书配置

- 启用 gzip、反向代理、限速等模块配置

  

---

# 📝 Nginx 基础配置示例

路径一般是 `/usr/local/nginx/conf/nginx.conf` 或 `/etc/nginx/nginx.conf`，根据你的安装路径调整。

```
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

- **worker_processes auto**：自动根据 CPU 核心数设置工作进程数

- **error_log 和 access_log**：错误日志和访问日志路径及格式

- **events**：设置单个进程最大连接数

- **http**：HTTP 服务器配置，包含 MIME 类型、压缩、连接优化等

- **gzip**：开启压缩，提高页面传输效率

- **server**：

- 监听 80 端口

- `root` 指定网站根目录

- `index` 指定默认首页

- `location /` 用于访问静态文件

- 可配置反向代理（注释部分）

  

---

## 启用配置并测试

1. **检测配置文件语法**

  

```
/usr/local/nginx/sbin/nginx -t
```

2. **重新加载配置**

  

```
sudo systemctl reload nginx
# 或者
/usr/local/nginx/sbin/nginx -s reload
```

---

## 扩展建议

- 配置 SSL（HTTPS）

- 配置反向代理和负载均衡

- 配置缓存和限速

- 配合 `systemctl` 管理服务

  

---

需要我帮你写一个 **支持 HTTPS 的 nginx.conf** 示例，或者包含反向代理、负载均衡的更复杂配置吗？

### 默认配置

```

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

生成自签名得ssl证书

```


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

```
openssl genrsa -out example.com.key 2048
```

- 生成一个 2048 位的私钥

- 文件名：`example.com.key`

  

---

### 2. 创建一个证书签名请求（CSR）

```
openssl req -new -key example.com.key -out example.com.csr
```

会要求输入一些信息，比如：

- 国家代码（Country Name）：CN

- 省份（State）：Beijing

- 组织名（Organization Name）：Test Org

- Common Name：**你的域名（如 example.com）或 IP 地址**

  

💡 **Common Name 非常重要，要填你访问时用的域名或 IP。**

---

### 3. 自签发证书（有效期 365 天）

```
openssl x509 -req -days 365 -in example.com.csr -signkey example.com.key -out example.com.crt
```

得到：

- 公钥证书：`example.com.crt`

- 私钥：`example.com.key`

  

你可以用这两个文件配置 Nginx 的 HTTPS。

---

## ✅ 二、Nginx 配置（示例）

```
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

```
sudo nginx -t
sudo nginx -s reload
```

然后在浏览器中访问 `https://localhost/` 或 `https://your_ip/`

- 浏览器会提示“不受信任的证书”，点击继续即可（因为是自签的，没有受信机构认证）。

- 你可以用 `curl -k https://localhost` 测试（`-k` 表示忽略证书校验）。

  

---