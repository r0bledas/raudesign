---
name: raudesign
description: Enforces the strict, high-contrast monochrome, air-gapped, zero-bloat UI/UX design specification defined in D:\dashboard\design.md. Use when designing, building, reviewing, or styling any dashboard, web interface, launcher, or UI component.
---

# /raudesign — Sovereign High-Contrast Monochrome UI/UX Design System

This skill enforces strict compliance with the authoritative design specification located at [`D:\dashboard\design.md`](file:///D:/dashboard/design.md). When working on any dashboard interface, HTML/CSS component, webview, or UI element, you MUST follow these non-negotiable principles.

---

## 1. Core Principles

1. **Strict High-Contrast Monochrome Palette**:
   - Deep pitch blacks: `#000000`, `#0a0a0a`, `#141414`.
   - Crisp whites: `#ffffff`, `#f0f0f0`.
   - Calibrated structural grays: `#333333`, `#444444`, `#555555`, `#888888`.
   - **Zero bright accent colors, gradients, or pastel fills** unless specifically representing functional raw binary/status telemetry.
   - Monospace font stack: `"Consolas", "Courier New", monospace`.

2. **Absolute Zero Emojis**:
   - Emojis (e.g. 🚀, 📁, 💻, ⚙️) are strictly forbidden across UI components, buttons, tabs, headers, dialogs, and toasts.
   - All visual glyphs must be pure geometric SVG vector path elements that inherit `currentColor` (`fill: currentColor` or `stroke: currentColor`).

3. **No Sluggish Transitions / No Animations on Interactive Structural Glyphs**:
   - Chevrons (`>`), status tags, and layout elements must NEVER animate, slide, bounce, or translate on hover.
   - Hover states must be immediate, crisp, and predictable (e.g. border color shift `#333` -> `#777` or text brightening `#888` -> `#fff`).

4. **100% Air-Gapped & Offline**:
   - Zero CDNs (no Google Fonts, FontAwesome, unpkg, cdnjs, Cloudflare).
   - Zero external tracking, analytics, or network requests.
   - All styles, scripts, SVG assets, and resources must be fully embedded or served locally from the local filesystem.

5. **Anti-Clutter Layout Hierarchy & Uniform Component Sizing**:
   - No redundant top metric counters, decorative summary banners, or assumed status badges.
   - Maximize information density above the fold with immediate search/filter capabilities.
   - Buttons must have fixed or minimum widths (`min-width: 78px; justify-content: center`) to prevent layout shifts.
   - **Uniform Tag & Badge Sizing (The "Max-Width Alignment" Rule)**: When multiple tags or status badges appear within a list, group, or tree view (e.g. `[README]`, `[HOWTO]`, `[MANUAL]`, `[OPS]`), they must never have ragged, mismatched widths. All tags in that container must follow the same uniform width (matched to the widest tag item, e.g. `min-width: 62px` or `min-width: 58px`) with centered text alignment and `box-sizing: border-box`.

---

## 2. Standard Monochrome Component Library

### Vector SVG Icons
Use clean, 16x16 wireframe or solid geometric SVGs:
```html
<!-- Burger Menu -->
<svg viewBox="0 0 16 16"><path d="M2 3h12M2 8h12M2 13h12" stroke="currentColor" stroke-width="2" stroke-linecap="square"/></svg>

<!-- Folder -->
<svg viewBox="0 0 16 16"><path d="M2 3h4l2 2h6v8H2z" fill="none" stroke="currentColor" stroke-width="1.5"/></svg>

<!-- File / Document -->
<svg viewBox="0 0 16 16"><path d="M3 2h7l4 4v8H3z" fill="none" stroke="currentColor" stroke-width="1.5"/><path d="M10 2v4h4" fill="none" stroke="currentColor" stroke-width="1.5"/></svg>

<!-- Terminal / CLI -->
<svg viewBox="0 0 16 16"><path d="M2 3l5 5-5 5M9 13h5" stroke="currentColor" stroke-width="2" stroke-linecap="round" fill="none"/></svg>
```

### Action Button Pattern
```css
.btn {
  background: #000000;
  color: #ffffff;
  border: 1px solid #666666;
  padding: 5px 10px;
  font-family: "Consolas", "Courier New", monospace;
  font-size: 11px;
  cursor: pointer;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  min-width: 78px;
  line-height: 1;
}
.btn:hover {
  background: #ffffff;
  color: #000000;
  border-color: #ffffff;
}
```

### Modal Overlay & Drawer Rules
- Modals and drawers use `#0c0c0c` with solid `1px` or `2px` borders in `#ffffff` or `#333333`.
- Backdrops must be deep blackout scrims (`rgba(0, 0, 0, 0.88)`).
- Always wire standard keyboard shortcuts (<kbd>Esc</kbd> to close, <kbd>F</kbd> for fullscreen, <kbd>/</kbd> to filter).

---

## 3. Reference
Whenever modifying or auditing UI code, reference the full specification at [`D:\dashboard\design.md`](file:///D:/dashboard/design.md).
