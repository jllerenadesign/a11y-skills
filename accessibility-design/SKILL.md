---
name: accessibility-design
description: >
  Audit and improve accessibility in design tools — Figma, Framer, or Webflow.
  Use when reviewing screens, components, flows, or design systems for
  accessibility issues before handoff or launch. Covers WCAG 2.2 AA compliance
  for visual design: color contrast, typography, spacing, touch targets, focus
  indicators, and annotation for developer handoff. Fully supports bilingual
  EN/AR designs and RTL layouts — checks mirroring, Arabic typography, and
  bidirectional component logic. Triggers when the user mentions Figma, Framer,
  Webflow, design review, screen audit, component accessibility, design system
  a11y, Arabic, RTL, bilingual, or asks to check if a design is accessible.
  Also use when the user shares a screenshot, frame, or component and wants
  accessibility feedback.
---

# Accessibility Design Auditor

Audit and improve accessibility in design tools (Figma, Framer, Webflow) before
code handoff or production. Produces a scored report with prioritized fixes.

## Modes

Detect which mode applies from user context:

- **`/audit`** — Full accessibility audit of a screen, frame, or component
- **`/audit-component`** — Focused audit of a single component or pattern
- **`/annotate`** — Generate handoff annotations for developers
- **`/check-contrast`** — Color contrast check only
- **`/design-system`** — Audit tokens, color palette, and component library rules
- **`/audit-rtl`** — RTL/bilingual audit for EN/AR designs (Figma, Framer, Webflow)

If the user doesn't specify a mode, infer it from context and confirm.
If bilingual or Arabic content is detected at any point, automatically apply RTL rules.

---

## Phase 1: Understand the context

Before auditing, clarify:

1. **Tool** — Figma, Framer, or Webflow? (affects annotation guidance)
2. **Scope** — Single screen, component, or full flow?
3. **Target level** — WCAG AA (default) or AAA?
4. **Bilingual?** — Does the design support Arabic / RTL?
   - If yes → read `references/rtl-design.md` and apply RTL checks alongside standard audit

If a screenshot or design file is shared, start the audit immediately.

---

## Phase 2: Run the audit

Check issues in priority order. Read `references/wcag-design.md` for detailed
rules per category.

### Priority table

| Priority | Category | Examples |
|----------|----------|---------|
| 🔴 Critical | Color contrast | Text < 4.5:1, UI components < 3:1 |
| 🔴 Critical | Missing labels | Unlabeled icons, images with no alt |
| 🔴 Critical | Touch targets | Interactive elements < 24×24px |
| 🟠 Serious | Focus indicators | No visible focus state on interactive elements |
| 🟠 Serious | Typography | Font < 12px, no resize support, poor line-height |
| 🟠 Serious | RTL mirroring | Icons/arrows not mirrored, layout not flipped for AR |
| 🟡 Moderate | Spacing & layout | Content too dense, no breathing room for zoom |
| 🟡 Moderate | Color only | Info conveyed by color alone, no secondary cue |
| 🟡 Moderate | Arabic typography | Wrong Arabic font, incorrect line-height or letter-spacing |
| 🔵 Info | Heading hierarchy | Skipped heading levels, decorative text styled as heading |
| 🔵 Info | Motion & animation | No reduced-motion consideration |
| 🔵 Info | Bidi edge cases | Mixed EN/AR inline text, number direction |

---

## Phase 3: Generate the report

Always use this exact format:

```
═══════════════════════════════════════════════════
ACCESSIBILITY DESIGN AUDIT: [Screen / Component Name]
Tool: [Figma / Framer / Webflow] · Level: WCAG 2.2 AA
═══════════════════════════════════════════════════

CRITICAL (X issues)
───────────────────
[A11Y-D] Color Contrast — Button label "Submit"
  Issue: Text contrast 2.8:1 — fails AA (requires 4.5:1)
  Current: #767676 on #FFFFFF
  Fix: Change text to #595959 or darker
  WCAG: 1.4.3 Contrast (Minimum)

SERIOUS (X issues)
──────────────────
[A11Y-D] Focus Indicator — Icon button "Search"
  Issue: No visible focus state defined in design
  Fix: Add 2px solid focus ring, min 3:1 contrast vs background
  WCAG: 2.4.7 Focus Visible / 2.4.11 Focus Not Obscured

MODERATE (X issues)
───────────────────
[A11Y-D] Color Only — Error state on form field
  Issue: Error indicated by red border only, no icon or text
  Fix: Add error icon + inline error message below field
  WCAG: 1.4.1 Use of Color

INFO (X notes)
──────────────
[A11Y-D] Heading hierarchy — Dashboard layout
  Note: "Card title" uses H3 styling but no H2 exists on screen
  Suggestion: Define heading levels in design annotations

═══════════════════════════════════════════════════
SCORE: XX / 100
───────────────────────────────────────────────────
Total issues: X
  🔴 Critical:  X  (-10 pts each)
  🟠 Serious:   X  (-5 pts each)
  🟡 Moderate:  X  (-2 pts each)
  🔵 Info:      X  (no deduction)

Quick wins (easy + high impact):
  ✨ [Fix] — estimated effort: X min
  ✨ [Fix] — estimated effort: X min
═══════════════════════════════════════════════════
```

Score calculation: start at 100, deduct per issue. Minimum 0.

---

## Phase 4: Annotations for handoff (if `/annotate` mode)

Read `references/figma-annotations.md` for tool-specific annotation patterns.

Key annotations to include:
- **Alt text** — for every image, icon, illustration
- **Focus order** — numbered sequence for keyboard navigation
- **ARIA roles** — for custom components (tabs, modals, accordions)
- **Error states** — what to announce to screen readers
- **Heading levels** — H1 → H2 → H3 hierarchy map

---

## Output behavior

- Be specific: quote the exact element and location
- Explain WHY each issue matters (1 sentence max)
- Give a concrete, actionable fix — not just "improve contrast"
- For design system audits, flag systemic issues separately from one-off issues
- Prefer minimal targeted fixes — don't suggest redesigning whole screens

---

## Reference files

- `references/wcag-design.md` — Full WCAG 2.2 rules for visual design
- `references/figma-annotations.md` — Annotation patterns for Figma, Framer, Webflow handoff
- `references/rtl-design.md` — RTL/bilingual rules for EN/AR designs (Arabic typography, mirroring, bidi)
