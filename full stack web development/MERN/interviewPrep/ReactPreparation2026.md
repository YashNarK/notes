# Ultimate Advanced React JS Interview Preparation Guide (2026 Edition)

## Table of contents

- [Ultimate Advanced React JS Interview Preparation Guide (2026 Edition)](#ultimate-advanced-react-js-interview-preparation-guide-2026-edition)
  - [Table of contents](#table-of-contents)
  - [Anatomy of This Guide](#anatomy-of-this-guide)
  - [React Core Architecture Refresh](#react-core-architecture-refresh)
  - [React 18 Concurrent Rendering Model](#react-18-concurrent-rendering-model)
  - [React 19 — What's New (2026 Focus)](#react-19--whats-new-2026-focus)
  - [Server-Side and Hybrid Rendering Strategies](#server-side-and-hybrid-rendering-strategies)
  - [State Management at Scale](#state-management-at-scale)
  - [Performance Optimization Master Class](#performance-optimization-master-class)
  - [Code Splitting and Asset Delivery](#code-splitting-and-asset-delivery)
  - [Advanced Hooks Patterns](#advanced-hooks-patterns)
  - [Accessibility (a11y) Excellence](#accessibility-a11y-excellence)
  - [Security Hardening Checklist](#security-hardening-checklist)
  - [Testing Strategies](#testing-strategies)
  - [Modern Build Toolchain (2026)](#modern-build-toolchain-2026)
  - [GraphQL with Apollo and TanStack Query](#graphql-with-apollo-and-tanstack-query)
  - [Micro-Frontends and Federated Modules](#micro-frontends-and-federated-modules)
  - [React Native Cross-Skill Insights](#react-native-cross-skill-insights)
  - [Common Senior-Level Interview Scenarios](#common-senior-level-interview-scenarios)
  - [High-Yield Comparative Tables](#high-yield-comparative-tables)
  - [Mock Interview Questions and Quick Hints](#mock-interview-questions-and-quick-hints)
  - [Final 8-Week Mastery Roadmap](#final-8-week-mastery-roadmap)
  - [Closing Thoughts](#closing-thoughts)

---

## Anatomy of This Guide

- Curated from 60+ up-to-date industry references.
- Updated for **React 19** (stable December 2024) and the 2026 interview landscape.
- Coverage spans React 18+/19 features, React Compiler, Server Actions, performance, security, testing, micro-frontends, GraphQL, and React Native crossover skills.
- Comparative tables, code snippets, and checklists reinforce retention.

---

## React Core Architecture Refresh

### Virtual DOM, Diffing & Reconciliation

React creates lightweight Virtual DOM trees, diffs them against previous renders, then batches minimal real-DOM mutations to keep UI and state in sync. Understanding how keys, list ordering, and component identity influence the diff algorithm is pivotal when optimizing renders.

### JSX Compilation Pipeline

JSX is translated by Babel (or the new SWC/Vite transform) into `React.createElement` calls — or with React 19's automatic JSX runtime, `_jsx` calls. Understanding the transform pipeline helps debug production bundles and tree-shaking issues.

### Component Taxonomy

1. **Function components with hooks** — the only pattern for all new code.
2. **Class components** — legacy code only; still needed for `componentDidCatch` (no hook equivalent yet).
3. **`React.memo()`** — skip re-renders when props are shallow-equal.
4. **`forwardRef`** — deprecated in React 19; refs are now plain props.
5. **`lazy` + `Suspense`** — code splitting and async component loading.

---

## React 18 Concurrent Rendering Model

### Concurrent Rendering

React 18 introduced an interruptible renderer that pauses, resumes, or abandons work based on priority, ensuring 60 fps interactions even under heavy computation.

- `startTransition()` — defers non-urgent state updates to background priority.
- `useDeferredValue()` — produces a deferred copy of a value for low-priority re-renders.
- `useTransition()` — returns `[isPending, startTransition]`; shows pending UI during transition.
- **Automatic batching** — groups state changes across `setTimeout`, Promises, and native events. React 17 only batched inside event handlers.

### Suspense Improvements

Streaming SSR pipes HTML chunks as soon as they're ready, while the client hydrates concurrently, dramatically shrinking Time-to-Interactive on slow networks.

```tsx
// Streaming with Suspense in Next.js App Router
export default function Page() {
  return (
    <Suspense fallback={<Skeleton />}>
      <SlowDataComponent />   {/* streams after fast shell */}
    </Suspense>
  );
}
```

---

## React 19 — What's New (2026 Focus)

React 19 (released December 2024) is the biggest release since React 18. **Interviewers in 2026 will expect fluency here.**

### `use()` Hook

Read a Promise or Context directly in render — replaces many `useEffect` + `useState` data-fetch patterns.

```tsx
import { use } from 'react';

function UserProfile({ userPromise }: { userPromise: Promise<User> }) {
  const user = use(userPromise);   // suspends until resolved
  return <h1>{user.name}</h1>;
}

// Use Context without useContext
function Theme() {
  const theme = use(ThemeContext);  // works conditionally, unlike useContext
  return <div className={theme} />;
}
```

### Server Actions

Functions marked `'use server'` run exclusively on the server and can be called directly from client components — eliminating manual API route plumbing.

```tsx
// app/actions.ts
'use server';
export async function createPost(formData: FormData) {
  const title = formData.get('title') as string;
  await db.posts.create({ title });
  revalidatePath('/posts');
}

// app/NewPost.tsx (client component)
import { createPost } from './actions';
export default function NewPost() {
  return (
    <form action={createPost}>
      <input name="title" />
      <button type="submit">Post</button>
    </form>
  );
}
```

### `useActionState`

Manages async action state (loading, result, error) for form submissions. Replaces the old `useFormState` from React DOM.

```tsx
import { useActionState } from 'react';
import { createPost } from './actions';

function NewPostForm() {
  const [state, action, isPending] = useActionState(createPost, null);

  return (
    <form action={action}>
      <input name="title" disabled={isPending} />
      {state?.error && <p role="alert">{state.error}</p>}
      <button disabled={isPending}>
        {isPending ? 'Posting...' : 'Post'}
      </button>
    </form>
  );
}
```

### `useFormStatus`

Read the submission state of the enclosing form — great for submit buttons inside deeply nested form components.

```tsx
import { useFormStatus } from 'react-dom';

function SubmitButton() {
  const { pending } = useFormStatus();
  return <button disabled={pending}>{pending ? 'Saving...' : 'Save'}</button>;
}
```

### `useOptimistic`

Apply an optimistic update during an async operation; automatically reverts if the operation fails.

```tsx
import { useOptimistic } from 'react';

function MessageList({ messages, sendMessage }) {
  const [optimisticMessages, addOptimistic] = useOptimistic(
    messages,
    (current, newMsg) => [...current, { ...newMsg, sending: true }]
  );

  async function handleSend(formData: FormData) {
    const text = formData.get('text') as string;
    addOptimistic({ text });          // instant UI update
    await sendMessage(text);          // real request
  }

  return (
    <form action={handleSend}>
      {optimisticMessages.map(m => <p key={m.id}>{m.text}</p>)}
      <input name="text" /><button>Send</button>
    </form>
  );
}
```

### React Compiler (React Forget)

The **React Compiler** (opt-in, now available for production) automatically inserts `memo`, `useMemo`, and `useCallback` at compile time. Manual memoization becomes optional.

```bash
npm install babel-plugin-react-compiler
```

```js
// babel.config.js
module.exports = {
  plugins: [['babel-plugin-react-compiler', {}]],
};
```

> **Interview key point:** The compiler analyzes data-flow to determine what needs memoizing. It does NOT change runtime behavior — it only adds memoization where the rules of React are followed correctly.

### ref as Prop (no more `forwardRef`)

```tsx
// React 19: refs are regular props
function Input({ ref, ...props }) {
  return <input ref={ref} {...props} />;
}

// React 18 (old): required forwardRef wrapper
const Input = forwardRef((props, ref) => <input ref={ref} {...props} />);
```

### Document Metadata Anywhere

`<title>`, `<meta>`, and `<link>` can now be rendered inside components — React hoists them to `<head>` automatically.

```tsx
function ProductPage({ product }) {
  return (
    <>
      <title>{product.name} | My Shop</title>
      <meta name="description" content={product.description} />
      <h1>{product.name}</h1>
    </>
  );
}
```

### Context as Provider

```tsx
// React 19
<ThemeContext value="dark">
  <App />
</ThemeContext>

// React 18 (still works)
<ThemeContext.Provider value="dark">
  <App />
</ThemeContext.Provider>
```

### Improved Hydration Error Messages

React 19 shows exactly which attribute or element caused a hydration mismatch, with a diff — dramatically easier to debug SSR issues.

---

## Server-Side and Hybrid Rendering Strategies

| Technique | Key Benefit | When to Prefer | Caveat |
|---|---|---|---|
| Traditional SSR | SEO & first-paint speed | Marketing pages | Higher server cost |
| Static Generation (SSG) | Zero runtime cost | Blogs, docs | Stale data risk |
| Streaming SSR | Progressive hydration | Dashboards | Framework support required |
| **React Server Components (stable)** | Zero JS for non-interactive UI | Data-heavy feeds | No hooks/browser APIs |
| **Server Actions** | Eliminates API route boilerplate | Forms, mutations | Server-only, Next.js 14+/15 |
| Incremental Static Regen (ISR) | Fresh static pages | E-commerce, news | Next.js specific |

### React Server Components vs Client Components

```tsx
// Server Component (default in Next.js App Router)
// Runs on server — can access DB, filesystem, env secrets
// No useState, useEffect, event handlers
async function ProductList() {
  const products = await db.products.findAll();  // direct DB call
  return <ul>{products.map(p => <li key={p.id}>{p.name}</li>)}</ul>;
}

// Client Component — add 'use client' directive
'use client';
import { useState } from 'react';
function AddToCart({ productId }) {
  const [added, setAdded] = useState(false);
  return <button onClick={() => setAdded(true)}>{added ? '✓' : 'Add'}</button>;
}
```

---

## State Management at Scale

### Local Hooks & Context

`useState`, `useReducer`, and `useContext` (or React 19's `use(Context)`) handle intra-component or domain-wide data without extra libraries.

### Global Stores Comparison (2026)

| Library | Boilerplate | Learning Curve | Best For | DevTools |
|---|---|---|---|---|
| Redux Toolkit + RTK Query | Moderate | Medium | Large teams, time-travel debug | Time-travel |
| **Zustand v5** | Minimal | Low | Small–medium apps, RSC-friendly | Redux DevTools |
| Jotai | Minimal | Low | Atom-based, fine-grained reactivity | Jotai DevTools |
| MobX | Minimal | Medium | Reactive OOP patterns | MobX DevTools |
| **TanStack Query v5** | Low | Low | Server state / async data fetching | React Query DevTools |
| Context API only | None | Low | Small apps with simple state | React DevTools |

> **2026 trend:** Server state (data from APIs) is increasingly handled by TanStack Query or the RSC data-fetch model, leaving client-side stores (Zustand, Jotai) for purely local UI state.

---

## Performance Optimization Master Class

### Memoization — Before vs After React Compiler

```tsx
// Before React Compiler (manual)
const expensiveList = useMemo(
  () => items.filter(i => i.active).sort((a, b) => b.score - a.score),
  [items]
);
const handleClick = useCallback(() => onSelect(id), [id, onSelect]);

// After React Compiler (automatic — compiler inserts memo for you)
const expensiveList = items.filter(i => i.active).sort((a, b) => b.score - a.score);
const handleClick = () => onSelect(id);
```

### List Virtualization

Apply `@tanstack/react-virtual` (the modern replacement for `react-window`) to render only visible rows, saving memory on huge feeds.

```tsx
import { useVirtualizer } from '@tanstack/react-virtual';

function VirtualList({ items }) {
  const parentRef = useRef(null);
  const virtualizer = useVirtualizer({
    count: items.length,
    getScrollElement: () => parentRef.current,
    estimateSize: () => 48,
  });

  return (
    <div ref={parentRef} style={{ height: '400px', overflow: 'auto' }}>
      <div style={{ height: `${virtualizer.getTotalSize()}px`, position: 'relative' }}>
        {virtualizer.getVirtualItems().map(row => (
          <div key={row.key} style={{ position: 'absolute', top: row.start, height: row.size }}>
            {items[row.index].name}
          </div>
        ))}
      </div>
    </div>
  );
}
```

### Profiling Workflow

1. Record with **React DevTools Profiler** (now shows React Compiler impact too).
2. Sort commits by "wasted" time.
3. Add `memo`, move heavy computation to Web Workers, or split components.
4. Measure **Core Web Vitals**: LCP, INP (replaced FID), CLS.

### Performance Checklist

| Technique | Dev Cost | Typical Win |
|---|---|---|
| React Compiler (auto-memo) | Minimal (install once) | 10–30% CPU |
| Code split per route | Medium | 30% bundle size |
| Virtualize lists | Medium | 90% fewer DOM nodes |
| Move heavy calc to Web Worker | High | 50% main-thread freed |
| `useDeferredValue` for filter input | Low | Smooth typing at 60fps |

---

## Code Splitting and Asset Delivery

| Mechanism | API | Ideal Use |
|---|---|---|
| Component Level | `React.lazy()` + `<Suspense>` | Modals, charts, heavy widgets |
| Route Level | Dynamic `import()` inside router | Large pages |
| Module Federation | Webpack 5 remote chunks | Micro-frontends |
| **Resource Preloading (React 19)** | `preinit`, `preload`, `prefetchDNS` | Fonts, critical CSS, 3rd-party scripts |

```tsx
// React 19 resource hints
import { preinit, preload } from 'react-dom';

function App() {
  preload('/fonts/inter.woff2', { as: 'font', crossOrigin: 'anonymous' });
  preinit('/analytics.js', { as: 'script' });
  return <Main />;
}
```

---

## Advanced Hooks Patterns

### Custom Hooks for Reuse

Abstract repetitive effects (e.g., `useDebounce`, `useLocalStorage`, `useFetch`) to keep components declarative.

```tsx
function useDebounce<T>(value: T, delay = 300): T {
  const [debounced, setDebounced] = useState(value);
  useEffect(() => {
    const id = setTimeout(() => setDebounced(value), delay);
    return () => clearTimeout(id);
  }, [value, delay]);
  return debounced;
}
```

### `useSyncExternalStore`

Concurrent-safe subscription to external mutable stores (Redux ≥4.2, Zustand use it internally).

```tsx
function useOnlineStatus() {
  return useSyncExternalStore(
    (callback) => {
      window.addEventListener('online', callback);
      window.addEventListener('offline', callback);
      return () => { window.removeEventListener('online', callback); window.removeEventListener('offline', callback); };
    },
    () => navigator.onLine,
    () => true   // server snapshot
  );
}
```

### Anti-Patterns (2026)

| Anti-Pattern | Problem | Fix |
|---|---|---|
| Conditional hooks | Breaks call order | Always call hooks unconditionally |
| Storing derived data in state | Double source of truth | Compute in render or `useMemo` |
| `useEffect` for data fetching | Waterfalls, race conditions | Use TanStack Query or RSC |
| Manual `useMemo`/`useCallback` everywhere | Premature optimization | Let React Compiler handle it |
| Over-using `useEffect` | Logic scattered | Move to event handlers or Server Actions |

---

## Accessibility (a11y) Excellence

1. Prefer semantic HTML — replace `<div onClick>` with `<button>`.
2. Supply `aria-*` attributes for custom widgets (menus, tabs, dialogs).
3. Manage focus after template swaps (`ref.current?.focus()`) to avoid lost keyboard context.
4. Use `role="alert"` and `aria-live="polite"` for dynamically injected error messages.
5. Test with axe-core, Lighthouse a11y audit, and a real screen reader (NVDA, VoiceOver).

```tsx
// Accessible loading state
function SaveButton({ isPending }) {
  return (
    <button aria-disabled={isPending} aria-busy={isPending}>
      {isPending ? 'Saving…' : 'Save'}
    </button>
  );
}

// Accessible error region
<div role="alert" aria-live="assertive">
  {error && <span>{error.message}</span>}
</div>
```

---

## Security Hardening Checklist

| Vulnerability | Attack Vector | Mitigation |
|---|---|---|
| XSS | `dangerouslySetInnerHTML` | Sanitize with DOMPurify; escape output by default |
| CSRF | Forged cookie requests | SameSite cookies + CSRF tokens |
| Secrets in client bundle | `process.env.SECRET` in Vite/CRA | Secrets must live server-side only |
| Dependency exploits | Outdated packages | `npm audit` + Snyk in CI |
| Server Action abuse | Unauthenticated form actions | Always authenticate inside Server Actions |
| Injection (Server Actions) | Unvalidated FormData | Validate with Zod before DB calls |

```tsx
// ✅ Safe Server Action — always authenticate + validate
'use server';
import { auth } from '@/lib/auth';
import { z } from 'zod';

const Schema = z.object({ title: z.string().min(1).max(200) });

export async function createPost(formData: FormData) {
  const session = await auth();
  if (!session) throw new Error('Unauthorized');

  const { title } = Schema.parse({ title: formData.get('title') });
  await db.posts.create({ title, userId: session.user.id });
}
```

---

## Testing Strategies

### Unit & Integration

- **Vitest** — now the default for Vite-based projects (faster than Jest, same API).
- **React Testing Library** — user-centric DOM queries (`getByRole`, `findByLabelText`).
- **MSW v2** — mock Service Worker for network-level request mocking.

```tsx
// Vitest + RTL + MSW example
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { http, HttpResponse } from 'msw';
import { server } from '../mocks/server';
import LoginForm from './LoginForm';

test('shows error on failed login', async () => {
  server.use(
    http.post('/api/login', () => HttpResponse.json({ error: 'Invalid credentials' }, { status: 401 }))
  );

  render(<LoginForm />);
  await userEvent.type(screen.getByLabelText('Email'), 'wrong@example.com');
  await userEvent.type(screen.getByLabelText('Password'), 'bad');
  await userEvent.click(screen.getByRole('button', { name: 'Sign in' }));

  expect(await screen.findByRole('alert')).toHaveTextContent('Invalid credentials');
});
```

### End-to-End (E2E)

- **Playwright** — cross-browser (Chromium, Firefox, WebKit), parallel, auto-wait. Standard for 2026.
- Combine with Server Action mocking for full coverage of RSC workflows.

### Testing Server Actions

```tsx
// Test Server Actions independently (no browser needed)
import { createPost } from './actions';

test('createPost validates title length', async () => {
  const formData = new FormData();
  formData.set('title', '');
  await expect(createPost(formData)).rejects.toThrow();
});
```

---

## Modern Build Toolchain (2026)

| Tool | Role | Notes |
|---|---|---|
| **Vite 6** | Dev server + bundler | Standard for new projects; CRA is deprecated |
| **SWC** | Transpiler | Replaces Babel in most Vite/Next setups; 20× faster |
| **Webpack 5** | Bundler | Still dominant in enterprise / Module Federation setups |
| **Turbopack** | Bundler (Next.js) | Rust-based, replaces Webpack in Next.js 15 dev mode |
| **Biome** | Linter + formatter | Replaces ESLint + Prettier in some teams; 25× faster |
| **React Compiler** | Optimizer | Automatic memoization; opt-in plugin |

> **CRA is officially deprecated** as of 2023. All new projects should use Vite, Next.js, or Remix.

---

## GraphQL with Apollo and TanStack Query

### Apollo Client

```tsx
import { ApolloClient, InMemoryCache, ApolloProvider } from '@apollo/client';

const client = new ApolloClient({
  uri: '/graphql',
  cache: new InMemoryCache(),
});

// useQuery hook
const { data, loading, error } = useQuery(GET_USER, { variables: { id } });

// useMutation hook
const [createUser, { loading: saving }] = useMutation(CREATE_USER, {
  refetchQueries: [{ query: GET_USERS }],
});
```

### TanStack Query v5 (for REST)

```tsx
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query';

function UserList() {
  const { data, isLoading } = useQuery({
    queryKey: ['users'],
    queryFn: () => fetch('/api/users').then(r => r.json()),
    staleTime: 60_000,
  });

  const queryClient = useQueryClient();
  const { mutate } = useMutation({
    mutationFn: (newUser) => fetch('/api/users', { method: 'POST', body: JSON.stringify(newUser) }),
    onSuccess: () => queryClient.invalidateQueries({ queryKey: ['users'] }),
  });
}
```

---

## Micro-Frontends and Federated Modules

Micro-frontends decompose the UI into independently deployable fragments, enabling polyrepo teams to iterate without merge bottlenecks.

### Implementation with Module Federation

```js
// webpack.config.js — Host app
new ModuleFederationPlugin({
  name: 'shell',
  remotes: {
    checkout: 'checkout@https://checkout.example.com/remoteEntry.js',
    catalog: 'catalog@https://catalog.example.com/remoteEntry.js',
  },
  shared: { react: { singleton: true }, 'react-dom': { singleton: true } },
})
```

Key rules:
1. Share a single React singleton to prevent duplicate hook contexts.
2. Use an event bus or Zustand cross-app store for cross-fragment communication.
3. Each remote owns its routing segment.
4. Deploy remotes independently — host loads them at runtime.

---

## React Native Cross-Skill Insights

| Topic | React Web | React Native | Interview Note |
|---|---|---|---|
| Rendering target | DOM elements | Native UI views via JSI bridge | Must explain new architecture |
| Styling | CSS / Tailwind | `StyleSheet.create` objects | No cascade, no rem units |
| Navigation | `react-router` / Next.js | `@react-navigation/native` | Stack, Tab, Drawer navigators |
| Animations | CSS / Framer Motion | Reanimated 3 (UI thread) | Avoid JS-thread animations |
| Data fetching | TanStack Query / RSC | TanStack Query | Same API, different environment |
| **New Architecture** | N/A | JSI + Fabric + TurboModules | Replaces the old Bridge |

---

## Common Senior-Level Interview Scenarios

1. **Explain how React 19's `use()` hook differs from `useEffect` for data fetching.**
   - `use()` suspends the component and works with Suspense boundaries; `useEffect` runs after render and doesn't suspend.

2. **When would you choose Server Actions over a traditional REST API route?**
   - Server Actions eliminate the client ↔ server HTTP round-trip for mutations that originate from forms. Use REST APIs when you need a public endpoint consumed by multiple clients.

3. **How does the React Compiler change your approach to memoization?**
   - You stop writing manual `useMemo`/`useCallback`. The compiler inserts them where safe based on data-flow analysis. Code must follow the Rules of React for the compiler to apply optimizations.

4. **Design a chat app that streams messages using Server Components and Server Actions.**
   - Server Component renders static history (zero JS); `useOptimistic` shows optimistic sent messages; a Server Action persists to DB and triggers revalidation.

5. **Explain automatic batching differences between React 17 and React 18.**
   - React 17: batches only inside synchronous React event handlers. React 18: batches across `setTimeout`, Promises, and native events.

6. **Optimize a list of 100,000 rows with dynamic heights.**
   - `@tanstack/react-virtual` for windowing + `useDeferredValue` for filter input to stay responsive.

---

## High-Yield Comparative Tables

### React 18 vs React 19 Key Differences

| Feature | React 18 | React 19 |
|---|---|---|
| Data fetching in render | Not supported | `use(promise)` suspends |
| Form handling | Manual state + `fetch` | Server Actions + `useActionState` |
| Optimistic updates | Third-party libs | `useOptimistic` built-in |
| `forwardRef` | Required for ref props | Refs are plain props |
| Context provider | `<Context.Provider value>` | `<Context value>` |
| Document metadata | `react-helmet` / `next/head` | Native `<title>`, `<meta>` anywhere |
| Memoization | Manual `useMemo`/`useCallback` | React Compiler automates |
| Hydration errors | Confusing diff | Detailed diff with exact mismatch |

### Performance Optimization Checklist

| Technique | Dev Cost | Typical Win |
|---|---|---|
| React Compiler | One-time setup | 10–30% CPU |
| Code split per route | Medium | 30% bundle |
| Virtualize lists (`react-virtual`) | Medium | 90% fewer DOM nodes |
| Web Worker for heavy compute | High | 50% main-thread freed |
| `useDeferredValue` for inputs | Low | Smooth 60fps typing |
| RSC for static content | Varies | Zero JS shipped for that subtree |

### Security Matrix

| Risk | Severity | Fix |
|---|---|---|
| XSS via `dangerouslySetInnerHTML` | High | DOMPurify sanitization |
| Unauthenticated Server Actions | High | Auth check inside every action |
| Secrets in client bundle | Critical | Server-only environment variables |
| CSRF | Medium | SameSite cookies |
| Outdated dependencies | High | `npm audit` + Snyk CI |

---

## Mock Interview Questions and Quick Hints

| Question | Hint |
|---|---|
| How does `useLayoutEffect` differ from `useEffect`? | Fires synchronously before browser paint — use for DOM measurement |
| Why might Context cause "global" re-renders? | Provider value identity changes every render; memoize the value or split contexts |
| What does `dangerouslySetInnerHTML` risk? | Bypasses React's escaping; XSS if user content is inserted unsanitized |
| What is the React Compiler and when can't it optimize? | Auto-memoization tool; can't optimize code that violates the Rules of React |
| How do Server Actions handle authentication? | They don't automatically — you must call `auth()` / session check inside every action |
| What replaced `useFormState` in React 19? | `useActionState` (moved from `react-dom` to `react`) |
| Implement an accessible autocomplete | ARIA `combobox` role, `aria-controls`, `aria-activedescendant`, keyboard arrow nav |
| What is `useOptimistic`? | Hook that shows an optimistic (assumed success) UI update while an async action is in-flight; reverts on error |
| How does Turbopack differ from Webpack? | Rust-based, incremental compilation, orders of magnitude faster HMR in development |

---

## Final 8-Week Mastery Roadmap

| Week | Focus | Outcome |
|---|---|---|
| 1 | React 19 docs + build small RSC + Server Actions app | Fluency in new APIs |
| 2 | React Compiler setup, test auto-memoization with DevTools | Understand compiler limits |
| 3 | Redux Toolkit + RTK Query vs TanStack Query v5 | Global and server state mastery |
| 4 | SSR streaming, RSC, ISR in Next.js 15 | Server rendering skills |
| 5 | Profile with React DevTools + Lighthouse; fix real perf issues | Performance wins on a live app |
| 6 | Security: Server Action auth, Zod validation, XSS, `npm audit` | Ship production-hardened features |
| 7 | Testing: Vitest + RTL + Playwright E2E; aim for 90% coverage | Testing confidence |
| 8 | Mock interviews — system design + live coding + whiteboard | Interview readiness |

---

## Closing Thoughts

Mastering the advanced topics above — especially React 19's `use()`, Server Actions, `useOptimistic`, and the React Compiler — will equip you to discuss React internals, trade-offs, and architectural decisions with conviction. These are precisely the topics senior engineers are expected to discuss fluently in 2026 interviews. Combine conceptual clarity with hands-on experiments, and you will be ready to ace even the toughest React panels.
