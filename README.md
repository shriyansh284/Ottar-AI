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
