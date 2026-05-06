---
title: "Nginx 基础配置完全指南：从入门到实战"
date: 2026-05-06T06:57:29+08:00
draft: false
categories: ["技术教程", "服务器运维"]
tags: ["Nginx", "Web 服务器", "反向代理", "负载均衡", "Linux"]
description: "全面解析 Nginx 服务器的基础配置，涵盖安装、核心配置、虚拟主机、反向代理、负载均衡等关键知识点，帮助开发者快速掌握 Nginx 配置技能。"
---

# Nginx 基础配置完全指南：从入门到实战

作为全球最流行的 Web 服务器之一，Nginx 以其高性能、稳定性和丰富的功能特性，成为现代 Web 架构中不可或缺的基础设施。无论是作为静态文件服务器、反向代理，还是负载均衡器，Nginx 都能出色地完成任务。本文将带你从零开始，系统学习 Nginx 的基础配置。

## 一、为什么选择 Nginx？

### 1. 卓越的性能表现

- **高并发处理能力**：采用事件驱动的异步架构，单台服务器可支持数万并发连接
- **低内存占用**：相比 Apache 等传统 Web 服务器，内存消耗更低
- **静态文件处理**：静态内容传输性能优异，可直接提供 HTML、CSS、JS、图片等资源

### 2. 多功能集成

```bash
# Nginx 可以胜任的多种角色
✓ Web 服务器          - 提供静态和动态内容
✓ 反向代理服务器      - 转发请求到后端应用
✓ 负载均衡器          - 分发流量到多个服务器
✓ HTTP 缓存服务器     - 加速内容访问
✓ SSL/TLS 终止        - 处理 HTTPS 加密
✓ API 网关            - 统一管理微服务入口
```

### 3. 广泛的应用场景

根据 W3Techs 的数据，Nginx 在全球顶级网站中的使用率已超过 Apache，成为最受欢迎的 Web 服务器软件。

## 二、Nginx 安装与初始配置

### 1. Linux 系统安装

**Ubuntu/Debian:**

```bash
# 添加 Nginx 官方仓库（可选）
sudo add-apt-repository ppa:nginx/stable
sudo apt update

# 安装 Nginx
sudo apt install nginx -y

# 验证安装
nginx -v

# 启动服务
sudo systemctl start nginx
sudo systemctl enable nginx
```

**CentOS/RHEL:**

```bash
# 安装 EPEL 仓库
sudo yum install epel-release -y

# 安装 Nginx
sudo yum install nginx -y

# 启动服务
sudo systemctl start nginx
sudo systemctl enable nginx
```

### 2. macOS 安装

```bash
# 使用 Homebrew
brew install nginx

# 启动服务
brew services start nginx
```

### 3. Windows 安装

访问 [nginx.org](https://nginx.org) 下载 Windows 版本，解压后运行：

```cmd
start nginx
nginx -s reload    # 重新加载配置
nginx -s stop      # 停止服务
```

### 4. 验证安装

打开浏览器访问 `http://localhost`，看到 Nginx 欢迎页面即表示安装成功。

![Nginx Architecture](/images/nginx-architecture.svg)
*Nginx 架构示意图*

## 三、Nginx 配置文件结构

### 1. 配置文件位置

```bash
# 主配置文件
/etc/nginx/nginx.conf

# 站点配置文件目录（Debian/Ubuntu）
/etc/nginx/sites-available/
/etc/nginx/sites-enabled/

# 站点配置文件目录（CentOS/RHEL）
/etc/nginx/conf.d/

# 日志文件
/var/log/nginx/access.log
/var/log/nginx/error.log
```

### 2. 配置文件层级结构

Nginx 配置文件采用模块化设计，主要包含以下几个层级：

```
Main Context（全局上下文）
├── Events Block（事件模块）
└── HTTP Block（HTTP 模块）
    ├── Server Block 1（虚拟主机 1）
    │   └── Location Blocks（路由规则）
    ├── Server Block 2（虚拟主机 2）
    │   └── Location Blocks
    └── Upstream Block（上游服务器组）
```

![Nginx Configuration Structure](/images/nginx-config-structure.svg)
*Nginx 配置文件层级结构*

### 3. 主配置文件详解

```nginx
# /etc/nginx/nginx.conf

# ========== Main Context ==========
# 运行 Nginx 的用户和用户组
user nginx;

# 工作进程数量（建议设置为 CPU 核心数）
worker_processes auto;

# 错误日志配置
error_log /var/log/nginx/error.log warn;

# PID 文件位置
pid /run/nginx.pid;

# 工作进程最大文件描述符数
worker_rlimit_nofile 65535;

# ========== Events Block ==========
events {
    # 每个工作进程的最大连接数
    worker_connections 4096;
    
    # 使用 epoll 模型（Linux）
    use epoll;
    
    # 接受多个连接
    multi_accept on;
}

# ========== HTTP Block ==========
http {
    # 基本设置
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;
    
    # 日志格式定义
    log_format main '$remote_addr - $remote_user [$time_local] "$request" '
                    '$status $body_bytes_sent "$http_referer" '
                    '"$http_user_agent" "$http_x_forwarded_for"';
    
    access_log /var/log/nginx/access.log main;
    
    # 性能优化
    sendfile        on;
    tcp_nopush      on;
    tcp_nodelay     on;
    keepalive_timeout 65;
    types_hash_max_size 2048;
    
    # Gzip 压缩
    gzip on;
    gzip_vary on;
    gzip_proxied any;
    gzip_comp_level 6;
    gzip_types text/plain text/css text/xml application/json 
               application/javascript application/xml;
    
    # 包含其他配置文件
    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;
}
```

## 四、Server Block 配置（虚拟主机）

### 1. 基础 Server Block

```nginx
server {
    # 监听端口
    listen 80;
    
    # 域名（可以有多个）
    server_name example.com www.example.com;
    
    # 网站根目录
    root /var/www/example.com/html;
    
    # 默认索引文件
    index index.html index.htm;
    
    # 字符集
    charset utf-8;
    
    # 访问日志
    access_log /var/log/nginx/example.com.access.log;
    error_log /var/log/nginx/example.com.error.log;
    
    # 根路径配置
    location / {
        try_files $uri $uri/ =404;
    }
    
    # 错误页面配置
    error_page 404 /404.html;
    error_page 500 502 503 504 /50x.html;
    
    location = /50x.html {
        root /usr/share/nginx/html;
    }
}
```

### 2. 多域名配置示例

```nginx
# 主域名
server {
    listen 80;
    server_name example.com;
    root /var/www/example.com;
    
    location / {
        try_files $uri $uri/ =404;
    }
}

# WWW 子域名重定向到主域名
server {
    listen 80;
    server_name www.example.com;
    
    # 301 永久重定向
    return 301 https://example.com$request_uri;
}

# 博客子域名
server {
    listen 80;
    server_name blog.example.com;
    root /var/www/blog;
    
    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }
    
    location ~ \.php$ {
        fastcgi_pass unix:/run/php/php-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}
```

### 3. HTTPS 配置

```nginx
server {
    listen 443 ssl http2;
    server_name example.com;
    
    # SSL 证书配置
    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
    
    # SSL 优化配置
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256;
    ssl_prefer_server_ciphers off;
    ssl_session_cache shared:SSL:10m;
    ssl_session_timeout 1d;
    ssl_session_tickets off;
    
    # HSTS（HTTP Strict Transport Security）
    add_header Strict-Transport-Security "max-age=63072000" always;
    
    root /var/www/example.com;
    index index.html;
    
    location / {
        try_files $uri $uri/ =404;
    }
}

# HTTP 自动跳转到 HTTPS
server {
    listen 80;
    server_name example.com;
    return 301 https://$host$request_uri;
}
```

## 五、Location 匹配规则

### 1. Location 语法

```nginx
location [modifier] pattern {
    # 配置内容
}
```

### 2. 匹配修饰符

| 修饰符 | 说明 | 优先级 |
|--------|------|--------|
| `=` | 精确匹配 | 最高 |
| `^~` | 前缀匹配（不检查正则） | 高 |
| `~` | 区分大小写的正则匹配 | 中 |
| `~*` | 不区分大小写的正则匹配 | 中 |
| 无 | 普通前缀匹配 | 低 |

### 3. 匹配示例

```nginx
server {
    listen 80;
    server_name example.com;
    root /var/www/html;
    
    # 精确匹配 - 优先级最高
    location = / {
        # 只匹配首页 /
        rewrite ^ /index.html;
    }
    
    # 前缀匹配
    location /images/ {
        # 匹配 /images/ 开头的路径
        expires 30d;
        add_header Cache-Control "public, immutable";
    }
    
    # 前缀匹配（优先于正则）
    location ^~ /static/ {
        # 匹配 /static/ 开头，不再检查正则
        alias /var/www/static/;
    }
    
    # 正则匹配（区分大小写）
    location ~ \.(gif|jpg|jpeg|png)$ {
        expires 7d;
    }
    
    # 正则匹配（不区分大小写）
    location ~* \.(css|js|woff|woff2)$ {
        expires 14d;
        add_header Cache-Control "public";
    }
    
    # 默认匹配
    location / {
        try_files $uri $uri/ =404;
    }
}
```

### 4. 常用 Location 配置

**静态文件服务：**

```nginx
location /assets/ {
    alias /var/www/assets/;
    expires 30d;
    add_header Cache-Control "public, no-transform";
    
    # 防止盗链
    valid_referers none blocked server_names *.example.com;
    if ($invalid_referer) {
        return 403;
    }
}
```

**PHP-FPM 配置：**

```nginx
location ~ \.php$ {
    try_files $uri =404;
    fastcgi_pass unix:/run/php/php8.1-fpm.sock;
    fastcgi_index index.php;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    include fastcgi_params;
    
    # 超时设置
    fastcgi_connect_timeout 60s;
    fastcgi_send_timeout 180s;
    fastcgi_read_timeout 180s;
    
    # 缓冲区设置
    fastcgi_buffer_size 128k;
    fastcgi_buffers 4 256k;
    fastcgi_busy_buffers_size 256k;
}
```

## 六、反向代理配置

### 1. 基础反向代理

```nginx
server {
    listen 80;
    server_name api.example.com;
    
    location / {
        # 代理到后端服务器
        proxy_pass http://127.0.0.1:3000;
        
        # 传递客户端信息
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        
        # 超时设置
        proxy_connect_timeout 60s;
        proxy_send_timeout 60s;
        proxy_read_timeout 60s;
        
        # 缓冲区设置
        proxy_buffering on;
        proxy_buffer_size 4k;
        proxy_buffers 8 4k;
    }
}
```

### 2. WebSocket 支持

```nginx
location /ws/ {
    proxy_pass http://backend_server;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_read_timeout 86400;
}
```

### 3. 完整反向代理示例

```nginx
upstream backend {
    server 127.0.0.1:3000;
    server 127.0.0.1:3001;
    keepalive 32;
}

server {
    listen 80;
    server_name app.example.com;
    
    # 静态资源直接服务
    location /static/ {
        alias /var/www/app/static/;
        expires 30d;
    }
    
    # API 请求代理到后端
    location /api/ {
        proxy_pass http://backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        
        # CORS 支持
        add_header Access-Control-Allow-Origin *;
        add_header Access-Control-Allow-Methods 'GET, POST, OPTIONS, PUT, DELETE';
        add_header Access-Control-Allow-Headers 'DNT,X-Mx-ReqToken,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Authorization';
        
        if ($request_method = 'OPTIONS') {
            return 204;
        }
    }
    
    # WebSocket 支持
    location /ws/ {
        proxy_pass http://backend;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
    }
    
    # 默认路由
    location / {
        proxy_pass http://backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

## 七、负载均衡配置

### 1. Upstream 模块基础

```nginx
# 定义上游服务器组
upstream backend_servers {
    # 轮询（默认）
    server 192.168.1.10:8080;
    server 192.168.1.11:8080;
    server 192.168.1.12:8080;
}

server {
    listen 80;
    server_name lb.example.com;
    
    location / {
        proxy_pass http://backend_servers;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

### 2. 负载均衡算法

**加权轮询：**

```nginx
upstream weighted_backend {
    server 192.168.1.10:8080 weight=3;  # 权重 3
    server 192.168.1.11:8080 weight=2;  # 权重 2
    server 192.168.1.12:8080 weight=1;  # 权重 1
}
```

**最少连接：**

```nginx
upstream least_conn_backend {
    least_conn;  # 最少连接算法
    server 192.168.1.10:8080;
    server 192.168.1.11:8080;
    server 192.168.1.12:8080;
}
```

**IP Hash（会话保持）：**

```nginx
upstream ip_hash_backend {
    ip_hash;  # 同一 IP 固定访问同一服务器
    server 192.168.1.10:8080;
    server 192.168.1.11:8080;
    server 192.168.1.12:8080;
}
```

**备用服务器：**

```nginx
upstream backup_backend {
    server 192.168.1.10:8080;
    server 192.168.1.11:8080;
    server 192.168.1.12:8080 backup;  # 备用服务器
}
```

![Nginx Load Balancing](/images/nginx-load-balancing.svg)
*Nginx 负载均衡示意图*

### 3. 健康检查与故障转移

```nginx
upstream resilient_backend {
    server 192.168.1.10:8080 max_fails=3 fail_timeout=30s;
    server 192.168.1.11:8080 max_fails=3 fail_timeout=30s;
    server 192.168.1.12:8080 max_fails=3 fail_timeout=30s;
    
    # 所有服务器失败时的备用
    server 192.168.1.13:8080 backup;
}

server {
    listen 80;
    server_name app.example.com;
    
    location / {
        proxy_pass http://resilient_backend;
        proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_504;
        proxy_connect_timeout 5s;
        proxy_read_timeout 30s;
    }
}
```

## 八、安全配置

### 1. 基础安全设置

```nginx
# 隐藏 Nginx 版本号
server_tokens off;

# 限制请求方法
if ($request_method !~ ^(GET|HEAD|POST|PUT|DELETE|OPTIONS)$) {
    return 405;
}

# 阻止常见攻击
location ~ /\.ht {
    deny all;
}

location ~ /\.(git|svn|env) {
    deny all;
}

# SQL 注入防护（基础）
set $block_sql_injections 0;
if ($query_string ~ "(union.*select)") { set $block_sql_injections 1; }
if ($query_string ~ "(concat.*\()") { set $block_sql_injections 1; }
if ($block_sql_injections = 1) {
    return 403;
}
```

### 2. 访问控制

**IP 白名单：**

```nginx
location /admin/ {
    allow 192.168.1.0/24;
    allow 10.0.0.0/8;
    deny all;
}
```

**IP 黑名单：**

```nginx
location / {
    deny 192.168.1.100;
    deny 10.0.0.0/8;
    allow all;
}
```

**密码保护：**

```bash
# 创建密码文件
sudo htpasswd -c /etc/nginx/.htpasswd username
```

```nginx
location /private/ {
    auth_basic "Restricted Area";
    auth_basic_user_file /etc/nginx/.htpasswd;
}
```

### 3. Rate Limiting（速率限制）

```nginx
http {
    # 定义限流区域
    limit_req_zone $binary_remote_addr zone=mylimit:10m rate=10r/s;
    limit_conn_zone $binary_remote_addr zone=conn_limit:10m;
    
    server {
        listen 80;
        server_name example.com;
        
        location / {
            # 请求频率限制
            limit_req zone=mylimit burst=20 nodelay;
            
            # 并发连接数限制
            limit_conn conn_limit 10;
            
            proxy_pass http://backend;
        }
        
        # API 接口更严格的限制
        location /api/ {
            limit_req zone=mylimit burst=5 nodelay;
            limit_conn conn_limit 5;
            
            proxy_pass http://backend;
        }
    }
}
```

### 4. SSL/TLS 最佳实践

```nginx
server {
    listen 443 ssl http2;
    server_name secure.example.com;
    
    ssl_certificate /etc/letsencrypt/live/secure.example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/secure.example.com/privkey.pem;
    
    # 仅启用安全协议
    ssl_protocols TLSv1.2 TLSv1.3;
    
    # 强加密套件
    ssl_ciphers 'ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384';
    ssl_prefer_server_ciphers off;
    
    # DH 参数
    ssl_dhparam /etc/nginx/dhparam.pem;
    
    # SSL 会话优化
    ssl_session_cache shared:SSL:50m;
    ssl_session_timeout 1d;
    ssl_session_tickets off;
    
    # OCSP Stapling
    ssl_stapling on;
    ssl_stapling_verify on;
    resolver 8.8.8.8 8.8.4.4 valid=300s;
    resolver_timeout 5s;
    
    # 安全头
    add_header Strict-Transport-Security "max-age=63072000" always;
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header Referrer-Policy "strict-origin-when-cross-origin" always;
}
```

## 九、性能优化

### 1. Worker 进程优化

```nginx
# 根据 CPU 核心数设置
worker_processes auto;

# 或使用固定数量
# worker_processes 4;

events {
    worker_connections 4096;
    multi_accept on;
    use epoll;
}

http {
    # 每个 worker 的最大文件描述符数
    worker_rlimit_nofile 65535;
}
```

### 2. 缓存优化

**静态文件缓存：**

```nginx
location ~* \.(jpg|jpeg|png|gif|ico|css|js|pdf|txt|woff|woff2)$ {
    expires 30d;
    add_header Cache-Control "public, no-transform";
    access_log off;
}
```

**代理缓存：**

```nginx
http {
    # 定义缓存路径和参数
    proxy_cache_path /var/cache/nginx levels=1:2 keys_zone=my_cache:100m 
                     max_size=10g inactive=60m use_temp_path=off;
    
    server {
        location / {
            proxy_cache my_cache;
            proxy_cache_valid 200 302 10m;
            proxy_cache_valid 404 1m;
            proxy_cache_use_stale error timeout updating http_500 http_502 http_503 http_504;
            proxy_cache_lock on;
            add_header X-Cache-Status $upstream_cache_status;
            
            proxy_pass http://backend;
        }
    }
}
```

### 3. Gzip 压缩

```nginx
http {
    gzip on;
    gzip_vary on;
    gzip_proxied any;
    gzip_comp_level 6;
    gzip_min_length 256;
    gzip_types 
        application/atom+xml
        application/geo+json
        application/javascript
        application/x-javascript
        application/json
        application/ld+json
        application/manifest+json
        application/rdf+xml
        application/rss+xml
        application/xhtml+xml
        application/xml
        font/eot
        font/otf
        font/ttf
        image/svg+xml
        text/css
        text/javascript
        text/plain
        text/xml;
}
```

### 4. 连接优化

```nginx
http {
    # TCP 优化
    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    
    # Keepalive 设置
    keepalive_timeout 65;
    keepalive_requests 100;
    
    # 重置超时
    reset_timedout_connection on;
    
    # 客户端体和头部限制
    client_max_body_size 10m;
    client_body_buffer_size 128k;
    client_header_buffer_size 1k;
    large_client_header_buffers 4 16k;
}
```

## 十、常用配置模板

### 1. WordPress 配置

```nginx
server {
    listen 80;
    server_name wordpress.example.com;
    root /var/www/wordpress;
    index index.php index.html;
    
    location / {
        try_files $uri $uri/ /index.php?$args;
    }
    
    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_pass unix:/run/php/php-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
        
        fastcgi_buffer_size 128k;
        fastcgi_buffers 4 256k;
        fastcgi_busy_buffers_size 256k;
    }
    
    location ~ /\.ht {
        deny all;
    }
    
    location ~ /wp-content/uploads/.*\.(php|php5|phtml)$ {
        deny all;
    }
}
```

### 2. Node.js 应用配置

```nginx
upstream node_app {
    server 127.0.0.1:3000;
    keepalive 64;
}

server {
    listen 80;
    server_name nodeapp.example.com;
    
    location / {
        proxy_pass http://node_app;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_cache_bypass $http_upgrade;
        proxy_read_timeout 90;
    }
    
    location /static/ {
        alias /var/www/nodeapp/public/;
        expires 30d;
    }
}
```

### 3. Docker 容器配置

```nginx
upstream docker_backend {
    server host.docker.internal:8080;
}

server {
    listen 80;
    server_name docker-app.example.com;
    
    location / {
        proxy_pass http://docker_backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

## 十一、调试与维护

### 1. 常用命令

```bash
# 测试配置文件语法
sudo nginx -t

# 查看 Nginx 版本
nginx -v

# 查看编译参数
nginx -V

# 重新加载配置（不中断服务）
sudo nginx -s reload

# 优雅停止
sudo nginx -s quit

# 立即停止
sudo nginx -s stop

# 查看运行状态
sudo systemctl status nginx

# 查看错误日志
tail -f /var/log/nginx/error.log

# 查看访问日志
tail -f /var/log/nginx/access.log

# 实时分析访问日志
awk '{print $1}' /var/log/nginx/access.log | sort | uniq -c | sort -nr | head -20
```

### 2. 常见问题排查

**配置文件错误：**

```bash
# 详细错误输出
sudo nginx -t 2>&1

# 检查特定文件
sudo nginx -c /etc/nginx/nginx.conf -t
```

**权限问题：**

```bash
# 检查文件所有权
ls -la /var/www/html

# 修复权限
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html
```

**端口占用：**

```bash
# 查看端口使用情况
sudo netstat -tulpn | grep :80
sudo lsof -i :80
```

### 3. 日志分析

```bash
# 统计访问量最高的 IP
awk '{print $1}' /var/log/nginx/access.log | sort | uniq -c | sort -nr | head -10

# 统计 404 错误
grep " 404 " /var/log/nginx/access.log | wc -l

# 查看特定时间段的请求
awk '$4 >= "[06/May/2026:10:00:00" && $4 <= "[06/May/2026:11:00:00"' /var/log/nginx/access.log

# 统计响应时间慢的请求
awk '($NF > 1) {print $7}' /var/log/nginx/access.log | sort -nr | head -10
```

## 十二、总结

通过本文的学习，你已经掌握了 Nginx 的基础配置知识：

1. **安装部署**：在不同操作系统上安装 Nginx
2. **配置结构**：理解 Main、Events、HTTP、Server、Location 层级关系
3. **虚拟主机**：配置多域名、HTTPS 站点
4. **Location 规则**：掌握各种匹配修饰符的使用
5. **反向代理**：配置代理转发、WebSocket 支持
6. **负载均衡**：实现多种负载均衡算法和健康检查
7. **安全加固**：配置访问控制、速率限制、SSL/TLS
8. **性能优化**：调整 Worker 进程、启用缓存和压缩
9. **实战模板**：WordPress、Node.js、Docker 等常见场景配置
10. **维护调试**：常用命令和故障排查技巧

**下一步建议：**

- 📚 深入学习 Nginx 模块开发
- 🔧 探索 Nginx Plus 企业版功能
- ☸️ 学习 Kubernetes Ingress 控制器
- 🔐 研究更高级的安全配置
- 📊 配置监控和告警系统

Nginx 是一个强大而灵活的工具，最好的学习方式就是在实际项目中不断实践和优化。希望本文能成为你 Nginx 学习之旅的良好起点！

---

**参考资源：**

- [Nginx 官方文档](https://nginx.org/en/docs/)
- [Nginx 配置最佳实践](https://www.nginx.com/resources/admin-guide/)
- [Mozilla SSL 配置生成器](https://ssl-config.mozilla.org/)
- [Nginx 社区论坛](https://forum.nginx.org/)
