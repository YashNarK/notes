# AI for Frontend Development

## Table of contents

- [AI for Frontend Development](#ai-for-frontend-development)
  - [Table of contents](#table-of-contents)
  - [AI-Assisted Development Overview](#ai-assisted-development-overview)
  - [GitHub Copilot](#github-copilot)
  - [OpenAI Codex and Code Models](#openai-codex-and-code-models)
  - [AI in the IDE Workflow](#ai-in-the-ide-workflow)
  - [Integrating AI APIs in Frontend Apps](#integrating-ai-apis-in-frontend-apps)
  - [AI-Generated UI Components](#ai-generated-ui-components)
  - [AI for Code Reviews](#ai-for-code-reviews)
  - [AI Testing Assistance](#ai-testing-assistance)
  - [Building AI-Powered Features](#building-ai-powered-features)
  - [Tools and Ecosystem](#tools-and-ecosystem)
  - [Responsible AI Development](#responsible-ai-development)

---

## AI-Assisted Development Overview

AI coding tools accelerate development by:
- **Autocompleting** code as you type (Copilot, Supermaven)
- **Generating** code from natural language prompts
- **Explaining** unfamiliar code
- **Refactoring** existing code to better patterns
- **Writing tests** for existing functions/components
- **Reviewing** code for bugs, performance issues, or accessibility problems

**The developer's role shifts** from writing every line to:
1. Defining requirements clearly
2. Prompting AI effectively
3. Reviewing, validating, and correcting AI output
4. Integrating AI-generated code safely

---

## GitHub Copilot

**GitHub Copilot** is an AI pair programmer integrated into VS Code and other editors (powered by OpenAI Codex / GPT-4o).

### Key Interactions

```
Inline Suggestions   → type code, Copilot completes it (Tab to accept)
Copilot Chat         → ask questions, get explanations, request changes
Inline Chat          → Ctrl+I to prompt Copilot at your cursor
/explain             → explain selected code
/fix                 → suggest a fix for a bug or error
/tests               → generate unit tests for selected code
/doc                 → generate documentation/JSDoc
/refactor            → suggest refactors for selected code
```

### Effective Usage Patterns

```tsx
// 1. Write a comment describing what you want — Copilot fills in the code

// Parse JWT token and extract the user ID and expiration time
// returns null if the token is invalid or expired
function parseJWT(token: string): { userId: string; expiresAt: Date } | null {
  // Copilot fills this in...
}

// 2. Write the function signature + types — Copilot infers the body
interface PaginatedResponse<T> {
  data: T[];
  total: number;
  page: number;
  pageSize: number;
}

async function fetchPaginated<T>(
  url: string,
  page: number,
  pageSize: number,
): Promise<PaginatedResponse<T>> {
  // Copilot fills this in...
}

// 3. Give examples in comments for edge cases
// Examples:
//   formatCurrency(1234.5, 'USD') → '$1,234.50'
//   formatCurrency(0, 'EUR')      → '€0.00'
//   formatCurrency(-99, 'GBP')    → '-£99.00'
function formatCurrency(amount: number, currency: string): string {
  // Copilot fills this in...
}
```

### Copilot Chat Prompts for Frontend

```
"Explain how React's reconciliation algorithm works"

"Refactor this useEffect to use React Query instead"

"This component re-renders too often. What's causing it and how do I fix it?"

"Write unit tests for this hook using React Testing Library and Jest"

"Convert this class component to a functional component with hooks"

"Add proper TypeScript types to this component"

"Make this form accessible — add ARIA labels and keyboard navigation"

"This CSS isn't working as expected: [paste CSS]. What's wrong?"
```

---

## OpenAI Codex and Code Models

**OpenAI Codex** (the model underlying early Copilot) and newer models like **GPT-4o** and **o1** can generate, explain, and debug code via the API.

### Using OpenAI API for Code Tasks

```ts
import OpenAI from 'openai';

const client = new OpenAI({ apiKey: process.env.OPENAI_API_KEY });

async function generateComponent(description: string): Promise<string> {
  const response = await client.chat.completions.create({
    model: 'gpt-4o',
    messages: [
      {
        role: 'system',
        content: `You are an expert React developer. 
          Generate TypeScript React components with:
          - Proper TypeScript types
          - Accessibility attributes (ARIA roles, labels)
          - Clean, maintainable code
          - Tailwind CSS for styling
          Output only the component code, no explanation.`
      },
      {
        role: 'user',
        content: `Create a React component: ${description}`
      }
    ],
    temperature: 0.2,    // lower = more deterministic code
  });

  return response.choices[0].message.content ?? '';
}

// Usage
const code = await generateComponent(
  'A data table with sorting, pagination, and row selection. Props: columns[], data[], onRowSelect'
);
```

### Structured Output for Code Generation

```ts
// Use structured output to get predictable results
const response = await client.chat.completions.create({
  model: 'gpt-4o',
  response_format: { type: 'json_object' },
  messages: [{
    role: 'user',
    content: `Analyze this React component for issues. Return JSON:
      { 
        "performance": ["issue1", "issue2"],
        "accessibility": ["issue1"],
        "security": ["issue1"],
        "suggestions": ["improvement1"]
      }
      
      Component:
      ${componentCode}`
  }],
});

const analysis = JSON.parse(response.choices[0].message.content);
```

---

## AI in the IDE Workflow

### Day-to-day workflow with AI assistance

```
1. Planning
   → Ask AI to break down a feature into components/tasks
   → "What components do I need to build a real-time chat UI?"

2. Scaffolding
   → Generate boilerplate code (component shells, test files, types)
   → Copilot autocompletes repetitive code

3. Implementation
   → Write complex logic with AI assistance
   → Ask for explanations of unfamiliar APIs

4. Review
   → "Review this function for edge cases"
   → "Is this accessible? What ARIA attributes am I missing?"
   → "Does this have any security issues?"

5. Testing
   → "Write tests for this hook"
   → "What edge cases should I test for this component?"

6. Documentation
   → "Write JSDoc for this function"
   → Generate README sections
```

### Copilot in VS Code — Key Shortcuts

```
Tab               → Accept inline suggestion
Esc               → Dismiss suggestion
Alt+]             → Next suggestion
Alt+[             → Previous suggestion
Ctrl+Enter        → Open suggestion panel
Ctrl+I            → Open inline chat at cursor
Ctrl+Shift+I      → Open Copilot Chat panel
```

---

## Integrating AI APIs in Frontend Apps

### Streaming AI Responses (Chat UIs)

```tsx
import OpenAI from 'openai';

const client = new OpenAI({ apiKey: process.env.VITE_OPENAI_KEY, dangerouslyAllowBrowser: true });
// ⚠️ Never expose real API keys in client-side code in production — proxy through your backend

function ChatInterface() {
  const [messages, setMessages] = useState<Message[]>([]);
  const [streaming, setStreaming] = useState('');

  const sendMessage = async (userMessage: string) => {
    const newMessages = [...messages, { role: 'user', content: userMessage }];
    setMessages(newMessages);
    setStreaming('');

    const stream = await client.chat.completions.create({
      model: 'gpt-4o',
      messages: newMessages,
      stream: true,
    });

    let fullResponse = '';
    for await (const chunk of stream) {
      const delta = chunk.choices[0]?.delta?.content ?? '';
      fullResponse += delta;
      setStreaming(fullResponse);   // update UI as chunks arrive
    }

    setMessages(prev => [...prev, { role: 'assistant', content: fullResponse }]);
    setStreaming('');
  };

  return (
    <div className="chat">
      {messages.map((msg, i) => <ChatMessage key={i} {...msg} />)}
      {streaming && <ChatMessage role="assistant" content={streaming} isStreaming />}
      <ChatInput onSend={sendMessage} />
    </div>
  );
}
```

### Secure API Proxy (Backend Route)

```ts
// next.js api route — keeps API key server-side
// app/api/chat/route.ts
import OpenAI from 'openai';
import { NextRequest } from 'next/server';

const client = new OpenAI({ apiKey: process.env.OPENAI_API_KEY });

export async function POST(req: NextRequest) {
  const { messages } = await req.json();

  const stream = await client.chat.completions.create({
    model: 'gpt-4o',
    messages,
    stream: true,
  });

  // Return as Server-Sent Events
  const encoder = new TextEncoder();
  const readable = new ReadableStream({
    async start(controller) {
      for await (const chunk of stream) {
        const text = chunk.choices[0]?.delta?.content ?? '';
        if (text) controller.enqueue(encoder.encode(`data: ${JSON.stringify({ text })}\n\n`));
      }
      controller.enqueue(encoder.encode('data: [DONE]\n\n'));
      controller.close();
    },
  });

  return new Response(readable, {
    headers: { 'Content-Type': 'text/event-stream', 'Cache-Control': 'no-cache' },
  });
}
```

---

## AI-Generated UI Components

### Using AI design tools

**Tools:**
- **v0.dev** (Vercel) — generate React + Tailwind UI from text/image prompts
- **Locofy.ai** — convert Figma designs to React code
- **Builder.io** — AI-powered visual editor with React code export
- **Figma AI** — generate and iterate on designs

### Workflow: AI → Production Component

```
1. Prompt AI (v0/Copilot) to generate a base component
2. Review the generated code:
   - Is the structure semantic/accessible?
   - Are types correct?
   - Are there performance issues?
   - Does it handle error/loading/empty states?
3. Fix issues identified in review
4. Add your own business logic
5. Write tests
6. Integrate into your component library/design system
```

---

## AI for Code Reviews

```ts
// Automated pre-commit review with AI
async function reviewDiff(diff: string): Promise<ReviewResult> {
  const response = await client.chat.completions.create({
    model: 'gpt-4o',
    messages: [{
      role: 'system',
      content: `You are a senior frontend developer reviewing a code diff.
        Check for:
        - React performance issues (unnecessary re-renders, missing memoization)
        - Accessibility violations
        - Security vulnerabilities (XSS, sensitive data exposure)
        - TypeScript type safety issues
        - Missing error handling
        - Breaking changes
        Be concise. Only mention real issues, not style preferences.`
    }, {
      role: 'user',
      content: `Review this diff:\n\n${diff}`
    }],
    temperature: 0.1,
  });
  return parseReview(response.choices[0].message.content);
}
```

---

## AI Testing Assistance

```
Prompts for generating tests:

"Write unit tests for this React hook. Include tests for:
  - Initial state
  - Successful async operation
  - Error handling
  - Edge cases (empty array, null, undefined)
  Use React Testing Library and Jest."

"Write a Playwright E2E test for the checkout flow:
  1. Add item to cart
  2. Go to checkout
  3. Fill shipping form
  4. Complete payment
  Assert the order confirmation page appears."

"What edge cases am I missing in my test suite for this component?"

"This test is flaky — it sometimes fails with a timeout. How do I fix it?"
```

---

## Building AI-Powered Features

Common AI features to integrate in frontend applications:

```tsx
// 1. Smart Search (semantic search)
function SmartSearch() {
  const [query, setQuery] = useState('');
  const { data: results } = useQuery({
    queryKey: ['search', query],
    queryFn: () => fetch(`/api/search?q=${encodeURIComponent(query)}`).then(r => r.json()),
    enabled: query.length > 2,
  });
  // ...
}

// 2. Auto-complete suggestions
function SmartInput({ onSelect }) {
  const [value, setValue] = useState('');
  const [suggestions, setSuggestions] = useState([]);
  const debouncedValue = useDebounce(value, 300);

  useEffect(() => {
    if (!debouncedValue) return;
    fetch(`/api/ai/suggest?q=${debouncedValue}`)
      .then(r => r.json())
      .then(setSuggestions);
  }, [debouncedValue]);
  // ...
}

// 3. Content summarization
async function summarizeText(text: string): Promise<string> {
  const res = await fetch('/api/ai/summarize', {
    method: 'POST',
    body: JSON.stringify({ text }),
    headers: { 'Content-Type': 'application/json' },
  });
  const { summary } = await res.json();
  return summary;
}

// 4. Image analysis
async function analyzeImage(file: File): Promise<string> {
  const base64 = await toBase64(file);
  const res = await fetch('/api/ai/analyze-image', {
    method: 'POST',
    body: JSON.stringify({ image: base64 }),
    headers: { 'Content-Type': 'application/json' },
  });
  return (await res.json()).description;
}
```

---

## Tools and Ecosystem

| Tool | Category | Use |
|---|---|---|
| **GitHub Copilot** | IDE assistant | Inline code completion, chat |
| **Cursor** | AI-first IDE | Full codebase context, multi-file edits |
| **v0.dev** | UI generation | Generate React + Tailwind from prompts |
| **Bolt.new** | Full-stack gen | Generate full apps from prompts |
| **Codeium** | IDE assistant | Free Copilot alternative |
| **Supermaven** | IDE autocomplete | Very fast token-level completion |
| **ChatGPT** | General AI | Debugging, explanations, architecture |
| **Claude** | General AI | Long context, nuanced code reviews |
| **Tabnine** | IDE assistant | Privacy-focused, on-premise option |
| **Vercel AI SDK** | SDK | Unified API for AI streaming in Next.js |
| **LangChain.js** | Framework | Build complex AI pipelines |

---

## Responsible AI Development

1. **Never expose API keys client-side** — proxy through a backend route
2. **Validate AI output** — don't blindly trust generated code, especially for:
   - Authentication/authorization logic
   - Data validation
   - Security-sensitive operations
3. **Review generated code** for accessibility, performance, and correctness
4. **Don't commit sensitive data in prompts** — prompts may be logged
5. **Rate limiting** — add rate limits on AI-powered endpoints to prevent abuse
6. **User awareness** — disclose when users interact with AI-generated content
7. **Hallucinations** — AI can generate plausible-looking but incorrect code. Always test.
8. **Intellectual property** — be aware of licensing concerns with AI-generated code


---

## Why Audit AI-Generated Code?

AI models (Copilot, GPT-4, Claude) can generate impressive-looking code that has subtle — or not so subtle — problems:

| Issue Type | Examples |
|---|---|
| **Hallucinated APIs** | Using non-existent React hooks, browser APIs with wrong signatures |
| **Outdated patterns** | Class components, legacy lifecycle methods, deprecated packages |
| **Missing error handling** | No try/catch, no loading/error states |
| **Security vulnerabilities** | XSS via `innerHTML`, exposed secrets, missing validation |
| **Performance issues** | Unnecessary re-renders, missing memoization, blocking operations |
| **Accessibility gaps** | Missing ARIA, non-keyboard-navigable widgets |
| **Subtle logic bugs** | Off-by-one errors, wrong comparison operators, async race conditions |
| **Incomplete implementation** | Placeholders like `// TODO: implement`, missing edge cases |

> **Rule:** AI is a fast first draft. You are the quality gate.

---

## The Auditing Mindset

```
Approach AI output as you would a junior developer's PR:
- Assume it's mostly right, but verify the important parts
- Don't rubber-stamp without reading
- Focus review effort on security and correctness first
- Test before you trust
```

**Questions to ask for every AI-generated piece of code:**

1. **Does it actually work?** → Run it. Write a quick test.
2. **Does it handle failures?** → What happens if the API returns 500? If the input is null?
3. **Is it using current, real APIs?** → Check the docs.
4. **Are there security implications?** → XSS, injection, data exposure?
5. **Is it accessible?** → Can keyboard users and screen readers use it?
6. **Would it hold up in production?** → Scale, performance, maintainability?

---

## Accuracy and Correctness Checks

### Verify API Signatures

```tsx
// AI might generate this — but check the actual API:
// ❌ useQuery signature is wrong in older AI training data
const { data } = useQuery('users', fetchUsers);    // old React Query v3 API

// ✅ React Query v5 (current)
const { data } = useQuery({ queryKey: ['users'], queryFn: fetchUsers });

// ❌ AI hallucinated prop
<Image quality="high" />    // 'high' isn't a valid value — should be a number

// ✅ Correct
<Image quality={85} />
```

### Verify Logic Correctness

```tsx
// AI generated this pagination logic — is it correct?
const totalPages = Math.ceil(total / pageSize);
const offset = (page - 1) * pageSize;  // ✅ correct

// But check edge cases:
// page = 0? → offset = -pageSize ❌
// total = 0? → totalPages = 0, but is page 1 shown? Need to check UI.
// pageSize = 0? → division by zero ❌

// ✅ Hardened version
const safePage = Math.max(1, page);
const safePageSize = Math.max(1, pageSize);
const totalPages = total === 0 ? 1 : Math.ceil(total / safePageSize);
const offset = (safePage - 1) * safePageSize;
```

### Async/Race Conditions

```tsx
// ❌ AI-generated code with race condition
function SearchResults({ query }) {
  const [results, setResults] = useState([]);

  useEffect(() => {
    fetch(`/api/search?q=${query}`)
      .then(r => r.json())
      .then(data => setResults(data));  // race condition: stale query may overwrite fresh
  }, [query]);
}

// ✅ Fixed with cleanup and abort
useEffect(() => {
  const controller = new AbortController();
  
  fetch(`/api/search?q=${query}`, { signal: controller.signal })
    .then(r => r.json())
    .then(data => setResults(data))
    .catch(err => {
      if (err.name !== 'AbortError') setError(err.message);
    });

  return () => controller.abort();   // cancel on query change or unmount
}, [query]);
```

### Stale Closures

```tsx
// ❌ AI-generated interval with stale closure
useEffect(() => {
  const interval = setInterval(() => {
    setCount(count + 1);   // `count` is always the initial value (closure)
  }, 1000);
  return () => clearInterval(interval);
}, []);   // empty deps = count is stale

// ✅ Use functional updater
useEffect(() => {
  const interval = setInterval(() => {
    setCount(prev => prev + 1);   // always uses latest value
  }, 1000);
  return () => clearInterval(interval);
}, []);
```

---

## Performance Auditing

### Unnecessary Re-renders

```tsx
// ❌ AI-generated — object literal creates new reference every render
function Parent() {
  return <Child style={{ color: 'red' }} />;
}

// ❌ Inline function recreated every render
function Parent() {
  return <MemoizedChild onClick={() => handleClick(id)} />;
}

// ✅ Fixed
const STYLE = { color: 'red' };
const handleClickItem = useCallback(() => handleClick(id), [id]);
```

### Missing Keys

```tsx
// ❌ AI often generates missing or wrong keys
items.map((item, index) => <Item key={index} item={item} />)
// Using index as key causes bugs on add/remove/reorder

// ✅ Use stable unique IDs
items.map(item => <Item key={item.id} item={item} />)
```

### Blocking the Main Thread

```tsx
// ❌ AI may generate synchronous heavy computation in render
function ProductList({ products }) {
  const sorted = products
    .filter(p => p.inStock)
    .sort((a, b) => b.rating - a.rating)
    .slice(0, 100);   // may be expensive if products is large
  // ...
}

// ✅ Memoize it
const sorted = useMemo(() =>
  products
    .filter(p => p.inStock)
    .sort((a, b) => b.rating - a.rating)
    .slice(0, 100),
  [products]
);
```

### Bundle Size

```tsx
// ❌ AI imports full library
import _ from 'lodash';
const result = _.groupBy(items, 'category');

// ✅ Tree-shakeable import
import groupBy from 'lodash/groupBy';
const result = groupBy(items, 'category');
```

---

## Security Auditing

### XSS (Cross-Site Scripting)

```tsx
// ❌ DANGEROUS — never set innerHTML with user data
function Comment({ content }) {
  return <div dangerouslySetInnerHTML={{ __html: content }} />;
}

// ✅ If you must render HTML — sanitize first
import DOMPurify from 'dompurify';
function Comment({ content }) {
  const clean = DOMPurify.sanitize(content);
  return <div dangerouslySetInnerHTML={{ __html: clean }} />;
}

// ✅ Better — use a Markdown renderer with sanitization
import ReactMarkdown from 'react-markdown';
function Comment({ content }) {
  return <ReactMarkdown>{content}</ReactMarkdown>;
}
```

### Exposed Secrets

```tsx
// ❌ AI might put secrets in frontend code
const API_KEY = 'sk-proj-abc123...';   // NEVER in client code
const response = await fetch(url, {
  headers: { 'Authorization': `Bearer ${process.env.REACT_APP_SECRET}` }
});

// Check: VITE_/REACT_APP_ env vars are bundled into client JS — never put secrets there
// ✅ Secrets must live on the server
```

### URL/Parameter Injection

```tsx
// ❌ AI might construct URLs without encoding
const url = `/api/search?q=${query}`;    // vulnerable if query has &, =, etc.

// ✅ Encode parameters
const params = new URLSearchParams({ q: query, page: String(page) });
const url = `/api/search?${params}`;
```

### Dependency Vulnerabilities

```bash
# Check AI-suggested packages for vulnerabilities
npm audit
npx better-npm-audit audit
# Check a package before adding it
npx npm-check-updates -u   # view available updates
```

---

## Accessibility Auditing

### Interactive Elements

```tsx
// ❌ AI often generates non-accessible custom components
<div onClick={handleClick} className="button">
  Click me
</div>
// Missing: role, tabIndex, keyboard handler

// ✅
<button onClick={handleClick}>
  Click me
</button>
// or if div is unavoidable:
<div
  role="button"
  tabIndex={0}
  onClick={handleClick}
  onKeyDown={e => (e.key === 'Enter' || e.key === ' ') && handleClick()}
>
  Click me
</div>
```

### Missing Labels

```tsx
// ❌ AI forgets form labels
<input type="search" placeholder="Search..." onChange={...} />

// ✅
<label htmlFor="search">Search</label>
<input id="search" type="search" placeholder="Search..." onChange={...} />

// Or aria-label for icon buttons
<button aria-label="Search">
  <SearchIcon aria-hidden="true" />
</button>
```

### Missing ARIA for Dynamic Content

```tsx
// ❌ Error messages that appear dynamically aren't announced
<div className={hasError ? 'error' : ''}>
  {error}
</div>

// ✅ Use role="alert" so screen readers announce it
<div role="alert" aria-live="assertive">
  {error && <span>{error}</span>}
</div>
```

---

## TypeScript and Type Safety

### Eliminating `any`

```tsx
// ❌ Common in AI output
const handleData = (data: any) => {
  return data.users.map((u: any) => u.name);
};

// ✅ Properly typed
interface UserApiResponse {
  users: Array<{ id: string; name: string; email: string }>;
  total: number;
}

const handleData = (data: UserApiResponse) => {
  return data.users.map(u => u.name);  // TypeScript infers string[]
};
```

### Type Assertions

```tsx
// ❌ AI often uses type assertions to silence errors
const user = response.data as User;   // bypasses type checking

// ✅ Validate at runtime (use Zod)
import { z } from 'zod';

const UserSchema = z.object({
  id: z.string(),
  name: z.string(),
  email: z.string().email(),
});

const user = UserSchema.parse(response.data);  // throws if invalid
```

### Non-null Assertions

```tsx
// ❌ AI over-uses non-null assertion (!)
const user = store.getUser()!;   // could still be null at runtime

// ✅ Handle the null case
const user = store.getUser();
if (!user) return <NotLoggedIn />;
```

---

## Production Readiness Checklist

Run through this before merging AI-generated code:

### Functionality
- [ ] Tested manually in the browser
- [ ] All props and edge cases handled (null, undefined, empty, large data)
- [ ] Loading state shown during async operations
- [ ] Error state shown and user can recover
- [ ] Empty state shown when there's no data

### Code Quality
- [ ] No `any` types (unless genuinely necessary with a comment)
- [ ] No `console.log` left in the code
- [ ] No commented-out code or TODOs without tracking
- [ ] No hardcoded values that should be configurable
- [ ] APIs used are current (verified in docs)

### Performance
- [ ] No unnecessary re-renders (verified with React DevTools)
- [ ] Expensive computations memoized
- [ ] Large lists virtualized
- [ ] No memory leaks (event listeners cleaned up, subscriptions cancelled)

### Security
- [ ] No `dangerouslySetInnerHTML` with unsanitized user input
- [ ] No secrets in client code or env vars
- [ ] User inputs validated/encoded before use in URLs or requests
- [ ] No prototype pollution risks

### Accessibility
- [ ] Keyboard navigation works
- [ ] Screen reader tested (or at minimum ARIA reviewed)
- [ ] All images have alt text
- [ ] Interactive elements have visible focus styles
- [ ] Dynamic content announced with aria-live

### Testing
- [ ] Unit/integration tests cover main behaviors
- [ ] Tests cover error cases

---

## Debugging AI-Generated Code

### Systematic Debugging Process

```
1. Read the error message fully
2. Check the stack trace — find the actual failing line
3. Verify the AI didn't use a wrong/outdated API
4. Add console.log or debugger to inspect values at the error point
5. Check if the code handles the actual data shape vs expected
6. Ask AI to explain what the code does (you may find mismatches)
7. Ask AI to fix a specific error, giving it the exact error message and context
```

### Prompting AI to Debug Its Own Code

```
"This code generates an error:
[error message and stack trace]

The component is:
[paste code]

The data it receives is:
[paste sample data or data shape]

Identify the root cause and fix it."
```

```
"This component has a race condition where the search results show 
stale data when the user types quickly. Here's the current code:
[paste code]
Fix the race condition using AbortController."
```

---

## Tools for Automated Auditing

```bash
# Linting (catches bugs and style issues)
npm run lint                          # ESLint

# Type checking
npx tsc --noEmit                      # TypeScript compiler check

# Accessibility
npx @axe-core/cli http://localhost:3000
npx playwright test --grep "a11y"     # custom a11y Playwright tests

# Security
npm audit                             # dependency vulnerabilities
npx snyk test                         # more detailed security scan

# Bundle analysis
npx source-map-explorer dist/main.*.js

# Performance
npx lighthouse http://localhost:3000 --output=json
```

**ESLint plugins for catching AI code issues:**
```json
{
  "plugins": [
    "react",
    "react-hooks",       // catches stale closures, missing deps
    "jsx-a11y",          // catches accessibility issues
    "@typescript-eslint" // catches type issues
  ],
  "rules": {
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "warn",
    "jsx-a11y/alt-text": "error",
    "jsx-a11y/interactive-supports-focus": "error",
    "@typescript-eslint/no-explicit-any": "warn"
  }
}
```

---

## Building a Review Workflow

```
1. AI generates code
         ↓
2. Run automated checks (lint, type-check, tests)
         ↓
3. Manual review checklist (accuracy, security, a11y, perf)
         ↓
4. Run in browser — test all states
         ↓
5. Ask AI to review its own output ("Review this for issues")
         ↓
6. Iterate and fix
         ↓
7. Merge
```

**Time allocation guideline:**
- Simple utility function → 5 min review
- UI component → 15–30 min review + manual test
- Auth/security-related code → 1+ hour review + security audit
- Complex async logic → review + write tests before trusting
