# 拾领星球 - 校园失物招领平台

## 项目简介

拾领星球是一个连接失主与拾主的公益平台，通过微信小程序实现失物招领、寻物启事的发布与匹配，结合消息沟通与社区互动，提升失物找回效率。

## 技术栈

### 后端
- Spring Boot 2.7.14
- Spring MVC
- MyBatis
- MySQL 8.0
- Redis
- WebSocket（实时聊天）
- MinIO（文件存储）

### 前端
- 微信小程序（原生开发）
- Vue 2.6（后台管理系统）
- Element UI

## 项目结构

```
F:\失物招领\
├── backend/              # Spring Boot 后端
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/
│   │   │   │   └── com/pickup/planet/
│   │   │   │       ├── config/        # 配置类
│   │   │   │       ├── controller/    # 控制器
│   │   │   │       ├── service/       # 服务层
│   │   │   │       ├── dao/          # 数据访问层
│   │   │   │       ├── entity/       # 实体类
│   │   │   │       ├── utils/        # 工具类
│   │   │   │       └── websocket/    # WebSocket
│   │   │   └── resources/
│   │   │       ├── mapper/           # MyBatis Mapper
│   │   │       └── application.yml   # 配置文件
│   │   └── test/
│   └── pom.xml
│
├── miniprogram/          # 微信小程序
│   ├── pages/            # 页面
│   │   ├── index/       # 首页
│   │   ├── hall/        # 大厅
│   │   ├── message/     # 消息
│   │   ├── user/        # 用户中心
│   │   ├── detail/      # 详情页
│   │   └── publish/     # 发布页
│   ├── api/             # API 接口
│   ├── utils/           # 工具类
│   ├── components/      # 组件
│   ├── app.js
│   ├── app.json
│   └── app.wxss
│
├── admin-vue/           # Vue 后台管理系统
│   ├── src/
│   │   ├── views/       # 页面
│   │   ├── router/      # 路由
│   │   ├── store/       # 状态管理
│   │   ├── api/         # API 接口
│   │   └── main.js
│   ├── public/
│   └── package.json
│
├── database/            # 数据库脚本
│   └── init.sql        # 初始化脚本
│
└── docs/               # 文档
```

## 快速开始

### 1. 环境准备

- JDK 1.8+
- Maven 3.6+
- MySQL 8.0+
- Redis 6.0+
- Node.js 14+
- MinIO（可选）
- 微信开发者工具

### 2. 数据库初始化

```bash
# 登录 MySQL
mysql -u root -p

# 执行初始化脚本
source F:\失物招领\database\init.sql
```

默认管理员账号：
- 用户名：admin
- 密码：admin123

### 3. 后端启动

#### 3.1 修改配置文件

编辑 `backend/src/main/resources/application.yml`，修改以下配置：

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/pickup_planet?useUnicode=true&characterEncoding=utf8&serverTimezone=Asia/Shanghai&useSSL=false
    username: root
    password: your-password  # 修改为你的 MySQL 密码

  redis:
    host: localhost
    port: 6379
    password:  # 如果有密码则填写

# MinIO 配置
minio:
  endpoint: http://localhost:9000
  access-key: minioadmin
  secret-key: minioadmin
  bucket-name: pickup-planet

# 微信小程序配置
wechat:
  miniapp:
    appid: your-appid      # 修改为你的小程序 AppID
    secret: your-secret    # 修改为你的小程序 Secret
```

#### 3.2 启动后端

```bash
cd F:\失物招领\backend

# 使用 Maven 启动
mvn spring-boot:run

# 或者打包后启动
mvn clean package
java -jar target/planet-1.0.0.jar
```

后端服务将在 http://localhost:8080/api 启动

### 4. 微信小程序启动

#### 4.1 修改配置

编辑 `miniprogram/app.js`，修改后端接口地址：

```javascript
globalData: {
  baseUrl: 'http://your-server-ip:8080/api',  // 修改为你的后端地址
  wsUrl: 'ws://your-server-ip:8080/api/ws/chat'
}
```

#### 4.2 导入项目

1. 打开微信开发者工具
2. 选择"导入项目"
3. 选择 `F:\失物招领\miniprogram` 目录
4. 填写 AppID
5. 点击"导入"

### 5. Vue 后台管理系统启动

```bash
cd F:\失物招领\admin-vue

# 安装依赖
npm install

# 启动开发服务器
npm run serve

# 构建生产版本
npm run build
```

访问 http://localhost:8081 即可进入后台管理系统

## 功能模块

### 微信小程序端

1. **主页模块**
   - 快捷入口（发布寻物/招领）
   - 智能推荐
   - 分类筛选
   - 最新信息展示

2. **大厅模块**
   - 失物信息列表
   - 寻物信息列表
   - 高级筛选
   - 搜索功能

3. **消息模块**
   - 实时聊天（WebSocket）
   - 社区论坛
   - 帖子发布与评论

4. **用户模块**
   - 个人中心
   - 信息管理
   - 实名认证
   - 信用分体系

### 后台管理系统

1. **数据总览**
   - 用户统计
   - 信息统计
   - 找回成功率

2. **用户管理**
   - 用户列表
   - 实名认证审核
   - 用户状态管理

3. **信息管理**
   - 寻物信息审核
   - 招领信息审核
   - 信息状态管理

4. **内容管理**
   - 社区帖子管理
   - 平台公告发布

## API 接口文档

### 用户相关

- `POST /api/user/login` - 微信登录
- `GET /api/user/info` - 获取用户信息
- `PUT /api/user/info` - 更新用户信息

### 寻物相关

- `POST /api/lost` - 发布寻物信息
- `GET /api/lost` - 查询寻物信息列表
- `GET /api/lost/{id}` - 查询寻物信息详情
- `PUT /api/lost/{id}` - 更新寻物信息
- `PUT /api/lost/{id}/status` - 更新寻物状态
- `PUT /api/lost/{id}/audit` - 审核寻物信息（管理员）

### 招领相关

- `POST /api/found` - 发布招领信息
- `GET /api/found` - 查询招领信息列表
- `GET /api/found/{id}` - 查询招领信息详情
- `PUT /api/found/{id}` - 更新招领信息
- `PUT /api/found/{id}/status` - 更新招领状态
- `PUT /api/found/{id}/audit` - 审核招领信息（管理员）

### WebSocket

- `ws://localhost:8080/api/ws/chat/{userId}` - 聊天 WebSocket 连接

## 注意事项

1. 首次运行需要先初始化数据库
2. 确保 Redis 和 MySQL 服务已启动
3. 微信小程序需要配置合法域名（生产环境）
4. MinIO 需要单独安装并配置（用于图片存储）
5. JWT Secret 建议在生产环境中修改为更复杂的值

## 常见问题

### 1. 数据库连接失败

检查 MySQL 服务是否启动，配置文件中的用户名密码是否正确。

### 2. 微信登录失败

检查小程序 AppID 和 Secret 是否配置正确。

### 3. 文件上传失败

检查 MinIO 服务是否启动，配置是否正确。

## 开发计划

- [ ] 实现智能匹配算法
- [ ] 添加消息推送功能
- [ ] 完善信用分体系
- [ ] 增加数据统计图表
- [ ] 支持多语言

## 贡献指南

欢迎提交 Issue 和 Pull Request！

## 许可证

MIT License

## 联系方式

如有问题，请联系：pickup-planet@example.com
