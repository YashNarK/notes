# Generative AI Course Notebook

This is a notebook for my Generative AI (GenAI) course, curated from Pluralsight courses. The course is designed for enthusiasts and beginners, providing a comprehensive understanding of generative AI models, prompt engineering, model architecture, prototyping, evaluation, and ethical considerations.

## Description

This self-paced learning path guides you through the world of generative AI models, demonstrating how to use prompts as input to generate desired output. Each course builds on the previous, providing a structured approach to learning.

By the end of this course, you will have a solid understanding of generative AI and its applications.

## Courses

1. **[AI & Generative AI Executive Briefing](AI_and_Generative_AI_Executive_Briefing.md)**
   - Gain insights into AI and Generative AI fundamentals.

2. **[Exploring Generative AI Models and Architecture](Exploring_Generative_AI_Models_and_Architecture.md)**
   - Dive deep into different types of generative AI models and their architectures.

3. **[ChatGPT and Generative AI: The Big Picture](ChatGPT_and_Generative_AI_The_Big_Picture.md)**
   - Understand the broader context of ChatGPT and Generative AI.

4. **[Getting Started on Prompt Engineering with Generative AI](Getting_Started_on_Prompt_Engineering_with_Generative_AI.md)**
   - Learn how to effectively use prompts for generative AI.

5. **[Navigating Generative AI Hurdles: Prototyping, Evaluation, and Ethics](Navigating_Generative_AI_Hurdles_Prototyping_Evaluation_and_Ethics.md)**
   - Explore the challenges of prototyping, evaluation, and ethical considerations in generative AI.

Feel free to follow the courses in the order listed above for the best learning experience.



---

## Why Prompt Engineering Matters for Frontend

AI models like GPT-4o, Claude, and Copilot can generate high-quality frontend code — but only when prompted well. Poor prompts lead to:
- Generic, unstyled components
- Missing accessibility attributes
- No TypeScript types
- No error/loading states
- Outdated APIs (React class components, etc.)

Good prompt engineering means **getting production-ready code** instead of throwaway boilerplate.

---

## Core Prompting Principles

### 1. Be Specific

```
❌ "Create a button component"

✅ "Create a React button component in TypeScript with:
   - Variants: primary, secondary, destructive, ghost
   - Sizes: sm, md, lg
   - Loading state with a spinner
   - Disabled state
   - An optional leading icon prop
   - ARIA attributes for accessibility
   - Styled with Tailwind CSS"
```

### 2. Provide Context

```
✅ "I'm building a Next.js 14 app using the App Router, TypeScript, 
   Tailwind CSS, and React Query. Create a..."
```

### 3. Specify the Constraints

```
✅ "The component should:
   - Use functional components with hooks (no class components)
   - Not use any external libraries beyond React
   - Have full TypeScript types (no `any`)
   - Be accessible (WCAG AA compliant)"
```

### 4. Show Examples (Few-Shot)

```
✅ "Generate a component similar to this existing one:

[paste an existing component]

But for displaying products instead of users."
```

### 5. Request Format

```
✅ "Output only the TypeScript code. No explanation needed."

✅ "Provide the solution with:
   1. The component code
   2. The TypeScript interface for its props
   3. A usage example"
```

---

## Prompting for HTML Generation

### Semantic Structure

```
"Generate semantic HTML5 for a blog article page that includes:
- A <header> with site navigation (logo + nav links)
- A <main> with a <article> containing:
  - Title (h1), author, publication date
  - A <figure> with an image and caption
  - Body content with h2 subheadings and paragraphs
  - A tags/categories section
- A <sidebar> with related articles
- A <footer>
Use appropriate ARIA landmark roles and heading hierarchy.
Do not include any CSS."
```

### Accessible Forms

```
"Generate an accessible HTML registration form with:
- Fields: full name, email, password, confirm password, date of birth
- Each field with a visible <label> using for/id
- Required fields marked with aria-required
- Password strength indicator
- Error message placeholders using aria-describedby
- A submit button
- A fieldset/legend for the personal info section
- Autocomplete attributes on each input"
```

---

## Prompting for CSS and Styling

### Component Styling

```
"Write CSS for a card component with the following specifications:
- White background, rounded corners (8px), box shadow
- 16px padding
- A header area with an image (16:9 aspect ratio)
- Title (18px, bold), subtitle (14px, muted gray), body text
- A footer with action buttons aligned to the right
- Hover effect: slight lift (transform + shadow)
- Dark mode support using @media (prefers-color-scheme: dark)
- Mobile responsive: full-width on screens < 600px
Output as CSS custom properties (variables) for colors and spacing."
```

### Animation

```
"Write CSS animations for:
- A skeleton loading shimmer effect
- A slide-in from the right for a sidebar (300ms, ease-out)
- A fade-in + scale for modal appearance
- A pulse animation for a notification badge
Use @keyframes and CSS variables. 
Respect prefers-reduced-motion media query."
```

### Responsive Layout

```
"Create a CSS Grid layout for a dashboard:
- 12-column grid
- Sidebar: 250px wide (collapses below 768px)
- Main content: remaining width
- Top navigation: full width, fixed
- Card grid: 3 columns (desktop), 2 (tablet), 1 (mobile)
Use CSS Grid and Flexbox, no external libraries."
```

---

## Prompting for JavaScript

### Utility Functions

```
"Write a JavaScript utility module that exports:

1. debounce(fn, delay) — delays fn until delay ms after last call
2. throttle(fn, limit) — limits fn to once per limit ms
3. deepClone(obj) — deep clones an object (handles Date, Array, nested objects)
4. groupBy(array, key) — groups array of objects by a key
5. formatDate(date, format) — formats a Date object ('YYYY-MM-DD', 'DD/MM/YYYY', 'relative')

Each function should:
- Have JSDoc comments with @param and @returns
- Handle edge cases (null, undefined, empty arrays)
- Be pure functions (no side effects)
- Include a usage example in the comment"
```

### Async Patterns

```
"Write a JavaScript function fetchWithRetry(url, options) that:
- Makes a fetch request
- Retries up to 3 times on network error or 5xx response
- Uses exponential backoff: 1s, 2s, 4s between retries
- Has a configurable timeout per attempt (default 10s) using AbortController
- Throws a descriptive error after all retries fail
- Returns the parsed JSON response on success"
```

---

## Prompting for React Components

### Full Component Prompt Template

```
"Create a React component: [ComponentName]

Purpose: [what it does in one sentence]

Props interface:
- [propName]: [type] — [description, required/optional, default value]

Behavior:
- [describe user interactions]
- [describe loading/error/empty states]
- [describe any animations or transitions]

Technical requirements:
- TypeScript (strict, no `any`)
- Tailwind CSS for styling
- React Query for data fetching (if applicable)
- Accessible (WCAG AA): proper ARIA roles, keyboard navigation
- react-hook-form for form validation (if applicable)

Include:
1. The component
2. The props TypeScript interface
3. A brief usage example"
```

### Specific Component Prompts

**Data Table:**
```
"Create a React DataTable component in TypeScript with:
- Generic type: DataTable<T extends Record<string, unknown>>
- Props: columns (ColumnDef<T>[]), data (T[]), isLoading, onRowClick
- Features: sortable columns (click header), pagination (10/25/50 per page)
- Row selection with checkboxes (single and bulk select)
- Loading skeleton (3 rows of shimmer placeholders)
- Empty state with an illustration and message
- Accessibility: role='grid', aria-sort, keyboard navigation with arrow keys
- Tailwind CSS styling"
```

**Modal / Dialog:**
```
"Create an accessible React Modal component:
- Uses the native <dialog> element
- Props: isOpen, onClose, title, children, size ('sm'|'md'|'lg'|'full')
- Features:
  - Focus trap inside modal when open
  - Focus returns to trigger element when closed
  - Closes on Escape key and backdrop click
  - Smooth open/close animation (fade + scale)
  - Scrollable body when content overflows
  - ARIA: role='dialog', aria-modal, aria-labelledby
- TypeScript, Tailwind CSS"
```

**Form with Validation:**
```
"Create a React registration form with react-hook-form and zod:

Fields with validation rules:
- name: required, 2-50 chars
- email: required, valid email format
- password: required, min 8 chars, at least 1 uppercase, 1 number, 1 special char
- confirmPassword: must match password
- agreeToTerms: required checkbox

The form should:
- Show inline errors below each field
- Disable submit button while submitting
- Show a success message after submission
- Call an onSubmit prop (async function) on valid submit
- Show server errors (e.g., 'Email already taken') returned from the API
- TypeScript types for form values"
```

---

## Prompting for TypeScript

### Type Definitions

```
"Generate TypeScript types for an e-commerce API response:

- Product: id, name, description, price (number), currency, stock, 
  images (array with url and alt), category (id + name), 
  variants (array: id, size, color, price, stock)
- Order: id, status enum (pending/processing/shipped/delivered/cancelled),
  userId, items (ProductId + quantity + priceAtOrder), 
  shippingAddress, totalAmount, createdAt, updatedAt
- User: id, email, name, role (customer|admin), 
  addresses (array with type home|work), createdAt

Use:
- Discriminated unions for status
- Readonly where appropriate
- Generic utility types where DRY applies
- Zod schema alongside each type for runtime validation"
```

### Improving Existing Types

```
"Improve the TypeScript types in this code. 
Replace `any` with proper types. 
Add missing generics. 
Use union types instead of string where appropriate.
Identify and fix type unsafe patterns.

[paste code]"
```

---

## Prompting for Testing

### Jest + RTL Tests

```
"Write comprehensive Jest + React Testing Library tests for this component:

[paste component code]

Test cases to cover:
1. Renders correctly with required props
2. Renders in each variant/state
3. User interactions (clicks, input, form submission)
4. Async operations (loading state → success state → error state)
5. Accessibility (correct ARIA attributes, keyboard navigation)
6. Edge cases: empty data, null props, boundary values

Use:
- userEvent (not fireEvent)
- MSW to mock API calls
- jest.fn() for callback props
- Descriptive test names: 'given X, when Y, then Z'"
```

### Playwright E2E Tests

```
"Write Playwright E2E tests for the checkout flow:

Steps:
1. User is on the product page
2. Selects size 'M', quantity 2
3. Clicks 'Add to Cart'
4. Verifies cart badge shows '2'
5. Clicks 'View Cart'
6. Verifies product appears in cart with correct quantity and price
7. Clicks 'Proceed to Checkout'
8. Fills in shipping form (name, address, city, zip, country)
9. Selects 'Standard Shipping'
10. Clicks 'Continue to Payment'
11. Enters card details
12. Clicks 'Place Order'
13. Verifies order confirmation page with order number

Use Page Object Model. Mock the payment API response.
Base URL: http://localhost:3000"
```

---

## Prompting for Code Review and Debugging

### Bug Investigation

```
"Debug this React component. It's supposed to [expected behavior], 
but instead [actual behavior]. Here's what I've tried: [attempts].

[paste component code]

Identify the root cause and provide a fix. Explain why it was happening."
```

### Performance Review

```
"Review this React component for performance issues:

[paste component]

Identify:
1. Unnecessary re-renders (explain why each occurs)
2. Missing memoization opportunities (useMemo, useCallback, React.memo)
3. Expensive computations that should be optimized
4. Missing keys or incorrect key usage
5. useEffect dependency array issues

For each issue, show the fix with before/after code."
```

### Accessibility Audit

```
"Audit this HTML/React component for accessibility issues.
Check against WCAG 2.1 AA criteria:
- Missing or incorrect ARIA roles/attributes
- Keyboard navigation problems
- Focus management issues
- Color contrast (describe any potential issues)
- Missing alternative text
- Form labeling
- Heading hierarchy

For each issue: describe the problem, its WCAG criterion, and the fix.

[paste code]"
```

---

## Prompting for Architecture Decisions

```
"I'm building a React SPA with these requirements:
- ~50 different pages
- Real-time notifications via WebSocket
- Complex forms with multi-step wizards
- A data grid with 10,000+ rows
- Role-based access control
- Must work offline (PWA)

Recommend:
1. State management approach (and why)
2. Routing strategy
3. Data fetching library
4. Key performance optimizations
5. Folder structure
Explain the tradeoffs of your choices."
```

---

## System Prompts for Code Generation

When using the OpenAI API to build a code generation tool, a good system prompt is essential.

```ts
const FRONTEND_SYSTEM_PROMPT = `
You are an expert senior frontend developer. When generating code, always:

TECHNICAL STANDARDS:
- Use TypeScript with strict mode (no \`any\` types)
- Use React functional components with hooks (never class components)
- Use modern ES2022+ syntax (optional chaining, nullish coalescing, etc.)

STYLING:
- Use Tailwind CSS utility classes
- Support dark mode with dark: variants
- Make components mobile-first and responsive

ACCESSIBILITY:
- Use semantic HTML elements
- Add ARIA attributes where native semantics aren't sufficient
- Ensure keyboard navigability for interactive elements
- Add visible focus indicators

QUALITY:
- Handle loading, error, and empty states
- Add TypeScript interfaces for all props
- Write components as pure as possible
- Avoid prop drilling deeper than 2 levels (use Context or composition)

OUTPUT FORMAT:
- Output only the code unless asked for explanation
- Include a brief usage example as a comment at the top
- Organize imports: React → third-party → local
`;
```

---

## Anti-Patterns to Avoid

| ❌ Poor Prompt | ✅ Better Approach |
|---|---|
| "Make a nav bar" | Specify: links, mobile behavior, active state, sticky, dark mode |
| "Fix my code" | Describe the exact bug, expected vs actual behavior |
| "Is this good?" | Ask about specific concerns: perf, a11y, types, security |
| "Write tests" | Specify: which behaviors, which test framework, what to mock |
| Pasting 500 lines | Isolate the relevant code section |
| Vague tech stack | Always specify: framework, version, CSS solution, TypeScript |
| One huge prompt | Break into smaller, focused prompts for complex tasks |
| Accepting first output | Iterate: "Now add error handling", "Make it accessible" |
