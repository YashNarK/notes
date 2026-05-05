# React — HTTP & Data Fetching

## Table of contents

- [React — HTTP \& Data Fetching](#react--http--data-fetching)
  - [Table of contents](#table-of-contents)
  - [HTTP Concepts](#http-concepts)
    - [What is HTTP?](#what-is-http)
    - [Request and Response](#request-and-response)
    - [REST APIs](#rest-apis)
    - [Synchronous vs Asynchronous Requests](#synchronous-vs-asynchronous-requests)
    - [The Problem with Raw fetch in React](#the-problem-with-raw-fetch-in-react)
    - [Transport vs Data Layer](#transport-vs-data-layer)
    - [What is Caching?](#what-is-caching)
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

## HTTP Concepts

### What is HTTP?

**HTTP (HyperText Transfer Protocol)** is the language browsers and servers use to communicate. Every time your app loads data from a backend, it speaks HTTP.

```
Browser                              Server
  │                                    │
  │── GET /api/users ─────────────────►│
  │                                    │  (database query)
  │◄── 200 OK + JSON body ────────────│
  │                                    │
```

### Request and Response

Every HTTP interaction has two parts:

**Request** — sent by the browser:
```
Method:  GET / POST / PUT / PATCH / DELETE
URL:     https://api.example.com/users/42
Headers: Content-Type: application/json
         Authorization: Bearer <token>
Body:    { "name": "Alice" }   ← only for POST/PUT/PATCH
```

**Response** — sent by the server:
```
Status:  200 OK / 201 Created / 400 Bad Request / 401 Unauthorized / 404 Not Found / 500 Server Error
Headers: Content-Type: application/json
Body:    { "id": 42, "name": "Alice" }
```

**Common status codes:**

| Code | Meaning |
|---|---|
| 200 | OK — request succeeded |
| 201 | Created — resource was created |
| 400 | Bad Request — client sent invalid data |
| 401 | Unauthorized — not logged in |
| 403 | Forbidden — logged in but no permission |
| 404 | Not Found — resource doesn't exist |
| 500 | Server Error — bug on the backend |

### REST APIs

**REST (Representational State Transfer)** is a convention for structuring API URLs and HTTP methods around **resources** (things like users, posts, orders).

```
GET    /users          → list all users
GET    /users/42       → get user with id 42
POST   /users          → create a new user
PUT    /users/42       → replace user 42 entirely
PATCH  /users/42       → partially update user 42
DELETE /users/42       → delete user 42
```

Most React apps talk to REST APIs. The backend exposes these endpoints; the frontend calls them.

### Synchronous vs Asynchronous Requests

HTTP requests take time — the browser must wait for the server to respond. JavaScript handles this with **Promises** and **async/await**, so the UI stays responsive while waiting.

```ts
// Synchronous (blocking) — the entire thread freezes until done
// NOT how HTTP works in JavaScript

// Asynchronous — correct approach
async function loadUser(id: number) {
  const response = await fetch(`/api/users/${id}`); // waits, but doesn't block
  const user = await response.json();
  return user;
}
```

### The Problem with Raw fetch in React

Using `fetch` directly inside a component requires a lot of boilerplate — and has hidden pitfalls:

```tsx
// Problems you have to solve manually every single time:
const [data, setData]       = useState(null);  // 1. store the result
const [loading, setLoading] = useState(true);  // 2. track loading state
const [error, setError]     = useState(null);  // 3. handle errors

useEffect(() => {
  let cancelled = false;   // 4. prevent race conditions (if component unmounts)
  fetch('/api/users')
    .then(r => { if (!r.ok) throw new Error('Failed'); return r.json(); })
    .then(d => { if (!cancelled) setData(d); })
    .catch(e => { if (!cancelled) setError(e); })
    .finally(() => { if (!cancelled) setLoading(false); });
  return () => { cancelled = true; };  // 5. cleanup
}, []);

// 6. No caching — refetches every time component mounts
// 7. No retry on network error
// 8. No background refresh
// 9. No deduplication if two components fetch the same thing
```

Libraries like **TanStack Query** solve all 9 of these problems automatically.

### Transport vs Data Layer

These are two different responsibilities:

| Layer | Job | Libraries |
|---|---|---|
| **Transport** | Make HTTP requests, attach headers, handle auth tokens | `fetch`, Axios |
| **Data layer** | Cache results, retry, refetch, sync with server | TanStack Query |

You typically use both together:
```
Axios    →  makes the actual HTTP call
TanStack Query  →  manages when/whether to make it
```

### What is Caching?

**Caching** means storing a previously fetched result so you don't have to fetch it again immediately.

```
Without caching:
  Navigate to Users list → fetch /api/users → show
  Navigate away
  Navigate back to Users list → fetch /api/users again → show

With caching (TanStack Query):
  Navigate to Users list → fetch /api/users → cache result → show
  Navigate away
  Navigate back → show cached data instantly → (background refetch to check for updates)
```

**Stale time** — how long cached data is considered fresh (no refetch needed).
**Cache time** — how long cached data is kept in memory after all components stop using it.

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
