# 🎬 L1 — AI 视频创作一站式技能库

> **L1 = Layer 1** — AI 视频创作的第一层基础设施

整合三个顶级开源技能，形成从"参考视频分析 → 提示词生成 → 导演级控制"的完整视频创作管线。

---

## 📦 三大模块

```
L1/
├── 01-参考视频分析/       🎯 视频拆解 + 分镜提取 + 同款提示词
├── 02-提示词工程/         ✍️ 即梦/Seedance 2.0 提示词生成体系
├── 03-导演级控制/         🎬 28子skill镜头/光线/运动/序列控制
├── 工作流模板/            🔗 三大模块组合使用场景
└── 参考资源/              📚 补充资料与词库
```

### 模块① · 参考视频分析

**来源:** `video-reproduction-analysis`（原创整理）

把一段参考视频拆解成：
- 逐镜头分镜表（景别/角度/运镜/构图/光线/色调）
- 音频转录（Whisper）+ 时间轴对齐
- 每段镜头的复制提示词（Seedance/Kling/Runway格式）
- 整体结构分析（节奏模式、钩子分析）

支持：YouTube / 小红书 / 抖音 / B站 / Instagram / 本地文件

### 模块② · 提示词工程

**来源:** `seedance-prompt-skill` ⭐2216

专为即梦 Seedance 2.0 写中文提示词。十大核心能力：
1. 纯文本生成 2. 一致性控制 3. 运镜复刻 4. 特效复刻
5. 剧情创作 6. 视频延长 7. 声音控制 8. 一镜到底
9. 视频编辑 10. 音乐卡点

内置 @引用系统（@图片1~9、@视频1~3、@音频1~3）、时间戳分镜法、超长视频拼接策略。

### 模块③ · 导演级控制

**来源:** `seedance-20`（28子skill系统）

每种子能力一个独立skill，按需调用：

| 领域 | 子skill |
|------|---------|
| 提示词写作 | seedance-prompt / seedance-prompt-short |
| 镜头语言 | seedance-camera |
| 光线设计 | seedance-lighting |
| 运动/动作 | seedance-motion |
| 音频/对白 | seedance-audio |
| 角色一致性 | seedance-characters |
| 多镜头序列 | seedance-sequence |
| 续接扩展 | seedance-continuation |
| 风格迁移 | seedance-style |
| 去油腻 | seedance-antislop |
| 模板库 | seedance-recipes |
| 特效 | seedance-vfx |
| 版权安全 | seedance-copyright / seedance-filter |
| 多语言词库 | 中/英/日/韩/西/俄 |

---

## 🔗 推荐工作流

### 流程A：参考视频 → 同款提示词

```
你看到一条不错的视频 → 
  L1/01-参考视频分析 → 拆解分镜 →
  L1/02-提示词工程 → 包装为即梦格式 →
  L1/03-导演级控制 → 微调镜头/光线/动作 →
  粘贴到即梦/可灵生成
```

### 流程B：从零创作

```
我有一个产品/人物/场景 →
  L1/02-提示词工程 → 写提示词草稿 →
  L1/03-导演级控制 → 加入镜头运动/光线方案 →
  生成测试 →
  不满意 → L1/02 迭代提示词
```

### 流程C：即梦 vs 可灵 路由

```
单套衣服 + 复杂动作 → 可灵 Kling 3.0（角色参考+动作模板）
多套衣服 + 简单动作 → 即梦 Seedance（@服装 @人物 多参考）
```

---

## 🛠 前置依赖

| 工具 | 用途 |
|------|------|
| ffmpeg | 视频抽帧、截取、场景检测 |
| yt-dlp | 下载各平台视频 |
| PySceneDetect | 精确镜头边界检测（可选） |
| openai-whisper | 音频转录（可选） |

```bash
pip install yt-dlp scenedetect openai-whisper
# ffmpeg: 从 https://ffmpeg.org/download.html 下载并加入PATH
```

---

## 📜 许可 & 致谢

- `02-提示词工程` 基于 [songguoxs/seedance-prompt-skill](https://github.com/songguoxs/seedance-prompt-skill) ⭐2216 — MIT
- `03-导演级控制` 基于 [Emily2040/seedance-2.0](https://github.com/Emily2040/seedance-2.0) ⭐107 — MIT
- `01-参考视频分析` 原创整理，综合 open-source 方法论
