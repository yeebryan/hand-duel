# Hand Duel

A real-time Rock Paper Scissors game controlled entirely by hand gestures — no buttons, no taps.

![HANDDUEL](https://img.shields.io/badge/HAND-DUEL-3effd8?style=for-the-badge&labelColor=080810)

## How It Works

### 1. Hand Tracking — MediaPipe
The game uses [MediaPipe Tasks Vision](https://ai.google.dev/edge/mediapipe/solutions/vision/hand_landmarker) (`HandLandmarker`) running in `VIDEO` mode with GPU acceleration. It streams 21 3D landmarks per hand from your device camera in real time — no server, no model download required at runtime (loaded from CDN).

### 2. Gesture Classification
A lightweight classifier reads four finger states (index, middle, ring, pinky) by comparing each fingertip's Y position to its PIP knuckle:

| Fingers up | Gesture   |
|-----------|-----------|
| 0         | ✊ Rock    |
| Index + Middle | ✌️ Scissors |
| 3 or more | 🖐️ Paper  |

### 3. Hold-to-Trigger UX
Rather than a button press, the game starts when you **hold a stable gesture for 1.5 seconds**. A radial SVG ring and a fill bar animate the charge — keeps the UI hands-free and phone-friendly.

### 4. Round Flow
1. Hold any gesture → 1.5 s charge fills
2. `3 → 2 → 1 → SHOOT!` countdown (750 ms per tick)
3. Player's gesture is captured at **SHOOT**; CPU picks randomly
4. CPU panel flashes with the result (win / lose / draw)
5. Result overlay shown — hold again to replay

### 5. Rendering
The hand skeleton is drawn on a `<canvas>` overlaid on the `<video>` feed using MediaPipe's `DrawingUtils`. The canvas and video are CSS `scaleX(-1)` mirrored so the view feels natural (selfie-style).

## Tech Stack

| Layer | Tool |
|---|---|
| Hand detection | MediaPipe Tasks Vision 0.10.14 |
| Language | Vanilla JS (ES modules) |
| Styling | Pure CSS custom properties + CSS animations |
| Fonts | Bebas Neue · JetBrains Mono (Google Fonts) |
| PWA | `manifest.json` — installable, fullscreen |
| Build | None — single `index.html` |

## Run Locally

Serve the folder over HTTPS (camera requires a secure context):

```bash
npx serve .
# or
python3 -m http.server 8080
```

Then open `http://localhost:8080` in Chrome or Safari.

> **Note:** On iOS, add to Home Screen for the best fullscreen experience.
