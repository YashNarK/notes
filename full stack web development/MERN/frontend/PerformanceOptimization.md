# Frontend Performance Optimization

## Table of contents

- [Frontend Performance Optimization](#frontend-performance-optimization)
  - [Table of contents](#table-of-contents)
  - [Core Web Vitals](#core-web-vitals)
  - [Measuring Performance](#measuring-performance)
  - [JavaScript Optimization](#javascript-optimization)
  - [React-Specific Optimizations](#react-specific-optimizations)
  - [Bundle Optimization](#bundle-optimization)
  - [Image Optimization](#image-optimization)
  - [CSS Optimization](#css-optimization)
  - [Network Optimization](#network-optimization)
  - [Caching Strategies](#caching-strategies)
  - [Lazy Loading](#lazy-loading)
  - [Virtualization](#virtualization)
  - [Fonts Optimization](#fonts-optimization)
  - [Performance Checklist](#performance-checklist)

---

## Core Web Vitals

Google's set of real-world user experience metrics. Used as a ranking signal.

| Metric | Measures | Good | Needs Improvement | Poor |
|---|---|---|---|---|
| **LCP** (Largest Contentful Paint) | Loading performance | ≤ 2.5s | 2.5–4s | > 4s |
| **INP** (Interaction to Next Paint) | Responsiveness | ≤ 200ms | 200–500ms | > 500ms |
| **CLS** (Cumulative Layout Shift) | Visual stability | ≤ 0.1 | 0.1–0.25 | > 0.25 |

**Deprecated (still referenced):**
- **FID** (First Input Delay) → replaced by INP
- **FCP** (First Contentful Paint) — supplementary metric

### Improving LCP

```html
<!-- 1. Preload the hero image -->
<link rel="preload" as="image" href="/hero.webp" fetchpriority="high" />

<!-- 2. No lazy loading on above-the-fold images -->
<img src="/hero.webp" alt="Hero" fetchpriority="high" />  <!-- NOT loading="lazy" -->

<!-- 3. Avoid render-blocking resources -->
<link rel="stylesheet" href="critical.css" />              <!-- inline critical CSS -->
<script src="analytics.js" defer></script>                 <!-- defer non-critical JS -->
```

### Improving INP

```js
// Break up long tasks (> 50ms) — they block the main thread
// ❌ Blocking
function processLargeData(items) {
  items.forEach(item => heavyProcess(item));  // blocks for 200ms
}

// ✅ Yielding to main thread
async function processLargeData(items) {
  for (const item of items) {
    await scheduler.yield();       // or: await new Promise(r => setTimeout(r, 0))
    heavyProcess(item);
  }
}

// ✅ Use Web Workers for CPU-heavy work
const worker = new Worker('/processor.worker.js');
worker.postMessage({ items });
worker.onmessage = (e) => updateUI(e.data);
```

### Improving CLS

```css
/* 1. Always set width/height on images */
img { width: 100%; height: auto; aspect-ratio: 16/9; }

/* 2. Reserve space for dynamic content */
.ad-container { min-height: 250px; }
.skeleton { height: 60px; background: #eee; }

/* 3. Avoid inserting content above existing content */
/* 4. Use CSS transforms for animations (not top/left/width) */
.slide { transform: translateX(0); transition: transform 0.3s; }
```

---

## Measuring Performance

```js
// Measure in code
performance.mark('render:start');
renderDashboard();
performance.mark('render:end');
performance.measure('render', 'render:start', 'render:end');
console.log(performance.getEntriesByName('render')[0].duration);

// Web Vitals library
import { onLCP, onINP, onCLS } from 'web-vitals';

onLCP(({ value, rating }) => {
  console.log(`LCP: ${value}ms (${rating})`);
  analytics.send({ metric: 'LCP', value, rating });
});
onINP(({ value }) => analytics.send({ metric: 'INP', value }));
onCLS(({ value }) => analytics.send({ metric: 'CLS', value }));
```

**Tools:**
- **Lighthouse** — Chrome DevTools, PageSpeed Insights
- **Chrome DevTools Performance panel** — flame charts, main thread analysis
- **Web Vitals Chrome Extension** — real-time metrics on any page
- **WebPageTest** — waterfall charts, filmstrip view
- **Vercel Analytics / Datadog RUM** — real user monitoring in production

---

## JavaScript Optimization

```js
// 1. Debounce expensive operations triggered by user input
function debounce(fn, delay) {
  let timer;
  return (...args) => {
    clearTimeout(timer);
    timer = setTimeout(() => fn(...args), delay);
  };
}
const onSearch = debounce(search, 300);

// 2. Throttle high-frequency events (scroll, resize, mousemove)
function throttle(fn, limit) {
  let lastCall = 0;
  return (...args) => {
    const now = Date.now();
    if (now - lastCall >= limit) {
      lastCall = now;
      fn(...args);
    }
  };
}
window.addEventListener('scroll', throttle(updateHeader, 100));

// 3. Memoize expensive calculations
const memoize = (fn) => {
  const cache = new Map();
  return (...args) => {
    const key = JSON.stringify(args);
    if (cache.has(key)) return cache.get(key);
    const result = fn(...args);
    cache.set(key, result);
    return result;
  };
};

// 4. Avoid memory leaks
function setup() {
  const handler = () => updateUI();
  window.addEventListener('resize', handler);
  return () => window.removeEventListener('resize', handler);   // cleanup
}
```

---

## React-Specific Optimizations

### React.memo

Prevent re-renders when props haven't changed.

```tsx
// ✅ Wrap expensive components that receive stable props
const ProductCard = React.memo(function ProductCard({ product, onAddToCart }) {
  return (
    <div>
      <h3>{product.name}</h3>
      <button onClick={onAddToCart}>Add</button>
    </div>
  );
});

// ❌ Don't memo everything — it adds overhead if props change frequently
// Memo is useful when: re-rendering is expensive AND parent re-renders often
```

### useMemo

Cache expensive calculations.

```tsx
function FilteredList({ items, filter, sortBy }) {
  // ✅ Recalculate only when items, filter, or sortBy changes
  const processedItems = useMemo(() => {
    return items
      .filter(item => item.category === filter)
      .sort((a, b) => a[sortBy].localeCompare(b[sortBy]));
  }, [items, filter, sortBy]);

  return <List items={processedItems} />;
}

// ❌ Not worth memoizing simple operations
const fullName = useMemo(() => `${firstName} ${lastName}`, [firstName, lastName]); // overkill
const fullName = `${firstName} ${lastName}`;  // ✅ just compute it
```

### useCallback

Stable function references for child components and effect dependencies.

```tsx
function Parent() {
  const [count, setCount] = useState(0);

  // ✅ Without useCallback, new function reference on every render
  //    → React.memo'd child re-renders even if nothing changed
  const handleDelete = useCallback((id: string) => {
    deleteItem(id);
  }, []);   // stable — empty deps

  const handleUpdate = useCallback((id: string, data: Partial<Item>) => {
    updateItem(id, data);
  }, [updateItem]);   // recalculate only when updateItem changes

  return <ItemList onDelete={handleDelete} onUpdate={handleUpdate} />;
}
```

### State Batching

React 18 automatically batches state updates (even in async callbacks).

```tsx
// In React 18, all updates in one event handler are batched
function handleClick() {
  setCount(c => c + 1);
  setLoading(false);
  setData(newData);
  // Single re-render — not three!
}

// Force a re-render with fresh state (using flushSync)
import { flushSync } from 'react-dom';
flushSync(() => setCount(c => c + 1));  // re-renders immediately
// then:
setLoading(false);                       // another re-render
```

### Avoiding Common Re-render Causes

```tsx
// ❌ Object/array literals in JSX — new reference every render
<Child style={{ color: 'red' }} />
<Child items={[1, 2, 3]} />

// ✅ Move outside component or memoize
const STYLE = { color: 'red' };
const ITEMS = [1, 2, 3];

// ❌ Inline functions as event handlers on memoized children
<MemoChild onClick={() => doSomething(id)} />  // new function every render

// ✅ useCallback
const onClick = useCallback(() => doSomething(id), [id]);
<MemoChild onClick={onClick} />

// ❌ Context causes all consumers to re-render
<UserContext.Provider value={{ user, theme }}>
// (user and theme updates cause all consumers to re-render)

// ✅ Split contexts
<UserContext.Provider value={user}>
  <ThemeContext.Provider value={theme}>
```

### Concurrent Features (React 18+)

```tsx
import { useTransition, useDeferredValue, startTransition } from 'react';

// useTransition — mark updates as non-urgent
function SearchPage() {
  const [query, setQuery] = useState('');
  const [isPending, startTransition] = useTransition();

  const handleChange = (e) => {
    setQuery(e.target.value);  // urgent — update input immediately
    startTransition(() => {
      setSearchResults(searchData(e.target.value));  // non-urgent — can be interrupted
    });
  };

  return (
    <>
      <input onChange={handleChange} />
      {isPending && <Spinner />}
      <Results />
    </>
  );
}

// useDeferredValue — defer a value for expensive renders
function Results({ query }) {
  const deferredQuery = useDeferredValue(query);  // lags behind input
  const results = useExpensiveFilter(deferredQuery);
  return <List items={results} />;
}
```

---

## Bundle Optimization

```js
// vite.config.ts / webpack config
export default {
  build: {
    rollupOptions: {
      output: {
        manualChunks: {
          'vendor-react': ['react', 'react-dom'],
          'vendor-charts': ['recharts', 'd3'],
          'vendor-ui': ['@radix-ui/react-dialog', '@radix-ui/react-dropdown-menu'],
        },
      },
    },
    chunkSizeWarningLimit: 500,  // warn if chunk > 500KB
  },
};
```

**Tree shaking — import only what you use:**
```js
// ❌ Imports entire lodash (~70KB)
import _ from 'lodash';
const sorted = _.sortBy(items, 'name');

// ✅ Import only what you need (~3KB)
import sortBy from 'lodash/sortBy';
// or:
import { sortBy } from 'lodash-es';  // ES modules enable tree-shaking

// ❌ Full icon set
import { AiOutlineClose, AiOutlineSearch } from 'react-icons/ai';
// ✅ Direct import
import AiOutlineClose from 'react-icons/ai/AiOutlineClose';
```

**Analyze your bundle:**
```bash
npm install --save-dev rollup-plugin-visualizer
# or
npx webpack-bundle-analyzer stats.json
# or
npx source-map-explorer dist/main.*.js
```

---

## Image Optimization

```html
<!-- WebP/AVIF with fallback -->
<picture>
  <source srcset="hero.avif" type="image/avif" />
  <source srcset="hero.webp" type="image/webp" />
  <img src="hero.jpg" alt="Hero" width="1200" height="600" />
</picture>

<!-- Responsive images -->
<img
  src="photo-800.webp"
  srcset="photo-400.webp 400w, photo-800.webp 800w, photo-1200.webp 1200w"
  sizes="(max-width: 600px) 400px, (max-width: 1000px) 800px, 1200px"
  alt="Photo"
  loading="lazy"
  decoding="async"
/>

<!-- fetchpriority for LCP image (don't lazy-load it) -->
<img src="hero.webp" fetchpriority="high" alt="Hero" />
```

**Next.js Image component:**
```tsx
import Image from 'next/image';

<Image
  src="/hero.webp"
  alt="Hero image"
  width={1200}
  height={600}
  priority          // above the fold
  placeholder="blur"
  blurDataURL="data:image/..."
/>
```

---

## CSS Optimization

```css
/* 1. Use CSS containment */
.card { contain: layout style; }

/* 2. Use will-change sparingly — hints to browser to create GPU layer */
.animated { will-change: transform; }
/* Remove after animation */

/* 3. Prefer CSS animations over JS animations (runs on compositor thread) */
@keyframes slide { from { transform: translateX(-100%); } to { transform: translateX(0); } }

/* 4. Avoid layout thrashing — don't mix reads and writes in loops */
/* ❌ */
elements.forEach(el => {
  el.style.height = el.offsetHeight + 10 + 'px';  // read then write in same frame
});
/* ✅ Batch reads, then batch writes */
const heights = elements.map(el => el.offsetHeight);
elements.forEach((el, i) => { el.style.height = heights[i] + 10 + 'px'; });
```

---

## Network Optimization

```html
<!-- Resource hints -->
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="dns-prefetch" href="https://cdn.example.com" />
<link rel="preload" as="script" href="/app.js" />
<link rel="prefetch" href="/next-page.js" />   <!-- low priority, for future nav -->

<!-- Critical CSS inlined -->
<style>/* above-the-fold critical styles */</style>
<link rel="stylesheet" href="full.css" media="print" onload="this.media='all'" />
```

**HTTP/2 and compression:**
- Enable **gzip** or **Brotli** compression on the server (60-80% size reduction)
- Use **HTTP/2** for multiplexed requests (no need for request bundling hacks)
- Set aggressive **cache headers** for static assets

---

## Caching Strategies

```
Cache-Control: public, max-age=31536000, immutable
→ For hashed assets (app.abc123.js) — cache 1 year, never revalidate

Cache-Control: no-cache
→ Always revalidate with server (ETag/Last-Modified comparison)

Cache-Control: no-store
→ Never cache (sensitive data)

Cache-Control: stale-while-revalidate=3600
→ Serve stale immediately, refresh in background
```

**Service Worker caching strategies:**

| Strategy | Use case |
|---|---|
| Cache-first | App shell, static assets |
| Network-first | API data that must be fresh |
| Stale-while-revalidate | Non-critical data |
| Cache-only | Offline assets |
| Network-only | Payment forms |

---

## Lazy Loading

```tsx
// React component lazy loading
const Modal = React.lazy(() => import('./Modal'));
const ChartComponent = React.lazy(() =>
  import('./Charts').then(m => ({ default: m.LineChart }))
);

// Intersection Observer for images
const LazyImage = ({ src, alt }) => {
  const imgRef = useRef(null);
  const [loaded, setLoaded] = useState(false);

  useEffect(() => {
    const observer = new IntersectionObserver(([entry]) => {
      if (entry.isIntersecting) {
        setLoaded(true);
        observer.disconnect();
      }
    }, { rootMargin: '200px' });
    if (imgRef.current) observer.observe(imgRef.current);
    return () => observer.disconnect();
  }, []);

  return (
    <img
      ref={imgRef}
      src={loaded ? src : undefined}
      data-src={src}
      alt={alt}
      loading="lazy"  // native lazy loading (browser support > 90%)
    />
  );
};
```

---

## Virtualization

Only render items visible in the viewport — critical for lists of 100+ items.

```tsx
// @tanstack/react-virtual
import { useVirtualizer } from '@tanstack/react-virtual';

function VirtualizedList({ items }) {
  const parentRef = useRef<HTMLDivElement>(null);

  const rowVirtualizer = useVirtualizer({
    count: items.length,
    getScrollElement: () => parentRef.current,
    estimateSize: () => 80,         // estimated row height
    overscan: 5,                    // render 5 extra items above/below
  });

  return (
    <div ref={parentRef} style={{ height: '600px', overflow: 'auto' }}>
      <div style={{ height: rowVirtualizer.getTotalSize(), position: 'relative' }}>
        {rowVirtualizer.getVirtualItems().map(virtualRow => (
          <div
            key={virtualRow.key}
            style={{
              position: 'absolute',
              top: 0,
              transform: `translateY(${virtualRow.start}px)`,
              width: '100%',
              height: `${virtualRow.size}px`,
            }}
          >
            <ListItem item={items[virtualRow.index]} />
          </div>
        ))}
      </div>
    </div>
  );
}
```

**Alternatives:** `react-window` (lighter), `react-virtuoso` (simpler API with auto-sizing)

---

## Fonts Optimization

```html
<!-- Preconnect to font provider -->
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />

<!-- Use font-display: swap — show fallback font until custom font loads -->
<style>
  @font-face {
    font-family: 'Inter';
    src: url('/fonts/inter.woff2') format('woff2');
    font-display: swap;
    font-weight: 100 900;   /* variable font = one file for all weights */
  }
</style>

<!-- Preload critical fonts -->
<link rel="preload" as="font" href="/fonts/inter.woff2" type="font/woff2" crossorigin />
```

---

## Performance Checklist

### Loading
- [ ] LCP image preloaded with `fetchpriority="high"`
- [ ] Images have `width`/`height` to prevent CLS
- [ ] Non-critical JS deferred with `defer` or `async`
- [ ] Critical CSS inlined; non-critical loaded asynchronously
- [ ] Fonts use `font-display: swap`
- [ ] Text compressed with Brotli/gzip
- [ ] Static assets have long `Cache-Control` headers with content hashing

### JavaScript
- [ ] Bundle analyzed — no unexpected large dependencies
- [ ] Tree shaking working (use ES modules)
- [ ] Route-based code splitting in place
- [ ] Heavy libraries lazy-loaded (chart libs, editors, etc.)
- [ ] Long tasks broken up (< 50ms) or moved to Web Workers

### React
- [ ] `React.memo` on expensive components with stable props
- [ ] `useCallback`/`useMemo` for stable references passed to memoized components
- [ ] No object/array literals directly in JSX props on memoized children
- [ ] `useTransition` for non-urgent state updates
- [ ] Virtualization for lists > 100 items

### Images
- [ ] WebP/AVIF format in use
- [ ] Responsive `srcset` and `sizes` attributes set
- [ ] `loading="lazy"` on below-the-fold images
- [ ] Images served from CDN

### Network
- [ ] CDN configured for static assets
- [ ] HTTP/2 enabled
- [ ] DNS prefetch/preconnect for third-party origins
- [ ] Service Worker for offline support (if applicable)
