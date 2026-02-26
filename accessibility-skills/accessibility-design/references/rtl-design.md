# RTL & Bilingual Design Rules (EN/AR)

Reference for `accessibility-design` skill — `/audit-rtl` mode and any design
with Arabic content. Load when the design includes Arabic text, RTL layouts,
or bilingual EN/AR screens.

---

## Core principle

Arabic is read right-to-left. A bilingual design is not just translated text —
it requires a mirrored layout, adapted components, and Arabic-specific typography.
Accessibility in RTL means both WCAG compliance AND correct directional behavior.

---

## 1. Layout mirroring

The entire layout flips horizontally for AR. What was on the left is now on
the right, and vice versa.

### What always mirrors ✅
| LTR (EN) | RTL (AR) |
|----------|----------|
| Content flows left → right | Content flows right → left |
| Navigation starts top-left | Navigation starts top-right |
| Primary action (CTA) is right-aligned | Primary action is left-aligned |
| Back arrow points ← | Back arrow points → |
| Progress bar fills left → right | Progress bar fills right → left |
| Breadcrumbs: Home > Section > Page | Breadcrumbs: صفحة > قسم > الرئيسية |
| Sidebar on the left | Sidebar on the right |
| Form labels left of input | Form labels right of input |
| Icons that indicate direction | Mirrored (back, forward, arrow, chevron) |

### What does NOT mirror ❌
| Element | Reason |
|---------|--------|
| Numbers (prices, dates, phone) | Universal LTR even in AR context |
| Latin logos and brand marks | Language-independent |
| Playback controls (▶ ⏸ ⏭) | Universal media convention |
| Checkmarks ✓ | Universal meaning |
| Warning/info icons (⚠ ℹ) | Universal symbols |
| Maps and geographic content | Direction-independent |
| Charts (if values are universal) | Axis labels may flip, but chart type doesn't |

---

## 2. Arabic typography

Arabic requires different type treatment than Latin — applying Latin spacing
rules to Arabic text breaks legibility and accessibility.

### Font selection
- Use a typeface specifically designed for Arabic — not a Latin font with Unicode Arabic fallback
- Recommended open-source Arabic fonts: **Noto Naskh Arabic**, **IBM Plex Arabic**, **Cairo**, **Tajawal**, **Almarai**
- For design systems: define an Arabic font stack separately from Latin
- Avoid: Times New Roman Arabic, system fallbacks, or fonts not designed for Arabic

### Size
- Arabic letters are more compact — minimum 14px for body text (vs 12px for Latin)
- Headlines: 18px+ for comfortable reading
- Never use the same minimum size rules as Latin text directly

### Line height
- Arabic needs more vertical space than Latin
- Minimum 1.8× font size (vs 1.5× for Latin)
- Ascenders and descenders in Arabic script require breathing room

### Letter spacing
- **Never** increase `letter-spacing` for Arabic — it breaks the connected letterforms
- `letter-spacing: 0` is the correct value for Arabic
- Word spacing (`word-spacing`) may be increased slightly if needed

### Text alignment
- Arabic body text: right-aligned (not justified as default)
- Justified Arabic text is acceptable only with proper Arabic hyphenation support
- Mixed EN/AR: align to the dominant language direction

### Numerals in Arabic context
- Eastern Arabic numerals (٠١٢٣٤٥٦٧٨٩) are used in many KSA/Gulf contexts
- Western Arabic numerals (0123456789) are also widely accepted
- Be consistent — don't mix both styles in the same design
- Dates, phone numbers, and prices: follow the platform convention

---

## 3. Component-level checks

### Navigation
- Hamburger / menu icon: same, but positioned top-right in AR
- Active indicator (underline, highlight): mirrors to right side of tab in AR
- Dropdown arrows: point down regardless of direction — no mirroring needed
- Back/forward navigation buttons: arrows must mirror

### Forms
- Labels: right-aligned, above or to the right of inputs in AR
- Placeholder text: right-aligned, in Arabic
- Error messages: right-aligned, below field in AR
- Checkbox / radio: label appears to the LEFT of the control in AR (not right)
- Input text direction: `dir="auto"` so users can type either language

### Cards & content blocks
- Image + text: image may move from left to right or vice versa depending on composition
- Icon + label: icon moves to the opposite side in AR
- Timestamps and meta info: right-aligned in AR

### Buttons and CTAs
- Text direction within button: mirrors naturally with `dir="rtl"`
- Icon position inside button: icon that was on the left of text moves to the right in AR
- Icon direction: directional icons (→, ←, ▶) must mirror; decorative icons do not

### Modals and overlays
- Title alignment: right in AR
- Close button (×): top-left in AR (was top-right in EN)
- Action buttons: order stays primary/secondary, but position mirrors

### Tables
- Column order: mirrors in AR (first column is rightmost)
- Sort arrows: mirror for RTL

---

## 4. Bilingual-specific design patterns

### Language toggle
- Must be clearly accessible and keyboard-reachable
- Label the toggle in both languages: "English / عربي"
- Toggle should be in a consistent, expected location (header)

### Mixed-language inline text
- When Arabic and English appear in the same line, visual rendering can break
- Design should account for bidi text reordering (e.g., a price "SAR 250" inside an Arabic sentence)
- Annotate bidi isolation zones for developers

### Icons with text labels
- If icon meaning is directional (next, back, expand), design both LTR and RTL versions
- If icon is illustrative (a house, a user), one version is fine

### RTL-specific spacing
- Add extra padding on the right side of containers in AR (was left in EN)
- Scroll indicators and overflow cues mirror

---

## 5. Annotation checklist for RTL handoff

Add these annotations when handing off bilingual designs:

- [ ] Indicate which components need mirrored variants
- [ ] Mark icons as "mirror" or "no mirror"
- [ ] Note font family for Arabic text separately from Latin
- [ ] Document text direction (`dir="rtl"` or `dir="auto"`) for each input
- [ ] Label bidi isolation zones for mixed EN/AR inline content
- [ ] Specify numeral style (Eastern Arabic vs Western Arabic)
- [ ] Note any components that should NOT mirror (media controls, logos)
- [ ] Mark the language toggle component and its behavior
- [ ] Specify Arabic line-height values (1.8× minimum)

---

## 6. Common RTL design mistakes to flag

| Mistake | What to report |
|---------|---------------|
| Same design used for both EN and AR without mirroring | 🔴 Critical — layout will be wrong for AR users |
| Latin font applied to Arabic text | 🟠 Serious — legibility failure |
| `letter-spacing` applied to Arabic | 🟠 Serious — breaks Arabic letterforms |
| Directional icon not mirrored (e.g., back arrow) | 🟠 Serious — confusing navigation |
| Error message left-aligned in AR | 🟡 Moderate — visual inconsistency |
| Checkbox label on wrong side | 🟡 Moderate — unexpected layout |
| Line-height too tight for Arabic | 🟡 Moderate — readability issue |
| Mixed numeral styles | 🔵 Info — inconsistency |
| No `dir` annotation on mixed-content inputs | 🔵 Info — dev may assume wrong direction |
