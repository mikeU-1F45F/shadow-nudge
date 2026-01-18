# ShadowNudge Development Tasks & Milestones

**Version**: 0.1.0-MVP  
**Last Updated**: 2026-01-18  
**Status**: Planning Phase

---

## Multi-Agent Task Management System

### Task Status Markers
Mark tasks inline using `<!-- STATUS: TODO -->` comments at the task header level:

- `<!-- STATUS: TODO -->` - Not started, ready for work
- `<!-- STATUS: WIP:agent-name -->` - In progress (include agent/worktree name)
- `<!-- STATUS: DONE -->` - Completed and verified
- `<!-- STATUS: BLOCKED:reason -->` - Blocked, cannot proceed

Example:
```markdown
<!-- STATUS: WIP:main-thread -->
### 1.1 Implement Mobile Detection Gate
- [x] UA detection logic
- [ ] Edge case testing
```

### Worktree Development Pattern
For parallel workstreams, create worktrees per phase:

```bash
# Create worktree for Phase 1
git worktree add ../shadow-nudge-phase1 phase1-mediapipe

# Agents work in respective worktrees
# When done, merge back to main
```

**File Locking**: Only one agent should own a file at a time. Check for `<!-- STATUS: WIP:agent-name -->` comments in source files.

---

## Phase 0: Foundation & Tooling (Week 1)

### 0.1 Repository Structure & Configuration
<!-- STATUS: TODO -->
- [ ] Initialize `package.json` with version 0.1.0
  - Add Bun test script: `"test": "bun test"`
  - Add TypeScript config
- [ ] Create `public/` directory structure
- [ ] Create `src/core/`, `src/workers/` directories
- [ ] Add `wrangler.toml` for Cloudflare Pages
- [ ] Create `.gitignore` (exclude build artifacts, models)
- [ ] Create `README.md` with quickstart

### 0.2 Development Tooling
<!-- STATUS: TODO -->
- [ ] Configure TypeScript (`tsconfig.json`, strict mode)
- [ ] Set up Bun testing framework
  - Create `tests/unit/` and `tests/integration/` directories
  - Add test utilities for mocking MediaPipe, IndexedDB
- [ ] Configure build pipeline (bun build or esbuild)
- [ ] Add development server config

### 0.3 Core Project Files (Skeleton)
<!-- STATUS: TODO -->
- [ ] Create `src/main.ts` (empty shell with imports)
- [ ] Create `public/index.html` (minimal HTMX-based shell)
- [ ] Create `public/sw.js` (service worker skeleton)
- [ ] Create placeholder files in `src/core/` and `src/workers/`

**Dependencies**: None  
**Outputs**: Working build pipeline, test framework, file structure

---

## Phase 1: Mobile Gate & MediaPipe Integration (Week 2)

### 1.1 Mobile Detection & Browser Warning
<!-- STATUS: TODO -->
- [ ] Implement UA detection in `src/main.ts` (block mobile)
- [ ] Add browser capability detection (WebGPU, OffscreenCanvas, WASM)
- [ ] Create warning UI for unsupported browsers (do not block, just warn)
- [ ] Add console logging for debugging support issues
- **Tests**: Unit tests for UA parsing, capability detection

### 1.2 MediaPipe Integration Core
<!-- STATUS: TODO -->
- [ ] Install `@mediapipe/holistic` package
- [ ] Create `src/core/detector.ts` wrapper class
- [ ] Implement model loading from `/models/` directory
- [ ] Add WebGPU detection and fallback logic
- [ ] Implement frame decimation (every 3rd frame)
- **Tests**: Mock MediaPipe, test initialization paths

### 1.3 Model Management
<!-- STATUS: TODO -->
- [ ] Download holistic-full model binary (~12MB)
- [ ] Place in `public/models/holistic-full.bin`
- [ ] Update service worker to cache model
- [ ] Add model loading progress UI
- **Tests**: Verify SW caching, model loading paths

**Dependencies**: Phase 0  
**Outputs**: Running MediaPipe with webcam, console landmark output

---

## Phase 2: Reference System & Basic UX (Week 3)

### 2.1 HTMX UI Shell
<!-- STATUS: TODO -->
- [ ] Create minimal HTMX interface
  - Capture reference pose button
  - Start/stop monitoring button
  - Status display area
- [ ] Add basic CSS (accessibility: high contrast, keyboard nav)
- [ ] Make UI responsive to landmark data
- [ ] Add ARIA labels for screen readers

### 2.2 IndexedDB Reference Store
<!-- STATUS: TODO -->
- [ ] Create `src/core/reference-store.ts`
- [ ] Implement save/load reference pose (single pose)
- [ ] Add IndexedDB schema versioning
- [ ] Handle initial state (no reference captured)
- **Tests**: IndexedDB mocking, version migrations

### 2.3 Reference Capture Flow
<!-- STATUS: TODO -->
- [ ] Wire up "Capture Reference" button
- [ ] Show countdown/confirmation UI
- [ ] Store landmarks from stable frame
- [ ] Handle capture errors (bad pose, partial detection)

**Dependencies**: Phase 1  
**Outputs**: Can capture and save reference pose, see it in UI

---

## Phase 3: Proximity & Deviation Detection (Week 4)

### 3.1 Hand-Face Proximity Detection
<!-- STATUS: TODO -->
- [ ] Create proximity calculator in `src/core/nudge-engine.ts`
- [ ] Calculate hand-landmark to face-landmark distances
- [ ] Implement threshold-based proximity alert
- [ ] Add short haptic feedback / visual warning
- [ ] Track proximity event frequency
- **Tests**: Hand-fixed distances, edge cases

### 3.2 Pose Deviation Engine
<!-- STATUS: TODO -->
- [ ] Implement landmark comparison algorithm
- [ ] Calculate per-joint deviation scores
- [ ] Aggregate into overall posture score
- [ ] Add configurable sensitivity thresholds
- **Tests**: Known poses, tolerance ranges

### 3.3 Proximity-First Alert System
<!-- STATUS: TODO -->
- [ ] Prioritize hand-face proximity alerts
- [ ] Implement cooldown timers (avoid spam)
- [ ] Add discrete visual indicators
- [ ] Log alerts to console for debugging

**Dependencies**: Phase 2  
**Outputs**: Proximity detection working, basic deviation scoring

---

## Phase 4: Worker Architecture & Alerts (Week 5)

### 4.1 OffscreenCanvas Worker Setup
<!-- STATUS: TODO -->
- [ ] Create `src/workers/ghost-renderer.ts`
- [ ] Transfer canvas control to worker
- [ ] Implement message passing protocol
- [ ] Create worker pool management

### 4.2 Ghost Outline Rendering
<!-- STATUS: TODO -->
- [ ] Draw reference pose outline (semi-transparent)
- [ ] Draw current pose outline (different color)
- [ ] Highlight deviation areas
- [ ] Update on every 3rd frame
- **Tests**: Worker message handling, rendering correctness

### 4.3 Background Flash Alerts
<!-- STATUS: TODO -->
- [ ] Implement backdrop flash for proximity alerts
- [ ] Different flash patterns for different alert types
- [ ] Add audio beep (optional, disabled by default)
- [ ] Make alerts accessible (screen reader announcements)

### 4.4 Nudge Engine Polish
<!-- STATUS: TODO -->
- [ ] Tweak deviation thresholds
- [ ] Add smoothing to reduce jitter
- [ ] Implement confidence filtering (low-confidence landmarks)

**Dependencies**: Phase 3  
**Outputs**: Ghost overlay, visual alerts, smoother detection

---

## Phase 5: Polish, Settings & Release Prep (Week 6)

### 5.1 Settings Panel
<!-- STATUS: TODO -->
- [ ] Add HTMX settings UI (sensitivity, model level)
- [ ] Persist settings to localStorage
- [ ] Add model complexity toggle (0, 1, 2)
- [ ] Alert type preferences

### 5.2 Battery API Integration
<!-- STATUS: TODO -->
- [ ] Monitor battery level
- [ ] Auto-throttle when < 20%
- [ ] Show battery indicator in UI
- [ ] Restore full performance when charging

### 5.3 Performance Optimizations
<!-- STATUS: TODO -->
- [ ] Add GPU memory cleanup (`holistic.close()`)
- [ ] Implement camera resolution selection
- [ ] Optimize JavaScript bundle size
- [ ] Profile and fix hot paths

### 5.4 Documentation & Final Testing
<!-- STATUS: TODO -->
- [ ] Update README with user guide
- [ ] Add inline code comments for complex algorithms
- [ ] Full integration testing across browsers
- [ ] Create release checklist

### 5.5 Release Preparation
<!-- STATUS: TODO -->
- [ ] Bump version to 0.1.0
- [ ] Tag release in git
- [ ] Deploy to Cloudflare Pages
- [ ] Verify service worker update mechanism
- [ ] Smoke test on multiple devices

**Dependencies**: Phase 4  
**Outputs**: MVP 0.1.0 ready for use

---

## Testing Strategy

### Unit Tests (Bun)
**Location**: `tests/unit/`
- Mock MediaPipe responses
- Test algorithms in isolation
- IndexedDB operations
- Utility functions

### Integration Tests (Bun)
**Location**: `tests/integration/`
- Full detector initialization
- End-to-end reference capture flow
- Alert triggering conditions
- Service worker behavior

### Manual Testing Matrix
| Browser | OS | GPU | WebGPU | Test Items |
|---------|----|-----|--------|------------|
| Chrome 113+ | Linux | NVIDIA | Yes | All features |
| Chrome 113+ | macOS | Apple Silicon | Yes | All features |
| Firefox 127+ | Linux | AMD | No | WASM fallback |
| Safari 17 | macOS | Apple Silicon | No | WASM fallback |

**Test Protocol**:
1. Start with no reference pose
2. Capture reference in good posture
3. Slouch - verify deviation alert
4. Move hand to face - verify proximity alert
5. Let idle for 5 minutes - verify no memory leaks
6. Check console for errors/warnings

---

## Agent Coordination Notes

### Sub-agent Scope Examples
- **ml-agent**: Owns `src/core/detector.ts`, MediaPipe integration
- **ui-agent**: Owns `public/index.html`, HTMX interactions
- **worker-agent**: Owns `src/workers/ghost-renderer.ts`
- **storage-agent**: Owns `src/core/reference-store.ts`

### Integration Points
- All agents coordinate through `src/main.ts` (main orchestrator)
- Message protocols defined in shared types
- Weekly integration merges required
- Cross-worktree testing before merging to main

### Conflict Resolution
1. Check this file for `<!-- STATUS: WIP -->` markers
2. Communicate via agent session identifiers
3. Main worktree has authority for integration decisions
4. Revert worktrees that break main branch build

---

## Notes & Open Questions

- **HTMX vs Vanilla JS**: Starting with HTMX for MVP, may migrate to vanilla for performance in v0.2.0
- **Lite Model**: Currently not in plan; will add if user feedback requests lower resource usage
- **Gesture Support**: Future enhancement for custom gestures as reference triggers
- **Data Export**: Consider allowing users to export reference poses (JSON) for backup
- **Telemetry**: Zero telemetry is strict requirement; all logging must be console-only

---

## Milestone Checklist

- [ ] **M0**: Repo structure, build tools, tests (end of Phase 0)
- [ ] **M1**: MediaPipe running with webcam (end of Phase 1)
- [ ] **M2**: Reference capture working (end of Phase 2)
- [ ] **M3**: Proximity detection alerting (end of Phase 3)
- [ ] **M4**: Ghost rendering and visual alerts (end of Phase 4)
- [ ] **M5**: MVP 0.1.0 released (end of Phase 5)
