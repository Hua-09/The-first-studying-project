# GitHub 项目部署完整指南

本指南详细介绍如何将项目上传到 GitHub 并部署，使其他人可以访问您的项目。

## 一、准备工作

### 1. 安装 Git
- **Windows**: 下载并安装 Git for Windows (https://git-scm.com/download/win)
- **macOS**: 使用 Homebrew 安装 `brew install git` 或从官网下载
- **Linux**: 使用包管理器安装 `sudo apt install git` (Ubuntu/Debian)

### 2. 创建 GitHub 账号
- 访问 https://github.com/ 注册账号
- 登录您的 GitHub 账号

### 3. 准备项目
- 确保项目结构清晰
- 移除敏感信息（如密码、API 密钥等）
- 创建 `README.md` 文件，介绍项目
- 创建 `.gitignore` 文件，排除不需要上传的文件

## 二、上传项目到 GitHub

### 1. 初始化 Git 仓库
在项目根目录打开命令行终端：

```bash
# 初始化 Git 仓库
git init

# 配置 Git 用户名和邮箱
git config user.name "Your Name"
git config user.email "your.email@example.com"
```

### 2. 添加文件到 Git

```bash
# 添加所有文件到 Git
git add .

# 查看已添加的文件
git status
```

### 3. 提交文件

```bash
# 提交文件
git commit -m "Initial commit: 项目名称"
```

### 4. 创建 GitHub 仓库
1. 登录 GitHub
2. 点击右上角的 "+", 选择 "New repository"
3. 输入仓库名称（例如：filecloud）
4. 选择仓库类型（公开或私有）
5. 不要勾选 "Initialize this repository with a README"（因为我们已经有了）
6. 点击 "Create repository"

### 5. 关联 GitHub 仓库
复制 GitHub 仓库的 HTTPS 或 SSH 地址，然后执行：

```bash
# 关联 GitHub 仓库
git remote add origin https://github.com/your-username/repository-name.git

# 查看远程仓库信息
git remote -v
```

### 6. 推送代码到 GitHub

```bash
# 推送代码到 GitHub
git push -u origin master
```

如果您使用的是 main 分支而不是 master 分支，请执行：

```bash
git push -u origin main
```

## 三、部署项目（使别人可以访问）

### 方法一：部署静态网站（GitHub Pages）

#### 1. 构建前端项目（如果有）

```bash
# 进入前端目录
cd frontend

# 安装依赖
npm install

# 构建项目
npm run build
```

#### 2. 部署到 GitHub Pages

##### 方法 A: 使用 gh-pages 包

```bash
# 安装 gh-pages 包
npm install -g gh-pages

# 部署到 GitHub Pages
gh-pages -d dist
```

##### 方法 B: 手动创建 gh-pages 分支

```bash
# 创建并切换到 gh-pages 分支
git checkout -b gh-pages

# 清理分支（只保留构建产物）
# 删除所有文件（除了构建产物）

# 添加构建产物
git add dist/*
git commit -m "Deploy to GitHub Pages"

# 推送分支
git push -u origin gh-pages
```

#### 3. 配置 GitHub Pages
1. 进入 GitHub 仓库 → Settings → Pages
2. 选择 `gh-pages` 分支作为源
3. 保存配置
4. 等待几分钟，访问 https://your-username.github.io/repository-name

### 方法二：部署后端服务（使用外部平台）

由于 GitHub Pages 只支持静态网站，后端需要部署到其他服务：

#### 推荐使用 Render（免费）
1. 访问 https://render.com 注册账号
2. 点击 "New Web Service"
3. 连接您的 GitHub 仓库
4. 配置：
   - **Build Command**: `pip install -r requirements.txt`
   - **Start Command**: `python main.py`
5. 点击 "Create Web Service"
6. 部署完成后，获取后端 API 地址

#### 配置前端 API 地址
1. 打开前端配置文件（如 `vite.config.js`）
2. 更新代理配置，指向 Render 提供的后端地址
3. 重新构建并部署前端

## 四、验证部署

### 1. 检查 GitHub 仓库
- 确认代码已成功上传
- 检查 gh-pages 分支是否存在
- 检查 GitHub Pages 配置是否正确

### 2. 访问部署的网站
- **前端**：https://your-username.github.io/repository-name
- **后端**：https://your-service.onrender.com

### 3. 测试功能
- 注册/登录
- 上传/下载文件
- 测试其他功能

## 五、常见问题及解决方案

### 1. 推送失败
- **原因**：GitHub 仓库不存在、权限不足、网络连接问题
- **解决**：检查仓库地址、确认权限、检查网络连接

### 2. 部署后网站无法访问
- **原因**：GitHub Pages 配置错误、构���产物不完整、后端服务未启动
- **解决**：检查 GitHub Pages 配置、重新构建前端、检查后端服务状态

### 3. API 调用失败
- **原因**：后端服务未运行、CORS 配置错误、API 地址配置错误
- **解决**：启动后端服务、配置 CORS、检查 API 地址

### 4. 依赖安装失败
- **原因**：网络连接问题、依赖版本冲突
- **解决**：检查网络连接、更新依赖版本

## 六、维护和更新

### 1. 更新代码

```bash
# 拉取最新代码
git pull

# 修改代码

# 提交更改
git add .
git commit -m "Update: 更改描述"

# 推送代码
git push
```

### 2. 更新前端部署

```bash
# 构建前端
npm run build

# 重新部署
gh-pages -d dist
```

### 3. 更新后端部署
- Render 会自动检测代码变更并重新部署
- 或手动触发部署

## 七、最佳实践

1. **版本控制**：定期提交代码，使用有意义的提交信息
2. **分支管理**：使用分支开发新功能，合并到主分支
3. **文档**：保持 README.md 最新，记录项目使用方法
4. **安全**：不要上传敏感信息，使用环境变量存储配置
5. **性能**：优化代码，减少构建产物大小
6. **监控**：监控服务状态，及时处理错误

## 八、示例项目结构

```
project/
├── backend/          # 后端代码
│   ├── main.py       # 主应用
│   ├── requirements.txt # 依赖
│   └── storage/      # 存储目录
├── frontend/         # 前端代码
│   ├── src/          # 源代码
│   ├── dist/         # 构建产物
│   └── package.json  # 项目配置
├── .gitignore        # Git 忽略文件
└── README.md         # 项目说明
```

## 九、联系方式

如有问题或建议，请联系：
- GitHub: https://github.com/your-username/repository-name

---

通过本指南，您可以成功将项目上传到 GitHub 并部署，使其他人可以访问和使用您的项目。