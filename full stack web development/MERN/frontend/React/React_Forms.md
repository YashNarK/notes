# React — Forms

Forms in React require controlled input management, submission handling, and integration with validation libraries.

## Table of contents

- [React — Forms](#react--forms)
  - [Table of contents](#table-of-contents)
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
