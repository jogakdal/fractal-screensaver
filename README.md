# FractalSaver — Real-Time Mandelbrot Zoom Screen Saver for Windows

> Turn your idle screen into an infinite journey through the Mandelbrot set.

---

## Gallery

| | |
|:---:|:---:|
| ![Screenshot 1](images/screenshot_01.png) | ![Screenshot 2](images/screenshot_02.png) |
| ![Screenshot 3](images/screenshot_03.png) | ![Screenshot 4](images/screenshot_04.png) |
| ![Screenshot 5](images/screenshot_05.png) | ![Screenshot 6](images/screenshot_06.png) |

![Magenta Theme Animation](images/magenta_animation.gif)

---

## What is FractalSaver?

FractalSaver is a free Windows screen saver that renders the Mandelbrot set in real time, continuously zooming into 100 hand-picked locations across 15 categories — from Seahorse Valleys and Spiral Galaxies to Misiurewicz Points and Mini Mandelbrot Copies.

Every run is unique. The visit order is shuffled, zoom targets are randomly offset, and colors shift continuously, so you'll never see the same animation twice.

---

## Key Features

### Dual Rendering Engine
- **GPU mode**: D3D11 Compute Shader with double-float emulation (~48-bit precision)
- **CPU fallback**: AVX2 SIMD (4-lane double), multi-threaded — works on any PC without a dedicated GPU

### Stunning Visuals
- **6 color themes**: Red, Green, Blue, Cyan, Magenta, Gold — shuffled randomly each run
- **4 coloring styles**: Smooth Gradient, Contour Bands, Stripe Average, Triangle Inequality Average (TIA)
- **24 visual combinations**: 6 themes x 4 styles, all shuffled for maximum variety
- **Auto color cycling**: colors rotate every frame for endlessly shifting palettes
- **2x2 supersampled anti-aliasing**: silky smooth edges at every zoom level
- **Up to 16,384x zoom**: deep dives into fractal structure with double-float precision

### Smart Zoom Animation
- **100 interesting locations** in 15 categories — never boring
- **Boundary tracking**: the camera automatically follows the most visually complex edges
- **Adaptive redirect**: mid-zoom direction changes seek out nearby intricate patterns
- **Curvature filtering**: flat, featureless boundaries are automatically skipped

### Overlay & Clock
- **System info overlay**: GPU/CPU model, RAM, resolution, FPS, zoom level, iteration count
- **7-segment digital clock**: bouncing LED-style display with premultiplied alpha blending

### Lightweight & Self-Contained
- **Single file** (~446 KB .exe) — no runtime dependencies, no .NET, no Java
- **Static CRT** (/MT) — runs on a clean Windows install
- **Built-in installer** — auto UAC elevation, no separate installer needed
- **Zero external dependencies** — everything is built from scratch in C++20

---

## System Requirements

| Component | Minimum |
|-----------|---------|
| OS | Windows 10 / 11 (64-bit) |
| CPU | Intel Haswell (2013+) or AMD Excavator (2015+) with AVX2 |
| GPU | Any DirectX 11 capable GPU (optional — CPU fallback available) |
| RAM | 64 MB free |
| Disk | < 1 MB |

---

## Installation

1. Download `FractalSaver.exe` from the [Releases](https://github.com/jogakdal/fractal-screensaver/releases) page
2. Double-click to run — UAC will auto-elevate and install the screen saver
3. The Screen Saver Settings dialog opens automatically

To preview without installing: `FractalSaver.exe /s`

To uninstall: `FractalSaver.exe /u` or use Windows standard uninstaller

---

## Settings

| Setting | Options | Default |
|---------|---------|---------|
| Rendering | GPU (auto) / Force CPU | GPU auto |
| Zoom Speed | 1 (slow) ~ 10 (fast) | 3 |
| Color Style | Auto Cycle / Smooth / Contour / Stripe / TIA | Auto Cycle |
| Info Overlay | On / Off | On |
| Digital Clock | On / Off | Off |

---

## Technical Highlights

- **C++20** with MSVC Build Tools 2022
- **D3D11 Compute Shader** (cs_5_0) with `precise` keyword and IEEE strictness for numerical accuracy
- **AVX2 intrinsics** (`__m256d`) — 4 pixels computed simultaneously
- **Double-float emulation** on GPU: Dekker splitting for ~48-bit mantissa precision
- **Cardioid/Period-2 bulb early rejection** — SIMD-accelerated, skips most interior points without iteration
- **One-pass row rendering** with stack-allocated buffers (~40 KB in L1 cache vs. 132 MB heap)
- **Exponential zoom** with adaptive panning and boundary-aware target tracking
- **Binary search boundary detection** (30~50 iterations, precision ~10^-9 to ~10^-15)
- **PCA-based curvature analysis** to filter out visually uninteresting boundaries
- **Whole Program Optimization** (/GL + /LTCG) and Link-Time Code Generation

---

## The Math Behind the Beauty

FractalSaver computes the classic Mandelbrot iteration **z = z² + c** for every pixel, with several enhancements:

- **Smooth iteration count**: `n + 1 - log₂(log₂(|z|))` eliminates color banding
- **Cosine palette**: Inigo Quilez-style color generation with per-channel phase offsets
- **Orbit traps** (Stripe/TIA): accumulate angular and radial orbit statistics for rich texturing
- **Interior coloring**: average orbit magnitude creates pseudo-iteration values for non-escaping points
- **Dynamic max iterations**: `100 + 50 × log₂(zoom)`, capped at 5000

---

## Download

- **GitHub**: https://github.com/jogakdal/fractal-screensaver/releases
- **WinCustomize**: https://www.wincustomize.com/explore/screensavers/1693/

---

## Support the Developer

If you enjoy FractalSaver, consider supporting its development:

[![Buy Me a Coffee](https://img.shields.io/badge/Buy%20Me%20a%20Coffee-ffdd00?style=for-the-badge&logo=buy-me-a-coffee&logoColor=black)](https://buymeacoffee.com/jogakdal)
[![GitHub Sponsors](https://img.shields.io/badge/GitHub%20Sponsors-ea4aaa?style=for-the-badge&logo=github-sponsors&logoColor=white)](https://github.com/sponsors/jogakdal)

---

## Author

**Yongho Hwang** ([@jogakdal](https://github.com/jogakdal))

- **Blog (Velog)**: https://velog.io/@jogakdal
- **Blog (Naver)**: https://blog.naver.com/jogakdal

---

## License

All rights reserved. This software is provided as-is for personal use.

---

*FractalSaver — Where mathematics meets art.*
