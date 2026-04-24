# 🔬 Face Lab — Real-Time Face Intelligence in the Browser

A browser-based face recognition and analysis lab built with **MediaPipe Tasks Vision**. No server, no backend, no installs — everything runs client-side on your device.

Built by **Vitaly Khristoliubov** for the Lynn University × Universitetit të Tiranës joint Cybersecurity Lab (Prof. Antoniou · April 2026).

---

## 🚀 Live Demo

Open `index.html` in any modern browser. No build step required.

```
facelab-github/
├── index.html        ← Landing page (open this)
├── style.css         ← Landing page styles
├── app.js            ← Landing page scripts
└── app/
    ├── index.html    ← Face Lab app (4-tab interface)
    ├── app.js        ← All app logic (MediaPipe, tracker, emotion, liveness)
    └── style.css     ← App styles
```

> **Tip:** To run locally with a proper server (avoids any browser CORS warnings):
> ```bash
> npx serve . -l 3000
> # Then open http://localhost:3000
> ```

---

## ✨ Features

### Detect Tab
- **MediaPipe BlazeFace** detector (short-range + full-range model toggle)
- Side-by-side benchmark: MediaPipe vs simulated Haar Cascade
- **CLAHE low-light enhancement** (histogram equalization toggle)
- Adjustable confidence threshold slider
- **IoU Multi-Person Tracker** — stable `#ID` per face, per-face color coding
- **Snap Export** — save current frame as PNG with timestamp watermark
- FPS / ms / face count stats overlay

### Identify Tab
- **Real-time face recognition** using 478-landmark descriptors (~300-dim, 3D normalized)
- Register faces by name (3–15 sample frames averaged)
- Match threshold slider (0.70–0.99)
- Upload photo for recognition
- **Emotion Detection** — 6 classes via blendshapes: 😊 Happy · 😮 Surprised · 😠 Angry · 😢 Sad · 😉 Wink · 😐 Neutral
- **IoU Multi-Person Tracker** — same stable IDs across frames
- **Liveness / Anti-Spoofing** — blink monitor (≥2 blinks in 6s = "✓ Live", else "⚠ Spoof?")
- **Snap Export** — save canvas frame as PNG
- Known faces list with avatars, sample count, delete

### PID Tracker Tab
- MediaPipe FaceDetector for face position tracking
- PID controller with Kp, Kd, Max Speed sliders
- RC drone telemetry log
- CSV export of flight data
- Real-time stats: YAW, FWD/BCK, Error px, Area

### Analysis Tab
- Chart.js 4-chart live grid: Yaw · Face X · Area · Error
- Demo data mode + CSV upload

---

## 🛠 Tech Stack

| Component | Library / API |
|---|---|
| Face Detection | MediaPipe Tasks Vision `@0.10.21` — BlazeFace |
| Face Landmarks | MediaPipe FaceLandmarker (478 points + blendshapes) |
| Multi-Person Tracking | Custom IoU tracker (pure JS) |
| Emotion Detection | Blendshape classifier (pure JS) |
| Charts | Chart.js 4.4.3 |
| Camera | MediaStream Web API |
| Export | Canvas 2D → Blob → PNG download |
| Storage | `localStorage` (face descriptors) |
| Fonts | Cabinet Grotesk + Satoshi (Fontshare CDN) |

No npm, no bundler, no framework. Pure HTML + CSS + Vanilla JS.

---

## 📁 File Reference

### `app/app.js` — Shared globals
| Symbol | Purpose |
|---|---|
| `FaceTracker` class | IoU-based tracker, `update(boxes)` → tracked faces with stable `id`, `color`, smoothed bbox |
| `snapCanvas(canvas, video, label)` | Export mirrored canvas frame as PNG with timestamp |
| `classifyEmotion(blendshapes)` | Map MediaPipe blendshapes → 6 emotion strings |
| `applyCLAHE(src, dstCtx, W, H)` | Histogram equalization for low-light detection |
| `initMediaPipe()` | Init FaceDetector respecting `window._fullRangeEnabled` flag |
| `makeFPSTracker()` | Rolling 30-frame FPS counter |

### `app/style.css` — Design system
- Dark navy `#0a0e1a` background, electric teal `#29b6d8` / `#0a7ea4` accents
- 4-tab responsive nav, mobile-first (safe area insets, notch support)
- Emotion badge, liveness chip, tracking label styles
- Dark / light mode toggle via `data-theme` attribute

---

## 🌐 Deploy to GitHub Pages

1. Push this folder to a GitHub repository
2. Go to **Settings → Pages**
3. Set source to `main` branch, root `/`
4. Your site will be live at `https://<username>.github.io/<repo>/`

The landing page (`index.html`) will load automatically, and the "Launch App" button opens `./app/`.

---

## 📋 Known Constraints

- **Camera required** for Detect, Identify, and PID tabs
- MediaPipe WASM files are loaded from jsDelivr CDN (`@0.10.21`) — internet connection required
- Face descriptors saved to `localStorage` — they persist across sessions in the same browser
- Photo upload recognition uses a separate IMAGE-mode FaceLandmarker instance

---

## 🎓 Academic Context

This project was created for the **Lynn University × Universitetit të Tiranës** joint cybersecurity research lab, exploring browser-native computer vision for identity verification, access control, and surveillance systems.

- **University:** Lynn University, Boca Raton, FL
- **Partner:** Universitetit të Tiranës, Faculty of Natural Sciences — Informatica
- **Course:** Cybersecurity Track
- **Supervisor:** Prof. Antoniou
- **Date:** April 2026

---

## 📄 License

MIT — free to use, modify, and distribute with attribution.
