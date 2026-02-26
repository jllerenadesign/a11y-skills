# WCAG 2.2 Code Patterns Reference

Reference for `accessibility-code` skill. Stack-agnostic — applies to HTML,
React/JSX, Vue, Svelte, or any rendered output.

---

## 1. Accessible Names — Every interactive element needs one

### Buttons
```html
<!-- Text button — name comes from content ✅ -->
<button>Save changes</button>

<!-- Icon-only button ✅ -->
<button aria-label="Close dialog">
  <svg aria-hidden="true" focusable="false">...</svg>
</button>

<!-- Button with icon + text — hide icon from SR ✅ -->
<button>
  <svg aria-hidden="true" focusable="false">...</svg>
  Search
</button>

<!-- ❌ No name — screen reader says "button" -->
<button><svg>...</svg></button>
```

### Links
```html
<!-- Meaningful text ✅ -->
<a href="/report">Download Q3 report (PDF)</a>

<!-- Disambiguating repeated "Read more" links ✅ -->
<a href="/post-1" aria-label="Read more about accessibility basics">Read more</a>

<!-- ❌ Meaningless — "click here" says nothing out of context -->
<a href="/report">Click here</a>
```

### Inputs
```html
<!-- Explicit label ✅ -->
<label for="email">Email address</label>
<input type="email" id="email">

<!-- Implicit label ✅ -->
<label>
  Email address
  <input type="email">
</label>

<!-- Visually hidden label (for tight layouts) ✅ -->
<label for="search" class="sr-only">Search</label>
<input type="text" id="search" placeholder="Search...">

<!-- ❌ Placeholder is not a label — disappears on focus -->
<input type="email" placeholder="Email address">
```

---

## 2. Keyboard Access

### Use native elements first
```html
<!-- ✅ Native button — keyboard accessible by default -->
<button onclick="handleClick()">Submit</button>

<!-- ✅ Native link -->
<a href="/page">Go to page</a>

<!-- ⚠️ Custom interactive element — only if native won't work -->
<div
  role="button"
  tabindex="0"
  onclick="handleClick()"
  onkeydown="if(e.key==='Enter'||e.key===' '){handleClick()}"
>
  Custom button
</div>

<!-- ❌ Div with click but no keyboard support -->
<div onclick="handleClick()">Click me</div>
```

### Focus management rules
- `tabindex="0"` — make element focusable in natural order
- `tabindex="-1"` — focusable by script only (not Tab key)
- Never use `tabindex > 0` — breaks natural tab order
- Never remove focus outline without providing a visible replacement

### Skip links
```html
<!-- First element in <body> ✅ -->
<a href="#main" class="sr-only focus:not-sr-only focus:absolute focus:top-4 focus:left-4">
  Skip to main content
</a>

<header>...</header>
<main id="main" tabindex="-1">...</main>
```

---

## 3. Focus Management for Modals & Overlays

```html
<!-- Pattern: dialog ✅ -->
<div
  role="dialog"
  aria-modal="true"
  aria-labelledby="dialog-title"
  aria-describedby="dialog-desc"
>
  <h2 id="dialog-title">Confirm deletion</h2>
  <p id="dialog-desc">This action cannot be undone.</p>
  <button>Cancel</button>
  <button>Delete</button>
</div>
```

JavaScript requirements:
1. On open: move focus to first focusable element inside dialog
2. While open: trap Tab/Shift+Tab within dialog
3. Escape key: close dialog
4. On close: return focus to the element that triggered the dialog

---

## 4. Semantic HTML & Landmarks

```html
<!-- Page structure ✅ -->
<header>
  <nav aria-label="Main navigation">...</nav>
</header>

<main>
  <h1>Page title</h1>
  <article>...</article>
  <aside aria-label="Related content">...</aside>
</main>

<footer>...</footer>

<!-- Multiple nav elements — distinguish them ✅ -->
<nav aria-label="Main navigation">...</nav>
<nav aria-label="Breadcrumb">...</nav>
<nav aria-label="Footer links">...</nav>
```

Prefer native elements:
| Don't | Do |
|-------|----|
| `<div class="button">` | `<button>` |
| `<div class="nav">` | `<nav>` |
| `<span class="heading">` | `<h2>` |
| `<div class="list">` | `<ul>` / `<ol>` |
| `<div class="header">` | `<header>` |

---

## 5. Forms & Error Handling

```html
<!-- Required field ✅ -->
<label for="name">
  Full name
  <span aria-hidden="true">*</span>
  <span class="sr-only">(required)</span>
</label>
<input
  type="text"
  id="name"
  required
  aria-required="true"
>

<!-- Validation error ✅ -->
<label for="email">Email address</label>
<input
  type="email"
  id="email"
  aria-invalid="true"
  aria-describedby="email-error"
>
<p id="email-error" role="alert">
  Enter a valid email address (e.g., name@example.com)
</p>

<!-- Error summary (for long forms) ✅ -->
<div role="alert" aria-live="assertive">
  <h2>Please fix these errors:</h2>
  <ul>
    <li><a href="#email">Email: Enter a valid email address</a></li>
  </ul>
</div>

<!-- Group related inputs ✅ -->
<fieldset>
  <legend>Notification preferences</legend>
  <label><input type="checkbox" name="email"> Email</label>
  <label><input type="checkbox" name="sms"> SMS</label>
</fieldset>
```

---

## 6. Images & Media

```html
<!-- Informative image ✅ -->
<img src="chart.png" alt="Bar chart showing Q3 revenue of $2.4M, up 18% from Q2">

<!-- Decorative image ✅ -->
<img src="divider.svg" alt="">

<!-- Functional image (inside link/button) ✅ -->
<a href="/"><img src="logo.png" alt="Company Name - Home"></a>

<!-- Icon that's purely decorative ✅ -->
<svg aria-hidden="true" focusable="false">...</svg>

<!-- SVG with meaning ✅ -->
<svg role="img" aria-label="Warning: action cannot be undone">...</svg>

<!-- Video ✅ -->
<video controls>
  <source src="video.mp4">
  <track kind="captions" src="captions.vtt" srclang="en" label="English">
</video>
```

---

## 7. Dynamic Content & Live Regions

```html
<!-- Status message (non-urgent) ✅ -->
<div role="status" aria-live="polite" aria-atomic="true">
  <!-- Inject: "3 results found" -->
</div>

<!-- Alert (urgent, interrupts) ✅ -->
<div role="alert" aria-live="assertive">
  <!-- Inject: "Error: Payment failed" -->
</div>

<!-- Loading state ✅ -->
<button aria-busy="true" aria-disabled="true">
  <span class="sr-only">Loading...</span>
  <span aria-hidden="true">⏳</span>
</button>

<!-- Progress ✅ -->
<div
  role="progressbar"
  aria-valuenow="60"
  aria-valuemin="0"
  aria-valuemax="100"
  aria-label="Upload progress"
>
  60%
</div>
```

---

## 8. Common Component Patterns

### Tabs
```html
<div role="tablist" aria-label="Account settings">
  <button role="tab" aria-selected="true" aria-controls="panel-profile" id="tab-profile">
    Profile
  </button>
  <button role="tab" aria-selected="false" aria-controls="panel-security" id="tab-security" tabindex="-1">
    Security
  </button>
</div>
<div role="tabpanel" id="panel-profile" aria-labelledby="tab-profile">...</div>
<div role="tabpanel" id="panel-security" aria-labelledby="tab-security" hidden>...</div>
```

### Accordion
```html
<button
  aria-expanded="false"
  aria-controls="section-1-content"
  id="section-1-header"
>
  Section 1
</button>
<div
  id="section-1-content"
  role="region"
  aria-labelledby="section-1-header"
  hidden
>
  ...
</div>
```

### Tooltip
```html
<button aria-describedby="tooltip-1">
  Save
</button>
<div role="tooltip" id="tooltip-1">
  Saves your current progress
</div>
```

---

## 9. CSS — Don't break accessibility

```css
/* ✅ Visually hide but keep accessible */
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border-width: 0;
}

/* ✅ Show on focus (for skip links etc.) */
.sr-only:focus {
  position: static;
  width: auto;
  height: auto;
  /* ... */
}

/* ✅ Custom focus style — never just remove */
:focus-visible {
  outline: 2px solid #0066CC;
  outline-offset: 2px;
}

/* ✅ Respect reduced motion */
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}

/* ❌ Never do this without a replacement */
:focus { outline: none; }
```

---

## 10. Testing checklist

### Automated (run first)
- axe DevTools browser extension
- WAVE browser extension
- Lighthouse accessibility audit (Chrome DevTools)

### Manual (always required)
- Navigate entire page using Tab only
- Activate all controls with Enter and Space
- Test modals: focus trapping, Escape to close, focus restore
- Verify focus indicators are visible on all elements
- Check page at 200% browser zoom — no content lost
- Disable CSS — is content still readable in logical order?

### Screen reader spot-check
- VoiceOver (Mac): Cmd+F5, navigate with Ctrl+Opt+Arrow
- NVDA (Windows, free): Tab/arrow navigation
- Check: headings, form labels, button names, error messages
