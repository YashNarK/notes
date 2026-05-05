# React — Managing App State

## Table of contents

- [React — Managing App State](#react--managing-app-state)
  - [Table of contents](#table-of-contents)
  - [State Concepts](#state-concepts)
    - [What is State?](#what-is-state)
    - [Why Not Just Use a Variable?](#why-not-just-use-a-variable)
    - [State vs Props](#state-vs-props)
    - [What is Immutability?](#what-is-immutability)
    - [The Four Kinds of State](#the-four-kinds-of-state)
    - [The Prop-Drilling Problem](#the-prop-drilling-problem)
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

## State Concepts

### What is State?

**State** is any data that can change over time and whose change should cause the UI to update.

Examples:
- Is the menu open or closed? → `boolean`
- What has the user typed into a search box? → `string`
- Which items are in the shopping cart? → `array`
- Is the user logged in? → `User | null`

When state changes, React automatically re-renders the affected components to reflect the new data. This is React's core value proposition — you describe what the UI should look like for a given state, and React handles updating the DOM.

```
State changes  →  React re-renders  →  DOM updates  →  User sees new UI
```

### Why Not Just Use a Variable?

```tsx
// ❌ This does NOT work — React doesn't know the variable changed
function Counter() {
  let count = 0;
  return (
    <div>
      <p>{count}</p>
      <button onClick={() => { count++; }}>  {/* count changes, but no re-render */}
        Increment
      </button>
    </div>
  );
}

// ✅ This works — React watches `count` and re-renders when it changes
function Counter() {
  const [count, setCount] = useState(0);
  return (
    <div>
      <p>{count}</p>
      <button onClick={() => setCount(c => c + 1)}>Increment</button>
    </div>
  );
}
```

`useState` stores the value **outside** the component function, so it survives re-renders. Calling `setCount` tells React "this changed — please re-render."

### State vs Props

| | State | Props |
|---|---|---|
| Owned by | The component itself | The parent component |
| Mutable? | Yes — via setter function | No — read-only inside component |
| Changes trigger re-render? | Yes | Yes |
| Passed down? | Only if you pass it as a prop | Received from parent |

```tsx
function Parent() {
  const [name, setName] = useState('Alice'); // ← state lives here
  return <Child name={name} />;              // ← passed as prop
}

function Child({ name }: { name: string }) {
  // name is a prop — Child cannot change it directly
  return <p>Hello, {name}</p>;
}
```

### What is Immutability?

React uses **reference equality** to detect state changes — it checks if the new value is a different object in memory, not whether its contents changed.

**Mutating** (modifying in place) = same reference → React thinks nothing changed:
```tsx
// ❌ Mutating — React will NOT re-render
const [items, setItems] = useState(['a', 'b']);
items.push('c');     // modifies the array in place
setItems(items);     // same array reference → React skips re-render
```

**Immutable update** (new object/array) = new reference → React re-renders:
```tsx
// ✅ Immutable — React detects the change and re-renders
setItems([...items, 'c']);          // new array
setUser({ ...user, name: 'Bob' }); // new object
```

This is why tools like **Immer** exist — they let you write "mutating" code that is secretly immutable under the hood.

### The Four Kinds of State

Understanding what *type* of state you're dealing with determines which tool to use:

| Kind | Description | Examples | Tool |
|---|---|---|---|
| **Local** | Belongs to one component | open/closed, input value, active tab | `useState`, `useReducer` |
| **Shared** | Needed by several components in a subtree | theme, currently logged-in user | Context API |
| **Global** | Needed anywhere in the app | shopping cart, notification queue | Zustand |
| **Server** | Comes from an API; must stay in sync | user list, product catalogue | TanStack Query |

> **Key insight:** Server state is fundamentally different from client state. It's owned by the server, can change at any time, and needs caching, refetching, and synchronisation. Don't store it in Zustand — use TanStack Query.

### The Prop-Drilling Problem

**Prop drilling** happens when you pass a value through many intermediate components just to get it to a deeply nested child.

```
App (has user state)
└── Layout
    └── Sidebar
        └── UserMenu         ← needs user
            └── UserAvatar   ← needs user
```

```tsx
// Without a solution — every layer must pass `user` down
<Layout user={user}>
  <Sidebar user={user}>
    <UserMenu user={user}>
      <UserAvatar user={user} />   {/* finally! */}
```

This is hard to maintain. The solution is to use **Context API** or **Zustand** so `UserAvatar` can read the value directly without all the intermediate components caring about it.

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

---

## Pure Components & StrictMode

### Pure Components

A **pure function** always returns the same output for the same input. A **pure component** always returns the same JSX for the same props.

React relies on this contract to optimise rendering. If a component has side effects during render (e.g., mutating external variables), React cannot safely re-render it predictably.

```tsx
// ❌ Impure — mutates a variable outside the component
let count = 0;
const Message = () => {
  count++;  // side effect during render
  return <div>Message {count}</div>;
};
// In StrictMode renders: Message 2, Message 4, Message 6...

// ✅ Pure — all mutations are local to the render call
const Message = () => {
  let count = 0;
  count++;  // local, safe
  return <div>Message {count}</div>;
};
// Always renders: Message 1
```

**Rule:** during the render phase, only read props and state — never write to anything that existed before the render call.

### StrictMode

`<React.StrictMode>` intentionally renders each component **twice** in development (using only the second render's output) to surface impure components:

```tsx
// main.tsx
ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

- Only active in development — no performance cost in production.
- If your component renders differently on the first vs second call, you have an impurity.
- StrictMode also detects deprecated lifecycle usage and unexpected side effects in `useEffect`.

---

## Immutable Update Patterns

React uses **reference equality** to decide if state changed. You must always return a new object/array — never mutate in place.

### Updating a flat object

```tsx
const [drink, setDrink] = useState({ title: 'cola', price: 5 });

// ❌ Mutation — same reference, React skips re-render
drink.price = 6;
setDrink(drink);

// ✅ New object via spread
setDrink({ ...drink, price: 6 });
```

### Updating nested objects

`...` is **shallow** — it only copies the top-level keys. Nested objects still share the same reference unless explicitly spread:

```tsx
const [user, setUser] = useState({
  name: 'John',
  address: { street: 'Baker Street', city: 'London', zipCode: '94111' },
});

// ❌ Wrong — city and zipCode are lost
setUser({ ...user, address: { street: 'Church Street' } });

// ✅ Spread at every level that changes
setUser({ ...user, address: { ...user.address, street: 'Church Street' } });
```

> **Gotcha**: the deeper the nesting, the more verbose this becomes. Use **Immer** (see below) to avoid the boilerplate.

### Updating an array

```tsx
const [tags, setTags] = useState(['happy', 'cheerful']);

// Add
setTags([...tags, 'exciting']);

// Remove
setTags(tags.filter(tag => tag !== 'cheerful'));

// Update one element
setTags(tags.map(tag => (tag === 'happy' ? 'happiness' : tag)));
```

### Updating an array of objects

```tsx
const [bugs, setBugs] = useState([
  { id: 1, name: 'UI Glitch',              fixed: false },
  { id: 2, name: 'Biometric login failed', fixed: false },
]);

// Fix bug 1
setBugs(bugs.map(bug => (bug.id === 1 ? { ...bug, fixed: true } : bug)));

// Delete bug 2
setBugs(bugs.filter(bug => bug.id !== 2));
```

---

## Immer — Simplifying Immutable Updates

When state is deeply nested, spread-based updates become verbose and error-prone. **Immer** lets you write apparently-mutating code that is actually safe: it records changes against a *draft* and produces a new immutable object.

```bash
npm i immer use-immer
```

### How Immer Works

Immer wraps your mutable code in a `produce()` call. Inside the recipe function you receive a **draft proxy** — all changes to the draft are recorded, and Immer produces a new immutable value. The original object is never touched.

### Without Immer vs with Immer

```tsx
const baseState = [
  { title: 'Learn TypeScript', done: true },
  { title: 'Try Immer',        done: false },
];

// ❌ Without Immer — shallow clone at every level manually
const next = baseState.slice();
next[1] = { ...next[1], done: true };
next.push({ title: 'Tweet about it', done: false });

// ✅ With Immer — write mutations directly against the draft
import { produce } from 'immer';

const next = produce(baseState, draft => {
  draft[1].done = true;
  draft.push({ title: 'Tweet about it', done: false });
});
```

### useState + Immer (`useImmer`)

```tsx
import { useImmer } from 'use-immer';

interface Todo { id: string; title: string; done: boolean; }

function TodoList() {
  const [todos, setTodos] = useImmer<Todo[]>([
    { id: 'react', title: 'Learn React', done: true  },
    { id: 'immer', title: 'Try Immer',  done: false },
  ]);

  const toggle = (id: string) => setTodos(draft => {
    const todo = draft.find(t => t.id === id)!;
    todo.done = !todo.done;  // looks like mutation — Immer makes it safe
  });

  const add = (title: string) => setTodos(draft => {
    draft.push({ id: crypto.randomUUID(), title, done: false });
  });

  return (
    <ul>
      {todos.map(t => (
        <li key={t.id} onClick={() => toggle(t.id)}
            style={{ textDecoration: t.done ? 'line-through' : 'none' }}>
          {t.title}
        </li>
      ))}
      <button onClick={() => add('New task')}>Add</button>
    </ul>
  );
}
```

### useReducer + Immer (`useImmerReducer`)

```tsx
import { useImmerReducer } from 'use-immer';

type Todo = { id: string; title: string; done: boolean };
type Action =
  | { type: 'toggle'; id: string }
  | { type: 'add';    title: string };

function todoReducer(draft: Todo[], action: Action) {
  switch (action.type) {
    case 'toggle': {
      const todo = draft.find(t => t.id === action.id)!;
      todo.done = !todo.done;  // direct mutation on draft — safe
      break;
    }
    case 'add':
      draft.push({ id: crypto.randomUUID(), title: action.title, done: false });
      break;
  }
}

function TodoList() {
  const [todos, dispatch] = useImmerReducer(todoReducer, []);
  // ...
}
```

> **When to use Immer**: any state that is a nested object or array with more than 2 levels of depth. For flat state, plain spread is fine.

---

## Multi-Context Pattern

Real apps often need multiple independent contexts (e.g., game filters + page number). The pattern is: create a context file per concern, wrap providers in `App.tsx`, consume via `useContext` in leaf components.

```ts
// src/contexts/gameQueryContext.ts
import { createContext, type Dispatch, type SetStateAction } from 'react';
import type { GameQuery } from '../types';

interface GameQueryContextType {
  gameQuery: GameQuery;
  setGameQuery: Dispatch<SetStateAction<GameQuery>>;
}
export const GameQueryContext = createContext<GameQueryContextType>({} as GameQueryContextType);
```

```ts
// src/contexts/pageNumberContext.ts
import { createContext, type Dispatch, type SetStateAction } from 'react';

interface PageNumberContextType {
  pageNumber: number;
  setPageNumber: Dispatch<SetStateAction<number>>;
}
export const PageNumberContext = createContext<PageNumberContextType>({} as PageNumberContextType);
```

```tsx
// App.tsx — compose providers
import { useState } from 'react';
import { GameQueryContext } from './contexts/gameQueryContext';
import { PageNumberContext } from './contexts/pageNumberContext';

function App() {
  const [pageNumber, setPageNumber] = useState(1);
  const [gameQuery, setGameQuery] = useState<GameQuery>({ page: 1 } as GameQuery);

  return (
    <GameQueryContext.Provider value={{ gameQuery, setGameQuery }}>
      <PageNumberContext.Provider value={{ pageNumber, setPageNumber }}>
        <OrderSelector />
        <PlatformSelector />
      </PageNumberContext.Provider>
    </GameQueryContext.Provider>
  );
}
```

```tsx
// PlatformSelector.tsx — consume both contexts
import { useContext } from 'react';
import { PageNumberContext } from '../../contexts/pageNumberContext';
import { GameQueryContext } from '../../contexts/gameQueryContext';

const PlatformSelector = () => {
  const { setPageNumber } = useContext(PageNumberContext);
  const { gameQuery, setGameQuery } = useContext(GameQueryContext);

  const handleSelect = (platform: Platform | null) => {
    setGameQuery({ ...gameQuery, platform, page: 1 });
    setPageNumber(1);
  };
  // ...
};
```

### Custom Provider (encapsulating state inside the context file)

Move state + provider into the context file so `App.tsx` stays clean:

```tsx
// src/contexts/SomeContext.tsx
import { createContext, useContext, useState, type ReactNode } from 'react';

interface SomeContextType { value: string; setValue: (v: string) => void; }
const SomeContext = createContext<SomeContextType | null>(null);

export function SomeProvider({ children }: { children: ReactNode }) {
  const [value, setValue] = useState('');
  return <SomeContext.Provider value={{ value, setValue }}>{children}</SomeContext.Provider>;
}

// Guard hook — throws a clear error when used outside the provider
export function useSomeContext() {
  const ctx = useContext(SomeContext);
  if (!ctx) throw new Error('useSomeContext must be used inside <SomeProvider>');
  return ctx;
}
```

```tsx
// App.tsx — clean, no internal state logic
import { SomeProvider } from './contexts/SomeContext';
export default function App() {
  return <SomeProvider><MyComponent /></SomeProvider>;
}
```

### Notes on React Context

- **Minimise renders**: split one large context into multiple small ones, each with a single responsibility. All consumers of a context re-render when *any* value in it changes.
- **When to use Context**: low-frequency global values — theme, locale, auth user. Do NOT use Context for server state (use TanStack Query) or high-frequency updates (use Zustand).
- **Alternatives**: Redux, MobX, Recoil, xState, Zustand (simplest for client state).

---

## Zustand — Preventing Unnecessary Re-renders

By default Zustand compares selected values by `Object.is`. When you select multiple values at once in an object literal, the object reference is always new → always re-renders. Use **selectors** + **`useShallow`** to fix this:

```tsx
import { useShallow } from 'zustand/react/shallow';
import { useCountStore } from './stores/countStore';

// ❌ New object on every render → always re-renders
const { bears, increment } = useCountStore(
  state => ({ bears: state.bears, increment: state.increment })
);

// ✅ Shallow comparison — only re-renders when bears or increment actually change
const { bears, increment } = useCountStore(
  useShallow(state => ({ bears: state.bears, increment: state.increment }))
);
```

**Architecture decision guide:**
| Scenario | Recommendation |
|---|---|
| Reusable, composable components | props + `useReducer` (portable) |
| App-specific components | Zustand store (simpler) |
| Frequently changing values | Zustand (not Context) |
| Server / async data | TanStack Query |
| Cross-framework portability | Redux (worth complexity only then) |

---

## Zustand DevTools

Use `simple-zustand-devtools` to inspect store state in the browser during development:

```bash
npm i simple-zustand-devtools
```

```ts
// stores/countStore.ts
import { create } from 'zustand';
import { mountStoreDevtool } from 'simple-zustand-devtools';

interface CountStore {
  count: number;
  increment: () => void;
  decrement: () => void;
}

const useCountStore = create<CountStore>(set => ({
  count: 0,
  increment: () => set(state => ({ count: state.count + 1 })),
  decrement: () => set(state => ({ count: state.count - 1 })),
}));

// Mount devtools only in development
if (process.env.NODE_ENV === 'development') {
  mountStoreDevtool('Counter Store', useCountStore);
}

export default useCountStore;
```

The panel appears in React DevTools under the name you pass — showing live store state and allowing state inspection.
