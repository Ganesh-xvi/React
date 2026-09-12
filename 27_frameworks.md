#  Frameworks

## Step 1 — Next.js Fundamentals

Your original roadmap says:

```text
[ ] Next.js
    ├── SSR
    ├── SSG
    ├── Routing
    └── TypeScript integration
```

We'll start with the basics before SSR/SSG.

---

## 1. What is Next.js?

**Next.js is a React framework.**

You already know React:

```text
React
 ↓
Build UI
 ↓
Components
 ↓
Hooks
 ↓
React Router
```

Next.js takes React and adds a lot of application-level features:

```text
Next.js
│
├── React
├── Routing
├── Server rendering
├── Static generation
├── API/backend capabilities
├── Image optimization
├── TypeScript support
└── Production optimizations
```

So:

> **React is a UI library. Next.js is a framework built around React.**

---

# 2. React vs Next.js

With a typical React + Vite application:

```text
Browser
   ↓
React application
   ↓
API
   ↓
Express
```

With Next.js, some work can happen on the server as part of the Next.js application:

```text
Browser
   ↓
Next.js
 ┌─┴─────────────┐
 ↓               ↓
React UI       Server
                 ↓
              Database/API
```

This doesn't mean Express becomes useless in every project. It means Next.js can handle many server-side application responsibilities itself.

---

# 3. Why use Next.js?

A plain React application requires you to choose and configure several things yourself.

For example:

```text
React
+
React Router
+
API solution
+
Rendering strategy
+
Build configuration
+
Various optimizations
```

Next.js provides an integrated framework:

```text
Next.js
  ↓
Many common application features
already integrated
```

This is one reason Next.js is popular for production React applications.

---

# 4. Creating a Next.js App

A common way to create a project is:

```bash
npx create-next-app@latest my-app
```

You'll be asked configuration questions such as:

```text
TypeScript?
ESLint?
Tailwind?
App Router?
```

For your roadmap, you'll use:

```text
TypeScript → Yes
App Router → Yes
```

You already know TypeScript, React and Tailwind, so these won't be completely new concepts.

---

# 5. Next.js Project Structure

A modern Next.js application commonly looks like:

```text
my-app/
│
├── app/
│   ├── page.tsx
│   ├── layout.tsx
│   └── ...
│
├── public/
│
├── package.json
├── tsconfig.json
└── next.config.ts
```

The important new directory is:

```text
app/
```

This is where the **App Router** lives.

---

# 6. `page.tsx`

Consider:

```tsx
export default function Home() {
  return <h1>Hello Next.js</h1>;
}
```

This file:

```text
app/page.tsx
```

represents the application's root page:

```text
/
```

So:

```text
app/page.tsx
      ↓
      /
```

---

# 7. Routing

This is one of the biggest differences you'll notice.

In React Router, you might write routes manually:

```text
<Route path="/about" ... />
<Route path="/products" ... />
```

Next.js uses **file-system based routing**.

For example:

```text
app/
├── page.tsx
├── about/
│   └── page.tsx
└── products/
    └── page.tsx
```

produces:

```text
/           → app/page.tsx
/about      → app/about/page.tsx
/products   → app/products/page.tsx
```

The folder structure determines the URL.

---

# 8. Dynamic Routes

Suppose you want:

```text
/products/123
/products/456
/products/789
```

You can create:

```text
app/
└── products/
    └── [id]/
        └── page.tsx
```

The `[id]` means:

> This part of the URL is dynamic.

So:

```text
/products/123
```

gives:

```text
id = 123
```

And:

```text
/products/456
```

gives:

```text
id = 456
```

This is similar to the dynamic routing you already learned with React Router.

---

# 9. `layout.tsx`

Next.js also has layouts.

For example:

```text
app/
├── layout.tsx
└── page.tsx
```

The layout can contain things shared across pages:

```text
┌─────────────────────────┐
│        Navbar           │
├─────────────────────────┤
│                         │
│       Page Content      │
│                         │
├─────────────────────────┤
│        Footer           │
└─────────────────────────┘
```

Instead of repeating the navbar and footer on every page, the layout wraps the pages.

---

# 10. The Important New Concept: Server Components

This is where Next.js becomes significantly different from the React you've already learned.

In modern Next.js App Router, components are **Server Components by default**.

That means a component can be rendered on the server.

For example:

```tsx
export default function Products() {
  return <h1>Products</h1>;
}
```

By default, this is a Server Component.

---

## Client Components

If a component needs browser-side interactivity such as:

```text
useState
useEffect
onClick
```

you can mark it as a Client Component:

```tsx
"use client";
```

Example:

```tsx
"use client";

import { useState } from "react";

export default function Counter() {
  const [count, setCount] = useState(0);

  return (
    <button onClick={() => setCount(count + 1)}>
      {count}
    </button>
  );
}
```

So remember:

```text
Server Component
→ default

Client Component
→ "use client"
```

---

# 11. Why Server Components?

Imagine a product page needs database information.

With a traditional React frontend:

```text
Browser
 ↓
React
 ↓
API request
 ↓
Backend
 ↓
Database
```

With a Next.js Server Component, server-side code can participate directly in rendering:

```text
Next.js Server
      ↓
   Database
      ↓
   Render page
      ↓
   Browser
```

This can reduce unnecessary client-side JavaScript and allow server-only logic/data access to remain on the server.

---

# 12. Don't Confuse Server Components with SSR

These concepts are related, but **not identical**.

### Server Component

Describes **where a React component executes/rendering logic can happen**.

### SSR

**Server-Side Rendering** describes a rendering strategy where HTML is generated on the server for a request.

We'll study SSR properly next.

For now:

```text
Server Components ≠ SSR
```

They are different concepts.

---

# 13. Next.js + TypeScript

You've already learned TypeScript, so Next.js supports it naturally.

Instead of:

```text
page.jsx
```

you'll commonly use:

```text
page.tsx
```

You can type props normally:

```tsx
type ProductProps = {
  name: string;
  price: number;
};

export default function Product({
  name,
  price,
}: ProductProps) {
  return (
    <div>
      <h2>{name}</h2>
      <p>{price}</p>
    </div>
  );
}
```

So your existing TypeScript knowledge carries directly into Next.js.

---

# 14. The Big Picture

You've gone from:

```text
React + Vite
```

to:

```text
Next.js
│
├── React
├── File-based routing
├── Layouts
├── Server Components
├── Client Components
├── SSR
├── SSG
└── TypeScript
```

The important thing today is understanding **what Next.js adds to React**.

---

## Phase 15 Progress

```text
[x] Next.js fundamentals
[ ] SSR
[ ] SSG
[ ] Routing
[ ] TypeScript integration
```

--------------------------------------------------------------------------------------------------------------------------------------------

**Plain React:**
You go to a webpage. First you see a **blank white screen**. Then, after a second, everything pops up. Why blank? Because your browser has to download and run all the code first before it can show anything.

**Server-Side Rendering (SSR):**
You go to a webpage. The page is **already fully built** on the server before it reaches you. So you see the content **immediately** — no blank screen wait.

That's it. That's the whole idea.

---


## Step 2 — SSR (Server-Side Rendering)

Now let's understand **SSR**, one of the main concepts in Next.js.

---

## 1. What is SSR?

**SSR = Server-Side Rendering.**

It means:

> The server generates the HTML for a page before sending it to the browser.

Let's compare it with the React flow you already know.

### Traditional React SPA

```text
Browser
   ↓
Download JavaScript
   ↓
React runs
   ↓
Fetch data
   ↓
Render UI
```

### SSR

```text
Browser
   ↓
Request page
   ↓
Next.js server
   ↓
Get data
   ↓
Generate HTML
   ↓
Send HTML to browser
```

So the browser can receive useful HTML **from the beginning**.

---

# 2. Simple Example

Imagine a product page:

```text
/products/123
```

The server knows:

```text
Product:
iPhone
Price:
$999
```

With SSR:

```text
User requests /products/123
          ↓
       Next.js
          ↓
      Get product
          ↓
   Generate HTML
          ↓
Browser receives:
"IPhone - $999"
```

The browser doesn't have to start with an empty page and wait for JavaScript to fetch the product before displaying it.

---

# 3. CSR vs SSR

### CSR — Client-Side Rendering

```text
Browser
   ↓
React JavaScript
   ↓
Fetch API
   ↓
Get data
   ↓
Render
```

### SSR — Server-Side Rendering

```text
Browser
   ↓
Next.js server
   ↓
Fetch data
   ↓
Generate HTML
   ↓
Browser
```

The key difference is **where the initial rendering happens**.

---

# 4. Why SSR?

There are several benefits.

### Faster initial content

The browser can receive already-rendered HTML.

### SEO

Search engines can more easily receive meaningful page content.

This is particularly useful for:

```text
Blogs
Product pages
News sites
Marketing pages
Documentation
```

### Server-side data access

The server can access server-only resources without exposing credentials to the browser.

For example:

```text
Next.js Server
      ↓
Database
      ↓
Product data
      ↓
HTML
      ↓
Browser
```

---

# 5. SSR Does NOT Mean "No JavaScript"

This is important.

A page can be rendered on the server **and still contain interactive React components**.

For example:

```text
Server
 ↓
Render product page
 ↓
Send HTML
 ↓
Browser
 ↓
React makes interactive parts functional
```

This process is commonly associated with **hydration**.

---

# 6. What is Hydration?

Suppose the server sends:

```html
<button>Buy</button>
```

The browser can display that button immediately.

But the button needs JavaScript for:

```text
click
 ↓
add product to cart
```

React connects its client-side behavior to the server-rendered HTML.

That's called **hydration**.

Conceptually:

```text
Server-rendered HTML
        +
React JavaScript
        ↓
Hydrated interactive UI
```

---

# 7. SSR Request Flow

A simplified SSR request looks like:

```text
User
 │
 │ GET /products/123
 ↓
Next.js Server
 │
 ├── Execute server code
 │
 ├── Fetch product
 │
 └── Render React
 │
 ↓
HTML Response
 │
 ↓
Browser
 │
 ↓
Hydration
 │
 ↓
Interactive page
```

---

# 8. SSR and Server Components

You may remember from the previous lesson:

> Server Components are the default in the Next.js App Router.

Don't treat these as the same thing.

```text
Server Component
    ↓
Where component code executes

SSR
    ↓
How a page is rendered for a request
```

They're related, but they're different concepts.

---

# 9. When SSR Is Useful

Imagine an e-commerce website.

A user visits:

```text
/products/laptop
```

You want the initial page to contain:

```text
Laptop
$1200
Description
Reviews
```

SSR can generate that page on the server.

Then interactive components such as:

```text
Add to cart
Quantity selector
Wishlist button
```

can be Client Components.

So you can combine:

```text
Server-rendered content
        +
Client-side interaction
```

---

# 10. SSR vs CSR — When to Use Which?

| CSR                    | SSR            |
| ---------------------- | -------------- |
| Dashboard              | Product page   |
| Admin panel            | Blog           |
| Highly interactive app | News page      |
| Private application    | Public content |
| SEO less important     | SEO important  |

This isn't an absolute rule. Modern Next.js applications often combine different rendering approaches on different parts of an application.

---

# 11. The Big Picture

You started with:

```text
React SPA

Browser
 ↓
React
 ↓
API
```

Now Next.js gives you another possibility:

```text
Next.js

Browser
 ↓
Next.js Server
 ↓
Data
 ↓
HTML
 ↓
Browser
 ↓
Hydration
```

And eventually we'll add **SSG**:

```text
SSR
→ render when the request happens

SSG
→ generate pages ahead of time
```

That distinction is the next major concept.

---

## Phase 15 Progress

```text
[x] Next.js fundamentals
[x] SSR
[ ] SSG
[ ] Routing
[ ] TypeScript integration
```

--------------------------------------------------------------------------------------------------------------------------------------------


## Step 3 — SSG (Static Site Generation)

Now we move to **SSG**, the third rendering approach you need to understand.

---

## 1. What is SSG?

**SSG = Static Site Generation.**

It means:

> Next.js generates the HTML for a page ahead of time, rather than generating it for every user request.

Think of it like preparing the page **before anyone asks for it**.

```text id="3v8k1d"
Build time
   ↓
Next.js generates HTML
   ↓
Static HTML stored
   ↓
User requests page
   ↓
HTML is served
```

---

# 2. SSR vs SSG

This is the important difference.

### SSR

The page is generated when the request happens:

```text id="x3d5qv"
User requests page
       ↓
Next.js server
       ↓
Get data
       ↓
Generate HTML
       ↓
Send page
```

### SSG

The page is generated beforehand:

```text id="tq5c7a"
Build application
       ↓
Generate HTML
       ↓
Store static page
       ↓
User requests page
       ↓
Serve existing HTML
```

So:

**SSR → generate at request time**

**SSG → generate at build time**

---

# 3. Simple Example

Imagine you have a blog.

You have:

```text id="w3k1vb"
/blog/react-basics
/blog/javascript-basics
/blog/typescript-basics
```

The articles don't change every second.

So during the build:

```text id="2v2q1q"
Next.js build
     ↓
Generate blog pages
     ↓
Static HTML
```

When users visit:

```text id="z0n6ha"
/blog/react-basics
```

the server can simply serve the already-generated page.

---

# 4. Why SSG Is Fast

Because the expensive rendering work can happen **before the user arrives**.

```text id="1p8s1k"
SSG:

Build time
   ↓
HTML already created
   ↓
User request
   ↓
Serve HTML quickly
```

This makes SSG particularly useful for content that doesn't change frequently.

Examples:

```text id="j4x7r9"
Documentation
Blogs
Marketing pages
Portfolio
About page
Landing pages
```

---

# 5. SSG vs SSR vs CSR

Now let's put the three together.

| Rendering | When is UI generated?        |
| --------- | ---------------------------- |
| CSR       | In the browser               |
| SSR       | On the server during request |
| SSG       | During build                 |

### CSR

```text id="3a2q9x"
Request
 ↓
HTML/JS
 ↓
Browser renders
```

### SSR

```text id="r2g4v8"
Request
 ↓
Server renders
 ↓
HTML
 ↓
Browser
```

### SSG

```text id="w7m1kf"
Build
 ↓
Generate HTML
 ↓
User request
 ↓
Serve HTML
```

---

# 6. Real-World Example

Suppose your website has:

### Home page

```text id="q4qz9k"
/
```

Content changes rarely.

**SSG** is a good choice.

### Blog article

```text id="4s9t6v"
/blog/my-article
```

Content changes occasionally.

**SSG** can be a good choice.

### User dashboard

```text id="6u0n2d"
/dashboard
```

Content depends on the logged-in user.

For example:

```text id="qv4p5m"
William's todos
```

changes depending on the user.

You'd generally use a dynamic/server approach rather than treating it as one shared static page.

---

# 7. What About Data Changes?

This is where things become more interesting.

Suppose you generate:

```text id="z9m2xp"
Product: Laptop
Price: $1000
```

Then the price changes to:

```text id="3h4n6b"
$900
```

A purely static page won't magically know about the new price.

That's why modern Next.js supports different caching and revalidation strategies.

One important concept is:

**ISR — Incremental Static Regeneration**

---

# 8. ISR

ISR allows a statically generated page to be updated periodically without rebuilding the entire application.

Conceptually:

```text id="b8p2vx"
Generate static page
        ↓
Serve page
        ↓
After configured period
        ↓
Regenerate page
        ↓
New version
```

So you can get some benefits of SSG while still allowing content to become fresh.

---

# 9. SSG Doesn't Mean "Never Changes"

This is a common misunderstanding.

Think:

```text id="8e4g3m"
SSG
 ↓
Generated ahead of request
```

not:

```text id="k3j9s1"
SSG
 ↓
Can never update
```

With modern Next.js features such as revalidation, static content can be refreshed.

---

# 10. How This Fits Next.js

Next.js lets you choose rendering strategies based on the page.

For example:

```text id="u7s4ka"
Next.js Application
│
├── Home
│     └── Static
│
├── Blog
│     └── Static / Revalidated
│
├── Product
│     └── Dynamic
│
└── Dashboard
      └── User-specific
```

You don't have to make your entire application exclusively SSR or exclusively SSG.

---

# 11. The Key Idea

Remember these three sentences:

> **CSR:** Browser generates the UI.

> **SSR:** Server generates the UI when the request happens.

> **SSG:** Next.js generates the UI ahead of time.

And:

> **ISR:** Static pages can be regenerated/updated periodically.

---

## Phase 15 Progress

```text id="m3r8x2"
[x] Next.js fundamentals
[x] SSR
[x] SSG
[ ] Routing
[ ] TypeScript integration
```

--------------------------------------------------------------------------------------------------------------------------------------------


## Step 4 — Next.js Routing

You already learned routing with **React Router**. Next.js does routing differently: it uses the **file system**.

---

## 1. File-Based Routing

In React Router, you define routes in code:

```text
<Route path="/about" ... />
<Route path="/products" ... />
```

In Next.js, the folder structure defines the routes.

```text
app/
├── page.tsx
├── about/
│   └── page.tsx
└── products/
    └── page.tsx
```

This creates:

```text
/            → app/page.tsx
/about       → app/about/page.tsx
/products    → app/products/page.tsx
```

So the basic rule is:

> **Folder = URL segment, `page.tsx` = page.**

---

# 2. Nested Routes

Suppose you have:

```text
app/
└── dashboard/
    ├── page.tsx
    └── settings/
        └── page.tsx
```

You get:

```text
/dashboard
/dashboard/settings
```

The folder hierarchy becomes the URL hierarchy.

```text
dashboard/
    ↓
/dashboard

settings/
    ↓
/dashboard/settings
```

---

# 3. Dynamic Routes

Suppose you have products:

```text
/products/1
/products/2
/products/3
```

You don't want to create:

```text
1/page.tsx
2/page.tsx
3/page.tsx
```

Instead:

```text
app/
└── products/
    └── [id]/
        └── page.tsx
```

`[id]` means:

> This URL segment is dynamic.

Therefore:

```text
/products/123
```

gives:

```text
id = 123
```

and:

```text
/products/456
```

gives:

```text
id = 456
```

---

# 4. Example Dynamic Page

A simplified example:

```tsx
type Props = {
  params: {
    id: string;
  };
};

export default function ProductPage({ params }: Props) {
  return <h1>Product {params.id}</h1>;
}
```

Visiting:

```text
/products/123
```

could display:

```text
Product 123
```

The important idea is the `[id]` folder.

---

# 5. Catch-All Routes

Next.js can also capture multiple URL segments.

```text
app/
└── docs/
    └── [...slug]/
        └── page.tsx
```

This can match things such as:

```text
/docs/react
/docs/react/hooks
/docs/react/hooks/use-state
```

The `...` means:

> Capture multiple segments.

You don't need this often at the beginning, but it's useful for documentation systems and similar applications.

---

# 6. Layouts

This is another important Next.js routing feature.

Suppose:

```text
app/
├── layout.tsx
└── page.tsx
```

`layout.tsx` can provide shared UI:

```text
┌─────────────────────────────┐
│           Navbar            │
├─────────────────────────────┤
│                             │
│         Page content        │
│                             │
├─────────────────────────────┤
│           Footer            │
└─────────────────────────────┘
```

The layout can remain while different pages change.

---

# 7. Nested Layouts

You can also have layouts inside route segments.

```text
app/
├── layout.tsx
├── page.tsx
└── dashboard/
    ├── layout.tsx
    ├── page.tsx
    └── settings/
        └── page.tsx
```

Now `/dashboard` can have its own dashboard layout.

Conceptually:

```text
Root Layout
     │
     ├── Home
     │
     └── Dashboard Layout
            │
            ├── Dashboard
            └── Settings
```

This is very useful for applications with sections such as:

```text
/admin
/dashboard
/account
```

---

# 8. Navigation

Next.js provides the `Link` component.

```tsx
import Link from "next/link";

export default function Home() {
  return (
    <div>
      <Link href="/about">About</Link>
      <Link href="/products">Products</Link>
    </div>
  );
}
```

Clicking these links navigates between pages.

Conceptually:

```text
Home
 │
 ├── About
 └── Products
```

You don't need to use traditional `<a href>` for normal internal Next.js navigation.

---

# 9. Next.js Routing vs React Router

| React Router             | Next.js                         |
| ------------------------ | ------------------------------- |
| Routes defined in code   | Routes defined by folders/files |
| `<Route>`                | `page.tsx`                      |
| Dynamic route parameters | `[id]`                          |
| Nested routes            | Nested folders                  |
| Layout routes            | `layout.tsx`                    |
| `<Link>`                 | `next/link`                     |

The concepts are similar; the implementation is different.

---

# 10. A Real Example

Imagine we're building your Todo application.

We could have:

```text
app/
├── page.tsx
├── login/
│   └── page.tsx
├── todos/
│   ├── page.tsx
│   └── [id]/
│       └── page.tsx
└── dashboard/
    ├── layout.tsx
    ├── page.tsx
    └── settings/
        └── page.tsx
```

That produces:

```text
/                       → Home
/login                  → Login
/todos                  → Todo list
/todos/123              → Todo #123
/dashboard              → Dashboard
/dashboard/settings     → Settings
```

This should look familiar because you've already learned dynamic and nested routing with React Router.

---

# 11. The Main Thing to Remember

Next.js routing is largely based on this pattern:

```text
app/
│
├── page.tsx
│      ↓
│      /
│
├── about/
│   └── page.tsx
│          ↓
│        /about
│
└── products/
    └── [id]/
        └── page.tsx
               ↓
          /products/:id
```

So instead of manually describing every route, **your file structure describes your routes**.

---

## Phase 15 Progress

```text
[x] Next.js fundamentals
[x] SSR
[x] SSG
[x] Routing
[ ] API routes 
[ ] TypeScript integration
```

--------------------------------------------------------------------------------------------------------------------------------------------

## Step 5 — TypeScript Integration

This is the **last item in Phase 15** from your roadmap.

The good news is that you already know TypeScript, so we're mainly learning **how TypeScript fits into Next.js**.

---

## 1. Next.js Supports TypeScript Directly

A Next.js project commonly uses:

```text
app/
├── page.tsx
├── layout.tsx
└── products/
    └── [id]/
        └── page.tsx
```

Notice:

```text
.tsx
```

instead of:

```text
.jsx
```

That means we can use TypeScript directly inside our React components.

---

# 2. Typing Component Props

Same concept you learned in React + TypeScript.

```tsx
type UserProps = {
  name: string;
  age: number;
};

export default function User({ name, age }: UserProps) {
  return (
    <div>
      {name} - {age}
    </div>
  );
}
```

Nothing special here.

Your existing TypeScript knowledge carries over.

---

# 3. Typing Dynamic Route Parameters

Remember:

```text
app/
└── products/
    └── [id]/
        └── page.tsx
```

The `id` comes from the URL.

We can type it:

```tsx
type Props = {
  params: {
    id: string;
  };
};
```

Then:

```tsx
export default function ProductPage({ params }: Props) {
  return <h1>Product {params.id}</h1>;
}
```

For:

```text
/products/123
```

we get:

```text
params.id
   ↓
"123"
```

Notice it's a **string**, because URL parameters are strings.

---

# 4. API Data

Suppose your Next.js application receives:

```json
{
  "id": 1,
  "title": "Learn Next.js",
  "completed": false
}
```

You can create a type:

```tsx
type Todo = {
  id: number;
  title: string;
  completed: boolean;
};
```

Then TypeScript knows what the data should look like.

```tsx
const todo: Todo = {
  id: 1,
  title: "Learn Next.js",
  completed: false,
};
```

This is the same TypeScript principle you've already learned.

---

# 5. Server Components + TypeScript

Server Components can also be written with TypeScript.

For example:

```tsx
type Product = {
  id: number;
  name: string;
  price: number;
};

export default async function Products() {
  const products: Product[] = await getProducts();

  return (
    <div>
      {products.map((product) => (
        <p key={product.id}>
          {product.name} - {product.price}
        </p>
      ))}
    </div>
  );
}
```

The important combination is:

```text
Next.js Server Component
        +
TypeScript
        ↓
Type-safe server-side code
```

---

# 6. Client Components + TypeScript

Client Components work the same way.

```tsx
"use client";

import { useState } from "react";

export default function Counter() {
  const [count, setCount] = useState<number>(0);

  return (
    <button onClick={() => setCount(count + 1)}>
      {count}
    </button>
  );
}
```

You already know:

```text
useState
props
events
TypeScript
```

Next.js doesn't change those fundamentals.

---

# 7. TypeScript Configuration

Next.js creates a:

```text
tsconfig.json
```

for TypeScript configuration.

For example:

```text
tsconfig.json
```

controls things such as:

```text
TypeScript compiler behavior
path aliases
strictness
included files
```

You learned `tsconfig` during your TypeScript phase, so this should be familiar.

---

# 8. API Routes and TypeScript

Next.js can also contain server-side API endpoints.

With the App Router, you can create:

```text
app/
└── api/
    └── todos/
        └── route.ts
```

Notice:

```text
route.ts
```

This can handle HTTP requests.

Conceptually:

```text
GET /api/todos
       ↓
route.ts
       ↓
Database
       ↓
JSON response
```

And TypeScript can type the data used by the endpoint.

---

# 9. Frontend + Backend Types

Suppose your backend returns:

```json
{
  "id": 1,
  "title": "Learn Next.js"
}
```

You can define:

```tsx
type Todo = {
  id: number;
  title: string;
};
```

Then your frontend can work with:

```tsx
const todo: Todo = data;
```

This helps catch mistakes such as:

```tsx
todo.titel
```

instead of:

```tsx
todo.title
```

TypeScript can catch that error during development.

---

# 10. The Full Picture

You've now seen how your previous knowledge fits into Next.js:

```text
                    Next.js
                       │
        ┌──────────────┼──────────────┐
        ↓              ↓              ↓
      React        TypeScript       Server
        │              │              │
   Components       Types          Server logic
   Hooks            Props          Data access
   Events           Params         API routes
```

So Next.js isn't replacing React or TypeScript.

It's combining them into a larger framework.

---

# 11. What You Should Remember

The important points from the entire TypeScript integration lesson:

```text
.tsx
→ React + TypeScript

params
→ can be typed

props
→ can be typed

API data
→ can be typed

Server Components
→ can use TypeScript

Client Components
→ can use TypeScript

route.ts
→ server/API endpoint can use TypeScript

tsconfig.json
→ TypeScript configuration
```

---

# Phase 15 Complete

Your roadmap is now:

```text
[x] Next.js fundamentals
[x] SSR
[x] SSG
[x] Routing
[x] TypeScript integration
```

**Phase 15 — Frameworks: COMPLETE**

--------------------------------------------------------------------------------------------------------------------------------------------