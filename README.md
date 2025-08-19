# AI Voice Cloning for Multilingual Customer Support 🎙️🌍

**A production-ready, 100% open‑source solution for multilingual, localized, voice-based customer support**

> Speak naturally in any supported language — the system transcribes, understands, and replies in a cloned voice, all without paid APIs.

🚀 **[Live Demo on Hugging Face Spaces](https://huggingface.co/spaces/vinayabc1824/AI-Voice-Cloning-for-Customer-Support)**

---

## ✨ Features

* **Fully open-source** — All components run locally or on Hugging Face Spaces.
* **Multilingual Speech-to-Text (STT)** — faster-whisper with optional language override.
* **Language-aware intent detection** — sentence-transformers with confidence thresholding.
* **Voice cloning Text-to-Speech (TTS)** — Coqui TTS (XTTS v2), fallback to YourTTS and Tacotron2.
* **Built-in language support** — English, Hindi, Spanish, Tamil, Arabic.
* **Modern Streamlit UI** — Mic recording, WAV upload, and real-time playback.

---

## 🛠 System Architecture

1. **STT:** faster-whisper transcribes audio + detects language.
2. **NLP:** sentence-transformers retrieves intent via cosine similarity.
3. **NLG:** Localized template-based responses per language.
4. **TTS:** Coqui XTTS v2 clones voice; fallbacks ensure reliability.

### 📊 Architecture Diagram

```
┌───────────────────────────┐    ┌──────────────────────┐    ┌────────────────────────┐
│   User Audio Input        │───▶│ Speech-to-Text (STT) │──▶│ Language Detection     │
│ (Mic or File Upload)      │    │   Faster-Whisper     │    │                        │
└───────────────────────────┘    └──────────────────────┘    └────────────────────────┘
             │                           │                             │
             ▼                           ▼                             ▼
┌───────────────────────────┐    ┌──────────────────────┐    ┌────────────────────────┐
│ NLP Intent Recognition    │    │ Localized Response   │    │ Text-to-Speech (TTS)   │
│ (Transformers / spaCy)    │    │ Generation           │    │   XTTS v2 / Fallbacks  │
└───────────────────────────┘    └──────────────────────┘    └────────────────────────┘
                                                                │
                                                                ▼
                                                     ┌────────────────────────┐
                                                     │   Audio Playback       │
                                                     │   (User's Voice Clone) │
                                                     └────────────────────────┘


```

---

## 📦 Requirements

* **OS:** Windows / macOS / Linux
* **Python:** 3.9–3.11
* **Hardware:** CPU-friendly, GPU recommended for speed

---

## ⚡ Installation

```bash
# Create and activate virtual environment
python -m venv .venv
source .venv/bin/activate  # Linux/macOS
.\.venv\Scripts\Activate.ps1  # Windows

# Install dependencies
pip install --upgrade pip
pip install -r requirements.txt --no-cache-dir

# Accept Coqui TTS Terms (if prompted)
export COQUI_TOS_AGREED=1  # Linux/macOS
$env:COQUI_TOS_AGREED="1"  # Windows
```

---

## 🚀 Quick Start

```bash
streamlit run app.py
```

Open `http://localhost:8501` or [try it live here](https://huggingface.co/spaces/vinayabc1824/AI-Voice-Cloning-for-Customer-Support).

---

## 🎯 Usage

1. Upload a **clean 5–10s mono WAV** as reference voice.
2. Choose **STT model size** (`tiny`/`base`/`small`/`medium`).
3. Record or upload query audio.
4. (Optional) Force language detection.
5. Review transcript, intent, and localized response.
6. Listen to the cloned-voice reply.

---

## 🔧 Configuration & Customization

* Add languages in `SUPPORTED_LANG_CODES`.
* Modify intent examples in `build_intent_bank()`.
* Update responses in `response_templates()`.
* Adjust STT device/precision in `load_stt_model()`.

---

## 💡 Performance Tips

* Use **GPU** for faster, higher-quality TTS.
* Keep reference voice **noise-free**.
* Use **medium** STT for better accuracy.

---

## 🐛 Troubleshooting

* **Coqui TOS error:** set `COQUI_TOS_AGREED=1`.
* **XTTS load errors:** use `TTS==0.21.3` & `transformers==4.41.2`; clear model cache.
* **Language mis-detection:** force language in UI.

---

## 📤 Deploy on Hugging Face Spaces

1. Create Space → Type: Streamlit.
2. Add `app.py`, `requirements.txt`, `harvard.wav`, `README.md`.
3. Add `COQUI_TOS_AGREED=1` in secrets.
4. Push to repo → auto-build.

**Live demo:** [https://huggingface.co/spaces/vinayabc1824/AI-Voice-Cloning-for-Customer-Support](https://huggingface.co/spaces/vinayabc1824/AI-Voice-Cloning-for-Customer-Support)

---

## 🔒 Security & Privacy

* All processing is local or within your Space.
* Avoid uploading sensitive data to public repos.

---

## 🗺 Roadmap

* LLM-powered dynamic replies
* Telephony integration
* CRM connectors
* Real-time streaming under 2s

---

## 📜 License

Open-source only — respect individual model licenses.

---

## 🙌 Acknowledgments

* **STT:** faster-whisper
* **NLP:** sentence-transformers
* **TTS:** Coqui TTS (XTTS v2, YourTTS, Tacotron2)

---

**Built with ❤️ for automated customer support**

