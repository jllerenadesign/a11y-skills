# Annotation Guide for Design Handoff

Reference for `accessibility-design` skill `/annotate` mode.
Covers Figma, Framer, and Webflow annotation patterns.

---

## Why annotate for accessibility

Annotations bridge the gap between visual design and accessible implementation.
Without them, developers make assumptions — and assumptions cause WCAG failures.
Annotate anything that isn't obvious from the visual design alone.

---

## What to always annotate

### 1. Alt text for images and icons
Annotate every image, illustration, and icon with one of:
- **Descriptive alt** — `alt="Woman holding coffee mug, smiling"`
- **Decorative** — `alt="" (decorative)`
- **Functional** — `alt="Search"` (when icon = button)
- **Complex** — `alt="Bar chart — see data table below for full description"`

### 2. Heading levels
Map visual type styles to semantic heading levels.
Example annotation: `"Section title" → H2`, `"Card title" → H3`

Do this even if the visual hierarchy seems obvious — developers need the map.

### 3. Focus order
Number the tab sequence for each screen, especially when:
- Visual layout differs from reading order (multi-column, absolute positioning)
- Custom components exist (tabs, carousels, menus)
- Modals or overlays are present

### 4. Interactive states
For every interactive component, design and annotate:
- Default
- Hover
- Focus (keyboard)
- Active / pressed
- Disabled
- Error (for inputs)
- Loading (if async)

### 5. Screen reader text (sr-only)
Annotate text that exists for assistive technology but is visually hidden.
Examples:
- Icon button label: `sr-only: "Close dialog"`
- Table context: `sr-only: "Sorted by date, ascending"`
- Progress: `sr-only: "Step 2 of 4"`

### 6. ARIA roles for custom components
Annotate the intended ARIA pattern for complex interactive components:

| Component | Annotation |
|-----------|-----------|
| Tabs | `role="tablist"`, `role="tab"`, `role="tabpanel"` |
| Accordion | `aria-expanded`, `aria-controls` |
| Modal | `role="dialog"`, `aria-modal="true"`, `aria-labelledby` |
| Tooltip | `role="tooltip"`, trigger `aria-describedby` |
| Combobox | `role="combobox"`, `aria-autocomplete` |
| Alert | `role="alert"` or `aria-live="assertive"` |
| Loading | `aria-busy="true"`, `aria-live="polite"` |

---

## Tool-specific annotation tips

### Figma
- Use A11y Annotation Kit (by Twitter/X or Indeed) from Community
- Microsoft's A11y Focus Order plugin for numbering tab sequence
- Add a dedicated "A11y" annotation layer per screen
- Use Stark plugin to validate contrast directly in Figma
- Consider a separate "Accessibility specs" page in your file

### Framer
- Add text layers marked as annotations (use a distinct color, e.g., purple)
- Use Framer's built-in accessibility panel for alt text on images
- For complex components, add a comment block with ARIA role info
- Test live preview with keyboard navigation before handoff

### Webflow
- Use the Accessibility panel in Designer for alt text and ARIA labels
- Add custom attributes via the element panel for ARIA roles
- Use Webflow's built-in label associations for form inputs
- Document heading levels in page settings or design notes

---

## Annotation format template

Use a consistent format for all accessibility annotations:

```
[A11Y] Element name
  Type: alt / heading / focus-order / sr-only / aria / state
  Value: "[exact text or instruction]"
  Notes: [optional context for developer]
```

Examples:

```
[A11Y] Hero image
  Type: alt
  Value: "Aerial view of Riyadh skyline at dusk"

[A11Y] Nav toggle button
  Type: sr-only + aria
  Value: sr-only="Open navigation menu"
         aria-expanded="false" (updates to "true" when open)
         aria-controls="main-nav"

[A11Y] Search results count
  Type: aria
  Value: aria-live="polite" aria-atomic="true"
  Notes: Announce when results update: "24 results found"

[A11Y] Tab sequence — Settings page
  Type: focus-order
  Value: 1→ Back button, 2→ Page title (H1), 3→ Tab: Profile,
         4→ Tab: Security, 5→ Tab: Notifications, 6→ First input in panel
```

---

## Checklist before handoff

- [ ] All images have alt text or marked decorative
- [ ] All icon-only buttons have accessible labels
- [ ] Heading hierarchy is documented (H1 → H2 → H3)
- [ ] Focus order is annotated for each screen
- [ ] All interactive states are designed (especially focus)
- [ ] Error states include text, not just color
- [ ] Custom components have ARIA role annotations
- [ ] Any sr-only text is documented
- [ ] Color contrast has been validated (not just visually checked)
- [ ] Touch targets meet 24×24px minimum (44×44px recommended)
