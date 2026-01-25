# ShadowNudge - AI Posture Companion

**Version**: 0.1.0-MVP  
**Privacy**: Zero telemetry, pure local-first  
**Target**: Desktop/laptop browsers only (Chrome 90+)  

ShadowNudge uses MediaPipe pose detection to monitor your posture and provide gentle nudges when you slouch or drift from your reference pose.

## Quickstart

### Installation

```bash
# Install dependencies
bun install

# Download MediaPipe model files (holistic-lite, ~4MB)
mkdir -p public/models
cd public/models
# Download from: https://storage.googleapis.com/mediapipe-models/...
# TODO: Add model download script
```

### Development

```bash
# Start dev server (http://localhost:3000)
bun run dev

# Build for production
bun run build

# Check code quality
bun run lint
bun run check

# Auto-format code
bun run format
```

### Deploy

```bash
# Deploy to Cloudflare Pages
bun run deploy
```

## Browser Requirements

- Chrome 90+ (recommended: 113+ for WebGPU)
- Edge 90+
- Firefox 88+ (OffscreenCanvas worker support limited)
- Safari 15+

**NOT supported on mobile devices** - blocked by user agent detection.

## Project Structure

```
├── public/               # Static assets served by dev server
│   ├── index.html       # App shell
│   ├── style.css        # Vanilla CSS
│   ├── main.js          # Compiled TypeScript (generated)
│   ├── main.js.map      # Source maps (generated)
│   ├── sw.js           # Service worker
│   └── models/         # MediaPipe model files
├── src/
│   ├── main.ts         # App bootstrap and orchestration
│   ├── core/           # Detection, storage, nudge engine
│   │   ├── detector.ts
│   │   ├── nudge-engine.ts
│   │   └── reference-store.ts
│   └── workers/        # OffscreenCanvas rendering
│       └── ghost-renderer.ts
├── build.ts            # Production build script
├── dev-server.ts       # Development server
└── package.json
```

## Key Features

- **Local Processing**: All pose detection runs in-browser using WebGPU/WASM
- **No Network**: After initial load, zero external requests
- **Capture Mode**: Set reference pose on app launch
- **Gentle Nudges**: Visual/audio alerts when posture deviates
- **Performance**: Frame decimation (1/3 frames) for 60-90 FPS

## Performance Notes

- First load: ~30s (model download + WASM compilation)
- Subsequent loads: <2s (from service worker cache)
- Target: 60-90 FPS on modern laptops
- Auto-switches to lite mode when battery <20%

## Development Notes

- No testing framework for MVP - rely on TypeScript LSP
- No CSS frameworks - vanilla CSS only
- Hand-written service worker for versioning control
- Biome for linting/formatting (format on save recommended)

## License

MIT

## Contributing

This is a personal project. Issues and PRs welcome for non-MVP features.
