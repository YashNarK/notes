# Multi-Frontend Architecture (Micro-Frontends)

## Table of contents

- [Multi-Frontend Architecture (Micro-Frontends)](#multi-frontend-architecture-micro-frontends)
  - [Table of contents](#table-of-contents)
  - [Starting Point — What Is a Frontend?](#starting-point--what-is-a-frontend)
  - [The Monolith Frontend Problem](#the-monolith-frontend-problem)
  - [What Is a Micro-Frontend?](#what-is-a-micro-frontend)
  - [Core Concepts and Mental Model](#core-concepts-and-mental-model)
  - [Integration Approaches](#integration-approaches)
    - [1. Build-Time Integration (NPM Packages)](#1-build-time-integration-npm-packages)
    - [2. Runtime Integration via iFrames](#2-runtime-integration-via-iframes)
    - [3. Runtime Integration via JavaScript (Script Tags)](#3-runtime-integration-via-javascript-script-tags)
    - [4. Module Federation (Webpack 5)](#4-module-federation-webpack-5)
    - [5. Server-Side Composition](#5-server-side-composition)
  - [Communication Between Micro-Frontends](#communication-between-micro-frontends)
    - [The Communication Problem](#the-communication-problem)
    - [1. Custom Browser Events](#1-custom-browser-events)
    - [2. Shared Event Bus (Pub/Sub)](#2-shared-event-bus-pubsub)
    - [3. URL and Query Parameters](#3-url-and-query-parameters)
    - [4. Shared Global State (Window Object)](#4-shared-global-state-window-object)
    - [5. Props and Callbacks (Module Federation)](#5-props-and-callbacks-module-federation)
    - [6. PostMessage API (iFrame Communication)](#6-postmessage-api-iframe-communication)
    - [7. Shared Backend / BFF (API-Mediated State)](#7-shared-backend--bff-api-mediated-state)
    - [8. Browser Storage (localStorage / sessionStorage / IndexedDB)](#8-browser-storage-localstorage--sessionstorage--indexeddb)
    - [Communication Strategy Comparison](#communication-strategy-comparison)
    - [Recommended Combination for Production](#recommended-combination-for-production)
  - [Routing in Micro-Frontends](#routing-in-micro-frontends)
  - [Shared Dependencies](#shared-dependencies)
  - [Styling Isolation](#styling-isolation)
  - [Authentication Across Micro-Frontends](#authentication-across-micro-frontends)
  - [Team Organization — Conway's Law](#team-organization--conways-law)
  - [When to Use Micro-Frontends](#when-to-use-micro-frontends)
  - [Pros and Cons](#pros-and-cons)
  - [Real-World Walkthrough — E-Commerce Platform](#real-world-walkthrough--e-commerce-platform)

---

## Starting Point — What Is a Frontend?

A **frontend** is everything users see and interact with in a browser — the buttons, forms, layouts, and animations. It is built with HTML, CSS, and JavaScript (or TypeScript).

In a modern web app, the frontend is a **JavaScript application** (React, Vue, Angular, etc.) that:

- Renders the UI based on data and user interactions
- Manages client-side state (what is shown, what is selected, what is loading)
- Communicates with backend APIs to fetch and persist data
- Handles routing — navigation between different views without full page reloads

Typically, an entire company's frontend lives in **one codebase**. This is called a **monolith frontend** and works perfectly well when the team and product are small.

---

## The Monolith Frontend Problem

Imagine a large e-commerce platform with these distinct product areas:

- Product catalog and search
- Shopping cart and checkout
- User profile and account settings
- Order tracking
- Reviews and ratings
- Admin dashboard

All of these are built inside **one massive React codebase**, owned by dozens of engineers across multiple teams.

### Problems that emerge at scale

**1. Slow builds and slow tests**

The entire app must be compiled and tested on every change — even if one team only changed a single button in the checkout flow.

```
[Engineer changes a button in Checkout]
  → Rebuild the entire 500 MB bundle
  → Run 8 000 unit/integration tests
  → Deploy everything
  → 40-minute pipeline for one button
```

**2. Tight coupling**

A change in a shared `Button` component can silently break the checkout page and the product catalog simultaneously — features that should be independent are entangled through shared code.

**3. Deployment bottlenecks**

All teams must coordinate releases on a shared deployment schedule. One team's unfinished feature can block another team's critical bugfix from reaching production.

**4. Scaling teams is difficult**

A codebase shared by 15 teams creates constant merge conflicts, unclear code ownership, and review bottlenecks. Onboarding a new engineer takes weeks because the codebase is enormous.

**5. Technology lock-in**

If the app was built in React 2018, upgrading to a modern framework means rewriting everything at once — a multi-month engineering effort.

---

## What Is a Micro-Frontend?

A **Micro-Frontend** is an architectural style where a frontend application is **decomposed into smaller, independently deployable pieces**, each owned by a separate autonomous team.

The concept is the **frontend equivalent of Microservices** — just as a backend is split into many small services instead of one monolith, the frontend is split into many small apps instead of one.

```
MONOLITH FRONTEND
┌──────────────────────────────────────────────────────┐
│                 Big React Application                │
│   ┌──────────┐   ┌──────────┐   ┌─────────────┐     │
│   │ Catalog  │   │ Checkout │   │   Account   │     │
│   └──────────┘   └──────────┘   └─────────────┘     │
│    ONE build, ONE deploy, ONE team owns all of it   │
└──────────────────────────────────────────────────────┘

MICRO-FRONTENDS
┌────────────┐    ┌────────────┐    ┌─────────────┐
│  Catalog   │    │  Checkout  │    │   Account   │
│   Team     │    │   Team     │    │   Team      │
│  (React)   │    │   (Vue)    │    │  (React)    │
│  own repo  │    │  own repo  │    │  own repo   │
│  own CI/CD │    │  own CI/CD │    │  own CI/CD  │
└────────────┘    └────────────┘    └─────────────┘
       ↓                ↓                  ↓
┌──────────────────────────────────────────────────────┐
│              Shell / Container Application           │
│   (orchestrates which MFE renders where, and when)  │
└──────────────────────────────────────────────────────┘
```

Each micro-frontend:
- Is a **self-contained application** with its own codebase and repository
- Has its **own CI/CD pipeline** — teams deploy independently without coordinating
- Can use a **different technology stack** (one team React, another Vue)
- Is **owned end-to-end by one cross-functional team** (frontend + backend + design + QA)

---

## Core Concepts and Mental Model

### The Shell (Container Application)

The **Shell** (also called the Container or Host) is the top-level application that:

1. Renders the outer layout shared across all features (navigation bar, header, footer)
2. Controls top-level routing — which MFE is active for each URL segment
3. Loads the appropriate Micro-Frontend dynamically based on the current route
4. Optionally provides shared services (auth context, analytics, design tokens)

```
User visits /checkout
         ↓
Shell App receives the URL
Shell sees /checkout → load Checkout MFE
Shell mounts Checkout MFE inside the main content area
Checkout MFE renders independently, fetches its own data
```

### The Micro-Frontends (Remotes)

Each MFE is a **self-contained app** that:
- Exposes its root component (or build artifact) for the Shell to consume
- Handles its own sub-routing (e.g., `/checkout/cart`, `/checkout/payment`)
- Manages its own internal state — it does not reach into other MFEs' stores
- Communicates directly with its own backend services

---

## Integration Approaches

There are five main ways to compose micro-frontends into one user-facing application. The choice determines how independently teams can deploy and how easy cross-MFE communication is.

### 1. Build-Time Integration (NPM Packages)

Each team publishes their MFE as an **npm package**. The Shell installs all packages and bundles them together at build time into one artifact.

```
Team Checkout  → publishes @company/checkout@1.2.3 to npm
Team Catalog   → publishes @company/catalog@2.0.0 to npm
Shell          → npm install @company/checkout @company/catalog → builds one bundle
```

```jsx
// Shell's package.json
{
  "dependencies": {
    "@company/checkout": "1.2.3",
    "@company/catalog":  "2.0.0"
  }
}

// Shell's App.jsx
import { CheckoutApp } from '@company/checkout';
import { CatalogApp }  from '@company/catalog';

function App() {
  return (
    <Routes>
      <Route path="/catalog/*"  element={<CatalogApp />} />
      <Route path="/checkout/*" element={<CheckoutApp />} />
    </Routes>
  );
}
```

**Critical limitation:** Checkout cannot be deployed without the Shell also being rebuilt and redeployed. This defeats the core purpose of independent deployment.

**When to use:** Small teams, simple apps, or when you just want shared component libraries — not true independent deployment.

---

### 2. Runtime Integration via iFrames

Each MFE is a completely independent web application hosted at its own URL. The Shell embeds them using HTML `<iframe>` elements.

```jsx
// Shell App
function CheckoutSection() {
  return (
    <iframe
      src="https://checkout.company.com"
      style={{ width: '100%', height: '600px', border: 'none' }}
      title="Checkout"
    />
  );
}
```

Each MFE runs at its own domain:
```
https://shell.company.com      ← Shell App (the outer layout)
https://catalog.company.com    ← Catalog MFE (in iframe)
https://checkout.company.com   ← Checkout MFE (in iframe)
https://account.company.com    ← Account MFE (in iframe)
```

**Advantages:**
- Maximum isolation — each MFE runs in its own browsing context; no shared globals
- No CSS or JS conflicts whatsoever
- Independent deployment is trivial — change one URL
- Any tech stack works

**Disadvantages:**
- Communication between Shell and iframe requires the `postMessage` API (complex)
- Responsive design is difficult — iframes do not resize fluidly
- The browser URL and history are not shared — deep-linking breaks
- Accessibility (keyboard focus management, screen readers) does not cross iframe boundaries
- SEO is very poor — crawlers cannot index iframe content

**When to use:** Legacy system integration, embedding a third-party or partner app, maximum isolation requirements for security reasons.

---

### 3. Runtime Integration via JavaScript (Script Tags)

Each MFE exposes a **JavaScript bundle** at a public URL. The Shell dynamically loads these bundles at runtime. No npm packages, no build-time coupling.

```html
<!-- shell/index.html — the Shell's HTML file -->
<div id="catalog-container"></div>
<div id="checkout-container"></div>

<!-- Bundles loaded at runtime from each MFE's CDN URL -->
<script src="https://catalog.company.com/bundle.js"></script>
<script src="https://checkout.company.com/bundle.js"></script>
```

Each MFE bundle registers a mount/unmount interface on `window`:

```js
// catalog/src/index.js — the Catalog MFE bundle
window.CatalogMFE = {
  mount(container, { onProductSelected }) {
    // Mount the React app into the container element provided by the Shell
    const root = createRoot(container);
    root.render(<CatalogApp onProductSelected={onProductSelected} />);
  },
  unmount(container) {
    // Clean up when the Shell removes this MFE from the page
    ReactDOM.unmountComponentAtNode(container);
  },
};
```

The Shell mounts and unmounts MFEs programmatically:

```jsx
// Shell — mounts the Catalog MFE into a container div
useEffect(() => {
  const container = document.getElementById('catalog-container');
  window.CatalogMFE.mount(container, {
    onProductSelected: (product) => {
      // Handle event fired from Catalog MFE
      setCartItems(prev => [...prev, product]);
    },
  });
  return () => window.CatalogMFE.unmount(container); // Clean up on route change
}, []);
```

**Advantages:**
- True runtime integration — each team deploys independently; the Shell does not rebuild
- MFEs share the same DOM, URL bar, and browser history
- More flexible and more natural than iFrames

**Disadvantages:**
- Relying on `window.X` as a global registry is fragile and untyped
- CSS and JS namespacing must be managed manually to avoid collisions
- Risk of loading multiple copies of React if not coordinated

---

### 4. Module Federation (Webpack 5)

**Module Federation** is the gold-standard approach for micro-frontends. It is a Webpack 5 feature that allows one JavaScript application to **dynamically import code from another running application at runtime** — as if it were a local module — without any npm publishing or server-side coordination.

```
Checkout app (deployed at https://checkout.company.com) exposes its CheckoutApp component
Shell app (deployed at https://shell.company.com) imports CheckoutApp at runtime — live
```

#### Configuring the Remote (the MFE that exposes code)

```js
// checkout/webpack.config.js
const { ModuleFederationPlugin } = require('webpack').container;

module.exports = {
  plugins: [
    new ModuleFederationPlugin({
      name: 'checkout',              // Unique identifier for this MFE
      filename: 'remoteEntry.js',   // The manifest file the Shell will fetch
      exposes: {
        './CheckoutApp': './src/CheckoutApp', // What this MFE shares with the world
      },
      shared: {
        // Tell Webpack that React should only exist once across all MFEs
        react:     { singleton: true, requiredVersion: '^18.0.0' },
        'react-dom': { singleton: true, requiredVersion: '^18.0.0' },
      },
    }),
  ],
};
```

#### Configuring the Shell (the Host that consumes remote code)

```js
// shell/webpack.config.js
const { ModuleFederationPlugin } = require('webpack').container;

module.exports = {
  plugins: [
    new ModuleFederationPlugin({
      name: 'shell',
      remotes: {
        // Tell the Shell where to find each MFE's manifest at runtime
        checkout: 'checkout@https://checkout.company.com/remoteEntry.js',
        catalog:  'catalog@https://catalog.company.com/remoteEntry.js',
      },
      shared: {
        react:     { singleton: true, requiredVersion: '^18.0.0' },
        'react-dom': { singleton: true, requiredVersion: '^18.0.0' },
      },
    }),
  ],
};
```

#### Using the Remote in the Shell

```jsx
// shell/src/App.jsx
import { lazy, Suspense } from 'react';

// Dynamic import from a remote — looks like a local import, runs from a different server
const CheckoutApp = lazy(() => import('checkout/CheckoutApp'));
const CatalogApp  = lazy(() => import('catalog/CatalogApp'));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <Routes>
        <Route path="/catalog/*"  element={<CatalogApp />} />
        <Route path="/checkout/*" element={<CheckoutApp />} />
      </Routes>
    </Suspense>
  );
}
```

#### How Module Federation works step by step

```
1. Shell's JavaScript starts executing
2. Shell encounters:  import('checkout/CheckoutApp')
3. Shell fetches:     https://checkout.company.com/remoteEntry.js
4. remoteEntry.js tells Webpack which chunks to download for CheckoutApp
5. Webpack downloads only the needed chunk (not the entire Checkout app)
6. Shared singleton (React) is already loaded — not downloaded again
7. CheckoutApp is rendered inside the Shell's DOM
```

**Key advantage — shared singletons:** Without `singleton: true`, both Shell and Checkout MFE would each bundle and load their own copy of React. Two React instances break hooks (hooks depend on a single React reference). The `shared` config ensures React is loaded exactly once by whichever app loads first.

#### Vite Plugin Federation (for Vite-based projects)

```bash
npm install @originjs/vite-plugin-federation --save-dev
```

```js
// vite.config.js (remote — the MFE)
import federation from '@originjs/vite-plugin-federation';

export default {
  plugins: [
    federation({
      name: 'checkout',
      filename: 'remoteEntry.js',
      exposes: { './CheckoutApp': './src/CheckoutApp.jsx' },
      shared: ['react', 'react-dom'],
    }),
  ],
  build: { target: 'esnext' }, // Required for top-level await
};
```

---

### 5. Server-Side Composition

Instead of the browser assembling micro-frontends, a **server** (or CDN edge node) composes them before sending HTML to the browser. The user receives a complete, pre-assembled HTML document.

```
User requests /checkout
         ↓
Nginx / API Gateway / Edge Worker
         ↓
Fetches HTML fragments in parallel from:
  → https://shell-service/header.html
  → https://checkout-service/checkout.html
  → https://shell-service/footer.html
         ↓
Stitches fragments into one complete HTML document
         ↓
Browser receives fully rendered HTML (excellent SEO, fast first paint)
```

Technologies that enable this:

| Technology | Description |
|---|---|
| **Server-Side Includes (SSI)** | Nginx/Apache directive to include remote HTML fragments |
| **Edge Side Includes (ESI)** | CDN-level composition (Varnish, Fastly) |
| **Next.js / Astro** | Framework-level composition with React Server Components |
| **Tailor (Zalando)** | Open-source server-side composition framework used in production |

**Best for:** Content-heavy apps where SEO is critical and server rendering is acceptable. Used by large e-commerce sites (Zalando uses this pattern).

---

## Communication Between Micro-Frontends

> This is the most important and most nuanced aspect of micro-frontend architecture. Read this section carefully.

### The Communication Problem

Micro-frontends are designed to be **isolated** — they run independently, own their state, and deploy separately. But real applications cannot be 100% isolated. Consider:

- A user adds a product to the cart in the **Catalog MFE** → the cart badge in the **Shell** must update
- A user logs in through the **Auth MFE** → every other MFE must know the user is now authenticated
- A user selects a payment method in the **Checkout MFE** → the order summary panel must update

**The challenge:** how do isolated applications share information without creating tight coupling?

**The golden rule:**

```
TIGHT COUPLING (bad) — direct import creates a dependency:
Catalog MFE directly imports CartStore from Checkout MFE
  → Catalog now depends on Checkout's internal store shape
  → Checkout cannot refactor its store without breaking Catalog

LOOSE COUPLING (good) — event-based contract:
Catalog MFE fires an event: "product-added-to-cart" with a payload
Checkout MFE listens for that event and updates its own store
  → Neither MFE knows about the other's internals
  → Either can change freely as long as the event contract is preserved
```

---

### 1. Custom Browser Events

The browser's built-in **CustomEvent API** is the simplest and most framework-agnostic communication mechanism. Events fire on `window` and any MFE anywhere on the page can listen to them.

```js
// ── Catalog MFE — fires an event when the user clicks "Add to Cart" ──────────
function addToCart(product) {
  const event = new CustomEvent('catalog:product-added', {
    detail: {
      id:       product.id,
      name:     product.name,
      price:    product.price,
      quantity: 1,
    },
    bubbles: true, // Event bubbles up to window
  });
  window.dispatchEvent(event);
}
```

```js
// ── Shell App — listens for the event to update the cart badge ────────────────
window.addEventListener('catalog:product-added', (event) => {
  const { id, name, price, quantity } = event.detail;
  setCartCount(prev => prev + quantity);
  showToast(`${name} added to cart`);
});
```

```js
// ── Checkout MFE — also listens to add the item to its own cart list ──────────
window.addEventListener('catalog:product-added', (event) => {
  cartStore.dispatch(addItem(event.detail));
});
```

**Event naming convention:** Use `namespace:action` format (e.g., `catalog:product-added`, `auth:user-logged-in`, `cart:item-removed`). This prevents name collisions between MFEs.

**TypeScript contract for type-safe events:**

```ts
// shared-types/events.ts — published as a shared npm package
export interface ProductAddedEventDetail {
  id:       string;
  name:     string;
  price:    number;
  quantity: number;
}

// Catalog MFE — dispatches a typed event
const event = new CustomEvent<ProductAddedEventDetail>('catalog:product-added', {
  detail: { id, name, price, quantity },
});
window.dispatchEvent(event);

// Shell App — receives the typed event
window.addEventListener('catalog:product-added', (e) => {
  const detail = (e as CustomEvent<ProductAddedEventDetail>).detail;
  setCartCount(prev => prev + detail.quantity);
});
```

**Pros:** Zero runtime dependencies, works across any framework, extremely lightweight.
**Cons:** No type safety without the shared-types pattern; events are fire-and-forget (no response/acknowledgment); hard to trace at scale without tooling.

---

### 2. Shared Event Bus (Pub/Sub)

A more structured alternative: a **shared event bus** singleton that all MFEs import from a shared library. This gives you a named, typed, centralized communication channel.

```js
// shared-lib/eventBus.js — published as @company/event-bus
class EventBus {
  constructor() {
    this._listeners = {};
  }

  on(event, listener) {
    if (!this._listeners[event]) this._listeners[event] = [];
    this._listeners[event].push(listener);
    // Return an unsubscribe function (important for React useEffect cleanup)
    return () => this.off(event, listener);
  }

  off(event, listener) {
    this._listeners[event] = (this._listeners[event] || []).filter(l => l !== listener);
  }

  emit(event, data) {
    (this._listeners[event] || []).forEach(listener => listener(data));
  }
}

// Singleton pattern — ensure all MFEs share the same bus instance
window.__APP_EVENT_BUS__ = window.__APP_EVENT_BUS__ || new EventBus();
export const eventBus = window.__APP_EVENT_BUS__;
```

```js
// Catalog MFE — emits an event
import { eventBus } from '@company/event-bus';

function addToCart(product) {
  eventBus.emit('product:added-to-cart', { ...product, quantity: 1 });
}
```

```jsx
// Checkout MFE — subscribes (React component)
import { eventBus } from '@company/event-bus';
import { useEffect } from 'react';

function CheckoutApp() {
  useEffect(() => {
    // Subscribe and capture the unsubscribe function
    const unsubscribe = eventBus.on('product:added-to-cart', (product) => {
      dispatch(addItem(product));
    });
    return unsubscribe; // Clean up subscription when the component unmounts
  }, []);

  // ... render
}
```

**Compared to CustomEvent:** The event bus is purely in-memory — no DOM interaction. It is faster, easier to unit test (just import the bus and emit), and types can be enforced via TypeScript generics.

**Critical requirement:** All MFEs must receive the **same bus instance** — achieved via the `window.__APP_EVENT_BUS__` singleton pattern or via Module Federation's `shared` config to share the bus package.

---

### 3. URL and Query Parameters

The URL is a **naturally shared state** visible to all MFEs simultaneously. Query parameters and route segments carry information that any MFE can read without any event wiring.

```
https://shop.com/catalog?productId=abc123&highlight=true

Catalog MFE reads: productId to highlight the selected product
Checkout MFE reads: productId to pre-select the item if the user navigates there
```

```js
// Catalog MFE — writes to the URL when the user selects a product
function selectProduct(id) {
  const url = new URL(window.location.href);
  url.searchParams.set('productId', id);
  window.history.pushState({}, '', url); // Changes URL without page reload
}

// Checkout MFE — reads the URL parameter on mount
function getSelectedProductId() {
  return new URLSearchParams(window.location.search).get('productId');
}
```

```js
// Any MFE — listen for URL changes (e.g., when Shell changes routes)
window.addEventListener('popstate', () => {
  const params = new URLSearchParams(window.location.search);
  const productId = params.get('productId');
  if (productId) highlightProduct(productId);
});
```

**Best for:** Sharing filter state, selected items, navigation context. Gives users shareable and bookmarkable URLs — a major UX advantage.

**Limitations:** Only primitive values (strings, numbers) fit cleanly in URLs. Complex objects require serialization (JSON + encodeURIComponent) and produce ugly URLs.

---

### 4. Shared Global State (Window Object)

Attach a shared state object to `window` that all MFEs read and write. The simplest approach — but also the riskiest.

```js
// Shell — initializes a shared namespace on startup
window.__APP_STATE__ = {
  user:      null,
  cartCount: 0,
  theme:     'light',
};

// Auth MFE — writes after login
window.__APP_STATE__.user = { id: 'u123', name: 'Alice', role: 'customer' };

// Catalog MFE — reads current user
const currentUser = window.__APP_STATE__.user;

// Checkout MFE — updates cart count
window.__APP_STATE__.cartCount += 1;
```

**Serious problems with this pattern:**
- No reactivity — other MFEs will not know the value changed unless they poll or you fire a separate event
- Global mutation is error-prone — any MFE can accidentally overwrite the entire object
- No type safety — runtime surprises are common
- Difficult to debug at scale

**Better pattern — combine write with notification:**

```js
// Write to shared state AND fire an event to notify listeners
function setUser(user) {
  window.__APP_STATE__.user = user;
  window.dispatchEvent(new CustomEvent('auth:user-changed', { detail: user }));
}
```

**When to use:** Avoid in production. Use only for simple, rarely-changing configuration (e.g., `window.__APP_CONFIG__ = { apiBaseUrl: '...' }`).

---

### 5. Props and Callbacks (Module Federation)

When using Module Federation, the Shell passes **props and callback functions** directly into the MFE's root component — exactly like standard React props. This is the cleanest and most explicit communication pattern.

```jsx
// Shell App — passes data and callbacks into MFEs as props
import { lazy, Suspense } from 'react';

const CatalogApp = lazy(() => import('catalog/CatalogApp'));

function App() {
  const [cartItems, setCartItems] = useState([]);

  const handleAddToCart = useCallback((product) => {
    setCartItems(prev => [...prev, product]);
    // Optionally also emit an event for the Checkout MFE to pick up
    eventBus.emit('product:added-to-cart', product);
  }, []);

  return (
    <Suspense fallback={<div>Loading Catalog...</div>}>
      <CatalogApp
        onAddToCart={handleAddToCart} // ← callback: child → parent communication
        theme="dark"                  // ← config prop: parent → child
        userId={currentUser?.id}      // ← data prop: parent → child
      />
    </Suspense>
  );
}
```

```jsx
// Catalog MFE root component — receives props from the Shell
export default function CatalogApp({ onAddToCart, theme, userId }) {
  return (
    <div className={`catalog theme-${theme}`}>
      {products.map(product => (
        <ProductCard
          key={product.id}
          product={product}
          onAddToCart={() => onAddToCart(product)} // Calls back to Shell
        />
      ))}
    </div>
  );
}
```

**This is the most maintainable pattern** when using Module Federation — it mirrors standard React data flow, is fully type-safe with TypeScript, and makes every data dependency explicit and traceable.

**Limitation:** Only works with Module Federation (or build-time integration). Cannot be used across `<iframe>` boundaries.

---

### 6. PostMessage API (iFrame Communication)

When MFEs are embedded in `<iframe>` elements, the same-origin policy prevents direct JavaScript access between frames. The **PostMessage API** is the only built-in browser mechanism for cross-frame communication.

```js
// Shell App — sends a message to the Checkout iframe
const iframe = document.getElementById('checkout-iframe');

function sendToCheckout(message) {
  iframe.contentWindow.postMessage(
    message,
    'https://checkout.company.com' // ALWAYS specify the exact target origin — never '*' in production
  );
}

// Example: tell Checkout to pre-fill with a specific product
sendToCheckout({ type: 'PRE_FILL_PRODUCT', payload: { id: 'abc123' } });
```

```js
// Checkout MFE (running inside the iframe) — listens for messages from the Shell
window.addEventListener('message', (event) => {
  // ─────────────────────────────────────────────────────────────────────────
  // CRITICAL SECURITY CHECK — ALWAYS validate the origin before trusting data
  // Skipping this check allows any page (including malicious ones) to send commands
  // ─────────────────────────────────────────────────────────────────────────
  if (event.origin !== 'https://shell.company.com') {
    console.warn('Message from untrusted origin rejected:', event.origin);
    return;
  }

  const { type, payload } = event.data;

  switch (type) {
    case 'PRE_FILL_PRODUCT':
      prefillProductInCart(payload.id);
      break;
    case 'CLEAR_CART':
      clearCart();
      break;
  }
});
```

```js
// Checkout MFE — sends a message back up to the Shell (the parent window)
function notifyShellCartUpdated(count) {
  window.parent.postMessage(
    { type: 'CART_UPDATED', payload: { count } },
    'https://shell.company.com' // Always specify target origin
  );
}
```

**Security rules for PostMessage:**
1. **Never use `'*'` as the `targetOrigin`** — it sends the message to all frames regardless of origin, exposing sensitive data
2. **Always validate `event.origin`** in the receiver before processing the message
3. **Treat `event.data` as untrusted input** — validate its shape before acting on it

---

### 7. Shared Backend / BFF (API-Mediated State)

Instead of communicating directly through the browser, MFEs communicate **indirectly through a shared backend**. One MFE writes data to a server; another MFE reads that same data from the server.

```
Catalog MFE → POST /api/cart/items → Cart Service → stores in database
Checkout MFE → GET /api/cart/items → Cart Service → reads from database → returns cart

Both MFEs are always in sync via the server.
No browser-to-browser event wiring needed at all.
```

**When this is the right approach:**
- Cart state — needs to persist across page refreshes, browser tabs, and devices
- User preferences — must be consistent everywhere the user logs in
- Any data that must survive a page reload or app restart

**Pattern: Backend for Frontend (BFF)**

Each MFE team can own a dedicated backend service that the frontend calls directly. This is the microservices pattern applied to the backend — perfectly complementing micro-frontends.

```
Browser (Shell)
  ├── Catalog MFE  ──────────────→  Catalog API      (products database)
  ├── Checkout MFE ──────────────→  Checkout API     (cart + orders database)
  └── Account MFE  ──────────────→  Account API      (user profile database)
```

---

### 8. Browser Storage (localStorage / sessionStorage / IndexedDB)

Browser storage is accessible to all JavaScript running on the **same origin** — making it a simple shared store without any pub/sub wiring.

```js
// Auth MFE — stores auth token after login
function onLoginSuccess(token) {
  localStorage.setItem('auth_token', token);
  // Also fire an event so other MFEs react immediately (storage has no built-in reactivity within a tab)
  window.dispatchEvent(new CustomEvent('auth:login', { detail: { token } }));
}

// Catalog MFE — reads auth token to attach to API requests
function apiRequest(url) {
  const token = localStorage.getItem('auth_token');
  return fetch(url, {
    headers: { Authorization: `Bearer ${token}` },
  });
}
```

```js
// Listen for storage changes across browser tabs (fires only for other tabs)
window.addEventListener('storage', (event) => {
  if (event.key === 'auth_token' && event.newValue === null) {
    // User logged out in another tab — redirect to login in this tab too
    window.location.href = '/login';
  }
});
```

**Key limitations:**
- The `storage` event does **not** fire within the same tab that made the change — combine with CustomEvent for same-tab reactivity
- `localStorage` is synchronous and can block the main thread for large reads
- `sessionStorage` is NOT shared between tabs — each tab has its own isolated copy
- Any JavaScript on the same origin can read `localStorage` — XSS vulnerabilities can steal tokens

---

### Communication Strategy Comparison

| Strategy | Direction | Framework-agnostic | Type-safe | Works with iFrames | Best for |
|---|---|---|---|---|---|
| Custom Browser Events | Any | Yes | With shared types | No | Simple notifications between any MFEs |
| Shared Event Bus | Any | Yes (shared lib) | Yes | No | Complex typed event flows |
| URL / Query Params | Any | Yes | No | Partially | Shareable state, navigation context |
| Window Object | Any | Yes | No | No | Read-only config — avoid for mutable state |
| Props + Callbacks | Parent → Child / Child → Parent | No (React only) | Yes | No | Module Federation parent→child data |
| PostMessage API | Any (cross-frame) | Yes | With effort | Yes | iFrame-based integration |
| Shared Backend API | Any (async via server) | Yes | Yes (API contract) | Yes | Persistent, cross-session, cross-device state |
| Browser Storage | Any | Yes | No | Partially (same origin) | Auth tokens, lightweight config |

---

### Recommended Combination for Production

No single strategy covers all cases. A production micro-frontend system typically combines several:

```
1. Module Federation (props + callbacks)
   → Parent (Shell) to child (MFE) configuration and callbacks

2. Custom Events or Event Bus
   → Sibling MFE notifications (e.g., Catalog → Checkout for cart updates)

3. Shared Backend API (BFF)
   → Persistent state that must survive refresh (cart, auth session, preferences)

4. URL parameters
   → Navigation context, filter state, deep-linkable UI selections
```

---

## Routing in Micro-Frontends

Routing is split into two independent levels.

### Level 1 — Shell Routing (top-level)

The Shell owns the first URL path segment and decides which MFE to render:

```
/           → Home MFE
/catalog/*  → Catalog MFE
/checkout/* → Checkout MFE
/account/*  → Account MFE
```

```jsx
// Shell App — top-level routing
function Shell() {
  return (
    <Routes>
      <Route path="/"           element={<HomeMFE />} />
      <Route path="/catalog/*"  element={<CatalogMFE />} />
      <Route path="/checkout/*" element={<CheckoutMFE />} />
      <Route path="/account/*"  element={<AccountMFE />} />
    </Routes>
  );
}
```

### Level 2 — MFE Routing (sub-routing)

Each MFE manages its own internal routes independently. The Shell does not know or care about sub-routes inside each MFE.

```
/catalog            → Product grid
/catalog/123        → Product detail page for item 123
/catalog/search     → Search results

/checkout/cart      → Cart review
/checkout/payment   → Payment form
/checkout/confirm   → Order confirmation
```

```jsx
// Catalog MFE — handles its own sub-routes independently
function CatalogApp() {
  return (
    <Routes>
      <Route path="/"        element={<ProductGrid />} />
      <Route path="/:id"     element={<ProductDetail />} />
      <Route path="/search"  element={<SearchResults />} />
    </Routes>
  );
}
```

**Rule:** The Shell owns the first path segment. Everything beneath it is the MFE's exclusive responsibility.

---

## Shared Dependencies

Without coordination, each MFE bundles its own copy of React. Loading four copies of React wastes hundreds of kilobytes of bandwidth and **breaks React hooks** (hooks require a single React reference across the entire page).

### Module Federation `shared` config

```js
// Declare in every MFE and the Shell
shared: {
  react: {
    singleton: true,           // Enforce: only one React instance ever loaded on the page
    requiredVersion: '^18.0.0',
    eager: true,               // Shell loads it upfront; MFEs use Shell's instance
  },
  'react-dom': {
    singleton: true,
    requiredVersion: '^18.0.0',
    eager: true,
  },
  // Share the design system so all MFEs use the same component versions
  '@company/design-system': {
    singleton: true,
    requiredVersion: '^3.0.0',
  },
},
```

With this configuration:
- React is downloaded **once** by the Shell
- All MFEs reuse that single instance transparently
- Webpack emits a warning if a MFE requires an incompatible version — version mismatch is caught at load time, not at runtime

---

## Styling Isolation

Without isolation, a CSS class `.button` defined in the Catalog MFE silently overrides `.button` in the Checkout MFE — causing unpredictable visual bugs that are extremely hard to trace.

| Approach | How it works | Collision risk | Complexity |
|---|---|---|---|
| **CSS Modules** | Generates scoped class names at build time (`.button_a3f9`) | None | Low |
| **CSS-in-JS** (styled-components, Emotion) | Injects scoped styles at runtime with unique class names | None | Low |
| **BEM naming** | `.catalog__button`, `.checkout__button` — namespace manually | Low (if disciplined) | Low (discipline-based) |
| **Shadow DOM** | Encapsulates styles completely in Web Components | None | High |
| **Tailwind CSS** | Utility classes — very low collision risk by design | Very low | Low |

**CSS Modules example (recommended default):**

```css
/* catalog/Button.module.css */
.button {
  background: #0f172a;
  color: white;
  padding: 0.5rem 1rem;
  border-radius: 4px;
}
```

```jsx
// catalog/Button.jsx
import styles from './Button.module.css';

export function Button({ children, onClick }) {
  return (
    <button className={styles.button} onClick={onClick}>
      {children}
    </button>
  );
}
// Renders as: <button class="button_a3f9bc4d">...</button>
// The hash is unique per file — cannot collide with Checkout's .button
```

---

## Authentication Across Micro-Frontends

All MFEs need to know whether the user is authenticated and who they are. Centralizing auth in the Shell prevents each MFE from implementing its own login flow.

### Pattern 1 — Shell handles auth, passes user as a prop

```jsx
// Shell App
function App() {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    // Shell validates the stored token on startup
    validateToken()
      .then(setUser)
      .catch(() => setUser(null))
      .finally(() => setLoading(false));
  }, []);

  if (loading) return <AppLoadingSpinner />;
  if (!user)   return <LoginPage onLogin={setUser} />;

  return (
    <Routes>
      <Route path="/catalog/*"  element={<CatalogMFE user={user} />} />
      <Route path="/checkout/*" element={<CheckoutMFE user={user} />} />
    </Routes>
  );
}
```

### Pattern 2 — Shared auth via events + localStorage (for runtime-loaded MFEs)

```js
// Auth MFE — after successful login
function onLoginSuccess({ token, user }) {
  localStorage.setItem('auth_token', token);
  // Notify all other MFEs currently mounted on the page
  window.dispatchEvent(new CustomEvent('auth:login', { detail: { user } }));
}

// Any MFE — subscribes on mount
window.addEventListener('auth:login',  (e) => setCurrentUser(e.detail.user));
window.addEventListener('auth:logout', ()  => setCurrentUser(null));

// Any MFE — on initial mount, check if already logged in
const token = localStorage.getItem('auth_token');
if (token) {
  validateToken(token)
    .then(user => setCurrentUser(user))
    .catch(() => localStorage.removeItem('auth_token'));
}
```

---

## Team Organization — Conway's Law

> *"Any organization that designs a system will produce a design whose structure is a copy of the organization's communication structure."*
> — Melvin Conway (1967)

Micro-frontends embrace Conway's Law rather than fighting it. Each MFE should map to one **autonomous, vertical team** that owns:

- The frontend feature (the MFE codebase)
- The backend services that power that feature
- The database for those services
- The CI/CD pipeline for their slice
- The monitoring, alerting, and on-call rotation for their domain

```
Catalog Team    → Catalog MFE + Catalog API + Product Database
Checkout Team   → Checkout MFE + Order API + Orders Database
Account Team    → Account MFE + User API + Users Database
Platform Team   → Shell App + Shared Design System + CI/CD templates + observability
```

This organizational alignment means the **Catalog Team can ship a feature on Monday without asking the Checkout Team for permission**. Teams release on their own cadence. No more deployment coordination meetings.

---

## When to Use Micro-Frontends

**Use Micro-Frontends when:**
- You have 5+ frontend teams working on the same codebase — team coordination is painful
- Independent deployment is a hard requirement — teams are blocked on each other's releases
- Different product areas have genuinely different technology needs
- Build and test pipelines take 20+ minutes — slow feedback loops are hurting productivity
- The organization already has a microservices backend — the frontend is the only remaining monolith

**Do NOT use Micro-Frontends when:**
- Your team is small (fewer than 3–4 frontend engineers total)
- The product is early-stage — the architecture is likely to change dramatically
- You do not yet have a mature CI/CD infrastructure to support multiple pipelines
- The communication complexity would cost more than the independence gains

> Start with a well-structured monolith. Extract micro-frontends only when the pain is real and measurable. Premature micro-frontend adoption is one of the most expensive engineering mistakes in frontend development.

---

## Pros and Cons

### Pros
- **Independent deployment** — teams ship features without blocking each other
- **Technology freedom** — different teams can use React, Vue, Angular, or vanilla JS independently
- **Fault isolation** — a runtime error in the Checkout MFE does not crash the Catalog MFE
- **Smaller, manageable codebases** — no more merge conflicts from 10 teams in one repo
- **Scalable team structure** — new teams can own new MFEs without touching existing code
- **Incremental migration** — rewrite one MFE at a time instead of a big-bang rewrite

### Cons
- **Operational complexity** — multiple repos, pipelines, deployments, dashboards, and on-call rotations to manage
- **Inter-MFE communication overhead** — sharing data requires event contracts, not a simple function call
- **UI inconsistency risk** — without a shared design system, each team drifts toward their own visual language
- **Duplicate dependencies** — without Module Federation `shared` config, React loads multiple times
- **High initial setup cost** — Shell orchestration, routing, Module Federation config, and shared auth all require significant investment
- **Testing complexity** — integration tests must run across multiple independently deployed applications

---

## Real-World Walkthrough — E-Commerce Platform

Trace a complete user flow through a micro-frontend architecture to see all the pieces working together.

**Setup:**
```
Shell:         https://shop.com             (React, owns nav + routing)
Catalog MFE:   https://catalog.shop.com     (React, products team)
Checkout MFE:  https://checkout.shop.com    (Vue, payments team)
Account MFE:   https://account.shop.com     (React, user team)
```

---

**Step 1 — User visits https://shop.com/catalog**

```
Browser downloads Shell HTML + JS bundle
Shell validates auth token from localStorage → user is logged in
Shell renders: nav bar, cart badge (count: 0), user avatar
Shell router: /catalog/* → load Catalog MFE
Shell fetches remoteEntry.js from https://catalog.shop.com
React is shared — not downloaded again (already loaded by Shell)
CatalogApp component is dynamically imported and mounted
CatalogApp fetches: GET https://catalog-api.shop.com/products
Products render in a responsive grid
```

---

**Step 2 — User clicks "Add to Cart" on Product A**

```
Catalog MFE calls: onAddToCart(productA)   [via props callback from Shell]
Shell's handleAddToCart executes:
  → setCartCount(prev => prev + 1)         [updates cart badge to 1]
  → eventBus.emit('product:added-to-cart', productA)

Shell re-renders nav bar: cart badge now shows "1"
(No full page reload — just a React state update)
```

---

**Step 3 — User navigates to /checkout**

```
User clicks cart icon in the Shell's nav bar
Shell router: unmounts CatalogApp, mounts CheckoutApp
Shell fetches remoteEntry.js from https://checkout.shop.com
CheckoutApp (Vue) mounts inside Shell's React tree
CheckoutApp listens for 'product:added-to-cart' events — already received it in Step 2
CheckoutApp also fetches: GET https://checkout-api.shop.com/cart
Server returns cart with Product A (persisted in Step 2's backend write)
Cart page renders: Product A with quantity 1, price, and checkout button
```

---

**Step 4 — User completes purchase**

```
User fills payment form and submits
CheckoutApp sends: POST https://checkout-api.shop.com/orders
Server returns: { orderId: 'ord_x1y2z3' }
CheckoutApp fires: new CustomEvent('checkout:order-placed', { detail: { orderId: 'ord_x1y2z3' } })

Shell listens:
  → setCartCount(0)                         [clears cart badge]
  → showSuccessNotification('Order placed!')

Shell navigates to /account/orders
AccountMFE mounts and fetches: GET https://account-api.shop.com/orders
User's order history renders, showing the brand-new order at the top
```

All of this happens without a single full page reload. Each team deployed their MFE independently — the Checkout team shipped a payment bug fix at 2 PM without waiting for the Catalog or Account teams.
