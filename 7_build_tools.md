# Build Tools

Your roadmap says:

* Vite
* npm / yarn / pnpm basics
* Environment variables
* Bundling basics

We'll start with **Vite**.

# Step 1 — What is Vite?

You already have a basic HTML/CSS/JS project:

```text
Todo App
├── index.html
├── style.css
└── script.js
```

When we move to React + TypeScript, our project becomes more complicated.

We need a tool that helps us:

* run the project locally
* process TypeScript
* process JSX/TSX
* bundle files
* provide a development server
* rebuild quickly when we make changes

**Vite** is that tool.

Think of it as:

```text
Your React + TypeScript project
              ↓
             Vite
              ↓
       Development server
              ↓
        Browser displays app
```

### Why are we learning Vite?

Your roadmap specifically says:

> **Vite (preferred over CRA)**

So when we start React, we'll normally create our React project using Vite.

For example, later we'll have a project like:

```text
React + TypeScript
        ↓
       Vite
        ↓
      Browser
```

### Important distinction

Vite is **not React**.

```text
React → UI library

TypeScript → Programming language/type system

Vite → Build/development tool
```

They work together:

```text
React
  +
TypeScript
  +
Vite
  ↓
React application
```

That's the basic idea.

Next we'll learn **npm**, because you'll see npm commands when working with Vite.

------------------------------------------------------------------------------------------------------------------------------------------

# Step 2 — What is npm?

**npm = Node Package Manager**

It is used to:

* install packages/libraries
* manage project dependencies
* run project scripts

Think of it like a **package manager for JavaScript projects**.

```text
Your project
    ↓
npm
    ↓
Packages / libraries
```

### Example

Suppose your project needs React.

Instead of manually downloading React files, npm can install it for your project.

Your project then keeps track of the packages it uses.

---

## `package.json`

When you have a JavaScript/TypeScript project, you'll usually have:

```text
package.json
```

This file contains information about your project and its dependencies.

For example, conceptually:

```text
package.json
│
├── project information
├── dependencies
├── dev dependencies
└── scripts
```

### Dependencies

These are packages your application needs.

For example:

```text
React
TypeScript
```

### Scripts

Scripts are shortcuts for commands used to work with your project.

For example, a Vite project commonly has scripts for:

```text
development
build
preview
```

You don't need to memorize the commands yet.

---

# npm vs Vite

This distinction is important.

```text
npm
 ↓
Manages packages and project scripts

Vite
 ↓
Runs/builds your application
```

They work together.

For example:

```text
npm
 ↓
installs Vite
 ↓
Vite
 ↓
runs your React application
```

---

## What about yarn and pnpm?

Your roadmap mentions:

* npm
* yarn
* pnpm

They are all **package managers**.

You don't need to learn all three deeply right now.

We'll primarily use **npm** while learning.

So remember:

> **npm manages the packages and scripts in your JavaScript/TypeScript project.**

Next we'll look at **how a Vite project is structured**, so you can understand what each file is doing.

------------------------------------------------------------------------------------------------------------------------------------------

# Step 3 — Vite Project Structure

Now let's understand what a Vite project looks like.

When we create a React + TypeScript project with Vite, you'll see something similar to:

```text
my-app/
│
├── src/
│   ├── App.tsx
│   ├── main.tsx
│   └── ...
│
├── public/
│
├── index.html
├── package.json
├── tsconfig.json
└── vite.config.ts
```

Don't worry about every file yet. We'll go one by one.

---

## 1. `src/`

This is where your **application source code** normally lives.

Think:

```text
src/
 ↓
Your actual application code
```

Later you'll have:

```text
src/
├── App.tsx
├── main.tsx
├── components/
├── pages/
└── ...
```

---

## 2. `App.tsx`

This is usually where your main React component starts.

The `.tsx` extension means:

```text
TS + JSX
```

So:

```text
.ts  → TypeScript

.tsx → TypeScript + JSX
```

We'll learn JSX when we start React.

---

## 3. `main.tsx`

This is the file that starts the React application and connects React to your HTML page.

Think:

```text
main.tsx
   ↓
starts React
   ↓
loads App
   ↓
browser
```

We'll understand this properly when we reach React.

---

## 4. `index.html`

This is still HTML.

Even though we're using React, there is still an HTML page.

React eventually gets attached to an element in this page.

---

## 5. `package.json`

We just discussed this.

It contains things like:

```text
project information
dependencies
scripts
```

---

## 6. `tsconfig.json`

We already learned this.

It contains **TypeScript configuration**.

---

## 7. `vite.config.ts`

This contains **Vite configuration**.

Think:

```text
tsconfig.json
    ↓
TypeScript settings

vite.config.ts
    ↓
Vite settings
```

You usually won't need to modify it much when starting out.

---

# The big picture

When we eventually build a React application:

```text
                 Vite
                  ↓
        ┌─────────┴─────────┐
        ↓                   ↓
   TypeScript              React
        ↓                   ↓
        └─────────┬─────────┘
                  ↓
              Browser
```

And the important folders/files have different responsibilities.

| File/Folder      | Purpose                 |
| ---------------- | ----------------------- |
| `src/`           | Application source code |
| `App.tsx`        | Main React component    |
| `main.tsx`       | Starts React            |
| `index.html`     | HTML entry page         |
| `package.json`   | Packages + scripts      |
| `tsconfig.json`  | TypeScript settings     |
| `vite.config.ts` | Vite settings           |

You don't need to memorize this now. We'll naturally use these files when we create the project.

Next: **Environment variables in Vite** — `.env` and `import.meta.env`.

------------------------------------------------------------------------------------------------------------------------------------------

# Step 4 — Environment Variables in Vite

Now we'll learn **environment variables**.

This is useful because applications often need values that we don't want to hard-code directly into our code.

For example, your API URL.

Instead of:

```text
https://api.example.com
```

directly inside your code, we can keep it in an environment file.

---

## 1. What is `.env`?

A `.env` file is a place where we can store configuration values.

For example:

```text
.env
```

could contain something like:

```text
VITE_API_URL=https://api.example.com
```

Think:

```text
.env
 ↓
configuration values
 ↓
application
```

---

## 2. Why does Vite use `VITE_`?

With Vite, frontend environment variables that should be available to your application normally need the `VITE_` prefix.

For example:

```text
VITE_API_URL=...
```

Then your application can access it through:

```text
import.meta.env.VITE_API_URL
```

So:

```text
.env
   ↓
VITE_API_URL
   ↓
import.meta.env.VITE_API_URL
```

---

## 3. Example with our Todo app

Instead of writing the API URL directly:

```text
https://jsonplaceholder.typicode.com
```

we could have:

```text
VITE_API_URL=https://jsonplaceholder.typicode.com
```

Then our application gets the value from:

```text
import.meta.env.VITE_API_URL
```

This makes it easier to change the API URL between environments.

For example:

```text
Development
     ↓
development API

Production
     ↓
production API
```

without changing the application code itself.

---

## Important security point

This is **very important** with frontend applications.

A `.env` variable beginning with `VITE_` is exposed to the frontend bundle.

So **do not put secrets there**.

For example, don't put:

```text
database password
private API key
secret token
```

in a frontend `VITE_` variable.

Think:

```text
VITE_...
   ↓
Frontend
   ↓
User can potentially see it
```

So environment variables in a frontend are mainly useful for **configuration**, not for hiding secrets.

---

### Simple definition

> **`.env` stores configuration values, and Vite exposes `VITE_` variables through `import.meta.env`.**

Next is the last Build Tools concept:

**Bundling — what it means and why Vite does it.**


------------------------------------------------------------------------------------------------------------------------------------------

# Step 5 — What is Bundling?

This is the **last concept in your Build Tools section**.

The word "bundling" sounds complicated, but the basic idea is simple.

## Imagine your application has many files

As your React project grows, you may have:

```text
src/
├── main.tsx
├── App.tsx
├── components/
│   ├── Header.tsx
│   ├── Todo.tsx
│   └── Button.tsx
├── utils/
│   └── api.ts
└── styles/
    └── app.css
```

Your application depends on all these files.

The browser ultimately needs a version of your application that can be efficiently delivered and run.

---

## What does a bundler do?

A build tool such as Vite processes your project and prepares it for the browser.

Conceptually:

```text
Your source files
       ↓
      Vite
       ↓
process / transform
       ↓
production files
       ↓
Browser
```

This process is commonly called **bundling/building**.

---

## Why is this needed?

Your code might contain:

```text
TypeScript
JSX
imports
multiple files
CSS
images
```

The browser doesn't simply receive your development project exactly as you wrote it.

The build process prepares everything for deployment.

---

## A simple example

Suppose:

```text
App.tsx
   ↓
imports
   ↓
Header.tsx
   ↓
imports
   ↓
Button.tsx
```

The build tool follows these relationships:

```text
App
 ↓
Header
 ↓
Button
```

and processes the application as a whole.

---

# Development vs Production

This distinction is important.

### During development

You are writing:

```text
React
TypeScript
TSX
CSS
```

Vite gives you a development environment so you can work on the application and see changes quickly.

### When deploying

You run a production build.

Conceptually:

```text
Development code
       ↓
     Vite
       ↓
Production build
       ↓
Deploy
```

The production version is prepared to be served efficiently to users.

---

## Don't worry about the deeper details yet

There are more advanced concepts such as:

* minification
* tree shaking
* code splitting
* chunks
* lazy loading

You'll encounter some of these later in your roadmap under **Performance Optimization** and **Advanced Topics**.

For now, remember:

> **Bundling/building means taking your application's source code and preparing it for the browser and production.**

---

# Build Tools — DONE

You've now covered:

* Vite
* npm basics
* Project structure
* Environment variables
* `import.meta.env`
* Bundling basics

------------------------------------------------------------------------------------------------------------------------------------------
