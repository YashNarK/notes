# React — UI Libraries & Component Systems

## Table of contents

- [React — UI Libraries \& Component Systems](#react--ui-libraries--component-systems)
  - [Table of contents](#table-of-contents)
  - [UI Concepts](#ui-concepts)
    - [What is a UI Component?](#what-is-a-ui-component)
    - [The Problem: Building from Scratch is Slow](#the-problem-building-from-scratch-is-slow)
    - [The Spectrum: Four Approaches to UI](#the-spectrum-four-approaches-to-ui)
    - [Accessibility (a11y)](#accessibility-a11y)
    - [Design Systems](#design-systems)
    - [Which Approach Should You Use?](#which-approach-should-you-use)
  - [Plain React](#plain-react)
    - [Built-in Primitives](#built-in-primitives)
    - [Popular UI Libraries](#popular-ui-libraries)
    - [Utility-First CSS](#utility-first-css)
    - [Headless Component Libraries](#headless-component-libraries)
  - [Next.js](#nextjs)
    - [SSR \& CSS Considerations](#ssr--css-considerations)
    - [Layouts \& UI Composition in App Router](#layouts--ui-composition-in-app-router)
    - [Recommended Libraries with Next.js](#recommended-libraries-with-nextjs)

---

## UI Concepts

### What is a UI Component?

A **UI component** is a self-contained, reusable piece of the interface — a button, a modal dialog, a dropdown menu, a date picker. It encapsulates structure (HTML), appearance (CSS), and sometimes behaviour (JavaScript) in one unit.

```
Without components:                With components:
─────────────────────────────────  ─────────────────────────────────
Copy-paste 50 lines of button      <Button variant="primary">Save</Button>
HTML+CSS everywhere                 reused everywhere, styled once
```

React's component model makes this natural — every piece of UI is a function that returns JSX.

### The Problem: Building from Scratch is Slow

Building a polished, accessible button from scratch involves far more than you'd expect:

```html
<!-- What a properly accessible button actually needs -->
<button
  type="button"
  aria-label="Save document"
  aria-disabled="false"
  class="btn btn-primary"
  tabindex="0"
>
  Save
</button>
```

And that's before you add:
- hover, active, focus, disabled styles
- keyboard navigation
- screen-reader announcements
- dark mode support
- responsive sizing

UI libraries solve this by giving you pre-built, tested, accessible components out of the box.

### The Spectrum: Four Approaches to UI

There is a spectrum from "fully styled" to "completely unstyled":

```
Fully styled ◄────────────────────────────────────────► Fully unstyled
     │                                                           │
 Component        Utility-first CSS          Headless
 Libraries                                   Libraries
(MUI, Ant,      (Tailwind CSS +              (Radix UI,
 Chakra)         shadcn/ui)                  Headless UI)
```

**1. Component Libraries** — pre-designed components you import and use. Fast to build with, opinionated visually. Hard to fully customise.

```tsx
import Button from '@mui/material/Button';
<Button variant="contained">Save</Button>  // looks like Material Design
```

**2. Utility-First CSS (Tailwind)** — no pre-built components; you compose them from atomic CSS classes. Full control over appearance.

```tsx
<button className="bg-blue-600 text-white px-4 py-2 rounded hover:bg-blue-700">
  Save
</button>
```

**3. Headless Component Libraries** — provide behaviour and accessibility only, zero styling. You bring your own CSS.

```tsx
import * as Dialog from '@radix-ui/react-dialog';
// Dialog opens/closes correctly, is accessible, traps focus —
// but you style every pixel yourself
```

**4. Copy-paste (shadcn/ui)** — generates source code into your project that you own and customise. Best of Radix (behaviour) + Tailwind (styling).

### Accessibility (a11y)

**Accessibility** means your UI works for all users, including those using screen readers, keyboard-only navigation, or high-contrast modes.

Key concepts:
- **ARIA attributes** (`aria-label`, `aria-expanded`, `role`) — tell screen readers what an element is
- **Focus management** — when a modal opens, focus must move into it; when it closes, focus must return
- **Keyboard navigation** — dropdowns must be operable with arrow keys; modals must close on Escape
- **Colour contrast** — text must have sufficient contrast ratio (WCAG AA: 4.5:1 for normal text)

> **Why it matters:** Inaccessible UIs exclude users with disabilities and may violate legal requirements (ADA, EN 301 549). Component libraries like Radix UI and Headless UI handle all of this for you.

### Design Systems

A **design system** is a collection of reusable components, tokens (colours, spacing, typography), and guidelines that ensure visual consistency across an entire product.

```
Design system
├── Tokens: --color-primary: #0066FF; --spacing-4: 16px;
├── Components: Button, Card, Modal, Input, Badge
├── Patterns: forms always use 8px gap between fields
└── Guidelines: use primary button only once per page
```

Libraries like MUI implement Material Design (Google's design system). Chakra UI lets you define your own tokens. shadcn/ui gives you the components to build your own.

### Which Approach Should You Use?

| Situation | Recommendation |
|---|---|
| Need to ship fast, don't care about custom look | MUI, Chakra, Ant Design |
| Custom design, full styling control | Tailwind CSS |
| Custom design + need accessibility out of the box | shadcn/ui (Radix + Tailwind) |
| Building your own design system | Radix UI or Headless UI + CSS Modules |
| Next.js with SSR | shadcn/ui or Tailwind (no emotion/styled-components issues) |

---

## Plain React

### Built-in Primitives

React renders JSX to the DOM via **ReactDOM**. Components are the fundamental UI unit.

```tsx
// Functional component with props
interface ButtonProps {
  label: string;
  onClick: () => void;
}

export function Button({ label, onClick }: ButtonProps) {
  return (
    <button className="btn" onClick={onClick}>
      {label}
    </button>
  );
}
```

### Popular UI Libraries

| Library | Install | Notes |
|---|---|---|
| **MUI (Material UI)** | `npm i @mui/material @emotion/react @emotion/styled` | Google Material Design |
| **Chakra UI** | `npm i @chakra-ui/react @emotion/react @emotion/styled` | Accessible, themeable |
| **Ant Design** | `npm i antd` | Enterprise-grade components |
| **shadcn/ui** | `npx shadcn-ui@latest init` | Copy-paste components (Radix + Tailwind) |
| **Mantine** | `npm i @mantine/core @mantine/hooks` | Fully featured, hooks included |

```tsx
// MUI example
import Button from '@mui/material/Button';

export function SaveButton() {
  return <Button variant="contained" color="primary">Save</Button>;
}
```

### Utility-First CSS

**Tailwind CSS** is the most popular utility-first approach. Pair it with `clsx` or `cva` for conditional classes.

```tsx
import clsx from 'clsx';

interface AlertProps {
  type: 'success' | 'error';
  message: string;
}

export function Alert({ type, message }: AlertProps) {
  return (
    <div className={clsx('rounded p-4 text-white', {
      'bg-green-600': type === 'success',
      'bg-red-600':   type === 'error',
    })}>
      {message}
    </div>
  );
}
```

### Headless Component Libraries

Headless libraries provide accessible behaviour with zero styling, letting you own the UI completely.

| Library | Notes |
|---|---|
| **Radix UI** | Unstyled, accessible primitives (Dialogs, Tooltips, etc.) |
| **Headless UI** | From Tailwind Labs; Menus, Listboxes, Transitions |
| **React Aria** | Adobe's accessibility-first hooks |

```tsx
import * as Dialog from '@radix-ui/react-dialog';

export function Modal({ children }: { children: React.ReactNode }) {
  return (
    <Dialog.Root>
      <Dialog.Trigger asChild>
        <button>Open</button>
      </Dialog.Trigger>
      <Dialog.Portal>
        <Dialog.Overlay className="fixed inset-0 bg-black/50" />
        <Dialog.Content className="fixed top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 bg-white p-6 rounded">
          {children}
        </Dialog.Content>
      </Dialog.Portal>
    </Dialog.Root>
  );
}
```

---

## Next.js

### SSR & CSS Considerations

Server-rendered components cannot use browser APIs. Client-only UI libraries must be wrapped in `'use client'` components or loaded dynamically.

```tsx
// app/components/ClientOnly.tsx
'use client';
import { useState, useEffect } from 'react';

export function ClientOnly({ children }: { children: React.ReactNode }) {
  const [mounted, setMounted] = useState(false);
  useEffect(() => setMounted(true), []);
  return mounted ? <>{children}</> : null;
}
```

**CSS Modules** are supported out of the box in Next.js — no additional setup needed.

```tsx
// app/components/Card.module.css  →  .card { border-radius: 8px; }
import styles from './Card.module.css';

export function Card({ title }: { title: string }) {
  return <div className={styles.card}>{title}</div>;
}
```

### Layouts & UI Composition in App Router

The App Router uses `layout.tsx` files to share persistent UI (nav, sidebar) across routes without remounting.

```
app/
  layout.tsx          ← root layout (html, body, global nav)
  dashboard/
    layout.tsx        ← dashboard shell (sidebar)
    page.tsx
```

```tsx
// app/layout.tsx
import { Navbar } from '@/components/Navbar';

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en">
      <body>
        <Navbar />
        <main>{children}</main>
      </body>
    </html>
  );
}
```

### Recommended Libraries with Next.js

| Library | Notes |
|---|---|
| **shadcn/ui** | Best choice — RSC-compatible, no provider required at root |
| **MUI** | Works, but requires `'use client'` wrapper and emotion SSR setup |
| **Tailwind CSS** | First-class support; configured in `tailwind.config.ts` |
| **next/font** | Built-in font optimisation (Google Fonts, local fonts) — use instead of `@import` |

```tsx
// app/layout.tsx — optimised font loading
import { Inter } from 'next/font/google';

const inter = Inter({ subsets: ['latin'] });

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en" className={inter.className}>
      <body>{children}</body>
    </html>
  );
}
```
