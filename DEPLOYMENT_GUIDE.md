# 🚀 完整部署指南 - Fragrance Yunnan 企业级跨境电商平台

## 📋 目录
1. [快速开始](#快速开始)
2. [项目结构](#项目结构)
3. [本地开发](#本地开发)
4. [Docker 部署](#docker-部署)
5. [生产环境部署](#生产环境部署)
6. [数据库设置](#数据库设置)
7. [安全配置](#安全配置)
8. [故障排查](#故障排查)

---

## 快速开始

### 前置要求
- Node.js 18+ 
- npm 9+
- Docker & Docker Compose (可选但推荐)
- PostgreSQL 15 (或使用 Docker)
- Redis 7 (或使用 Docker)

### 5 分钟快速启动

```bash
# 1. 克隆仓库
git clone https://github.com/xynong/fragrance-yunnan.git
cd fragrance-yunnan

# 2. 切换到开发分支
git checkout feat/enterprise-setup

# 3. 复制环境配置
cp .env.example .env

# 4. 使用 Docker Compose 启动（推荐）
docker-compose up -d

# 5. 执行数据库迁移
docker-compose exec app npm run db:migrate

# 6. 访问网站
# 前端: http://localhost
# 后端 API: http://localhost:5000
# API 文档: http://localhost:5000/api/docs
```

---

## 项目结构

```
fragrance-yunnan/
├── public/                       # 前端静态资源
│   ├── index.html               # 首页
│   ├── products.html            # 产品页
│   ├── batches.html             # 批次查询
│   ├── science.html             # 科普页
│   ├── about.html               # 关于页
│   ├── contact.html             # 联系页
│   ├── assets/
│   │   ├── css/
│   │   │   └── style.css        # ✨ 全局样式
│   │   ├── js/
│   │   │   ├── main.js          # ✨ 通用逻辑
│   │   │   ├── batches.js       # ✨ 批次查询逻辑
│   │   │   └── footer.js        # ✨ 页脚组件
│   │   └── images/              # 图片资源
│   │       ├── logo.png
│   │       ├── products/
│   │       └── plants/
│   └── data/
│       ├── products.json        # 产品数据
│       └── batches.json         # 批次数据（实时同步）
│
├── server/                       # 后端 API 服务
│   ├── index.js                 # 服务入口
│   ├── config/
│   │   ├── database.js          # 数据库连接配置
│   │   ├── redis.js             # Redis 缓存配置
│   │   └── env.js               # 环境变量验证
│   ├── middleware/
│   │   ├── auth.js              # JWT 认证中间件
│   │   ├── security.js          # 安全中间件（CORS、Helmet）
│   │   ├── rateLimit.js         # 速率限制
│   │   ├── errorHandler.js      # 错误处理
│   │   └── logger.js            # 日志中间件
│   ├── routes/
│   │   ├── auth.js              # 认证路由（登录、注册、JWT）
│   │   ├── users.js             # 用户管理
│   │   ├── products.js          # 产品 API
│   │   ├── batches.js           # 批次查询 API
│   │   ├── orders.js            # 订单管理 API
│   │   ├── payments.js          # 支付 API（Stripe、支付宝、微信）
│   │   └── admin.js             # 管理后台 API
│   ├── controllers/
│   │   ├── authController.js
│   │   ├── productController.js
│   │   ├── batchController.js
│   │   ├── orderController.js
│   │   └── paymentController.js
│   ├── models/
│   │   ├── User.js
│   │   ├── Product.js
│   │   ├── Batch.js
│   │   ├── Order.js
│   │   └── Payment.js
│   ├── services/
│   │   ├── authService.js       # 认证业务逻辑
│   │   ├── productService.js    # 产品业务逻辑
│   │   ├── orderService.js      # 订单业务逻辑
│   │   ├── paymentService.js    # 支付网关集成
│   │   ├── emailService.js      # 邮件服务
│   │   ├── cacheService.js      # 缓存管理
│   │   └── i18nService.js       # 国际化服务
│   ├── utils/
│   │   ├── validators.js        # 数据验证
│   │   ├── helpers.js           # 辅助函数
│   │   ├── jwt.js               # JWT 工具
│   │   └── encryption.js        # 加密工具
│   └── constants/
│       ├── statusCodes.js
│       ├── messages.js
│       └── config.js
│
├── scripts/
│   ├── init-db.sql              # 初始化数据库 SQL
│   ├── seed-db.sql              # 种子数据
│   ├── migrate.js               # 数据库迁移脚本
│   ├── seed.js                  # 数据库填充脚本
│   └── reset-db.js              # 重置数据库脚本
│
├── tests/
│   ├── unit/                    # 单元测试
│   │   ├── services/
│   │   └── utils/
│   ├── integration/             # 集成测试
│   │   ├── api/
│   │   └── database/
│   └── setup.js                 # 测试配置
│
├── docs/
│   ├── API.md                   # API 文档
│   ├── DATABASE.md              # 数据库设计
│   ├── ARCHITECTURE.md          # 架构设计
│   ├── SECURITY.md              # 安全指南
│   └── DEPLOYMENT.md            # 部署指南
│
├── .github/
│   └── workflows/
│       ├── deploy.yml           # 部署工作流
│       └── security.yml         # 安全扫描工作流
│
├── nginx.conf                   # Nginx 反向代理配置
├── Dockerfile                   # Docker 镜像构建
├── docker-compose.yml           # Docker 多服务编排
├── .dockerignore
├── .env.example                 # 环境变量示例
├── .gitignore
├── .eslintrc.json               # ESLint 配置
├── .prettierrc                  # Prettier 格式化配置
├── package.json                 # 项目依赖
└── README.md                    # 项目说明
```

---

## 本地开发

### 1. 安装依赖

```bash
npm install
```

### 2. 创建本地 .env 文件

```bash
cp .env.example .env
```

编辑 `.env`，设置本地开发环境：

```bash
NODE_ENV=development
PORT=5000
DATABASE_URL=postgresql://fragrance:fragrancepass123@localhost:5432/fragrance_db
REDIS_URL=redis://localhost:6379
JWT_SECRET=dev-secret-key-min-32-chars-long-for-testing
```

### 3. 启动数据库（使用 Docker）

```bash
# 仅启动数据库和 Redis
docker-compose up -d db redis

# 等待数据库启动
sleep 5

# 初始化数据库
npm run db:migrate
npm run db:seed
```

### 4. 启动开发服务器

```bash
npm run dev
```

服务器将运行在 `http://localhost:5000`

### 5. 在另一个终端启动前端（纯静态）

```bash
# 使用 Python 简单服务器
cd public && python3 -m http.server 3000

# 或使用 Node 简单服务器
cd public && npx serve
```

前端将运行在 `http://localhost:3000`

---

## Docker 部署

### 使用 Docker Compose 一键启动

```bash
# 1. 启动所有服务（数据库、Redis、应用）
docker-compose up -d

# 2. 查看日志
docker-compose logs -f app

# 3. 执行数据库迁移
docker-compose exec app npm run db:migrate

# 4. 访问应用
# 前端: http://localhost
# 后端: http://localhost:5000
# API 文档: http://localhost:5000/api/docs
```

### Docker 镜像手动构建

```bash
# 构建镜像
docker build -t fragrance-yunnan:latest .

# 运行容器
docker run -d \
  -p 5000:5000 \
  --env-file .env \
  --name fragrance-app \
  fragrance-yunnan:latest

# 查看日志
docker logs -f fragrance-app
```

### 监控 Docker 服务

```bash
# 查看所有运行中的容器
docker-compose ps

# 查看容器资源使用情况
docker stats

# 进入应用容器
docker-compose exec app sh

# 查看数据库日志
docker-compose logs db

# 查看 Redis 日志
docker-compose logs redis
```

---

## 生产环境部署

### 前置条件

- 云服务器（AWS EC2、DigitalOcean、阿里云等）
- 域名（xyong.ccwu.cc）
- SSL 证书（Let's Encrypt 免费）
- GitHub 账户（用于自动部署）

### 方案 1：使用 GitHub Actions 自动部署（推荐）

#### 1. 配置部署密钥

在 GitHub 仓库设置中添加以下 Secrets：

```
DEPLOY_HOST          = 你的服务器 IP（如 47.96.123.45）
DEPLOY_USER          = 服务器用户名（如 ubuntu）
DEPLOY_SSH_KEY       = 服务器 SSH 私钥
DEPLOY_PORT          = SSH 端口（默认 22）
DOCKER_USERNAME      = Docker Hub 用户名
DOCKER_PASSWORD      = Docker Hub 密码
SLACK_WEBHOOK        = Slack 通知 Webhook（可选）
```

#### 2. GitHub Actions 工作流会自动：

- ✅ 运行测试和代码检查
- ✅ 构建 Docker 镜像并推送到 Docker Hub
- ✅ SSH 连接到服务器
- ✅ 部署新版本
- ✅ 运行数据库迁移
- ✅ 发送通知

#### 3. 推送代码到 main 分支即可自动部署

```bash
git push origin main
```

### 方案 2：手动部署到服务器

#### 1. 服务器初始化（一次性）

```bash
# 登录服务器
ssh ubuntu@your-server-ip

# 安装 Docker 和 Docker Compose
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker $USER
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

# 克隆仓库
git clone https://github.com/xynong/fragrance-yunnan.git
cd fragrance-yunnan
git checkout feat/enterprise-setup

# 创建生产环境配置
cp .env.example .env
# 编辑 .env，设置生产密钥、数据库连接等

# 启动应用
docker-compose up -d

# 初始化数据库
docker-compose exec app npm run db:migrate
```

#### 2. 配置 Nginx 反向代理

```bash
# 安装 Nginx
sudo apt-get update && sudo apt-get install -y nginx

# 创建 Nginx 配置
sudo vim /etc/nginx/sites-available/xyong.ccwu.cc
```

配置内容：

```nginx
upstream app {
  server localhost:5000;
}

server {
  listen 80;
  server_name xyong.ccwu.cc www.xyong.ccwu.cc;
  return 301 https://$server_name$request_uri;
}

server {
  listen 443 ssl http2;
  server_name xyong.ccwu.cc www.xyong.ccwu.cc;

  ssl_certificate /etc/letsencrypt/live/xyong.ccwu.cc/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/xyong.ccwu.cc/privkey.pem;

  ssl_protocols TLSv1.2 TLSv1.3;
  ssl_ciphers HIGH:!aNULL:!MD5;
  ssl_prefer_server_ciphers on;

  # 前端静态资源
  location / {
    root /app/fragrance-yunnan/public;
    try_files $uri $uri/ /index.html;
  }

  # 后端 API
  location /api/ {
    proxy_pass http://app;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
  }

  # 健康检查
  location /health {
    proxy_pass http://app;
  }
}
```

#### 3. 启用 Nginx 配置并获取 SSL 证书

```bash
# 启用站点
sudo ln -s /etc/nginx/sites-available/xyong.ccwu.cc /etc/nginx/sites-enabled/

# 测试配置
sudo nginx -t

# 重启 Nginx
sudo systemctl restart nginx

# 使用 Certbot 获取 Let's Encrypt 证书
sudo apt-get install -y certbot python3-certbot-nginx
sudo certbot certonly --nginx -d xyong.ccwu.cc -d www.xyong.ccwu.cc

# 自动更新证书
sudo systemctl enable certbot.timer
```

#### 4. 监控和维护

```bash
# 查看 Docker 日志
docker-compose logs -f

# 备份数据库
docker-compose exec db pg_dump -U fragrance fragrance_db > backup_$(date +%Y%m%d_%H%M%S).sql

# 更新应用
git pull origin main
docker-compose down
docker-compose up -d
docker-compose exec app npm run db:migrate
```

---

## 数据库设置

### 初始化数据库

```bash
npm run db:migrate
npm run db:seed
```

### 数据库备份

```bash
# 备份
docker-compose exec db pg_dump -U fragrance fragrance_db > backup.sql

# 恢复
docker-compose exec -T db psql -U fragrance fragrance_db < backup.sql
```

### 数据库访问

```bash
# 进入数据库 CLI
docker-compose exec db psql -U fragrance -d fragrance_db

# 常用 SQL
\\dt                    # 列出所有表
\\d users              # 查看 users 表结构
SELECT * FROM users;   # 查询用户
\\q                    # 退出
```

---

## 安全配置

### 🔐 核心安全措施

1. **环境变量隐藏**
   - `.env` 文件添加到 `.gitignore`
   - 使用 `.env.example` 作为模板
   - 生产密钥存储在 GitHub Secrets

2. **JWT 认证**
   - 所有 API 请求需要 `Authorization: Bearer <token>` 头
   - Token 过期时间设置为 7 天
   - 刷新 Token 机制

3. **数据库安全**
   - 使用 PostgreSQL 强密码
   - 启用 SSL 连接
   - 定期备份
   - 角色级别权限管理

4. **速率限制**
   - API 限制每 15 分钟 100 个请求
   - 登录尝试限制
   - DDoS 防护

5. **CORS 配置**
   - 仅允许特定域名
   - 生产环境限制为 `https://xyong.ccwu.cc`

6. **数据加密**
   - 密码使用 bcrypt 加密（10 轮）
   - 支付数据加密存储
   - HTTPS 传输加密

7. **审计日志**
   - 所有修改操作记录
   - 支付交易完整日志
   - 用户登录日志

### 常见安全错误（避免）

```javascript
// ❌ 错误：明文存储密码
user.password = req.body.password;

// ✅ 正确：加密密码
user.password = await bcrypt.hash(req.body.password, 10);

// ❌ 错误：忘记验证输入
const product = await Product.find(req.body.id);

// ✅ 正确：验证所有输入
const { id } = req.body;
if (!id || !Number.isInteger(id)) {
  throw new Error('Invalid product ID');
}

// ❌ 错误：在日志中记录敏感信息
logger.info('Payment card: ' + cardNumber);

// ✅ 正确：过滤敏感信息
logger.info('Payment card: ****' + cardNumber.slice(-4));
```

---

## 故障排查

### 问题 1：Docker 容器启动失败

```bash
# 查看错误日志
docker-compose logs app

# 检查 Docker 资源
docker system df

# 清理无用镜像和容器
docker system prune -a

# 重建镜像
docker-compose build --no-cache
```

### 问题 2：数据库连接失败

```bash
# 检查数据库状态
docker-compose ps db

# 查看数据库日志
docker-compose logs db

# 验证连接字符串（在容器内测试）
docker-compose exec db psql -U fragrance -d fragrance_db -c "SELECT 1"
```

### 问题 3：前端页面无法加载

```bash
# 检查 Nginx 日志
sudo tail -f /var/log/nginx/error.log

# 验证前端文件
ls -la public/

# 测试后端连接
curl http://localhost:5000/health
```

### 问题 4：JWT 认证失败

```bash
# 检查 JWT_SECRET 配置
echo $JWT_SECRET

# 验证 token 有效性
node -e "const jwt = require('jsonwebtoken'); console.log(jwt.verify('your-token', process.env.JWT_SECRET))"
```

### 问题 5：支付网关错误

```bash
# 验证 API 密钥
echo $STRIPE_SECRET_KEY

# 检查支付日志
docker-compose logs app | grep -i payment

# 测试 API 连接
curl -H "Authorization: Bearer $STRIPE_SECRET_KEY" https://api.stripe.com/v1/charges
```

---

## 📞 获取帮助

- 📖 查看完整文档：`./docs/`
- 🐛 报告 Bug：GitHub Issues
- 💬 讨论功能：GitHub Discussions
- 📧 联系支持：admin@xyong.ccwu.cc

---

**最后更新**：2026-08-09  
**维护者**：Xynong 团队
