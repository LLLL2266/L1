# 01 · 参考视频分析

> 分析参考视频 → 提取逐镜头结构 → 生成AI视频复刻提示词

支持平台：即梦 Seedance 2.0 / 可灵 Kling 3.0 / Runway Gen-4

---

## 完整工作流

```
用户提供视频URL/文件
  ↓
1. 下载视频（yt-dlp → YouTube/IG/小红书/TikTok/B站）
2. 截取目标时长（ffmpeg）
3. 镜头检测（PySceneDetect / ffmpeg逐帧+视觉模型）
4. 提取关键帧（每镜头开始/中间/结束共3帧）
5. 音频转录（Whisper — 可选，有对白时使用）
6. 视觉分析每段镜头（豆包视觉模型/其他VLM）
7. 生成复刻提示词（Seedance/Kling/Runway格式）
8. 输出：逐镜头分析表 + 可直接粘贴的提示词
```

---

## 分步详解

### Step 0：收集输入
- 视频路径/URL
- 产品/角色描述（如有）
- 目标平台（默认即梦 Seedance 2.0）

### Step 1：下载视频
```bash
# YouTube / Instagram / 小红书 / TikTok
yt-dlp "URL" -o "video.mp4"

# B站
bili video BV_ID --yaml

# 本地文件 — 直接用路径
```

### Step 2：探查元数据
```bash
ffprobe -v quiet -print_format json -show_format -show_streams "video.mp4"
```
收集：时长(s)、分辨率、fps

### Step 3：镜头检测

**方法A — PySceneDetect（推荐）：**
```python
from scenedetect import detect, ContentDetector
scene_list = detect("video.mp4", ContentDetector(threshold=27.0))
```
输出：逐镜头时间码

**方法B — ffmpeg逐帧+视觉模型（Windows兼容）：**
```bash
ffmpeg -i "video.mp4" -vf "fps=1" frames/frame_%04d.jpg
```
然后用视觉模型识别场景切换点。

### Step 4：提取关键帧
每段镜头提取3帧（开始/中间/结束）：
```bash
ffmpeg -ss SHOT_START -i "video.mp4" -frames:v 1 frame_001_start.jpg
ffmpeg -ss SHOT_MID   -i "video.mp4" -frames:v 1 frame_001_mid.jpg
ffmpeg -ss SHOT_END   -i "video.mp4" -frames:v 1 frame_001_end.jpg
```

### Step 5：音频转录
```python
import whisper
model = whisper.load_model("base")
result = model.transcribe("audio.wav")
```

### Step 6：视觉分析（每段镜头）

用视觉模型（优先豆包 doubao-seed-2-0-pro-260215）分析每段镜头的3帧，输出JSON：

```json
{
  "shot_id": 1,
  "timecode": "00:00 - 00:04",
  "duration_sec": 4,
  "shot_size": "全景/中景/特写",
  "angle": "平视/俯拍/仰拍/过肩",
  "camera_movement": "固定/推/拉/摇/移/跟/手持/复合",
  "movement_detail": "镜头缓慢向右平移，跟随人物走向",
  "composition": "主体置于画面左1/3，右侧留白，前景有虚化树叶",
  "lighting": "日间自然逆光偏暖",
  "color_palette": ["#2c5b8c", "#5b6b52"],
  "subjects": ["女孩", "自行车"],
  "subject_action": "推着自行车在湖边小路缓慢停步，朝远处张望",
  "setting": "湖边树荫小路，背景有湖面和远山",
  "mood": ["静谧", "梦境感"],
  "narrative_function": "建立场景氛围/引入角色",
  "transcript": "今天天气真好...",
  "notable_techniques": "柳树垂枝形成上方框景"
}
```

### Step 7：生成复刻提示词

Seedance 格式模板：
```text
保持@图片1、@图片2、@图片3的画面主体、构图关系和光影氛围一致。

{主体}位于{场景}，{主体动作}。画面情绪偏{氛围}。

镜头以{景别}、{角度}呈现，{运镜描述}

保持人物/主体特征稳定，动作细节自然，不添加多余文字、水印或畸变肢体。
```

### Step 8：运镜判别（从三帧图片推断）

| 现象 | 运镜类型 |
|------|---------|
| 主体逐渐变大 | 推（push in） |
| 主体逐渐变小 | 拉（pull out） |
| 背景横向滑动 | 摇（pan） |
| 整个画面横向平移 | 移（track/dolly） |
| 镜头跟随主体运动 | 跟（follow） |
| 不规则抖动 | 手持（handheld） |
| 三帧几乎一样 | 固定（static） |
| 多种方式混合 | 复合（compound） |

---

## ⚠️ 常见坑

| 问题 | 解决方案 |
|------|---------|
| ffmpeg scene detect Windows报错 | 改用逐帧提取+视觉模型 |
| yt-dlp被B站屏蔽 | 用bili-cli代替 |
| Seedance拒绝真人脸 | 换可灵 Kling 3.0 |
| 提示词>260词 | 压缩场景描述，保留节奏骨架 |
| 源视频>15秒 | 在自然节奏点拆分 |
| 内容检查拦截 | 收紧运动描述，检查禁用词 |

---

## 依赖

```bash
pip install yt-dlp scenedetect openai-whisper bilibili-cli
```
ffmpeg：从 https://ffmpeg.org/download.html 下载并加入PATH
