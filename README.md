AI Voice Cloning for Multilingual Localized Customer Support

Overview
Fully open-source, real-time voice customer support prototype. It transcribes user speech, understands intent in multiple languages, and responds in a cloned, natural-sounding voice. The app runs locally or on Hugging Face Spaces with no paid APIs.

Highlights
- Multilingual STT: faster-whisper (CPU-friendly), with optional language override
- Multilingual NLP: sentence-transformers with language-restricted cosine matching and confidence threshold
- Voice Cloning TTS: Coqui TTS (XTTS v2 preferred), automatic fallback to YourTTS or Tacotron2
- Languages: English (en), Hindi (hi), Spanish (es), Tamil (ta), Arabic (ar) by default; easily extendable
- Streamlit UI: Mic recording or WAV upload; selectable STT model size for accuracy/latency trade-offs
- Privacy: No cloud calls; all processing local/in-Space

Architecture
- STT: faster-whisper (CT2 int8) → transcript + language
- NLP: SentenceTransformer (paraphrase-multilingual-MiniLM-L12-v2) → language-aware intent retrieval with min score threshold
- NLG: Localized response templates per intent per language
- TTS: Coqui TTS (XTTS v2) with `speaker_wav` cloning; fallback to YourTTS or Tacotron2 when needed

Repository Layout
- `app.py`: Streamlit app implementing STT → NLP → TTS pipeline, with robustness features
- `requirements.txt`: Pinned dependencies for compatibility
- `harvard.wav`: Sample reference voice (fallback if no upload)
- `README.md`: This documentation

Getting Started
Prerequisites
- Python 3.9–3.11
- Windows, macOS, or Linux
- Optional GPU (CUDA) for faster TTS/STT

Install
1) Create a virtual environment
   - Windows (PowerShell):
     python -m venv .venv
     .\.venv\Scripts\Activate.ps1
   - macOS/Linux:
     python -m venv .venv
     source .venv/bin/activate

2) Install dependencies
   pip install --upgrade pip
   pip install -r requirements.txt --no-cache-dir

3) Accept Coqui TTS Terms of Service (if prompted)
   The app sets this automatically, but if needed:
   - Windows (PowerShell):  $env:COQUI_TOS_AGREED = "1"
   - macOS/Linux:            export COQUI_TOS_AGREED=1

Run
streamlit run app.py

Using the App
1) Reference voice
   - Upload a clean 5–10s WAV of the desired speaker, or use `harvard.wav`.
2) Choose STT model size (sidebar)
   - tiny/base/small/medium. Larger = better accuracy, higher latency.
3) Record or upload audio
   - Use the mic button or upload a WAV query.
4) Optional: Force language
   - Set the language selector to enforce STT in a target language (e.g., en).
5) Review results
   - The UI shows detected language, transcript, classified intent, and generated reply.
6) Listen to synthesized response
   - TTS clones the reference speaker when possible. If XTTS fails on your setup, the app auto-falls back and warns in the UI.

Customization
Add new languages
1) Update `SUPPORTED_LANG_CODES` in `app.py` with ISO-639-1 code (e.g., "fr").
2) Extend `build_intent_bank()` with example utterances in the new language for each intent.
3) Add localized responses in `response_templates()`.

Modify intents/responses
- Add new intents by introducing keys in `build_intent_bank()` and `response_templates()`.
- Examples drive retrieval; add several varied phrasings per language.

Voice Cloning Tips
- Use a clean, noise-free 5–10s mono WAV.
- Keep recording conditions consistent for stable cloning.
- You can swap speakers anytime by uploading a new WAV.

Performance & Quality
- STT size: Try “medium” for better accuracy; “small” balances speed/quality.
- Language override: If detection seems wrong, force the intended language.
- GPU: If available, speeds up TTS/STT significantly.
- XTTS fallback: If XTTS errors (e.g., GPT2 generate), YourTTS or Tacotron2 is used automatically.

Troubleshooting
- Coqui ToS error
  - Set `COQUI_TOS_AGREED=1` env var as shown above.
- XTTS load errors / GPT2 generate AttributeError
  - This repo pins: `TTS==0.21.3`, `transformers==4.41.2`.
  - Clear model cache and retry:
    - Windows: delete `%APPDATA%/tts/tts_models/multilingual/multi-dataset/xtts_v2`
    - macOS/Linux: delete `~/.local/share/tts/tts_models/multilingual/multi-dataset/xtts_v2`
  - The app falls back to YourTTS then Tacotron2 automatically.
- Wrong language detected for English
  - Use the “Force language” selector and try a larger STT model.
- Clumsy audio
  - Ensure clean reference WAV, try YourTTS fallback (auto), and consider GPU.

Hugging Face Spaces (Streamlit)
1) Create a new Space
   - Type: Streamlit
   - Hardware: CPU works; GPU recommended
2) Add files
   - `app.py`, `requirements.txt`, `harvard.wav`, `README.md`
3) (Optional) Set Space secret or variable
   - `COQUI_TOS_AGREED=1`
4) Deploy
   - Push to the Space repo; it will auto-build and launch.

Model Fine-Tuning (Optional)
- STT: Fine-tune Whisper via community tools if needed.
- NLP: Train a small multilingual classifier or fine-tune embeddings with labeled data.
- TTS: Coqui TTS supports XTTS/YourTTS training; see upstream docs for datasets and training.

License
Open-source components; respect licenses for models and datasets.


