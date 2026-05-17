# ✋ ASL Word Translator — Sign to Voice

A real-time American Sign Language (ASL) to spoken English translator that runs entirely in your browser. No server, no backend, no installation required.

![ASL Translator Demo](https://img.shields.io/badge/status-live-brightgreen) ![License](https://img.shields.io/badge/license-MIT-blue) ![HTML](https://img.shields.io/badge/built%20with-HTML%2FCSS%2FJS-orange)

---

## 🚀 Live Demo

> Just open `index.html` in Chrome or Edge — that's it.

Or host it free on **GitHub Pages**:
1. Push this repo to GitHub
2. Go to **Settings → Pages → Source → main branch**
3. Your app is live at `https://yourusername.github.io/asl-translator`

---

## ✨ Features

- **Real-time hand tracking** using [MediaPipe Hands](https://mediapipe.dev/) (21 landmarks per hand)
- **Word-level ASL recognition** — detects full words/phrases, not just letters
- **Hold-to-confirm** — hold a sign for 1.2 seconds to add it (prevents false positives)
- **Text-to-speech** via the Web Speech API — reads your full sentence aloud
- **Auto-speak mode** — automatically speaks after a 2-second pause
- **25 supported signs** out of the box
- **Quick-add palette** — tap any word to add it manually
- **Spoken history** with replay
- Runs at ~10fps inference on low-end hardware (throttled to prevent lag)

---

## 🤟 Supported Signs (25 words)

| Sign | ASL Handshape |
|------|--------------|
| HELLO | Open hand raised, palm out |
| THANK YOU | Flat hand moves from chin forward |
| YES | S-fist nods |
| NO | Index + middle snap to thumb |
| PLEASE | Flat hand circles on chest |
| SORRY | A-fist circles on chest |
| I LOVE YOU | ILY handshape (pinky + index + thumb) |
| HELP | Thumb-up lifted on flat palm |
| STOP | Flat hand chops sideways |
| MORE | Fingertips tap together (flat O) |
| WATER | W-hand (3 fingers) taps chin |
| EAT / FOOD | Flat O taps toward mouth |
| GOOD | All fingers out, mid-level |
| BAD | Fingers up, palm faces down |
| WANT | Claw hands pull toward body |
| WHERE | Index finger points/wags |
| WHO | L-hand near chin |
| MY / MINE | Flat hand on chest |
| YOU | Index points forward |
| WAIT | Spread open hand (5-hand) |
| COME | Index finger beckons |
| NAME | H-hand horizontal |
| AGAIN | Bent hand arcs into palm |
| UNDERSTAND | Index flicks up from forehead |
| KNOW | Flat hand taps temple |

---

## 🛠️ Tech Stack

| Technology | Purpose |
|-----------|---------|
| [MediaPipe Hands](https://mediapipe.dev/) | Real-time hand landmark detection (21 points) |
| Web Speech API | Text-to-speech (built into Chrome/Edge) |
| Vanilla JS | Sign classification logic |
| HTML5 Canvas | Live video + landmark rendering |

Zero dependencies to install. Everything loads from CDN.

---

## 📁 File Structure

```
asl-translator/
├── index.html      ← The entire app (HTML + CSS + JS in one file)
└── README.md       ← This file
```

---

## 🖥️ How to Run Locally

```bash
# Clone the repo
git clone https://github.com/yourusername/asl-translator.git
cd asl-translator

# Option 1: Just open the file
open index.html   # macOS
start index.html  # Windows

# Option 2: Serve it (recommended — avoids some browser restrictions)
npx serve .
# or
python -m http.server 8000
```

Then open `http://localhost:8000` in Chrome or Edge.

> ⚠️ **Safari is not supported** — it lacks full MediaPipe WebAssembly support. Use Chrome or Edge.

---

## ⚙️ How It Works

1. **MediaPipe Hands** tracks 21 3D landmarks on your hand at ~10fps
2. Each frame, a **geometric classifier** checks the spatial relationships between landmarks (finger extension, curl, spread, position) against known ASL handshapes
3. When a sign is held steady for **1.2 seconds**, the word is added to the sentence
4. After a **2-second pause**, the sentence can be spoken aloud via **Web Speech API**

### Performance optimizations
- `modelComplexity: 0` (lite model) — significantly faster than the full model
- Frame throttling: only 1 in 3 video frames is sent to MediaPipe
- `busy` flag prevents frame queuing on slow hardware
- Camera resolution locked to 320×240

---

## 🔧 Customization

### Add new signs
In `index.html`, find the `SIGNS` array and add an entry:

```javascript
{
  word: 'YOUR WORD',
  desc: 'Description of the handshape',
  fn(lm) {
    const { f, d2, lm: L } = makeHelpers(lm);
    // f.index, f.middle, f.ring, f.pinky, f.thumb = true if finger is extended
    // d2(a, b) = distance between landmark a and b
    // L[n].x, L[n].y = normalized position (0-1)
    return someCondition ? 0.80 : null; // return confidence or null
  }
}
```

MediaPipe landmark indices:
```
0 = wrist
1-4 = thumb (1=CMC, 2=MCP, 3=IP, 4=tip)
5-8 = index finger
9-12 = middle finger
13-16 = ring finger
17-20 = pinky
```

### Change hold duration
```javascript
const HOLD_MS = 1200;  // milliseconds to hold a sign
```

### Change auto-speak pause
```javascript
const SIL_MS = 2000;  // milliseconds of silence before auto-speak triggers
```

---

## ⚡ Limitations & Roadmap

**Current limitations:**
- Geometric heuristics can confuse similar handshapes (e.g. GOOD vs WAIT)
- Single hand only
- No motion-based signs (signs that require movement, like WANT or AGAIN, use position approximations)

**Possible improvements:**
- [ ] Train a TensorFlow.js classifier on the [ASL dataset](https://www.kaggle.com/datasets/grassknoted/asl-alphabet) for higher accuracy
- [ ] Add two-hand sign support
- [ ] Add motion detection (track landmark change over time)
- [ ] Claude API integration for grammar correction of the sentence
- [ ] ElevenLabs integration for more natural voice output

---



## 🙌 Credits

Aditi Sharma
