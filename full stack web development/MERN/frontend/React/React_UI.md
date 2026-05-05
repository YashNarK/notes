# React — UI Libraries & Component Systems

React itself only handles the UI rendering layer. The ecosystem offers a rich set of libraries to build polished interfaces.

## Table of contents

- [React — UI Libraries \& Component Systems](#react--ui-libraries--component-systems)
  - [Table of contents](#table-of-contents)
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
