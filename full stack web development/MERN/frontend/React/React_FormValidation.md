# React — Form Validation

## Table of contents

- [React — Form Validation](#react--form-validation)
  - [Table of contents](#table-of-contents)
  - [Validation Concepts](#validation-concepts)
    - [What is Validation?](#what-is-validation)
    - [Client-Side vs Server-Side Validation](#client-side-vs-server-side-validation)
    - [Validation Timing](#validation-timing)
    - [Manual Validation vs Schema-Based Validation](#manual-validation-vs-schema-based-validation)
    - [What is a Schema?](#what-is-a-schema)
    - [Error Messages and UX](#error-messages-and-ux)
  - [Plain React](#plain-react)
    - [Zod Schemas](#zod-schemas)
    - [Common Validation Rules](#common-validation-rules)
    - [React Hook Form + Zod Resolver](#react-hook-form--zod-resolver)
    - [Custom Validators](#custom-validators)
    - [Async Validation](#async-validation)
  - [Next.js](#nextjs)
    - [Server-side Validation with Zod](#server-side-validation-with-zod)
    - [Returning Validation Errors to the Client](#returning-validation-errors-to-the-client)
    - [Shared Schema Between Client and Server](#shared-schema-between-client-and-server)

---

## Validation Concepts

### What is Validation?

**Validation** is the process of checking that user input meets your requirements before doing anything with it (saving to database, sending email, making a payment).

Without validation:
```
User submits: email = "not-an-email", age = -5, name = ""
→ Stored in database as garbage
→ Email sending fails silently
→ System crashes on unexpected data
```

With validation:
```
User submits: email = "not-an-email"
→ Rejected immediately with "Please enter a valid email address"
→ User fixes it → submit succeeds
```

### Client-Side vs Server-Side Validation

There are two places validation can run — and **you need both**:

**Client-side validation** — runs in the browser, before the HTTP request is sent:

```tsx
// Checked in JavaScript, no server round-trip
if (!email.includes('@')) {
  setError('Invalid email');
  return; // stops submission
}
```

✅ Instant feedback — no waiting for a server response  
✅ Saves bandwidth — bad data never reaches the server  
❌ **Can be bypassed** — anyone can open DevTools and submit directly

**Server-side validation** — runs on the backend, after receiving the request:

```ts
// In a Server Action or API route
const result = schema.safeParse(formData);
if (!result.success) return { error: result.error };
```

✅ **Cannot be bypassed** — always runs, no matter how the request was made  
✅ Can check things the client can't (e.g., "is this email already taken?" via database)  
❌ Requires a network round-trip — slower feedback

> **Rule:** Always validate on the server. Client-side validation is an enhancement for UX, not a security measure.

### Validation Timing

When should validation run? There are three options:

| Timing | Trigger | UX impact |
|---|---|---|
| `onChange` | Every keystroke | Aggressive — shows errors while user is still typing |
| `onBlur` | When field loses focus | Good balance — shows error after user leaves the field |
| `onSubmit` | When submit button is clicked | Minimal — user only sees errors after trying to submit |

**Recommended:** validate `onSubmit` first (shows all errors at once), then switch to `onChange` **after** the first failed submit so the user gets live feedback while fixing.

React Hook Form supports this with `mode: 'onSubmit'` + `reValidateMode: 'onChange'`.

### Manual Validation vs Schema-Based Validation

**Manual validation** — write `if` statements for each rule:

```tsx
const errors: Record<string, string> = {};

if (!name) errors.name = 'Name is required';
if (name.length > 50) errors.name = 'Name too long';
if (!email) errors.email = 'Email is required';
if (!/\S+@\S+\.\S+/.test(email)) errors.email = 'Invalid email';
if (!password) errors.password = 'Password is required';
if (password.length < 8) errors.password = 'At least 8 characters';
if (password !== confirmPassword) errors.confirmPassword = 'Passwords do not match';
```

**Schema-based validation** — declare the shape and rules once:

```ts
const schema = z.object({
  name:            z.string().min(1).max(50),
  email:           z.string().email(),
  password:        z.string().min(8),
  confirmPassword: z.string(),
}).refine(d => d.password === d.confirmPassword, {
  message: 'Passwords do not match',
  path: ['confirmPassword'],
});
```

| | Manual | Schema-based (Zod) |
|---|---|---|
| Code volume | Many `if` statements | Concise, declarative |
| Type inference | Manual TypeScript types | Automatic from schema |
| Reuse on server | Duplicate logic | Same schema on client + server |
| Nested objects | Verbose | Clean |

### What is a Schema?

A **schema** is a declaration of what valid data looks like — its shape, types, and constraints.

```
Schema:  name must be a non-empty string
         email must match email format
         age must be a number between 0 and 120

Data:    { name: "Alice", email: "alice@example.com", age: 28 }  → ✅ valid
         { name: "", email: "not-email", age: -1 }               → ❌ 3 errors
```

Zod schemas serve double duty:
1. **Runtime validation** — check data at runtime
2. **TypeScript types** — infer a TypeScript type automatically via `z.infer<typeof schema>`

### Error Messages and UX

Good error messages tell the user **what's wrong and how to fix it**:

| ❌ Unhelpful | ✅ Helpful |
|---|---|
| "Invalid input" | "Please enter a valid email address (e.g. you@example.com)" |
| "Error" | "Password must be at least 8 characters" |
| "Field required" | "Name is required" |

**Placement:** Show errors directly below the field they belong to, not in a banner at the top of the form.

**Timing:** Don't show errors before the user has had a chance to fill in a field — this is called "premature validation" and is frustrating.

---

## Plain React

### Zod Schemas

Zod defines data shapes as TypeScript types and runtime validators in one declaration.

```bash
npm i zod
```

```ts
import { z } from 'zod';

const userSchema = z.object({
  name:      z.string().min(1, 'Name is required').max(50),
  email:     z.string().email('Invalid email address'),
  age:       z.number().int().min(0).max(120).optional(),
  role:      z.enum(['admin', 'editor', 'viewer']).default('viewer'),
  password:  z.string().min(8, 'At least 8 characters'),
  confirmPassword: z.string(),
}).refine(data => data.password === data.confirmPassword, {
  message: 'Passwords do not match',
  path: ['confirmPassword'],
});

// Infer the TypeScript type automatically
type User = z.infer<typeof userSchema>;
```

### Common Validation Rules

```ts
import { z } from 'zod';

z.string().url('Must be a valid URL')
z.string().regex(/^\d{5}$/, 'Must be a 5-digit ZIP code')
z.string().uuid()
z.string().startsWith('https://')
z.number().positive().finite()
z.number().multipleOf(5)
z.array(z.string()).nonempty().max(10)
z.date().min(new Date('2000-01-01'), 'Too old')
z.boolean()
z.literal('active')
z.union([z.string(), z.number()])
z.discriminatedUnion('type', [
  z.object({ type: z.literal('text'), content: z.string() }),
  z.object({ type: z.literal('image'), src: z.string().url() }),
])
```

### React Hook Form + Zod Resolver

The `@hookform/resolvers` package connects RHF with Zod.

```bash
npm i @hookform/resolvers
```

```tsx
import { useForm, SubmitHandler } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { z } from 'zod';

const schema = z.object({
  name:  z.string().min(1, 'Name is required'),
  email: z.string().email('Invalid email'),
  age:   z.coerce.number().min(18, 'Must be 18 or older'),
});

type FormValues = z.infer<typeof schema>;

export function RegisterForm() {
  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm<FormValues>({ resolver: zodResolver(schema) });

  const onSubmit: SubmitHandler<FormValues> = data => {
    console.log(data); // typed as FormValues
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <div>
        <input {...register('name')} placeholder="Name" />
        {errors.name && <p>{errors.name.message}</p>}
      </div>
      <div>
        <input {...register('email')} type="email" placeholder="Email" />
        {errors.email && <p>{errors.email.message}</p>}
      </div>
      <div>
        <input {...register('age')} type="number" placeholder="Age" />
        {errors.age && <p>{errors.age.message}</p>}
      </div>
      <button type="submit">Register</button>
    </form>
  );
}
```

> **Tip**: Use `z.coerce.number()` for number inputs — HTML inputs always return strings; `coerce` converts them automatically.

### Custom Validators

Add cross-field or business-logic validation with `.superRefine()`:

```ts
const checkoutSchema = z.object({
  country: z.string(),
  state:   z.string(),
}).superRefine((data, ctx) => {
  const usStates = ['CA', 'NY', 'TX'];
  if (data.country === 'US' && !usStates.includes(data.state)) {
    ctx.addIssue({
      code: z.ZodIssueCode.custom,
      message: 'Invalid US state',
      path: ['state'],
    });
  }
});
```

### Async Validation

Use `.refine()` with an async function for server-side uniqueness checks:

```ts
const emailSchema = z.object({
  email: z.string().email().refine(
    async email => {
      const res = await fetch(`/api/check-email?email=${email}`);
      const { available } = await res.json();
      return available;
    },
    { message: 'Email already taken' }
  ),
});

// With RHF: set mode: 'onBlur' to avoid too-frequent API calls
const { register, handleSubmit } = useForm({ resolver: zodResolver(emailSchema), mode: 'onBlur' });
```

---

## Next.js

### Server-side Validation with Zod

Always validate on the server in Server Actions — client-side validation can be bypassed.

```ts
// app/actions.ts
'use server';
import { z } from 'zod';

const schema = z.object({
  name:  z.string().min(1),
  email: z.string().email(),
});

export async function createUser(_prevState: unknown, formData: FormData) {
  const result = schema.safeParse({
    name:  formData.get('name'),
    email: formData.get('email'),
  });

  if (!result.success) {
    // Return structured errors to the client
    return { success: false, errors: result.error.flatten().fieldErrors };
  }

  await db.user.create({ data: result.data });
  return { success: true, errors: null };
}
```

### Returning Validation Errors to the Client

```tsx
'use client';
import { useFormState } from 'react-dom';
import { createUser } from '@/app/actions';

type State = { success: boolean; errors: { name?: string[]; email?: string[] } | null };

export function CreateUserForm() {
  const [state, action] = useFormState<State, FormData>(createUser, { success: false, errors: null });

  return (
    <form action={action}>
      <div>
        <input name="name" />
        {state.errors?.name && <p>{state.errors.name[0]}</p>}
      </div>
      <div>
        <input name="email" type="email" />
        {state.errors?.email && <p>{state.errors.email[0]}</p>}
      </div>
      {state.success && <p>User created!</p>}
      <button type="submit">Create</button>
    </form>
  );
}
```

### Shared Schema Between Client and Server

Define the Zod schema once and use it on both sides:

```ts
// lib/schemas/user.ts — shared, no 'use server' or 'use client'
import { z } from 'zod';

export const createUserSchema = z.object({
  name:  z.string().min(1, 'Name is required'),
  email: z.string().email('Invalid email'),
});

export type CreateUserInput = z.infer<typeof createUserSchema>;
```

```tsx
// Client: react-hook-form + zodResolver
import { createUserSchema, CreateUserInput } from '@/lib/schemas/user';
const form = useForm<CreateUserInput>({ resolver: zodResolver(createUserSchema) });

// Server: safeParse in Server Action
import { createUserSchema } from '@/lib/schemas/user';
const result = createUserSchema.safeParse(Object.fromEntries(formData));
```
