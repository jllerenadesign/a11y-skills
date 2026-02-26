---
name: accessibility-code
description: >
  Audit and fix accessibility issues in live web pages or any codebase — HTML,
  React, Vue, Next.js, Svelte, Framer sites, Webflow exports, or plain HTML/CSS.
  Works on source code files OR live production URLs. Fully supports bilingual
  EN/AR pages and RTL layouts — checks dir attributes, ARIA bidi patterns,
  Arabic font rendering, and mirroring logic. Use when the user wants to check
  WCAG compliance of a website, review rendered HTML for a11y issues, fix ARIA
  or keyboard navigation problems, audit a URL that is live in production, or
  check Arabic/RTL accessibility. Triggers when the user shares a URL, a code
  file, or asks to "audit", "check accessibility", "fix a11y", "RTL", "Arabic",
  or "is this page accessible". Stack-agnostic: analyzes the rendered DOM output,
  not the source framework.
---

# Accessibility Code Auditor

Audit and fix accessibility issues in any live web page or codebase.
Works on HTML output — framework doesn't matter.

## Input types

- **URL** — live page in production (Framer, Webflow, Next.js, anything)
- **Code file(s)** — HTML, JSX, Vue, Svelte, or any template file
- **Snippet** — pasted component or block of markup

If given a URL, analyze the rendered HTML/DOM. If given source code, analyze
the markup as written. The rules are the same either way.

---

## Modes

- **`/audit <url or file>`** — Full WCAG 2.2 AA audit, scored report
- **`/fix <file>`** — Targeted fixes only, minimal changes (no rewrites)
- **`/audit-component <snippet>`** — Audit a single component or pattern
- **`/quick <url or file>`** — Critical issues only, fast scan
- **`/audit-rtl <url or file>`** — RTL/bilingual audit for EN/AR pages

If no mode is specified, infer from context. Default to `/audit`.
If Arabic text, `dir="rtl"`, or `lang="ar"` is detected at any point, automatically apply RTL rules from `references/rtl-code.md`.

---

## Phase 1: Scan

Read `references/wcag-code.md` before starting any audit.

Scan in priority order:

| Priority | Category | What to check |
|----------|----------|--------------|
| 🔴 Critical | Accessible names | Buttons, links, inputs without labels |
| 🔴 Critical | Keyboard access | Elements unreachable by Tab, missing Enter/Space |
| 🔴 Critical | Focus management | Modals without focus trap, focus not restored |
| 🟠 Serious | Semantics | Divs/spans used as buttons, missing landmarks |
| 🟠 Serious | Forms | Missing labels, no error association, no aria-invalid |
| 🟠 Serious | Focus visibility | Outline removed, no visible replacement |
| 🟠 Serious | RTL structure | Missing `dir="rtl"` on `<html>` or containers, broken layout |
| 🟡 Moderate | Announcements | Dynamic content without aria-live |
| 🟡 Moderate | Heading hierarchy | Skipped levels, multiple H1s |
| 🟡 Moderate | Images | Missing alt, wrong alt on functional images |
| 🟡 Moderate | Bidi text | Mixed EN/AR inline without `dir` isolation |
| 🔵 Info | Touch targets | Elements < 24×24px |
| 🔵 Info | Motion | No prefers-reduced-motion handling |
| 🔵 Info | Arabic font | System fallback used instead of proper Arabic typeface |

---

## Phase 2: Generate report

Always use this format:

```
═══════════════════════════════════════════════════
ACCESSIBILITY AUDIT: [URL or filename]
Stack: [detected or "unknown"] · Level: WCAG 2.2 AA
═══════════════════════════════════════════════════

CRITICAL (X issues)
───────────────────
[A11Y] Icon button — navigation toggle
  Issue: No accessible name. Screen readers announce "button" with no context.
  Location: <header> → <nav> → first button
  Current:  <button><svg>...</svg></button>
  Fix:      <button aria-label="Open navigation menu"><svg aria-hidden="true">...</svg></button>
  WCAG:     4.1.2 Name, Role, Value

SERIOUS (X issues)
──────────────────
[A11Y] Form input — email field
  Issue: Input has no associated label. Placeholder is not a label.
  Location: <form#newsletter>
  Current:  <input type="email" placeholder="Enter your email">
  Fix:      <label for="email">Email address</label>
            <input type="email" id="email" placeholder="Enter your email">
  WCAG:     1.3.1 Info and Relationships

MODERATE (X issues)
───────────────────
[A11Y] Toast notification
  Issue: Success message injected into DOM but not announced to screen readers.
  Location: <div class="toast">
  Current:  <div class="toast">Item saved!</div>
  Fix:      <div class="toast" role="status" aria-live="polite">Item saved!</div>
  WCAG:     4.1.3 Status Messages

INFO (X notes)
──────────────
[A11Y] Heading hierarchy
  Note: Page jumps from H1 to H3 in the features section.
  Suggestion: Add H2 "Features" or change H3s to H2s.
  WCAG:     1.3.1 Info and Relationships

═══════════════════════════════════════════════════
SCORE: XX / 100
───────────────────────────────────────────────────
Total issues: X
  🔴 Critical:  X  (-10 pts each)
  🟠 Serious:   X  (-5 pts each)
  🟡 Moderate:  X  (-2 pts each)
  🔵 Info:      X  (no deduction)

Quick wins (easy + high impact):
  ✨ [specific fix] — ~X min
  ✨ [specific fix] — ~X min
═══════════════════════════════════════════════════
```

---

## Phase 3: Fixes (if `/fix` mode)

Read `references/wcag-code.md` for correct patterns per issue type.

Rules for fixes:
- Prefer **native HTML** over ARIA when possible
- Make **minimal, targeted changes** — don't refactor unrelated code
- Don't add ARIA where native semantics already solve the problem
- Quote the exact current code, then show the fixed version
- For complex widgets (menu, combobox, dialog), use established patterns from WAI-ARIA APG — don't invent custom behavior

Fix output format:
```
FIX: [Issue title]
File: [filename or "inline"]

BEFORE:
[exact current code]

AFTER:
[fixed code]

Why: [one sentence explaining the change]
```

---

## Output behavior

- Always show exact code snippets — never vague descriptions
- Explain the user impact in plain language ("Screen readers will announce X")
- For `/fix` mode, never rewrite more than needed
- For URL audits, note that automated analysis may miss visual-only issues
  (color contrast, small text) — recommend manual verification with browser tools

---

## Reference files

- `references/wcag-code.md` — ARIA patterns, semantic HTML, keyboard rules
- `references/audit-report.md` — Extended report template and scoring reference
- `references/rtl-code.md` — RTL/bilingual rules: dir attribute, bidi ARIA, Arabic font stacks, mirroring
