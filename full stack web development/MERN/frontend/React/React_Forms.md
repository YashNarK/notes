# React — Forms

## Table of contents

- [React — Forms](#react--forms)
  - [Table of contents](#table-of-contents)
  - [Form Concepts](#form-concepts)
    - [What is an HTML Form?](#what-is-an-html-form)
    - [The Form Lifecycle](#the-form-lifecycle)
    - [Controlled vs Uncontrolled Inputs](#controlled-vs-uncontrolled-inputs)
    - [What is FormData?](#what-is-formdata)
    - [Why Forms in React Need Extra Care](#why-forms-in-react-need-extra-care)
  - [Plain React](#plain-react)
    - [Uncontrolled Forms (HTML Native)](#uncontrolled-forms-html-native)
    - [Controlled Forms with useState](#controlled-forms-with-usestate)
    - [React Hook Form (Recommended)](#react-hook-form-recommended)
    - [Field Arrays](#field-arrays)
    - [Integrating with Validation (Zod)](#integrating-with-validation-zod)
  - [Next.js](#nextjs)
    - [Server Actions (Recommended)](#server-actions-recommended)
    - [Progressive Enhancement](#progressive-enhancement)
    - [useFormState \& useFormStatus](#useformstate--useformstatus)
    - [React Hook Form with Server Actions](#react-hook-form-with-server-actions)

---

## Form Concepts

### What is an HTML Form?

A **form** is the primary way users send data to a server in a web application. Login screens, search boxes, checkout flows, profile editors — all are forms.

In plain HTML, a form collects input and submits it via HTTP:

```html
<form action="/login" method="POST">
  <input name="email" type="email" />
  <input name="password" type="password" />
  <button type="submit">Login</button>
</form>
<!-- Clicking submit sends POST /login with body: email=...&password=... -->
<!-- The browser navigates to the response page (full page reload) -->
```

In a React SPA, you intercept the form submission with JavaScript to avoid the page reload and handle the response in-app.

### The Form Lifecycle

```
1. Render          → display empty/pre-filled input fields
2. User types      → input values change
3. Validate        → check values before submission (optional at each keystroke, required on submit)
4. Submit          → user clicks submit button
5. Send to server  → make HTTP request (or call Server Action in Next.js)
6. Handle response → show success message, redirect, or display server errors
7. Reset / update  → clear form or show updated data
```

### Controlled vs Uncontrolled Inputs

This is the most important concept in React forms.

**Uncontrolled input** — the DOM owns the value. React doesn't track it.

```tsx
// React doesn't know what's typed until you read it on submit
<input name="email" type="email" defaultValue="alice@example.com" />
```

**Controlled input** — React owns the value via state. Every keystroke updates state.

```tsx
const [email, setEmail] = useState('');
<input
  value={email}                           // React controls the displayed value
  onChange={e => setEmail(e.target.value)} // React updates state on every keystroke
/>
```

| | Controlled | Uncontrolled |
|---|---|---|
| Value lives in | React state | DOM |
| Access value | `state` variable | `ref.current.value` or `FormData` |
| Live validation | ✅ Easy | ⚠️ Harder |
| Re-renders per keystroke | Yes (every key) | No |
| Good for | Complex forms with interdependencies | Simple, large forms |

> **React Hook Form uses uncontrolled inputs** under the hood — this is why it's more performant than `useState`-based forms for large forms.

### What is FormData?

`FormData` is a browser built-in that collects all named inputs in a form:

```tsx
const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
  e.preventDefault();
  const data = new FormData(e.currentTarget);
  console.log(data.get('email'));    // "alice@example.com"
  console.log(data.get('password')); // "secret"
};
```

Every `<input>`, `<select>`, and `<textarea>` with a `name` attribute is automatically included. This is how Server Actions in Next.js receive form data.

### Why Forms in React Need Extra Care

Plain HTML forms do a full page reload on submit. React forms must:

1. **Prevent default browser behaviour** — `e.preventDefault()`
2. **Track input values** — controlled state or `FormData`
3. **Validate before submitting** — show errors without a page reload
4. **Handle async submission** — show loading state while the server responds
5. **Handle server errors** — display validation errors returned from the backend
6. **Prevent double submission** — disable the submit button while pending

Doing all this manually for every form is tedious. **React Hook Form** handles steps 2–6 for you.

---

## Plain React

### Uncontrolled Forms (HTML Native)

Use `FormData` for simple forms where you don't need live field values.

```tsx
export function SimpleForm() {
  const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    const data = new FormData(e.currentTarget);
    console.log(data.get('email'), data.get('password'));
  };

  return (
    <form onSubmit={handleSubmit}>
      <input name="email" type="email" required />
      <input name="password" type="password" required />
      <button type="submit">Login</button>
    </form>
  );
}
```

### Controlled Forms with useState

Controlled inputs bind value to state, enabling live validation and conditional rendering.

```tsx
import { useState } from 'react';

export function ContactForm() {
  const [name, setName]   = useState('');
  const [email, setEmail] = useState('');
  const [error, setError] = useState('');

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    if (!email.includes('@')) { setError('Invalid email'); return; }
    setError('');
    console.log({ name, email });
  };

  return (
    <form onSubmit={handleSubmit}>
      <input value={name}  onChange={e => setName(e.target.value)}  placeholder="Name" />
      <input value={email} onChange={e => setEmail(e.target.value)} type="email" placeholder="Email" />
      {error && <p className="error">{error}</p>}
      <button type="submit">Send</button>
    </form>
  );
}
```

> **Gotcha**: Every keystroke triggers a re-render in controlled forms. For large forms, prefer React Hook Form which uses uncontrolled inputs under the hood.

### React Hook Form (Recommended)

React Hook Form (RHF) uses uncontrolled inputs with a `register` API, minimising re-renders.

```bash
npm i react-hook-form
```

```tsx
import { useForm, SubmitHandler } from 'react-hook-form';

interface FormValues {
  firstName: string;
  email: string;
  age: number;
}

export function SignupForm() {
  const {
    register,
    handleSubmit,
    formState: { errors, isSubmitting },
    reset,
  } = useForm<FormValues>({ defaultValues: { firstName: '', email: '', age: 18 } });

  const onSubmit: SubmitHandler<FormValues> = async data => {
    await api.post('/users', data);
    reset();
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input
        {...register('firstName', { required: 'First name is required' })}
        placeholder="First name"
      />
      {errors.firstName && <span>{errors.firstName.message}</span>}

      <input
        {...register('email', {
          required: 'Email is required',
          pattern: { value: /\S+@\S+\.\S+/, message: 'Invalid email' },
        })}
        type="email"
      />
      {errors.email && <span>{errors.email.message}</span>}

      <input
        {...register('age', { valueAsNumber: true, min: { value: 0, message: 'Must be positive' } })}
        type="number"
      />
      {errors.age && <span>{errors.age.message}</span>}

      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? 'Submitting…' : 'Sign Up'}
      </button>
    </form>
  );
}
```

### Field Arrays

Manage dynamic lists of fields with `useFieldArray`:

```tsx
import { useForm, useFieldArray } from 'react-hook-form';

interface FormValues { tags: { value: string }[] }

export function TagsForm() {
  const { register, control, handleSubmit } = useForm<FormValues>({
    defaultValues: { tags: [{ value: '' }] },
  });
  const { fields, append, remove } = useFieldArray({ control, name: 'tags' });

  return (
    <form onSubmit={handleSubmit(console.log)}>
      {fields.map((field, index) => (
        <div key={field.id}>
          <input {...register(`tags.${index}.value`)} />
          <button type="button" onClick={() => remove(index)}>Remove</button>
        </div>
      ))}
      <button type="button" onClick={() => append({ value: '' })}>Add Tag</button>
      <button type="submit">Save</button>
    </form>
  );
}
```

### Integrating with Validation (Zod)

See [React_FormValidation.md](React_FormValidation.md) for full Zod + RHF integration.

---

## Next.js

### Server Actions (Recommended)

Server Actions let you handle form submissions server-side with no API route boilerplate.

```tsx
// app/actions.ts
'use server';
import { z } from 'zod';
import { revalidatePath } from 'next/cache';

const schema = z.object({
  name: z.string().min(1),
  email: z.string().email(),
});

export async function createUser(formData: FormData) {
  const result = schema.safeParse({
    name:  formData.get('name'),
    email: formData.get('email'),
  });

  if (!result.success) {
    return { error: result.error.flatten().fieldErrors };
  }

  await db.user.create({ data: result.data });
  revalidatePath('/users');
}
```

```tsx
// app/users/new/page.tsx — Server Component
import { createUser } from '@/app/actions';

export default function NewUserPage() {
  return (
    <form action={createUser}>
      <input name="name"  placeholder="Name"  required />
      <input name="email" type="email" placeholder="Email" required />
      <button type="submit">Create</button>
    </form>
  );
}
```

### Progressive Enhancement

Forms with Server Actions work **without JavaScript** — the `action` attribute sends a standard HTML form POST. JavaScript enhances the experience with `useFormStatus` and `useFormState`.

### useFormState & useFormStatus

```tsx
'use client';
import { useFormState, useFormStatus } from 'react-dom';
import { createUser } from '@/app/actions';

function SubmitButton() {
  const { pending } = useFormStatus();
  return <button type="submit" disabled={pending}>{pending ? 'Saving…' : 'Create'}</button>;
}

export function CreateUserForm() {
  const [state, action] = useFormState(createUser, { error: null });

  return (
    <form action={action}>
      <input name="name" />
      <input name="email" type="email" />
      {state.error && <p>{JSON.stringify(state.error)}</p>}
      <SubmitButton />
    </form>
  );
}
```

> `useFormStatus` must be used in a **child** of the `<form>` element, not in the same component.

### React Hook Form with Server Actions

Combine RHF's client-side DX with Server Actions for the submission:

```tsx
'use client';
import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { z } from 'zod';
import { createUser } from '@/app/actions';

const schema = z.object({ name: z.string().min(1), email: z.string().email() });
type FormValues = z.infer<typeof schema>;

export function CreateUserForm() {
  const { register, handleSubmit, formState: { errors } } = useForm<FormValues>({
    resolver: zodResolver(schema),
  });

  const onSubmit = async (data: FormValues) => {
    const fd = new FormData();
    Object.entries(data).forEach(([k, v]) => fd.append(k, v));
    await createUser(fd);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input {...register('name')} />
      {errors.name && <span>{errors.name.message}</span>}
      <input {...register('email')} type="email" />
      {errors.email && <span>{errors.email.message}</span>}
      <button type="submit">Create</button>
    </form>
  );
}
```
