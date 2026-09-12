# Step 16 — React Hook Form

Now we move to the **last part of Phase 7: Form State / Validation**.

Your roadmap says:

```text
Form state/validation: React Hook Form + Zod
```

We'll learn **React Hook Form first**, then **Zod**.

---

# 1. What Problem Are We Solving?

Imagine a signup form:

```text
Name
[____________]

Email
[____________]

Password
[____________]

[ Sign Up ]
```

With normal React, you might create:

```text
name state
email state
password state
```

Then create handlers for each input.

For a small form, that's okay.

But imagine a form with:

```text
Name
Email
Password
Confirm Password
Phone
Address
City
Country
Age
Company
...
```

Managing every field manually becomes repetitive.

That's where **React Hook Form** helps.

---

# 2. What Is React Hook Form?

React Hook Form is a library that helps us manage:

* Form values
* Form submission
* Validation
* Errors
* Form state

Think:

```text
React Hook Form
       ↓
Manages the form
       ↓
values
errors
submit
validation
```

---

# 3. Install It

Since you're using Vite + React + TypeScript:

```text
npm install react-hook-form
```

After installation, we can use:

```text
useForm()
```

---

# 4. Create a Signup Form

Create:

```text
src/SignupForm.tsx
```

Start with:

```tsx
import { useForm } from "react-hook-form";

function SignupForm() {
    const {
        register,
        handleSubmit
    } = useForm();

    return (
        <form>
            <input
                {...register("name")}
                placeholder="Name"
            />

            <input
                {...register("email")}
                placeholder="Email"
            />

            <button type="submit">
                Sign Up
            </button>
        </form>
    );
}

export default SignupForm;
```

Don't worry about the `...register()` part yet.

We'll understand it.

---

# 5. What Is `useForm()`?

This:

```tsx
const {
    register,
    handleSubmit
} = useForm();
```

gives us functions for working with the form.

The important ones we'll learn are:

```text
register
handleSubmit
formState
```

Later we'll also see:

```text
watch
setValue
reset
```

---

# 6. What Does `register()` Do?

This:

```tsx
register("name")
```

means:

> Register this input as a field called `name`.

For example:

```tsx
<input {...register("name")} />
```

Now React Hook Form knows:

```text
Form
│
└── name
```

If the user types:

```text
William
```

React Hook Form tracks:

```text
name = "William"
```

---

# 7. Multiple Fields

We can register:

```tsx
<input {...register("name")} />

<input {...register("email")} />

<input {...register("password")} />
```

Now the form data looks like:

```text
{
    name: "...",
    email: "...",
    password: "..."
}
```

React Hook Form manages those values for us.

---

# 8. `handleSubmit()`

Now we need to handle the form submission.

Create a function:

```tsx
const onSubmit = (data) => {
    console.log(data);
};
```

Then:

```tsx
<form onSubmit={handleSubmit(onSubmit)}>
```

Complete example:

```tsx
import { useForm } from "react-hook-form";

function SignupForm() {
    const {
        register,
        handleSubmit
    } = useForm();

    const onSubmit = (data) => {
        console.log(data);
    };

    return (
        <form onSubmit={handleSubmit(onSubmit)}>
            <input
                {...register("name")}
                placeholder="Name"
            />

            <input
                {...register("email")}
                placeholder="Email"
            />

            <input
                {...register("password")}
                placeholder="Password"
            />

            <button type="submit">
                Sign Up
            </button>
        </form>
    );
}

export default SignupForm;
```

---

# 9. What Happens When We Submit?

Suppose the user enters:

```text
Name:
William

Email:
william@example.com

Password:
123456
```

and clicks:

```text
Sign Up
```

The flow is:

```text
User fills form
       ↓
React Hook Form
       ↓
handleSubmit()
       ↓
onSubmit()
       ↓
form data
```

The `data` will contain:

```text
{
    name: "William",
    email: "william@example.com",
    password: "123456"
}
```

---

# 10. Why Is This Better Than `useState()`?

With `useState()`, we might do:

```text
name state
email state
password state
```

Then:

```text
onNameChange
onEmailChange
onPasswordChange
```

React Hook Form gives us:

```text
useForm()
    ↓
register()
    ↓
handleSubmit()
```

So there is less repetitive form-management code.

---

# 11. Now Add Validation

Our form currently accepts anything.

For example:

```text
Name: empty
Email: abc
Password: 1
```

We don't want that.

React Hook Form allows validation rules directly in `register()`.

For example:

```tsx
<input
    {...register("name", {
        required: true
    })}
/>
```

This means:

> Name is required.

---

# 12. Email Validation

We can add:

```tsx
<input
    {...register("email", {
        required: true,
        pattern: /^\S+@\S+$/i
    })}
/>
```

Now we're starting to create validation rules.

But there's a problem.

As forms become more complex, putting lots of validation rules inside `register()` can become difficult to maintain.

That's where **Zod** comes in.

---

# 13. React Hook Form + Zod

The combination we'll eventually use is:

```text
React Hook Form
       +
      Zod
```

Think of their responsibilities like this:

```text
React Hook Form
       ↓
Manages the form

Zod
       ↓
Defines the validation rules
```

For example:

```text
React Hook Form
     ↓
"What values did the user enter?"

Zod
     ↓
"Are those values valid?"
```

---

# 14. Our Final Form Architecture

We'll eventually build:

```text
SignupForm
     │
     ↓
React Hook Form
     │
     ↓
Form values
     │
     ↓
Zod validation
     │
     ├── Valid
     │     ↓
     │   Submit
     │
     └── Invalid
           ↓
        Show errors
```

---

# 15. Important Distinction

Don't confuse these:

### Redux

Used for **shared application state**.

Example:

```text
cart
auth
user
global settings
```

### React Hook Form

Used for **form state**.

Example:

```text
name
email
password
```

### Zod

Used for **validation**.

Example:

```text
email must be valid
password must have minimum length
name is required
```

So:

```text
Redux
→ application state

React Hook Form
→ form state

Zod
→ validation
```

This distinction is very important.

---

# 16. Phase 7 Status

We have now started the final section:

```text
## 7. State Management

[x] Context API
[x] Redux Toolkit
[x] Cart System

[x] React Hook Form basics
[ ] React Hook Form + TypeScript
[ ] Zod
[ ] React Hook Form + Zod
[ ] Validated Signup Form
```

The **next step** will be:

### Step 17 — React Hook Form + TypeScript

We'll type our form properly:

```text
FormData
   ↓
name
email
password
```

Then TypeScript will know exactly what `data` contains.

-------------------------------------------------------------------------------------------------------------------------------------------

# Step 17 — React Hook Form + TypeScript

Now we'll connect **React Hook Form with TypeScript**.

The main problem from the previous step was this:

```tsx
const onSubmit = (data) => {
    console.log(data);
};
```

TypeScript doesn't know what `data` contains.

We are going to fix that.

---

# 1. Create a Form Type

Our signup form has:

```text
Name
Email
Password
```

So we define:

```tsx
type SignupFormData = {
    name: string;
    email: string;
    password: string;
};
```

Now TypeScript knows exactly what our form data looks like.

---

# 2. Give `useForm()` the Type

Instead of:

```tsx
const {
    register,
    handleSubmit
} = useForm();
```

we write:

```tsx
const {
    register,
    handleSubmit
} = useForm<SignupFormData>();
```

This tells React Hook Form:

> This form uses `SignupFormData`.

---

# 3. Type `onSubmit()`

Now:

```tsx
const onSubmit = (data: SignupFormData) => {
    console.log(data);
};
```

TypeScript knows:

```text
data
│
├── name: string
├── email: string
└── password: string
```

---

# 4. Complete Example

```tsx
import { useForm } from "react-hook-form";

type SignupFormData = {
    name: string;
    email: string;
    password: string;
};

function SignupForm() {
    const {
        register,
        handleSubmit
    } = useForm<SignupFormData>();

    const onSubmit = (data: SignupFormData) => {
        console.log(data);
    };

    return (
        <form onSubmit={handleSubmit(onSubmit)}>
            <input
                {...register("name")}
                placeholder="Name"
            />

            <input
                {...register("email")}
                placeholder="Email"
            />

            <input
                {...register("password")}
                placeholder="Password"
            />

            <button type="submit">
                Sign Up
            </button>
        </form>
    );
}

export default SignupForm;
```

---

# 5. Why This Is Useful

Now look at:

```tsx
register("name")
```

Because we told React Hook Form:

```tsx
useForm<SignupFormData>()
```

TypeScript knows that valid fields are:

```text
name
email
password
```

So this is correct:

```tsx
register("name")
```

```tsx
register("email")
```

```tsx
register("password")
```

But if you accidentally write:

```tsx
register("username")
```

TypeScript can warn you because `username` isn't part of `SignupFormData`.

That's one of the main benefits of using TypeScript here.

---

# 6. What Happens to `data`?

Suppose the user enters:

```text
Name
William

Email
william@example.com

Password
123456
```

Then:

```tsx
const onSubmit = (data: SignupFormData) => {
    console.log(data);
};
```

receives:

```text
data
│
├── name
│     └── "William"
│
├── email
│     └── "william@example.com"
│
└── password
      └── "123456"
```

TypeScript knows all of this.

---

# 7. Add Default Values

React Hook Form can also start the form with default values.

```tsx
const {
    register,
    handleSubmit
} = useForm<SignupFormData>({
    defaultValues: {
        name: "",
        email: "",
        password: ""
    }
});
```

So our form starts as:

```text
name: ""
email: ""
password: ""
```

This isn't always necessary, but it's useful when you want explicit initial values.

---

# 8. Add Basic Validation

We can also add validation rules.

For example:

```tsx
<input
    {...register("name", {
        required: "Name is required"
    })}
/>
```

And:

```tsx
<input
    {...register("email", {
        required: "Email is required"
    })}
/>
```

And:

```tsx
<input
    {...register("password", {
        required: "Password is required",
        minLength: {
            value: 6,
            message: "Password must be at least 6 characters"
        }
    })}
/>
```

Now the form has validation.

But we need a way to display those errors.

---

# 9. `formState.errors`

React Hook Form gives us:

```tsx
formState
```

Inside it:

```text
errors
```

So:

```tsx
const {
    register,
    handleSubmit,
    formState: { errors }
} = useForm<SignupFormData>();
```

Now we can check:

```tsx
errors.name
```

```tsx
errors.email
```

```tsx
errors.password
```

---

# 10. Display an Error

For example:

```tsx
{errors.name && (
    <p>{errors.name.message}</p>
)}
```

If the user doesn't enter a name:

```text
Name is required
```

will appear.

---

# 11. Full Example With Validation

```tsx
import { useForm } from "react-hook-form";

type SignupFormData = {
    name: string;
    email: string;
    password: string;
};

function SignupForm() {
    const {
        register,
        handleSubmit,
        formState: { errors }
    } = useForm<SignupFormData>();

    const onSubmit = (data: SignupFormData) => {
        console.log(data);
    };

    return (
        <form onSubmit={handleSubmit(onSubmit)}>
            <div>
                <input
                    {...register("name", {
                        required: "Name is required"
                    })}
                    placeholder="Name"
                />

                {errors.name && (
                    <p>{errors.name.message}</p>
                )}
            </div>

            <div>
                <input
                    {...register("email", {
                        required: "Email is required"
                    })}
                    placeholder="Email"
                />

                {errors.email && (
                    <p>{errors.email.message}</p>
                )}
            </div>

            <div>
                <input
                    type="password"
                    {...register("password", {
                        required: "Password is required",
                        minLength: {
                            value: 6,
                            message:
                                "Password must be at least 6 characters"
                        }
                    })}
                    placeholder="Password"
                />

                {errors.password && (
                    <p>{errors.password.message}</p>
                )}
            </div>

            <button type="submit">
                Sign Up
            </button>
        </form>
    );
}

export default SignupForm;
```

---

# 12. Understand the Flow

When the user clicks **Sign Up**:

```text
User
 ↓
Submit
 ↓
handleSubmit()
 ↓
Validation
 ↓
 ┌───────────────┐
 │               │
Valid          Invalid
 │               │
 ↓               ↓
onSubmit()    errors
 │               │
 ↓               ↓
data          Show message
```

For example:

```text
Name: William
Email: william@example.com
Password: 123456
```

Valid:

```text
        ↓
   onSubmit(data)
```

But:

```text
Name: ""
Email: ""
Password: "1"
```

Invalid:

```text
        ↓
      errors
        ↓
Name is required
Email is required
Password must be at least 6 characters
```

---

# 13. Important: We Are Not Finished With Validation Yet

Right now we're putting rules directly inside:

```tsx
register(...)
```

For example:

```tsx
required
minLength
```

This works.

But our roadmap specifically says:

```text
React Hook Form + Zod
```

So we eventually want validation to look more like:

```text
React Hook Form
      │
      ↓
     Zod
      │
      ↓
Validation schema
```

Instead of putting every validation rule inside every input.

---

# 14. React Hook Form vs Zod

Remember this distinction:

### React Hook Form

Manages:

```text
form values
submission
form state
errors
```

### Zod

Defines:

```text
validation rules
```

For example, Zod can define:

```text
name → required
email → valid email
password → minimum 6 characters
```

So:

```text
                 Signup Form
                     │
                     ↓
              React Hook Form
                     │
                     ↓
                    Zod
                     │
             ┌───────┴───────┐
             ↓               ↓
           Valid           Invalid
             ↓               ↓
         Submit            Errors
```

---

# 15. Phase 7 Progress

```text
## 7. State Management

### Context API

[x] Context API
[x] Auth state app

### Redux Toolkit

[x] Redux basics
[x] Counter
[x] Cart System
[x] TypeScript + Redux

### Forms

[x] React Hook Form basics
[x] React Hook Form + TypeScript
[x] Form validation basics

[ ] Zod
[ ] React Hook Form + Zod
[ ] Validated Signup Form
```

### Next → Step 18: Zod

We'll learn **Zod from the beginning**:

```text
What is Zod?
        ↓
Schema
        ↓
string()
number()
email()
min()
        ↓
Validation
```

Then we'll connect Zod to React Hook Form and finish the **validated signup form**, completing Phase 7.

------------------------------------------------------------------------------------------------------------------------------------------

# Step 18 — Zod

Now we'll learn the **Zod** part of your roadmap.

Our goal is:

```text
React Hook Form + Zod
        ↓
Validated Signup Form
```

---

# 1. What Is Zod?

**Zod is a TypeScript-first validation library.**

Its main job is:

> Define what valid data should look like and check whether the data follows those rules.

For example, we want:

```text
name     → required string
email    → valid email
password → minimum 6 characters
```

Zod lets us describe those rules in one place.

---

# 2. Install Zod

In your project:

```text
npm install zod
```

We will also need the React Hook Form integration package:

```text
npm install @hookform/resolvers
```

So our setup is:

```text
React Hook Form
        +
     Zod
        +
 @hookform/resolvers
```

---

# 3. Create a Zod Schema

Create:

```text
src/schemas/signupSchema.ts
```

Start with:

```tsx
import { z } from "zod";

export const signupSchema = z.object({
    name: z.string(),
    email: z.string().email(),
    password: z.string().min(6)
});
```

This is our **validation schema**.

---

# 4. Understand `z.object()`

We wrote:

```tsx
z.object({
    name: ...,
    email: ...,
    password: ...
})
```

This means:

> The data must be an object containing these fields.

So Zod expects:

```text
{
    name,
    email,
    password
}
```

---

# 5. `z.string()`

We wrote:

```tsx
name: z.string()
```

This means:

> `name` must be a string.

Valid:

```text
"William"
```

Invalid:

```text
123
```

---

# 6. Email Validation

We wrote:

```tsx
email: z.string().email()
```

This means:

```text
email
 ↓
must be a string
 ↓
must have valid email format
```

Valid:

```text
william@example.com
```

Invalid:

```text
hello
```

---

# 7. Password Validation

We wrote:

```tsx
password: z.string().min(6)
```

This means:

> Password must contain at least 6 characters.

Valid:

```text
123456
```

Invalid:

```text
123
```

---

# 8. Our Schema

So the complete schema means:

```text
Signup data
│
├── name
│    └── must be string
│
├── email
│    └── must be valid email
│
└── password
     └── minimum 6 characters
```

This is much easier to understand than scattering validation rules throughout the component.

---

# 9. Add Custom Error Messages

We can make the validation messages more useful:

```tsx
import { z } from "zod";

export const signupSchema = z.object({
    name: z
        .string()
        .min(1, "Name is required"),

    email: z
        .string()
        .email("Enter a valid email"),

    password: z
        .string()
        .min(6, "Password must be at least 6 characters")
});
```

Now Zod knows what message to return when validation fails.

---

# 10. Zod Can Also Generate the TypeScript Type

This is one of the best features.

Instead of manually writing:

```tsx
type SignupFormData = {
    name: string;
    email: string;
    password: string;
};
```

we can derive the type from the schema.

```tsx
export type SignupFormData =
    z.infer<typeof signupSchema>;
```

Now:

```text
Zod Schema
     ↓
z.infer
     ↓
TypeScript Type
```

So our schema becomes the **single source of truth**.

---

# 11. Complete Schema

Our file:

```text
src/schemas/signupSchema.ts
```

contains:

```tsx
import { z } from "zod";

export const signupSchema = z.object({
    name: z
        .string()
        .min(1, "Name is required"),

    email: z
        .string()
        .email("Enter a valid email"),

    password: z
        .string()
        .min(6, "Password must be at least 6 characters")
});

export type SignupFormData =
    z.infer<typeof signupSchema>;
```

---

# 12. Now Connect Zod to React Hook Form

This is where:

```text
@hookform/resolvers
```

comes in.

In `SignupForm.tsx`:

```tsx
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";

import {
    signupSchema,
    SignupFormData
} from "./schemas/signupSchema";
```

Then:

```tsx
const {
    register,
    handleSubmit,
    formState: { errors }
} = useForm<SignupFormData>({
    resolver: zodResolver(signupSchema)
});
```

The important line is:

```tsx
resolver: zodResolver(signupSchema)
```

This connects:

```text
React Hook Form
       ↓
    Resolver
       ↓
      Zod
```

---

# 13. Complete Signup Form

```tsx
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";

import {
    signupSchema,
    SignupFormData
} from "./schemas/signupSchema";

function SignupForm() {
    const {
        register,
        handleSubmit,
        formState: { errors }
    } = useForm<SignupFormData>({
        resolver: zodResolver(signupSchema)
    });

    const onSubmit = (data: SignupFormData) => {
        console.log(data);
    };

    return (
        <form onSubmit={handleSubmit(onSubmit)}>
            <div>
                <input
                    {...register("name")}
                    placeholder="Name"
                />

                {errors.name && (
                    <p>{errors.name.message}</p>
                )}
            </div>

            <div>
                <input
                    {...register("email")}
                    placeholder="Email"
                />

                {errors.email && (
                    <p>{errors.email.message}</p>
                )}
            </div>

            <div>
                <input
                    type="password"
                    {...register("password")}
                    placeholder="Password"
                />

                {errors.password && (
                    <p>{errors.password.message}</p>
                )}
            </div>

            <button type="submit">
                Sign Up
            </button>
        </form>
    );
}

export default SignupForm;
```

---

# 14. Notice Something Important

Look at our inputs.

We no longer have:

```tsx
register("email", {
    required: "Email is required"
})
```

Instead:

```tsx
register("email")
```

Why?

Because the validation is now defined in our Zod schema:

```text
signupSchema.ts
```

So the component focuses on the UI.

The schema focuses on validation.

---

# 15. Full Flow

Now when the user submits:

```text
Name: William
Email: william@example.com
Password: 123456
```

the flow is:

```text
User
 ↓
Submit
 ↓
React Hook Form
 ↓
Zod
 ↓
Validation
 ↓
Valid
 ↓
onSubmit()
```

If the user enters:

```text
Name: William
Email: hello
Password: 123
```

then:

```text
React Hook Form
       ↓
      Zod
       ↓
   Validation
       ↓
    Invalid
       ↓
    errors
```

The user sees:

```text
Enter a valid email

Password must be at least 6 characters
```

---

# 16. The Big Picture

You should now understand the responsibility of each tool:

```text
┌──────────────────────┐
│   React Hook Form    │
│                      │
│ Form management      │
│ Form submission      │
│ Form state           │
└──────────┬───────────┘
           │
           ↓
┌──────────────────────┐
│         Zod          │
│                      │
│ Validation rules     │
│ Schema               │
└──────────────────────┘
```

And TypeScript sits across both:

```text
TypeScript
    │
    ├── React Hook Form types
    │
    └── Zod inferred types
```

---

# 17. Why `z.infer()` Is Useful

Remember our schema:

```tsx
const signupSchema = z.object({
    name: z.string(),
    email: z.string().email(),
    password: z.string().min(6)
});
```

We can derive:

```tsx
type SignupFormData =
    z.infer<typeof signupSchema>;
```

So if later we change the schema:

```text
Add phone
```

the TypeScript type automatically changes too.

We don't need to maintain two separate definitions.

That's why this pattern is very common:

```text
Schema
   ↓
z.infer
   ↓
TypeScript type
```

---

# 18. Phase 7 — COMPLETE

Your roadmap said:

```text
## 7. State Management

- Context API

- Redux Toolkit

- Form state/validation:
  React Hook Form + Zod

Build:
auth state app,
cart system,
validated signup form.
```

We have now covered:

```text
[x] Context API
[x] Redux Toolkit
[x] Redux Counter
[x] Redux Cart System

[x] React Hook Form
[x] React Hook Form + TypeScript
[x] Zod
[x] React Hook Form + Zod
[x] Validation
[x] Validated Signup Form
```

So **Phase 7 is finished**.

---

# Roadmap Status

We started with:

```text
0. Foundations
1. TypeScript
2. Build Tools
3. React Basics
4. Component Thinking
5. React Hooks
6. Routing
7. State Management  ← CURRENTLY COMPLETED
8. Styling
9. API Integration
10. Performance
11. Testing
12. Advanced Patterns
13. Backend
13.5 AI Integration
14. Full Stack Integration
15. Next.js
16. Advanced Topics
17. Projects
```

### Next → Phase 8: Styling

We'll start with:

```text
CSS Modules
    ↓
Styled Components
    ↓
Tailwind
    ↓
Responsive Design
    ↓
Accessibility
```

And, as we've been doing, we'll learn each topic **step by step with a small example**, rather than jumping directly into a large project.


------------------------------------------------------------------------------------------------------------------------------------------