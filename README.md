# a11y-skills

Accessibility skills for AI coding agents — audit and fix accessibility issues in design tools and live web pages.

Built for **WCAG 2.2 AA** compliance with full **EN/AR bilingual and RTL support** — the only accessibility skill set in the ecosystem with Arabic-specific design and code rules.

Works with Claude Code, Cursor, Copilot, Codex, and any agent that supports the [open skills standard](https://skills.sh).

---

## Skills

### `accessibility-design`
Audit screens, components, and design systems in **Figma, Framer, or Webflow** before code handoff.

Covers: color contrast, typography, touch targets, focus indicators, heading hierarchy, annotation for developer handoff, and RTL/Arabic design rules.

### `accessibility-code`
Audit and fix accessibility issues in any **live URL or codebase** — HTML, React, Vue, Next.js, Svelte, Framer sites, Webflow exports, or plain HTML/CSS.

Stack-agnostic: analyzes the rendered DOM output, not the source framework.

---

## Install

```bash
npx skills add https://github.com/jllerena1/a11y-skills --skill accessibility-design
npx skills add https://github.com/jllerena1/a11y-skills --skill accessibility-code
```

---

## Usage

### Design audit (Figma, Framer, Webflow)
```
/audit          → Full scored audit of a screen or component
/audit-component → Single component audit
/annotate        → Generate handoff annotations for developers
/check-contrast  → Color contrast check only
/design-system   → Audit your token/color library
/audit-rtl       → EN/AR bilingual and RTL audit
```

### Code / production audit (any URL or file)
```
/audit <url>          → Full WCAG 2.2 AA audit with score
/fix <file>           → Targeted fixes, no rewrites
/audit-component      → Single component audit
/quick <url>          → Critical issues only, fast scan
/audit-rtl <url>      → EN/AR bilingual and RTL audit
```

---

## Report format

Every audit produces a scored report with prioritized issues:

```
═══════════════════════════════════════════════════
ACCESSIBILITY AUDIT: Homepage
Score: 74 / 100
═══════════════════════════════════════════════════

🔴 CRITICAL (2 issues)   -10 pts each
🟠 SERIOUS  (1 issue)    -5 pts each
🟡 MODERATE (3 issues)   -2 pts each
🔵 INFO     (2 notes)    no deduction

Quick wins:
  ✨ Add aria-label to icon buttons — ~2 min — +20 pts
  ✨ Fix email input label — ~5 min — +10 pts
═══════════════════════════════════════════════════
```

---

## RTL / Arabic support

Both skills include full EN/AR bilingual support:

- `dir="rtl"` and `lang="ar"` validation
- Arabic typography rules (IBM Plex Arabic, Cairo, Tajawal — line-height 1.8, letter-spacing 0)
- Layout mirroring checks (what mirrors, what doesn't)
- `<bdi>` and `unicode-bidi` isolation for mixed EN/AR content
- CSS logical properties (`margin-inline-start` vs `margin-left`)
- Icon mirroring (`transform: scaleX(-1)` for directional icons)
- ARIA labels in correct language per section
- Screen reader testing guidance for Arabic (VoiceOver, NVDA)

---

## Compatibility

Tested with Claude Code · Works with any agent supporting the open skills standard

---

## License

MIT
