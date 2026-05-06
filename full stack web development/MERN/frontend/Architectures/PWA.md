# Progressive Web Application (PWA)

## Table of contents

- [Progressive Web Application (PWA)](#progressive-web-application-pwa)
  - [Table of contents](#table-of-contents)
  - [What Is a PWA?](#what-is-a-pwa)
  - [The Three Pillars of a PWA](#the-three-pillars-of-a-pwa)
  - [Service Workers](#service-workers)
  - [Web App Manifest](#web-app-manifest)
  - [HTTPS Requirement](#https-requirement)
  - [The App Shell Architecture](#the-app-shell-architecture)
  - [Caching Strategies](#caching-strategies)
  - [Offline-First Design](#offline-first-design)
  - [Background Sync](#background-sync)
  - [Push Notifications](#push-notifications)
  - [Installation — Add to Home Screen](#installation--add-to-home-screen)
  - [PWA vs Native App vs SPA](#pwa-vs-native-app-vs-spa)
  - [Auditing with Lighthouse](#auditing-with-lighthouse)
  - [When to Build a PWA](#when-to-build-a-pwa)
  - [Pros and Cons](#pros-and-cons)

---

## What Is a PWA?

A **Progressive Web Application (PWA)** is a web application that uses modern browser APIs to deliver a native-app-like experience — including offline access, push notifications, and home screen installation — while remaining built with standard web technologies (HTML, CSS, JavaScript).

The word "progressive" means the app **works for every user** regardless of browser choice, but **progressively enhances** the experience for users on capable browsers.

| Browser capability | What the user gets |
|---|---|
| Any browser | A working website |
| Modern browser, not installed | Offline caching, fast repeat loads |
| Modern browser + installed | App icon, fullscreen mode, push notifications |

PWAs close the gap between web apps and native mobile/desktop apps without requiring the App Store or Google Play.

**Real-world PWAs:** Twitter Lite, Starbucks, Flipkart, Pinterest, Uber — all built as PWAs to serve users on slow networks or low-memory devices. Twitter Lite reduced data usage by 70% and increased engagement by 65%.

---

## The Three Pillars of a PWA

A PWA must satisfy three core requirements:

```
1. HTTPS            — Secure connection required
2. Service Worker   — Background script for offline support and caching
3. Web App Manifest — JSON file that describes the "app" identity
```

All three must be present for a browser to offer the "Add to Home Screen" installation prompt.

---

## Service Workers

### What is a Service Worker?

A **Service Worker** is a JavaScript file that runs in the **background**, in a thread completely separate from the browser's main thread. It acts as a **programmable network proxy** — intercepting every network request the web page makes and deciding how to respond.

```
Without Service Worker:
[Page] --fetch /api/data--> [Network] ---> [Server]

With Service Worker:
[Page] --fetch /api/data--> [Service Worker] --if cached?--> [Cache Storage]
                                              |
                                              --if not cached--> [Network] ---> [Server]
```

### Service Worker lifecycle

```
1. Registration — Page calls navigator.serviceWorker.register('/sw.js')
2. Installation — sw.js runs the `install` event (pre-cache static assets here)
3. Activation   — Old SW is replaced; clean up stale caches here
4. Idle         — SW sleeps to save battery; woken up by fetch/push/sync events
5. Termination  — Browser terminates idle SWs (they restart on the next event)
```

### Registering a Service Worker

In your main `main.tsx` / `index.js`:

```js
// Only register if the browser supports Service Workers
if ('serviceWorker' in navigator) {
  window.addEventListener('load', async () => {
    try {
      const registration = await navigator.serviceWorker.register('/sw.js');
      console.log('SW registered. Scope:', registration.scope);
    } catch (error) {
      console.error('SW registration failed:', error);
    }
  });
}
```

The `scope` defaults to the directory of `sw.js`. A SW at `/sw.js` controls all pages under `/`.

### Writing a Service Worker

```js
// sw.js — must live at the root of your project (public/sw.js)
const CACHE_NAME = 'my-app-v1';

const PRECACHE_URLS = [
  '/',
  '/index.html',
  '/assets/main.css',
  '/assets/main.js',
  '/offline.html',
];

// ── INSTALL: Pre-cache critical assets ────────────────────────────────────────
self.addEventListener('install', (event) => {
  event.waitUntil(
    caches.open(CACHE_NAME).then((cache) => cache.addAll(PRECACHE_URLS))
  );
  self.skipWaiting(); // Activate immediately without waiting for old SW to finish
});

// ── ACTIVATE: Clean up old caches ─────────────────────────────────────────────
self.addEventListener('activate', (event) => {
  event.waitUntil(
    caches.keys().then((keys) =>
      Promise.all(
        keys
          .filter((key) => key !== CACHE_NAME)
          .map((key) => caches.delete(key))
      )
    )
  );
  self.clients.claim(); // Take control of all open pages immediately
});

// ── FETCH: Intercept every network request ────────────────────────────────────
self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request).then((cached) => {
      if (cached) return cached;       // Serve from cache if available
      return fetch(event.request);     // Otherwise fetch from network
    })
  );
});
```

### Important Service Worker rules

- SWs **only work over HTTPS** (or `localhost` for development)
- SWs **cannot access the DOM** — they run in a separate thread with no window/document access
- SWs are **event-driven** — they wake up to handle events and then go back to sleep
- A new SW only takes control after all tabs running the old SW are closed — unless `skipWaiting()` + `clients.claim()` is used

---

## Web App Manifest

The **Web App Manifest** is a JSON file that tells the browser metadata about your app: name, icons, theme colors, and how it should launch.

```json
{
  "name": "My Awesome App",
  "short_name": "AwesomeApp",
  "description": "A full-featured progressive web app",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#ffffff",
  "theme_color": "#0f172a",
  "orientation": "portrait-primary",
  "icons": [
    {
      "src": "/icons/icon-192.png",
      "sizes": "192x192",
      "type": "image/png",
      "purpose": "any maskable"
    },
    {
      "src": "/icons/icon-512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ],
  "screenshots": [
    {
      "src": "/screenshots/home.png",
      "sizes": "1280x800",
      "type": "image/png",
      "form_factor": "wide"
    }
  ]
}
```

Link it from your HTML `<head>`:

```html
<link rel="manifest" href="/manifest.json" />
<meta name="theme-color" content="#0f172a" />
<!-- iOS-specific meta tags (Safari does not read manifest fully) -->
<meta name="apple-mobile-web-app-capable" content="yes" />
<meta name="apple-mobile-web-app-status-bar-style" content="default" />
<link rel="apple-touch-icon" href="/icons/icon-192.png" />
```

### Display modes

| Value | Appearance |
|---|---|
| `standalone` | Looks like a native app — no browser address bar or navigation UI |
| `fullscreen` | Covers the entire screen, including status bar (used for games) |
| `minimal-ui` | A small browser navigation bar remains visible |
| `browser` | Normal browser tab — not a PWA-like experience |

---

## HTTPS Requirement

Service Workers require a secure connection because they intercept **all** network requests. If a rogue SW could be registered over plain HTTP, an attacker on the same network could inject malicious responses for every page visit.

**Exception:** `localhost` always works without HTTPS, for development convenience.

---

## The App Shell Architecture

The **App Shell** is the minimal HTML/CSS/JS needed to render the outer UI of your app — navigation bar, sidebar, footer — without any dynamic content.

```
App Shell = skeleton of the UI (loads instantly from cache)
Content   = data fetched from the API (loads as soon as network responds)
```

```
First visit:
  1. SW pre-caches the shell during install
  2. Page renders the shell
  3. Page fetches content from API → data fills in

Second visit (even offline):
  1. SW serves the shell from cache → UI appears in milliseconds
  2. App shows cached content or a friendly "You are offline" state for fresh data
```

This pattern makes **perceived performance** excellent — users see the shell immediately, then data fills in asynchronously.

---

## Caching Strategies

The Service Worker can implement different caching strategies depending on the type of resource:

### 1. Cache First (offline-first)

Check the cache first; only go to the network if the cache misses. Best for static assets that rarely change.

```js
self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request).then((cached) => cached || fetch(event.request))
  );
});
```

### 2. Network First (always-fresh)

Try the network first; fall back to cache if the network fails. Best for API responses where fresh data matters most.

```js
self.addEventListener('fetch', (event) => {
  event.respondWith(
    fetch(event.request)
      .then((response) => {
        // Clone the response before consuming it — responses are single-use streams
        const clone = response.clone();
        caches.open(CACHE_NAME).then((cache) => cache.put(event.request, clone));
        return response;
      })
      .catch(() => caches.match(event.request)) // Network failed — serve stale cache
  );
});
```

### 3. Stale-While-Revalidate

Return the cached version immediately, then fetch from the network and update the cache in the background. Best for assets where near-fresh is good enough.

```js
self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.open(CACHE_NAME).then((cache) =>
      cache.match(event.request).then((cached) => {
        const networkFetch = fetch(event.request).then((response) => {
          cache.put(event.request, response.clone()); // Update cache in background
          return response;
        });
        return cached || networkFetch; // Return cached immediately if available
      })
    )
  );
});
```

### 4. Cache Only

Only serve from cache — never hit the network. Used for assets that were pre-cached during install.

### 5. Network Only

Always go to the network — skip the cache entirely. Used for non-GET requests (POST, PUT, DELETE) where caching would be dangerous.

### Strategy decision guide

| Resource type | Recommended strategy |
|---|---|
| HTML shell / App shell | Cache First |
| CSS / JS bundles (hashed filenames) | Cache First |
| API responses (user data, feeds) | Network First |
| Images and media | Stale-While-Revalidate |
| Form submissions (POST/PUT/DELETE) | Network Only |

---

## Offline-First Design

**Offline-first** means designing your app to work without a network connection by default, and treating the network as an enhancement rather than a requirement.

### Design principles

1. **Cache the shell** — The app should render instantly, even offline
2. **Cache recent data** — Show the last-known state when the network is unavailable
3. **Queue writes** — If the user submits a form offline, queue it for delivery later
4. **Communicate clearly** — Tell the user when they are offline and which data may be stale

```js
// Detect online/offline status
window.addEventListener('online', () => {
  document.getElementById('status').textContent = 'Back online!';
  syncPendingData(); // Send any queued changes
});

window.addEventListener('offline', () => {
  document.getElementById('status').textContent = 'You are offline. Changes will sync when reconnected.';
});

// Check status on load
if (!navigator.onLine) {
  showOfflineBanner();
}
```

---

## Background Sync

**Background Sync** lets a Service Worker defer network operations until the user has a stable connection — even if they have already closed the browser tab.

```js
// In the page — queue a sync when offline
async function submitForm(data) {
  if (!navigator.onLine) {
    await saveToPendingQueue(data);         // Save to IndexedDB
    const sw = await navigator.serviceWorker.ready;
    await sw.sync.register('sync-forms');  // Register a sync tag
    showMessage('Saved! Will sync when back online.');
  } else {
    await sendToAPI(data);
  }
}

// In sw.js — the browser fires this event when connectivity is restored
self.addEventListener('sync', (event) => {
  if (event.tag === 'sync-forms') {
    event.waitUntil(syncPendingForms());
  }
});

async function syncPendingForms() {
  const pending = await getPendingFromQueue(); // Read from IndexedDB
  for (const item of pending) {
    await fetch('/api/submissions', {
      method: 'POST',
      body: JSON.stringify(item),
      headers: { 'Content-Type': 'application/json' },
    });
    await removeFromQueue(item.id);
  }
}
```

---

## Push Notifications

PWAs can receive push notifications from a server even when the app is not open.

### How it works

```
1. User grants notification permission in the browser
2. Browser generates a unique push subscription (endpoint URL + encryption keys)
3. App sends the subscription object to your server (save it in a database)
4. Server sends a push message to the browser's push service (Google FCM, Apple APNS)
5. Browser wakes up the Service Worker
6. SW receives the push event and displays a notification
```

### Requesting permission and subscribing

```js
async function subscribeToPush() {
  const permission = await Notification.requestPermission();
  if (permission !== 'granted') return;

  const sw = await navigator.serviceWorker.ready;
  const subscription = await sw.pushManager.subscribe({
    userVisibleOnly: true,  // Must be true — prevents hidden/silent pushes
    applicationServerKey: urlBase64ToUint8Array(PUBLIC_VAPID_KEY),
  });

  // Send the subscription to your server to store it
  await fetch('/api/push/subscribe', {
    method: 'POST',
    body: JSON.stringify(subscription),
    headers: { 'Content-Type': 'application/json' },
  });
}
```

### Handling push events in the Service Worker

```js
// sw.js
self.addEventListener('push', (event) => {
  const data = event.data?.json() ?? { title: 'New notification', body: '' };

  event.waitUntil(
    self.registration.showNotification(data.title, {
      body: data.body,
      icon: '/icons/icon-192.png',
      badge: '/icons/badge.png',
      data: { url: data.url }, // Store the URL to open on click
    })
  );
});

self.addEventListener('notificationclick', (event) => {
  event.notification.close();
  event.waitUntil(
    clients.openWindow(event.notification.data.url)
  );
});
```

---

## Installation — Add to Home Screen

When all three pillars are satisfied (HTTPS + Service Worker + Manifest), modern browsers show an installation prompt.

### The `beforeinstallprompt` event

```js
let deferredPrompt;

// Browser fires this when the PWA is installable
window.addEventListener('beforeinstallprompt', (event) => {
  event.preventDefault();                                   // Suppress the browser's automatic prompt
  deferredPrompt = event;                                   // Save it to trigger on your own button
  document.getElementById('install-btn').style.display = 'block';
});

document.getElementById('install-btn').addEventListener('click', async () => {
  deferredPrompt.prompt();                                  // Show the install dialog
  const { outcome } = await deferredPrompt.userChoice;
  console.log(`Install outcome: ${outcome}`);              // 'accepted' or 'dismissed'
  deferredPrompt = null;
  document.getElementById('install-btn').style.display = 'none';
});
```

Once installed:
- The app appears on the home screen or desktop with its own icon
- It opens in `standalone` mode — no browser address bar or tabs
- It gets its own entry in the OS app switcher
- `display-mode: standalone` CSS media query returns `true`

```css
@media (display-mode: standalone) {
  /* Styles that only apply when running as an installed PWA */
  .browser-hint { display: none; }
}
```

---

## PWA vs Native App vs SPA

| Feature | SPA (web only) | PWA | Native App |
|---|---|---|---|
| Offline support | No | Yes | Yes |
| Push notifications | No | Yes | Yes |
| Home screen install | No | Yes | Yes |
| App store distribution | No | Optional | Required |
| Access to device hardware | Very limited | Moderate | Full |
| Update delivery | Instant (deploy to server) | Instant (no store review) | App store review required |
| Bundle size | Small JS bundle | Small JS bundle | 10s–100s of MB |
| Development cost | Low | Low–Medium | High (iOS + Android separate) |
| Performance ceiling | Browser-limited | Browser-limited | Native (highest) |
| Cross-platform | Yes | Yes | No (must build per OS) |

---

## Auditing with Lighthouse

Google's **Lighthouse** tool (built into Chrome DevTools) audits your PWA against a checklist of requirements.

```
DevTools → Lighthouse tab → Select "Progressive Web App" → Analyze page
```

Key checks Lighthouse performs:

| Check | Why it matters |
|---|---|
| Registers a Service Worker | Core PWA requirement |
| Web App Manifest exists with required fields | Required for installation |
| Responds with 200 when offline | App Shell caching works |
| Has icons of the correct sizes | Renders properly when installed |
| Sets a theme color | Browser chrome matches the app brand |
| Content sized correctly for viewport | Mobile usability |
| Served over HTTPS | Security requirement |

### Running Lighthouse from the CLI

```bash
npm install -g lighthouse
lighthouse https://your-app.com --view
```

---

## When to Build a PWA

**Build a PWA when:**
- Your users are on mobile or slow networks (Twitter Lite reduced data usage by 70%)
- Offline capability is needed but you cannot afford a native app
- Push notifications are important for user re-engagement
- You want one codebase that works on web, mobile, and desktop as an installed app
- You want to add offline/install features incrementally to an existing SPA

**Skip PWA (use native or SSR) when:**
- Deep device hardware access is needed (complex Bluetooth, NFC, AR camera)
- App Store discoverability is critical to the business model
- Maximum frame-rate performance is required (AAA games, AR/VR)

---

## Pros and Cons

### Pros
- Single codebase works across desktop, mobile, and tablet
- Installable without an App Store — no 30% revenue cut, no review process delays
- Instant updates — no waiting for app store approval
- Works offline or on flaky networks
- Progressive enhancement — still functional in older browsers
- Dramatically smaller download size compared to native apps

### Cons
- Full native hardware APIs are not accessible (limited Bluetooth, NFC, USB)
- iOS Safari historically lagged in PWA support (push notifications only available since iOS 16.4)
- Service Worker debugging and cache invalidation can be complex
- Installed feel is not identical to native, especially on iOS
- Background Sync is not yet supported in all major browsers
