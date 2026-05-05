# Frontend System Design & Architecture

## Table of contents

- [Frontend System Design \& Architecture](#frontend-system-design--architecture)
  - [Table of contents](#table-of-contents)
  - [Core Principles](#core-principles)
  - [Component Design Principles](#component-design-principles)
  - [Application Architecture Patterns](#application-architecture-patterns)
  - [Folder Structure](#folder-structure)
  - [Micro-Frontends](#micro-frontends)
  - [Monorepos](#monorepos)
  - [Design Systems and Component Libraries](#design-systems-and-component-libraries)
  - [API Design from the Frontend Perspective](#api-design-from-the-frontend-perspective)
  - [State Architecture](#state-architecture)
  - [Rendering Strategies](#rendering-strategies)
  - [Scalability Patterns](#scalability-patterns)
  - [Interview System Design Framework](#interview-system-design-framework)

---

## Core Principles

1. **Separation of Concerns** — UI, business logic, and data fetching should be separate
2. **Single Responsibility** — each module/component does one thing well
3. **DRY** — Don't Repeat Yourself — but don't abstract prematurely
4. **Composition over Inheritance** — prefer composing small components
5. **Colocation** — keep related code together (styles, tests, types near component)
6. **Loose Coupling** — components shouldn't depend on each other's internals

---

## Component Design Principles

### Atomic Design

Organize components in a hierarchy from smallest to largest:

```
Atoms       → Button, Input, Badge, Icon
Molecules   → SearchBar (Input + Button), FormField (Label + Input + Error)
Organisms   → Navbar (Logo + Nav + Search + Profile), ProductCard
Templates   → PageLayout, DashboardLayout
Pages       → HomePage, ProductPage (instances of templates with real data)
```

### Smart vs Dumb Components

| | Smart (Container) | Dumb (Presentational) |
|---|---|---|
| State | ✅ Local/global state | ❌ Stateless (mostly) |
| Data fetching | ✅ | ❌ |
| Business logic | ✅ | ❌ |
| Props | Passes data down | Receives data, fires events up |
| Reusability | Low | High |
| Testing | Integration | Unit |

```tsx
// Dumb / Presentational component
function UserCard({ user, onFollow }: { user: User; onFollow: () => void }) {
  return (
    <div className="card">
      <img src={user.avatar} alt={user.name} />
      <h3>{user.name}</h3>
      <button onClick={onFollow}>Follow</button>
    </div>
  );
}

// Smart / Container component
function UserCardContainer({ userId }: { userId: string }) {
  const { data: user } = useQuery({ queryKey: ['user', userId], queryFn: () => fetchUser(userId) });
  const follow = useMutation({ mutationFn: () => followUser(userId) });

  if (!user) return <Skeleton />;
  return <UserCard user={user} onFollow={() => follow.mutate()} />;
}
```

### Compound Components Pattern

```tsx
// Components that share implicit state — used in design systems
function Tabs({ children, defaultValue }: TabsProps) {
  const [activeTab, setActiveTab] = useState(defaultValue);
  return (
    <TabsContext.Provider value={{ activeTab, setActiveTab }}>
      <div className="tabs">{children}</div>
    </TabsContext.Provider>
  );
}

Tabs.List = function TabsList({ children }) {
  return <div role="tablist">{children}</div>;
};

Tabs.Tab = function Tab({ value, children }) {
  const { activeTab, setActiveTab } = useTabsContext();
  return (
    <button role="tab" aria-selected={activeTab === value} onClick={() => setActiveTab(value)}>
      {children}
    </button>
  );
};

// Usage — clean, readable API
<Tabs defaultValue="posts">
  <Tabs.List>
    <Tabs.Tab value="posts">Posts</Tabs.Tab>
    <Tabs.Tab value="likes">Likes</Tabs.Tab>
  </Tabs.List>
</Tabs>
```

### Render Props and Custom Hooks

```tsx
// Render prop — share stateful logic
function DataFetcher({ url, render }: { url: string; render: (data: any) => ReactNode }) {
  const { data, loading } = useFetch(url);
  return loading ? <Spinner /> : render(data);
}

<DataFetcher url="/api/users" render={(users) => <UserList users={users} />} />

// Custom hook — preferred modern approach
function useDebounce<T>(value: T, delay: number): T {
  const [debounced, setDebounced] = useState(value);
  useEffect(() => {
    const timer = setTimeout(() => setDebounced(value), delay);
    return () => clearTimeout(timer);
  }, [value, delay]);
  return debounced;
}

function SearchInput() {
  const [query, setQuery] = useState('');
  const debouncedQuery = useDebounce(query, 300);
  const results = useSearchResults(debouncedQuery);
  // ...
}
```

---

## Application Architecture Patterns

### Feature-Based Architecture

```
src/
  features/
    auth/
      components/
        LoginForm.tsx
        AuthGuard.tsx
      hooks/
        useAuth.ts
      store/
        authSlice.ts
      api/
        auth.api.ts
      types/
        auth.types.ts
      index.ts              ← public API (barrel export)
    
    products/
      components/
      hooks/
      store/
      api/
      index.ts
  
  shared/
    components/             ← reusable UI (Button, Modal, etc.)
    hooks/                  ← shared hooks (useDebounce, useLocalStorage)
    utils/
    types/
    lib/                    ← third-party wrappers
  
  app/
    router.tsx
    store.ts
    App.tsx
```

### Layer Architecture

```
Presentation Layer   → React components, pages, layout
Application Layer    → hooks, state management, use cases
Domain Layer         → business entities, types, business logic
Infrastructure Layer → API clients, local storage, websocket
```

---

## Folder Structure

**Large-scale project:**
```
src/
  app/            ← app-level setup (providers, router, global store)
  pages/          ← route-level components
  features/       ← feature modules (see above)
  shared/         
    ui/            ← design system components
    hooks/
    utils/
    constants/
    types/
  assets/         ← images, fonts, icons
  styles/         ← global CSS, theme
  config/         ← environment config
  tests/          ← test utilities, mocks
```

**Colocation rule:**
```
features/
  products/
    ProductCard.tsx
    ProductCard.test.tsx    ← test next to the component
    ProductCard.stories.tsx ← Storybook story next to the component
    ProductCard.module.css  ← styles next to the component
```

---

## Micro-Frontends

Breaking a large frontend into independently deployable pieces owned by different teams.

### Approaches

| Approach | Description | Tools |
|---|---|---|
| **Module Federation** | Share code at runtime via webpack | Webpack 5, Rspack |
| **iframes** | Complete isolation, hard to style | — |
| **Web Components** | Browser-native custom elements | Lit, Stencil |
| **Single-SPA** | Router-level composition | single-spa |
| **NPM packages** | Shared components as packages | — |

### Module Federation (Webpack 5)

```js
// host/webpack.config.js
plugins: [
  new ModuleFederationPlugin({
    name: 'host',
    remotes: {
      productApp: 'productApp@http://localhost:3001/remoteEntry.js',
      cartApp: 'cartApp@http://localhost:3002/remoteEntry.js',
    },
    shared: { react: { singleton: true }, 'react-dom': { singleton: true } },
  }),
],

// host/src/App.tsx
const ProductList = React.lazy(() => import('productApp/ProductList'));
const Cart = React.lazy(() => import('cartApp/Cart'));

// remote/webpack.config.js  
plugins: [
  new ModuleFederationPlugin({
    name: 'productApp',
    filename: 'remoteEntry.js',
    exposes: {
      './ProductList': './src/components/ProductList',
    },
  }),
],
```

### Key Challenges

- **Shared dependencies** — avoid loading React twice (use `singleton: true`)
- **Styling conflicts** — use CSS Modules, Shadow DOM, or scoped class prefixes
- **Communication** — use custom events, shared state via URL, or event bus
- **Auth** — centralized auth service or token passing
- **Routing** — parent router vs child router coordination

---

## Monorepos

A single repository containing multiple related packages/apps.

```
monorepo/
  apps/
    web/              ← Next.js app
    mobile/           ← React Native app
    admin/            ← Vite admin panel
  packages/
    ui/               ← shared component library
    utils/            ← shared utility functions
    types/            ← shared TypeScript types
    config/           ← shared eslint/tsconfig
  package.json        ← root (workspaces)
  turbo.json          ← Turborepo config
```

**Tools: Turborepo, Nx, pnpm workspaces, Lerna**

```json
// package.json (root)
{
  "workspaces": ["apps/*", "packages/*"]
}

// apps/web/package.json
{
  "dependencies": {
    "@myorg/ui": "workspace:*",
    "@myorg/utils": "workspace:*"
  }
}
```

```bash
# Turborepo — parallel builds with caching
turbo run build          # build all apps and packages in parallel
turbo run test --filter=@myorg/ui  # run only ui package tests
```

---

## Design Systems and Component Libraries

A design system provides a shared visual language and component API for consistency.

```
design-system/
  tokens/           ← colors, typography, spacing, shadow (as CSS vars or JS)
  primitives/       ← low-level: Box, Text, Stack, Grid
  components/       ← Button, Card, Modal, Form
  patterns/         ← composed: DataTable, PageHeader
  docs/             ← Storybook
```

**Design tokens:**
```ts
// tokens.ts
export const tokens = {
  colors: {
    primary: { 50: '#eff6ff', 500: '#3b82f6', 900: '#1e3a8a' },
    danger:  { 500: '#ef4444' },
  },
  spacing: { 1: '0.25rem', 2: '0.5rem', 4: '1rem', 8: '2rem' },
  fontSize: { sm: '0.875rem', base: '1rem', lg: '1.125rem' },
  borderRadius: { md: '0.375rem', full: '9999px' },
};
```

**Storybook for component documentation:**
```tsx
// Button.stories.tsx
import type { Meta, StoryObj } from '@storybook/react';
const meta: Meta<typeof Button> = {
  component: Button,
  tags: ['autodocs'],
};
export default meta;

export const Primary: StoryObj<typeof Button> = {
  args: { variant: 'primary', children: 'Click me' },
};
```

---

## API Design from the Frontend Perspective

**BFF (Backend for Frontend):**
- A server layer tailored to the frontend's data needs
- Aggregates multiple microservices, reduces over-fetching
- Can be a Next.js API route, tRPC, or GraphQL server

**Data fetching strategies:**
```tsx
// REST — multiple endpoints, potential over/under-fetching
const user = await fetch('/api/users/1').then(r => r.json());
const posts = await fetch('/api/users/1/posts').then(r => r.json());

// GraphQL — ask for exactly what you need
const { data } = useQuery(gql`
  query GetUser($id: ID!) {
    user(id: $id) {
      name
      posts { title createdAt }
    }
  }
`, { variables: { id } });

// tRPC — end-to-end type safety, no schema
const user = trpc.user.getById.useQuery(id);
const createPost = trpc.post.create.useMutation();
```

---

## State Architecture

See also: [StateManagement.md](./StateManagement.md)

**Categorize state, then choose the right layer:**

```
URL State           → route params, query params (?page=2&filter=active)
Server State        → React Query / SWR (API data, cache)
Global UI State     → Zustand / Context (theme, sidebar, user preferences)
Form State          → react-hook-form
Local UI State      → useState (open/close, hover)
```

**Avoid these anti-patterns:**
```tsx
// ❌ Storing server data in Redux/Zustand (React Query does this better)
dispatch(setUsers(await fetchUsers()))  // duplicates server cache in global store

// ❌ useEffect for data fetching in new apps
useEffect(() => { fetch(url).then(...) }, []);  // use React Query instead

// ❌ Single global store for everything
// ✅ Split into domain-specific stores or use server state tools for API data
```

---

## Rendering Strategies

| Strategy | When rendered | When to use |
|---|---|---|
| **CSR** (Client-Side Rendering) | On client after JS loads | Dashboards, auth-gated apps |
| **SSR** (Server-Side Rendering) | On server per request | SEO-critical, personalized pages |
| **SSG** (Static Site Generation) | At build time | Marketing pages, blogs |
| **ISR** (Incremental Static Regen) | At build + periodically | E-commerce, news |
| **RSC** (React Server Components) | On server, stream to client | Data-heavy pages, no interactivity needed |

**Next.js strategy per-route:**
```tsx
// SSG — no dynamic data
export const dynamic = 'force-static';

// SSR — fresh every request
export const dynamic = 'force-dynamic';

// ISR — revalidate every 60 seconds
export const revalidate = 60;

// Dynamic route with SSG
export async function generateStaticParams() {
  const posts = await getPosts();
  return posts.map(p => ({ slug: p.slug }));
}
```

---

## Scalability Patterns

**Code Splitting and Lazy Loading:**
```tsx
// Route-level splitting (React Router / Next.js does this automatically)
const Dashboard = React.lazy(() => import('./pages/Dashboard'));
const Analytics = React.lazy(() => import('./pages/Analytics'));

<Suspense fallback={<PageLoader />}>
  <Routes>
    <Route path="/dashboard" element={<Dashboard />} />
    <Route path="/analytics" element={<Analytics />} />
  </Routes>
</Suspense>

// Component-level splitting
const RichTextEditor = React.lazy(() => import('./RichTextEditor'));
// Load only when the editor is actually opened
```

**Virtualization for long lists:**
```tsx
import { useVirtualizer } from '@tanstack/react-virtual';

function VirtualList({ items }) {
  const parentRef = useRef<HTMLDivElement>(null);
  const virtualizer = useVirtualizer({
    count: items.length,
    getScrollElement: () => parentRef.current,
    estimateSize: () => 60,
  });

  return (
    <div ref={parentRef} style={{ height: '500px', overflow: 'auto' }}>
      <div style={{ height: virtualizer.getTotalSize() }}>
        {virtualizer.getVirtualItems().map(vItem => (
          <div key={vItem.key} style={{ transform: `translateY(${vItem.start}px)` }}>
            <ListItem item={items[vItem.index]} />
          </div>
        ))}
      </div>
    </div>
  );
}
```

---

## Interview System Design Framework

When asked to design a frontend system (e.g., "Design a real-time dashboard"):

1. **Clarify requirements** — users, scale, features, real-time needs?
2. **Define components/pages** — sketch UI, identify main views
3. **Component breakdown** — atomic design, smart vs dumb
4. **State management** — what state, where does it live?
5. **Data fetching** — REST/GraphQL/WebSocket, caching strategy
6. **Performance** — code splitting, lazy loading, virtualization
7. **Rendering strategy** — CSR, SSR, SSG, RSC?
8. **Scalability** — micro-frontends, monorepo, design system
9. **Accessibility** — semantic HTML, keyboard nav, ARIA
10. **Testing** — unit, integration, e2e strategy
