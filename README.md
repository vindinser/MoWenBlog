# MOWEN - 玄牝之门，是谓天地根

> 项目基于 [POETIZE](https://gitee.com/littledokey/poetize) —— 一个程序员的全栈浪漫。

## 技术栈

| 模块   | 技术栈             | 组件                         |
| ------ | ------------------ | ---------------------------- |
| 后端   | Spring Boot 2.7    | MySQL + MyBatis-Plus + T-io  |
| 博客   | Vue 2              | Element UI                   |
| 聊天室 | Vue 3              | Element Plus + Naive UI      |
| 移动端 | CSS                | 响应式自适应                 |

## 功能特性

- **博客系统**：文章、表白墙、图片墙、收藏夹、音乐播放、视频播放、留言、友链、后台管理
- **聊天室系统**：朋友圈、好友、群聊

## 项目结构

```
MoWen/
├── mowen-ui/          # 博客前端 (Vue 2 + Element UI)
├── mowen-im-ui/       # 聊天室前端 (Vue 3 + Element Plus)
├── mowen-server/      # 后端服务 (Spring Boot)
│   ├── poetry-web/    # 主模块
│   └── sql/           # 数据库脚本
└── README.md
```

## 本地开发

### 环境要求

- Node.js 20+（推荐 v20.10.0）
- Java 8+
- MySQL 5.7+
- Maven 3.6+

### 1. 数据库

```bash
# 创建数据库
mysql -u root -p -e "CREATE DATABASE mowen DEFAULT CHARACTER SET utf8mb4;"

# 导入数据
mysql -u root -p mowen < mowen-server/sql/mowen.sql
```

### 2. 后端

```bash
cd mowen-server

# 修改数据库连接（编辑 poetry-web/src/main/resources/application.yml）
# 将 datasource.url 改为: jdbc:mysql://localhost:3306/mowen?...

# 打包
mvn clean package -DskipTests

# 运行
java -jar poetry-web/target/mowen-server.jar
```

后端启动后运行在 `http://localhost:9528`

### 3. 前端 - 博客

```bash
cd mowen-ui
npm install
npm run dev
```

博客前端启动在 `http://localhost:8090`

### 4. 前端 - 聊天室（可选）

```bash
cd mowen-im-ui
npm install
npm run dev
```

聊天室前端启动在 `http://localhost:9529`

> 聊天室依赖博客系统的登录模块，需通过博客的「联系我」入口进入。

## 端口说明

| 服务       | 端口 | 地址                         |
| ---------- | ---- | ---------------------------- |
| 博客前端   | 8090 | http://localhost:8090         |
| 聊天室前端 | 9529 | http://localhost:9529         |
| 后端 API   | 9528 | http://localhost:9528         |

## 配置说明

### 数据库配置

编辑 `mowen-server/poetry-web/src/main/resources/application.yml`：

```yaml
spring:
  datasource:
    username: root
    password: your_password
    url: jdbc:mysql://localhost:3306/mowen?useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai
```

### 文件存储

支持两种存储方式，在 `application.yml` 中配置：

```yaml
# 本地存储
store:
  type: local
local:
  enable: true

# 七牛云存储
store:
  type: qiniu
qiniu:
  enable: true
  accessKey: your_access_key
  secretKey: your_secret_key
  bucket: your_bucket
  downloadUrl: your_domain
```

> 本地存储需要 Nginx 代理 `/uploads` 路径才能访问上传的文件。

### 邮件配置

如需邮件功能（评论通知等），修改 `application.yml` 中的 `spring.mail` 配置。修改邮箱后需重启后端。

## 后台管理

启动后访问 http://localhost/admin 进入后台管理界面。

- 默认管理员账号：`ZS`
- 默认密码：`aaa`

可在「配置管理」中自定义网站信息。

## 部署

生产环境部署需要：

1. 安装 Nginx、Java 8+、MySQL
2. 导入数据库脚本
3. 打包前后端项目
4. 配置 Nginx 反向代理

```
# Nginx 配置示例
server {
    listen 80;
    server_name your_domain;

    # 博客前端
    location / {
        root /path/to/mowen-ui/dist;
        index index.html;
        try_files $uri $uri/ /index.html;
    }

    # 聊天室前端
    location /im/ {
        alias /path/to/mowen-im-ui/dist/;
        index index.html;
        try_files $uri $uri/ /im/index.html;
    }

    # 后端 API
    location /api/ {
        proxy_pass http://localhost:9528;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    # 文件服务（本地存储时需要）
    location /uploads/ {
        alias /data/uploads/;
    }
}
```

## 注意事项

- 本地开发时，上传的文件无法直接访问（需 Nginx 代理），服务器部署后正常
- 聊天室系统是非必须的，可独立部署或不部署
- 部分配置修改后需重启后端（如邮箱授权码、数据库连接等）
- Maven 使用华为云镜像，国内网络构建更快

## License

[MIT](LICENSE)
