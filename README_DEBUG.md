# 登录问题调试指南

## 问题症状
登录页面的"选择姓名"下拉列表没有显示用户选项

## 可能的原因和解决方案

### 1. 检查浏览器控制台
打开浏览器的开发者工具（F12），查看Console标签页：

**正常情况下应该看到：**
```
开始加载用户列表...
正在从服务器加载用户列表...
API地址: https://web-production-8b7a9.up.railway.app/auth/users
服务器响应状态: 200
服务器返回数据: {users: Array(xx)}
开始填充 xx 个用户到下拉列表...
处理用户 1: 姓名=xxx, 部门=xxx
...
✓ 成功加载 xx 个用户到下拉列表
用户列表加载完成
```

**如果看到错误信息：**
- `HTTP 404/500` → 后端API问题，检查服务器是否运行
- `CORS error` → 跨域问题，需要后端配置CORS
- `网络错误` → 检查网络连接
- `找不到用户选择下拉框元素` → DOM加载问题

### 2. 检查API响应
在浏览器中直接访问：
```
https://web-production-8b7a9.up.railway.app/auth/users
```

**应该返回类似这样的数据：**
```json
{
  "users": [
    {
      "name": "张三",
      "department": "销售部",
      "role": "销售"
    },
    ...
  ]
}
```

### 3. 手动测试
打开浏览器控制台，执行以下命令测试：

```javascript
// 检查下拉框元素是否存在
document.getElementById('userSelect')

// 手动调用加载用户函数
loadUsers()

// 检查state中的用户数据
state.users
```

### 4. 网络检查
在开发者工具的 Network 标签页中：
1. 刷新页面
2. 查找 `/auth/users` 请求
3. 检查请求状态（Status）
4. 查看响应内容（Response）

### 5. 本地开发模式
如果是本地开发，修改 `index.html` 中的API地址：

找到这一行：
```javascript
const CONFIG = {
    API_BASE_URL: 'https://web-production-8b7a9.up.railway.app',
    // API_BASE_URL: 'http://localhost:8000', // 本地开发时使用
};
```

改为：
```javascript
const CONFIG = {
    // API_BASE_URL: 'https://web-production-8b7a9.up.railway.app',
    API_BASE_URL: 'http://localhost:8000', // 本地开发时使用
};
```

### 6. 清除缓存
有时候浏览器缓存会导致问题：
1. 按 Ctrl + Shift + Delete
2. 清除浏览器缓存
3. 刷新页面（Ctrl + F5 强制刷新）

### 7. 检查localStorage
打开控制台执行：
```javascript
// 查看已保存的用户信息
localStorage.getItem('user')

// 如果有问题，清除它
localStorage.removeItem('user')
```

## 常见错误及解决方案

### 错误1: "加载用户列表失败: HTTP 404"
**原因**: 后端服务器没有运行或API路径错误
**解决**: 
1. 检查后端服务是否启动
2. 确认API地址是否正确

### 错误2: "CORS policy error"
**原因**: 跨域请求被阻止
**解决**: 需要在后端添加CORS配置，允许前端域名访问

### 错误3: 下拉列表显示"请选择..."但没有其他选项
**原因**: 
- API返回的用户数组为空
- 用户数据格式不正确
- 用户name字段为空

**解决**: 
1. 检查控制台日志看具体是哪个原因
2. 检查飞书表格中是否有用户数据
3. 确认用户数据的字段名称正确（name, department, role）

### 错误4: "找不到用户选择下拉框元素"
**原因**: JavaScript代码在DOM加载完成前执行
**解决**: 代码已经修复，使用DOMContentLoaded事件

## 最新修复内容

已修复的问题：
1. ✅ 确保在DOM加载完成后才初始化
2. ✅ 添加详细的调试日志
3. ✅ 优化错误提示信息
4. ✅ 分离登录表单初始化逻辑

## 如果问题仍未解决

请提供以下信息：
1. 浏览器控制台的完整日志
2. Network标签中 `/auth/users` 请求的详细信息
3. 使用的浏览器和版本
4. 是否是本地开发还是生产环境

## 快速测试脚本

在浏览器控制台粘贴运行：
```javascript
console.log('=== 登录问题诊断 ===');
console.log('1. userSelect元素:', document.getElementById('userSelect'));
console.log('2. userSelect内容:', document.getElementById('userSelect')?.innerHTML);
console.log('3. state.users:', window.state?.users);
console.log('4. localStorage user:', localStorage.getItem('user'));

fetch('https://web-production-8b7a9.up.railway.app/auth/users')
  .then(r => r.json())
  .then(d => console.log('5. API响应:', d))
  .catch(e => console.error('6. API错误:', e));
```
