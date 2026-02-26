# RTL & Bilingual Code Rules (EN/AR)

Reference for `accessibility-code` skill — `/audit-rtl` mode and any page
with Arabic content. Load when `dir="rtl"`, `lang="ar"`, or Arabic text is
detected in the source or URL. Stack-agnostic — applies to any HTML output.

---

## 1. Document-level direction

```html
<!-- ✅ Arabic page — set on <html> -->
<html lang="ar" dir="rtl">

<!-- ✅ English page -->
<html lang="en" dir="ltr">

<!-- ✅ Bilingual — set per section or let browser infer -->
<html lang="ar" dir="rtl">
  <section lang="en" dir="ltr">English content</section>
  <section lang="ar" dir="rtl">محتوى عربي</section>
</html>

<!-- ❌ No dir set on Arabic page — layout may break in unexpected places -->
<html lang="ar">
```

### Common `lang` values for KSA/Gulf context
- `ar` — Arabic (generic)
- `ar-SA` — Arabic (Saudi Arabia)
- `ar-AE` — Arabic (UAE)
- `en-SA` — English as used in Saudi Arabia

---

## 2. Bidi isolation for mixed content

Mixed EN/AR inline text needs explicit direction isolation to prevent
the Unicode Bidi Algorithm from reordering content unexpectedly.

```html
<!-- ✅ Isolate a price or number inside Arabic text -->
<p dir="rtl">
  السعر هو
  <bdi>SAR 250</bdi>
  فقط
</p>

<!-- ✅ CSS isolation alternative -->
<span style="unicode-bidi: isolate; direction: ltr;">SAR 250</span>

<!-- ✅ For user-generated content where direction is unknown -->
<p dir="auto">This detects direction from first strong character</p>

<!-- ❌ No isolation — number may render in wrong position -->
<p dir="rtl">السعر هو SAR 250 فقط</p>
```

### When to use `dir="auto"`
- Input fields where user can type either Arabic or English
- User-generated content (comments, messages)
- Any field where content direction is unpredictable

```html
<!-- ✅ Auto-detect for inputs -->
<input type="text" dir="auto" lang="ar">
<textarea dir="auto"></textarea>
```

---

## 3. CSS logical properties (modern RTL approach)

Prefer CSS logical properties over physical ones — they flip automatically
with `dir`.

```css
/* ❌ Physical — must override manually for RTL */
.card {
  margin-left: 16px;
  padding-right: 24px;
  border-left: 4px solid blue;
  float: left;
  text-align: left;
}

/* ✅ Logical — works for both LTR and RTL automatically */
.card {
  margin-inline-start: 16px;
  padding-inline-end: 24px;
  border-inline-start: 4px solid blue;
  float: inline-start;
  text-align: start;
}
```

### Logical property reference
| Physical | Logical equivalent |
|----------|--------------------|
| `margin-left` | `margin-inline-start` |
| `margin-right` | `margin-inline-end` |
| `padding-left` | `padding-inline-start` |
| `padding-right` | `padding-inline-end` |
| `border-left` | `border-inline-start` |
| `left: X` | `inset-inline-start: X` |
| `right: X` | `inset-inline-end: X` |
| `text-align: left` | `text-align: start` |
| `text-align: right` | `text-align: end` |
| `float: left` | `float: inline-start` |

---

## 4. Mirroring icons and directional assets

Icons that convey direction must be mirrored in RTL. Use CSS transform — don't
maintain separate icon files.

```css
/* ✅ Mirror directional icons in RTL context */
[dir="rtl"] .icon-arrow-forward,
[dir="rtl"] .icon-arrow-back,
[dir="rtl"] .icon-chevron-right,
[dir="rtl"] .icon-chevron-left,
[dir="rtl"] .icon-next,
[dir="rtl"] .icon-breadcrumb-separator {
  transform: scaleX(-1);
}
```

### Icons that mirror ✅
Back arrows, forward arrows, chevrons indicating direction, progress indicators,
breadcrumb separators, expand/collapse controls, next/previous.

### Icons that do NOT mirror ❌
Play/pause/stop (media), checkmarks, close (×), warning (⚠), info (ℹ),
logos, brand marks, maps, clocks, non-directional illustrations.

---

## 5. Arabic typography in CSS

```css
/* ✅ Arabic font stack */
body[lang="ar"],
:lang(ar) {
  font-family: 'IBM Plex Arabic', 'Noto Naskh Arabic', 'Cairo',
               'Segoe UI Arabic', 'Arial', sans-serif;

  /* Arabic needs more line height than Latin */
  line-height: 1.8;

  /* NEVER increase letter-spacing for Arabic */
  letter-spacing: 0;

  /* Minimum size for Arabic body text */
  font-size: 14px; /* or 0.875rem — slightly larger than Latin minimum */
}

/* ✅ Headings in Arabic */
h1:lang(ar), h2:lang(ar), h3:lang(ar) {
  line-height: 1.6;
  letter-spacing: 0; /* always 0 for Arabic */
}

/* ❌ Never apply positive letter-spacing to Arabic */
.arabic-text {
  letter-spacing: 0.05em; /* breaks connected letterforms */
}
```

---

## 6. Form accessibility in RTL

```html
<!-- ✅ Arabic form — labels right of inputs, text right-aligned -->
<form dir="rtl" lang="ar">

  <div class="field">
    <label for="name-ar">الاسم الكامل</label>
    <input
      type="text"
      id="name-ar"
      name="name"
      dir="auto"
      required
      aria-required="true"
    >
  </div>

  <!-- ✅ Checkbox: label on LEFT of control in RTL -->
  <label>
    <input type="checkbox" name="agree">
    أوافق على الشروط والأحكام
  </label>
  <!-- Note: browser renders label to the left of checkbox in RTL
       because the label comes AFTER the input in DOM order -->

  <!-- ✅ Error message: right-aligned, linked to input -->
  <input
    type="email"
    id="email-ar"
    aria-invalid="true"
    aria-describedby="email-error-ar"
    dir="auto"
  >
  <p id="email-error-ar" role="alert" dir="rtl">
    يرجى إدخال بريد إلكتروني صحيح
  </p>

</form>
```

---

## 7. ARIA in bilingual context

```html
<!-- ✅ Language-specific accessible names -->
<button aria-label="بحث">
  <svg aria-hidden="true">...</svg>
</button>

<!-- ✅ Navigation landmark in Arabic -->
<nav aria-label="التنقل الرئيسي" dir="rtl">...</nav>

<!-- ✅ Live region for Arabic announcements -->
<div
  role="status"
  aria-live="polite"
  aria-atomic="true"
  dir="rtl"
  lang="ar"
>
  <!-- Inject: "تم الحفظ بنجاح" -->
</div>

<!-- ✅ Bilingual page — label in same language as section -->
<main>
  <section lang="en" aria-label="English content">...</section>
  <section lang="ar" aria-label="المحتوى العربي" dir="rtl">...</section>
</main>
```

---

## 8. Common RTL code issues to flag

| Issue | Severity | Fix |
|-------|----------|-----|
| `<html>` missing `dir="rtl"` for Arabic page | 🔴 Critical | Add `dir="rtl"` to `<html>` |
| `<html>` missing `lang="ar"` | 🔴 Critical | Add correct `lang` attribute |
| Physical CSS properties used (margin-left, padding-right) without RTL overrides | 🟠 Serious | Replace with logical properties or add `[dir="rtl"]` overrides |
| Directional icons not mirrored | 🟠 Serious | Add `transform: scaleX(-1)` for RTL |
| `letter-spacing` > 0 applied to Arabic text | 🟠 Serious | Set `letter-spacing: 0` |
| `line-height` < 1.8 for Arabic body text | 🟡 Moderate | Increase to minimum 1.8 |
| Mixed EN/AR inline without `<bdi>` or `unicode-bidi: isolate` | 🟡 Moderate | Wrap mixed content in `<bdi>` |
| Input without `dir="auto"` for bilingual entry | 🟡 Moderate | Add `dir="auto"` |
| ARIA labels in wrong language for section | 🟡 Moderate | Match ARIA label language to content |
| System Arabic font fallback (no explicit Arabic font) | 🔵 Info | Define explicit Arabic font stack |
| Positive `tabindex` breaks RTL tab order | 🟠 Serious | Remove positive tabindex values |

---

## 9. Testing RTL accessibility

### Automated
- Run axe DevTools — it flags missing `lang` and `dir` attributes
- Validate `lang` attribute with W3C Validator
- Check logical property support with browser devtools

### Manual
- Switch browser language to Arabic and reload — does layout flip?
- Navigate with keyboard in RTL — does tab order follow visual right-to-left flow?
- Test with screen reader in Arabic (VoiceOver supports Arabic, NVDA with eSpeak Arabic)
- Check mixed EN/AR content for bidi rendering issues
- Verify icons mirror correctly on RTL switch
- Test forms: labels, errors, and placeholders in Arabic

### Screen reader Arabic support
- **VoiceOver (Mac/iOS)**: add Arabic (Saudi Arabia) to preferred languages, switch TTS voice to Maged or Tarik
- **NVDA (Windows)**: install Arabic language pack + eSpeak Arabic or OneCore Arabic voice
- **Narrator (Windows)**: supports Arabic via OneCore voices
