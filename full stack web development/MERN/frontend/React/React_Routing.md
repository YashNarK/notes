# React — Routing

## Table of contents

- [React — Routing](#react--routing)
  - [Table of contents](#table-of-contents)
  - [Routing Concepts](#routing-concepts)
    - [What is Routing?](#what-is-routing)
    - [Server-Side Routing](#server-side-routing)
    - [Client-Side Routing](#client-side-routing)
    - [Hash Routing](#hash-routing)
    - [History API Routing](#history-api-routing)
    - [File-Based vs Code-Based Routing](#file-based-vs-code-based-routing)
    - [Which Strategy Should You Use?](#which-strategy-should-you-use)
  - [Plain React](#plain-react)
    - [React Router v6](#react-router-v6)
    - [HashRouter — When You Cannot Configure the Server](#hashrouter--when-you-cannot-configure-the-server)
    - [Nested Routes and Outlet](#nested-routes-and-outlet)
    - [TanStack Router](#tanstack-router)
    - [Choosing Between Them](#choosing-between-them)
  - [Next.js](#nextjs)
    - [App Router (Recommended)](#app-router-recommended)
    - [Dynamic Routes](#dynamic-routes)
    - [Route Groups \& Parallel Routes](#route-groups--parallel-routes)
    - [Navigation](#navigation)

---

## Routing Concepts

### What is Routing?

**Routing** is the mechanism that decides which UI to show based on the current URL.

In a traditional multi-page website, every URL maps to a separate HTML file on the server:

```
User visits /about
  → Browser sends HTTP GET /about to server
  → Server responds with about.html
  → Browser does a full page reload
```

In a React app (a **Single-Page Application**, or SPA), there is only **one HTML file** (`index.html`). The JavaScript application intercepts navigation and swaps out components without ever reloading the page. This is **client-side routing**.

```
User clicks a link to /about
  → Browser does NOT reload
  → JavaScript reads the URL, renders <AboutPage /> instead
  → Address bar updates to /about (but no network request)
```

---

### Server-Side Routing

The traditional model. Every URL request hits the server, which returns a full HTML response.

```
/          → returns index.html
/about     → returns about.html
/users/42  → returns user-42.html  (or a 404 if it doesn't exist)
```

**Pros:** Works without JavaScript; great for SEO; simple mental model.  
**Cons:** Full page reload on every navigation; slower perceived performance.

> **Relevance to React:** Next.js uses a hybrid model — it does server-side routing for the initial page load (good for SEO) and then switches to client-side routing for subsequent navigations.

---

### Client-Side Routing

The SPA model. The server always returns the same `index.html` for every route, and JavaScript handles the rest.

```
/          → server returns index.html → React renders <HomePage />
/about     → server returns index.html → React renders <AboutPage />
/users/42  → server returns index.html → React renders <UserPage id="42" />
```

The browser has two built-in mechanisms to change the URL without reloading the page:
1. **Hash routing** — uses the `#` fragment
2. **History API routing** — uses `pushState` / `replaceState`

---

### Hash Routing

The URL fragment (`#`) was originally designed for in-page anchor links. Client-side routers hijack it to simulate navigation.

```
https://myapp.com/#/
https://myapp.com/#/about
https://myapp.com/#/users/42
```

**How it works:** Everything after `#` is never sent to the server. The browser fires a `hashchange` event when it changes, which the router library listens to.

```js
window.addEventListener('hashchange', () => {
  const path = window.location.hash.slice(1); // e.g. "/about"
  renderPage(path);
});
```

**Pros:**
- Works on any static file server with zero configuration — the server always serves `index.html` and never sees the path.
- Works in environments where you cannot control server redirects (e.g., GitHub Pages).

**Cons:**
- The `#` looks ugly in the URL.
- The fragment is not sent to the server, so server-side rendering is impossible.
- Not ideal for SEO (search engines historically ignored the fragment).

---

### History API Routing

The modern standard. Uses the browser's [History API](https://developer.mozilla.org/en-US/docs/Web/API/History_API) to change the URL without reloading.

```js
// Push a new entry onto the browser history stack
window.history.pushState({}, '', '/about');

// Replace the current entry (no new history entry)
window.history.replaceState({}, '', '/about');

// Listen for the back/forward button
window.addEventListener('popstate', () => {
  renderPage(window.location.pathname);
});
```

The URL looks like a normal server URL:

```
https://myapp.com/
https://myapp.com/about
https://myapp.com/users/42
```

**Pros:** Clean URLs; compatible with SSR; better for SEO.

**Cons — the catch:** If the user refreshes at `https://myapp.com/about`, the browser sends `GET /about` to the server. If the server doesn't know about that route, it returns a **404**. You must configure the server to return `index.html` for all routes:

```nginx
# Nginx example — serve index.html for any unmatched route
location / {
  try_files $uri $uri/ /index.html;
}
```

> **React Router's `<BrowserRouter>`** uses the History API. **`<HashRouter>`** uses hash routing.

---

### File-Based vs Code-Based Routing

These are two ways to **define** your routes — both can run on top of either hash or history routing.

**Code-based routing** — you define routes explicitly in JavaScript/TypeScript:

```tsx
// You write every route in code
<Routes>
  <Route path="/"       element={<Home />} />
  <Route path="/about"  element={<About />} />
  <Route path="/users/:id" element={<User />} />
</Routes>
```

**File-based routing** — the router reads your **folder structure** and generates routes automatically. Create a file → you get a route.

```
pages/
  index.tsx        → /
  about.tsx        → /about
  users/
    [id].tsx       → /users/:id
```

| | Code-based | File-based |
|---|---|---|
| Route definition | Explicit in JS | Folder/file structure |
| Flexibility | Full control | Convention-driven |
| Discoverability | Requires reading code | Folder structure is self-documenting |
| Examples | React Router v6 | Next.js, TanStack Router (plugin), Remix |

---

### Which Strategy Should You Use?

| Situation | Recommendation |
|---|---|
| Plain React SPA, full server control | History API (`BrowserRouter`) |
| Plain React SPA, static host (GitHub Pages, S3) | Hash routing (`HashRouter`) |
| Plain React SPA, large app, type safety matters | TanStack Router |
| Full-stack app with SSR / SEO requirements | Next.js App Router |

---

## Plain React

React has no built-in router. You add one as a library.

### React Router v6

The long-standing standard for React routing. Uses the History API by default via `<BrowserRouter>`.

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
        <Route path="/"          element={<HomePage />} />
        <Route path="/users/:userId" element={<UserPage />} />
        <Route path="*"          element={<NotFound />} />   {/* catch-all 404 */}
      </Routes>
    </BrowserRouter>
  );
}

function UserPage() {
  const { userId } = useParams<{ userId: string }>();
  return <div>User: {userId}</div>;
}
```

> **Remember:** `<BrowserRouter>` uses the History API, so your server must be configured to return `index.html` for all routes (see [History API Routing](#history-api-routing) above). Most dev servers (Vite, CRA) do this automatically.

### HashRouter — When You Cannot Configure the Server

If you're deploying to a static host where you have no control over server redirects, swap `<BrowserRouter>` for `<HashRouter>`. No other code changes needed.

```tsx
import { HashRouter, Routes, Route } from 'react-router-dom';

function App() {
  return (
    <HashRouter>   {/* URLs become: /#/, /#/about, /#/users/42 */}
      <Routes>
        <Route path="/"     element={<HomePage />} />
        <Route path="/about" element={<AboutPage />} />
      </Routes>
    </HashRouter>
  );
}
```

### Nested Routes and Outlet

Nested routes let child routes render **inside** a parent layout without remounting the layout on navigation. The parent uses `<Outlet />` as the placeholder where the child renders.

```tsx
import { BrowserRouter, Routes, Route, Outlet, Link } from 'react-router-dom';

// Shared layout — renders once, children swap via <Outlet>
function DashboardLayout() {
  return (
    <div>
      <nav>
        <Link to="/dashboard">Overview</Link>
        <Link to="/dashboard/settings">Settings</Link>
      </nav>
      <main>
        <Outlet />   {/* child route renders here */}
      </main>
    </div>
  );
}

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/dashboard" element={<DashboardLayout />}>
          <Route index        element={<Overview />} />   {/* /dashboard */}
          <Route path="settings" element={<Settings />} /> {/* /dashboard/settings */}
        </Route>
      </Routes>
    </BrowserRouter>
  );
}
```

`<Outlet />` is what makes it possible to have a persistent sidebar/navbar that doesn't remount as you navigate between child routes.

### TanStack Router

**TanStack Router** is the modern, fully type-safe alternative. It supports both code-based and file-based route definitions and gives you full TypeScript inference on route params and search params.

```bash
npm i @tanstack/react-router
```

```tsx
// main.tsx — code-based route definition
import { createRouter, createRoute, createRootRoute, RouterProvider } from '@tanstack/react-router';

const rootRoute  = createRootRoute({ component: RootLayout });
const indexRoute = createRoute({ getParentRoute: () => rootRoute, path: '/', component: HomePage });
const userRoute  = createRoute({
  getParentRoute: () => rootRoute,
  path: '/users/$userId',
  component: UserPage,
});

const router = createRouter({ routeTree: rootRoute.addChildren([indexRoute, userRoute]) });

function UserPage() {
  const { userId } = userRoute.useParams(); // TypeScript knows this is a string
  return <div>User: {userId}</div>;
}

export default function App() {
  return <RouterProvider router={router} />;
}
```

**Key features:**
- Fully type-safe `useParams`, `useSearch`, `useNavigate` — no manual generics needed
- Automatic route-based code splitting (lazy loading)
- Built-in loader / pending states (similar to Remix)
- Optional file-based routing via `@tanstack/router-plugin`
- Devtools: `@tanstack/router-devtools`

### Choosing Between Them

| Feature | React Router v6 | TanStack Router |
|---|---|---|
| Type-safe params/search | ⚠️ Manual generics | ✅ Full inference |
| File-based routing | ❌ | ✅ Optional plugin |
| Loaders / data fetching | ✅ (v6.4+) | ✅ Built-in |
| HashRouter support | ✅ | ✅ |
| Bundle size | Smaller | Slightly larger |
| Maturity | Battle-tested | Newer, rapidly growing |

---

## Next.js

Next.js has routing built in — no library to install. It uses **file-based routing** on top of **server-side + client-side hybrid** navigation.

### App Router (Recommended)

The file system inside `app/` is the routing layer. Every `page.tsx` becomes a route; `layout.tsx` files wrap routes with shared UI.

```
app/
  layout.tsx        ← root layout (wraps everything)
  page.tsx          → /
  about/
    page.tsx        → /about
  users/
    page.tsx        → /users
    [id]/
      page.tsx      → /users/:id
```

```tsx
// app/page.tsx — Server Component by default
export default function HomePage() {
  return <h1>Home</h1>;
}
```

```tsx
// app/layout.tsx — persistent shell, never remounts between navigations
export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en">
      <body>
        <nav>…</nav>
        <main>{children}</main>   {/* equivalent to <Outlet /> */}
      </body>
    </html>
  );
}
```

> **`layout.tsx` is Next.js's equivalent of React Router's `<Outlet />`** — it defines a persistent shell and renders the current page inside `{children}`.

### Dynamic Routes

Wrap a folder name in `[brackets]` to create a dynamic segment:

```tsx
// app/users/[id]/page.tsx
interface Props {
  params: { id: string };
}

export default function UserPage({ params }: Props) {
  return <div>User ID: {params.id}</div>;
}
```

**Catch-all segments**: `app/docs/[...slug]/page.tsx` matches `/docs/a/b/c` — `slug` is `['a', 'b', 'c']`.

**Optional catch-all**: `app/docs/[[...slug]]/page.tsx` also matches `/docs` (slug is `undefined`).

### Route Groups & Parallel Routes

**Route groups** `(groupName)` organise routes into folders without affecting the URL. Use them to apply different layouts to different sets of routes:

```
app/
  (auth)/
    layout.tsx          ← auth layout (centered card, no nav)
    login/page.tsx      → /login
    register/page.tsx   → /register
  (dashboard)/
    layout.tsx          ← dashboard layout (sidebar + nav)
    overview/page.tsx   → /overview
    settings/page.tsx   → /settings
```

**Parallel routes** render multiple pages simultaneously in the same layout using `@slot` folders — useful for dashboards with independent panes:

```
app/dashboard/
  layout.tsx
  @analytics/page.tsx      ← props.analytics
  @notifications/page.tsx  ← props.notifications
  page.tsx
```

```tsx
// app/dashboard/layout.tsx
export default function DashboardLayout({
  children,
  analytics,
  notifications,
}: {
  children: React.ReactNode;
  analytics: React.ReactNode;
  notifications: React.ReactNode;
}) {
  return (
    <div className="dashboard">
      <main>{children}</main>
      <aside>{analytics}</aside>
      <aside>{notifications}</aside>
    </div>
  );
}
```

### Navigation

Use `<Link>` for declarative navigation and `useRouter` for programmatic navigation in Client Components.

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
      {/* prefetch={false} disables automatic prefetching for this link */}
      <Link href="/heavy-page" prefetch={false}>Heavy Page</Link>

      <button onClick={() => router.push('/dashboard')}>Go to Dashboard</button>
      <button onClick={() => router.back()}>Back</button>
      <button onClick={() => router.replace('/login')}>Replace history</button>
    </nav>
  );
}
```

**`redirect()`** is used inside Server Components and Server Actions (runs before the response is sent):

```tsx
import { redirect } from 'next/navigation';

export default async function ProtectedPage() {
  const session = await getSession();
  if (!session) redirect('/login'); // server-side redirect, no client JS needed
  return <div>Protected content</div>;
}
```


---

## React Router DOM Hooks (Plain React)

These hooks are only available inside components that are rendered within a **data router** (e.g., `createBrowserRouter`). They give you programmatic access to routing state.

### 1. `useLocation`

Returns the current **location object** (`{ pathname, search, hash, state, key }`). Useful for analytics or triggering side effects on navigation.

```tsx
import { useEffect } from 'react';
import { useLocation } from 'react-router-dom';

function PageAnalytics() {
  const location = useLocation();

  useEffect(() => {
    // Fire a page-view event on every route change
    analytics.page(location.pathname);
  }, [location]);

  return null;
}
```

### 2. `useNavigate`

Returns a **navigate function** for programmatic navigation — redirects after form submission, logout, or inactivity.

```tsx
import { useEffect } from 'react';
import { useNavigate } from 'react-router-dom';

function useInactivityRedirect(timeoutMs = 30_000) {
  const navigate = useNavigate();

  useEffect(() => {
    const timer = setTimeout(() => {
      navigate('/session-timed-out');   // navigate to a path
    }, timeoutMs);
    return () => clearTimeout(timer);
  }, [navigate, timeoutMs]);
}
```

**Two signatures:**

```ts
// 1. Navigate to a path (with optional state/replace)
navigate('/dashboard', { replace: true, state: { from: '/login' } });

// 2. Move through history stack (like browser back/forward)
navigate(-1);  // back
navigate(1);   // forward
```

### 3. `useParams`

Returns the **dynamic segment values** from the matched route path. Child routes inherit parent params.

```tsx
// Route definition: path="games/:slug"
import { useParams } from 'react-router-dom';

function GameDetailsPage() {
  const { slug } = useParams<{ slug: string }>();
  // slug = "the-witcher-3" when URL is /games/the-witcher-3
}
```

### 4. `useSearchParams`

Reads and updates the **query string** (`?key=value`) without a full page reload. Similar to `useState` for URL search params.

```tsx
import { useSearchParams } from 'react-router-dom';

function SearchPage() {
  const [searchParams, setSearchParams] = useSearchParams();
  const query    = searchParams.get('q')    ?? '';
  const category = searchParams.get('cat')  ?? 'all';

  return (
    <div>
      <input
        value={query}
        onChange={e =>
          // ❌ setSearchParams({ q: e.target.value })  — drops other params!
          // ✅ Spread existing params first to preserve them
          setSearchParams({ ...Object.fromEntries(searchParams), q: e.target.value })
        }
      />
      <select
        value={category}
        onChange={e =>
          setSearchParams({ ...Object.fromEntries(searchParams), cat: e.target.value })
        }
      >
        <option value="all">All</option>
        <option value="tech">Tech</option>
      </select>
    </div>
  );
}
```

> **Gotcha**: `setSearchParams({ q: value })` **replaces all** query params. Always spread the current params if you only want to update one.

### 5. `useRouteError`

Available inside an `errorElement`. Returns whatever was thrown during an action, loader, or render.

```tsx
import { isRouteErrorResponse, useRouteError } from 'react-router-dom';

function ErrorPage() {
  const error = useRouteError();

  const message = isRouteErrorResponse(error)
    ? `${error.status} — ${error.statusText}`  // known HTTP error (404, 403…)
    : 'An unexpected error occurred';           // runtime exception

  return (
    <div style={{ textAlign: 'center', paddingTop: '4rem' }}>
      <h1>Oops!</h1>
      <p>{message}</p>
    </div>
  );
}
```

> `isRouteErrorResponse` is a type-guard — it returns `true` when the error is an HTTP response (thrown from a `loader` or `action`), allowing you to show different UI for 404 vs generic crashes.

---

## Full App Setup: Router + TanStack Query (Plain React)

Production apps typically compose React Router with TanStack Query and a UI library (e.g., Chakra UI). The key wiring lives in `routes.tsx` and `main.tsx`.

### `routes.tsx` — centralised route config

```tsx
// src/routes.tsx
import { createBrowserRouter } from 'react-router-dom';
import Layout         from './pages/Layout';
import HomePage       from './pages/HomePage';
import GameDetailsPage from './pages/GameDetailsPage';
import ErrorPage      from './pages/ErrorPage';

const router = createBrowserRouter([
  {
    path: '/game-hub',
    element: <Layout />,       // persistent shell (NavBar, Sidebar)
    errorElement: <ErrorPage />,
    children: [
      { index: true,              element: <HomePage /> },
      { path: 'games/:slug',      element: <GameDetailsPage /> },
    ],
  },
]);

export default router;
```

### `main.tsx` — root provider composition

```tsx
// src/main.tsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import { ChakraProvider, ColorModeScript } from '@chakra-ui/react';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { ReactQueryDevtools } from '@tanstack/react-query-devtools';
import { RouterProvider } from 'react-router-dom';
import router from './routes.tsx';
import theme from './theme.ts';
import './index.css';

const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      retry: 3,
      staleTime:           1000 * 60 * 10,   // 10 min — don't refetch fresh data
      gcTime:              1000 * 60 * 5,    // 5 min — evict unused cache
      refetchOnWindowFocus: true,
      refetchOnReconnect:   true,
      refetchOnMount:       true,
    },
  },
});

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <ChakraProvider theme={theme}>
      <ColorModeScript initialColorMode={theme.config.initialColorMode} />
      <QueryClientProvider client={queryClient}>
        <RouterProvider router={router} />
        <ReactQueryDevtools />
      </QueryClientProvider>
    </ChakraProvider>
  </React.StrictMode>
);
```

**Provider order matters:**
- `ChakraProvider` wraps everything so Chakra components get access to the theme.
- `QueryClientProvider` wraps `RouterProvider` so that any component rendered by a route can call `useQuery`/`useMutation`.
- `RouterProvider` is the outermost *routing* boundary — it renders the matched route component tree.
