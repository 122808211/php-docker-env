### 🎟 Docker 开发环境说明

本项目使用 Docker 构建 PHP + Nginx + MySQL 的开发环境，适合 Laravel 等 PHP 项目开发部署。

------

### 📁 目录结构说明

```
.
├── docker-compose.yml     # Docker 服务组装配置
├── nginx/
│   └── default.conf        # Nginx 虚拟主机配置
├── php/
│   └── Dockerfile          # PHP 自定义镜像（如 php-fpm）
└── .env                    # Laravel 项目环境配置文件
```

------

### 🚀 快速启动步骤

1. **克隆代码**

```bash
git clone git@github.com:yourname/yourrepo.git
cd yourrepo
```

1. **启动服务**

```bash
docker compose up -d
```

1. **配置本地 hosts 文件**

将如下内容添加到本机 `/etc/hosts` 文件（macOS/Linux）或 `C:\Windows\System32\drivers\etc\hosts`（Windows）：

```
127.0.0.1 myproject.local
```

1. **访问站点**

打开浏览器访问：

```
http://myproject.local:8088
```

------

### ⚙️ 服务说明

| 服务    | 说明                       | 端口映射   |
| ------- | -------------------------- | ---------- |
| nginx   | 反向代理 + 静态服务        | 8088:80    |
| php-fpm | PHP 8.2 服务               | 无外部端口 |
| mysql   | 可选服务（本地已有可省略） | 3306:3306  |

------

### 📌 Nginx 配置示例

```nginx
server {
    listen 80;
    server_name myproject.local;

    root /apps/web/public;
    index index.php index.html;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        fastcgi_pass php82:9000;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }

    access_log /var/log/nginx/access.log;
    error_log  /var/log/nginx/error.log;
}
```

------

### 🛠 常用命令

```bash
# 构建并启动容器
docker compose up -d

# 停止容器
docker compose down

# 查看容器状态
docker ps

# 进入 PHP 容器
docker exec -it php82 bash
```

------

### 📅 说明

- 代码目录挂载在 `/apps/web/`
- PHP 和 Nginx 通过 Docker 网络通信（如：`fastcgi_pass php82:9000`）
- MySQL 如使用本机安装的，可直接连接 `localhost:3306`
