# Warehouse Dashboard — Design Specification & System Architecture (`design.md`)

This document serves as the authoritative, permanent design specification and decision ledger for the **Warehouse Universal Dashboard**. It encodes all architectural choices, constraints, user feedback corrections, and visual interface rules established during development.

---

## 1. Core Principles & Constraints

### 1.1 `/raudesign` System Design Ethos
When invoked or applied, `/raudesign` enforces strict adherence to the visual and operational guidelines specified in this document:
- **Strict High-Contrast Monochrome**: Deep pitch blacks (`#000000`, `#0a0a0a`, `#141414`) contrasted directly against crisp whites (`#ffffff`, `#f0f0f0`) and calibrated grays (`#333333`, `#555555`, `#888888`).
- **No Emojis**: Emojis are strictly banned from UI elements, buttons, headers, and logs. All icons must be pure geometric SVG vector path elements that inherit `currentColor`.
- **No Animations / No Transitions on Interactive Glyphs**: 
  - Chevron (`>`) and structural indicators do not animate, shift, or translate on hover.
  - Buttons and cards maintain instant, crisp state changes without sluggish motion delays or bouncy visual fluff.
- **Air-Gapped & Offline**: Zero CDNs, zero cloud fonts, zero analytics, zero external network requests. Everything is self-contained and operates purely offline.

---

## 2. Layout & Visual Hierarchy

### 2.1 Minimalist Header & Zero-Clutter Philosophy
- **No Redundant Counter Cards / Stats Banners**: Top metric summary cards (e.g. "Universal Launchers", "Storytold Craft Apps", "GGUF AI Models", "ZIM Knowledge Repos") are completely omitted to prevent visual noise and keep cards immediately accessible above the fold.
- **Clean Streamlined Header**: The header presents solely the system title and status indicator without redundant sub-banners ("100% AIR-GAPPED &bull; DYNAMIC DISCOVERY &bull; ZERO TELEMETRY").
- **Streamlined Search Input**: The universal search bar uses the placeholder `Search...` (replacing long verbose labels like `FILTER APPLICATIONS, MODELS, OR SCRIPTS...`).

### 2.2 Card Anatomy & Geometry
Each application, launcher, model, knowledge archive, manual, map, image, and docset is represented as an interactive card adhering to strict coordinate rules:

1. **Static Right-Center / Leading Chevron (`>`)**:
   - Interactive elements and card-like rows (including quick documentation items in the burger menu) feature a high-contrast static chevron (`>`) indicating navigable or openable actions.
   - It does **not** animate on hover; only its color sharpens from `#888888` to `#ffffff`.
2. **Absolute Top-Right Badges (`[SCRIPT]`, `[DOCS]`)**:
   - **Position**: `position: absolute; top: 10px; right: 12px;`
   - **Behavior**: Floats independently in the upper-right corner of the card.
   - **Title Protection**: The card title row has `padding-right: 90px` to guarantee that long titles never collide with or wrap under the top-right badges.
3. **Uniform Tag & Badge Sizing (The "Max-Width Alignment" Rule)**:
   - When multiple badges or tags appear in a group, column, list, or tree view (e.g. quick document tags `[MANUAL]`, `[OPS]`, `[CATALOG]`, `[RULES]`, or tree badges `[README]`, `[HOWTO]`, `[DOC]`):
     - **All tags must share a uniform, fixed or minimum width calibrated to the largest tag in that container**.
     - Never allow ragged, mismatched tag widths within the same list or component.
     - Center the text within the badge (`display: inline-flex; align-items: center; justify-content: center; text-align: center;`) with `box-sizing: border-box; flex-shrink: 0;`.
4. **Custom High-Contrast Monochrome Scrollbars**:
   - Default browser scrollbars (especially Chrome's rounded, light-themed, or jitter-inducing scrollbars) are replaced with a calibrated high-contrast monochrome design:
     - 8px thin scrollbars (`::-webkit-scrollbar { width: 8px; height: 8px; }`)
     - Pitch black track (`#000000`)
     - Dark gray thumb (`#2a2a2a`) with sharp `#555555` hover contrast and Firefox standard `scrollbar-color: #2a2a2a #000000; scrollbar-width: thin`.
     - Prevents layout shaking or dynamic width collapse during data reloads.
5. **Executable Hover & Copy-to-Clipboard**:
   - Executable links (e.g. `artcraft.exe`, `wordcraft-cli.exe`, batch script names) are rendered with `.exe-link`.
   - **Hover Effect**: Underlined with high contrast (`text-decoration: underline; text-decoration-thickness: 2px; text-underline-offset: 3px;`).
   - **Click Action**: Copies the absolute local filesystem path (e.g. `<drive>\Warehouse\craft\artcraft\bin\artcraft.exe`) directly to the clipboard and surfaces a confirmation toast.

---

## 3. Button Stability & Launch Feedback

### 3.1 Button Width & Layout Shift Prevention
- Action buttons (`RUN`, `CLI`, `LAUNCH`, `MANUAL`, `KIWIX`, `OPEN`, `ZEAL`, `EXPLORE`) have an explicit `min-width: 78px` and `justify-content: center`.
- When an action is triggered, buttons never collapse or stretch unpredictably due to varying text lengths.

### 3.2 Delayed "Launching..." Indicator
- Fast-launching processes that respond within 1 second execute silently without displaying a fleeting, jittery "Launching..." popup.
- A debounced timer (`setTimeout(..., 1000)`) triggers the "Launching..." toast only if the process spawn takes longer than 1,000ms.
- Once launched, the toast updates cleanly to `[+] Started: <Target>` and automatically fades out.

---

## 4. Documentation & Modal Overlay Reader

### 4.1 Fullscreen Toggle & Keyboard Shortcuts
- Clicking an application card or its `MANUAL` button opens its associated `README.md` documentation in a modal overlay.
- **Keyboard Navigation**:
  - <kbd>F</kbd> or <kbd>Shift+F</kbd>: Toggles modal between standard dialog mode (`max-width: 950px; max-height: 88vh;`) and true full-screen overlay (`100vw; 100vh;`).
  - <kbd>Esc</kbd>: Closes the documentation modal immediately.
  - <kbd>/</kbd>: Focuses the universal application/model search filter when modal is closed.

### 4.2 High-Contrast Markdown Renderer
The built-in parser renders standard and GitHub-flavored markdown without external libraries:
- **Headings**: `h1` through `h4` styled with crisp bottom borders and bold typography.
- **Code & Pre**: Inverted dark blocks (`#111111`, border `#333333`) with syntax-safe HTML escaping.
- **Tables**: Fully bordered, zebra-striped, horizontally scrollable tables.
- **Lists & Blockquotes**: Clean indentations and high-contrast gray left borders.

---

## 5. Process Execution & Spawning Architecture

### 5.1 Interactive Windows & Shell Spawning
- **Direct Interactive Spawning**:
  Batch launchers (`.bat`) and CLI binaries are launched using `exec('start "<Title>" cmd.exe /c "<Path>"', { cwd: path.dirname(resolved) })`.
- **Visible Command Prompts**:
  Background node spawning must **never** swallow `stdio` or run invisibly when interactive user visibility is required. Clicking launchers (such as Kiwix, VLC, Calibre, or Craft CLIs) opens their interactive window directly on the desktop.

### 5.2 Port 7070 Auto-Recovery & Process Scavenger
- **No Unhandled EADDRINUSE (505)**:
  `server.js` implements a port scavenger that uses `netstat` on startup to detect and terminate any lingering or orphaned process listening on port `7070`.
- An explicit error listener traps `EADDRINUSE` and automatically retries gracefully rather than throwing unhandled process exceptions.
- `START_DASHBOARD.bat` executes an automated pre-flight PID cleanup before booting Node.

---

## 6. Directory Structure & Assets

```
dashboard\
├── server.js               # Zero-dependency HTTP server, discovery engine, API, and UI
├── START_DASHBOARD.bat     # Clean launcher with port auto-clear and browser launcher
└── design.md               # Authoritative system specification and design ledger
```

---

## 7. Navigation Drawer & Interactive Folder Tree Viewer (`/raudesign`)

### 7.1 Burger Menu & Navigation Drawer
- **Burger Menu Button**:
  - Positioned in the header next to the system title.
  - Strict monochrome SVG wireframe icon (`<svg viewBox="0 0 16 16"><path d="M2 3h12M2 8h12M2 13h12" stroke="currentColor" stroke-width="2" stroke-linecap="square"/></svg>`).
  - Labeled `MENU (M)` with keyboard shortcut binding to <kbd>M</kbd>.
- **Slide-out Navigation Drawer**:
  - Dark container (`#0a0a0a`), bordered on the right with `#ffffff` (2px).
  - Background overlay scrim (`rgba(0, 0, 0, 0.75)`).
  - Dismissible via <kbd>Esc</kbd>, close button `[ESC / X]`, or clicking the background scrim.

### 7.2 Core Document Quick Access
- Direct access to essential root operational documents without searching:
  - `README.md` (Master Survivability & Toolchain Guide)
  - `OFFLINE_OPERATIONS_MANUAL.txt` (Distributed RPC Cluster & Offline RAG Operations)
  - `CATALOG.md` (Complete Application & Tool Catalog)
  - `rules.md` (Drive Operational Rules)
  - `DUMB_VERSION_GUIDE.txt` (30-Second Quick Start)
  - `dashboard/design.md` (System Design Specification)

### 7.3 Lazy Folder Tree Viewer (`/api/tree`)
- **Lazy On-Demand Traversal**:
  - Protects mechanical exFAT drives by querying directories only upon expansion (`/api/tree?path=...`).
  - Prevents recursive filesystem scans.
- **Dedicated Readme & How-To Prioritization**:
  - Files matching `README` or `howto` (`.md`, `.txt`, `.rst`) are sorted to the top of folder listings.
  - Tagged with crisp monochrome badges (`[README]`, `[HOWTO]`, `[DOC]`).
  - Clicking any documentation file directly launches the built-in modal reader with fullscreen and companion doc toggles (`[HOWTO.MD]` / `[README.MD]`).
  - Clicking non-document files copies their absolute path to the clipboard with toast confirmation.

