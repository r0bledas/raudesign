# /raudesign

**Sovereign High-Contrast Monochrome UI/UX Design System Specification & Antigravity Skill**

---

## Overview

`/raudesign` is an air-gapped, zero-bloat, high-contrast monochrome design system and Antigravity assistant skill. It enforces strict visual discipline and operational integrity across local-first web dashboards, launchers, and offline tools.

Authoritative specification: [`design.md`](./design.md)  
Antigravity skill: [`SKILL.md`](./SKILL.md)

---

## Core Principles

1. **Strict High-Contrast Monochrome**: Deep pitch blacks (`#000000`, `#0a0a0a`, `#141414`), crisp whites (`#ffffff`, `#f0f0f0`), and calibrated structural grays (`#333333`, `#555555`, `#888888`).
2. **Absolute Zero Emojis**: Emojis are strictly forbidden. All icons must be pure geometric SVG vector path elements that inherit `currentColor`.
3. **No Sluggish Transitions / No Animations on Interactive Structural Glyphs**: Interactive chevrons (`>`), status tags, and layout elements must NEVER animate, slide, bounce, or translate on hover.
4. **100% Air-Gapped & Offline**: Zero CDNs, zero cloud fonts, zero analytics, zero external network requests.
5. **Uniform Tag & Badge Sizing (The "Max-Width Alignment" Rule)**: When multiple tags or status badges appear within a list, group, or tree view (e.g. `[README]`, `[HOWTO]`, `[MANUAL]`, `[OPS]`), they must never have ragged, mismatched widths. All tags in that container must follow the same uniform width (matched to the widest tag item) with centered text alignment and `box-sizing: border-box`.
6. **Button Stability**: Fixed or minimum widths (`min-width: 78px; justify-content: center`) to prevent layout shifts.

---

## Installation & Usage in Antigravity

Clone or place into your Antigravity skills configuration directory:

```bash
# User Custom Skills Directory
cd %USERPROFILE%\.gemini\config\skills
git clone https://github.com/r0bledas/raudesign.git
```

Whenever building or refactoring UI components, Antigravity references `/raudesign` to guarantee design consistency.
