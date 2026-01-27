# 智能商机助手 - 前端

基于纯 HTML/CSS/JavaScript 的智能商机管理系统前端界面。

## 功能特性

- 身份登录（销售/管理员）
- 实时聊天界面
- 商机录入、更新、查询
- 快捷操作卡片
- 响应式设计
- 美观的 UI 设计

## 技术栈

- HTML5
- CSS3（Flexbox、Grid、动画）
- 原生 JavaScript（无框架）
- Google Fonts（Noto Sans SC + Inter）

## 本地运行

### 方法一：直接打开

直接在浏览器中打开 `index.html` 文件。

### 方法二：使用本地服务器（推荐）

```bash
cd opportunity-bot-frontend

# 使用 Python 3
python -m http.server 3000

# 或使用 Python 2
python -m SimpleHTTPServer 3000

# 或使用 Node.js http-server
npx http-server -p 3000
```

然后访问：http://localhost:3000

## 配置 API 地址

打开 `index.html`，找到 `CONFIG` 对象（约在第 650 行）：

```javascript
const CONFIG = {
    // 本地开发时用这个
    API_BASE_URL: 'http://localhost:8000',
    
    // 生产环境部署时，把上面注释掉，用下面这个
    // API_BASE_URL: 'https://your-backend.railway.app',
};
```

### 本地开发

保持默认配置：
```javascript
API_BASE_URL: 'http://localhost:8000',
```

确保后端服务已在 http://localhost:8000 运行。

### 生产部署

修改为后端的 Railway 域名：
```javascript
API_BASE_URL: 'https://your-backend.railway.app',
```

## 部署到 Vercel

### 方法一：通过 GitHub（推荐）

1. 创建 GitHub 仓库并上传代码：

```bash
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/your-username/opportunity-bot-frontend.git
git push -u origin main
```

2. 登录 [vercel.com](https://vercel.com)

3. 点击 **Add New** → **Project**

4. 选择 **Import Git Repository**，选择你的仓库

5. 配置项目：
   - Framework Preset: 选择 **Other**
   - Root Directory: `.`（默认）
   - Build Command: 留空
   - Output Directory: `.`（默认）

6. 点击 **Deploy**

7. 部署完成后，Vercel 会自动生成域名（如 `https://your-app.vercel.app`）

### 方法二：通过 Vercel CLI

```bash
# 安装 Vercel CLI
npm install -g vercel

# 登录
vercel login

# 部署
vercel

# 生产部署
vercel --prod
```

### 重要：部署前修改 API 地址

**在部署到 Vercel 之前，必须先修改 `index.html` 中的 `API_BASE_URL`！**

1. 打开 `index.html`
2. 找到 `CONFIG` 对象
3. 注释掉本地开发的配置，启用生产配置：

```javascript
const CONFIG = {
    // 本地开发时用这个
    // API_BASE_URL: 'http://localhost:8000',
    
    // 生产环境部署时，把上面注释掉，用下面这个
    API_BASE_URL: 'https://your-backend.railway.app',  // 替换成你的后端域名
};
```

4. 保存文件，然后部署

## UI 配色方案

- 主色：蓝色 `rgb(46, 84, 161)`
- 辅助色：白色 `#ffffff`
- 点缀色：绿色 `#06c863`、青色 `#00d4ff`
- 背景：浅灰 `#f5f7fa`
- 文字：深灰 `#1a1a1a`

## 功能说明

### 登录页面

- 选择身份（销售/管理员）
- 从下拉列表选择姓名
- 管理员需要输入密钥验证

### 主界面

- **侧边栏**：
  - Logo 和新建对话按钮
  - 历史对话列表（当前版本未实现持久化）
  - 用户信息显示（姓名、角色、部门）
  - 退出登录按钮

- **聊天区**：
  - 欢迎页：显示快捷功能卡片
  - 消息列表：显示用户和 AI 的对话
  - AI 回复时显示加载动画

- **输入区**：
  - 多行文本输入框（自动高度调整）
  - Enter 发送，Shift+Enter 换行
  - 发送按钮

### 快捷功能

点击快捷卡片可以直接发送预设消息：
- 录入新商机
- 更新商机
- 查询商机
- 超时提醒

## 响应式设计

- 桌面端（>768px）：显示侧边栏
- 移动端（≤768px）：隐藏侧边栏，全屏聊天

## 浏览器兼容性

- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

## 常见问题

### 1. 如何修改 API 地址？

编辑 `index.html`，修改 `CONFIG.API_BASE_URL` 的值。

### 2. 登录后刷新页面会退出吗？

不会。登录状态保存在 `localStorage` 中，刷新页面会自动恢复。

### 3. 如何清除登录状态？

点击侧边栏底部的"退出"按钮，或手动清除浏览器的 `localStorage`。

### 4. 为什么无法连接后端？

检查以下几点：
- 后端服务是否正常运行
- `API_BASE_URL` 配置是否正确
- 浏览器控制台是否有 CORS 错误（需要后端配置 CORS）

### 5. 如何自定义样式？

所有样式都在 `<style>` 标签中，使用 CSS 变量管理颜色：

```css
:root {
    --primary-color: rgb(46, 84, 161);
    --accent-green: #06c863;
    /* ... */
}
```

修改这些变量即可快速调整配色。

## 许可证

MIT
