# React — Form Validation

Form validation ensures data integrity before it reaches the server. Zod is the recommended schema library; it integrates with React Hook Form via a resolver.

## Table of contents

- [React — Form Validation](#react--form-validation)
  - [Table of contents](#table-of-contents)
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
