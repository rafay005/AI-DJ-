# 🎧 AI Mood DJ

An AI-powered music recommender that reads your mood from your **face** and your **words**, then plays a matching playlist on **Spotify**.

It combines:
- **CNN facial emotion detection** (live webcam)
- **NLP sentiment analysis** (text you type about how you feel)
- **Spotify API** integration to pick and play tracks that fit the detected mood

---

## ✨ Features

- 📷 Real-time facial emotion recognition from webcam frames
- 💬 Text sentiment analysis ("rough day at work", "feeling hyped!")
- 🧠 Mood fusion: combines face + text signals into one final mood
- 🎵 Mood → audio features mapping (valence, energy, tempo, danceability)
- 🔗 Spotify integration: fetches recommendations / builds a playlist and starts playback
- 🖥️ Simple web UI to run everything in one place

---

## 🏗️ How It Works

```
 Webcam frame ──► Face detection (OpenCV) ──► CNN emotion classifier ──┐
                                                                       ├──► Mood fusion ──► Spotify query ──► Playlist / playback
 User text ─────► Sentiment model (NLP) ───────────────────────────────┘
```

1. **Face pipeline** – OpenCV detects the face, crops it to 48×48 grayscale, and the CNN predicts one of 7 emotions: `angry, disgust, fear, happy, neutral, sad, surprise`.
2. **Text pipeline** – The sentiment model scores the user's text as positive / neutral / negative with a confidence score.
3. **Fusion** – Both outputs are weighted and combined into a final mood (e.g. `happy`, `calm`, `sad`, `energetic`, `angry`).
4. **Music** – The mood is mapped to Spotify audio-feature targets and used to fetch matching tracks.

### Mood → music mapping (default)

| Mood      | Valence | Energy | Example vibe          |
|-----------|---------|--------|-----------------------|
| Happy     | High    | High   | Upbeat pop            |
| Sad       | Low     | Low    | Acoustic / soft       |
| Angry     | Low     | High   | Rock / high tempo     |
| Calm      | Mid     | Low    | Lo-fi / chill         |
| Surprised | High    | Mid    | Indie / dance         |

<!-- TODO: replace with the exact mapping used in your code -->

---

## 🧰 Tech Stack

<!-- TODO: confirm — stack below is the typical setup, adjust to match your build -->
- **Python 3.10+**
- **TensorFlow / Keras** – CNN emotion model (trained on FER-2013)
- **OpenCV** – face detection and frame capture
- **NLP** – VADER / Hugging Face transformers for sentiment
- **Spotipy** – Spotify Web API client
- **Flask / Streamlit** – web interface

---

## 📁 Project Structure

```
ai-mood-dj/
├── app.py                  # main app / UI entry point
├── emotion/
│   ├── model.py            # CNN architecture + loading
│   ├── detect.py           # webcam capture + face detection + prediction
│   └── emotion_model.h5    # trained weights
├── sentiment/
│   └── analyze.py          # text sentiment analysis
├── music/
│   ├── spotify_client.py   # Spotify auth + queries
│   └── mood_mapper.py      # mood → audio features
├── fusion.py               # combines face + text mood
├── requirements.txt
└── .env.example
```

<!-- TODO: update to the real folder layout -->

---

## 🚀 Getting Started

### 1. Clone and install

```bash
git clone https://github.com/<your-username>/ai-mood-dj.git
cd ai-mood-dj
python -m venv venv
# Windows
venv\Scripts\activate
# macOS / Linux
source venv/bin/activate
pip install -r requirements.txt
```

### 2. Set up Spotify credentials

1. Go to the [Spotify Developer Dashboard](https://developer.spotify.com/dashboard) and create an app.
2. Add `http://127.0.0.1:8888/callback` as a Redirect URI.
3. Copy `.env.example` to `.env` and fill in:

```env
SPOTIPY_CLIENT_ID=your_client_id
SPOTIPY_CLIENT_SECRET=your_client_secret
SPOTIPY_REDIRECT_URI=http://127.0.0.1:8888/callback
```

> Playback control needs a **Spotify Premium** account and an active Spotify device (desktop/mobile app open).

### 3. Run

```bash
python app.py
```

Allow webcam access, type how you're feeling (optional), and hit **Detect Mood** — the DJ takes it from there.

---

## 🧠 Model Details

**Emotion CNN**
- Dataset: FER-2013 (48×48 grayscale, 7 classes)
- Architecture: stacked Conv2D → BatchNorm → MaxPool → Dropout blocks, then Dense + Softmax
- Accuracy: `<!-- TODO: add your val accuracy -->`

**Retrain (optional)**

```bash
python emotion/train.py --data path/to/fer2013 --epochs 50
```

---

## 🔮 Future Improvements

- Voice tone emotion detection
- Learning user taste over time (personalized mood mapping)
- Smooth mood transitions between tracks
- Mobile app version

---

## 👤 Author

**Abdul Rafay** — ML / Computer Vision / AI Engineer
<!-- TODO: add GitHub / LinkedIn / portfolio links -->

## 📄 License

MIT
