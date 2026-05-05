# React — Internationalization (i18n)

## Table of contents

- [React — Internationalization (i18n)](#react--internationalization-i18n)
  - [Table of contents](#table-of-contents)
  - [i18n Concepts](#i18n-concepts)
    - [What is i18n?](#what-is-i18n)
    - [i18n vs l10n](#i18n-vs-l10n)
    - [Key Vocabulary](#key-vocabulary)
    - [The Problem: Hard-Coded Strings Don't Scale](#the-problem-hard-coded-strings-dont-scale)
    - [How Translation Libraries Work](#how-translation-libraries-work)
    - [Pluralisation and Interpolation](#pluralisation-and-interpolation)
    - [Right-to-Left (RTL) Languages](#right-to-left-rtl-languages)
    - [Date, Number, and Currency Formatting](#date-number-and-currency-formatting)
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

## i18n Concepts

### What is i18n?

**Internationalization (i18n)** is the process of designing your app so it can be adapted to different languages and regions **without changing the source code** for each one.

The "18" in i18n stands for the 18 letters between the "i" and the "n" in "internationalization".

A fully internationalised app can support French, Arabic, Japanese, and English by only swapping out a translation file — no code changes needed.

### i18n vs l10n

These two terms are often confused:

| Term | Full name | What it means |
|---|---|---|
| **i18n** | Internationalization | Building the infrastructure that *allows* translation |
| **l10n** | Localization | Actually *translating* and adapting content for a specific locale |

```
i18n = making your app translation-ready
l10n = French translator fills in the French strings
```

You do i18n once (in code). l10n is done per language, often by translators.

### Key Vocabulary

| Term | Definition | Example |
|---|---|---|
| **Locale** | A language + region combination | `en-US`, `fr-FR`, `ar-SA`, `zh-TW` |
| **Translation key** | A stable identifier for a piece of text | `"nav.home"`, `"errors.required"` |
| **Namespace** | A group of related translation keys | `"common"`, `"auth"`, `"dashboard"` |
| **Fallback locale** | Used when a key is missing in the current locale | `en` fallback when `fr` key is missing |
| **Pluralization** | Different text for singular vs plural | "1 item" vs "3 items" |
| **Interpolation** | Inserting dynamic values into translated strings | `"Welcome, {{name}}"` |

### The Problem: Hard-Coded Strings Don't Scale

```tsx
// ❌ Hard-coded — you'd need to duplicate this component for each language
function Greeting() {
  return <h1>Welcome, Alice!</h1>;
}

// ❌ Manual translation with a variable — still doesn't scale
const messages = { en: 'Welcome', fr: 'Bienvenue' };
function Greeting({ lang }: { lang: string }) {
  return <h1>{messages[lang]}, Alice!</h1>;
}
// Problems: plurals, date formats, word order varies by language,
// no tooling support, translators can't work with this
```

### How Translation Libraries Work

Translation libraries separate text from code using **JSON translation files**:

```json
// locales/en/translation.json
{ "greeting": "Welcome, {{name}}!" }

// locales/fr/translation.json
{ "greeting": "Bienvenue, {{name}} !" }
```

Your component only references the **key**, never the actual string:

```tsx
// Component is language-agnostic
const { t } = useTranslation();
return <h1>{t('greeting', { name: 'Alice' })}</h1>;
// Outputs "Welcome, Alice!" in English, "Bienvenue, Alice !" in French
```

When the user switches language, the library swaps the translation file and React re-renders — no component changes needed.

### Pluralisation and Interpolation

Different languages have very different plural rules. English has two forms (1 item / N items). Russian has four. Arabic has six.

i18n libraries handle this with special plural keys:

```json
// English
{ "itemCount": "You have {{count}} item", "itemCount_other": "You have {{count}} items" }

// Russian needs four forms: _one, _few, _many, _other
{ "itemCount_one": "У вас {{count}} товар", "itemCount_few": "У вас {{count}} товара", ... }
```

**Interpolation** lets you embed dynamic values safely:

```tsx
t('greeting', { name: user.name })   // "Welcome, Alice!"
t('itemCount', { count: 3 })         // "You have 3 items"
```

### Right-to-Left (RTL) Languages

Arabic, Hebrew, Persian, and Urdu are written right-to-left. Your layout must mirror:

```css
/* HTML attribute — set on <html> element */
html[dir="rtl"] .sidebar { right: 0; left: auto; }

/* Logical properties — automatically flip in RTL */
padding-inline-start: 16px; /* = padding-left in LTR, padding-right in RTL */
margin-inline-end: 8px;
```

```tsx
// Set dir and lang on the html element based on locale
<html lang={locale} dir={locale === 'ar' ? 'rtl' : 'ltr'}>
```

### Date, Number, and Currency Formatting

Formatting rules vary enormously by locale. Never format these manually — use the built-in `Intl` API:

```ts
// Dates
new Intl.DateTimeFormat('en-US').format(new Date()) // "5/5/2026"
new Intl.DateTimeFormat('de-DE').format(new Date()) // "5.5.2026"
new Intl.DateTimeFormat('ar-SA').format(new Date()) // "١٤٤٧/١١/٧"

// Numbers
new Intl.NumberFormat('en-US').format(1234567)  // "1,234,567"
new Intl.NumberFormat('de-DE').format(1234567)  // "1.234.567"

// Currency
new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD' }).format(42.5)  // "$42.50"
new Intl.NumberFormat('fr-FR', { style: 'currency', currency: 'EUR' }).format(42.5)  // "42,50 €"
```

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
