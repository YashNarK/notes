# React — Routing

React has no built-in router. The ecosystem provides two dominant solutions; Next.js ships its own file-based router.

## Table of contents

- [React — Routing](#react--routing)
  - [Table of contents](#table-of-contents)
  - [Plain React](#plain-react)
    - [TanStack Router](#tanstack-router)
    - [React Router v6](#react-router-v6)
    - [Choosing Between Them](#choosing-between-them)
  - [Next.js](#nextjs)
    - [App Router (Recommended)](#app-router-recommended)
    - [Dynamic Routes](#dynamic-routes)
    - [Route Groups \& Parallel Routes](#route-groups--parallel-routes)
    - [Navigation](#navigation)

---

## Plain React

### TanStack Router

**TanStack Router** is the modern, fully type-safe choice for SPAs. It supports file-based or code-based route definitions.

```bash
npm i @tanstack/react-router
```

```tsx
// main.tsx — code-based route definition
import { createRouter, createRoute, createRootRoute, RouterProvider } from '@tanstack/react-router';
import { RootLayout } from './RootLayout';
import { HomePage } from './pages/HomePage';
import { UserPage } from './pages/UserPage';

const rootRoute = createRootRoute({ component: RootLayout });

const indexRoute = createRoute({ getParentRoute: () => rootRoute, path: '/', component: HomePage });
const userRoute  = createRoute({
  getParentRoute: () => rootRoute,
  path: '/users/$userId',
  component: UserPage,
});

const routeTree = rootRoute.addChildren([indexRoute, userRoute]);
const router = createRouter({ routeTree });

// Full TypeScript inference on params
function UserPage() {
  const { userId } = userRoute.useParams(); // typed: string
  return <div>User: {userId}</div>;
}

export default function App() {
  return <RouterProvider router={router} />;
}
```

**Key features:**
- Automatic route splitting (lazy loading)
- Type-safe `useParams`, `useSearch`, `useNavigate`
- Built-in loader / pending states (similar to Remix)
- Devtools: `@tanstack/router-devtools`

### React Router v6

The long-standing standard. Simpler API, less type safety than TanStack Router.

```bash
npm i react-router-dom
```

```tsx
import { BrowserRouter, Routes, Route, Link, useParams } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/users/42">User 42</Link>
      </nav>
      <Routes>
        <Route path="/" element={<HomePage />} />
        <Route path="/users/:userId" element={<UserPage />} />
        <Route path="*" element={<NotFound />} />
      </Routes>
    </BrowserRouter>
  );
}

function UserPage() {
  const { userId } = useParams<{ userId: string }>();
  return <div>User: {userId}</div>;
}
```

**Nested routes** share layouts without remounting:

```tsx
<Routes>
  <Route path="/dashboard" element={<DashboardLayout />}>
    <Route index element={<Overview />} />
    <Route path="settings" element={<Settings />} />
  </Route>
</Routes>
```

### Choosing Between Them

| Feature | TanStack Router | React Router v6 |
|---|---|---|
| Type-safe params/search | ✅ Full inference | ⚠️ Manual generics |
| File-based routing | ✅ Optional plugin | ❌ |
| Loaders / data fetching | ✅ Built-in | ✅ (v6.4+) |
| Bundle size | Slightly larger | Smaller |
| Maturity | Newer | Battle-tested |

---

## Next.js

### App Router (Recommended)

Next.js App Router uses the **file system** as the routing layer. Every `page.tsx` inside `app/` becomes a route.

```
app/
  page.tsx              → /
  about/
    page.tsx            → /about
  users/
    page.tsx            → /users
    [id]/
      page.tsx          → /users/:id
```

```tsx
// app/page.tsx
export default function HomePage() {
  return <h1>Home</h1>;
}
```

### Dynamic Routes

```tsx
// app/users/[id]/page.tsx
interface Props {
  params: { id: string };
}

export default function UserPage({ params }: Props) {
  return <div>User ID: {params.id}</div>;
}
```

**Catch-all segments**: `app/docs/[...slug]/page.tsx` matches `/docs/a/b/c`.

**Optional catch-all**: `app/docs/[[...slug]]/page.tsx` also matches `/docs`.

### Route Groups & Parallel Routes

Route groups `(groupName)` organise routes without affecting the URL:

```
app/
  (auth)/
    login/page.tsx      → /login
    register/page.tsx   → /register
  (dashboard)/
    layout.tsx          ← shared dashboard shell
    overview/page.tsx   → /overview
```

Parallel routes render multiple pages in the same layout simultaneously using `@slot` folders:

```
app/dashboard/
  layout.tsx
  @analytics/page.tsx   ← rendered at props.analytics
  @notifications/page.tsx
  page.tsx
```

### Navigation

Use `<Link>` for client-side navigation and `useRouter` for programmatic navigation.

```tsx
'use client';
import Link from 'next/link';
import { useRouter } from 'next/navigation';

export function Nav() {
  const router = useRouter();

  return (
    <nav>
      <Link href="/">Home</Link>
      <Link href="/about">About</Link>
      <button onClick={() => router.push('/dashboard')}>Go to Dashboard</button>
      <button onClick={() => router.back()}>Back</button>
    </nav>
  );
}
```

**`redirect()`** can be used inside Server Components and Server Actions:

```tsx
import { redirect } from 'next/navigation';

export default async function ProtectedPage() {
  const session = await getSession();
  if (!session) redirect('/login');
  return <div>Protected content</div>;
}
```
