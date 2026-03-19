# SPEC - Minimal Elegant Subtitle Styling

## Overview
Restyle jimakuChan's subtitle display for a minimal, elegant stream overlay. Oxanium font for English recognition text, Noto Sans JP for Japanese translation, subtle shadow instead of thick stroke outlines.

## Functional Requirements

### Font Changes
- Recognition text (English): **Oxanium** (Google Font, geometric/tech)
  - Import via: `https://fonts.googleapis.com/css2?family=Oxanium:wght@400;600&display=swap`
  - Add as font option in index.html font selectors
- Translation text (Japanese): **Noto Sans JP** (already imported in main.html)
  - Set as default font-family for translation line 1

### Text Styling — Shadow Replaces Stroke
- **Keep the 3-layer system (bg/fg/imb) for backward compatibility**
- Set default `st_width` (stroke width) to `0` in index.html for all 4 text lines (recognition + 3 translations) — currently defaults to `6`
- Apply `text-shadow: 0 1px 4px rgba(0,0,0,0.7)` to the `-fg` layer only
- The `-bg` layer remains but renders no stroke (width=0), preserving the system for users who want to re-enable stroke via settings
- White text color (`#FFFFFF`) for both lines

### Text Sizing (in pt, matching codebase convention)
- Recognition text: **30pt**, font-weight 600 (semi-bold)
- Translation text: **24pt**, font-weight 400 (regular)

### Layout
- Bottom-aligned, centered
- Recognition text on top, translation below
- Minimal spacing between lines
- Translation targets 2 and 3: set to `none` by default (divs remain in DOM but render empty)

### Default Configuration
- Recognition language: English (`en-US`) — only affects fresh installs/cleared storage; existing localStorage overrides
- Translation target 1: Japanese (`ja`)
- Translation targets 2 and 3: `none`
- Note: Translation requires GAS key or Chrome translator to be configured — this is expected behavior, not a bug

### OBS Browser Source Optimization
- Set subtitle container to fixed width (`1920px`) so text layout is consistent regardless of viewport
- Ensure transparent background works cleanly (no background color on body)
- Container should be bottom-aligned and centered within the fixed frame

## Non-Functional Requirements
- All existing customization still works (user can change fonts/colors/sizes/stroke via settings)
- Changes are to **default values only** — URL parameters and settings overrides still apply
- No breaking changes to the URL parameter system
- Font loading uses `display=swap` to prevent flash of invisible text during stream

## Technical Approach
1. Add Oxanium Google Font import to main.html (css2 format with wght@400;600)
2. Add Oxanium option to font selectors in index.html
3. Change default `st_width` values from `6` to `0` in index.html (lines ~226, 247, 268, 289)
4. Add `text-shadow` CSS to `-fg` class elements in main.html
5. Update default font-family for recognition text → Oxanium, translation 1 → Noto Sans JP
6. Update default size values to 30pt (recognition) and 24pt (translation)
7. Change default recognition language select to `en-US`
8. Change default translation target 1 to `ja`
