# Web Accessibility (a11y)

## Table of contents

- [Web Accessibility (a11y)](#web-accessibility-a11y)
  - [Table of contents](#table-of-contents)
  - [What is Accessibility?](#what-is-accessibility)
  - [WCAG Guidelines](#wcag-guidelines)
  - [Semantic HTML](#semantic-html)
  - [ARIA (Accessible Rich Internet Applications)](#aria-accessible-rich-internet-applications)
  - [Keyboard Navigation](#keyboard-navigation)
  - [Focus Management](#focus-management)
  - [Color and Contrast](#color-and-contrast)
  - [Images and Media](#images-and-media)
  - [Forms](#forms)
  - [React and Accessibility](#react-and-accessibility)
  - [Testing Accessibility](#testing-accessibility)
  - [Common Mistakes and Fixes](#common-mistakes-and-fixes)

---

## What is Accessibility?

**Web accessibility** means designing and building websites and applications so that people with disabilities can use them. This includes people with:

- **Visual** impairments (blind, low vision, color blindness)
- **Motor** impairments (can't use a mouse, use switch access or keyboard only)
- **Auditory** impairments (deaf, hard of hearing)
- **Cognitive** impairments (dyslexia, ADHD, memory issues)

Accessibility also benefits:
- Users on slow networks or older devices
- Users with temporary impairments (broken arm, bright sunlight)
- SEO (screen readers and search engine crawlers are similar)
- Legal compliance (ADA, EN 301 549, Section 508)

---

## WCAG Guidelines

The **Web Content Accessibility Guidelines (WCAG)** provide the standard for web accessibility. Current version: **WCAG 2.2**.

### The 4 Principles (POUR)

| Principle | Meaning |
|---|---|
| **Perceivable** | Info must be presentable to users in ways they can perceive |
| **Operable** | UI and navigation must be operable by all users |
| **Understandable** | Info and UI must be understandable |
| **Robust** | Content must be robust enough for assistive technologies |

### Conformance Levels

| Level | Meaning |
|---|---|
| **A** | Minimum accessibility |
| **AA** | The standard for most legal requirements |
| **AAA** | Enhanced accessibility (not required for all content) |

**Most projects target WCAG 2.1/2.2 Level AA.**

---

## Semantic HTML

Use the right HTML element for the job — this is the single most impactful a11y improvement.

```html
<!-- ❌ Non-semantic -->
<div onclick="navigate()">Home</div>
<div class="heading">Page Title</div>
<div class="btn" onclick="submit()">Submit</div>

<!-- ✅ Semantic -->
<a href="/">Home</a>
<h1>Page Title</h1>
<button type="submit">Submit</button>
```

**Landmark elements** — help screen reader users navigate:
```html
<header>    <!-- site header -->
<nav>       <!-- navigation -->
<main>      <!-- main content (one per page) -->
<article>   <!-- self-contained content -->
<section>   <!-- thematic grouping -->
<aside>     <!-- complementary content -->
<footer>    <!-- site footer -->
```

**Heading hierarchy:**
```html
<!-- ✅ Logical, sequential order -->
<h1>Page Title</h1>
  <h2>Section One</h2>
    <h3>Subsection</h3>
  <h2>Section Two</h2>

<!-- ❌ Skipping levels for visual styling -->
<h1>Page Title</h1>
<h4>Section</h4>  <!-- skip h2, h3 -->
```

**Interactive elements:**
```html
<!-- Always use native elements when possible -->
<button>   <!-- keyboard focus, click events, role=button -->
<a href>   <!-- for navigation -->
<input>    <!-- form controls -->
<select>   <!-- dropdown -->
<details>  <!-- disclosure widget -->
<dialog>   <!-- modal -->
```

---

## ARIA (Accessible Rich Internet Applications)

ARIA adds accessibility information that HTML alone can't express. **Only use ARIA when native HTML can't do the job.**

> **Rule #1:** No ARIA is better than bad ARIA.

### ARIA Roles

```html
<!-- Use when native HTML element isn't available -->
<div role="alert">Error: Invalid email</div>          <!-- live region -->
<div role="status">Saved successfully</div>           <!-- polite live region -->
<div role="progressbar" aria-valuenow="75" 
     aria-valuemin="0" aria-valuemax="100">75%</div>
<div role="tab" aria-selected="true">Tab 1</div>
<div role="tabpanel">Content</div>
<div role="dialog" aria-modal="true">Modal</div>
```

### ARIA States and Properties

```html
<!-- aria-label: visible text replacement -->
<button aria-label="Close dialog">✕</button>

<!-- aria-labelledby: reference another element's text -->
<h2 id="modal-title">Settings</h2>
<div role="dialog" aria-labelledby="modal-title">...</div>

<!-- aria-describedby: additional description -->
<input aria-describedby="email-hint" type="email" />
<span id="email-hint">We'll never share your email</span>

<!-- aria-expanded: for toggles -->
<button aria-expanded="false" aria-controls="menu">Menu</button>
<ul id="menu" hidden>...</ul>

<!-- aria-hidden: remove from accessibility tree -->
<span aria-hidden="true">★★★★☆</span>
<span class="sr-only">Rating: 4 out of 5 stars</span>

<!-- aria-live: announce dynamic content -->
<div aria-live="polite">       <!-- wait for user to be idle -->
<div aria-live="assertive">    <!-- interrupt (for errors/alerts)

<!-- aria-required, aria-invalid, aria-disabled -->
<input aria-required="true" aria-invalid="true" />
<button aria-disabled="true">Submit</button>
```

### Screen-reader-only text (visually hidden)

```css
/* Show to screen readers but hide visually */
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
```

```html
<button>
  <svg aria-hidden="true">...</svg>  <!-- hide icon from SR -->
  <span class="sr-only">Delete post</span>
</button>
```

---

## Keyboard Navigation

All interactive functionality must be accessible via keyboard alone.

```
Tab           → move focus to next interactive element
Shift+Tab     → move focus backward
Enter/Space   → activate button, follow link
Arrow keys    → navigate within widgets (menus, tabs, radios)
Escape        → close modals, dismiss dropdowns
Home/End      → jump to first/last item in a list
```

**Making custom elements keyboard accessible:**
```html
<!-- ❌ div click isn't keyboard accessible -->
<div onclick="doSomething()">Click me</div>

<!-- ✅ Add tabindex and keyboard handler -->
<div 
  role="button" 
  tabindex="0"
  onclick="doSomething()"
  onkeydown="if(e.key==='Enter'||e.key===' ') doSomething()"
>
  Click me
</div>

<!-- ✅ Even better: just use a button -->
<button onclick="doSomething()">Click me</button>
```

**`tabindex` values:**
```html
tabindex="0"    <!-- natural tab order (same as native elements) -->
tabindex="-1"   <!-- focusable via JS but not via Tab key -->
tabindex="5"    <!-- ❌ Avoid positive values — disrupts natural order -->
```

---

## Focus Management

```js
// Programmatically move focus (use tabindex="-1" on non-interactive elements)
const heading = document.querySelector('h1');
heading.setAttribute('tabindex', '-1');
heading.focus();

// Focus trap in modals
function trapFocus(element) {
  const focusable = element.querySelectorAll(
    'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
  );
  const first = focusable[0];
  const last = focusable[focusable.length - 1];

  element.addEventListener('keydown', (e) => {
    if (e.key !== 'Tab') return;
    if (e.shiftKey) {
      if (document.activeElement === first) { last.focus(); e.preventDefault(); }
    } else {
      if (document.activeElement === last) { first.focus(); e.preventDefault(); }
    }
  });
}

// Visible focus indicator — never use outline:none without a replacement!
:focus-visible {
  outline: 2px solid #005fcc;
  outline-offset: 2px;
}
```

---

## Color and Contrast

**WCAG contrast requirements:**

| Text size | Minimum (AA) | Enhanced (AAA) |
|---|---|---|
| Normal text (< 18pt) | 4.5:1 | 7:1 |
| Large text (≥ 18pt or 14pt bold) | 3:1 | 4.5:1 |
| UI components / graphics | 3:1 | — |

```css
/* ❌ Low contrast — fails WCAG AA */
.text { color: #999; background: #fff; }  /* 2.85:1 */

/* ✅ Sufficient contrast */
.text { color: #595959; background: #fff; }  /* 7.0:1 ✅ */
```

**Color blindness rules:**
- Never use color as the **only** indicator of meaning
- Always pair color with text, icons, or patterns

```html
<!-- ❌ Color only -->
<span style="color:red">Error</span>

<!-- ✅ Color + icon + text -->
<span class="error">
  <svg aria-hidden="true"><!-- error icon --></svg>
  Error: Invalid email address
</span>
```

**Tools:** [Contrast Checker](https://webaim.org/resources/contrastchecker/), browser DevTools (Color Picker shows contrast ratio).

---

## Images and Media

```html
<!-- Informative image — describe what it conveys -->
<img src="chart.png" alt="Bar chart showing Q4 revenue increased 23% to $1.2M" />

<!-- Decorative image — empty alt so screen readers skip it -->
<img src="divider.svg" alt="" role="presentation" />

<!-- Functional image (icon button) — describe the action -->
<button><img src="search.svg" alt="Search" /></button>

<!-- Complex image — link to a longer description -->
<figure>
  <img src="infographic.png" alt="Process flow diagram — see description below" />
  <figcaption>...</figcaption>
</figure>

<!-- Video -->
<video controls>
  <source src="video.mp4" type="video/mp4" />
  <track kind="captions" src="captions.vtt" srclang="en" label="English" default />
</video>

<!-- Audio transcript -->
<audio controls src="podcast.mp3"></audio>
<details>
  <summary>Transcript</summary>
  <p>Full transcript text...</p>
</details>
```

---

## Forms

```html
<!-- ✅ Always associate label with input -->
<label for="email">Email address</label>
<input type="email" id="email" name="email" required autocomplete="email" />

<!-- Using aria-label when visible label isn't possible -->
<input type="search" aria-label="Search products" />

<!-- Error messages -->
<label for="email">Email</label>
<input 
  type="email" 
  id="email"
  aria-required="true"
  aria-invalid="true"
  aria-describedby="email-error"
/>
<span id="email-error" role="alert">Please enter a valid email address</span>

<!-- Grouping related inputs -->
<fieldset>
  <legend>Shipping address</legend>
  <label for="street">Street</label>
  <input id="street" type="text" />
  <label for="city">City</label>
  <input id="city" type="text" />
</fieldset>

<!-- Radio group -->
<fieldset>
  <legend>Payment method</legend>
  <label><input type="radio" name="payment" value="card"> Credit card</label>
  <label><input type="radio" name="payment" value="paypal"> PayPal</label>
</fieldset>
```

---

## React and Accessibility

```tsx
// htmlFor instead of for
<label htmlFor="email">Email</label>
<input id="email" type="email" />

// className instead of class — doesn't affect a11y but note the difference

// Dynamic content announcements
function Alert({ message }: { message: string }) {
  return (
    <div role="alert" aria-live="assertive">
      {message}
    </div>
  );
}

// Focus management in modals
import { useEffect, useRef } from 'react';

function Modal({ isOpen, onClose, children }) {
  const firstFocusRef = useRef<HTMLButtonElement>(null);

  useEffect(() => {
    if (isOpen) firstFocusRef.current?.focus();
  }, [isOpen]);

  if (!isOpen) return null;

  return (
    <div role="dialog" aria-modal="true" aria-labelledby="modal-title">
      <h2 id="modal-title">Settings</h2>
      {children}
      <button ref={firstFocusRef} onClick={onClose}>Close</button>
    </div>
  );
}

// Route changes — announce page title to screen readers
useEffect(() => {
  document.title = `${pageTitle} | My App`;
  // announce route change
  const announce = document.getElementById('route-announcer');
  if (announce) announce.textContent = `Navigated to ${pageTitle}`;
}, [pageTitle]);
```

**React accessibility libraries:**
- `@radix-ui/*` — headless, accessible primitives (Dialog, Dropdown, etc.)
- `react-aria` (Adobe) — comprehensive accessibility hooks
- `focus-trap-react` — focus trapping for modals
- `@axe-core/react` — runtime a11y checking in development

---

## Testing Accessibility

**Automated testing (catches ~30-40% of issues):**
```bash
npm install --save-dev @axe-core/react
```
```tsx
// Development only
if (process.env.NODE_ENV !== 'production') {
  import('@axe-core/react').then(axe => {
    axe.default(React, ReactDOM, 1000);
  });
}
```

**Jest + jest-axe:**
```tsx
import { axe, toHaveNoViolations } from 'jest-axe';
expect.extend(toHaveNoViolations);

test('form is accessible', async () => {
  const { container } = render(<LoginForm />);
  const results = await axe(container);
  expect(results).toHaveNoViolations();
});
```

**Browser extensions:**
- **axe DevTools** (Deque) — most comprehensive
- **WAVE** (WebAIM) — visual overlay
- **Lighthouse** — built into Chrome DevTools

**Manual testing checklist:**
- [ ] Navigate entire page using keyboard only
- [ ] Test with screen reader (NVDA+Firefox, VoiceOver+Safari, JAWS+Chrome)
- [ ] Check at 200% browser zoom
- [ ] Test with Windows High Contrast mode
- [ ] Verify all focus indicators are visible
- [ ] Check color contrast ratios

---

## Common Mistakes and Fixes

| ❌ Mistake | ✅ Fix |
|---|---|
| `<div onclick>` for buttons | Use `<button>` |
| Missing alt text on images | Add descriptive `alt` or `alt=""` for decorative |
| `outline: none` on focus | Replace with custom visible focus style |
| Missing `<label>` on inputs | Associate `<label for>` or use `aria-label` |
| Low color contrast | Meet WCAG AA minimums (4.5:1 for text) |
| `<table>` for layout | Use CSS Grid/Flexbox |
| Heading tags for styling | Use headings semantically; style with CSS |
| Missing page `<title>` | Update `document.title` on route changes |
| Icon buttons without labels | Add `aria-label` or `<span class="sr-only">` |
| Modal without focus trap | Implement focus management inside modal |
| Using color alone to convey info | Pair with text, icons, or patterns |
| `aria-hidden` on interactive element | Never hide interactive elements from SR |
