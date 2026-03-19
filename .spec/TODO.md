# Minimal Elegant Subtitle Styling - Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Restyle jimakuChan subtitle overlay with Oxanium + Noto Sans JP fonts, subtle shadow instead of stroke, optimized for OBS Browser Source at 1920px fixed width.

**Architecture:** Change default styling values in index.html (settings UI) and main.html (display). Add Oxanium Google Font import. Replace stroke defaults with text-shadow on -fg layer. Set fixed-width container for consistent OBS rendering.

**Tech Stack:** HTML, CSS, vanilla JS (no build system)

---

### Task 1: Add Oxanium Google Font import to both HTML files

**Files:**
- Modify: `main.html:6-14` (Google Font imports)
- Modify: `index.html:6-14` (Google Font imports)

- [ ] **Step 1:** Add Oxanium import line after Sawarabi Gothic import in both files:
```html
<link href="https://fonts.googleapis.com/css2?family=Oxanium:wght@400;600&display=swap" rel="stylesheet">
```

---

### Task 2: Add Oxanium option to font selectors in index.html

**Files:**
- Modify: `index.html:327-356` (speech_text_font_selector)
- Modify: `index.html:390+` (trans_text_font_selector)
- Modify: `index.html:452+` (trans_text2_font_selector)
- Modify: `index.html:514+` (trans_text3_font_selector)

- [ ] **Step 1:** Add `<option value="Oxanium">Oxanium</option>` to all 4 font selector dropdowns, after the M PLUS Rounded 1c option.

---

### Task 3: Change default styling — stroke width to 0, sizes, and add text-shadow

**Files:**
- Modify: `index.html:218` (size1 range: 25→30)
- Modify: `index.html:226` (st_width1 range: 6→0)
- Modify: `index.html:239` (size2 range: 25→24)
- Modify: `index.html:247` (st_width2 range: 6→0)
- Modify: `index.html:268` (st_width3 range: 6→0)
- Modify: `index.html:289` (st_width4 range: 6→0)
- Modify: `main.html:134-141` (.stroke-single-fg CSS)

- [ ] **Step 1:** Change `value="6"` to `value="0"` on all 4 `st_width` range inputs (lines 226, 247, 268, 289)
- [ ] **Step 2:** Change size1 default `value="25"` → `value="30"`, size2 default `value="25"` → `value="24"`
- [ ] **Step 3:** Add `text-shadow: 0 1px 4px rgba(0,0,0,0.7);` to `.stroke-single-fg` CSS class in main.html

---

### Task 4: Change default language and font selections

**Files:**
- Modify: `index.html:302` (recog: ja → en-US selected)
- Modify: `index.html:330` (font selector: M PLUS Rounded → Oxanium selected)
- Modify: `index.html:363` (trans: none → ja selected)

- [ ] **Step 1:** Recognition language: move `selected` from `value="ja"` to `value="en-US"`
- [ ] **Step 2:** Font selector: move `selected` from M PLUS Rounded 1c to Oxanium
- [ ] **Step 3:** Translation target 1: move `selected` from `value="none"` to `value="ja"`

---

### Task 5: Update preset defaults and URL builder fallbacks

**Files:**
- Modify: `index.html:2282-2327` (default preset)
- Modify: `index.html:1100-1106` (URL builder fallbacks)

- [ ] **Step 1:** Update "default" preset: recog→"en-US", size1→"30", weight1→"600", st_width1→"0", trans→"ja", size2→"24", weight2→"400", st_width2→"0", speech_text_font_selector→"Oxanium", trans_text_font_selector→"Noto Sans JP"
- [ ] **Step 2:** Change URL builder fallback st_width values from `"6"` to `"0"` (lines 1100-1106)

---

### Task 6: Fixed-width container for OBS Browser Source

**Files:**
- Modify: `main.html:49-61` (body CSS)

- [ ] **Step 1:** Set `width: 1920px; min-width: 1920px;` on `body` in main.html CSS (transparent background is already default)

---

### Task 7: Manual testing (user performs)

- [ ] User starts server, opens index.html
- [ ] Verify: Oxanium in font dropdown, selected by default
- [ ] Verify: English recognition, Japanese translation target
- [ ] Verify: Stroke width slider at 0
- [ ] Verify: OBS Browser Source at 1920x1080 — fixed width, subtle shadow, correct fonts
- [ ] Verify: Speech recognition and translation display correctly

---

### Task 8: Commit (after testing passes)

- [ ] Commit: `git add main.html index.html && git commit -m "feat: minimal elegant styling with Oxanium/Noto Sans JP, shadow, OBS fixed width"`

---

## 完了済み
- [x] 初期セットアップ
- [x] make_project スキルによるプロジェクト構成整備
