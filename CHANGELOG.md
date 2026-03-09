# Changelog

[한국어](CHANGELOG.ko.md)

## v1.2.1 (2026-03-09)

### New Features
- **Multi-monitor support**: renders on the largest monitor, mirrors to all others with aspect-ratio-preserving center crop
  - Overlay and clock displayed on primary monitor only
  - Single monitor works exactly as before
- **Multi-monitor emulation**: `/emu` (dual) or `/emu:N` (N=2~4) for testing multi-monitor layouts on a single display
- **Unified binary**: installer + screen saver combined in a single executable
  - Extension-based mode detection (.exe=install, .scr=screensaver)
  - AV-friendly: no resource embedding patterns

### Improvements
- **Color channel shuffle**: 6 color channels (R/G/B/C/M/Gold) now shuffle per target instead of cycling sequentially
- **Screenshot format**: changed from BMP to PNG for smaller file size
- **Overlay readability**: double-outline text rendering for visibility on any background (bright/dark)

### Bug Fixes
- Fixed desk.cpl preview window starting with black screen

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

### Improvements
- **Color style shuffle**: randomized instead of sequential cycling
- **Redesigned settings dialog**: Zoom Speed slider (1~10), Color Style combo box, Overlay/Clock checkboxes
- **Performance**: CPU and GPU optimizations (SIMD early rejection, FMA3, one-pass rendering, conditional orbit trap)
- **Softer interior coloring**: reduced palette frequency and amplitude for gentler contrast
- **Smart installer**: TaskDialog launcher with install/preview/uninstall options, version comparison
- **Cross-edition policy**: Full and Lite editions are mutually exclusive (auto-uninstall the other)
- **Static CRT**: no Visual C++ runtime dependency

### Bug Fixes
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
