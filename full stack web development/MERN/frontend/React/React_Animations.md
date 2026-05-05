# React — Animations

## Table of contents

- [React — Animations](#react--animations)
  - [Table of contents](#table-of-contents)
  - [Animation Concepts](#animation-concepts)
    - [What is an Animation in the Browser?](#what-is-an-animation-in-the-browser)
    - [CSS Transitions vs CSS Animations vs JavaScript Animations](#css-transitions-vs-css-animations-vs-javascript-animations)
    - [The Browser Rendering Pipeline](#the-browser-rendering-pipeline)
    - [Cheap vs Expensive Properties to Animate](#cheap-vs-expensive-properties-to-animate)
    - [Easing — The Feel of Motion](#easing--the-feel-of-motion)
    - [Duration-Based vs Spring-Based Animation](#duration-based-vs-spring-based-animation)
    - [The Exit Animation Problem in React](#the-exit-animation-problem-in-react)
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

## Animation Concepts

### What is an Animation in the Browser?

An **animation** is a change in an element's visual properties over time. The browser draws the change frame-by-frame (typically 60 frames per second).

```
Frame 1:  opacity = 0,  translateY = 20px   ← element is invisible, shifted down
Frame 15: opacity = 0.5, translateY = 10px  ← halfway through
Frame 30: opacity = 1,  translateY = 0px    ← element is fully visible, in place
```

At 60fps, 30 frames takes 500ms. The user sees a smooth fade-in slide-up transition.

### CSS Transitions vs CSS Animations vs JavaScript Animations

There are three mechanisms to animate in the browser:

**CSS Transitions** — animate between two states when a CSS class changes. Simple and GPU-accelerated.

```css
.box {
  opacity: 0;
  transition: opacity 300ms ease;  /* animate any opacity change over 300ms */
}
.box.visible {
  opacity: 1;
}
```

```tsx
<div className={`box ${isVisible ? 'visible' : ''}`} />
```

**CSS Animations (`@keyframes`)** — define a multi-step animation sequence. Runs automatically, no state change needed.

```css
@keyframes spin {
  from { transform: rotate(0deg); }
  to   { transform: rotate(360deg); }
}

.spinner {
  animation: spin 1s linear infinite;
}
```

**JavaScript Animations** — drive animations from JS, giving you full control over timing, physics, and interactivity (e.g., drag to dismiss, scroll-driven animations).

```tsx
// Framer Motion — JS drives the animation
<motion.div animate={{ opacity: 1, y: 0 }} initial={{ opacity: 0, y: 20 }} />
```

| | CSS Transitions | CSS Keyframes | JS (Framer Motion) |
|---|---|---|---|
| Control | Simple state A→B | Multi-step sequences | Full — physics, gestures, scroll |
| Performance | ✅ GPU-accelerated | ✅ GPU-accelerated | ✅ (with correct properties) |
| Exit animations | ❌ Not possible | ❌ Not possible | ✅ AnimatePresence |
| Complexity | Low | Medium | Higher |

### The Browser Rendering Pipeline

The browser goes through three stages to paint pixels:

```
1. Layout (Reflow)   → calculate size and position of every element
2. Paint             → fill in pixels (colours, borders, shadows)
3. Composite         → layer composition on the GPU
```

**Animating properties that trigger Layout is expensive** — the browser must recalculate positions for potentially hundreds of elements on every frame:

```css
/* ❌ Triggers Layout — expensive */
width, height, top, left, margin, padding, font-size
```

**Animating properties that only trigger Composite is cheap** — happens entirely on the GPU:

```css
/* ✅ Compositor-only — always smooth */
transform: translateX() translateY() scale() rotate()
opacity
```

### Cheap vs Expensive Properties to Animate

| Property | Pipeline stage | Performance |
|---|---|---|
| `transform` | Composite only | ✅ Always smooth |
| `opacity` | Composite only | ✅ Always smooth |
| `filter` (blur, etc.) | Paint + Composite | ⚠️ Usually fine |
| `background-color` | Paint | ⚠️ Usually fine |
| `width`, `height` | Layout + Paint + Composite | ❌ Can cause jank |
| `top`, `left` | Layout + Paint + Composite | ❌ Use `transform` instead |
| `box-shadow` | Paint | ❌ Expensive |

**Rule:** Always animate `transform` and `opacity`. Simulate position changes with `transform: translate()`, not `top`/`left`.

### Easing — The Feel of Motion

**Easing** controls the acceleration curve of an animation — how fast it starts and ends.

```
linear     ──────────── constant speed — feels robotic
ease-in    ─────────/   starts slow, ends fast — good for exits
ease-out   \─────────   starts fast, ends slow — good for entrances ✅
ease-in-out \────────/  slow at both ends — natural, polished ✅
```

**Natural motion** in the real world always has ease-in-out character — objects don't instantly jump to full speed. Most UI animations should use `ease-out` (elements entering) or `ease-in-out`.

### Duration-Based vs Spring-Based Animation

**Duration-based** — you specify exactly how long an animation takes.

```css
transition: transform 300ms ease-out;
```

Fixed timing can feel mechanical, especially for interactive animations (dragging, physics).

**Spring-based** — instead of duration, you define physical properties: tension (stiffness) and friction (damping). The animation resolves when the spring "settles".

```tsx
// React Spring — physics model, no fixed duration
useSpring({ tension: 300, friction: 20 })
```

Spring animations feel more natural because they behave like real-world objects. They're especially good for gestures and drag interactions.

### The Exit Animation Problem in React

This is the most misunderstood animation challenge in React.

When a component **unmounts** (is removed from the JSX tree), React removes it from the DOM **immediately** — there's no chance for a CSS exit animation to play.

```tsx
// ❌ This exit animation NEVER plays — element is gone before CSS can run
{isOpen && <div className="modal fade-out">...</div>}
```

The solution requires keeping the element in the DOM until the exit animation completes. **Framer Motion's `AnimatePresence`** handles this automatically — it delays unmounting until the `exit` animation finishes.

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
