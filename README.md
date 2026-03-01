# FractalSaver

A real-time Mandelbrot fractal screensaver for Windows with GPU-accelerated rendering.

## Features

- **GPU Acceleration**: D3D11 Compute Shader (cs_5_0) with double-float precision
- **CPU Fallback**: AVX2 SIMD 8-wide parallel computation when GPU is unavailable
- **100 Exploration Points**: 15 categories including Seahorse Valley, Elephant Valley, Spiral Galaxies, Mini Mandelbrot Copies, and more
- **Smart Boundary Tracking**: Curvature-based filtering ensures the camera always follows visually interesting fractal boundaries
- **Cosine Palette Coloring**: 4 styles (Smooth, Contour, Stripe, TIA) that cycle automatically
- **Smooth Transitions**: FadeIn/Visible/FadeOut state machine animation

## Download

Get the latest version from the [Releases](https://github.com/jogakdal/fractal-screensaver/releases) page.

## Installation

1. Run `install.exe` to install the screensaver automatically
2. Or manually copy `FractalSaver.scr` to `C:\Windows\System32\`, then select it in Desktop > Personalize > Lock Screen > Screen Saver Settings

## System Requirements

- Windows 10/11 (64-bit)
- DirectX 11 compatible GPU (recommended) or AVX2 capable CPU
- Minimum 512MB GPU memory (for GPU mode)

## Screenshots

(coming soon)

## License

All rights reserved. This software is provided as-is for personal use.
