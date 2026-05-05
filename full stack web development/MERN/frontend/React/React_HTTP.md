# React — HTTP & Data Fetching

React itself makes no HTTP calls. The ecosystem uses Axios or Fetch for transport and TanStack Query for caching, retries, and synchronisation.

## Table of contents

- [React — HTTP \& Data Fetching](#react--http--data-fetching)
  - [Table of contents](#table-of-contents)
  - [Plain React](#plain-react)
    - [Fetch API](#fetch-api)
    - [Axios](#axios)
    - [TanStack Query (Recommended)](#tanstack-query-recommended)
    - [Mutations with TanStack Query](#mutations-with-tanstack-query)
  - [Next.js](#nextjs)
    - [Server Component Fetch (Recommended)](#server-component-fetch-recommended)
    - [Revalidation \& Caching](#revalidation--caching)
    - [Route Handlers (API Routes)](#route-handlers-api-routes)
    - [Client-side Fetching in Next.js](#client-side-fetching-in-nextjs)

---

## Plain React

### Fetch API

The native browser API. Works without any extra dependency.

```tsx
import { useEffect, useState } from 'react';

interface User { id: number; name: string; }

export function UserList() {
  const [users, setUsers]   = useState<User[]>([]);
  const [error, setError]   = useState<string | null>(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch('https://jsonplaceholder.typicode.com/users')
      .then(res => {
        if (!res.ok) throw new Error(`HTTP ${res.status}`);
        return res.json() as Promise<User[]>;
      })
      .then(data => setUsers(data))
      .catch(err => setError(err.message))
      .finally(() => setLoading(false));
  }, []);

  if (loading) return <p>Loading…</p>;
  if (error)   return <p>Error: {error}</p>;
  return <ul>{users.map(u => <li key={u.id}>{u.name}</li>)}</ul>;
}
```

> **Gotcha**: Always handle the `!res.ok` case manually — `fetch` does NOT reject on 4xx/5xx responses.

### Axios

Axios automatically throws on 4xx/5xx, supports request/response interceptors, and handles JSON serialisation.

```bash
npm i axios
```

```tsx
// lib/api.ts — shared Axios instance
import axios from 'axios';

export const api = axios.create({
  baseURL: import.meta.env.VITE_API_URL,
  timeout: 10_000,
  headers: { 'Content-Type': 'application/json' },
});

// Attach auth token on every request
api.interceptors.request.use(config => {
  const token = localStorage.getItem('token');
  if (token) config.headers.Authorization = `Bearer ${token}`;
  return config;
});

// Global error handling
api.interceptors.response.use(
  res => res,
  err => {
    if (err.response?.status === 401) window.location.href = '/login';
    return Promise.reject(err);
  }
);
```

```tsx
// Usage in a component
import { api } from '@/lib/api';

const { data } = await api.get<User[]>('/users');
```

### TanStack Query (Recommended)

TanStack Query handles caching, background refetching, pagination, and optimistic updates automatically.

```bash
npm i @tanstack/react-query
```

```tsx
// main.tsx — wrap the app
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { ReactQueryDevtools } from '@tanstack/react-query-devtools';

const queryClient = new QueryClient();

export default function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <UserList />
      <ReactQueryDevtools initialIsOpen={false} />
    </QueryClientProvider>
  );
}
```

```tsx
// hooks/useUsers.ts
import { useQuery } from '@tanstack/react-query';
import { api } from '@/lib/api';

export function useUsers() {
  return useQuery({
    queryKey: ['users'],
    queryFn: () => api.get<User[]>('/users').then(r => r.data),
    staleTime: 1000 * 60 * 5, // 5 minutes
  });
}

// Component
function UserList() {
  const { data: users, isLoading, error } = useUsers();
  if (isLoading) return <p>Loading…</p>;
  if (error)     return <p>Error: {String(error)}</p>;
  return <ul>{users!.map(u => <li key={u.id}>{u.name}</li>)}</ul>;
}
```

**Built-in features:**
- Auto retry on failure (3 retries by default)
- Background refetch on window focus
- Paginated queries: `useInfiniteQuery`
- Cache invalidation: `queryClient.invalidateQueries`

### Mutations with TanStack Query

```tsx
import { useMutation, useQueryClient } from '@tanstack/react-query';

function CreateUserForm() {
  const queryClient = useQueryClient();

  const mutation = useMutation({
    mutationFn: (newUser: { name: string }) =>
      api.post<User>('/users', newUser).then(r => r.data),
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['users'] }); // re-fetch list
    },
  });

  return (
    <button
      onClick={() => mutation.mutate({ name: 'Alice' })}
      disabled={mutation.isPending}
    >
      {mutation.isPending ? 'Creating…' : 'Create User'}
    </button>
  );
}
```

---

## Next.js

### Server Component Fetch (Recommended)

In the App Router, Server Components can `await` fetch directly. No `useEffect` needed.

```tsx
// app/users/page.tsx — Server Component
interface User { id: number; name: string; }

async function getUsers(): Promise<User[]> {
  const res = await fetch('https://api.example.com/users', {
    next: { revalidate: 60 }, // ISR: re-fetch at most every 60s
  });
  if (!res.ok) throw new Error('Failed to fetch users');
  return res.json();
}

export default async function UsersPage() {
  const users = await getUsers();
  return <ul>{users.map(u => <li key={u.id}>{u.name}</li>)}</ul>;
}
```

### Revalidation & Caching

| Option | Behaviour |
|---|---|
| `cache: 'force-cache'` | Static — cached indefinitely (default) |
| `next: { revalidate: N }` | ISR — re-fetch after N seconds |
| `cache: 'no-store'` | Dynamic — never cached, always fresh |
| `revalidatePath('/users')` | Manually purge cache for a path (in Server Actions) |
| `revalidateTag('users')` | Purge by tag across multiple fetch calls |

```tsx
// Tag-based revalidation
const res = await fetch('/api/users', { next: { tags: ['users'] } });

// In a Server Action or Route Handler:
import { revalidateTag } from 'next/cache';
revalidateTag('users');
```

### Route Handlers (API Routes)

`app/api/*/route.ts` files expose API endpoints.

```tsx
// app/api/users/route.ts
import { NextResponse } from 'next/server';

export async function GET() {
  const users = await db.user.findMany();
  return NextResponse.json(users);
}

export async function POST(request: Request) {
  const body = await request.json();
  const user = await db.user.create({ data: body });
  return NextResponse.json(user, { status: 201 });
}
```

### Client-side Fetching in Next.js

When data must be fetched client-side (e.g., user-specific, post-interaction), use TanStack Query inside `'use client'` components exactly as in plain React.

```tsx
'use client';
import { useQuery } from '@tanstack/react-query';

export function UserDashboard() {
  const { data } = useQuery({
    queryKey: ['profile'],
    queryFn: () => fetch('/api/me').then(r => r.json()),
  });
  return <div>{data?.name}</div>;
}
```

> **Best practice**: Fetch as much as possible in Server Components; fall back to TanStack Query only for interactive or real-time data.
