# FractalSaver — Real-Time Mandelbrot Zoom Screen Saver for Windows

🌐 [한국어](README.ko.md)

> Turn your idle screen into an infinite journey through the Mandelbrot set.

---

## Gallery

| | |
|:---:|:---:|
| ![Screenshot 1](images/FractalSaver_screenshot_Classic_2600.png) | ![Screenshot 2](images/FractalSaver_screenshot_Classic_3300.png) |
| ![Screenshot 3](images/FractalSaver_screenshot_Classic_4800.png) | ![Screenshot 4](images/FractalSaver_screenshot_Classic_5700.png) |
| ![Screenshot 5](images/FractalSaver_screenshot_Classic_6500.png) | ![Screenshot 6](images/FractalSaver_screenshot_Classic_7700.png) |

---

## What is FractalSaver?

FractalSaver is a free Windows screen saver that renders the Mandelbrot set in real time, continuously zooming into 39 curated locations — from Seahorse Valleys and Spiral Galaxies to Mini Mandelbrot Copies and Deep Needle structures.

Every run is unique. The visit order is shuffled, color themes and styles are randomly combined, so you'll never see the same animation twice.

---

## Key Features

### CPU Rendering Engine
- **AVX2 SIMD** (4-lane double), multi-threaded with 4-row batch distribution
- **Brent cycle detection**: early exit for interior pixels in mini-bulbs
- **LUT-optimized coloring**: cos/log/exp lookup tables for fast per-pixel color computation
- **Render-time-based deceleration**: zoom automatically slows when frames take longer, maintaining smooth interpolation

### Stunning Visuals
- **6 color themes**: Red, Green, Blue, Cyan, Magenta, Gold — shuffled randomly each run
- **5 coloring styles**: Smooth Gradient, Contour Bands, Stripe Average, Triangle Inequality Average (TIA), Classic
- **30 visual combinations**: 6 themes x 5 styles, all shuffled for maximum variety
- **Auto color cycling**: colors rotate every frame for endlessly shifting palettes
- **2x2 supersampled anti-aliasing**: silky smooth edges at every zoom level

### Smart Zoom Animation
- **39 verified routes** — each tested for render performance and visual quality
- **Waypoint paths**: zoom target coordinates change automatically at specific zoom depths, following curated routes through the fractal
- **Smooth transitions**: longer fade in/out (1.1s) between routes for elegant visual flow

### Multi-Monitor Support
- **Automatic primary selection**: renders on the largest monitor, mirrors to all others with aspect-ratio-preserving center crop
- **Secondary monitors**: lightweight mirror windows with input handling, cursor hiding, and 2-second grace period
- **Emulation mode**: `/emu` (dual) or `/emu:N` (N=2~4) for testing multi-monitor layouts on a single display

### Overlay & Clock
- **System info overlay**: CPU model, RAM, resolution, FPS, zoom level, iteration count
- **7-segment digital clock**: bouncing LED-style display with premultiplied alpha blending

### Unified Installer
- **Single binary**: installer + screen saver combined — extension-based mode detection (.exe=install, .scr=screensaver)
- **Auto-update notification**: checks GitHub Releases for new versions, shows download link in settings dialog

### Lightweight & Self-Contained
- **Single file** (~500 KB .exe) — no runtime dependencies, no .NET, no Java
- **Static CRT** (/MT) — runs on a clean Windows install
- **Built-in installer** — auto UAC elevation, no separate installer needed
- **Zero external dependencies** — everything is built from scratch in C++20

---

## System Requirements

| Component | Minimum |
|-----------|---------|
| OS | Windows 10 / 11 (64-bit) |
| CPU | Intel Haswell (2013+) or AMD Excavator (2015+) with AVX2 |
| RAM | 64 MB free |
| Disk | < 1 MB |

---

## Installation

1. [Download](#download) `FractalSaver.exe` (or `FractalSaverLite.exe` for older PCs)
2. Double-click to run — the launcher dialog appears
3. Click **"Install"** — UAC will auto-elevate and install the screen saver
4. Select **"Fractal"** from the dropdown in the Screen Saver Settings and click **Apply**

To preview without installing: click **"Preview"** in the launcher, or run `FractalSaver.exe /s`

To uninstall: run the same .exe again and click **"Uninstall"**, or run with `/u` flag

---

## Settings

| Setting | Options | Default |
|---------|---------|---------|
| Zoom Speed | 1 (slow) ~ 10 (fast) | 8 |
| Color Style | Auto Cycle / Smooth / Contour / Stripe / TIA / Classic | Auto Cycle |
| Fractal Rendering | On / Off | On |
| Info Overlay | On / Off | On |
| Digital Clock | On / Off | Off |

---

## Technical Highlights

- **C++20** with MSVC Build Tools 2022
- **AVX2 intrinsics** (`__m256d`) — 4 pixels computed simultaneously
- **Brent cycle detection** — early exit for interior pixels beyond iteration 32
- **ColorPalette LUT** — cos (1024), log (4096), exp (512) entry lookup tables
- **Cardioid/Period-2 bulb early rejection** — SIMD-accelerated, skips most interior points without iteration
- **One-pass row rendering** with stack-allocated buffers (~40 KB in L1 cache)
- **Waypoint zoom paths** — target coordinates change at specific zoom depths
- **Render-time-based zoom deceleration** — maintains smooth interpolation at any render speed
- **Whole Program Optimization** (/GL + /LTCG) and Link-Time Code Generation

---

## The Math Behind the Beauty

FractalSaver computes the classic Mandelbrot iteration **z = z² + c** for every pixel, with several enhancements:

- **Smooth iteration count**: `n + 1 - log₂(log₂(|z|))` eliminates color banding
- **Cosine palette**: Inigo Quilez-style color generation with per-channel phase offsets
- **Orbit traps** (Stripe/TIA): accumulate angular and radial orbit statistics for rich texturing
- **Dynamic max iterations**: `200 + 120 * log₂(zoom)` with deep zoom boost, rounded to 100 units

---

## Download

| Edition | Description | Download |
|---------|-------------|----------|
| **Full** | Native resolution, uncapped FPS | [**FractalSaver.exe**](https://github.com/jogakdal/fractal-screensaver/releases/latest/download/FractalSaver_v1.3.0.exe) |
| **Lite** | Half resolution, 30 fps cap — for older PCs | [**FractalSaverLite.exe**](https://github.com/jogakdal/fractal-screensaver/releases/latest/download/FractalSaverLite_v1.3.0.exe) |

- [All releases](https://github.com/jogakdal/fractal-screensaver/releases)
- [WinCustomize](https://www.wincustomize.com/explore/screensavers/1693/)

---

## Support the Developer

If you enjoy FractalSaver, consider supporting its development:

[![Buy Me a Coffee](https://img.shields.io/badge/Buy%20Me%20a%20Coffee-ffdd00?style=for-the-badge&logo=buy-me-a-coffee&logoColor=black)](https://buymeacoffee.com/jogakdal)
[![GitHub Sponsors](https://img.shields.io/badge/GitHub%20Sponsors-ea4aaa?style=for-the-badge&logo=github-sponsors&logoColor=white)](https://github.com/sponsors/jogakdal)

---

## Author

**Yongho Hwang** ([@jogakdal](https://github.com/jogakdal))

- **Email**: jogakdal@gmail.com / jogakdal@naver.com
- **Blog (Velog)**: https://velog.io/@jogakdal
- **Blog (Tistory)**: https://crescent-moon.tistory.com/
- **Blog (Naver)**: https://blog.naver.com/jogakdal

---

## License

All rights reserved. This software is provided as-is for personal use.

---

*FractalSaver — Where mathematics meets art.*
