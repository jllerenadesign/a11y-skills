# WCAG 2.2 Rules for Visual Design

Reference for `accessibility-design` skill. Covers design-phase rules only —
no code implementation. Load this file when running a full audit.

---

## 1. Color Contrast (WCAG 1.4.3, 1.4.6, 1.4.11)

### Text contrast requirements
| Text type | AA minimum | AAA enhanced |
|-----------|-----------|-------------|
| Normal text (< 18pt / < 14pt bold) | 4.5:1 | 7:1 |
| Large text (≥ 18pt or ≥ 14pt bold) | 3:1 | 4.5:1 |
| Disabled text | Exempt | Exempt |
| Logo / decorative | Exempt | Exempt |

### UI component contrast (1.4.11)
- Input borders, focus rings, button outlines: **3:1** against adjacent color
- Icons that convey meaning: **3:1** against background
- Charts, graphs, data viz: **3:1** between adjacent segments

### Tips for design tools
- In Figma: use Stark plugin or A11y Color Contrast Checker plugin
- In Framer: use browser DevTools or Stark browser extension
- Use Eightshapes Contrast Grid to validate entire color palette

---

## 2. Typography (WCAG 1.4.4, 1.4.8, 1.4.12)

- **Minimum font size**: 12px for body text; 11px absolute minimum for any text
- **Line height**: 1.5× font size minimum for body text
- **Letter spacing**: 0.12em minimum for body text
- **Paragraph spacing**: 2× font size minimum between paragraphs
- **Text resize**: Design must not break at 200% zoom (1.4.4)
- **Text spacing**: Content must reflow without loss of functionality when spacing
  is increased (1.4.12)

---

## 3. Touch Targets (WCAG 2.5.5, 2.5.8)

| Standard | Minimum size | Recommendation |
|----------|-------------|----------------|
| WCAG 2.2 AA (2.5.8) | 24×24 CSS px | Prefer larger |
| WCAG 2.2 AAA (2.5.5) | 44×44 CSS px | Best practice |
| Apple HIG | — | 44×44 pt |
| Google Material | — | 48×48 dp |

- Spacing exception: if target < 24px, ensure 10px clearance around it
- Apply to: buttons, links, checkboxes, radio buttons, toggles, icons with actions

---

## 4. Focus Indicators (WCAG 2.4.7, 2.4.11, 2.4.12, 2.4.13)

Every interactive element must have a visible focus state designed explicitly.

### Minimum requirements (WCAG 2.4.13 — AA in 2.2)
- Focus indicator must have **3:1 contrast** against adjacent unfocused colors
- Focus indicator must enclose the component (2px perimeter minimum)
- Focused element must not be entirely hidden by other content (2.4.11)

### Recommended approach
- 2px solid outline + 2px offset is the most accessible pattern
- Use `box-shadow` alternative when outline clips (e.g., rounded corners)
- Define focus styles for every interactive component in your design system
- Never rely on browser default alone — design it explicitly

---

## 5. Color as the Only Visual Cue (WCAG 1.4.1)

Color must not be the **only** way to convey information. Always pair with:

| Signal | Color alone ❌ | Color + secondary cue ✅ |
|--------|--------------|------------------------|
| Error state | Red border | Red border + error icon + error text |
| Required field | Red asterisk only | Red asterisk + "(required)" label |
| Selected state | Blue fill only | Blue fill + checkmark or bold text |
| Link in text | Blue color | Blue + underline |
| Chart legend | Color swatches | Color + pattern or label |

---

## 6. Images & Icons (WCAG 1.1.1)

### Informative images
- Must have descriptive alt text
- Alt text describes the **purpose**, not the appearance ("Chart showing Q3 growth of 24%", not "Bar chart")

### Decorative images
- Alt text = empty string `alt=""`
- Mark as decorative in design annotations

### Icon-only buttons
- Must have accessible label annotated for dev handoff
- Use tooltip label = accessible name (e.g., "Search", "Close dialog")
- Never rely on placeholder or title attribute alone

### Functional images (logos, CTAs)
- Alt text = the action or destination ("Go to homepage")

---

## 7. Heading Hierarchy (WCAG 1.3.1, 2.4.6)

- H1: Page or screen title (one per screen)
- H2: Major sections
- H3: Subsections within H2
- Never skip levels (H1 → H3 is invalid)
- Heading levels are semantic, not styling choices — annotate them separately from visual type styles

---

## 8. Forms & Inputs (WCAG 1.3.1, 3.3.1, 3.3.2)

- Every input must have a **visible label** (not just placeholder)
- Placeholder text ≠ label — it disappears and often fails contrast
- Error messages must be:
  - Specific ("Password must be at least 8 characters")
  - Linked to the field visually and semantically
  - Not reliant on color alone
- Required fields: mark visually AND with "(required)" text or accessible equivalent
- Group related inputs with fieldset + legend annotation

---

## 9. Motion & Animation (WCAG 2.3.3)

- Any animation > 5 seconds must have a pause/stop control
- Design a `prefers-reduced-motion` variant for essential animations
- Avoid content that flashes more than 3 times per second (seizure risk)
- Note reduced-motion behavior in design annotations

---

## 10. Layout & Zoom (WCAG 1.3.4, 1.4.10)

- Content must reflow at 320px width without horizontal scrolling (1.4.10)
- Avoid fixed-height containers that clip content when text is enlarged
- Design must work in both portrait and landscape (1.3.4)
- Don't restrict viewport zoom in designs intended for web
