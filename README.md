# 🧠 Personal Long-Term Memory Agent (Qwen Multi-Modal)

A multi-modal, AI-powered personal memory system that can ingest and understand **text, images, videos, and audio**, then store them as unified semantic embeddings for long-term retrieval and reasoning.

Powered by:

- **Qwen-VL** for image & video captioning
- **Qwen-long** for summarization
- **text-embedding-v4** for vector representation
- **Whisper** for local audio transcription
- **Optional YOLO** for object detection in video keyframes

This project provides:

- 🔍 **Cross-modal semantic search**
- 🧠 **RAG-enabled QA**
- 🎞 **Enhanced video understanding** (timeline, chapters, captions)
- 🎧 **Audio understanding via Whisper**
- 🖥 **Streamlit Web UI**
- 🧰 **CLI toolkit**
- ♻ **Incremental memory updates**

------

## 🌟 Features

### 🧩 Multi-Modal Ingestion

| Modality | Processing Pipeline                                          | Description                    |
| -------- | ------------------------------------------------------------ | ------------------------------ |
| Text     | text-embedding-v4                                            | Direct embedding               |
| Image    | Qwen-VL image captioning → summary → embedding               | High-quality description       |
| Video    | Keyframes → Qwen-VL caption → YOLO detection → timeline → chapter segmentation → summary | *Richest multi-modal pipeline* |
| Audio    | Whisper transcription → Qwen summary → embedding             | Local audio ASR                |

------

## 🎞 Enhanced Video Pipeline

The video ingestion pipeline includes:

- **Adaptive keyframe extraction** (based on video length)

- **Qwen-VL caption for every keyframe**

- **Optional YOLO object detection**

- **Structured timeline**:

  ```
  00:00 → A man walks by the sea
  00:15 → Camera zooms on a child playing
  ...
  ```

- **Timeline summary**

- **Automatic chapters** (title + summary)

- **One-sentence overall video summary (Qwen-long)**

- **text-embedding-v4 embedding**

------

## 🎧 Whisper Audio Pipeline

- Local audio transcription (no upload, no privacy leakage)
- One-sentence summary via Qwen-long
- Embedding with text-embedding-v4

------

## 🔍 Cross-Modal Semantic Retrieval

Ask natural language questions like:

- “When did I go to the beach?”
- “Which meeting mentioned project deadlines?”
- “Where are the images containing birthday cake?”

The system retrieves relevant **text, images, videos, and audio** together.

------

# 📦 Installation

```bash
git clone <repo>
cd Personal-Long-Term-Memory-Agent
pip install -r requirements.txt
```

Install Whisper:

```bash
pip install openai-whisper
brew install ffmpeg     # macOS
sudo apt install ffmpeg # Ubuntu
```

Optional YOLO:

```bash
pip install ultralytics
```

------

# 🔐 API Configuration

This project uses **Alibaba DashScope (Qwen) OpenAI-compatible API**.

```bash
export DASHSCOPE_API_KEY="your-key"
export PYTHONPATH=./src
```

------

# 📁 Prepare Memory Data

Example directory:

```
memory_data/
    notes.txt
    meeting.m4a
    holiday_trip.mp4
    birthday_photo.jpg
```

Files are automatically classified by type.

------

# 🏗 Build Memory Index

```bash
python -m memory_agent.cli.cli_qwen index \
    --root ./memory_data \
    --out memory_index.pt
```

Example output:

```
Found 10 files
Ingesting: holiday_trip.mp4
Ingesting: meeting.m4a
Ingesting: birthday_photo.jpg
Index saved: memory_index.pt
```

------

# 🖥 Run Web Interface

```bash
streamlit run src/memory_agent/web/app.py
```

The Web UI provides:

- Cross-modal search
- Video playback + timeline + chapters
- Keyframe gallery
- Audio playback
- Text preview

------

# 🧰 CLI Usage

### Semantic search:

```bash
python -m memory_agent.cli.cli_qwen search \
    --index memory_index.pt \
    --query "beach trip"
```

### RAG question answering:

```bash
python -m memory_agent.cli.cli_qwen qa \
    --index memory_index.pt \
    --query "Summarize all my work meetings."
```

------

# 🧩 Multi-Modal Pipelines

## Text

```
text → text-embedding-v4
```

## Image

```
image → Qwen-VL caption → summary → embedding
```

## Video

```
video
→ adaptive keyframe extraction
→ Qwen-VL captions for each frame
→ YOLO object detection (optional)
→ timeline (timestamped descriptions)
→ timeline summary
→ chapters (title + summary for segments)
→ one-sentence overall summary (Qwen-long)
→ text-embedding-v4 embedding
```

## Audio

```
audio → Whisper transcription → Qwen summary → text-embedding-v4
```

All modalities are unified into **one shared semantic vector space**.

------

# 🧱 Project Structure

```
src/memory_agent/
    models/
        memory_index_qwen.py
        memory_item.py

    pipelines/
        ingest_text_qwen.py
        ingest_image_qwen.py
        ingest_audio_qwen.py
        ingest_video_qwen.py   # Enhanced video pipeline

    llm/
        qwen_client.py         # Qwen caption / summary / embedding
        local_whisper.py

    cli/
        cli_qwen.py

    web/
        app.py

    utils/
        file_utils.py
```

------

# ❓ FAQ

### 1. Do large images exceed the data-uri limit?

No — all images are automatically:

- resized (max side 768px)
- JPEG-compressed (quality=80)

Guaranteed to fit DashScope’s 10MB limit.

------

### 2. Whisper fails due to missing `ffmpeg`?

Install it:

```
brew install ffmpeg
sudo apt install ffmpeg
```

------

### 3. YOLO is required?

No.
 If YOLO is not installed, object detection is simply skipped.

------

### 4. Can the memory index be incrementally updated?

Yes — Web UI supports uploading new files to extend the index.

------

