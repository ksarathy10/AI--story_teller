# AI Story Creator

A full-stack AI application that turns images, text, or voice into short, captivating stories with optional audio narration, supporting multiple Indian languages.

## 🎯 Features

- Image → Story (BLIP + Gemini pipeline)
- Text → Story (direct prompt)
- Voice → Story (speech-to-text placeholder → story)
- Multi-language output: English + Indian languages (Hindi, Tamil, Telugu, Malayalam, Bengali, etc.)
- Audio narration via TTS (MP3)
- Modern responsive UI with progress feedback

## 🛠️ Tech Stack

### Frontend
- **React 18** with TypeScript
- **TailwindCSS** for styling
- **Vite** for fast development
- **Lucide React** for icons
- **React Hot Toast** for notifications

### Backend
- FastAPI (Python)
- BLIP (Salesforce/blip-image-captioning-large) for image understanding
- Google Gemini 1.5 Flash for story generation and enhancement
- Translator service (googletrans) for optional translation
- Google TTS (gTTS) for audio narration
- PyTorch for model execution

## 🚀 Quick Start

### Prerequisites
- Python 3.10+ 
- Node.js 18+
- 16GB RAM recommended for first-time model downloads

### Backend Setup

Option A (Windows, simple):

1) Double-click `start-backend.bat`

Option B (manual):

1) Open a terminal in `backend/`
2) Install deps: `pip install -r requirements.txt`
3) Run: `python -m uvicorn main:app --reload --host 0.0.0.0 --port 8000`

Backend runs at http://localhost:8000

### Configuration (.env)

Create a `backend/.env` file by copying the example and adding your key:

1) Copy the template: `backend/.env.example` → `backend/.env`
2) Set your Google Gemini API key:

```
GOOGLE_API_KEY=your_google_api_key_here
```

Note: `.env` is git-ignored and never pushed.

### Frontend Setup

Option A (Windows, simple):

1) Double-click `start-frontend.bat`

Option B (manual):

1) Open a terminal in `frontend/`
2) `npm install`
3) `npm run dev`

Frontend runs at http://localhost:5173

## 🎮 How to Use

1. **Upload an Image**: Drag and drop or click to select any JPG/PNG image
2. **Select Language**: Choose from English or any Indian regional language
3. **Generate Story**: Click the "Generate Story & Audio" button
4. **Enjoy**: Read the generated story and listen to the audio narration

## 🧠 AI Pipeline

1) Image → BLIP caption/summary (base context)
2) Gemini 1.5 Flash → enriched story (target language + age tone)
3) Optional translator → fallback/extra languages
4) TTS (gTTS) → MP3 narration

See `ARCHITECTURE.md` and `FEATURES_AND_TECH.md` for full details.

## ⚡ Performance Notes

- Local model caching (first run downloads are slower)
- Async FastAPI endpoints for responsiveness
- Graceful fallbacks and clear error messages in UI

## 🌐 API Endpoints

- POST `/caption` - Caption an image (BLIP)
- POST `/story` - Story from text caption
- POST `/story-from-image` - Story directly from image (BLIP → Gemini)
- POST `/translate` - Translate text
- POST `/tts` - Generate MP3
- GET `/health` - Basic health + models loaded

## 🎨 UI Features

- Glassmorphism-inspired cards, animations, and progress feedback
- Image/Text/Voice tabs with validation
- Story display and audio player with download

## 🔧 Troubleshooting

### Common Issues

1. **Models not loading**: Ensure you have enough RAM (16GB recommended)
2. **Slow generation**: First run downloads models, subsequent runs are faster
3. **Audio not playing**: Check if the backend is running on port 8000

### Notes

- Speech-to-text in the UI is a placeholder; replace with your STT provider if needed.

## 📱 Mobile Support

Responsive layout. Image processing and file selection are easier on desktop.

## 🤝 Contributing

Feel free to submit issues, fork the repository, and create pull requests for any improvements.

## 📄 License

This project is open source and available under the MIT License.