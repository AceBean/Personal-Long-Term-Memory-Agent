# 🧠 Personal Long-Term Memory Agent (Qwen Multi-Modal)

# 📘 个人长期记忆智能体（通义千问多模态版）

------

# 🌐 Overview | 项目简介

**Personal Long-Term Memory Agent** 是一个能够理解 **文本、图片、视频、音频** 的多模态长期记忆系统。本系统使用 **通义千问 Qwen 系列模型** 实现跨模态语义表示与检索，同时本地支持 Whisper 音频识别，实现高质量的个人知识库与回忆辅助。

系统核心能力：

- 多模态 ingest：文本 / 图片 / 视频 / 音频
- 统一 embedding：所有模态最终压缩为 **一句话摘要 + text-embedding-v4（1024维）**
- 跨模态检索：输入一句自然语言即可检索所有模态
- 视频增强：关键帧抽取、caption、timeline、章节分段
- Whisper 本地音频转写（不上传文件）
- Web UI（Streamlit）
- CLI 工具（index / search / QA）
- 支持增量更新索引

------

# ✨ Features | 功能亮点

## 🚀 Multi-Modal Ingest（多模态处理）

| 模态 | 处理方式                                                   | 描述           |
| ---- | ---------------------------------------------------------- | -------------- |
| 文本 | text-embedding-v4                                          | 直接编码       |
| 图片 | Qwen-VL caption → embedding                                | 无需 OCR       |
| 视频 | 关键帧 caption + YOLO 检测 + timeline + chapters + summary | 完整增强       |
| 音频 | Whisper → Qwen summary → embedding                         | 全本地音频识别 |

------

## 🎞 Enhanced Video Pipeline（增强视频处理）

- 自适应关键帧采样（按视频时长动态调整）
- Qwen-VL caption（浓缩语义）
- 可选 YOLO 物体检测
- Timeline（00:00 → 场景描述）
- Timeline Summary（一句话总结整段视频时间轴）
- 章节自动划分（标题 + 概述）
- Qwen-long 一句话视频摘要
- text-embedding-v4 embedding

------

## 🎧 Whisper Audio Pipeline（音频处理）

- Whisper 本地识别（不上传文件）
- Qwen-long 一句话总结
- text-embedding-v4 encoding

------

## 🔍 Cross-Modal Semantic Retrieval（跨模态语义检索）

例如：

- “我什么时候去过海边？”
- “有哪段会议里我提到XXX？”
- “生日蛋糕在哪里出现过？”

系统会自动检索文本、图片、视频、音频。

------

## 🖥 Web UI（网页界面）

- 搜索栏
- 视频播放器
- Timeline 展示
- 视频章节（标题+摘要）
- 关键帧画廊（含 caption / YOLO objects）
- 音频播放器
- 文本全文预览

------

# 📦 Installation | 安装

```bash
git clone <repo>
cd Personal-Long-Term-Memory-Agent
pip install -r requirements.txt
```

Whisper 需要安装：

```bash
pip install openai-whisper
brew install ffmpeg   # macOS
sudo apt install ffmpeg
```

YOLO（可选）：

```bash
pip install ultralytics
```

------

# 🔐 API Configuration | 配置 API Key

使用阿里百炼（DashScope）OpenAI 兼容接口：

```bash
export DASHSCOPE_API_KEY="your-key"
export PYTHONPATH=./src
```

------

# 📁 Data Preparation | 数据准备

创建一个资料目录：

```
memory_data/
    notes.txt
    holiday.mp4
    meeting.m4a
    birthday.jpg
```

系统自动识别文件类型。

------

# 🏗 Build Memory Index | 构建记忆索引

```bash
python -m memory_agent.cli.cli_qwen index \
    --root ./memory_data \
    --out memory_index.pt
```

输出示例：

```
Found 12 files
Ingesting holiday.mp4
Ingesting meeting.m4a
Index saved: memory_index.pt
```

------

# 🖥 Run Web UI | 启动 Web 界面

```bash
streamlit run src/memory_agent/web/app.py
```

功能：

- 搜索所有模态
- 视频播放器 + timeline + chapters
- 音频播放器
- 关键帧画廊
- 文本内容自动展示

------

# 🧰 CLI Usage | 命令行工具

### 搜索：

```bash
python -m memory_agent.cli.cli_qwen search \
    --index memory_index.pt \
    --query "海边的内容"
```

### RAG 问答：

```bash
python -m memory_agent.cli.cli_qwen qa \
    --index memory_index.pt \
    --query "总结我所有的旅行经历"
```

------

# 🧩 Multi-Modal Pipeline (Final)

# 🧩 最终版多模态处理流程

## 📄 Text 文本

```
text → text-embedding-v4
```

## 🖼 Image 图片

```
image → Qwen-VL caption → summary → embedding
```

## 🎞 Video 视频（增强版）

```
Video  
→ Keyframe Extraction (Adaptive)  
→ Qwen-VL caption  
→ YOLO object detection (optional)  
→ Timeline (00:00 → 描述)  
→ Timeline Summary（总结时间线）  
→ Chapters（标题 + 概述）  
→ Video One-Sentence Summary  
→ text-embedding-v4 embedding
```

## 🎧 Audio 音频

```
audio → Whisper transcription  
      → Qwen Summary  
      → text-embedding-v4
```

最终所有模态统一到 **一个语义向量空间（1024维）**。

------

# 🏛 Project Structure | 项目结构

```
src/memory_agent/
    models/
        memory_index_qwen.py
        memory_item.py

    pipelines/
        ingest_text_qwen.py
        ingest_image_qwen.py
        ingest_audio_qwen.py
        ingest_video_qwen.py   ← 视频增强逻辑（无 OCR）

    llm/
        qwen_client.py         ← Qwen caption/summary/embedding
        local_whisper.py       ← Whisper ASR

    cli/
        cli_qwen.py

    web/
        app.py

    utils/
        file_utils.py
```

------

# ❓ FAQ

### 1. 图片太大会报错？

不用担心，我们自动：

- 缩放到最长边 768
- JPEG 压缩到 quality=80

绝不超过 10MB。

------

### 2. Whisper 无法使用？

请安装 ffmpeg：

```
brew install ffmpeg
sudo apt install ffmpeg
```

------

### 3. YOLO 必须安装吗？

不是。
 没有 YOLO → 自动跳过物体检测。

------

### 4. 支持增量更新吗？

支持。
 Web UI 可上传新文件并自动追加到索引。

------

