# React Server Components

## Table of contents

- [React Server Components](#react-server-components)
  - [Table of contents](#table-of-contents)
  - [What are React Server Components?](#what-are-react-server-components)
  - [Server vs Client Components](#server-vs-client-components)
  - [The 'use client' Directive](#the-use-client-directive)
  - [The 'use server' Directive and Server Actions](#the-use-server-directive-and-server-actions)
  - [Data Fetching in RSC](#data-fetching-in-rsc)
  - [Suspense and Streaming](#suspense-and-streaming)
  - [Caching in Next.js App Router](#caching-in-nextjs-app-router)
  - [Next.js App Router — File Conventions](#nextjs-app-router--file-conventions)
  - [Patterns and Composition](#patterns-and-composition)
  - [When to Use Each Component Type](#when-to-use-each-component-type)
  - [Common Mistakes](#common-mistakes)

---

## What are React Server Components?

**React Server Components (RSC)** are a new type of React component that render exclusively on the server. They ship **zero JavaScript** to the client, enabling:

- Direct access to server-side resources (databases, file system, env vars)
- Reduced client-side bundle size
- Improved initial page load performance
- Automatic code splitting

RSC is the default rendering model in **Next.js 13+ App Router**.

```
Traditional React (CSR)          RSC
───────────────────────           ────────────────────
Client downloads JS     →         Server renders HTML
Client executes JS      →         Only interactive parts sent as JS
Client fetches data     →         Data fetched on server before render
```

---

## Server vs Client Components

| Feature | Server Component | Client Component |
|---|---|---|
| Runs on | Server only | Browser (and server for SSR) |
| Can use hooks | ❌ | ✅ (`useState`, `useEffect`, etc.) |
| Can handle events | ❌ | ✅ (`onClick`, `onChange`, etc.) |
| Access server resources | ✅ (DB, fs, env) | ❌ |
| Browser APIs | ❌ | ✅ |
| JS in bundle | ❌ (zero) | ✅ |
| Default in App Router | ✅ | ❌ (requires `'use client'`) |

---

## The 'use client' Directive

Add `'use client'` at the top of a file to mark it and all its imports as client components.

```tsx
'use client';

import { useState } from 'react';

export function Counter() {
  const [count, setCount] = useState(0);
  return (
    <button onClick={() => setCount(c => c + 1)}>
      Count: {count}
    </button>
  );
}
```

**Key rules:**
- Only needed at the **boundary** — components below it are automatically client components
- Client components can still be **rendered on the server** (for SSR/SSG), but their JS is sent to the browser
- Import a client component in a server component is fine; the reverse is not (without workarounds)

---

## The 'use server' Directive and Server Actions

**Server Actions** are async functions that run on the server, callable from client components (forms, event handlers).

```tsx
// app/actions.ts
'use server';

import { db } from '@/lib/db';
import { revalidatePath } from 'next/cache';

export async function createPost(formData: FormData) {
  const title = formData.get('title') as string;
  const content = formData.get('content') as string;

  await db.post.create({ data: { title, content } });
  revalidatePath('/posts');
}

export async function deletePost(id: string) {
  await db.post.delete({ where: { id } });
  revalidatePath('/posts');
}
```

**Using Server Actions in forms (native):**
```tsx
// app/posts/new/page.tsx — Server Component
import { createPost } from '@/app/actions';

export default function NewPostPage() {
  return (
    <form action={createPost}>
      <input name="title" required />
      <textarea name="content" />
      <button type="submit">Create</button>
    </form>
  );
}
```

**Using Server Actions from client event handlers:**
```tsx
'use client';

import { deletePost } from '@/app/actions';
import { useTransition } from 'react';

export function DeleteButton({ id }: { id: string }) {
  const [isPending, startTransition] = useTransition();

  return (
    <button
      disabled={isPending}
      onClick={() => startTransition(() => deletePost(id))}
    >
      {isPending ? 'Deleting...' : 'Delete'}
    </button>
  );
}
```

---

## Data Fetching in RSC

Server Components can be `async` and directly `await` data.

```tsx
// app/users/page.tsx — Server Component
import { db } from '@/lib/db';

export default async function UsersPage() {
  // Direct DB access — no API layer needed
  const users = await db.user.findMany({ orderBy: { name: 'asc' } });

  return (
    <ul>
      {users.map(user => <li key={user.id}>{user.name}</li>)}
    </ul>
  );
}
```

**Parallel data fetching:**
```tsx
export default async function DashboardPage() {
  // Both requests fire in parallel
  const [user, posts, analytics] = await Promise.all([
    fetchUser(),
    fetchPosts(),
    fetchAnalytics(),
  ]);

  return <Dashboard user={user} posts={posts} analytics={analytics} />;
}
```

**Passing server data to client components:**
```tsx
// Server Component — fetches, passes as props
export default async function PostPage({ params }: { params: { id: string } }) {
  const post = await db.post.findUnique({ where: { id: params.id } });
  return <PostEditor initialData={post} />;   // PostEditor is a client component
}
```

---

## Suspense and Streaming

**Suspense** lets you show a fallback while async content loads. With RSC, it enables **streaming** — the server sends HTML in chunks as each part becomes ready.

```tsx
import { Suspense } from 'react';

export default function Page() {
  return (
    <main>
      <h1>Dashboard</h1>   {/* renders immediately */}
      
      <Suspense fallback={<Skeleton />}>
        <SlowDataComponent />   {/* streams in when ready */}
      </Suspense>
      
      <Suspense fallback={<ChartSkeleton />}>
        <AnalyticsChart />
      </Suspense>
    </main>
  );
}

// This async Server Component delays its own Suspense boundary
async function SlowDataComponent() {
  const data = await slowDatabaseQuery();
  return <DataTable data={data} />;
}
```

**`loading.tsx` — automatic Suspense:**
```tsx
// app/posts/loading.tsx — shown while posts/page.tsx loads
export default function Loading() {
  return <PostListSkeleton />;
}
```

**`error.tsx` — automatic Error Boundary:**
```tsx
// app/posts/error.tsx
'use client';  // error boundaries must be client components

export default function Error({ error, reset }: {
  error: Error,
  reset: () => void
}) {
  return (
    <div>
      <h2>Something went wrong</h2>
      <p>{error.message}</p>
      <button onClick={reset}>Retry</button>
    </div>
  );
}
```

---

## Caching in Next.js App Router

Next.js has multiple layers of caching for RSC data fetching.

```tsx
// fetch() caching options
fetch('/api/data')                           // cached (default)
fetch('/api/data', { cache: 'no-store' })    // never cache (dynamic)
fetch('/api/data', { next: { revalidate: 60 } })  // revalidate every 60s (ISR)

// Data cache with tags
fetch('/api/posts', { next: { tags: ['posts'] } })

// Revalidate on demand
import { revalidateTag, revalidatePath } from 'next/cache';
revalidateTag('posts');           // in a server action
revalidatePath('/posts');

// unstable_cache — cache non-fetch async operations
import { unstable_cache } from 'next/cache';

const getCachedUser = unstable_cache(
  async (id: string) => db.user.findUnique({ where: { id } }),
  ['user'],
  { revalidate: 300, tags: ['user'] }
);
```

---

## Next.js App Router — File Conventions

```
app/
  layout.tsx          ← root layout (required)
  page.tsx            ← / route
  loading.tsx         ← loading UI (auto Suspense)
  error.tsx           ← error boundary ('use client')
  not-found.tsx       ← 404 page
  
  dashboard/
    layout.tsx        ← nested layout (wraps dashboard routes)
    page.tsx          ← /dashboard
    
  posts/
    page.tsx          ← /posts
    [id]/
      page.tsx        ← /posts/[id]  (dynamic segment)
    
  (marketing)/        ← route group (not in URL)
    about/page.tsx    ← /about
    
  @modal/             ← parallel route slot
    (.)photo/[id]/    ← intercepted route
```

**Special exports from `page.tsx`:**
```tsx
// Static generation params (for dynamic routes)
export async function generateStaticParams() {
  const posts = await db.post.findMany();
  return posts.map(p => ({ id: p.id }));
}

// Page metadata
export async function generateMetadata({ params }) {
  const post = await getPost(params.id);
  return { title: post.title, description: post.excerpt };
}

// Route segment config
export const dynamic = 'force-dynamic';    // or 'force-static'
export const revalidate = 60;              // ISR
export const runtime = 'edge';            // or 'nodejs'
```

---

## Patterns and Composition

**Interleaving server and client components:**
```tsx
// ✅ Pass client component as children to a server component
export default function ServerLayout({ children }) {
  return (
    <div>
      <ServerSideNav />      {/* server component */}
      {children}             {/* could be client component */}
    </div>
  );
}

// ✅ Pass server component as children to a client component
'use client';
function Modal({ children }) {  // children rendered on server
  const [open, setOpen] = useState(false);
  return open ? <dialog>{children}</dialog> : null;
}

// Usage in a server component:
<Modal><UserProfile userId={id} /></Modal>
```

**Server Component as data layer:**
```tsx
// Fetch in server component, distribute via props
async function PostsPage() {
  const posts = await getPosts();
  return (
    <>
      <PostFilter />                          {/* client component */}
      <PostList posts={posts} />              {/* server component */}
      <PostPagination totalCount={posts.length} /> {/* client component */}
    </>
  );
}
```

---

## When to Use Each Component Type

**Use Server Components for:**
- Pages and layouts
- Data fetching (database, API calls)
- Sensitive logic (API keys, auth checks)
- Static content (blog posts, product pages)
- Large dependencies that don't need interactivity

**Use Client Components for:**
- Interactivity (`onClick`, `onChange`, form state)
- Browser-only APIs (localStorage, geolocation, canvas)
- React hooks (`useState`, `useEffect`, custom hooks)
- Real-time updates (WebSockets)
- Third-party UI libraries (most require `'use client'`)

---

## Common Mistakes

```tsx
// ❌ Using hooks in a Server Component
export default function Page() {
  const [count, setCount] = useState(0);  // Error!
}

// ❌ Importing a Server Component into a Client Component
'use client';
import { ServerOnlyData } from './ServerComponent';  // Error!

// ✅ Pass it as children instead
// In the parent server component:
<ClientWrapper><ServerOnlyData /></ClientWrapper>

// ❌ Passing non-serializable values (functions) as props to client components
<ClientComponent onClick={myServerFunction} />  // functions aren't serializable!
// ✅ Use Server Actions instead

// ❌ Making everything a Client Component (defeats the purpose)
// ✅ Push `'use client'` as far down the tree as possible — keep leaves interactive

// ❌ Fetching data in a Client Component with useEffect when RSC is available
// ✅ Fetch in a Server Component and pass as props
```
