# 银行卡号-车架号生成器

一个基于 Django 框架的 Web 应用，用于生成符合校验规则的银行卡号和车架号（VIN）。

## 📋 功能特性

- ✅ **银行卡号生成**：使用 Luhn 算法生成符合校验规则的银行卡号
- ✅ **车架号生成**：生成符合 ISO 3779 标准的 17 位车架号（VIN）
- ✅ **多家银行支持**：支持农业银行、工商银行、中国银行、邮储银行
- ✅ **自动切换**：选择银行后自动刷新，无需点击按钮
- ✅ **一键复制**：支持复制银行卡号和车架号，带气泡提示
- ✅ **环境变量配置**：支持通过环境变量自定义配置
- ✅ **Docker 部署**：支持 Docker 和 Docker Compose 快速部署

## 🏗️ 技术栈

| 组件 | 技术 |
|------|------|
| 后端框架 | Django 3.2.18 |
| 语言 | Python 3.7+ |
| 前端 | HTML5 + CSS3 + JavaScript |
| 部署 | Docker + Docker Compose |

## 🚀 快速开始

### 本地运行

#### 1. 安装依赖

```bash
cd generator_info
pip install -r requirements.txt
```

#### 2. 启动服务

```bash
python manage.py runserver 0.0.0.0:8000
```

#### 3. 访问应用

打开浏览器访问：**http://localhost:8000/**

### Docker 部署

#### 1. 使用 Docker Compose（推荐）

```bash
# 启动服务（后台运行）
docker-compose up -d

# 或前台运行查看日志
docker-compose up
```

#### 2. 访问应用

打开浏览器访问：**http://localhost:8000/**

#### 3. 停止服务

```bash
docker-compose down
```

## ⚙️ 环境变量配置

在 `.env` 文件中配置以下环境变量：

| 变量名 | 说明 | 默认值 | 示例 |
|--------|------|--------|------|
| `CARD_NO_LEN` | 银行卡号长度 | 19 | 16, 19 |
| `TOAST_DURATION` | 气泡提示显示时间（秒） | 3 | 5, 10 |

### 本地开发环境配置

创建 `generator_info/.env` 文件：

```env
CARD_NO_LEN=19
TOAST_DURATION=3
```

### Docker 环境配置

在项目根目录创建 `.env` 文件：

```env
CARD_NO_LEN=19
TOAST_DURATION=5
```

然后重启服务：

```bash
docker-compose up -d --build
```

## 📖 使用说明

### 1. 生成银行卡号和车架号

应用启动后，页面会自动生成 10 组银行卡号和车架号。

### 2. 切换银行

点击下拉框选择不同的银行，系统会自动刷新页面，生成对应银行的银行卡号。

**支持的银行：**

- 🏦 农业银行（卡号前缀：622848）
- 🏦 工商银行（卡号前缀：622203）
- 🏦 中国银行（卡号前缀：621331）
- 🏦 邮储银行（卡号前缀：622188）

### 3. 复制信息

点击每行的"复制卡号"或"复制车架号"按钮，可以将对应信息复制到剪贴板。页面顶部会显示气泡提示：

- ✅ **绿色提示**：复制成功
- ❌ **红色提示**：复制失败

气泡提示会在指定时间后自动消失（默认 3 秒，可通过 `TOAST_DURATION` 环境变量配置）。

## 🐳 Docker 常用命令

```bash
# 构建镜像
docker-compose build

# 启动服务
docker-compose up -d

# 停止服务
docker-compose down

# 查看日志
docker-compose logs -f

# 进入容器
docker-compose exec web /bin/bash

# 查看容器状态
docker-compose ps

# 重新构建并启动
docker-compose up -d --build

# 删除容器和镜像
docker-compose down --rmi all
```

## 📁 项目结构

```
generator-bankcode-vin/
├── Dockerfile                  # Docker 镜像构建文件
├── docker-compose.yml          # Docker Compose 编排文件
├── .env                       # 环境变量配置（本地开发）
├── generator_info/             # Django 应用主目录
│   ├── generator_info/        # Django 项目配置
│   │   ├── settings.py       # 项目设置
│   │   ├── urls.py           # URL 路由配置
│   │   ├── wsgi.py           # WSGI 入口
│   │   └── asgi.py           # ASGI 入口
│   ├── view/                 # 视图层
│   │   └── view.py           # 主视图逻辑
│   ├── tools/                # 工具模块
│   │   └── luhn.py           # Luhn 算法实现
│   ├── templates/             # HTML 模板
│   │   └── show.html          # 前端页面
│   ├── .env                  # 环境变量配置
│   ├── .env.example          # 环境变量示例
│   ├── manage.py             # Django 管理脚本
│   └── requirements.txt       # 依赖列表
└── README.md                  # 项目说明文档
```

## 🔧 核心算法

### Luhn 算法

银行卡号校验使用 Luhn 算法（模 10 算法），该算法被广泛用于验证信用卡号、银行卡号等。

**算法步骤：**

1. 从卡号最后一位数字开始，偶数位乘以 2
2. 如果乘积大于 10，则将个位和十位数字相加
3. 将所有数字相加
4. 总和能够被 10 整除则为有效卡号

### VIN 车架号

车架号（Vehicle Identification Number）符合 ISO 3779 标准，包含：

- 第 1-3 位：世界制造厂识别码（WMI）
- 第 4-9 位：车辆说明部分（VDS）
- 第 10 位：车型年份
- 第 11 位：装配厂
- 第 12-17 位：车辆生产序列号

本应用生成的车架号第 9 位为校验位，符合标准规范。

## ⚠️ 免责声明

本应用生成的银行卡号和车架号**仅供测试使用**，并非真实的银行卡号或车架号。请勿将其用于任何非法用途或欺诈行为。

## 📝 许可证

本项目仅供学习和研究使用。

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

---

**开发时间**：2026 年 3 月
