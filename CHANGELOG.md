# Changelog

[한국어](CHANGELOG.ko.md)

## v1.4.0 (2026-04-11)

### New Features
- **Sculpted 3D color style** (6th style) — mathematically accurate surface normals give the fractal boundary a true 3D relief appearance
  - Distance Estimator (DE) partial derivative: dc_{n+1} = 2*z_n*dc_n + 1 accumulated per iteration
  - On escape, u = z/dc complex division yields a unit 2D surface normal in screen space
  - External terrain is blended in from the spatial gradient of smoothIter (potential function) so the area outside the set also gains smooth relief
  - Blinn-Phong shading: ambient 0.18 + diffuse 0.55*(N·L) + specular 0.55*(N·H)^16
  - Light L = (-0.5, -0.4, 0.7681), halfway H pre-computed as a constant
  - 2x2 supersamples carry independent normals, giving free anti-aliasing on the silhouette
  - Available via settings dropdown (7th item) or `/style:6` command line

### Improvements
- **PerturbationSIMD** extended with AVX2 dc partial-derivative lanes (vDcr/vDci)
  - `needsNormal` flag ensures other styles pay zero cost (only 4 MUL + 2 FMA per iteration when style 6 is active)
  - Scalar fallback also updated to match
  - `ComputeRowDirect` signature extended with `normalXOut`/`normalYOut` output buffers
- **ColorPalette::kInteriorEpsilon** promoted to a shared public constant (used by both ColorPalette and PerturbationSIMD)

### Bug Fixes
- Silenced two long-standing compiler warnings
  - `ZoomAnimator` C4456: shadowed `zoomDepth` in waypoint block renamed to `curZoomDepth`
  - `FractalContent` C4996: `_wfopen` replaced with `_wfopen_s`

### Downloads
| Edition | Description | Download |
|---------|-------------|----------|
| **Full** | Native resolution, uncapped FPS | [**FractalSaver_v1.4.0.exe**](https://github.com/jogakdal/fractal-screensaver/releases/download/v1.4.0/FractalSaver_v1.4.0.exe) |
| **Lite** | Half resolution, 30 fps cap | [**FractalSaverLite_v1.4.0.exe**](https://github.com/jogakdal/fractal-screensaver/releases/download/v1.4.0/FractalSaverLite_v1.4.0.exe) |

## v1.3.1 (2026-04-05)

### Bug Fixes
- Fixed screen flickering on secondary monitor in multi-monitor setups
  - Added back buffer to mirror windows for flicker-free WM_PAINT handling
  - Moved mirror window update to after frame compositing (prevents reading half-rendered surface)
  - Removed unnecessary CS_HREDRAW|CS_VREDRAW window style from mirror windows

### Downloads
| Edition | Description | Download |
|---------|-------------|----------|
| **Full** | Native resolution, uncapped FPS | [**FractalSaver_v1.3.1.exe**](https://github.com/jogakdal/fractal-screensaver/releases/download/v1.3.1/FractalSaver_v1.3.1.exe) |
| **Lite** | Half resolution, 30 fps cap | [**FractalSaverLite_v1.3.1.exe**](https://github.com/jogakdal/fractal-screensaver/releases/download/v1.3.1/FractalSaverLite_v1.3.1.exe) |

## v1.3.0 (2026-03-27)

### New Features
- **Waypoint zoom paths**: zoom target coordinates automatically change at specific zoom depths, following curated routes

### Improvements
- **CPU-only rendering**: GPU render mode completely removed; unified to AVX2 SIMD multithreaded CPU rendering
  - Consistent zoom interpolation quality with no inter-frame jumps
- **Brent cycle detection**: early exit for interior pixels in all mini-bulbs (activates after iteration 32)
- **ColorPalette LUT optimization**: cos/log/exp replaced with lookup tables + linear interpolation
- **TIA accuracy**: changed from squared approximation to exact sqrt-based triangle inequality formula
- **Zoom interpolation method**: switched from source crop to destination enlargement (eliminates integer truncation jitter)
- **Render-time-based zoom deceleration**: automatically slows zoom when render time increases, maintaining interpolation quality
- **39 verified routes**: slow routes removed via render cost testing, fast deep-zoom routes restored, 6 routes hand-tuned with waypoints
- **Longer fade in/out**: increased from 0.4s to 1.1s for more visible transitions
- **CPU multithread batching**: 4-row batch fetch_add reduces atomic contention
- **maxIter rounded to 100**: eliminates per-frame maxIter fluctuation for TIA orbit trap stability
- **Faster shutdown**: worker threads check cancellation flag during row rendering for immediate exit
- **GPU usage display removed**: GPU load measurement/display disabled for CPU-only mode
- **Dead code cleanup**: removed boundary tracking, redirect, curvature detection and related constants/variables

### Bug Fixes
- Fixed deep zoom (10^8+) screen warping to another region then snapping back (interpolation zoomRatio clamped from 3.0 to 8.0)
- Fixed exterior pixels at RGB(0,0,0) being treated as transparent by TransparentBlt (minimum brightness guaranteed)
- Added `/style:N` command-line option for forcing a specific color style

### Downloads
| Edition | Description | Download |
|---------|-------------|----------|
| **Full** | Native resolution, uncapped FPS | [**FractalSaver_v1.3.0.exe**](https://github.com/jogakdal/fractal-screensaver/releases/download/v1.3.0/FractalSaver_v1.3.0.exe) |
| **Lite** | Half resolution, 30 fps cap | [**FractalSaverLite_v1.3.0.exe**](https://github.com/jogakdal/fractal-screensaver/releases/download/v1.3.0/FractalSaverLite_v1.3.0.exe) |

### Installation

1. Download **FractalSaver_v1.3.0.exe** (or Lite version) from the table above
2. Run the downloaded .exe file
3. Click **"Install"** in the launcher dialog
   - Administrator privileges will be requested automatically (UAC)
   - The screensaver is copied to System32 and registered
   - The Windows Screen Saver Settings dialog opens automatically
4. Select **"Fractal"** from the dropdown and click **Apply**

To **uninstall**, run the same .exe again and click "Uninstall", or run with `/u` flag from command line.

To **preview** without installing, click "Preview" in the launcher dialog, or run: `FractalSaver_v1.3.0.exe /s`

### System Requirements
- Windows 10 or later
- AVX2-capable CPU (Intel Haswell 2013+ / AMD Excavator 2015+)

## v1.2.4 (2026-03-13)

### Improvements
- **Settings dialog redesign**: migrated from groupbox layout to TaskDialog-style visual design
  - White header panel displaying app title and version
  - Bold section labels replace traditional groupboxes
  - Display section moved to the top for easier access
  - "Fractal rendering" toggle now disables both Rendering and Animation sections when unchecked
- **Author & sponsor info**: new footer area in settings dialog with author links and sponsor buttons
  - Blog links (Velog, Tistory, Naver Blog) and mailto contact
  - Sponsor links (Buy Me a Coffee, GitHub Sponsors)
  - SysLink controls with automatic centering, white background panel
- **Checkbox captions updated** across all 7 languages:
  - "Show animation" -> "Fractal rendering"
  - "Show overlay (zoom / FPS)" -> "Show info text"

### Downloads
| Edition | Description | Download |
|---------|-------------|----------|
| **Full** | Native resolution, uncapped FPS | [**FractalSaver_v1.2.4.exe**](https://github.com/jogakdal/fractal-screensaver/releases/download/v1.2.4/FractalSaver_v1.2.4.exe) |
| **Lite** | Half resolution, 30 fps cap | [**FractalSaverLite_v1.2.4.exe**](https://github.com/jogakdal/fractal-screensaver/releases/download/v1.2.4/FractalSaverLite_v1.2.4.exe) |

## v1.2.3 (2026-03-12)

### Improvements
- **CPU usage in overlay**: moved from the dynamic info line to the CPU spec line
  - Now displayed as "CPU: Intel Core i7-... (15% in use)" (localized for all 7 languages)
  - Dynamic line simplified to: Render / Zoom / FPS / Style / Path
- **CPU usage updates when rendering is off**: previously froze at the last measured value when animation was disabled via settings
- **Overlay when rendering is off**: the dynamic info line (render/zoom/FPS/style) is hidden when animation is disabled, showing only system info and CPU usage

### Downloads
| Edition | Description | Download |
|---------|-------------|----------|
| **Full** | Native resolution, uncapped FPS | [**FractalSaver_v1.2.3.exe**](https://github.com/jogakdal/fractal-screensaver/releases/download/v1.2.3/FractalSaver_v1.2.3.exe) |
| **Lite** | Half resolution, 30 fps cap | [**FractalSaverLite_v1.2.3.exe**](https://github.com/jogakdal/fractal-screensaver/releases/download/v1.2.3/FractalSaverLite_v1.2.3.exe) |

## v1.2.2 (2026-03-11)

### New Features
- **Animation toggle**: independently enable/disable fractal rendering, overlay, and clock
  - New checkbox in Display group of settings dialog (all 7 languages)
  - Real-time toggle via F1 settings during screensaver

### Improvements
- **Overlay text visibility**: two-pass shadow + text AlphaBlend compositing
  - Pass 1: dark shadow for contrast on bright backgrounds
  - Pass 2: semi-transparent white text for readability on dark backgrounds
  - 32-bit DIB + premultiplied alpha (same pipeline as clock)
- **Clock bounce direction**: always diagonal (45/135/225/315 degrees)
- **Clock colon blinking**: precise 1-second cycle (0.5s on / 0.5s off)
- **Overlay text order**: changed from GPU-CPU-RAM to CPU-RAM-GPU
- **F1 help text**: simplified to "F1: Settings" (all 7 languages)
- **GPU TDR prevention**: Compute Shader dispatched in 64-row chunks with Flush()
- **GPU Device Lost recovery**: auto-fallback to CPU rendering on device loss
- **Long-run stability**: ZoomAnimator async data race fix, _malloca null check, RenderSurface resource leak prevention

### Downloads
| Edition | Description | Download |
|---------|-------------|----------|
| **Full** | Native resolution, uncapped FPS | [**FractalSaver_v1.2.2.exe**](https://github.com/jogakdal/fractal-screensaver/releases/download/v1.2.2/FractalSaver_v1.2.2.exe) |
| **Lite** | Half resolution, 30 fps cap | [**FractalSaverLite_v1.2.2.exe**](https://github.com/jogakdal/fractal-screensaver/releases/download/v1.2.2/FractalSaverLite_v1.2.2.exe) |

## v1.2.1 (2026-03-09)

### New Features
- **Multi-monitor support**: renders on the largest monitor, mirrors to all others with aspect-ratio-preserving center crop
  - Overlay and clock displayed on primary monitor only
  - Single monitor works exactly as before
- **Multi-monitor emulation**: `/emu` (dual) or `/emu:N` (N=2~4) for testing multi-monitor layouts on a single display

### Improvements
- **Overlay readability**: double-outline text rendering for visibility on any background (bright/dark)
- **Locale-aware update link**: Korean locale users directed to CHANGELOG.ko.md
- **Auto-kill on install**: existing screensaver processes terminated automatically before installation

### Changes
- Replaced unnecessary non-ASCII characters with ASCII equivalents in comments and docs

## v1.2.0 (2026-03-04)

### New Features
- **7 languages**: English, Korean, Japanese, Simplified/Traditional Chinese, German, French, Spanish
  - Auto-detects OS locale, override with `/lang:xx`
  - Installer, settings dialog, overlay, and system info all localized
- **7-segment digital clock**: bouncing LED-style display with dim off-segments
- **System info overlay**: GPU/CPU model, RAM, resolution, FPS, zoom level, iteration count
- **Triangle Inequality Average (TIA)**: 4th coloring style with radial orbit patterns
- **Periodic screenshots**: `/screenshot:interval[:folder]` command-line option
- **Auto update check**: background check via GitHub Releases API, manual check in settings dialog
- **Lite edition**: half resolution + 30 fps cap for older PCs and integrated GPUs
- **Unified binary**: installer + screen saver combined in a single executable
  - Extension-based mode detection (.exe=install, .scr=screensaver)
  - AV-friendly: no resource embedding patterns

### Improvements
- **Framework/content separation**: ScreenSaverEngine + IScreenSaverContent architecture, framework extracted to wsse.lib
- **Feature flags**: hasOverlay, hasClock, hasAutoUpdate in AppDescriptor
- **Color channel shuffle**: 6 color channels (R/G/B/C/M/Gold) shuffled per target
- **Screenshot format**: changed from BMP to PNG (GDI+ encoder)
- **Color style shuffle**: randomized instead of sequential cycling
- **Redesigned settings dialog**: Zoom Speed slider (1~10), Color Style combo box, Overlay/Clock checkboxes
- **Performance**: CPU and GPU optimizations (SIMD early rejection, FMA3, one-pass rendering, conditional orbit trap)
- **Softer interior coloring**: reduced palette frequency and amplitude for gentler contrast
- **Smart installer**: TaskDialog launcher with install/preview/uninstall options, version comparison
- **Cross-edition policy**: Full and Lite editions are mutually exclusive (auto-uninstall the other)
- **Static CRT**: no Visual C++ runtime dependency

### Bug Fixes
- Fixed desk.cpl preview window starting with black screen (ZoomAnimator SkipFadeIn)
- Fixed color discontinuity at interior/exterior boundary
- Fixed teleport artifact when zoom direction reverses
- Fixed mouse cursor not showing during settings dialog
- Fixed panning speed causing zoom-out appearance
- Improved zoom diversity with better RNG seeding and wider target offsets

## v1.1.0 (2026-03-01)

### New Features
- **6 monochrome color channels**: Red, Green, Blue, Cyan, Magenta, Gold
  - Distinct interior/exterior tinting within the same color family

### Improvements
- Expanded zoom locations from 25 to 100 (15 categories)
- PCA curvature filtering: automatically skips visually monotonous boundaries
- Self-contained installer with embedded screen saver

## v1.0.0 (2026-02-28)

### Initial Release
- **Real-time Mandelbrot zoom** screen saver for Windows
- **GPU rendering**: D3D11 Compute Shader with double-float emulation (~48-bit precision)
- **CPU rendering**: AVX2 SIMD 4-pixel parallel computation, multi-threaded
- **2x2 supersampled anti-aliasing**
- **Exponential zoom**: adaptive panning + boundary tracking
- **Settings dialog**: Force CPU, Zoom Speed, Color Style, Overlay/Clock
- **Standard Windows screen saver interface**: /s, /c, /p support
