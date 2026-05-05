# React — Animations

React has no built-in animation system. The ecosystem offers libraries ranging from declarative physics-based animation to performant CSS-driven transitions.

## Table of contents

- [React — Animations](#react--animations)
  - [Table of contents](#table-of-contents)
  - [Plain React](#plain-react)
    - [CSS Transitions \& Keyframes](#css-transitions--keyframes)
    - [Framer Motion (Recommended)](#framer-motion-recommended)
    - [Gesture Animations](#gesture-animations)
    - [Layout Animations](#layout-animations)
    - [AnimatePresence — Exit Animations](#animatepresence--exit-animations)
    - [React Spring](#react-spring)
    - [Choosing a Library](#choosing-a-library)
  - [Next.js](#nextjs)
    - [SSR Considerations](#ssr-considerations)
    - [Page Transition Animations](#page-transition-animations)
    - [LazyMotion — Reducing Bundle Size](#lazymotion--reducing-bundle-size)
    - [Avoiding Hydration Mismatches](#avoiding-hydration-mismatches)

---

## Plain React

### CSS Transitions & Keyframes

For simple show/hide or hover effects, plain CSS is the most performant option.

```tsx
// Toggle with CSS transition
import { useState } from 'react';
import './styles.css';

export function Tooltip({ text }: { text: string }) {
  const [visible, setVisible] = useState(false);
  return (
    <div
      onMouseEnter={() => setVisible(true)}
      onMouseLeave={() => setVisible(false)}
      style={{ position: 'relative', display: 'inline-block' }}
    >
      Hover me
      <div className={`tooltip ${visible ? 'tooltip--visible' : ''}`}>{text}</div>
    </div>
  );
}
```

```css
/* styles.css */
.tooltip {
  opacity: 0;
  transform: translateY(4px);
  transition: opacity 200ms ease, transform 200ms ease;
  position: absolute;
  background: #333;
  color: #fff;
  padding: 4px 8px;
  border-radius: 4px;
  pointer-events: none;
}
.tooltip--visible {
  opacity: 1;
  transform: translateY(0);
}
```

> **Gotcha**: CSS transitions cannot animate elements that are unmounted from the DOM (`display: none`). Use Framer Motion's `AnimatePresence` for exit animations.

### Framer Motion (Recommended)

Framer Motion is the most popular React animation library — declarative, physics-based, and accessible.

```bash
npm i framer-motion
```

**Basic animation** with `motion` components:

```tsx
import { motion } from 'framer-motion';

export function FadeInCard({ children }: { children: React.ReactNode }) {
  return (
    <motion.div
      initial={{ opacity: 0, y: 20 }}     // start state
      animate={{ opacity: 1, y: 0 }}      // end state
      transition={{ duration: 0.4, ease: 'easeOut' }}
    >
      {children}
    </motion.div>
  );
}
```

**Variants** — define named states and orchestrate children:

```tsx
import { motion } from 'framer-motion';

const containerVariants = {
  hidden: { opacity: 0 },
  visible: {
    opacity: 1,
    transition: { staggerChildren: 0.1 }, // children animate one after another
  },
};

const itemVariants = {
  hidden: { opacity: 0, x: -20 },
  visible: { opacity: 1, x: 0 },
};

export function AnimatedList({ items }: { items: string[] }) {
  return (
    <motion.ul variants={containerVariants} initial="hidden" animate="visible">
      {items.map(item => (
        <motion.li key={item} variants={itemVariants}>
          {item}
        </motion.li>
      ))}
    </motion.ul>
  );
}
```

### Gesture Animations

Framer Motion makes drag and hover interactions trivial:

```tsx
import { motion } from 'framer-motion';

export function DraggableCard() {
  return (
    <motion.div
      drag
      dragConstraints={{ left: -100, right: 100, top: -50, bottom: 50 }}
      whileHover={{ scale: 1.05 }}
      whileTap={{ scale: 0.95 }}
      style={{ width: 100, height: 100, background: 'steelblue', borderRadius: 8, cursor: 'grab' }}
    />
  );
}
```

### Layout Animations

Animate position changes automatically when the DOM layout shifts:

```tsx
import { motion, LayoutGroup } from 'framer-motion';
import { useState } from 'react';

export function SortableList() {
  const [items, setItems] = useState(['A', 'B', 'C']);
  const shuffle = () => setItems(i => [...i].sort(() => Math.random() - 0.5));

  return (
    <LayoutGroup>
      <button onClick={shuffle}>Shuffle</button>
      <ul>
        {items.map(item => (
          <motion.li key={item} layout transition={{ type: 'spring', stiffness: 350, damping: 30 }}>
            {item}
          </motion.li>
        ))}
      </ul>
    </LayoutGroup>
  );
}
```

### AnimatePresence — Exit Animations

`AnimatePresence` enables exit animations when components unmount:

```tsx
import { motion, AnimatePresence } from 'framer-motion';
import { useState } from 'react';

export function Modal({ isOpen }: { isOpen: boolean }) {
  return (
    <AnimatePresence>
      {isOpen && (
        <motion.div
          key="modal"
          initial={{ opacity: 0, scale: 0.9 }}
          animate={{ opacity: 1, scale: 1 }}
          exit={{ opacity: 0, scale: 0.9 }}       // plays on unmount
          transition={{ duration: 0.25 }}
          style={{ position: 'fixed', inset: 0, background: 'white', padding: 32 }}
        >
          Modal content
        </motion.div>
      )}
    </AnimatePresence>
  );
}
```

### React Spring

An alternative to Framer Motion with a physics-first API (no duration — spring tension/friction instead).

```bash
npm i @react-spring/web
```

```tsx
import { useSpring, animated } from '@react-spring/web';

export function BouncyButton() {
  const [pressed, setPressed] = useSpring(() => ({ scale: 1, config: { tension: 300, friction: 10 } }));

  return (
    <animated.button
      style={{ scale: pressed.scale }}
      onMouseDown={() => setPressed({ scale: 0.9 })}
      onMouseUp={() => setPressed({ scale: 1 })}
    >
      Press me
    </animated.button>
  );
}
```

### Choosing a Library

| Need | Recommended |
|---|---|
| Simple hover / transitions | CSS only |
| Declarative, complex animations | Framer Motion |
| Physics-based springs | React Spring |
| SVG path animations | Framer Motion (`pathLength`) |
| Scroll-driven animations | Framer Motion `useScroll` |
| Lightweight (no library) | CSS `@keyframes` + `animation` |

---

## Next.js

### SSR Considerations

Framer Motion's `motion` components render correctly on the server, but some APIs (`useMotionValue`, `useCycle`) require client context.

```tsx
// ✅ Safe in Server Components — just renders static HTML
import { motion } from 'framer-motion';

export default function HeroSection() {
  return (
    <motion.h1 initial={{ opacity: 0 }} animate={{ opacity: 1 }}>
      Welcome
    </motion.h1>
  );
}
```

```tsx
// ❌ Requires 'use client' — uses browser APIs
'use client';
import { motion, useScroll } from 'framer-motion';

export function ScrollProgress() {
  const { scrollYProgress } = useScroll();
  return <motion.div style={{ scaleX: scrollYProgress, height: 4, background: 'blue' }} />;
}
```

### Page Transition Animations

Animate between routes using `AnimatePresence` in the root layout. In the App Router, wrap page content:

```tsx
// app/template.tsx — re-mounts on every navigation (unlike layout.tsx)
'use client';
import { motion } from 'framer-motion';

export default function Template({ children }: { children: React.ReactNode }) {
  return (
    <motion.div
      initial={{ opacity: 0, y: 8 }}
      animate={{ opacity: 1, y: 0 }}
      exit={{ opacity: 0, y: 8 }}
      transition={{ duration: 0.3 }}
    >
      {children}
    </motion.div>
  );
}
```

> `template.tsx` is preferred over `layout.tsx` for page transitions because it re-creates its children on every navigation, allowing exit animations.

### LazyMotion — Reducing Bundle Size

`LazyMotion` defers loading animation features until needed, reducing the initial JS bundle.

```tsx
// app/providers.tsx
'use client';
import { LazyMotion, domAnimation } from 'framer-motion';

export function Providers({ children }: { children: React.ReactNode }) {
  return <LazyMotion features={domAnimation}>{children}</LazyMotion>;
}
```

```tsx
// In components, use `m` instead of `motion` when LazyMotion is active
import { m } from 'framer-motion';

export function Card() {
  return <m.div initial={{ opacity: 0 }} animate={{ opacity: 1 }}>Content</m.div>;
}
```

| Bundle | Size (gzip) |
|---|---|
| Full `motion` | ~34 KB |
| `LazyMotion` + `domAnimation` | ~16 KB |
| `LazyMotion` + `domMax` (gestures) | ~21 KB |

### Avoiding Hydration Mismatches

Animations that depend on viewport size or `window` can cause hydration mismatches. Guard with `useEffect`:

```tsx
'use client';
import { motion } from 'framer-motion';
import { useState, useEffect } from 'react';

export function ViewportAwareAnimation() {
  const [isMounted, setIsMounted] = useState(false);
  useEffect(() => setIsMounted(true), []);

  if (!isMounted) return <div>Content</div>; // static SSR output

  return (
    <motion.div
      initial={{ x: -window.innerWidth }}
      animate={{ x: 0 }}
    >
      Content
    </motion.div>
  );
}
```
