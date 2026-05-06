# Single Page Application (SPA)

## Table of contents

- [Single Page Application (SPA)](#single-page-application-spa)
  - [Table of contents](#table-of-contents)
  - [What Is a Web Application?](#what-is-a-web-application)
  - [Traditional Multi-Page Applications](#traditional-multi-page-applications)
  - [What Is a Single Page Application?](#what-is-a-single-page-application)
  - [How SPAs Work Internally](#how-spas-work-internally)
  - [Client-Side Routing](#client-side-routing)
  - [SPA vs MPA Comparison](#spa-vs-mpa-comparison)
  - [Building a SPA — Tooling and Frameworks](#building-a-spa--tooling-and-frameworks)
  - [Data Fetching in SPAs](#data-fetching-in-spas)
  - [Code Splitting and Lazy Loading](#code-splitting-and-lazy-loading)
  - [SEO Challenges and Solutions](#seo-challenges-and-solutions)
  - [Authentication in SPAs](#authentication-in-spas)
  - [Performance Considerations](#performance-considerations)
  - [When to Use an SPA](#when-to-use-an-spa)
  - [Pros and Cons](#pros-and-cons)

---

## What Is a Web Application?

Before understanding SPAs, we need to understand how web applications deliver content to users.

Every time you visit a website, your **browser** (the client) sends a request to a **server**. The server returns content — typically HTML, CSS, and JavaScript — which the browser renders as a visual page.

Two fundamental rendering strategies exist:

| Strategy | Who builds the HTML? | When? |
|---|---|---|
| **Server-Side Rendering (SSR)** | The server | On every request |
| **Client-Side Rendering (CSR)** | The browser | After downloading JS |

---

## Traditional Multi-Page Applications

In a classic **Multi-Page Application (MPA)**:

1. User clicks a link (e.g., "About Us")
2. Browser sends a `GET /about` HTTP request to the server
3. Server queries a database, builds an HTML page, and returns the full HTML document
4. Browser **discards the current page** and renders the new HTML from scratch
5. All scripts, stylesheets, and images reload

```
[Browser] --GET /home---> [Server] --> returns full HTML page
[Browser] --GET /about--> [Server] --> returns full HTML page  (full reload)
[Browser] --GET /contact-> [Server] --> returns full HTML page (full reload)
```

**Every navigation = a full page reload.**

Examples of MPAs: traditional WordPress sites, early PHP apps (Laravel Blade views), Ruby on Rails with ERB templates.

---

## What Is a Single Page Application?

A **Single Page Application (SPA)** loads a single HTML file once. Subsequent navigation is handled entirely on the client side by JavaScript — the page **never fully reloads**.

```
[Browser] --GET /--> [Server] --> returns ONE HTML file + JS bundle
[Browser] navigates to /about   --> JS intercepts, renders new view in-place (no server request)
[Browser] navigates to /contact --> JS intercepts, renders new view in-place (no server request)
```

The "single page" refers to the single HTML document, not a single screen. SPAs can have hundreds of distinct "pages" (views) — they just render them without a full page reload.

The initial HTML returned by the server is almost empty:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>My App</title>
</head>
<body>
  <div id="root"></div>   <!-- The entire app mounts here -->
  <script type="module" src="/assets/main.js"></script>
</body>
</html>
```

The JavaScript bundle (e.g., React) downloads, executes, and **populates** the `#root` div with the entire UI. Every subsequent interaction is handled by JavaScript — no page reload occurs.

---

## How SPAs Work Internally

### Step-by-step boot sequence

```
1. Request     — Browser requests GET /
2. Shell download — Server returns the thin HTML shell + JS/CSS assets
3. JS execution — Browser parses and executes the JS bundle (React, Vue, etc.)
4. Initial render — Framework renders the home view into #root
5. API calls   — App fetches data from backend APIs (REST or GraphQL)
6. Re-render   — Framework updates only the changed DOM nodes with new data
```

### Virtual DOM (React's approach)

React maintains an in-memory tree of UI components (the Virtual DOM). When state changes:

1. React re-renders the affected component in the Virtual DOM
2. React **diffs** the new tree against the previous tree (reconciliation)
3. React applies only the **minimum set of real DOM updates**

This makes updates fast — there is no full page repaint.

```
State change → Virtual DOM update → Diff (reconciliation) → Minimal real DOM patch
```

---

## Client-Side Routing

Navigation in an SPA does not send requests to the server. Instead, a **client-side router** intercepts URL changes and renders the matching component.

### The History API

Modern browsers expose `window.history.pushState()`, which lets JavaScript change the browser URL **without triggering a page load**:

```js
// Push a new URL into browser history without reloading the page
window.history.pushState({}, '', '/about');
// The URL bar now shows /about — but no HTTP request was sent
```

Client-side routers (React Router, Vue Router) use this API under the hood.

### React Router example

```jsx
import { BrowserRouter, Routes, Route, Link } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
        <Link to="/contact">Contact</Link>
      </nav>

      <Routes>
        <Route path="/"        element={<Home />} />
        <Route path="/about"   element={<About />} />
        <Route path="/contact" element={<Contact />} />
        <Route path="*"        element={<NotFound />} />
      </Routes>
    </BrowserRouter>
  );
}
```

Clicking `<Link to="/about">` calls `history.pushState('/about')`, the router reads the new pathname, and renders `<About />` — no server round-trip occurs.

### Hash routing (legacy)

Before the History API, SPAs used URL hashes (`/#/about`). The hash portion is never sent to the server, so the server always received `GET /`. Hash routing still works but produces ugly URLs.

```
http://example.com/#/about   ← hash routing (legacy)
http://example.com/about     ← pushState routing (modern)
```

### Server-side fallback requirement

Because the server only knows about `/`, if a user bookmarks `https://app.com/about` and opens it fresh, the server must return the same `index.html` for **any** path. Configure your server to serve `index.html` for all unmatched routes:

```nginx
# Nginx configuration for a SPA
location / {
  try_files $uri $uri/ /index.html;
}
```

---

## SPA vs MPA Comparison

| Aspect | SPA | MPA |
|---|---|---|
| Initial load | Slower (large JS bundle) | Faster (server returns ready HTML) |
| Subsequent navigation | Instant (no server round-trip) | Slower (full page reload) |
| User experience | App-like, smooth transitions | Page blink on every navigation |
| SEO | Requires extra work | Excellent (server sends real HTML) |
| Server load | Low (serves static assets + APIs) | Higher (server builds each page) |
| Caching | JS bundle cacheable aggressively | Harder to cache dynamic pages |
| Offline support | Possible with Service Workers | Very limited |
| State management | Complex (must manage in-browser state) | Simple (state lives on server) |
| Back/forward button | Must be handled explicitly | Works natively |
| Development complexity | Higher (bundler, routing, state) | Lower |
| Best for | Highly interactive apps (dashboards, editors) | Content-heavy sites, blogs, e-commerce |

---

## Building a SPA — Tooling and Frameworks

### Popular frameworks

| Framework | Language | Approach |
|---|---|---|
| React | JS / TS | Library — compose with React Router, Redux, etc. |
| Vue | JS / TS | Progressive framework, gentler learning curve |
| Angular | TypeScript | Full opinionated framework, built-in router/forms/DI |
| Svelte | JS / TS | Compiles away at build time, tiny runtime |

### Project setup with Vite + React

```bash
npm create vite@latest my-spa -- --template react-ts
cd my-spa
npm install
npm run dev
```

Vite's dev server starts in milliseconds. For production:

```bash
npm run build    # outputs dist/ with optimized, hashed assets
npm run preview  # local preview of the production build
```

The output `dist/index.html` is the single HTML shell. All assets are content-hashed for long-term browser caching.

---

## Data Fetching in SPAs

SPAs communicate with the backend via HTTP APIs (typically REST or GraphQL). JavaScript calls the API and updates the UI without any page navigation.

### Using the Fetch API

```js
// GET — fetch a list of users
async function fetchUsers() {
  const response = await fetch('https://api.example.com/users');
  if (!response.ok) throw new Error(`HTTP error: ${response.status}`);
  return response.json();
}

// POST — create a new user
async function createUser(user) {
  const response = await fetch('https://api.example.com/users', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(user),
  });
  return response.json();
}
```

### React Query (recommended pattern)

React Query adds automatic caching, background refetching, and loading/error states on top of plain fetch:

```jsx
import { useQuery, QueryClient, QueryClientProvider } from '@tanstack/react-query';

const queryClient = new QueryClient();

function UserList() {
  const { data, isLoading, error } = useQuery({
    queryKey: ['users'],
    queryFn: () => fetch('/api/users').then(r => r.json()),
  });

  if (isLoading) return <p>Loading...</p>;
  if (error)     return <p>Error: {error.message}</p>;

  return <ul>{data.map(u => <li key={u.id}>{u.name}</li>)}</ul>;
}

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <UserList />
    </QueryClientProvider>
  );
}
```

---

## Code Splitting and Lazy Loading

A large SPA ships a large JS bundle. Code splitting breaks this into smaller chunks that load only when needed, reducing the initial download.

### React.lazy + Suspense

```jsx
import { lazy, Suspense } from 'react';

// Instead of: import Dashboard from './Dashboard';
const Dashboard = lazy(() => import('./Dashboard'));
const Settings  = lazy(() => import('./Settings'));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <Routes>
        <Route path="/dashboard" element={<Dashboard />} />
        <Route path="/settings"  element={<Settings />} />
      </Routes>
    </Suspense>
  );
}
```

Vite/Webpack automatically creates a separate `.js` chunk for each lazy-loaded route. The browser downloads the `Dashboard` chunk only when the user first navigates to `/dashboard`.

### Route-based vs component-based splitting

| Strategy | What gets split | Best for |
|---|---|---|
| Route-based | Each page route | Most apps — easy, high impact |
| Component-based | Heavy components (charts, rich-text editors) | Libraries loaded conditionally |

---

## SEO Challenges and Solutions

Search engine crawlers historically struggled with SPAs because the HTML shell is empty — content is injected by JavaScript after the page loads. This has improved but remains a concern.

### Solutions

| Solution | How it works | Complexity |
|---|---|---|
| **SSR (Server-Side Rendering)** | Server runs your JS and returns pre-built HTML | High — requires a Node server |
| **SSG (Static Site Generation)** | Build tool generates all HTML pages at build time | Medium — works for static content |
| **Pre-rendering / Snapshot** | A headless browser renders pages and saves the HTML | Low — no server changes |
| **Dynamic rendering** | Serve pre-rendered HTML to bots, SPA to real users | Medium |

**Next.js** (for React) provides SSR and SSG with the same React codebase:

```jsx
// pages/about.tsx — Next.js SSR
export async function getServerSideProps() {
  const data = await fetchData();
  return { props: { data } };
}

export default function About({ data }) {
  return <main>{data.title}</main>;
}
// Server sends fully-formed HTML — crawlers see real content immediately
```

---

## Authentication in SPAs

SPAs cannot use traditional session cookies easily because they talk to APIs from the browser. Two common patterns:

### JWT stored in sessionStorage (common)

```js
// After login, store the token
async function login(credentials) {
  const res = await fetch('/api/auth/login', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(credentials),
  });
  const { token } = await res.json();
  sessionStorage.setItem('token', token); // survives navigation, cleared on tab close
}

// Attach token to every subsequent API request
async function apiRequest(url, options = {}) {
  const token = sessionStorage.getItem('token');
  return fetch(url, {
    ...options,
    headers: {
      ...options.headers,
      Authorization: `Bearer ${token}`,
    },
  });
}
```

> **Security note:** Storing tokens in `localStorage` exposes them to XSS attacks. Prefer `httpOnly` cookies or in-memory storage for sensitive applications.

### Cookie-based auth (most secure)

The server sets an `httpOnly` cookie after login. JavaScript cannot read it. The browser automatically sends it on every same-origin request — no manual header attachment needed. Use a CSRF token for mutation requests.

---

## Performance Considerations

| Technique | Description |
|---|---|
| **Code splitting** | Split bundle by route — ship only what is needed per page |
| **Tree shaking** | Remove unused exports at build time (Vite/Webpack do this automatically) |
| **Asset preloading** | `<link rel="preload">` critical resources before they are needed |
| **Image optimization** | WebP format, lazy loading with `loading="lazy"`, responsive sizes |
| **Memoization** | `React.memo`, `useMemo`, `useCallback` prevent needless re-renders |
| **Virtualization** | Render only visible list items — use `react-window` or `@tanstack/virtual` |
| **Service Worker caching** | Cache assets for instant repeat loads (see PWA notes) |

---

## When to Use an SPA

**Choose SPA when:**
- The product is a highly interactive app (dashboard, editor, chat, project management)
- Users spend extended sessions in the app without needing fast first-load page counts
- Offline capability is needed
- You want a clean separation between frontend and backend teams

**Avoid SPA (prefer SSR/MPA) when:**
- SEO is critical and you cannot add SSR infrastructure
- Content is mostly static (blog, marketing site, documentation)
- Initial load performance is paramount for users on slow networks
- The project is small and the added complexity of client-side routing is not justified

---

## Pros and Cons

### Pros
- Smooth, app-like user experience after the initial load
- Reduced server load — server only serves APIs and static files
- Clear separation of frontend and backend concerns
- Rich tooling ecosystem (Vite, React, TypeScript, React Query)
- Easy to add offline capability with Service Workers

### Cons
- Larger initial JS bundle → slower first contentful paint
- SEO requires extra work (SSR or pre-rendering)
- Browser history and deep-linking require explicit management
- JavaScript errors can break the entire UI (no fallback HTML)
- State management complexity grows significantly as the app scales
