# 03 · 导演级控制

> 28个子skill，精调视频的每一帧

整合自 ⭐107 开源项目 [Emily2040/seedance-2.0](https://github.com/Emily2040/seedance-2.0)

---

## 子skill总览

### 📝 提示词写作

| 子skill | 用途 |
|---------|------|
| **seedance-prompt** | 从零构建完整提示词，导演公式：主体+动作+场景+镜头+光线+音频+约束 |
| **seedance-prompt-short** | 快速短提示词（40-110字），适合简单生成 |

### 📐 镜头语言

| 子skill | 用途 |
|---------|------|
| **seedance-camera** | 单一运镜设计：启动画面→移动→速度→主体关系→终点 |
| | 锁定镜头→对白/VFX；推→发现/实现；跟随→旅行/追踪 |
| | 环绕→全角度展示；升降/航拍→规模感；手持→真实感 |

### 💡 光线设计

| 子skill | 用途 |
|---------|------|
| **seedance-lighting** | 光线方案：主光/补光/轮廓光/背景光 |
| | 时间段光质（黄金时刻/蓝调时刻） |
| | 光源方向（侧光/逆光/顶光/底光/漫射） |

### 🏃 运动/动作

| 子skill | 用途 |
|---------|------|
| **seedance-motion** | 人物动作、身体运动、肢体语言 |
| | 走秀/转身/展示/抬手/行走/坐立等具体动作描述 |

### 🔊 音频/对白

| 子skill | 用途 |
|---------|------|
| **seedance-audio** | 音色参考、对白生成、音效设计、唇音同步 |

### 👤 角色一致性

| 子skill | 用途 |
|---------|------|
| **seedance-characters** | 跨片段保持角色外观统一 |
| | 利用@图片参考锁定角色特征 |

### 🎬 多镜头序列

| 子skill | 用途 |
|---------|------|
| **seedance-sequence** | 多片段时间线规划、状态继承 |
| | 每段一个叙事任务+明确的终点画面 |
| | 跨会话状态胶囊，下次生成无缝衔接 |

### 🔄 续接扩展

| 子skill | 用途 |
|---------|------|
| **seedance-continuation** | 从已生成视频的最后一帧继续延长 |
| | 保持运镜方向、动作节奏、光照一致性 |

### 🎨 风格迁移

| 子skill | 用途 |
|---------|------|
| **seedance-style** | 视觉风格迁移（商业摄影/自然生活/走秀/复古/赛博朋克等） |

### 🧹 去油腻

| 子skill | 用途 |
|---------|------|
| **seedance-antislop** | 清除提示词中的空洞修饰词 |
| | 替换"电影级"为具体的镜头参数 |

### 📋 模板库

| 子skill | 用途 |
|---------|------|
| **seedance-recipes** | 预设场景模板（电商/短剧/产品/科普/音乐视频） |

### ✨ 特效

| 子skill | 用途 |
|---------|------|
| **seedance-vfx** | 粒子特效、光效、转场特效、爆炸、魔法等 |

### 📖 中文案例与词库

| 子skill | 用途 |
|---------|------|
| **seedance-examples-zh** | 中文实战案例库 |
| **seedance-vocab-zh** | 中文镜头语言词汇表 |

### 🌐 多语言词库

| 子skill | 语言 |
|---------|------|
| seedance-vocab-en | 英语 |
| seedance-vocab-ja | 日语 |
| seedance-vocab-ko | 韩语 |
| seedance-vocab-es | 西班牙语 |
| seedance-vocab-ru | 俄语 |

### 🔒 版权与安全

| 子skill | 用途 |
|---------|------|
| **seedance-copyright** | IP安全改写、版权风险评估 |
| **seedance-filter** | 内容安全检查、敏感词过滤 |

### 🔧 工具链

| 子skill | 用途 |
|---------|------|
| **seedance-pipeline** | API调用、批量生成、队列管理 |
| **seedance-troubleshoot** | 出片诊断、失败分析、修复方案 |

---

## 选择指南

### 按场景选子skill

```
写提示词 → seedance-prompt + seedance-prompt-short
调整镜头 → seedance-camera
调光线  → seedance-lighting
加动作  → seedance-motion
加配音  → seedance-audio
保持角色一致 → seedance-characters
多段视频 → seedance-sequence + seedance-continuation
换风格  → seedance-style
去油腻  → seedance-antislop
用模板  → seedance-recipes
加特效  → seedance-vfx
```

### 按流程走

```
完整操作回路：

1. 收集目标/平台/模式/时长/宽高比/参考/音频/安全风险
2. 平台检查（即梦 vs 可灵）
3. 序列判断（单片段 → seedance-prompt / 多镜头 → seedance-sequence）
4. 模式选择（T2V/I2V/V2V/R2V/FLF2V）
5. 写提示词
6. 质量检查（seedance-antislop 去油腻 + 一致性检查）
7. 修复循环（seedance-troubleshoot）
```

---

## 核心原则

1. **听到意图背后的感觉** — 用户说"让它有家的感觉"时，翻译成具体的镜头/光线/色彩语言（暖色调/柔光/室内自然光/浅景深）
2. **保持故事状态** — 跨对话记住角色、场景、已决定的参数、已失败的方式
3. **适应使用者水平** — 对初学者说大白话，对导演说专业术语
