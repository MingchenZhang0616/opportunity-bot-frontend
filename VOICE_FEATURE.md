# 语音输入功能说明

## 当前状态
✅ **已禁用，显示"即将上线"提示**

语音按钮已保留在UI中，但处于禁用状态：
- 按钮呈灰色半透明显示
- 鼠标悬停显示"即将上线"提示
- 点击无响应（cursor: not-allowed）

## 为什么没有实现？

**Claude API本身不支持语音识别**
- Claude是文本对话模型，只能处理文本输入输出
- 语音转文字需要单独的语音识别服务
- 需要在前端或后端集成额外的语音识别API

## 未来实现方案

### 方案1：浏览器Web Speech API（推荐快速实现）

**优点：**
- 完全免费
- 无需后端支持
- 实现简单快速

**缺点：**
- 仅Chrome/Edge浏览器支持
- 需要联网（在线识别）
- 隐私问题（语音发送到Google服务器）
- 准确度一般

**实现代码示例：**
```javascript
// 检查浏览器支持
if ('webkitSpeechRecognition' in window || 'SpeechRecognition' in window) {
    const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
    const recognition = new SpeechRecognition();
    
    recognition.lang = 'zh-CN'; // 中文识别
    recognition.continuous = false; // 单次识别
    recognition.interimResults = false;
    
    recognition.onresult = (event) => {
        const transcript = event.results[0][0].transcript;
        document.getElementById('messageTextarea').value = transcript;
    };
    
    recognition.onerror = (event) => {
        console.error('语音识别错误:', event.error);
    };
    
    // 开始识别
    document.getElementById('voiceButton').addEventListener('click', () => {
        recognition.start();
    });
}
```

---

### 方案2：百度语音识别（推荐企业使用）

**优点：**
- 识别准确度高
- 支持中文各种方言
- 支持长语音识别
- 可离线使用（需额外配置）

**缺点：**
- 需要申请API密钥
- 需要付费（有免费额度）
- 需要后端支持

**费用：**
- 每天前5万次调用免费
- 超出部分：0.0018元/次

**实现步骤：**
1. 注册百度智能云账号
2. 创建语音识别应用
3. 获取API Key和Secret Key
4. 前端录音 → 发送音频到后端
5. 后端调用百度API转换文字
6. 返回文字给前端填充

**官方文档：**
https://ai.baidu.com/tech/speech/asr

---

### 方案3：讯飞语音识别

**优点：**
- 中文识别准确度极高
- 支持实时转写
- 支持各种方言和口音

**缺点：**
- 需要申请API
- 需要付费
- 需要后端支持

**费用：**
- 每天前500次免费
- 超出部分根据套餐收费

**官方文档：**
https://www.xfyun.cn/doc/asr/voicedictation/API.html

---

### 方案4：阿里云语音识别

**优点：**
- 企业级稳定性
- 支持多种场景（通用、客服等）
- 中文识别准确

**缺点：**
- 相对较贵
- 需要后端支持

**费用：**
- 按时长计费：0.00125元/秒
- 约0.075元/分钟

**官方文档：**
https://help.aliyun.com/document_detail/84428.html

---

## 推荐实现路径

### 阶段1：快速验证（1-2小时）
使用浏览器Web Speech API实现基础功能
- 前端直接调用
- 无需后端改动
- 仅支持Chrome/Edge
- 验证用户是否真的需要语音功能

### 阶段2：企业级部署（1-3天）
根据实际使用情况，选择百度或讯飞
- 申请API密钥
- 后端集成语音识别服务
- 前端录音上传
- 优化用户体验

### 阶段3：优化体验（持续）
- 添加实时语音转文字显示
- 支持长语音识别
- 添加降噪处理
- 支持方言识别

---

## 成本估算

**假设每天100个用户，每人使用5次语音输入：**

| 方案 | 月调用量 | 月成本 | 备注 |
|------|---------|--------|------|
| Web Speech API | 15,000次 | ¥0 | 免费但有限制 |
| 百度语音 | 15,000次 | ¥0 | 在免费额度内 |
| 讯飞语音 | 15,000次 | ~¥50 | 超出免费500次 |
| 阿里云 | 15,000次×30秒 | ~¥450 | 按时长计费 |

**推荐：百度语音识别**
- 性价比最高
- 准确度好
- 有充足的免费额度

---

## 实现优先级

当前建议：**暂不实现**

原因：
1. 大部分商机录入场景在办公室，打字更方便
2. 语音识别准确度不如人工输入
3. 需要额外的开发和维护成本
4. 用户可能更习惯打字

**触发实现条件：**
- 用户强烈要求语音功能
- 移动端使用场景增多
- 需要在开车等特殊场景下使用

---

## 如需启用，请联系开发团队

提供以下方案的完整实现：
- [ ] Web Speech API快速版（免费）
- [ ] 百度语音识别专业版（推荐）
- [ ] 讯飞语音识别高级版
- [ ] 阿里云语音识别企业版

预计实现时间：1-3天（取决于选择的方案）
