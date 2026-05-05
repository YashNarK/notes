# React — Internationalization (i18n)

Internationalization enables your app to serve users in multiple languages and locales, handling translations, date/number formatting, and pluralisation.

## Table of contents

- [React — Internationalization (i18n)](#react--internationalization-i18n)
  - [Table of contents](#table-of-contents)
  - [Plain React](#plain-react)
    - [react-i18next (Recommended)](#react-i18next-recommended)
    - [Setup](#setup)
    - [Using Translations in Components](#using-translations-in-components)
    - [Pluralisation \& Interpolation](#pluralisation--interpolation)
    - [Date \& Number Formatting](#date--number-formatting)
  - [Next.js](#nextjs)
    - [next-intl (Recommended)](#next-intl-recommended)
    - [Setup](#setup-1)
    - [Using Translations](#using-translations)
    - [Locale-aware Routing](#locale-aware-routing)
    - [Server vs Client Components](#server-vs-client-components)

---

## Plain React

### react-i18next (Recommended)

**i18next** is the most popular i18n framework; **react-i18next** provides React bindings.

```bash
npm i i18next react-i18next i18next-browser-languagedetector i18next-http-backend
```

### Setup

```
src/
  i18n/
    index.ts
    locales/
      en/translation.json
      fr/translation.json
```

```json
// src/i18n/locales/en/translation.json
{
  "welcome": "Welcome, {{name}}!",
  "itemCount": "You have {{count}} item",
  "itemCount_other": "You have {{count}} items",
  "nav": {
    "home": "Home",
    "about": "About"
  }
}
```

```json
// src/i18n/locales/fr/translation.json
{
  "welcome": "Bienvenue, {{name}} !",
  "itemCount": "Vous avez {{count}} article",
  "itemCount_other": "Vous avez {{count}} articles",
  "nav": {
    "home": "Accueil",
    "about": "À propos"
  }
}
```

```ts
// src/i18n/index.ts
import i18n from 'i18next';
import { initReactI18next } from 'react-i18next';
import HttpBackend from 'i18next-http-backend';
import LanguageDetector from 'i18next-browser-languagedetector';

i18n
  .use(HttpBackend)             // load translations from /public/locales
  .use(LanguageDetector)        // detect browser language
  .use(initReactI18next)
  .init({
    fallbackLng: 'en',
    supportedLngs: ['en', 'fr'],
    interpolation: { escapeValue: false }, // React already escapes
    backend: { loadPath: '/locales/{{lng}}/{{ns}}.json' },
  });

export default i18n;
```

```tsx
// main.tsx
import './i18n';
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode><App /></React.StrictMode>
);
```

### Using Translations in Components

```tsx
import { useTranslation } from 'react-i18next';

export function Navbar() {
  const { t, i18n } = useTranslation();

  const switchLang = (lng: string) => i18n.changeLanguage(lng);

  return (
    <nav>
      <a href="/">{t('nav.home')}</a>
      <a href="/about">{t('nav.about')}</a>
      <button onClick={() => switchLang('en')}>EN</button>
      <button onClick={() => switchLang('fr')}>FR</button>
    </nav>
  );
}
```

### Pluralisation & Interpolation

```tsx
export function CartSummary({ name, count }: { name: string; count: number }) {
  const { t } = useTranslation();
  return (
    <p>
      {t('welcome', { name })}   {/* "Welcome, Alice!" */}
      {t('itemCount', { count })} {/* "You have 1 item" / "You have 3 items" */}
    </p>
  );
}
```

### Date & Number Formatting

Use the native `Intl` API for locale-aware formatting without extra libraries:

```tsx
function formatDate(date: Date, locale: string) {
  return new Intl.DateTimeFormat(locale, { dateStyle: 'long' }).format(date);
  // "May 5, 2026" (en) / "5 mai 2026" (fr)
}

function formatCurrency(amount: number, locale: string, currency: string) {
  return new Intl.NumberFormat(locale, { style: 'currency', currency }).format(amount);
  // "$1,234.56" (en-US/USD) / "1 234,56 €" (fr-FR/EUR)
}
```

---

## Next.js

### next-intl (Recommended)

**next-intl** is purpose-built for Next.js App Router with full RSC support.

```bash
npm i next-intl
```

### Setup

```
messages/
  en.json
  fr.json
app/
  [locale]/
    layout.tsx
    page.tsx
middleware.ts
next.config.ts
```

```json
// messages/en.json
{
  "HomePage": {
    "title": "Hello world!",
    "description": "Welcome to our app."
  }
}
```

```ts
// middleware.ts
import createMiddleware from 'next-intl/middleware';

export default createMiddleware({
  locales: ['en', 'fr'],
  defaultLocale: 'en',
});

export const config = { matcher: ['/((?!api|_next|.*\\..*).*)'] };
```

```ts
// next.config.ts
import createNextIntlPlugin from 'next-intl/plugin';
const withNextIntl = createNextIntlPlugin();
export default withNextIntl({ /* next config */ });
```

```ts
// i18n.ts (next-intl config)
import { getRequestConfig } from 'next-intl/server';

export default getRequestConfig(async ({ locale }) => ({
  messages: (await import(`./messages/${locale}.json`)).default,
}));
```

### Using Translations

```tsx
// app/[locale]/layout.tsx
import { NextIntlClientProvider } from 'next-intl';
import { getMessages } from 'next-intl/server';

export default async function LocaleLayout({
  children,
  params: { locale },
}: {
  children: React.ReactNode;
  params: { locale: string };
}) {
  const messages = await getMessages();
  return (
    <html lang={locale}>
      <body>
        <NextIntlClientProvider messages={messages}>
          {children}
        </NextIntlClientProvider>
      </body>
    </html>
  );
}
```

```tsx
// app/[locale]/page.tsx — Server Component (no 'use client')
import { useTranslations } from 'next-intl';

export default function HomePage() {
  const t = useTranslations('HomePage');
  return (
    <main>
      <h1>{t('title')}</h1>
      <p>{t('description')}</p>
    </main>
  );
}
```

### Locale-aware Routing

With the `[locale]` folder convention, every route is automatically prefixed:

| File | Route (en) | Route (fr) |
|---|---|---|
| `app/[locale]/page.tsx` | `/en` | `/fr` |
| `app/[locale]/about/page.tsx` | `/en/about` | `/fr/about` |

Use `next-intl`'s `Link` for locale-aware navigation:

```tsx
'use client';
import { Link } from '@/navigation'; // generated by next-intl/navigation
// or
import { useRouter, usePathname } from '@/navigation';
```

### Server vs Client Components

next-intl supports both RSC and Client components:

| Context | Import |
|---|---|
| Server Component | `import { useTranslations } from 'next-intl'` |
| Client Component | `import { useTranslations } from 'next-intl'` (same API, needs Provider) |
| Server Action / util | `import { getTranslations } from 'next-intl/server'` |

```tsx
// Server Action example
import { getTranslations } from 'next-intl/server';

export async function sendWelcomeEmail(userId: string) {
  const t = await getTranslations('Emails');
  const subject = t('welcomeSubject');
  // ...send email
}
```
