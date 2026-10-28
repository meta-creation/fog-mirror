# 🌫️ Breath on Glass — Fog Mirror

> **A virtual bathroom mirror for your webcam.** Fog it up with your breath, then write in it with a fingertip — all in the browser, nothing uploaded, no server needed.

---

## ✨ Demo

![Fog Mirror Preview](https://raw.githubusercontent.com/your-username/fog-mirror/main/preview.gif)

---

## 🎯 What It Does

**Breath on Glass** turns your webcam into a steamy bathroom mirror:

| Gesture | Effect |
|---|---|
| 🫁 **Breathe** softly at your laptop | Fog blooms right where your breath lands |
| 🤌 **Pinch** (thumb + index finger) | Wipe away fog and draw — your face shows through |
| ✊ **Fist** + swipe | Clear a wide patch, like wiping with your sleeve |

It uses your **microphone** to detect breath and **MediaPipe** to track your hand in real time — everything runs locally in the tab.

---

## 🚀 Quick Start

Because the browser blocks the camera on `file://` URLs, you need to serve it over `localhost`:

**Python (built-in):**
```bash
cd fog-mirror
python3 -m http.server 8000
```
Then open → [http://localhost:8000](http://localhost:8000)

**Node (if you prefer):**
```bash
npx serve .
```

---

## ⌨️ Keyboard Shortcuts

| Key | Action |
|---|---|
| `Space` (hold) | Fog the mirror |
| `C` | Clear all drawing |
| `G` | Toggle glow on/off |
| `S` | Save a photo (PNG) |
| `D` | Toggle debug readouts |
| `Esc` | Return to start screen |

There's also a **brush size slider** and **glow duration slider** in the bottom bar.

---

## 🔒 Privacy

**Nothing ever leaves your machine.**

- Camera frames are processed **entirely in the browser tab**
- Microphone audio is read as raw amplitude only — no recording
- No analytics, no cookies, no network requests (beyond loading MediaPipe from a CDN once)

---

## 🛠️ How It Works

```
Webcam feed
    │
    ├── (low-res copy) ──► blur + brighten + tint ──► "frost" layer
    │                                                       │
    ├── Fog canvas (alpha mask) ◄── breath blobs            │
    │        │                  ◄── hand wipe/draw          │
    │        └─────────────────────── masks ───────────────►│
    │                                                       ▼
    │                                              Composited mirror
    └── MediaPipe Hands + Face ──► hand tracking + fog centering
         └── Microphone analyser ──► breath strength
```

| Layer | Role |
|---|---|
| **fog canvas** | Alpha-only mask — white = fogged, transparent = clear |
| **frost layer** | Blurred + brightened + tinted camera, revealed only where the fog mask is white |
| **glow canvas** | Half-res canvas for glowing strokes; composited with `lighter` blend mode |
| **smallC** | Tiny (25 % of screen) canvas — makes the blur cheap |

---

## 📁 Project Structure

```
fog-mirror/
└── index.html      # Everything — HTML, CSS, and JS in one self-contained file
```

The entire project is a **single HTML file** (~830 lines). No build step, no dependencies to install.

---

## 🧰 Tech Stack

| Technology | Purpose |
|---|---|
| **Canvas 2D API** | Compositing, fog mask, glow layer |
| **MediaPipe Hands** | Real-time 21-point hand landmark detection |
| **MediaPipe Face Detection** | Mouth-position tracking to centre fog bloom |
| **Web Audio API** | Microphone amplitude for breath detection |
| **CSS animations** | Start-screen steam, animated mirror illustration |

MediaPipe models are loaded from jsDelivr CDN (pinned versions to avoid broken builds).

---

## 🎨 Customisation

Open `index.html` and edit the `CFG` object near the top of the `<script>`:

```js
const CFG = {
  fogStart:  0.55,   // initial fog opacity (0 = clear, 1 = totally white)
  blur:      18,     // frost blur radius
  bright:    1.18,   // frost brightness multiplier
  tint:      '242,238,231',  // RGB of the frost tint colour
  blobs:     12,     // breath blobs per frame
  glowHue:   null,   // null = rainbow per stroke, 0-360 = fixed hue
  // ... and many more
};
```

---

## 🌐 Deployment

This is a static site — drop `index.html` anywhere:

- **GitHub Pages** — push to a repo and enable Pages in Settings
- **Netlify / Vercel** — drag-and-drop the folder
- **Any static host** — just needs HTTPS (required for camera access)

> ⚠️ The camera API requires **HTTPS** or **localhost**. Plain `http://` on a remote host will not work.

---

## 📄 License

MIT — do whatever you like, just don't claim you made MediaPipe.

---

<p align="center">Made with 🫧 and a steamy bathroom vibe</p>
