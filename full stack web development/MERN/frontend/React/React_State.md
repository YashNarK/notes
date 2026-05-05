# React — Managing App State

State management covers local component state, shared cross-component state, and server-synchronised state.

## Table of contents

- [React — Managing App State](#react--managing-app-state)
  - [Table of contents](#table-of-contents)
  - [Plain React](#plain-react)
    - [Local State — useState](#local-state--usestate)
    - [Derived \& Computed State](#derived--computed-state)
    - [Complex Local State — useReducer](#complex-local-state--usereducer)
    - [Shared State — Context API](#shared-state--context-api)
    - [Global State — Zustand (Recommended)](#global-state--zustand-recommended)
    - [Server State — TanStack Query](#server-state--tanstack-query)
    - [Choosing the Right Tool](#choosing-the-right-tool)
  - [Next.js](#nextjs)
    - [Server vs Client State](#server-vs-client-state)
    - [Using Zustand with Next.js](#using-zustand-with-nextjs)
    - [URL as State](#url-as-state)

---

## Plain React

### Local State — useState

Use for UI state that belongs to a single component.

```tsx
import { useState } from 'react';

export function Counter() {
  const [count, setCount] = useState(0);
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(c => c + 1)}>+1</button>
    </div>
  );
}
```

> **Gotcha**: State updates are asynchronous and batched. Always use the functional form `setCount(c => c + 1)` when the new value depends on the previous one.

### Derived & Computed State

Compute values from existing state instead of creating redundant state.

```tsx
const [items, setItems] = useState<string[]>([]);
// ✅ Derive — do not store filteredItems in state
const filteredItems = items.filter(item => item.startsWith('A'));
```

### Complex Local State — useReducer

Use when state transitions are complex or multiple sub-values change together.

```tsx
type Action =
  | { type: 'increment' }
  | { type: 'decrement' }
  | { type: 'reset'; payload: number };

function reducer(state: number, action: Action): number {
  switch (action.type) {
    case 'increment': return state + 1;
    case 'decrement': return state - 1;
    case 'reset':     return action.payload;
    default:          return state;
  }
}

export function Counter() {
  const [count, dispatch] = useReducer(reducer, 0);
  return (
    <div>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
      <span>{count}</span>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'reset', payload: 0 })}>Reset</button>
    </div>
  );
}
```

### Shared State — Context API

Share state across a component subtree without prop-drilling. Best for low-frequency updates (theme, locale, auth).

```tsx
import { createContext, useContext, useState } from 'react';

interface ThemeContextType {
  theme: 'light' | 'dark';
  toggle: () => void;
}

const ThemeContext = createContext<ThemeContextType | null>(null);

export function ThemeProvider({ children }: { children: React.ReactNode }) {
  const [theme, setTheme] = useState<'light' | 'dark'>('light');
  const toggle = () => setTheme(t => (t === 'light' ? 'dark' : 'light'));
  return (
    <ThemeContext.Provider value={{ theme, toggle }}>
      {children}
    </ThemeContext.Provider>
  );
}

export function useTheme() {
  const ctx = useContext(ThemeContext);
  if (!ctx) throw new Error('useTheme must be used within ThemeProvider');
  return ctx;
}
```

> **Gotcha**: Every consumer re-renders when the context value changes. Split contexts by update frequency or use `useMemo` on the value object.

### Global State — Zustand (Recommended)

Zustand provides global state with minimal boilerplate and no Provider wrapping.

```bash
npm i zustand
```

```tsx
import { create } from 'zustand';

interface CartStore {
  items: string[];
  addItem: (item: string) => void;
  removeItem: (item: string) => void;
  clear: () => void;
}

export const useCartStore = create<CartStore>(set => ({
  items: [],
  addItem:    item => set(state => ({ items: [...state.items, item] })),
  removeItem: item => set(state => ({ items: state.items.filter(i => i !== item) })),
  clear:      ()   => set({ items: [] }),
}));

// Usage — components only re-render when selected slice changes
function CartCount() {
  const count = useCartStore(state => state.items.length);
  return <span>Items: {count}</span>;
}
```

**Persist to localStorage** with the `persist` middleware:

```tsx
import { create } from 'zustand';
import { persist } from 'zustand/middleware';

export const useCartStore = create<CartStore>()(
  persist(
    set => ({ items: [], addItem: item => set(s => ({ items: [...s.items, item] })), clear: () => set({ items: [] }) }),
    { name: 'cart-storage' }
  )
);
```

### Server State — TanStack Query

Keep server-originated data (API responses) out of global state entirely. See [React_HTTP.md](React_HTTP.md) for full coverage.

```tsx
// Server state belongs in TanStack Query, NOT Zustand
const { data: users } = useQuery({ queryKey: ['users'], queryFn: fetchUsers });
```

### Choosing the Right Tool

| Use case | Tool |
|---|---|
| Local UI state (toggle, input) | `useState` |
| Complex state with actions | `useReducer` |
| Theme / locale / auth | Context API |
| Global client state (cart, preferences) | Zustand |
| Server data (API responses) | TanStack Query |
| Cross-tab synchronisation | Zustand + `persist` middleware |

---

## Next.js

### Server vs Client State

In Next.js, prefer **moving state to the server** where possible:

- Page-level data → fetch in Server Components (no state at all)
- User-specific data → pass as `props` from Server to Client components
- Interactive/ephemeral state → `useState` in `'use client'` components

```tsx
// app/dashboard/page.tsx — Server Component, no state needed
export default async function Dashboard() {
  const stats = await fetchStats(); // runs on server
  return <StatsDisplay stats={stats} />;
}
```

### Using Zustand with Next.js

Zustand stores are singletons. In SSR, each request must get a fresh store to avoid cross-request state leakage. Use the **store factory** pattern:

```tsx
// lib/store.ts
import { createStore } from 'zustand';

export type CartState = { items: string[]; addItem: (item: string) => void };

export const createCartStore = () =>
  createStore<CartState>(set => ({
    items: [],
    addItem: item => set(s => ({ items: [...s.items, item] })),
  }));
```

```tsx
// providers/CartProvider.tsx
'use client';
import { createContext, useContext, useRef } from 'react';
import { useStore } from 'zustand';
import { createCartStore, CartState } from '@/lib/store';

type CartStore = ReturnType<typeof createCartStore>;
const CartStoreContext = createContext<CartStore | null>(null);

export function CartProvider({ children }: { children: React.ReactNode }) {
  const storeRef = useRef<CartStore>();
  if (!storeRef.current) storeRef.current = createCartStore();
  return <CartStoreContext.Provider value={storeRef.current}>{children}</CartStoreContext.Provider>;
}

export function useCartStore<T>(selector: (state: CartState) => T) {
  const store = useContext(CartStoreContext);
  if (!store) throw new Error('Missing CartProvider');
  return useStore(store, selector);
}
```

### URL as State

The Next.js App Router makes URL search params a first-class state mechanism for shareable, bookmarkable UI state.

```tsx
'use client';
import { useSearchParams, useRouter, usePathname } from 'next/navigation';

export function FilterBar() {
  const searchParams = useSearchParams();
  const router = useRouter();
  const pathname = usePathname();

  const setFilter = (key: string, value: string) => {
    const params = new URLSearchParams(searchParams.toString());
    params.set(key, value);
    router.replace(`${pathname}?${params.toString()}`);
  };

  return (
    <select onChange={e => setFilter('status', e.target.value)}>
      <option value="all">All</option>
      <option value="active">Active</option>
      <option value="done">Done</option>
    </select>
  );
}
```
