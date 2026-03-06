---
tags:
  - gdd
  - technical
  - platform
  - requirements
---

# Platform & Requirements

Defines the target platforms, minimum requirements, cross-platform strategy, and technology stack considerations for Stormbound Online. This document establishes what the game runs on, what it demands from the player's hardware, and how the deployment model supports the server-authoritative architecture defined in [[Technical Architecture]].

---

## 1. Primary Platform: Web Browser

Stormbound Online is a **reactive text-based MMORPG**. The interface is panels, buttons, text logs, and cards — not rendered 3D environments or pixel art worlds. The browser is the natural platform for this kind of application.

### Target Browsers

| Browser | Minimum Version | Notes |
|---------|----------------|-------|
| Google Chrome | 110+ | Primary development target. Largest market share. |
| Mozilla Firefox | 110+ | Full support. WebSocket implementation tested explicitly. |
| Microsoft Edge | 110+ | Chromium-based. Inherits Chrome compatibility. |
| Apple Safari | 16+ | WebSocket and CSS Grid support confirmed from this version. Tested on macOS and iOS Safari. |

**Policy:** Support the last 4 major versions of each evergreen browser. Older versions receive no testing or bug fixes.

### No Plugin Requirements

- No Flash, Java, Silverlight, or browser extensions required
- No WebGL required (the game is text and UI panels, not rendered graphics)
- No WebAssembly required at MVP (potential optimization target post-MVP for combat resolver client-side prediction)
- Standard HTML5, CSS3, JavaScript — nothing exotic

### Why Browser First

| Advantage | Reasoning |
|-----------|-----------|
| Zero install friction | Players access the game via URL. No download, no launcher, no update client. |
| Cross-platform by default | Any OS with a modern browser runs the game. No platform-specific builds at launch. |
| Rapid iteration | Web deployments are instant. No app store review cycles. No client patch distribution. |
| Natural fit for text UI | The reactive panel-based interface described in [[UI Design]] maps directly to web UI frameworks. DOM manipulation, CSS styling, and reactive data binding are mature, well-tooled technologies. |
| WebSocket native support | Real-time combat communication per [[Technical Architecture#Networking Model]] uses WebSocket, which is a browser-native protocol. |

---

## 2. Secondary Platform: Desktop Client

A standalone desktop application for players who prefer a dedicated window, system tray presence, and separation from their browser tabs.

### Implementation Approach

| Option | Recommendation | Trade-offs |
|--------|---------------|------------|
| **Tauri** (Rust-based) | **Recommended** | Optimizes for: small binary size (~10-15MB vs 150MB+ Electron), lower memory footprint, native OS integration. Sacrifices: smaller ecosystem than Electron, fewer off-the-shelf plugins. |
| **Electron** (Chromium-based) | Fallback | Optimizes for: mature ecosystem, proven at scale (Discord, VS Code), extensive plugin library. Sacrifices: large binary size, high base memory usage (~150-300MB before game loads). |

**Decision rationale:** Stormbound Online is a text-based game. The heavy runtime of Electron is disproportionate to the application's rendering demands. Tauri wraps the OS-native webview, producing a lighter client that respects the game's minimalist UI philosophy.

### Desktop-Specific Features

| Feature | Description |
|---------|-------------|
| Native window management | Resizable, minimize to tray, fullscreen toggle |
| System tray | Background presence with notification badges (marketplace sale completed, dungeon queue ready, guild events) |
| Offline notification support | OS-native notifications when the game is minimized or in tray. Triggered by SSE channel per [[Technical Architecture#Protocol Selection]]. |
| Auto-update | Silent background updates. Game is always current. No manual patching. |
| Deep linking | `stormbound://` protocol handler for build codes, party invites, marketplace links |

### Codebase Strategy

**Same codebase.** The desktop client wraps the same web application. No forked code. No platform-specific game logic. The wrapper provides:

1. Native window chrome (title bar, minimize/maximize/close)
2. System tray integration
3. OS notification bridge
4. Auto-updater
5. Deep link handler

All game logic, UI rendering, combat interaction, and server communication are identical to the browser version.

---

## 3. Mobile Considerations

### Tablet Support (Stretch Goal — Post-MVP)

The reactive panel layout described in [[UI Design]] can adapt to tablet form factors with responsive design.

| Aspect | Approach |
|--------|----------|
| Orientation | **Landscape preferred.** The combat panel, action bar, and combat log require horizontal space. Portrait mode is not supported — the information density is too high. |
| Touch targets | Commit slot selection buttons sized to minimum 44x44px touch targets (Apple HIG / Material Design guidelines). Action bar buttons enlarged for touch. |
| Panel layout | Combat panel and build panel switch to stacked layout on screens < 1024px wide. Hub panel collapses navigation into a hamburger menu. |
| Text size | Base font size increased by 20% on touch devices. Combat log entries use larger line height. |
| Minimum tablet resolution | 1024x768 (iPad Mini class and above) |

### Phone Support (Not Planned)

| Reason | Detail |
|--------|--------|
| UI density | The combat interface requires simultaneous visibility of: action bar (4 gems + normal attack), enemy intent, combat log, status bar, and positioning indicator. This does not compress to phone screens without sacrificing core readability. |
| Information warfare | PvP combat depends on reading the combat log for pattern detection per [[Combat System]]. Small screens make log reading impractical. |
| Commit phase interaction | Selecting and ordering 2-3 actions into commit slots within an 8-10 second window demands precise interaction that phone touch targets cannot reliably support. |

**Position:** Phone support is a non-goal. If community demand warrants reconsideration, it would require a fundamentally different UI — not a responsive adaptation of the existing one. That is a separate product decision.

### PWA Capability

The web application should be built as a **Progressive Web App** to bridge the gap between browser and native:

| PWA Feature | Value |
|-------------|-------|
| Add to Home Screen | Tablet users can pin the game to their home screen for app-like launch |
| Service Worker | Cache static assets (UI framework, fonts, color definitions) for faster load times. Does NOT cache game state — all state is server-authoritative per [[Technical Architecture#Client-Server Authority Model]]. |
| Web App Manifest | Custom icon, splash screen, standalone display mode (hides browser chrome) |
| Push Notifications | Web push for marketplace alerts, queue readiness, guild events when the tab is backgrounded |

**What PWA does NOT provide:** Offline play. The game requires a persistent server connection. The service worker caches presentation assets only.

---

## 4. Minimum Requirements

### Browser Requirements

| Requirement | Minimum | Recommended |
|-------------|---------|-------------|
| Screen resolution | 1280x720 | 1920x1080 |
| Browser RAM allocation | ~512 MB | ~1 GB |
| Internet connection | 1 Mbps stable | 5 Mbps+ |
| Latency (to game server) | < 500ms RTT | < 200ms RTT |
| JavaScript | Enabled (required) | Enabled |
| Cookies / Local Storage | Enabled (session management) | Enabled |
| WebSocket support | Required | Required |

### Latency Tolerance (from Technical Architecture)

Per [[Technical Architecture#Latency Tolerance]], the game's phase-based combat system is latency-tolerant by design:

| Phase | Budget | Player Impact at High Latency |
|-------|--------|-------------------------------|
| DRAW delivery | < 200ms | Player sees their hand slightly delayed. Imperceptible up to 200ms. |
| Commit upload | < 500ms | 8-10 second Commit window absorbs jitter. 500ms RTT still leaves 7.5-9.5s of decision time. |
| Clash result delivery | < 200ms | Brief pause feels like "resolution time." Natural in a text-based interface. |

**Key insight:** Unlike real-time action games, Stormbound Online's turn-based commit structure means players on 300ms+ connections experience no mechanical disadvantage. The 8-10 second Commit window is the design feature that makes this possible. Players on high-latency connections (satellite, mobile tethering, intercontinental) can play competitively.

### Desktop Client Requirements

| Requirement | Minimum | Recommended |
|-------------|---------|-------------|
| OS | Windows 10+ / macOS 12+ / Linux (Ubuntu 22.04+) | Latest stable |
| RAM | 2 GB system (512 MB allocated to client) | 4 GB+ |
| Disk space | 50 MB (Tauri) / 200 MB (Electron fallback) | Same |
| CPU | Any x86-64 or ARM64 processor from 2015+ | Any modern processor |
| GPU | Not required (no GPU rendering) | Not required |
| Internet | Same as browser | Same as browser |

### What Is NOT Required

| Component | Status | Reasoning |
|-----------|--------|-----------|
| Dedicated GPU | Not required | No 3D rendering, no WebGL, no canvas-heavy operations. Text and CSS. |
| High-end CPU | Not required | Client is a render terminal per [[Technical Architecture]]. All game logic runs server-side. |
| Large disk space | Not required | No local asset downloads, no texture packs, no audio files at MVP. |
| Controller / gamepad | Not supported | Text-based UI with button interaction. Mouse/keyboard and touch are the input methods. |

---

## 5. Cross-Platform Play

### Unified Server Architecture

All platforms connect to the same server infrastructure. There is no platform segregation.

| Aspect | Implementation |
|--------|----------------|
| Server access | All clients (browser, desktop, tablet PWA) connect to the same Gateway API per [[Technical Architecture#Architecture Overview]] |
| Account system | Platform-agnostic accounts. Email + password or OAuth (Google, Discord). No platform-locked accounts. |
| Character data | Server-authoritative. A player can start a session in Chrome on their desktop, close it, and resume on their tablet PWA. Character state is identical. |
| Settings sync | UI preferences (panel layout, text size, color overrides per [[Accessibility System]]) stored server-side and synced across devices |
| Build codes | Universal. A build code generated on any platform works on every platform. Build codes are text strings per [[UI Design#Build Panel]]. |
| Marketplace | Single global marketplace. No platform-segmented economies. |
| Social | Friend lists, guild membership, chat history — all server-side, all cross-platform. |

### No Platform-Exclusive Content

There is no content, cosmetic, or mechanical feature available on one platform but not another. The game is the same game everywhere. This is a hard constraint.

**Rationale:** Platform exclusives fragment the player base, create support burden, and contradict the build-diversity-first philosophy. A build code shared on a community forum must work identically regardless of where the receiving player plays.

---

## 6. Technology Stack Recommendation

These are high-level design recommendations. Engineering decisions on specific libraries and versions are deferred to implementation.

### Frontend

| Layer | Recommendation | Rationale |
|-------|---------------|-----------|
| UI framework | **Svelte** or **SolidJS** | Optimizes for: reactive UI performance, small bundle size, minimal runtime overhead. Text-based games need snappy DOM updates for combat log entries, action bar state, and status changes. Both frameworks compile to minimal JavaScript with fine-grained reactivity. Sacrifices: smaller hiring pool than React, fewer off-the-shelf component libraries. |
| Alternative | **React** | Optimizes for: largest ecosystem, easiest hiring, most community resources. Sacrifices: virtual DOM overhead (acceptable for text UI but unnecessary), larger bundle size. |
| State management | Framework-native (Svelte stores / SolidJS signals / React context + reducer) | No external state library needed at MVP. Game state comes from the server via WebSocket. Client state is minimal: current panel view, local UI preferences, pending commit selection. |
| Styling | **CSS Modules** or **Tailwind CSS** | The text color language defined in [[UI Design#Text Color Language]] requires precise color control. CSS variables for the element and alignment color palettes. |
| WebSocket client | Native `WebSocket` API or lightweight wrapper (e.g., `reconnecting-websocket`) | No heavy WebSocket library needed. The protocol is simple: JSON messages with typed event names per [[Technical Architecture#Message Format]]. |

### Communication

| Protocol | Client Implementation |
|----------|-----------------------|
| WebSocket | Combat phases (DRAW, COMMIT, CLASH_RESULT, AFTERMATH), hub presence, chat, real-time notifications |
| REST (HTTPS) | Marketplace browse/search, build inspection, profile queries, crafting, inventory management |
| SSE | Marketplace price alerts, world event announcements, guild notifications (fallback channel) |

Per [[Technical Architecture#Protocol Selection]], these three protocols cover all client-server communication needs.

### PWA Infrastructure

| Component | Purpose |
|-----------|---------|
| Service Worker | Static asset caching, push notification handling |
| Web App Manifest | App metadata, icons, display mode, theme color |
| IndexedDB | Client-side cache for combat log history (last 5 matches for offline review — read-only archive, not game state) |

### Desktop Wrapper

| Component | Purpose |
|-----------|---------|
| Tauri runtime | Native window, system tray, OS notifications, auto-updater, deep linking |
| Same web bundle | Loaded from local files, not from a URL. Faster startup, no CDN dependency. |

---

## 7. Deployment Model

### Always Online

Stormbound Online is a **server-authoritative MMO**. There is no offline mode.

| Aspect | Policy |
|--------|--------|
| Server dependency | All game state lives on the server. The client is a render terminal. Per [[Technical Architecture#Client-Server Authority Model]]: "Clients are render terminals." |
| Offline play | Not supported. Not planned. The game's core systems (combat resolution, NPC economy, behavioral identity, gem provenance) require server computation. |
| Single-player mode | Does not exist. Even solo PvE content runs on server-managed instances per [[Technical Architecture#Dungeon Instances]]. |

### Connection Loss Handling

Per [[Technical Architecture#State Persistence & Recovery]], the game handles disconnection gracefully across all contexts:

| Context | Behavior | Player Experience |
|---------|----------|-------------------|
| Hub (social) | Reconnect returns to same hub shard. No data loss. | Brief loading screen. Chat history preserved. |
| PvP combat | 15-second reconnection window. Auto-commit if timer expires. Forfeit after 60 seconds. | "Reconnecting..." overlay. Combat log replays missed rounds on reconnect. |
| PvE dungeon | 60-second reconnection window. AI auto-commits for disconnected player. | "Reconnecting..." overlay. Party continues without interruption. |
| Marketplace transaction | Atomic. Either completed or not. | Transaction succeeds or fails cleanly. No partial state. |
| Crafting | Atomic. Materials consumed and item created, or neither. | Same as marketplace. |

### Reconnection UX

When connection is lost, the client displays:

```
+-----------------------------------------------+
|                                                |
|           CONNECTION LOST                      |
|                                                |
|       Attempting to reconnect...               |
|       ████████████░░░░  Attempt 3/5            |
|                                                |
|       Your game state is safe.                 |
|       Combat will auto-commit if needed.       |
|                                                |
+-----------------------------------------------+
```

- Client attempts reconnection with **exponential backoff and jitter** per [[Technical Architecture#Risks]] (prevents thundering herd on server restart)
- 5 retry attempts over ~30 seconds before showing "Connection Failed — Retry / Exit" prompt
- No game state is lost. The server holds all state. Reconnection restores the player to their exact prior context.

### Regional Deployment

| Phase | Deployment | Target Region |
|-------|-----------|---------------|
| MVP | Single region | US-East or EU-West (based on beta population geography) |
| Growth (10K players) | Single region, multi-AZ | Same region, redundancy across availability zones |
| Scale (50K players) | Multi-region | US-East + EU-West + AP-Southeast. Players connect to nearest region. |

Per [[Technical Architecture#Risks]], single-region deployment at MVP is acceptable because the 8-10 second Commit window tolerates 200ms+ RTT for intercontinental play. Multi-region deployment is a scaling optimization, not a launch requirement.

---

## Platform Decision Summary

| Platform | Status | Priority | Timeline |
|----------|--------|----------|----------|
| Web browser (Chrome, Firefox, Safari, Edge) | **Primary** | Critical | MVP |
| Desktop client (Tauri wrapper) | **Secondary** | High | MVP or shortly after |
| Tablet (responsive web / PWA) | **Stretch** | Medium | Post-MVP |
| Phone | **Not planned** | Low | Not on roadmap |

---

## Related Pages

- [[Technical Architecture]] -- Server architecture, networking, persistence
- [[UI Design]] -- Panel layout, text color language, responsive feedback
- [[Accessibility System]] -- Settings that affect cross-platform display
- [[State Persistence & Recovery]] -- Disconnect handling and data guarantees
- [[GDD Hub]] -- Master document index
