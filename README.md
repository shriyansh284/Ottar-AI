# 🎬 AI Video Assistant

> **Transform video meetings into actionable intelligence with AI-powered transcription, summarization, and conversational search**

An end-to-end meeting intelligence platform that automatically transcribes videos, extracts key insights, and enables natural language Q&A with your meeting content using Retrieval-Augmented Generation (RAG).

![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)
![License](https://img.shields.io/badge/License-MIT-green.svg)
![Streamlit](https://img.shields.io/badge/Streamlit-1.35.0+-red.svg)
![LangChain](https://img.shields.io/badge/LangChain-0.2.0+-orange.svg)

---

## ✨ Features

### 🎯 Core Capabilities
- **Multi-Source Input**: Process YouTube URLs or local video/audio files
- **Bilingual Transcription**: 
  - **English**: OpenAI Whisper (local, offline)
  - **Hinglish**: Sarvam AI (cloud-based, auto-translates to English)
- **Intelligent Summarization**: LangChain map-reduce pipeline with Mistral AI
- **Automated Extraction**:
  - ✅ Action items (with owners & deadlines)
  - 🔑 Key decisions
  - ❓ Open questions/follow-ups
- **RAG-Powered Chat**: Ask questions about your meetings in natural language
- **Modern Web UI**: Beautiful Streamlit interface with real-time pipeline status

### 🚀 Advanced Features
- **Chunked Processing**: Handles long videos (10-minute segments)
- **Vector Search**: ChromaDB + HuggingFace embeddings for semantic retrieval
- **LCEL Chains**: Modular LangChain Expression Language pipelines
- **Progress Tracking**: Live status updates for each processing stage

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     INPUT SOURCES                           │
│           YouTube URLs  │  Local Video Files                │
└─────────────┬───────────────────────────────────────────────┘
              │
              ▼
┌─────────────────────────────────────────────────────────────┐
│                  AUDIO PROCESSOR                            │
│        yt-dlp Download + pydub Conversion & Chunking        │
└─────────────┬───────────────────────────────────────────────┘
              │
              ▼
┌─────────────────────────────────────────────────────────────┐
│                   TRANSCRIBER                               │
│     Whisper (English)  │  Sarvam AI (Hinglish)              │
└─────────────┬───────────────────────────────────────────────┘
              │
              ▼
┌─────────────────────────────────────────────────────────────┐
│              LLM PROCESSING (Mistral AI)                    │
│  ┌──────────┬──────────────┬──────────────┬──────────────┐ │
│  │  Title   │  Summary     │  Action      │  Decisions   │ │
│  │  Gen     │  (Map-Reduce)│  Items       │  & Questions │ │
│  └──────────┴──────────────┴──────────────┴──────────────┘ │
└─────────────┬───────────────────────────────────────────────┘
              │
              ▼
┌─────────────────────────────────────────────────────────────┐
│                    RAG ENGINE                               │
│  Text Splitter → HuggingFace Embeddings → ChromaDB         │
│                 Semantic Retriever                          │
└─────────────┬───────────────────────────────────────────────┘
              │
              ▼
┌─────────────────────────────────────────────────────────────┐
│               STREAMLIT WEB INTERFACE                       │
│     Interactive UI + Chat + Results Display                 │
└─────────────────────────────────────────────────────────────┘
```

---

## 🛠️ Tech Stack

### Core Technologies
- **Python 3.10+**: Primary language
- **LangChain 0.2+**: LLM orchestration & RAG pipelines
- **Mistral AI**: Text generation & summarization
- **OpenAI Whisper**: Local speech-to-text
- **Sarvam AI**: Hinglish transcription & translation

### ML & Embeddings
- **ChromaDB**: Vector database
- **HuggingFace Sentence Transformers**: Text embeddings (`all-MiniLM-L6-v2`)
- **PyTorch**: Backend for Whisper

### Audio Processing
- **yt-dlp**: YouTube download
- **pydub**: Audio manipulation
- **FFmpeg**: Format conversion

### UI & Utilities
- **Streamlit**: Web interface
- **python-dotenv**: Environment management

---

## 📦 Installation

### Prerequisites

```bash
# Python 3.10 or higher
python --version

# FFmpeg (required for audio processing)
# Ubuntu/Debian:
sudo apt update && sudo apt install ffmpeg

# macOS:
brew install ffmpeg

# Windows: Download from https://ffmpeg.org/download.html
```

### Setup

1. **Clone the repository**
```bash
git clone https://github.com/AkarshVyas/AI-Video-Assistant-.git
cd AI-Video-Assistant-
```

2. **Create virtual environment**
```bash
python -m venv venv

# Activate (Linux/macOS):
source venv/bin/activate

# Activate (Windows):
venv\Scripts\activate
```

3. **Install dependencies**
```bash
pip install -r Requirements.txt
```

4. **Configure API keys**

Create a `.env` file in the root directory:

```env
# Required - Get from https://console.mistral.ai/
MISTRAL_API_KEY=your_mistral_api_key_here

# Optional - For Hinglish support (https://www.sarvam.ai/)
SARVAM_API_KEY=your_sarvam_api_key_here
SARVAM_STT_MODEL=saaras:v2.5

# Optional - Whisper model size (tiny/base/small/medium/large)
WHISPER_MODEL=small
```

---

## 🚀 Usage

### Streamlit Web App (Recommended)

```bash
streamlit run app.py
```

**Then:**
1. Open browser to `http://localhost:8501`
2. Enter YouTube URL or local file path in sidebar
3. Select language (English/Hinglish)
4. Click **⚡ Analyse**
5. View results & chat with your meeting!

![Demo Screenshot](https://via.placeholder.com/800x450.png?text=AI+Video+Assistant+Demo)

### CLI Mode

```bash
python main.py
```

**Interactive prompts:**
- Enter source (URL or file path)
- Choose language
- Get results printed to terminal
- Enter chat mode to ask questions

### Python API

```python
from main import run_pipeline

result = run_pipeline(
    source="https://youtube.com/watch?v=example",
    language="english"
)

print(result["title"])
print(result["summary"])
print(result["action_items"])

# Use RAG chain
from core.rag_engine import ask_question
answer = ask_question(result["rag_chain"], "What were the key decisions?")
```

---

## 📁 Project Structure

```
AI-Video-Assistant-/
│
├── app.py                  # Streamlit web interface
├── main.py                 # CLI entry point
├── Requirements.txt        # Python dependencies
│
├── core/                   # Core processing modules
│   ├── transcriber.py      # Whisper + Sarvam STT
│   ├── summarizer.py       # Map-reduce summarization
│   ├── extractor.py        # Action items/decisions/questions
│   ├── rag_engine.py       # RAG chain builder
│   └── vector_store.py     # ChromaDB vector storage
│
└── utils/                  # Utilities
    └── audio_processor.py  # Download/convert/chunk audio
```

---

## 🎨 Key Features Explained

### 1. Intelligent Audio Processing
- **Auto-detection**: YouTube URLs vs local files
- **Format conversion**: Standardizes to 16kHz mono WAV
- **Smart chunking**: 10-minute segments for efficient processing
- **Memory optimization**: Processes large videos without OOM errors

### 2. Dual-Language Transcription
- **English**: Uses local Whisper model (offline, no API costs)
- **Hinglish**: Sarvam AI automatically translates Hindi/Hinglish to English
- **Chunked Sarvam**: Handles 30s API limit by splitting into 25s pieces

### 3. LLM-Powered Extraction
All extraction uses **Mistral Small** via LangChain LCEL:
- **Title Generation**: Concise 8-word meeting titles
- **Map-Reduce Summarization**: Processes long transcripts in chunks
- **Structured Extraction**: Numbered lists with context

### 4. RAG Implementation
- **Text splitting**: 500-char chunks with 50-char overlap
- **Embeddings**: `all-MiniLM-L6-v2` (384 dimensions)
- **Retrieval**: Top-4 semantic similarity search
- **Context-aware answers**: LLM responds only from transcript context

---

## 📊 Example Output

### Transcript Summary
```
📌 Title: Q4 Product Roadmap Strategy Session

📋 Summary:
• Discussed upcoming feature releases for Q4
• Decided to prioritize mobile app development
• Allocated $150K budget for UI/UX redesign
• Planned sprint structure: 2-week cycles starting Nov 1

✅ Action Items:
1. Create technical spec for mobile app - Owner: Sarah - Deadline: Oct 25
2. Schedule design review meeting - Owner: Mike - Deadline: Not specified
3. Update project timeline in Jira - Owner: Team Lead - Deadline: This week

🔑 Key Decisions:
1. Move forward with React Native for mobile development
2. Postpone API v3 migration to Q1 2025
3. Hire additional frontend developer

❓ Open Questions:
1. Should we support iOS 14 or iOS 15 minimum?
2. What's the fallback plan if design review gets delayed?
```

### RAG Chat Example
```
You: Who is responsible for the mobile app technical spec?
🤖 Assistant: Sarah is responsible for creating the technical spec 
for the mobile app, with a deadline of October 25.

You: What was decided about the API migration?
🤖 Assistant: The team decided to postpone the API v3 migration to Q1 2025.
```

---

## 🔧 Configuration

### Environment Variables

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `MISTRAL_API_KEY` | ✅ Yes | - | Mistral AI API key |
| `SARVAM_API_KEY` | ⚠️ For Hinglish | - | Sarvam AI API key |
| `WHISPER_MODEL` | ❌ No | `small` | Whisper model: tiny/base/small/medium/large |
| `SARVAM_STT_MODEL` | ❌ No | `saaras:v2.5` | Sarvam STT model version |

### Performance Tuning

**Whisper Model Selection:**
- `tiny`: Fastest, lowest accuracy (~1GB RAM)
- `base`: Good balance (~1GB RAM)
- `small`: **Recommended** (~2GB RAM)
- `medium`: Better accuracy (~5GB RAM)
- `large`: Best accuracy (~10GB RAM)

**Chunk Size (audio_processor.py):**
```python
chunk_minutes = 10  # Adjust based on memory (5-15 recommended)
```

**RAG Retrieval:**
```python
k = 4  # Number of chunks retrieved (2-6 recommended)
```

---

## 🎯 Use Cases

- **Meeting Documentation**: Automatically generate meeting minutes
- **Video Archival**: Make old recordings searchable
- **Training Content**: Extract key points from instructional videos
- **Podcast Processing**: Transcribe & summarize podcast episodes
- **Compliance**: Maintain searchable records of important discussions
- **Research**: Analyze interview transcripts

---

## 🐛 Troubleshooting

### Common Issues

**1. FFmpeg not found**
```bash
# Verify installation
ffmpeg -version

# If missing, reinstall (see Prerequisites)
```

**2. Whisper model download slow**
- Models download on first run (~150MB for 'small')
- Stored in `~/.cache/whisper/`
- Switch to `tiny` model for faster testing

**3. Mistral API rate limits**
- Free tier: ~20 requests/minute
- Add delays between requests or upgrade plan

**4. Sarvam API errors**
- Verify API key in `.env`
- Check audio chunks are ≤30 seconds
- Ensure proper WAV format (16kHz mono)

**5. ChromaDB persistence issues**
```bash
# Clear vector database
rm -rf vector_db/
```

**6. Memory errors with large videos**
- Reduce `chunk_minutes` in `audio_processor.py`
- Use smaller Whisper model (`tiny` or `base`)
- Process shorter video segments

---

## 🚧 Roadmap

- [ ] Multi-speaker diarization (speaker labels)
- [ ] Export to PDF/Word formats
- [ ] Support for additional languages (Spanish, French, etc.)
- [ ] Real-time processing for live meetings
- [ ] Integration with Zoom/Google Meet APIs
- [ ] Sentiment analysis
- [ ] Custom prompt templates
- [ ] Docker containerization
- [ ] Web API for programmatic access
- [ ] Batch processing multiple videos

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

### Development Guidelines
- Follow PEP 8 style guide
- Add docstrings to all functions
- Update tests for new features
- Update documentation as needed

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- **OpenAI Whisper** for state-of-the-art speech recognition
- **Mistral AI** for powerful LLM capabilities
- **LangChain** for elegant LLM orchestration
- **Sarvam AI** for Hinglish support
- **Streamlit** for rapid UI development
- **ChromaDB** for efficient vector storage
- **HuggingFace** for embedding models

---

## 📧 Contact

**Akarsh Vyas**
- GitHub: [@AkarshVyas](https://github.com/AkarshVyas)
- Project Link: [https://github.com/AkarshVyas/AI-Video-Assistant-](https://github.com/AkarshVyas/AI-Video-Assistant-)

---

## 💡 FAQ

**Q: Can I use this without API keys?**  
A: Yes, for English-only transcription using Whisper (local). You'll need Mistral API key for summarization features.

**Q: How much does it cost to run?**  
A: Mistral API: ~$0.002-0.006 per meeting. Whisper is free (local). Sarvam pricing varies.

**Q: Can it process live meetings?**  
A: Not yet. Currently supports pre-recorded videos/audio only. Live processing is on the roadmap.

**Q: What video formats are supported?**  
A: Any format FFmpeg supports: MP4, AVI, MKV, MOV, WEBM, etc.

**Q: Can I use a different LLM?**  
A: Yes! Modify `core/summarizer.py` and `core/extractor.py` to use OpenAI, Anthropic, or other LangChain-compatible models.

**Q: Is my data private?**  
A: Local processing (Whisper) is fully private. Mistral/Sarvam APIs receive transcript text. Check their privacy policies.

---

## ⭐ Star History

If you find this project useful, please consider giving it a star! ⭐

[![Star History Chart](https://api.star-history.com/svg?repos=AkarshVyas/AI-Video-Assistant-&type=Date)](https://star-history.com/#AkarshVyas/AI-Video-Assistant-&Date)

---

## 📈 Project Stats

- **Languages**: Python
- **Lines of Code**: ~1,000+
- **Dependencies**: 15+ packages
- **Processing Speed**: ~2-3x real-time (depends on model)
- **Supported Languages**: English, Hinglish

---

**Built with ❤️ for better meeting intelligence**

*Making video meetings searchable, actionable, and insightful.*
