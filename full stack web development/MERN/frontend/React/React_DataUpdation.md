# React — Data Updation

## Table of contents

- [React — Data Updation](#react--data-updation)
  - [Table of contents](#table-of-contents)
  - [Data Updation Concepts](#data-updation-concepts)
    - [What is a Mutation?](#what-is-a-mutation)
    - [CRUD Operations](#crud-operations)
    - [The Read/Write Lifecycle](#the-readwrite-lifecycle)
    - [Immutability Revisited](#immutability-revisited)
    - [What is an Optimistic Update?](#what-is-an-optimistic-update)
    - [The Update vs Refetch Decision](#the-update-vs-refetch-decision)
  - [Plain React](#plain-react)
    - [Immutable Updates — The Problem](#immutable-updates--the-problem)
    - [Immer — Write Mutating Code, Get Immutable Updates](#immer--write-mutating-code-get-immutable-updates)
    - [Immer with useReducer](#immer-with-usereducer)
    - [Immer with Zustand](#immer-with-zustand)
    - [Server Mutations with TanStack Query](#server-mutations-with-tanstack-query)
    - [Optimistic Updates](#optimistic-updates)
  - [Next.js](#nextjs)
    - [Server Actions for Mutations](#server-actions-for-mutations)
    - [Cache Revalidation After Mutations](#cache-revalidation-after-mutations)
    - [Optimistic UI with useOptimistic](#optimistic-ui-with-useoptimistic)
    - [Combining Immer with Server Actions](#combining-immer-with-server-actions)

---

## Data Updation Concepts

### What is a Mutation?

In software, a **mutation** is any operation that changes data — as opposed to a **query**, which only reads data.

```
Query   → GET  /api/todos        → reads a list, nothing changes
Mutation → POST  /api/todos       → creates a new todo
Mutation → PATCH /api/todos/42    → updates todo 42
Mutation → DELETE /api/todos/42   → deletes todo 42
```

TanStack Query distinguishes between `useQuery` (for reads) and `useMutation` (for writes) for this reason. Each has different behaviour — queries cache and auto-refetch; mutations don't cache and fire on demand.

### CRUD Operations

**CRUD** stands for Create, Read, Update, Delete — the four fundamental data operations:

| Operation | HTTP Method | SQL | Example |
|---|---|---|---|
| **C**reate | POST | INSERT | Add a new user |
| **R**ead | GET | SELECT | Fetch user list |
| **U**pdate | PUT / PATCH | UPDATE | Change user's name |
| **D**elete | DELETE | DELETE | Remove a user |

`PUT` replaces the entire resource. `PATCH` updates only the fields you send:

```
PUT  /users/42  { name: "Bob", email: "bob@x.com", role: "admin" }  ← full replace
PATCH /users/42 { name: "Bob" }                                      ← partial update
```

### The Read/Write Lifecycle

After a successful mutation, the UI must reflect the new data. There are two strategies:

**Refetch** — invalidate the cache and re-fetch from the server:
```
User deletes todo → DELETE /api/todos/42 → invalidate 'todos' cache → GET /api/todos → re-render
```

**Cache update** — update the local cache directly without a second network request:
```
User deletes todo → DELETE /api/todos/42 → remove item from TanStack Query cache → re-render
```

Refetch is simpler but slower. Cache update is instant but can go stale if other users also change data.

### Immutability Revisited

React tracks state changes by comparing **object references** (memory addresses), not object contents. This means you must always create a **new** object/array when updating state.

```ts
// Both are arrays with the same values, but different references
const a = [1, 2, 3];
const b = [...a];    // spread creates a new array

a === b  // false — different references ← React detects a change
a === a  // true  — same reference      ← React thinks nothing changed
```

For deeply nested objects, creating new references at every level by hand becomes messy:

```ts
// Updating a city deep inside a nested object
setState({
  ...state,
  user: {
    ...state.user,
    profile: {
      ...state.user.profile,
      address: {
        ...state.user.profile.address,
        city: 'London',   // ← the actual change
      }
    }
  }
});
```

**Immer** solves this by letting you write direct mutations while producing a new reference internally.

### What is an Optimistic Update?

An **optimistic update** is when you immediately show the expected result in the UI *before* the server confirms the change.

```
Without optimistic update:
  User clicks "Like" → button stays un-liked → wait 300ms → server responds → button shows liked
  (feels sluggish)

With optimistic update:
  User clicks "Like" → button shows liked instantly → server confirms → done
                                                    ↓ (or) server fails → revert to un-liked
  (feels instant)
```

The trade-off: if the server rejects the mutation, you must **roll back** the optimistic change. This requires saving a snapshot of the previous state.

### The Update vs Refetch Decision

| Strategy | When to use | Trade-off |
|---|---|---|
| **Refetch** | When accuracy matters; multi-user data | Slower, always fresh |
| **Cache update** | Single-user data; you trust the response | Faster, may be stale |
| **Optimistic update** | High-frequency actions (like, follow, toggle) | Best UX, needs rollback logic |

---

## Plain React

### Immutable Updates — The Problem

React only re-renders when it detects a new object reference. Mutating state in-place causes silent bugs.

```tsx
// ❌ Wrong — mutating state directly, React will not re-render
const [todos, setTodos] = useState([{ id: 1, done: false }]);
todos[0].done = true;       // mutation!
setTodos(todos);            // same reference, no re-render

// ✅ Correct — create a new array with spread
setTodos(todos.map(t => t.id === 1 ? { ...t, done: true } : t));
```

Spread-based updates become verbose for deeply nested objects.

### Immer — Write Mutating Code, Get Immutable Updates

Immer lets you write "mutating" logic that Immer converts to an immutable update internally.

```bash
npm i immer
```

```tsx
import { produce } from 'immer';

interface Todo { id: number; text: string; done: boolean; }

const [todos, setTodos] = useState<Todo[]>([
  { id: 1, text: 'Learn React', done: false },
  { id: 2, text: 'Learn Immer', done: false },
]);

// Toggle done — Immer makes this as simple as mutating
const toggleTodo = (id: number) =>
  setTodos(produce(draft => {
    const todo = draft.find(t => t.id === id);
    if (todo) todo.done = !todo.done;
  }));

// Deeply nested update — far cleaner than manual spread
interface State { user: { profile: { address: { city: string } } } }

const [state, setState] = useState<State>({ user: { profile: { address: { city: 'Paris' } } } });

const updateCity = (city: string) =>
  setState(produce(draft => {
    draft.user.profile.address.city = city; // direct mutation, Immer handles immutability
  }));
```

### Immer with useReducer

Use `produce` as the reducer function for clean, readable action handlers:

```tsx
import { useReducer } from 'react';
import { produce, Draft } from 'immer';

interface CartItem { id: number; qty: number; }
type CartState = { items: CartItem[] };
type CartAction =
  | { type: 'increment'; id: number }
  | { type: 'remove'; id: number }
  | { type: 'clear' };

const cartReducer = produce((draft: Draft<CartState>, action: CartAction) => {
  switch (action.type) {
    case 'increment': {
      const item = draft.items.find(i => i.id === action.id);
      if (item) item.qty += 1;
      break;
    }
    case 'remove':
      draft.items = draft.items.filter(i => i.id !== action.id);
      break;
    case 'clear':
      draft.items = [];
      break;
  }
});

export function Cart() {
  const [cart, dispatch] = useReducer(cartReducer, { items: [{ id: 1, qty: 2 }] });
  return (
    <ul>
      {cart.items.map(item => (
        <li key={item.id}>
          Qty: {item.qty}
          <button onClick={() => dispatch({ type: 'increment', id: item.id })}>+</button>
          <button onClick={() => dispatch({ type: 'remove', id: item.id })}>Remove</button>
        </li>
      ))}
    </ul>
  );
}
```

### Immer with Zustand

Zustand has built-in Immer middleware:

```tsx
import { create } from 'zustand';
import { immer } from 'zustand/middleware/immer';

interface TodoStore {
  todos: { id: number; text: string; done: boolean }[];
  toggle: (id: number) => void;
}

export const useTodoStore = create<TodoStore>()(
  immer(set => ({
    todos: [],
    toggle: id => set(state => {
      const todo = state.todos.find(t => t.id === id);
      if (todo) todo.done = !todo.done; // direct mutation — Immer handles it
    }),
  }))
);
```

### Server Mutations with TanStack Query

Use `useMutation` for create, update, and delete operations:

```tsx
import { useMutation, useQueryClient } from '@tanstack/react-query';
import { api } from '@/lib/api';

interface Todo { id: number; text: string; done: boolean }

export function usePatchTodo() {
  const queryClient = useQueryClient();
  return useMutation({
    mutationFn: ({ id, patch }: { id: number; patch: Partial<Todo> }) =>
      api.patch<Todo>(`/todos/${id}`, patch).then(r => r.data),
    onSuccess: (updatedTodo) => {
      // Update the specific item in the cache without refetching
      queryClient.setQueryData<Todo[]>(['todos'], old =>
        old?.map(t => t.id === updatedTodo.id ? updatedTodo : t) ?? []
      );
    },
  });
}
```

### Optimistic Updates

Apply the change immediately in the UI, then revert if the server returns an error:

```tsx
const mutation = useMutation({
  mutationFn: ({ id, done }: { id: number; done: boolean }) =>
    api.patch(`/todos/${id}`, { done }),

  onMutate: async ({ id, done }) => {
    await queryClient.cancelQueries({ queryKey: ['todos'] });
    const previous = queryClient.getQueryData<Todo[]>(['todos']);

    // Optimistically update the cache
    queryClient.setQueryData<Todo[]>(['todos'], old =>
      old?.map(t => t.id === id ? { ...t, done } : t) ?? []
    );
    return { previous }; // snapshot for rollback
  },

  onError: (_err, _vars, context) => {
    // Rollback on failure
    if (context?.previous) queryClient.setQueryData(['todos'], context.previous);
  },

  onSettled: () => {
    queryClient.invalidateQueries({ queryKey: ['todos'] });
  },
});
```

---

## Next.js

### Server Actions for Mutations

Server Actions run on the server and can directly call the database — no API route needed.

```ts
// app/actions/todos.ts
'use server';
import { revalidatePath } from 'next/cache';
import { z } from 'zod';

const patchSchema = z.object({ id: z.coerce.number(), done: z.coerce.boolean() });

export async function toggleTodo(formData: FormData) {
  const { id, done } = patchSchema.parse(Object.fromEntries(formData));
  await db.todo.update({ where: { id }, data: { done } });
  revalidatePath('/todos');
}
```

```tsx
// app/todos/page.tsx
import { toggleTodo } from '@/app/actions/todos';

export default async function TodosPage() {
  const todos = await db.todo.findMany();
  return (
    <ul>
      {todos.map(todo => (
        <li key={todo.id}>
          <form action={toggleTodo}>
            <input type="hidden" name="id" value={todo.id} />
            <input type="hidden" name="done" value={String(!todo.done)} />
            <button type="submit">{todo.done ? '✅' : '⬜'} {todo.text}</button>
          </form>
        </li>
      ))}
    </ul>
  );
}
```

### Cache Revalidation After Mutations

| Function | Effect |
|---|---|
| `revalidatePath('/todos')` | Purge cache for a specific route |
| `revalidatePath('/', 'layout')` | Purge all routes under a layout |
| `revalidateTag('todos')` | Purge all fetch calls tagged `'todos'` |
| `redirect('/todos')` | Navigate after mutation (replaces revalidatePath) |

### Optimistic UI with useOptimistic

React 19 / Next.js 14+ provides `useOptimistic` for instant UI feedback while a Server Action runs:

```tsx
'use client';
import { useOptimistic, useTransition } from 'react';
import { toggleTodo } from '@/app/actions/todos';

interface Todo { id: number; text: string; done: boolean }

export function TodoList({ initialTodos }: { initialTodos: Todo[] }) {
  const [optimisticTodos, setOptimisticTodo] = useOptimistic(
    initialTodos,
    (state, { id, done }: { id: number; done: boolean }) =>
      state.map(t => t.id === id ? { ...t, done } : t)
  );
  const [, startTransition] = useTransition();

  const handleToggle = (todo: Todo) => {
    startTransition(async () => {
      setOptimisticTodo({ id: todo.id, done: !todo.done }); // immediate UI update
      await toggleTodo(new FormData()); // actual server call
    });
  };

  return (
    <ul>
      {optimisticTodos.map(todo => (
        <li key={todo.id} style={{ opacity: todo.done ? 0.5 : 1 }}>
          <button onClick={() => handleToggle(todo)}>
            {todo.done ? '✅' : '⬜'} {todo.text}
          </button>
        </li>
      ))}
    </ul>
  );
}
```

### Combining Immer with Server Actions

Immer remains useful for managing complex local UI state while Server Actions handle persistence:

```tsx
'use client';
import { useState } from 'react';
import { produce } from 'immer';
import { saveProfile } from '@/app/actions/profile';

interface Profile { name: string; bio: string; links: string[] }

export function ProfileEditor({ initial }: { initial: Profile }) {
  const [profile, setProfile] = useState(initial);

  const addLink = (url: string) =>
    setProfile(produce(draft => { draft.links.push(url); }));

  const removeLink = (i: number) =>
    setProfile(produce(draft => { draft.links.splice(i, 1); }));

  return (
    <form action={() => saveProfile(profile)}>
      <input value={profile.name} onChange={e => setProfile(produce(d => { d.name = e.target.value; }))} />
      <textarea value={profile.bio} onChange={e => setProfile(produce(d => { d.bio = e.target.value; }))} />
      {profile.links.map((link, i) => (
        <div key={i}>{link} <button type="button" onClick={() => removeLink(i)}>✕</button></div>
      ))}
      <button type="button" onClick={() => addLink('https://')}>Add link</button>
      <button type="submit">Save</button>
    </form>
  );
}
```
