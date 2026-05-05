# React — Data Updation

Data updation covers how to apply immutable state changes locally (Immer), trigger server mutations (TanStack Query / Server Actions), and keep the UI optimistically in sync.

## Table of contents

- [React — Data Updation](#react--data-updation)
  - [Table of contents](#table-of-contents)
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
