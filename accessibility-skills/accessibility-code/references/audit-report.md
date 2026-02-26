# Audit Report Template & Scoring Reference

Extended reference for `accessibility-code` skill report generation.

---

## Scoring system

| Issue severity | Points deducted | When to use |
|---------------|----------------|-------------|
| 🔴 Critical | -10 pts | Blocks access entirely for some users |
| 🟠 Serious | -5 pts | Significantly impairs usability |
| 🟡 Moderate | -2 pts | Reduces experience, has workaround |
| 🔵 Info | 0 pts | Best practice, not a failure |

Starting score: 100. Minimum: 0.

### Score interpretation
| Score | Status |
|-------|--------|
| 90–100 | ✅ Excellent — minor improvements only |
| 75–89 | 🟡 Good — a few issues to address |
| 60–74 | 🟠 Needs work — several significant issues |
| 40–59 | 🔴 Poor — blocking issues present |
| < 40 | ❌ Critical — not accessible for many users |

---

## Issue severity decision guide

**Critical** — use when:
- Screen reader users cannot identify an element's purpose
- Keyboard-only users cannot reach or activate an element
- Focus is trapped and users cannot escape
- Form cannot be completed without a mouse

**Serious** — use when:
- Element is accessible but confusing or unnecessarily hard to use
- Focus indicator is absent (keyboard users see no current position)
- Error messages exist but aren't associated with the field
- Custom component lacks required ARIA attributes

**Moderate** — use when:
- Dynamic content updates without announcement (workaround: refresh)
- Heading hierarchy is inconsistent but content is still navigable
- Touch targets are small but still tappable

**Info** — use when:
- Pattern works but a better practice exists
- Semantic improvement would help but current is not a failure
- Redundant ARIA that doesn't cause harm but adds noise

---

## Full report template

```
═══════════════════════════════════════════════════
ACCESSIBILITY AUDIT
Target: [URL or filename]
Date: [YYYY-MM-DD]
Scope: [Full page / Component / Snippet]
Standard: WCAG 2.2 Level AA
═══════════════════════════════════════════════════

EXECUTIVE SUMMARY
─────────────────
Score: XX / 100
Status: [Excellent / Good / Needs work / Poor / Critical]

[1–2 sentence plain-language summary of the main issues]

CRITICAL ISSUES — Must fix (X found)
─────────────────────────────────────
[One block per issue:]

[A11Y-X] [Short issue title]
  Impact:   [Who is affected and how]
  Location: [element selector, file:line, or URL section]
  Current:
    [exact code snippet]
  Fix:
    [corrected code snippet]
  WCAG: [criterion number + name]

[Repeat for each critical issue]

SERIOUS ISSUES — Should fix (X found)
──────────────────────────────────────
[Same format]

MODERATE ISSUES — Consider fixing (X found)
────────────────────────────────────────────
[Same format]

INFO — Best practice notes (X found)
─────────────────────────────────────
[Shorter format: issue + suggestion + WCAG ref]

═══════════════════════════════════════════════════
SCORE BREAKDOWN
───────────────────────────────────────────────────
🔴 Critical:  X issues × -10 = -XX pts
🟠 Serious:   X issues × -5  = -XX pts
🟡 Moderate:  X issues × -2  = -XX pts
🔵 Info:      X notes  × 0   = 0 pts
─────────────────────────────────────
Final score: 100 - XX = XX / 100

═══════════════════════════════════════════════════
QUICK WINS
───────────────────────────────────────────────────
Highest impact fixes by effort:

✨ [Fix title] — ~X minutes — saves X pts
   [one-line description]

✨ [Fix title] — ~X minutes — saves X pts
   [one-line description]

═══════════════════════════════════════════════════
NEXT STEPS
───────────────────────────────────────────────────
1. Fix critical issues first — they block access entirely
2. Run axe DevTools or WAVE to catch any automated issues missed
3. Test with keyboard navigation (Tab through entire page)
4. Spot-check with VoiceOver or NVDA on key flows
5. Re-audit after fixes to verify score improvement
═══════════════════════════════════════════════════
```

---

## Quick-scan report (for `/quick` mode)

```
⚡ QUICK SCAN: [URL or filename]
Standard: WCAG 2.2 AA · Critical issues only

🔴 CRITICAL (X found)
──────────────────────
1. [Issue] — [Location] — Fix: [one-line fix]
2. [Issue] — [Location] — Fix: [one-line fix]

⚠️  This is a fast scan. Run /audit for full scored report.
Estimated time to fix critical issues: ~X minutes
```

---

## Component audit report (for `/audit-component` mode)

```
🔍 COMPONENT AUDIT: [Component name]
Standard: WCAG 2.2 AA

Issues: X critical, X serious, X moderate
Score: XX / 100

[Full issue blocks same as main report]

PATTERN NOTES:
[Any notes about the component type — e.g., "For modals, see WAI-ARIA APG
dialog pattern: https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/"]
```
