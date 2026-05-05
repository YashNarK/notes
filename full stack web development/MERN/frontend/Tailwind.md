# Tailwind CSS

## Table of contents

- [Tailwind CSS](#tailwind-css)
  - [Table of contents](#table-of-contents)
  - [What is Tailwind CSS?](#what-is-tailwind-css)
  - [Installation](#installation)
    - [Vite + React](#vite--react)
    - [Next.js](#nextjs)
    - [Tailwind v4 (CSS-first config)](#tailwind-v4-css-first-config)
  - [Core Philosophy — Utility-First](#core-philosophy--utility-first)
  - [Responsive Design](#responsive-design)
  - [Dark Mode](#dark-mode)
  - [State Variants (hover, focus, active…)](#state-variants-hover-focus-active)
  - [Spacing, Sizing, and Layout Utilities](#spacing-sizing-and-layout-utilities)
    - [Spacing](#spacing)
    - [Flexbox](#flexbox)
    - [Grid](#grid)
    - [Sizing](#sizing)
  - [Typography Utilities](#typography-utilities)
  - [Colors and Backgrounds](#colors-and-backgrounds)
  - [Borders and Rounded Corners](#borders-and-rounded-corners)
  - [Shadows and Effects](#shadows-and-effects)
  - [Custom Configuration (tailwind.config.js / v4)](#custom-configuration-tailwindconfigjs--v4)
    - [Tailwind v3 — tailwind.config.js](#tailwind-v3--tailwindconfigjs)
    - [Tailwind v4 — CSS-first config](#tailwind-v4--css-first-config-1)
  - [Directives (@apply, @layer, @theme)](#directives-apply-layer-theme)
  - [Component Patterns with Tailwind](#component-patterns-with-tailwind)
    - [Extracting reusable classes with @apply](#extracting-reusable-classes-with-apply)
    - [Composing with clsx / tailwind-merge](#composing-with-clsx--tailwind-merge)
  - [Tailwind with React](#tailwind-with-react)
  - [Tailwind v3 vs v4 — Key Differences](#tailwind-v3-vs-v4--key-differences)
  - [Common Pitfalls](#common-pitfalls)
  - [Useful Plugins](#useful-plugins)

---

## What is Tailwind CSS?

Tailwind is a **utility-first CSS framework** — instead of writing `.btn { padding: 8px 16px; ... }`, you compose styles directly in HTML/JSX using small, single-purpose utility classes like `px-4 py-2 rounded bg-blue-500 text-white`.

**Key benefits:**
- No context-switching between CSS and HTML files
- No naming things (no `.card__header--active`)
- Styles don't leak — utilities are scoped by definition
- JIT (Just-In-Time) engine ships only classes you actually use → tiny production CSS

**When NOT to use Tailwind:**
- Teams that strongly prefer semantic CSS and BEM
- Projects with a pre-existing large CSS codebase
- When design tokens are managed centrally outside the codebase

---

## Installation

### Vite + React

```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

`tailwind.config.js`:
```js
/** @type {import('tailwindcss').Config} */
export default {
  content: ['./index.html', './src/**/*.{js,ts,jsx,tsx}'],
  theme: { extend: {} },
  plugins: [],
}
```

`src/index.css`:
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

Import in `main.tsx`:
```tsx
import './index.css';
```

### Next.js

```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

`tailwind.config.js`:
```js
export default {
  content: ['./app/**/*.{ts,tsx}', './components/**/*.{ts,tsx}'],
  theme: { extend: {} },
  plugins: [],
}
```

### Tailwind v4 (CSS-first config)

Tailwind v4 (released 2025) moves all configuration into CSS. No `tailwind.config.js` needed.

```bash
npm install tailwindcss @tailwindcss/vite
```

`vite.config.ts`:
```ts
import tailwindcss from '@tailwindcss/vite';
export default { plugins: [tailwindcss()] };
```

`src/index.css`:
```css
@import "tailwindcss";

/* Customizations live here — no JS config file */
@theme {
  --color-brand: #6366f1;
  --font-display: 'Inter', sans-serif;
  --spacing-18: 4.5rem;
}
```

---

## Core Philosophy — Utility-First

```tsx
{/* ❌ Traditional approach — writing custom CSS */}
<button className="submit-btn">Submit</button>
// .submit-btn { background: #3b82f6; color: white; padding: 8px 16px; border-radius: 6px; }

{/* ✅ Tailwind approach — utilities inline */}
<button className="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600 transition">
  Submit
</button>
```

**Reading Tailwind classes:**

| Class | CSS equivalent |
|---|---|
| `p-4` | `padding: 1rem` |
| `px-4` | `padding-left: 1rem; padding-right: 1rem` |
| `mt-2` | `margin-top: 0.5rem` |
| `w-full` | `width: 100%` |
| `text-lg` | `font-size: 1.125rem` |
| `font-bold` | `font-weight: 700` |
| `flex` | `display: flex` |
| `gap-4` | `gap: 1rem` |
| `rounded-lg` | `border-radius: 0.5rem` |
| `shadow-md` | box shadow |

---

## Responsive Design

Tailwind uses **mobile-first** breakpoint prefixes. Apply a prefix to activate a style **at that breakpoint and above**.

| Prefix | Min-width | Device |
|---|---|---|
| *(none)* | 0px | All — mobile first |
| `sm:` | 640px | Small tablet |
| `md:` | 768px | Tablet |
| `lg:` | 1024px | Desktop |
| `xl:` | 1280px | Large desktop |
| `2xl:` | 1536px | Wide screens |

```tsx
<div className="
  flex flex-col        // mobile: stacked column
  md:flex-row          // tablet+: side by side
  gap-4
  p-4 md:p-8           // more padding on tablet+
">
  <aside className="w-full md:w-64">Sidebar</aside>
  <main  className="flex-1">Content</main>
</div>
```

---

## Dark Mode

```js
// tailwind.config.js
export default {
  darkMode: 'class',  // or 'media' for automatic OS-level
  // ...
}
```

```tsx
{/* 'class' strategy — toggle 'dark' on <html> */}
<html className="dark">
  ...
  <div className="bg-white dark:bg-gray-900 text-gray-900 dark:text-gray-100">
    Content
  </div>
```

```tsx
// Toggle dark mode in React
function useDarkMode() {
  const [dark, setDark] = useState(false);
  useEffect(() => {
    document.documentElement.classList.toggle('dark', dark);
  }, [dark]);
  return [dark, setDark] as const;
}
```

---

## State Variants (hover, focus, active…)

```tsx
<button className="
  bg-indigo-600
  hover:bg-indigo-700      // on hover
  focus:outline-none
  focus:ring-2
  focus:ring-indigo-400    // on focus
  active:scale-95          // on click
  disabled:opacity-50      // when disabled
  disabled:cursor-not-allowed
  transition-all duration-150
">
  Click me
</button>
```

Other variants:
```
group-hover:   // parent has 'group', child reacts to parent hover
peer-focus:    // sibling reacts when 'peer' element is focused
first:         // :first-child
last:          // :last-child
odd: / even:   // :nth-child
placeholder:   // input placeholder styling
selection:     // text selection styling
```

---

## Spacing, Sizing, and Layout Utilities

### Spacing

Tailwind uses a **4px base unit** (1 unit = 4px):

```
p-0 = 0      p-1 = 4px    p-2 = 8px    p-4 = 16px
p-6 = 24px   p-8 = 32px   p-12 = 48px  p-16 = 64px
```

Axis variants: `px-` (horizontal), `py-` (vertical), `pt-` `pr-` `pb-` `pl-` (individual sides)  
Same applies to `m-` (margin), `gap-`, `space-x-`, `space-y-`

### Flexbox

```tsx
<div className="flex items-center justify-between gap-4 flex-wrap">
  {/* items-center    = align-items: center       */}
  {/* justify-between = justify-content: space-between */}
  {/* flex-wrap       = flex-wrap: wrap            */}
</div>

// Direction
flex-row / flex-col / flex-row-reverse / flex-col-reverse

// Grow / shrink
flex-1       // flex: 1 1 0%
flex-auto    // flex: 1 1 auto
flex-none    // flex: none
grow / shrink / grow-0 / shrink-0
```

### Grid

```tsx
<div className="grid grid-cols-3 gap-4">
  <div>1</div><div>2</div><div>3</div>
</div>

// Responsive grid
<div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">

// Spanning
<div className="col-span-2">  // spans 2 columns
<div className="col-start-2 col-end-4">
```

### Sizing

```
w-4 = 1rem      w-1/2 = 50%     w-full = 100%    w-screen = 100vw
h-4 = 1rem      h-1/2 = 50%     h-full = 100%    h-screen = 100vh
max-w-sm / max-w-md / max-w-lg / max-w-xl / max-w-2xl / max-w-7xl
min-h-screen
```

---

## Typography Utilities

```tsx
<h1 className="text-4xl font-bold tracking-tight leading-tight text-gray-900">
  Heading
</h1>
<p className="text-base font-normal text-gray-600 leading-relaxed">
  Body text
</p>
<span className="text-sm font-medium uppercase tracking-wide text-indigo-600">
  Label
</span>
```

| Utility | Values |
|---|---|
| `text-{size}` | xs, sm, base, lg, xl, 2xl … 9xl |
| `font-{weight}` | thin(100), light, normal, medium, semibold, bold, extrabold, black(900) |
| `leading-{size}` | none, tight, snug, normal, relaxed, loose |
| `tracking-{size}` | tighter, tight, normal, wide, wider, widest |
| `text-{color}-{shade}` | `text-gray-700`, `text-indigo-500` |
| `truncate` | overflow: hidden; text-overflow: ellipsis; white-space: nowrap |
| `line-clamp-{n}` | clamp text to n lines (via `@tailwindcss/line-clamp` or built-in v3.3+) |

---

## Colors and Backgrounds

Tailwind ships a large default color palette: each color has shades 50–950.

```tsx
// Background
bg-white / bg-gray-100 / bg-indigo-500 / bg-transparent
bg-gradient-to-r from-indigo-500 to-purple-600

// Text
text-gray-900 / text-white / text-indigo-600

// Border
border border-gray-300 focus:border-indigo-500

// Opacity modifier (v3+)
bg-black/50    // background-color: rgb(0 0 0 / 0.5)
text-white/80  // opacity 80%
```

---

## Borders and Rounded Corners

```tsx
border              // 1px solid
border-2            // 2px
border-gray-300     // border color
border-t            // border-top only

rounded             // 0.25rem
rounded-md          // 0.375rem
rounded-lg          // 0.5rem
rounded-xl          // 0.75rem
rounded-2xl         // 1rem
rounded-full        // 9999px — pills, circles
rounded-none

divide-y divide-gray-200  // borders between flex/grid children
```

---

## Shadows and Effects

```tsx
shadow-sm / shadow / shadow-md / shadow-lg / shadow-xl / shadow-2xl / shadow-none

// Ring (outline for focus states)
ring-2 ring-indigo-500 ring-offset-2

// Opacity
opacity-0 / opacity-50 / opacity-100

// Backdrop (frosted glass)
backdrop-blur-md backdrop-brightness-75

// Transition & animation
transition transition-all duration-200 ease-in-out
animate-spin animate-pulse animate-bounce
```

---

## Custom Configuration (tailwind.config.js / v4)

### Tailwind v3 — tailwind.config.js

```js
/** @type {import('tailwindcss').Config} */
export default {
  content: ['./src/**/*.{ts,tsx}'],
  darkMode: 'class',
  theme: {
    extend: {
      colors: {
        brand: {
          50:  '#eef2ff',
          500: '#6366f1',
          900: '#312e81',
        },
      },
      fontFamily: {
        display: ['Inter', 'sans-serif'],
      },
      spacing: {
        18: '4.5rem',
        72: '18rem',
      },
      screens: {
        '3xl': '1920px',
      },
      borderRadius: {
        '4xl': '2rem',
      },
    },
  },
  plugins: [
    require('@tailwindcss/typography'),
    require('@tailwindcss/forms'),
  ],
}
```

### Tailwind v4 — CSS-first config

```css
@import "tailwindcss";

@theme {
  --color-brand-500: #6366f1;
  --font-display: 'Inter', sans-serif;
  --spacing-18: 4.5rem;
  --breakpoint-3xl: 1920px;
}

/* Override a single default */
@theme {
  --color-gray-*: initial;   /* remove all gray defaults */
  --color-gray-100: #f4f4f5;
  --color-gray-900: #18181b;
}
```

---

## Directives (@apply, @layer, @theme)

```css
/* @apply — extract repeated utilities into a reusable class */
@layer components {
  .btn-primary {
    @apply inline-flex items-center px-4 py-2 rounded-lg bg-indigo-600
           text-white font-medium hover:bg-indigo-700 transition-colors
           focus:outline-none focus:ring-2 focus:ring-indigo-400;
  }

  .input-field {
    @apply w-full px-3 py-2 border border-gray-300 rounded-md
           focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-transparent;
  }
}

/* @layer base — override Tailwind's preflight defaults */
@layer base {
  body { @apply font-sans text-gray-900 antialiased; }
  h1   { @apply text-4xl font-bold; }
}

/* @layer utilities — add custom utilities */
@layer utilities {
  .text-balance { text-wrap: balance; }
}
```

> **Rule of thumb:** Use `@apply` sparingly — only for true reusable components. Inline utilities for one-offs.

---

## Component Patterns with Tailwind

### Extracting reusable classes with @apply

```css
/* Prefer this for multi-use UI primitives */
@layer components {
  .card {
    @apply bg-white rounded-xl shadow-md p-6 border border-gray-100;
  }
}
```

### Composing with clsx / tailwind-merge

When class names are conditional, use `clsx` (logic) + `tailwind-merge` (deduplication):

```bash
npm install clsx tailwind-merge
```

```tsx
import { clsx } from 'clsx';
import { twMerge } from 'tailwind-merge';

// Helper — combine both
export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs));
}

// Usage
function Button({ variant = 'primary', size = 'md', className, ...props }) {
  return (
    <button
      className={cn(
        'inline-flex items-center font-medium rounded-lg transition',
        variant === 'primary' && 'bg-indigo-600 text-white hover:bg-indigo-700',
        variant === 'ghost'   && 'bg-transparent text-indigo-600 hover:bg-indigo-50',
        size === 'sm' && 'px-3 py-1.5 text-sm',
        size === 'md' && 'px-4 py-2 text-base',
        size === 'lg' && 'px-6 py-3 text-lg',
        className  // consumer overrides — tailwind-merge resolves conflicts
      )}
      {...props}
    />
  );
}
```

> `tailwind-merge` resolves class conflicts: `twMerge('px-2 px-4')` → `'px-4'` (last wins).

---

## Tailwind with React

```tsx
// Typed variant pattern (popular in design systems)
import { cva, type VariantProps } from 'class-variance-authority';

const buttonVariants = cva(
  // base classes
  'inline-flex items-center justify-center rounded-md font-medium transition-colors focus-visible:outline-none focus-visible:ring-2',
  {
    variants: {
      variant: {
        default:   'bg-indigo-600 text-white hover:bg-indigo-700',
        outline:   'border border-indigo-600 text-indigo-600 hover:bg-indigo-50',
        ghost:     'hover:bg-gray-100 text-gray-700',
        danger:    'bg-red-600 text-white hover:bg-red-700',
      },
      size: {
        sm: 'h-8  px-3 text-sm',
        md: 'h-10 px-4 text-sm',
        lg: 'h-12 px-6 text-base',
      },
    },
    defaultVariants: { variant: 'default', size: 'md' },
  }
);

interface ButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement>,
    VariantProps<typeof buttonVariants> {}

export function Button({ variant, size, className, ...props }: ButtonProps) {
  return <button className={cn(buttonVariants({ variant, size }), className)} {...props} />;
}

// Usage
<Button variant="outline" size="lg">Learn more</Button>
```

---

## Tailwind v3 vs v4 — Key Differences

| Feature | v3 | v4 |
|---|---|---|
| Configuration | `tailwind.config.js` | CSS `@theme {}` block |
| Import | `@tailwind base/components/utilities` | `@import "tailwindcss"` |
| CSS variables | Optional | First-class — all tokens are CSS vars |
| Build tool | PostCSS | Native Vite/PostCSS plugin (Oxide engine) |
| Performance | Fast | ~5× faster than v3 |
| Arbitrary values | `w-[123px]` | Same |
| Dark mode | `darkMode: 'class'` in config | Same via `@variant dark {}` |
| Plugins | `require(...)` in config | `@plugin "..."` in CSS |

---

## Common Pitfalls

| Problem | Cause | Fix |
|---|---|---|
| Class has no effect | Dynamic class construction (`"text-" + color`) | Use full class strings; Tailwind purges dynamic strings |
| Styles lost in prod | `content` glob missing `.tsx` | Add all template extensions to `content` |
| Class conflict | Both `p-2` and `p-4` applied | Use `tailwind-merge` / `cn()` helper |
| Huge CSS in dev | Expected — JIT ships only used classes in prod | Normal; check production build size |
| Can't override | Specificity fights with `@apply` | Use `!important` modifier: `!bg-red-500` |

```tsx
// ❌ Dynamic construction — Tailwind can't detect this
const color = 'blue';
<div className={`text-${color}-500`} />  // 'text-blue-500' gets purged

// ✅ Use a lookup table with full class names
const colorMap = { blue: 'text-blue-500', red: 'text-red-500' };
<div className={colorMap[color]} />
```

---

## Useful Plugins

| Plugin | Purpose |
|---|---|
| `@tailwindcss/typography` | Beautiful prose styles (`.prose` class) — great for markdown content |
| `@tailwindcss/forms` | Sensible form element resets that work with utilities |
| `@tailwindcss/aspect-ratio` | `aspect-w-16 aspect-h-9` responsive aspect ratio containers |
| `class-variance-authority` (CVA) | Type-safe variant-based component API |
| `tailwind-merge` | Resolves conflicting Tailwind class names |
| `clsx` | Conditional class concatenation utility |
| `shadcn/ui` | Copy-paste component library built on Tailwind + Radix UI |
| `daisyUI` | Semantic component classes on top of Tailwind |
