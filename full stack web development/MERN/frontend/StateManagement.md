# Advanced State Management

## Table of contents

- [Advanced State Management](#advanced-state-management)
  - [Table of contents](#table-of-contents)
  - [When to Use What](#when-to-use-what)
  - [React Context API](#react-context-api)
  - [Zustand](#zustand)
  - [Jotai](#jotai)
  - [MobX](#mobx)
  - [Recoil](#recoil)
  - [Redux Toolkit (Quick Reference)](#redux-toolkit-quick-reference)
  - [React Query / TanStack Query](#react-query--tanstack-query)
  - [Comparison Table](#comparison-table)
  - [Choosing the Right Tool](#choosing-the-right-tool)

---

## When to Use What

Not all state is the same. Categorizing state helps choose the right solution:

| State Type | Examples | Best Tool |
|---|---|---|
| **Local UI state** | open/close modal, form input | `useState` |
| **Shared local state** | sibling component communication | Lift state / Context |
| **Global UI state** | theme, locale, sidebar open | Context or Zustand |
| **Server/async state** | API responses, loading, cache | React Query / SWR |
| **Complex domain state** | shopping cart, multi-step form | Zustand, Redux Toolkit |
| **Atomic/derived state** | computed values, filters | Jotai, Recoil |
| **Observable/reactive state** | real-time data, reactive objects | MobX |

---

## React Context API

Built into React — no extra library needed. Best for low-frequency global state (theme, locale, auth user).

```tsx
// 1. Create context with a typed default
import { createContext, useContext, useState, ReactNode } from 'react';

type Theme = 'light' | 'dark';
interface ThemeContextType {
  theme: Theme;
  toggleTheme: () => void;
}

const ThemeContext = createContext<ThemeContextType | null>(null);

// 2. Create a custom hook for safe access
export function useTheme() {
  const ctx = useContext(ThemeContext);
  if (!ctx) throw new Error('useTheme must be used within ThemeProvider');
  return ctx;
}

// 3. Create the Provider
export function ThemeProvider({ children }: { children: ReactNode }) {
  const [theme, setTheme] = useState<Theme>('light');
  const toggleTheme = () => setTheme(t => t === 'light' ? 'dark' : 'light');

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

// 4. Use anywhere in the tree
function Header() {
  const { theme, toggleTheme } = useTheme();
  return <button onClick={toggleTheme}>Current: {theme}</button>;
}
```

**Performance — prevent unnecessary re-renders:**
```tsx
// Split contexts for unrelated state (avoid bundling fast + slow changing state)
<UserProvider>
  <ThemeProvider>
    <App />
  </ThemeProvider>
</UserProvider>

// Use useMemo for the context value
const value = useMemo(() => ({ theme, toggleTheme }), [theme]);
<ThemeContext.Provider value={value}>

// Use React.memo on consumers that only need stable parts
const ThemedButton = React.memo(function ThemedButton({ onClick, label }) {
  const { theme } = useTheme();
  return <button className={theme} onClick={onClick}>{label}</button>;
});
```

**Limitation:** Every consumer re-renders when context value changes, even if they use only a small part of it. For frequent updates (e.g., mouse position), use Zustand or Jotai instead.

---

## Zustand

Lightweight (~1KB), unopinionated, flexible global state. Minimal boilerplate.

```bash
npm install zustand
```

**Basic store:**
```tsx
import { create } from 'zustand';

interface CounterStore {
  count: number;
  increment: () => void;
  decrement: () => void;
  reset: () => void;
}

const useCounter = create<CounterStore>((set) => ({
  count: 0,
  increment: () => set(state => ({ count: state.count + 1 })),
  decrement: () => set(state => ({ count: state.count - 1 })),
  reset: () => set({ count: 0 }),
}));

// Use in components
function Counter() {
  const count = useCounter(state => state.count);         // only re-renders when count changes
  const increment = useCounter(state => state.increment); // stable function reference

  return <button onClick={increment}>{count}</button>;
}
```

**Async actions and side effects:**
```tsx
interface UserStore {
  user: User | null;
  loading: boolean;
  error: string | null;
  fetchUser: (id: string) => Promise<void>;
}

const useUserStore = create<UserStore>((set) => ({
  user: null,
  loading: false,
  error: null,
  fetchUser: async (id) => {
    set({ loading: true, error: null });
    try {
      const user = await api.getUser(id);
      set({ user, loading: false });
    } catch (err) {
      set({ error: err.message, loading: false });
    }
  },
}));
```

**Slices pattern (large stores):**
```tsx
const useStore = create<BearSlice & FishSlice>((...a) => ({
  ...createBearSlice(...a),
  ...createFishSlice(...a),
}));
```

**Middleware:**
```tsx
import { persist, devtools, immer } from 'zustand/middleware';

const useStore = create(
  devtools(
    persist(
      immer((set) => ({
        cart: [],
        addItem: (item) => set(state => { state.cart.push(item); }),
      })),
      { name: 'cart-storage' }   // persists to localStorage
    )
  )
);
```

---

## Jotai

Atomic state model — compose state from small pieces (**atoms**). Similar to Recoil but simpler.

```bash
npm install jotai
```

```tsx
import { atom, useAtom, useAtomValue, useSetAtom } from 'jotai';

// 1. Define atoms (primitives)
const countAtom = atom(0);
const nameAtom = atom('Alice');

// 2. Derived atoms (read-only)
const doubledAtom = atom((get) => get(countAtom) * 2);

// 3. Writable derived atoms
const uppercaseNameAtom = atom(
  (get) => get(nameAtom).toUpperCase(),
  (get, set, value: string) => set(nameAtom, value.toLowerCase())
);

// 4. Async atoms
const userAtom = atom(async (get) => {
  const id = get(userIdAtom);
  const res = await fetch(`/api/users/${id}`);
  return res.json();
});

// Usage
function Counter() {
  const [count, setCount] = useAtom(countAtom);
  const doubled = useAtomValue(doubledAtom);      // read-only
  const setCount2 = useSetAtom(countAtom);        // write-only (no re-render from read)

  return (
    <div>
      <p>Count: {count}, Doubled: {doubled}</p>
      <button onClick={() => setCount(c => c + 1)}>+</button>
    </div>
  );
}
```

**atomWithStorage (persistence):**
```tsx
import { atomWithStorage } from 'jotai/utils';
const themeAtom = atomWithStorage('theme', 'light');
```

**Why Jotai?**
- Fine-grained re-renders (only atoms that changed cause re-render)
- No boilerplate (no reducers, no actions, no selectors)
- Colocate state with components (like `useState` but shareable)
- Works great with Suspense

---

## MobX

Observable state management — state is **reactive**. Mutations trigger automatic re-renders. Object-oriented approach.

```bash
npm install mobx mobx-react-lite
```

```tsx
import { makeAutoObservable, runInAction } from 'mobx';
import { observer } from 'mobx-react-lite';

class CartStore {
  items: CartItem[] = [];
  loading = false;

  constructor() {
    makeAutoObservable(this);   // auto-observes all properties and methods
  }

  // Action — modifies state
  addItem(item: CartItem) {
    this.items.push(item);
  }

  removeItem(id: string) {
    this.items = this.items.filter(i => i.id !== id);
  }

  // Computed value — auto-derived, cached
  get total() {
    return this.items.reduce((sum, item) => sum + item.price * item.qty, 0);
  }

  get itemCount() {
    return this.items.length;
  }

  // Async action
  async fetchCart() {
    this.loading = true;
    try {
      const items = await api.getCart();
      runInAction(() => {            // mutations inside async must be wrapped
        this.items = items;
        this.loading = false;
      });
    } catch {
      runInAction(() => { this.loading = false; });
    }
  }
}

const cart = new CartStore();

// observer() makes component re-render when observed state changes
const CartSummary = observer(() => {
  return (
    <div>
      <p>Items: {cart.itemCount}</p>
      <p>Total: ${cart.total}</p>
      <button onClick={() => cart.fetchCart()}>Refresh</button>
    </div>
  );
});
```

**React Context with MobX:**
```tsx
const CartContext = createContext<CartStore | null>(null);
const useCart = () => useContext(CartContext)!;

function App() {
  const [store] = useState(() => new CartStore());
  return (
    <CartContext.Provider value={store}>
      <CartSummary />
    </CartContext.Provider>
  );
}
```

---

## Recoil

Facebook's atom-based state library for React. More feature-rich than Jotai but heavier.

```bash
npm install recoil
```

```tsx
import { atom, selector, useRecoilState, useRecoilValue, RecoilRoot } from 'recoil';

// Atom
const textAtom = atom({ key: 'text', default: '' });

// Selector (derived state)
const charCountSelector = selector({
  key: 'charCount',
  get: ({ get }) => get(textAtom).length,
});

// Async selector
const currentUserSelector = selector({
  key: 'currentUser',
  get: async ({ get }) => {
    const id = get(userIdAtom);
    const res = await fetch(`/api/users/${id}`);
    return res.json();
  },
});

function TextInput() {
  const [text, setText] = useRecoilState(textAtom);
  const count = useRecoilValue(charCountSelector);
  return (
    <div>
      <input value={text} onChange={e => setText(e.target.value)} />
      <p>Characters: {count}</p>
    </div>
  );
}

// Wrap app with RecoilRoot
<RecoilRoot><App /></RecoilRoot>
```

---

## Redux Toolkit (Quick Reference)

For complex, large-scale app state. See `Redux.md` and `ReduxPatterns.md` for full notes.

```tsx
// Slice
const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: state => { state.value += 1; },
    decrement: state => { state.value -= 1; },
  },
});

// Async thunk
export const fetchUser = createAsyncThunk('user/fetch', async (id: string) => {
  const res = await fetch(`/api/users/${id}`);
  return res.json();
});

// RTK Query — data fetching + caching
const api = createApi({
  baseQuery: fetchBaseQuery({ baseUrl: '/api' }),
  endpoints: (build) => ({
    getUser: build.query<User, string>({ query: (id) => `/users/${id}` }),
    updateUser: build.mutation<User, Partial<User>>({
      query: (user) => ({ url: `/users/${user.id}`, method: 'PATCH', body: user }),
    }),
  }),
});
```

---

## React Query / TanStack Query

Specialized for **server state** — fetching, caching, synchronizing, and updating data from APIs.

```bash
npm install @tanstack/react-query
```

```tsx
import { useQuery, useMutation, useQueryClient, QueryClient, QueryClientProvider } from '@tanstack/react-query';

// Setup
const queryClient = new QueryClient();
<QueryClientProvider client={queryClient}><App /></QueryClientProvider>

// Fetching data
function UserProfile({ id }: { id: string }) {
  const { data: user, isLoading, isError, error } = useQuery({
    queryKey: ['users', id],
    queryFn: () => fetch(`/api/users/${id}`).then(r => r.json()),
    staleTime: 5 * 60 * 1000,   // consider fresh for 5 minutes
    gcTime: 10 * 60 * 1000,     // keep in cache for 10 minutes
  });

  if (isLoading) return <Spinner />;
  if (isError) return <Error message={error.message} />;
  return <div>{user.name}</div>;
}

// Mutations
function EditUser({ user }) {
  const queryClient = useQueryClient();

  const mutation = useMutation({
    mutationFn: (updatedUser) =>
      fetch(`/api/users/${updatedUser.id}`, {
        method: 'PATCH',
        body: JSON.stringify(updatedUser),
      }).then(r => r.json()),
    onSuccess: (data) => {
      // Update cached data
      queryClient.setQueryData(['users', data.id], data);
      // Or invalidate to refetch
      queryClient.invalidateQueries({ queryKey: ['users'] });
    },
  });

  return (
    <button onClick={() => mutation.mutate({ ...user, name: 'New Name' })}>
      {mutation.isPending ? 'Saving...' : 'Save'}
    </button>
  );
}

// Prefetching
await queryClient.prefetchQuery({
  queryKey: ['users', id],
  queryFn: () => fetchUser(id),
});

// Infinite queries (pagination)
const { data, fetchNextPage, hasNextPage } = useInfiniteQuery({
  queryKey: ['posts'],
  queryFn: ({ pageParam = 1 }) =>
    fetch(`/api/posts?page=${pageParam}`).then(r => r.json()),
  getNextPageParam: (lastPage) => lastPage.nextPage,
});
```

---

## Comparison Table

| Library | Size | Boilerplate | Re-renders | Async | DevTools | Best For |
|---|---|---|---|---|---|---|
| Context API | 0KB | Low | On any change | Manual | ❌ | Theme, auth, locale |
| Zustand | ~1KB | Very Low | Selector-based | Built-in | ✅ | General global state |
| Jotai | ~3KB | Minimal | Per-atom | Suspense | ✅ | Atomic/derived state |
| MobX | ~22KB | Medium | Auto (reactive) | Built-in | ✅ | OOP, reactive systems |
| Recoil | ~79KB | Medium | Per-atom | Suspense | ✅ | Complex atom graphs |
| Redux Toolkit | ~40KB | Medium | Selector-based | Thunk/RTK Query | ✅ | Large-scale, teams |
| React Query | ~13KB | Low | Query-based | Specialized | ✅ | Server/async state |

---

## Choosing the Right Tool

```
Need to share state between 2-3 components?
  → Lift state up to common parent

Sharing across many components, low update frequency?
  → React Context API

Global UI state (theme, sidebar), moderate complexity?
  → Zustand

Fine-grained, atomic, or heavily derived state?
  → Jotai

Object-oriented domain model, reactive updates?
  → MobX

Server data, API caching, background sync?
  → React Query / TanStack Query

Large team, complex app, need time-travel debugging?
  → Redux Toolkit
```
