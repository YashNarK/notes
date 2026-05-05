# Frontend Testing

## Table of contents

- [Frontend Testing](#frontend-testing)
  - [Table of contents](#table-of-contents)
  - [Testing Pyramid](#testing-pyramid)
  - [Jest Fundamentals](#jest-fundamentals)
  - [React Testing Library (RTL)](#react-testing-library-rtl)
  - [Testing User Interactions](#testing-user-interactions)
  - [Async Testing](#async-testing)
  - [Mocking](#mocking)
  - [Testing Hooks](#testing-hooks)
  - [Snapshot Testing](#snapshot-testing)
  - [Code Coverage](#code-coverage)
  - [Vitest (Vite alternative to Jest)](#vitest-vite-alternative-to-jest)
  - [Testing Best Practices](#testing-best-practices)
  - [Overview and Comparison (Playwright vs Puppeteer)](#overview-and-comparison)
  - [Playwright Setup](#playwright-setup)
  - [Playwright Core API](#playwright-core-api)
    - [Navigation](#navigation)
    - [Interactions](#interactions)
    - [Waiting](#waiting)
    - [Dialogs](#dialogs)
  - [Locators (Playwright)](#locators-playwright)
  - [Assertions (Playwright)](#assertions-playwright)
  - [Page Object Model (POM)](#page-object-model-pom)
  - [Playwright Configuration](#playwright-configuration)
  - [Handling Authentication](#handling-authentication)
  - [Network Interception](#network-interception)
  - [Screenshots and Visual Testing](#screenshots-and-visual-testing)
  - [Puppeteer Fundamentals](#puppeteer-fundamentals)
  - [CI/CD Integration](#cicd-integration)
  - [Intelligent Workflows and Scraping](#intelligent-workflows-and-scraping)

---

## Testing Pyramid

```
        ╱▔▔▔▔▔▔▔╲
       ╱   E2E    ╲     Few, slow, expensive
      ╱─────────────╲   (Playwright, Cypress)
     ╱  Integration  ╲  More, moderate speed
    ╱─────────────────╲ (RTL + MSW)
   ╱     Unit Tests    ╲ Many, fast, cheap
  ╱───────────────────── (Jest, Vitest)
```

| Type | What it tests | Speed | Confidence |
|---|---|---|---|
| **Unit** | Individual function, hook, or component | Fast (ms) | Low–Medium |
| **Integration** | Multiple components + data fetching | Medium | High |
| **E2E** | Full user flows in real browser | Slow (s) | Very High |

---

## Jest Fundamentals

```bash
npm install --save-dev jest @types/jest jest-environment-jsdom
# or (create-react-app and Next.js configure Jest out of the box)
```

**Writing tests:**
```ts
// math.test.ts
import { add, multiply } from './math';

describe('Math utils', () => {
  it('adds two numbers', () => {
    expect(add(2, 3)).toBe(5);
  });

  it('multiplies two numbers', () => {
    expect(multiply(4, 5)).toBe(20);
  });
});
```

**Common matchers:**
```ts
expect(value).toBe(5)                  // strict equality (Object.is)
expect(value).toEqual({ a: 1 })       // deep equality
expect(value).toBeTruthy()
expect(value).toBeFalsy()
expect(value).toBeNull()
expect(value).toBeUndefined()
expect(value).toBeDefined()
expect(value).toBeGreaterThan(3)
expect(value).toBeCloseTo(0.3, 5)     // floating point comparison
expect(arr).toContain('item')
expect(arr).toHaveLength(3)
expect(obj).toHaveProperty('name', 'Alice')
expect(fn).toThrow('error message')
expect(fn).toThrow(TypeError)
expect(fn).not.toThrow()

// Strings
expect(str).toMatch(/regex/)
expect(str).toContain('substring')
```

**Setup and teardown:**
```ts
describe('User service', () => {
  let db;

  beforeAll(async () => {
    db = await connectToTestDb();
  });

  afterAll(async () => {
    await db.disconnect();
  });

  beforeEach(async () => {
    await db.clear();
    await db.seed(testUsers);
  });

  afterEach(() => {
    jest.clearAllMocks();
  });
});
```

**Running tests:**
```bash
npx jest                        # run all tests
npx jest --watch                # watch mode
npx jest --coverage             # with coverage report
npx jest user.test.ts           # specific file
npx jest --testNamePattern="adds"  # filter by test name
npx jest --updateSnapshot       # update snapshots
```

---

## React Testing Library (RTL)

RTL tests components the way users interact with them — by querying the DOM, not component internals.

```bash
npm install --save-dev @testing-library/react @testing-library/jest-dom @testing-library/user-event
```

**Setup:**
```ts
// jest.setup.ts
import '@testing-library/jest-dom';   // adds custom matchers (toBeInTheDocument, etc.)

// jest.config.ts
export default {
  testEnvironment: 'jsdom',
  setupFilesAfterFramework: ['./jest.setup.ts'],
};
```

**Querying elements:**
```tsx
import { render, screen } from '@testing-library/react';
import { UserCard } from './UserCard';

test('renders user name', () => {
  render(<UserCard name="Alice" role="Developer" />);

  // Queries (prefer in this order):
  screen.getByRole('heading', { name: /alice/i })      // ✅ most accessible
  screen.getByLabelText('Email address')               // ✅ for form fields
  screen.getByPlaceholderText('Search...')
  screen.getByText('Submit')                           // ✅ for buttons/text
  screen.getByDisplayValue('Alice')                    // for inputs with values
  screen.getByAltText('Profile photo')
  screen.getByTitle('Close')
  screen.getByTestId('user-card')                      // last resort — use data-testid

  // Variants
  getByX    // throws if not found
  queryByX  // returns null if not found (for asserting absence)
  findByX   // async — returns Promise, waits for element to appear
  getAllByX  // returns array

  // Assertions
  expect(screen.getByRole('heading')).toBeInTheDocument();
  expect(screen.getByRole('button')).toBeEnabled();
  expect(screen.getByRole('button')).toBeDisabled();
  expect(screen.getByRole('checkbox')).toBeChecked();
  expect(screen.getByText('Error')).toBeVisible();
  expect(screen.queryByText('Loading')).not.toBeInTheDocument();
  expect(screen.getByRole('textbox')).toHaveValue('Alice');
  expect(screen.getByRole('link')).toHaveAttribute('href', '/profile');
  expect(container.firstChild).toHaveClass('active');
  expect(screen.getByRole('heading')).toHaveTextContent('Welcome, Alice!');
});
```

**Common ARIA roles reference:**
```
button, link, heading, textbox, checkbox, radio,
combobox (select), listbox, option, menu, menuitem,
dialog, alert, alertdialog, progressbar, img, list, listitem
```

---

## Testing User Interactions

```tsx
import userEvent from '@testing-library/user-event';

test('submits login form', async () => {
  const mockLogin = jest.fn().mockResolvedValue({ token: 'abc' });
  render(<LoginForm onLogin={mockLogin} />);

  // userEvent simulates real user behavior
  const user = userEvent.setup();

  await user.type(screen.getByLabelText('Email'), 'alice@example.com');
  await user.type(screen.getByLabelText('Password'), 'secret123');
  await user.click(screen.getByRole('button', { name: /sign in/i }));

  expect(mockLogin).toHaveBeenCalledWith({
    email: 'alice@example.com',
    password: 'secret123',
  });
});

test('toggles password visibility', async () => {
  render(<PasswordInput />);
  const user = userEvent.setup();

  const input = screen.getByLabelText('Password');
  expect(input).toHaveAttribute('type', 'password');

  await user.click(screen.getByRole('button', { name: /show password/i }));
  expect(input).toHaveAttribute('type', 'text');
});

test('handles keyboard navigation', async () => {
  render(<Dropdown options={['React', 'Vue', 'Angular']} />);
  const user = userEvent.setup();

  await user.click(screen.getByRole('button', { name: /select framework/i }));
  await user.keyboard('{ArrowDown}{ArrowDown}{Enter}');

  expect(screen.getByRole('button')).toHaveTextContent('Vue');
});
```

---

## Async Testing

```tsx
import { waitFor, waitForElementToBeRemoved } from '@testing-library/react';

test('loads and displays users', async () => {
  render(<UserList />);

  // Assert loading state
  expect(screen.getByText('Loading...')).toBeInTheDocument();

  // Wait for async content
  await screen.findByRole('list');                          // findBy* = async getBy*

  // Or with waitFor for complex conditions
  await waitFor(() => {
    expect(screen.getAllByRole('listitem')).toHaveLength(3);
  });

  // Wait for element to disappear
  await waitForElementToBeRemoved(() => screen.queryByText('Loading...'));

  // Assert final state
  expect(screen.getByText('Alice')).toBeInTheDocument();
  expect(screen.getByText('Bob')).toBeInTheDocument();
});

test('handles API error', async () => {
  // Mock fetch to fail
  global.fetch = jest.fn().mockRejectedValue(new Error('Network error'));

  render(<UserList />);

  await screen.findByRole('alert');
  expect(screen.getByText(/failed to load/i)).toBeInTheDocument();
});
```

---

## Mocking

### Mocking Modules

```ts
// Mock an entire module
jest.mock('../api/users');
import { fetchUsers } from '../api/users';

// Now fetchUsers is a jest.fn()
(fetchUsers as jest.Mock).mockResolvedValue([
  { id: 1, name: 'Alice' },
  { id: 2, name: 'Bob' },
]);

// Mock module with factory
jest.mock('next/router', () => ({
  useRouter: () => ({
    push: jest.fn(),
    pathname: '/dashboard',
    query: {},
  }),
}));
```

### Mocking fetch / HTTP Requests (MSW)

**MSW (Mock Service Worker)** is the preferred way to mock API requests in tests.

```bash
npm install --save-dev msw
```

```ts
// src/mocks/handlers.ts
import { http, HttpResponse } from 'msw';

export const handlers = [
  http.get('/api/users', () => {
    return HttpResponse.json([
      { id: 1, name: 'Alice' },
      { id: 2, name: 'Bob' },
    ]);
  }),

  http.post('/api/users', async ({ request }) => {
    const body = await request.json();
    return HttpResponse.json({ id: 3, ...body }, { status: 201 });
  }),

  http.get('/api/users/:id', ({ params }) => {
    if (params.id === '999') {
      return HttpResponse.json({ message: 'Not found' }, { status: 404 });
    }
    return HttpResponse.json({ id: params.id, name: 'Alice' });
  }),
];

// src/mocks/server.ts
import { setupServer } from 'msw/node';
import { handlers } from './handlers';
export const server = setupServer(...handlers);

// jest.setup.ts
import { server } from './src/mocks/server';
beforeAll(() => server.listen());
afterEach(() => server.resetHandlers());
afterAll(() => server.close());

// Override in a specific test
test('shows error on 500', async () => {
  server.use(
    http.get('/api/users', () => HttpResponse.json({ error: 'Server Error' }, { status: 500 }))
  );
  render(<UserList />);
  await screen.findByText(/something went wrong/i);
});
```

### Mocking Timers

```ts
jest.useFakeTimers();
jest.useRealTimers();

test('debounced search fires after delay', async () => {
  jest.useFakeTimers();
  const onSearch = jest.fn();
  render(<SearchInput onSearch={onSearch} />);
  const user = userEvent.setup({ advanceTimers: jest.advanceTimersByTime });

  await user.type(screen.getByRole('textbox'), 'react');
  
  expect(onSearch).not.toHaveBeenCalled();
  jest.advanceTimersByTime(300);
  expect(onSearch).toHaveBeenCalledWith('react');
  
  jest.useRealTimers();
});
```

---

## Testing Hooks

```tsx
import { renderHook, act } from '@testing-library/react';
import { useCounter } from './useCounter';

test('increments count', () => {
  const { result } = renderHook(() => useCounter());

  expect(result.current.count).toBe(0);

  act(() => {
    result.current.increment();
  });

  expect(result.current.count).toBe(1);
});

// Hook with context providers
test('hook that uses context', () => {
  const wrapper = ({ children }) => (
    <ThemeProvider theme="dark">{children}</ThemeProvider>
  );
  const { result } = renderHook(() => useTheme(), { wrapper });
  expect(result.current.theme).toBe('dark');
});

// Async hook
test('fetches data', async () => {
  const { result } = renderHook(() => useFetchUser('1'));

  expect(result.current.loading).toBe(true);

  await waitFor(() => {
    expect(result.current.loading).toBe(false);
  });

  expect(result.current.user?.name).toBe('Alice');
});
```

---

## Snapshot Testing

```tsx
import { render } from '@testing-library/react';

test('matches snapshot', () => {
  const { container } = render(<Button variant="primary">Submit</Button>);
  expect(container).toMatchSnapshot();
  // or inline:
  expect(container).toMatchInlineSnapshot(`
    <div>
      <button class="btn btn-primary">Submit</button>
    </div>
  `);
});
```

**When to use snapshots:**
- ✅ Design system components (verify visual contract doesn't change)
- ❌ Complex components with logic (hard to review diffs)
- ❌ Third-party component trees (too noisy)

---

## Code Coverage

```bash
npx jest --coverage
```

**`jest.config.ts` — configure coverage thresholds:**
```ts
export default {
  collectCoverageFrom: ['src/**/*.{ts,tsx}', '!src/**/*.stories.*'],
  coverageThreshold: {
    global: {
      branches: 70,
      functions: 75,
      lines: 80,
      statements: 80,
    },
  },
};
```

**Coverage types:**
- **Statement** — every line executed?
- **Branch** — every `if/else` path taken?
- **Function** — every function called?
- **Line** — every line covered?

> 100% coverage doesn't mean no bugs. Focus on meaningful tests over coverage numbers.

---

## Vitest (Vite alternative to Jest)

Vitest is Jest-compatible but faster — uses Vite's transformation pipeline.

```bash
npm install --save-dev vitest @vitest/ui jsdom @testing-library/react
```

```ts
// vite.config.ts
/// <reference types="vitest" />
export default defineConfig({
  test: {
    globals: true,
    environment: 'jsdom',
    setupFiles: './src/test/setup.ts',
    coverage: {
      reporter: ['text', 'json', 'html'],
    },
  },
});
```

```bash
npx vitest              # watch mode by default
npx vitest run          # single run
npx vitest --ui         # browser-based UI
npx vitest --coverage
```

The test syntax is identical to Jest — just replace `jest.*` with `vi.*` for mocking:

```ts
import { vi } from 'vitest';

vi.mock('../api/users');
vi.fn()
vi.spyOn(object, 'method')
vi.useFakeTimers()
```

---

## Testing Best Practices

1. **Test behavior, not implementation**
   ```tsx
   // ❌ Testing implementation details
   expect(component.state('isOpen')).toBe(true);
   
   // ✅ Testing what the user sees
   expect(screen.getByRole('menu')).toBeVisible();
   ```

2. **Prefer `findBy*` for async content** over `waitFor` + `getBy*`

3. **Use `userEvent` over `fireEvent`** — simulates real user behavior

4. **Always clean up** — `afterEach(() => jest.clearAllMocks())`

5. **Name tests clearly** — "given X, when Y, then Z"
   ```ts
   it('shows an error message when the email is invalid', ...)
   ```

6. **Don't test third-party libraries** — assume they work

7. **Mock at the boundary** — mock HTTP calls (MSW), not module internals

8. **Keep tests deterministic** — no random data, no real network calls, no real timers

9. **One assertion concept per test** — a test can have multiple `expect()` calls if they test the same behavior

10. **Avoid `data-testid`** — use accessible queries instead; `data-testid` is a last resort


---

## Overview and Comparison

Browser automation tools control a real browser (Chromium, Firefox, Safari) programmatically.

| Feature | Playwright | Puppeteer |
|---|---|---|
| Browsers | Chromium, Firefox, WebKit (Safari) | Chromium only (+ Firefox experimental) |
| Language | JS/TS, Python, Java, C# | JS/TS |
| Auto-wait | ✅ Built-in | ❌ Manual waits |
| Test runner | ✅ Built-in (`@playwright/test`) | ❌ Use Jest/Mocha |
| Assertions | ✅ Web-first assertions | Limited |
| Parallel tests | ✅ Per-worker isolation | Manual |
| Debugging | ✅ Trace viewer, Inspector | DevTools |
| Mobile emulation | ✅ | ✅ |
| Maintained by | Microsoft | Google |
| Best for | E2E testing | Scraping, PDF generation, automation |

**Recommendation:** Use **Playwright** for E2E testing. Use **Puppeteer** for scraping or single-purpose automation tasks.

---

## Playwright Setup

```bash
npm init playwright@latest

# Or manual setup
npm install --save-dev @playwright/test
npx playwright install            # downloads browsers
npx playwright install chromium   # specific browser only
```

**Basic test file:**
```ts
// tests/example.spec.ts
import { test, expect } from '@playwright/test';

test('homepage has correct title', async ({ page }) => {
  await page.goto('https://example.com');
  await expect(page).toHaveTitle(/Example Domain/);
});

test('user can log in', async ({ page }) => {
  await page.goto('/login');
  await page.getByLabel('Email').fill('alice@example.com');
  await page.getByLabel('Password').fill('secret');
  await page.getByRole('button', { name: 'Sign in' }).click();
  await expect(page).toHaveURL('/dashboard');
  await expect(page.getByText('Welcome, Alice')).toBeVisible();
});
```

```bash
npx playwright test                      # run all tests
npx playwright test --headed             # show browser window
npx playwright test login.spec.ts        # specific file
npx playwright test --grep "login"       # filter by name
npx playwright test --debug              # open Inspector
npx playwright codegen https://example.com  # record interactions → generate code
npx playwright show-report               # open HTML report
```

---

## Playwright Core API

### Navigation

```ts
await page.goto('https://example.com');
await page.goto('/dashboard', { waitUntil: 'networkidle' });
await page.reload();
await page.goBack();
await page.goForward();

// Wait for specific URL
await page.waitForURL('**/dashboard');
await page.waitForURL(/success/);
```

### Interactions

```ts
// Click
await page.getByRole('button', { name: 'Submit' }).click();
await page.getByText('More').click({ button: 'right' });      // right click
await page.getByRole('cell').nth(2).dblclick();               // double click

// Typing
await page.getByLabel('Username').fill('alice');              // clear + type
await page.getByLabel('Username').clear();
await page.getByLabel('Search').pressSequentially('hello', { delay: 50 }); // key by key

// Keyboard
await page.keyboard.press('Enter');
await page.keyboard.press('Control+a');
await page.keyboard.type('Hello world');

// Mouse
await page.mouse.move(100, 200);
await page.mouse.click(100, 200);
await page.mouse.wheel(0, 200);           // scroll

// Drag and drop
await page.getByText('Item 1').dragTo(page.getByText('Drop here'));

// Select
await page.getByRole('combobox').selectOption('value');
await page.getByRole('combobox').selectOption({ label: 'Option Label' });

// Checkbox / Radio
await page.getByRole('checkbox', { name: 'Remember me' }).check();
await page.getByRole('checkbox').uncheck();

// File upload
await page.getByLabel('Upload').setInputFiles('./photo.png');
await page.getByLabel('Upload').setInputFiles(['file1.png', 'file2.png']);
```

### Waiting

Playwright auto-waits for elements to be actionable — you rarely need explicit waits.

```ts
// Auto-wait is built-in — these are for special cases
await page.waitForSelector('.loaded');
await page.waitForLoadState('networkidle');
await page.waitForTimeout(1000);      // avoid — use explicit conditions instead

// Wait for a response
const [response] = await Promise.all([
  page.waitForResponse('**/api/users'),
  page.getByRole('button', { name: 'Load' }).click(),
]);
const data = await response.json();

// Wait for a function to return true
await page.waitForFunction(() => document.title.includes('Loaded'));
```

### Dialogs

```ts
// Handle alert/confirm/prompt before triggering action
page.on('dialog', async dialog => {
  console.log(dialog.type(), dialog.message());
  await dialog.accept();         // or dialog.dismiss() or dialog.accept('input text')
});
await page.getByText('Delete').click();
```

---

## Locators (Playwright)

Locators are Playwright's recommended way to find elements. They are **auto-waiting** and **retry-capable**.

```ts
// Preferred (accessible) locators
page.getByRole('button', { name: 'Submit' })
page.getByRole('heading', { name: 'Dashboard', level: 1 })
page.getByLabel('Email address')
page.getByPlaceholder('Search...')
page.getByText('Welcome back')
page.getByAltText('Company logo')
page.getByTitle('Close panel')
page.getByTestId('submit-button')   // data-testid="submit-button"

// Chaining and filtering
page.getByRole('listitem').filter({ hasText: 'Alice' })
page.getByRole('listitem').filter({ has: page.getByRole('button') })

// Nth element
page.getByRole('row').nth(0)       // first row
page.getByRole('row').last()       // last row

// Inside a container
const card = page.getByTestId('user-card');
card.getByRole('button', { name: 'Follow' }).click();

// CSS selectors (last resort)
page.locator('.card > h2')
page.locator('[data-state="open"]')

// Combining locators
page.locator('.list').locator('li').filter({ hasText: 'Active' })
```

---

## Assertions (Playwright)

Web-first assertions — they **automatically retry** until the condition is met or timeout is reached.

```ts
// Page assertions
await expect(page).toHaveTitle('Dashboard | MyApp');
await expect(page).toHaveURL(/dashboard/);

// Element assertions
await expect(locator).toBeVisible();
await expect(locator).toBeHidden();
await expect(locator).toBeEnabled();
await expect(locator).toBeDisabled();
await expect(locator).toBeChecked();
await expect(locator).toBeFocused();
await expect(locator).toBeEmpty();

await expect(locator).toHaveText('Welcome, Alice!');
await expect(locator).toContainText('Alice');
await expect(locator).toHaveValue('alice@example.com');
await expect(locator).toHaveAttribute('href', '/profile');
await expect(locator).toHaveClass(/active/);
await expect(locator).toHaveCount(5);

// Negation
await expect(locator).not.toBeVisible();
await expect(locator).not.toHaveText('Error');

// Soft assertions — don't stop the test on failure
await expect.soft(locator).toHaveText('Alice');
await expect.soft(locator).toBeVisible();
// Both failures reported at the end
```

---

## Page Object Model (POM)

Encapsulate page interactions in a class — keeps tests clean and maintainable.

```ts
// tests/pages/LoginPage.ts
import { Page, Locator, expect } from '@playwright/test';

export class LoginPage {
  readonly page: Page;
  readonly emailInput: Locator;
  readonly passwordInput: Locator;
  readonly submitButton: Locator;
  readonly errorMessage: Locator;

  constructor(page: Page) {
    this.page = page;
    this.emailInput = page.getByLabel('Email');
    this.passwordInput = page.getByLabel('Password');
    this.submitButton = page.getByRole('button', { name: 'Sign in' });
    this.errorMessage = page.getByRole('alert');
  }

  async goto() {
    await this.page.goto('/login');
  }

  async login(email: string, password: string) {
    await this.emailInput.fill(email);
    await this.passwordInput.fill(password);
    await this.submitButton.click();
  }

  async expectLoginSuccess() {
    await expect(this.page).toHaveURL('/dashboard');
  }

  async expectError(message: string) {
    await expect(this.errorMessage).toContainText(message);
  }
}

// tests/login.spec.ts
import { test } from '@playwright/test';
import { LoginPage } from './pages/LoginPage';

test.describe('Login', () => {
  let loginPage: LoginPage;

  test.beforeEach(async ({ page }) => {
    loginPage = new LoginPage(page);
    await loginPage.goto();
  });

  test('successful login', async () => {
    await loginPage.login('alice@example.com', 'password123');
    await loginPage.expectLoginSuccess();
  });

  test('shows error on wrong password', async () => {
    await loginPage.login('alice@example.com', 'wrong');
    await loginPage.expectError('Invalid credentials');
  });
});
```

---

## Playwright Configuration

```ts
// playwright.config.ts
import { defineConfig, devices } from '@playwright/test';

export default defineConfig({
  testDir: './tests',
  timeout: 30_000,
  expect: { timeout: 5_000 },
  fullyParallel: true,
  retries: process.env.CI ? 2 : 0,         // retry on CI
  workers: process.env.CI ? 1 : undefined,  // sequential on CI (or set to 4)
  reporter: [['html'], ['list']],

  use: {
    baseURL: 'http://localhost:3000',
    screenshot: 'only-on-failure',
    video: 'retain-on-failure',
    trace: 'on-first-retry',
  },

  projects: [
    { name: 'chromium', use: { ...devices['Desktop Chrome'] } },
    { name: 'firefox',  use: { ...devices['Desktop Firefox'] } },
    { name: 'webkit',   use: { ...devices['Desktop Safari'] } },
    { name: 'mobile',   use: { ...devices['iPhone 14'] } },
  ],

  // Start local dev server before tests
  webServer: {
    command: 'npm run build && npm run preview',
    port: 3000,
    reuseExistingServer: !process.env.CI,
  },
});
```

---

## Handling Authentication

```ts
// Global setup — log in once, save storage state
// tests/global-setup.ts
import { chromium } from '@playwright/test';

export default async function globalSetup() {
  const browser = await chromium.launch();
  const page = await browser.newPage();

  await page.goto('http://localhost:3000/login');
  await page.getByLabel('Email').fill('admin@example.com');
  await page.getByLabel('Password').fill('password');
  await page.getByRole('button', { name: 'Sign in' }).click();
  await page.waitForURL('/dashboard');

  // Save auth state to file
  await page.context().storageState({ path: 'tests/.auth/user.json' });
  await browser.close();
}

// playwright.config.ts
export default defineConfig({
  globalSetup: './tests/global-setup.ts',
  use: {
    storageState: 'tests/.auth/user.json',  // reuse auth for all tests
  },
});

// Tests that need different auth roles
test.use({ storageState: 'tests/.auth/admin.json' });
test('admin sees delete button', async ({ page }) => { ... });
```

---

## Network Interception

```ts
// Mock API responses
await page.route('**/api/users', async route => {
  await route.fulfill({
    status: 200,
    contentType: 'application/json',
    body: JSON.stringify([{ id: 1, name: 'Mocked Alice' }]),
  });
});

// Modify a real response
await page.route('**/api/users', async route => {
  const response = await route.fetch();
  const data = await response.json();
  data.push({ id: 99, name: 'Extra User' });
  await route.fulfill({ json: data });
});

// Block specific resources
await page.route('**/*.{png,jpg,gif,webp}', route => route.abort());
await page.route('**/analytics/**', route => route.abort());

// Intercept and inspect requests
page.on('request', req => console.log(req.method(), req.url()));
page.on('response', res => console.log(res.status(), res.url()));

// Wait for a specific request
const [req] = await Promise.all([
  page.waitForRequest(r => r.url().includes('/api/submit') && r.method() === 'POST'),
  page.getByRole('button', { name: 'Submit' }).click(),
]);
const body = req.postDataJSON();
expect(body.email).toBe('alice@example.com');
```

---

## Screenshots and Visual Testing

```ts
// Take screenshots
await page.screenshot({ path: 'screenshot.png' });
await page.screenshot({ path: 'full.png', fullPage: true });
await locator.screenshot({ path: 'element.png' });

// Visual regression testing
await expect(page).toHaveScreenshot('dashboard.png');
await expect(page).toHaveScreenshot({ maxDiffPixels: 100 });
await expect(locator).toHaveScreenshot('card.png');
```

```bash
# Update snapshots
npx playwright test --update-snapshots
```

---

## Puppeteer Fundamentals

```bash
npm install puppeteer
```

```ts
import puppeteer from 'puppeteer';

const browser = await puppeteer.launch({ headless: true });
const page = await browser.newPage();

// Navigation
await page.goto('https://example.com', { waitUntil: 'networkidle2' });

// Selectors
await page.click('button#submit');
await page.type('input[name="email"]', 'alice@example.com');
const text = await page.$eval('h1', el => el.textContent);
const elements = await page.$$('li.item');

// Wait
await page.waitForSelector('.loaded');
await page.waitForNavigation();
await page.waitForFunction(() => document.readyState === 'complete');

// Extract data (scraping)
const data = await page.evaluate(() => {
  return Array.from(document.querySelectorAll('article')).map(a => ({
    title: a.querySelector('h2')?.textContent,
    link: a.querySelector('a')?.href,
  }));
});

// PDF generation
await page.pdf({ path: 'report.pdf', format: 'A4', printBackground: true });

// Screenshot
await page.screenshot({ path: 'snap.png', fullPage: true });

// Intercept requests
await page.setRequestInterception(true);
page.on('request', req => {
  if (req.resourceType() === 'image') req.abort();
  else req.continue();
});

await browser.close();
```

---

## CI/CD Integration

```yaml
# .github/workflows/e2e.yml
- name: Install Playwright browsers
  run: npx playwright install --with-deps chromium

- name: Run Playwright tests
  run: npx playwright test
  env:
    CI: true

- name: Upload Playwright report
  uses: actions/upload-artifact@v4
  if: always()
  with:
    name: playwright-report
    path: playwright-report/
    retention-days: 30
```

---

## Intelligent Workflows and Scraping

Playwright and Puppeteer are powerful beyond just testing:

```ts
// Automated form filling (RPA — Robotic Process Automation)
async function fillExpenseReport(data: ExpenseData) {
  await page.goto('/expenses/new');
  await page.getByLabel('Amount').fill(String(data.amount));
  await page.getByLabel('Category').selectOption(data.category);
  await page.getByLabel('Description').fill(data.description);
  await page.getByRole('button', { name: 'Submit' }).click();
  await expect(page).toHaveURL(/expenses\/\d+/);
}

// Web scraping with structure
async function scrapeProductList(url: string) {
  await page.goto(url);
  await page.waitForSelector('.product-card');

  return page.evaluate(() =>
    [...document.querySelectorAll('.product-card')].map(card => ({
      name: card.querySelector('.product-name')?.textContent?.trim(),
      price: card.querySelector('.price')?.textContent?.trim(),
      inStock: !card.querySelector('.out-of-stock'),
    }))
  );
}

// Monitoring — check if a page is up and working
async function healthCheck(url: string) {
  try {
    await page.goto(url, { timeout: 10000 });
    await expect(page.getByRole('heading')).toBeVisible();
    return { status: 'ok' };
  } catch (err) {
    return { status: 'down', error: err.message };
  }
}
```
