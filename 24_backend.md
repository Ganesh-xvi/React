# Step 1: What is Node.js?

Let's start from the beginning.

---

# 1. The Problem Before Node.js

You already know JavaScript was mainly used in the browser.

```text
Browser
   ↓
JavaScript
   ↓
Interactive Web Pages
```

For example:

```js
button.addEventListener("click", () => {
    console.log("Clicked");
});
```

JavaScript traditionally runs inside:

```text
Chrome
Firefox
Edge
Safari
```

But then a question came:

> Can JavaScript run outside the browser?

The answer is **Node.js**.

---

# 2. What Is Node.js?

> **Node.js is a runtime environment that allows JavaScript to run outside the browser.**

Simple diagram:

```text
Before:

JavaScript
    ↓
Browser only


With Node.js:

JavaScript
    ↓
Browser

JavaScript
    ↓
Server (Node.js)
```

---

# 3. What Does "Runtime Environment" Mean?

A runtime environment is simply an environment where code can execute.

For example:

```text
Python
↓
Python Runtime

Java
↓
JVM

JavaScript in Browser
↓
Browser Runtime

JavaScript on Server
↓
Node.js Runtime
```

Node.js provides an environment to execute JavaScript on your computer or server.

---

# 4. Example

Create a file:

```text
app.js
```

Inside:

```js
console.log("Hello from Node.js");
```

Run it:

```text
node app.js
```

Output:

```text
Hello from Node.js
```

No browser is involved.

That's Node.js.

---

# 5. Why Do We Need Node.js?

Because frontend applications need a backend.

Your React application currently looks something like:

```text
React
  ↓
Browser
  ↓
User Interface
```

But where does the data come from?

```text
React
   ↓
Backend API
   ↓
Database
```

Node.js can build that backend.

---

# 6. Full Stack Architecture

This is where you're heading:

```text
React Frontend
      ↓ HTTP Request
Node.js + Express Backend
      ↓
Database
```

Example:

```text
User clicks "Add Todo"

React
  ↓
POST /todos
  ↓
Node.js + Express
  ↓
Database
  ↓
Todo saved
  ↓
Response
  ↓
React updates UI
```

---

# 7. What Can Node.js Do?

Node.js can:

```text
✓ Create APIs
✓ Connect to databases
✓ Handle authentication
✓ Read/write files
✓ Handle file uploads
✓ Send emails
✓ Build real-time applications
✓ Connect to external APIs
```

For your roadmap, we mainly focus on:

```text
Node.js
   ↓
Express.js
   ↓
REST API
   ↓
Database
   ↓
Authentication
```

---

# 8. Node.js Is NOT a Programming Language

This is important.

```text
JavaScript = Programming Language

Node.js = Runtime Environment
```

Think:

```text
JavaScript
    ↓
Code you write

Node.js
    ↓
Runs that JavaScript outside the browser
```

---

# 9. Browser JavaScript vs Node.js

| Browser JavaScript  | Node.js                                 |
| ------------------- | --------------------------------------- |
| Runs in browser     | Runs on server/computer                 |
| Has DOM             | No DOM by default                       |
| Can manipulate HTML | Cannot directly manipulate browser HTML |
| `window` available  | `window` usually unavailable            |
| Handles UI          | Handles backend logic                   |

Example:

Browser:

```js
document.querySelector("button");
```

Node.js:

```js
// document does not exist
```

Instead, Node.js can do backend work:

```text
Read files
Connect database
Create server
Handle requests
```

---

# 10. Where Does Express Come In?

Node.js alone can create a server.

But writing everything using raw Node.js can be more difficult.

So we use:

> **Express.js**

```text
Node.js
   ↓
Express.js
   ↓
Backend API
```

Think of Express as a framework built on top of Node.js.

Similar idea:

```text
JavaScript
   ↓
React

Node.js
   ↓
Express
```

Not exactly the same thing, but useful for understanding.

---

# 11. Real Full Stack Flow

By the end of this backend phase, you'll understand:

```text
React Application
       ↓
       ↓ API Request
       ↓
Express Server
       ↓
Middleware
       ↓
Routes
       ↓
Controller / Logic
       ↓
Database
       ↓
Response
       ↓
React Application
```

This is the full-stack world you're entering now.

---

# The Most Important Things to Remember

```text
Node.js ≠ JavaScript
```

Instead:

```text
JavaScript = Language

Node.js = Environment that runs JavaScript outside the browser
```

And:

```text
React
   ↓
Frontend

Node.js + Express
   ↓
Backend
```

---

# Phase 13 Progress

```text
Backend — Node.js + Express

[x] Step 1 — What is Node.js?
[ ] Step 2 — Installing / Understanding Node.js Environment
[ ] Step 3 — npm and package.json
[ ] Step 4 — Node Modules
[ ] Step 5 — Built-in Modules
[ ] Step 6 — Creating a Basic Server
```
-----------------------------------------------------------------------------------------------------------------

# Step 2: Node.js Environment — How Node Runs JavaScript

Now you know:

> Node.js allows JavaScript to run outside the browser.

But how does that actually work?

---

# 1. You Write a JavaScript File

For example:

```text
app.js
```

Inside:

```js
console.log("Hello");
```

This is just normal JavaScript.

---

# 2. Node.js Executes the File

When you run:

```text
node app.js
```

The flow is:

```text
app.js
   ↓
Node.js Runtime
   ↓
JavaScript Engine
   ↓
Code Executes
```

---

# 3. Node.js Uses the V8 Engine

Node.js uses Google's **V8 JavaScript engine**.

V8 is also used by Chrome.

```text
JavaScript Code
      ↓
V8 Engine
      ↓
Machine Code
      ↓
Computer Executes It
```

You don't need to deeply understand V8 right now.

Just remember:

> **Node.js uses the V8 engine to execute JavaScript.**

---

# 4. Browser vs Node Environment

This is important because both run JavaScript, but they provide different environments.

### Browser:

```text
JavaScript
    +
Browser APIs
```

Examples:

```js
document
window
localStorage
```

### Node.js:

```text
JavaScript
    +
Node APIs
```

Examples:

```text
fs
http
path
process
```

---

# 5. Example Difference

This works in a browser:

```js
document.querySelector("h1");
```

But in Node.js:

```text
ReferenceError:
document is not defined
```

Why?

Because Node.js has no browser DOM.

---

But Node.js provides things that browsers don't.

For example:

```js
console.log(process.platform);
```

Node can access information about the system.

---

# 6. Node.js Environment Mental Model

Think of JavaScript as the language.

The environment decides what extra tools are available.

```text
JavaScript Language
        │
        ├── Browser Environment
        │      ├── DOM
        │      ├── window
        │      └── localStorage
        │
        └── Node.js Environment
               ├── fs
               ├── http
               ├── path
               └── process
```

Same JavaScript.

Different environment.

---

# 7. Installing Node.js

When Node.js is installed, you usually get:

```text
node
npm
```

### `node`

Runs JavaScript files.

```text
node app.js
```

### `npm`

Manages packages.

We'll learn this properly in the next step.

---

# 8. Checking Node Installation

You can check the Node version:

```text
node --version
```

Or:

```text
node -v
```

Example output:

```text
v22.x.x
```

Check npm:

```text
npm -v
```

---

# 9. Node REPL

Node also has something called REPL.

REPL means:

```text
Read
Evaluate
Print
Loop
```

If you type:

```text
node
```

You enter an interactive JavaScript environment.

Example:

```text
> 2 + 2
4
```

Or:

```text
> const name = "John"
> name
'John'
```

This is useful for quickly testing JavaScript.

But in real projects, you'll mostly work with files.

---

# 10. Running a Node Project

A basic Node project could look like:

```text
my-backend/
│
├── app.js
└── package.json
```

You write your backend code inside:

```text
app.js
```

Then run it with Node.

Eventually:

```text
node app.js
```

Your application starts.

Later, with Express:

```text
my-backend/
│
├── src/
│   ├── server.js
│   ├── routes/
│   └── controllers/
│
├── package.json
└── .env
```

But don't worry about this structure yet.

We'll build toward it step by step.

---

# 11. Very Important Concept: Node Is Not the Server

Beginners sometimes think:

> "Node.js itself is a server."

Not exactly.

Node.js is the runtime environment.

You can use Node.js to **create a server**.

```text
Node.js
   ↓
Allows JavaScript to run
   ↓
You write server code
   ↓
Server handles requests
```

Later:

```text
Node.js
   +
Express
   ↓
Web Server / API
```

---

# Summary

```text
JavaScript
   ↓
Node.js Runtime
   ↓
V8 Engine
   ↓
Code Executes
```

And remember:

| Browser         | Node.js         |
| --------------- | --------------- |
| Runs JavaScript | Runs JavaScript |
| Has DOM         | No DOM          |
| `window`        | `process`       |
| Browser APIs    | Node APIs       |

---

# Phase 13 Progress

```text
[x] Step 1 — What is Node.js?
[x] Step 2 — Node.js Environment
[ ] Step 3 — npm and package.json
[ ] Step 4 — Node Modules
[ ] Step 5 — Built-in Modules
[ ] Step 6 — Creating a Basic Server
```

-----------------------------------------------------------------------------------------------------------------

# Step 3: npm and `package.json`

This is one of the most important Node.js basics.

---

# 1. What is npm?

**npm = Node Package Manager**

It helps us install and manage external packages.

For example, later we will use:

```text
Express
Mongoose
jsonwebtoken
bcrypt
Zod
```

Instead of writing everything ourselves, we can install packages created by other developers.

```text
Your Node.js Project
        ↓
       npm
        ↓
Install Packages
        ↓
Express / Mongoose / etc.
```

---

# 2. What Is a Package?

A package is reusable code.

For example:

```text
Express
```

Provides tools for building servers and APIs.

Instead of building everything from scratch:

```text
Create HTTP server manually
Create routing manually
Create middleware manually
```

We install Express.

---

# 3. Creating a Node Project

Create a folder:

```text
my-backend/
```

Then initialize npm:

```text
npm init
```

npm will ask questions such as:

```text
Package name?
Version?
Description?
Entry point?
```

After completion, npm creates:

```text
package.json
```

---

# 4. What Is `package.json`?

Think of `package.json` as the identity/configuration file of a Node.js project.

Example:

```json
{
  "name": "my-backend",
  "version": "1.0.0",
  "description": "",
  "main": "index.js"
}
```

It contains information about your project.

---

# 5. Why Is `package.json` Important?

It can contain:

```text
Project name
Version
Dependencies
Scripts
Configuration
```

Example:

```text
package.json
│
├── Project Information
├── Dependencies
├── Scripts
└── Configuration
```

---

# 6. Installing a Package

Suppose we want Express.

```text
npm install express
```

npm will:

```text
1. Download Express
2. Add it to node_modules
3. Add it to package.json
4. Update package-lock.json
```

Your project becomes:

```text
my-backend/
│
├── node_modules/
├── package.json
├── package-lock.json
└── index.js
```

---

# 7. What Is `node_modules`?

This folder contains installed packages.

Example:

```text
node_modules/
├── express/
├── accepts/
├── body-parser/
└── many other dependencies
```

You usually do not manually edit this folder.

---

# 8. Should We Push `node_modules` to Git?

No.

Usually:

```text
node_modules/
```

is inside `.gitignore`.

Why?

Because it can be huge.

Instead, Git stores:

```text
package.json
package-lock.json
```

Another developer can simply run:

```text
npm install
```

npm reads:

```text
package.json
```

and installs the required dependencies.

---

# 9. Dependencies

After installing Express, your `package.json` may contain:

```json
{
  "dependencies": {
    "express": "^5.0.0"
  }
}
```

This means:

> This project depends on Express.

---

# 10. `dependencies` vs `devDependencies`

You will see two types.

### Dependencies

Packages required for the application to run.

```json
"dependencies": {
  "express": "^5.0.0"
}
```

Examples:

```text
express
mongoose
bcrypt
jsonwebtoken
```

---

### Dev Dependencies

Packages mainly used during development.

Examples:

```text
nodemon
eslint
prettier
typescript
```

Installed using:

```text
npm install -D nodemon
```

Then:

```json
{
  "devDependencies": {
    "nodemon": "..."
  }
}
```

---

# 11. What Is `package-lock.json`?

This file locks the exact versions of installed packages.

Imagine:

```text
package.json

express: ^5.0.0
```

The `^` can allow compatible version updates.

But `package-lock.json` records the exact versions actually installed.

This helps ensure:

```text
Developer A
        ↓
Same package versions

Developer B
        ↓
Same package versions
```

You should generally commit `package-lock.json` to Git.

---

# 12. npm Scripts

Another important part of `package.json`:

```json
{
  "scripts": {
    "start": "node index.js"
  }
}
```

Now instead of:

```text
node index.js
```

You can run:

```text
npm start
```

npm executes:

```text
node index.js
```

---

# 13. Development Script

Later you might have:

```json
{
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js"
  }
}
```

Then:

```text
npm run dev
```

npm runs:

```text
nodemon index.js
```

We'll properly learn scripts later.

For now, just understand the concept.

---

# 14. The Complete Mental Model

```text
npm
│
├── Install packages
├── Remove packages
├── Manage dependencies
└── Run project scripts
```

And:

```text
package.json
│
├── Project information
├── Dependencies
├── Dev dependencies
└── Scripts
```

---

# 15. Important Files

A typical Node project:

```text
my-backend/
│
├── node_modules/       → Installed packages
├── package.json        → Project configuration
├── package-lock.json   → Exact package versions
├── .gitignore          → Files Git ignores
└── index.js            → Application code
```

---

# The Most Important Things to Remember

### npm

```text
npm = Node Package Manager
```

### package

```text
Package = Reusable code/library
```

### package.json

```text
package.json
=
Project configuration + dependencies + scripts
```

### node_modules

```text
node_modules
=
Installed packages
```

### package-lock.json

```text
package-lock.json
=
Locks exact installed versions
```

---

# Simple Real-World Flow

```text
Create Project
      ↓
npm init
      ↓
package.json created
      ↓
npm install express
      ↓
node_modules created
      ↓
Express added to dependencies
```

---

# Phase 13 Progress

```text
[x] Step 1 — What is Node.js?
[x] Step 2 — Node.js Environment
[x] Step 3 — npm and package.json

[ ] Step 4 — Node Modules
[ ] Step 5 — Built-in Modules
[ ] Step 6 — Creating a Basic Server
```
----------------------------------------------------------------------------------------------------------------

# Step 4: Node.js Modules

Modules are how we split our Node.js application into multiple files and share code between them.

You already know a similar concept from frontend JavaScript.

---

# 1. The Problem

Imagine putting everything in one file:

```text
index.js

├── User logic
├── Todo logic
├── Authentication
├── Database logic
└── API logic
```

This becomes difficult to manage.

Instead:

```text
project/

├── index.js
├── users.js
├── todos.js
└── auth.js
```

Each file handles a specific responsibility.

This is where **modules** come in.

---

# 2. What Is a Module?

Simply:

> A module is a file containing reusable code.

For example:

```text
math.js
```

```js
function add(a, b) {
    return a + b;
}
```

This file is a module.

We can use its code in another file.

---

# 3. Two Module Systems in Node.js

Node.js commonly uses two systems:

```text
1. CommonJS
2. ES Modules (ESM)
```

You may encounter both in real projects.

---

# 4. CommonJS Modules

This is the traditional Node.js module system.

### Export:

```js
// math.js

function add(a, b) {
    return a + b;
}

module.exports = add;
```

### Import:

```js
// index.js

const add = require("./math");

console.log(add(2, 3));
```

Output:

```text
5
```

---

# 5. ES Modules

This is the modern JavaScript module system.

You already saw this in React.

### Export:

```js
// math.js

export function add(a, b) {
    return a + b;
}
```

### Import:

```js
// index.js

import { add } from "./math.js";

console.log(add(2, 3));
```

Notice:

```text
CommonJS          ES Modules

require()         import
module.exports    export
```

---

# 6. Which One Should You Use?

For modern Node.js projects, ES Modules are commonly used.

Since you already know React and modern JavaScript:

```text
export
import
```

will feel familiar.

We will primarily use **ES Modules** going forward.

---

# 7. Enabling ES Modules in Node.js

In `package.json`:

```json
{
    "type": "module"
}
```

Now Node.js understands:

```js
import
export
```

Example:

### `math.js`

```js
export function add(a, b) {
    return a + b;
}
```

### `index.js`

```js
import { add } from "./math.js";

console.log(add(5, 10));
```

---

# 8. Named Export vs Default Export

You already learned this in JavaScript/React, but let's connect it to Node.js.

### Named Export

```js
export function add() {}

export function subtract() {}
```

Import:

```js
import { add, subtract } from "./math.js";
```

---

### Default Export

```js
function add() {}

export default add;
```

Import:

```js
import add from "./math.js";
```

---

# 9. Multiple Modules

Real backend projects might look like:

```text
src/

├── server.js
├── routes/
│   ├── userRoutes.js
│   └── todoRoutes.js
│
├── controllers/
│   ├── userController.js
│   └── todoController.js
│
└── utils/
    └── helpers.js
```

Then modules communicate using:

```text
export
  ↓
import
```

For example:

```text
todoRoutes.js
      ↓ imports
todoController.js
```

---

# 10. Real Backend Flow

Later you'll see something like:

```text
Server
  ↓
Routes
  ↓
Controller
  ↓
Database
```

Modules help connect these files.

Example:

```text
server.js
   ↓ import
todoRoutes.js
   ↓ import
todoController.js
   ↓ import
todoModel.js
```

Each file has its own responsibility.

---

# 11. Important Difference From Browser Modules

In browser JavaScript, you might write:

```js
import { something } from "./file.js";
```

In Node.js ES Modules:

```js
import { something } from "./file.js";
```

Very similar.

But remember:

Node.js has its own runtime environment and module configuration.

For our projects:

```text
package.json
     ↓
"type": "module"
     ↓
Use import/export
```

---

# 12. Simple Mental Model

```text
File A
│
│ export
↓
Reusable Code
│
│ import
↓
File B
```

Or:

```text
math.js
   ↓ exports
add()

index.js
   ↓ imports
add()
```

---

# 13. Why Modules Are Important

Without modules:

```text
One huge file ❌
```

With modules:

```text
Small organized files ✅
```

This becomes extremely important when building:

```text
Express APIs
Authentication
Database models
Middleware
Controllers
Routes
```

A backend application can contain dozens or hundreds of files.

Modules keep everything connected and organized.

---

# CommonJS vs ES Modules Summary

| CommonJS                           | ES Modules                                 |
| ---------------------------------- | ------------------------------------------ |
| `require()`                        | `import`                                   |
| `module.exports`                   | `export`                                   |
| Traditional Node.js                | Modern JavaScript                          |
| `.js` imports often omit extension | Relative ESM imports usually include `.js` |

For our learning:

```text
We will mainly use:

ES Modules
import/export
```

---

# What You Should Remember

```text
Module
=
A file containing reusable code
```

Two systems:

```text
CommonJS

require()
module.exports
```

```text
ES Modules

import
export
```

Modern Node setup:

```json
{
    "type": "module"
}
```

---

# Phase 13 Progress

```text
[x] Step 1 — What is Node.js?
[x] Step 2 — Node.js Environment
[x] Step 3 — npm and package.json
[x] Step 4 — Node.js Modules

[ ] Step 5 — Built-in Node Modules
[ ] Step 6 — Creating a Basic HTTP Server
```

## Next → Step 5: Built-in Node.js Modules

We'll learn modules that Node.js already provides, such as:

```text
fs
path
os
process
```
-----------------------------------------------------------------------------------------------------------------------------------------

# Step 5: Built-in Node.js Modules

So far, we learned that Node.js supports modules.

Now let's learn something important:

> Node.js already provides many useful modules. You don't always need to install a package.

These are called **built-in modules**.

---

# 1. What Are Built-in Modules?

Node.js comes with modules already included.

For example:

```text
fs
path
os
http
process
```

You can use them directly.

```text
Node.js Installed
       ↓
Built-in Modules Available
```

No need for:

```text
npm install fs ❌
```

---

# 2. Important Built-in Modules

For now, focus on these:

```text
fs      → File system
path    → File/folder paths
os      → Operating system information
http    → Create HTTP servers
process → Information about current Node process
```

Let's understand each one.

---

# 3. `fs` — File System Module

`fs` means:

> File System

It allows Node.js to work with files.

For example:

```text
Create files
Read files
Write files
Delete files
```

Import it:

```js
import fs from "fs";
```

Example:

```js
import fs from "fs";

fs.writeFileSync(
    "hello.txt",
    "Hello World"
);
```

This creates:

```text
hello.txt
```

with:

```text
Hello World
```

---

# 4. Reading a File

Suppose:

```text
hello.txt
```

contains:

```text
Hello World
```

We can read it:

```js
import fs from "fs";

const data = fs.readFileSync(
    "hello.txt",
    "utf-8"
);

console.log(data);
```

Output:

```text
Hello World
```

---

# 5. Why `fs` Is Useful

Node.js can interact with the computer's file system.

```text
Node.js
   ↓
fs module
   ↓
Files
```

Later, backend applications may use file-related operations for:

```text
Uploads
Logs
Configuration
File processing
```

---

# 6. `path` Module

The `path` module helps work with file and folder paths.

Import:

```js
import path from "path";
```

Example:

```js
const filePath = path.join(
    "folder",
    "file.txt"
);

console.log(filePath);
```

It creates the correct path for your operating system.

Conceptually:

```text
folder/file.txt
```

The exact path separator can differ between operating systems.

That's why `path` is useful.

---

# 7. Why Not Just Write Paths Manually?

You could write:

```text
folder/file.txt
```

But operating systems can handle paths differently.

```text
Windows
folder\file.txt

Linux/macOS
folder/file.txt
```

The `path` module helps Node.js handle this correctly.

---

# 8. `os` Module

`os` means:

> Operating System

It provides information about the computer running Node.js.

Import:

```js
import os from "os";
```

Example:

```js
console.log(os.platform());
```

It might return:

```text
win32
```

Other useful information:

```text
Operating system
CPU information
Memory information
Architecture
```

---

# 9. `process`

`process` is slightly different.

It is globally available in Node.js.

You usually don't need to import it.

```js
console.log(process.platform);
```

It provides information about the currently running Node.js process.

Important things we'll use later:

```text
process.env
process.argv
process.cwd()
process.exit()
```

The most important one for backend development is:

```js
process.env
```

This is how Node.js accesses environment variables.

Example:

```js
console.log(process.env.PORT);
```

We'll study this properly later.

---

# 10. `http` Module

This module is very important.

It allows Node.js to create an HTTP server.

Import:

```js
import http from "http";
```

Conceptually:

```text
Browser
   ↓ HTTP Request
Node.js HTTP Server
   ↓
Response
```

Example:

```js
import http from "http";

const server = http.createServer(
    (request, response) => {
        response.end("Hello World");
    }
);

server.listen(3000);
```

Don't worry about understanding this code completely yet.

Our next step is specifically about creating an HTTP server.

---

# 11. Built-in Module vs npm Package

Important difference:

### Built-in Module

Already comes with Node.js.

```js
import fs from "fs";
```

No installation needed.

---

### External Package

Must be installed using npm.

```bash
npm install express
```

Then:

```js
import express from "express";
```

---

# Mental Model

```text
Node.js
│
├── Built-in Modules
│   ├── fs
│   ├── path
│   ├── http
│   └── os
│
└── External Packages
    ├── express
    ├── mongoose
    ├── bcrypt
    └── jsonwebtoken
```

---

# 12. Which Built-in Modules Matter Most?

For your backend journey:

| Module    | Purpose                  | Importance |
| --------- | ------------------------ | ---------- |
| `fs`      | Work with files          | Medium     |
| `path`    | Handle file paths        | Medium     |
| `http`    | Create HTTP servers      | High       |
| `process` | Environment/process info | Very High  |
| `os`      | System information       | Low/Medium |

You don't need to memorize every Node.js built-in module.

Just understand that they exist and what they generally do.

---

# What You Should Remember

```text
Built-in Module
=
A module already provided by Node.js
```

Examples:

```text
fs      → Files
path    → File paths
http    → HTTP servers
process → Environment/process information
os      → Operating system information
```

And:

```text
Built-in Module
    ↓
No npm installation needed


External Package
    ↓
npm install required
```

---

# Phase 13 Progress

```text
[x] Step 1 — What is Node.js?
[x] Step 2 — Node.js Environment
[x] Step 3 — npm and package.json
[x] Step 4 — Node.js Modules
[x] Step 5 — Built-in Node.js Modules

[ ] Step 6 — Creating a Basic HTTP Server
```

------------------------------------------------------------------------------------------------------------------------------------------

# Step 6: Creating Your First Node.js HTTP Server

Now we move from Node.js concepts to an actual backend server.

---

# 1. What Is an HTTP Server?

A server listens for requests and sends responses.

```text
Browser / Client
      ↓ Request
Node.js Server
      ↓ Response
Browser / Client
```

For example:

```text
Browser requests:

GET /


Server responds:

Hello World
```

---

# 2. Creating a Server with Node.js

Node.js provides the built-in `http` module.

```js
import http from "http";
```

Then create a server:

```js
const server = http.createServer((req, res) => {
    res.end("Hello World");
});
```

Finally, tell the server which port to listen on:

```js
server.listen(3000);
```

Complete code:

```js
import http from "http";

const server = http.createServer((req, res) => {
    res.end("Hello World");
});

server.listen(3000);
```

---

# 3. Understanding the Flow

Let's break it down.

### Step 1: Import HTTP

```js
import http from "http";
```

We are using Node's built-in HTTP module.

---

### Step 2: Create Server

```js
const server = http.createServer();
```

This creates an HTTP server.

But we need to tell it what to do when a request arrives.

```js
const server = http.createServer((req, res) => {
});
```

---

# 4. What Are `req` and `res`?

This is extremely important.

```js
(req, res)
```

Means:

```text
req = Request
res = Response
```

Flow:

```text
Client
   ↓
Request (req)
   ↓
Server
   ↓
Response (res)
   ↓
Client
```

---

## `req` — Request

Contains information about what the client requested.

For example:

```text
URL
HTTP Method
Headers
Request data
```

You can check:

```js
console.log(req.url);
```

If the user visits:

```text
/about
```

Then:

```text
req.url
=
/about
```

---

## `res` — Response

Used to send something back to the client.

Example:

```js
res.end("Hello World");
```

The browser receives:

```text
Hello World
```

---

# 5. What Is a Port?

Your computer can run multiple servers.

A port identifies which server should receive the request.

Example:

```text
localhost:3000
```

Here:

```text
localhost = Your computer
3000 = Port number
```

Flow:

```text
Browser
   ↓
localhost:3000
   ↓
Node.js Server
```

---

# 6. `server.listen()`

```js
server.listen(3000);
```

This means:

> Start the server and listen for requests on port 3000.

You can also add a callback:

```js
server.listen(3000, () => {
    console.log("Server running on port 3000");
});
```

---

# 7. Running the Server

Suppose your file is:

```text
server.js
```

Run:

```text
node server.js
```

Terminal:

```text
Server running on port 3000
```

Then open:

```text
http://localhost:3000
```

The browser sends a request to your Node.js server.

Your server responds:

```text
Hello World
```

---

# 8. Complete Request-Response Cycle

This is one of the most important backend concepts.

```text
1. User opens browser

2. Browser sends request

       GET /

3. Node.js receives request

       req

4. Your server processes it

5. Server sends response

       res

6. Browser receives response
```

Visual:

```text
Browser
   │
   │ GET /
   ▼
Node.js Server
   │
   │ "Hello World"
   ▼
Browser
```

---

# 9. Different URLs

We can check which URL the user requests.

```js
import http from "http";

const server = http.createServer((req, res) => {

    if (req.url === "/") {
        res.end("Home Page");
    }

    if (req.url === "/about") {
        res.end("About Page");
    }

});

server.listen(3000);
```

Now:

```text
localhost:3000
```

Returns:

```text
Home Page
```

And:

```text
localhost:3000/about
```

Returns:

```text
About Page
```

This is a very basic form of routing.

---

# 10. HTTP Methods

Requests also have methods.

Common methods:

```text
GET     → Get data
POST    → Create data
PUT     → Update data
DELETE  → Delete data
```

You can check:

```js
console.log(req.method);
```

Example:

```text
GET /
POST /users
DELETE /users/1
```

Later, Express will make this much easier.

---

# 11. Basic Server vs Express

With raw Node.js:

```js
if (req.url === "/users") {
    // Handle users
}
```

With Express:

```js
app.get("/users", (req, res) => {
    res.json(users);
});
```

Express gives us a much cleaner way to build APIs.

That's why most Node.js backend applications use a framework like Express.

---

# 12. Mental Model

```text
Node.js
   ↓
HTTP Module
   ↓
Create Server
   ↓
Listen on Port
   ↓
Receive Request (req)
   ↓
Process Request
   ↓
Send Response (res)
```

---

# What You Should Remember

### Server

```text
Listens for requests and sends responses.
```

### Request

```text
req = Information coming from the client.
```

### Response

```text
res = Information sent back to the client.
```

### Port

```text
localhost:3000

3000 = Port where the server runs.
```

### Core Flow

```text
Client
  ↓ Request
Server
  ↓ Response
Client
```

---

# Phase 13 Progress

```text
[x] Step 1 — What is Node.js?
[x] Step 2 — Node.js Environment
[x] Step 3 — npm and package.json
[x] Step 4 — Node.js Modules
[x] Step 5 — Built-in Node.js Modules
[x] Step 6 — Creating a Basic HTTP Server
```

## Next → Step 7: HTTP Fundamentals

Before moving to Express, we should clearly understand:

```text
HTTP
Request
Response
Methods
Status Codes
Headers
```

* `req` = Request coming **from the client to the server**
* `res` = Response going **from the server back to the client**

---------------------------------------------------------------------------------------------------------------------------------------

# Step 7: HTTP Fundamentals

Before Express, you need to clearly understand how communication happens between frontend and backend.

This is one of the most important backend concepts.

---

# 1. What is HTTP?

**HTTP = HyperText Transfer Protocol**

Don't worry about memorizing the full name.

Simply:

> HTTP is the set of rules used for communication between a client and a server.

```text
Client (React / Browser)
        ↓ HTTP Request
Server (Node.js)
        ↓ HTTP Response
Client
```

---

# 2. Client and Server

### Client

The client requests something.

Examples:

```text
Browser
React Application
Mobile App
Postman
```

### Server

The server receives the request and sends a response.

```text
Node.js
Express
Backend API
```

Example:

```text
React App
    ↓
GET /todos
    ↓
Node.js Server
    ↓
Returns Todos
    ↓
React App
```

---

# 3. HTTP Request

A request contains information about what the client wants.

Example:

```text
GET /users
```

A request has several important parts:

```text
HTTP Request
│
├── Method
├── URL
├── Headers
├── Query Parameters
└── Body
```

Let's understand each.

---

# 4. HTTP Methods

The method tells the server what action the client wants to perform.

The main methods are:

| Method | Meaning               |
| ------ | --------------------- |
| GET    | Get data              |
| POST   | Create data           |
| PUT    | Replace/update data   |
| PATCH  | Partially update data |
| DELETE | Delete data           |

Example:

```text
GET /todos
```

Means:

> Give me all todos.

```text
POST /todos
```

Means:

> Create a new todo.

```text
DELETE /todos/1
```

Means:

> Delete todo with ID 1.

---

# 5. URL / Endpoint

An endpoint is the URL the client requests.

Example:

```text
/users
```

Or:

```text
/todos
```

Or:

```text
/todos/5
```

Full request:

```text
GET /todos
```

Breakdown:

```text
GET      → HTTP Method
/todos   → Endpoint
```

---

# 6. HTTP Response

After processing the request, the server sends a response.

A response usually contains:

```text
HTTP Response
│
├── Status Code
├── Headers
└── Response Body
```

Example:

```json
{
    "id": 1,
    "title": "Learn Node.js"
}
```

---

# 7. Status Codes

Status codes tell the client what happened.

The most important ones:

### Success

| Code | Meaning    |
| ---- | ---------- |
| 200  | OK         |
| 201  | Created    |
| 204  | No Content |

### Client Errors

| Code | Meaning      |
| ---- | ------------ |
| 400  | Bad Request  |
| 401  | Unauthorized |
| 403  | Forbidden    |
| 404  | Not Found    |

### Server Errors

| Code | Meaning               |
| ---- | --------------------- |
| 500  | Internal Server Error |

---

# 8. Example: Successful Request

Client:

```text
GET /todos
```

Server response:

```text
Status: 200 OK
```

Body:

```json
[
    {
        "id": 1,
        "title": "Learn Node.js"
    }
]
```

---

# 9. Example: Creating Data

Client:

```text
POST /todos
```

Body:

```json
{
    "title": "Learn Express"
}
```

Server:

```text
Creates Todo
```

Response:

```text
201 Created
```

Body:

```json
{
    "id": 2,
    "title": "Learn Express"
}
```

---

# 10. Headers

Headers contain additional information about the request or response.

Example:

```text
Content-Type: application/json
```

This tells the server:

> The request body contains JSON.

Another common header:

```text
Authorization: Bearer TOKEN
```

We'll use this later for JWT authentication.

You don't need to memorize headers now.

Just understand:

```text
Headers
=
Additional information about the request/response.
```

---

# 11. Request Body

The body contains data sent to the server.

Usually used with:

```text
POST
PUT
PATCH
```

Example:

```json
{
    "name": "John",
    "email": "john@example.com"
}
```

The server receives this data and processes it.

---

# 12. Query Parameters

Query parameters provide additional information in the URL.

Example:

```text
/products?category=phone
```

Breakdown:

```text
/products
    ↓
Endpoint

category=phone
    ↓
Query Parameter
```

Another example:

```text
/users?page=2&limit=10
```

This might mean:

```text
Page: 2
Limit: 10 users
```

---

# 13. Route Parameters

Route parameters identify a specific resource.

Example:

```text
/users/5
```

Here:

```text
5 = User ID
```

Conceptually:

```text
/users/:id
```

Examples:

```text
/users/1
/users/2
/users/10
```

We will use route parameters extensively in Express.

---

# 14. Complete Example

Imagine your React application wants one user.

### Request

```text
GET /users/5
```

The server receives:

```text
Method: GET
Route: /users/5
```

The server finds user 5.

### Response

```text
Status: 200
```

```json
{
    "id": 5,
    "name": "John"
}
```

Complete flow:

```text
React
  ↓ GET /users/5
Express Server
  ↓
Database
  ↓ User Found
Express Server
  ↓ 200 + User Data
React
```

---

# 15. HTTP Mental Model

```text
CLIENT

Method
URL
Headers
Body
    │
    │ HTTP Request
    ▼
SERVER
    │
    │ Process Request
    ▼
DATABASE
    │
    ▼
SERVER
    │
    │ HTTP Response
    ▼
CLIENT

Status Code
Headers
Body
```

---

# Most Important Things to Remember

## HTTP

```text
Communication between Client and Server
```

## Request

```text
Method
URL
Headers
Body
```

## Response

```text
Status Code
Headers
Body
```

## Main Methods

```text
GET     → Read
POST    → Create
PUT     → Update
PATCH   → Partial Update
DELETE  → Delete
```

## Common Status Codes

```text
200 → Success
201 → Created
400 → Bad Request
401 → Unauthorized
403 → Forbidden
404 → Not Found
500 → Server Error
```

---

# Phase 13 Progress

```text
[x] Step 1 — What is Node.js?
[x] Step 2 — Node.js Environment
[x] Step 3 — npm and package.json
[x] Step 4 — Node.js Modules
[x] Step 5 — Built-in Node.js Modules
[x] Step 6 — Basic HTTP Server
[x] Step 7 — HTTP Fundamentals

[ ] Step 8 — Introduction to Express.js
```

----------------------------------------------------------------------------------------------------------------------------------------

# Step 8: Introduction to Express.js

Now that you understand Node.js and HTTP, let's move to **Express.js**.

---

# 1. What is Express.js?

> **Express.js is a web framework for Node.js used to build servers and APIs more easily.**

The relationship is:

```text
JavaScript
    ↓
Node.js
    ↓
Express.js
    ↓
Backend API
```

* JavaScript → programming language
* Node.js → runtime environment
* Express.js → backend framework

---

# 2. Why Do We Need Express?

You already created a server using raw Node.js:

```js
import http from "http";

const server = http.createServer((req, res) => {
    res.end("Hello World");
});

server.listen(3000);
```

This works.

But imagine building many routes:

```text
/users
/users/1
/todos
/todos/1
/products
/auth/login
/auth/register
```

With raw Node.js, handling everything manually becomes difficult.

Express makes this easier.

---

# 3. Express Example

With Express:

```js
import express from "express";

const app = express();

app.get("/", (req, res) => {
    res.send("Hello World");
});

app.listen(3000);
```

Much cleaner.

---

# 4. Understanding the Code

### Import Express

```js
import express from "express";
```

We import the Express package.

Remember: Express is not built into Node.js.

So first it must be installed:

```text
npm install express
```

---

### Create an Express Application

```js
const app = express();
```

Think of `app` as your Express server application.

```text
Express
   ↓
app
   ↓
Routes + Middleware + API
```

---

### Create a Route

```js
app.get("/", (req, res) => {
    res.send("Hello World");
});
```

Break it down:

```text
app.get()
```

Means:

> Handle a GET request.

```text
"/"
```

Means:

> Home route.

```text
(req, res)
```

Same concept you learned before:

```text
req = Request
res = Response
```

---

# 5. Start the Server

```js
app.listen(3000);
```

This starts the server on port 3000.

Full code:

```js
import express from "express";

const app = express();

app.get("/", (req, res) => {
    res.send("Hello World");
});

app.listen(3000);
```

---

# 6. The Request Flow

When someone visits:

```text
localhost:3000
```

The flow is:

```text
Browser
   ↓ GET /
Express Server
   ↓
app.get("/")
   ↓
res.send("Hello World")
   ↓
Browser
```

---

# 7. Multiple Routes

Express makes routing very simple.

```js
app.get("/", (req, res) => {
    res.send("Home Page");
});

app.get("/about", (req, res) => {
    res.send("About Page");
});

app.get("/users", (req, res) => {
    res.send("Users");
});
```

Now:

```text
GET /        → Home Page
GET /about   → About Page
GET /users   → Users
```

---

# 8. Express vs Raw Node.js

| Raw Node.js                 | Express                 |
| --------------------------- | ----------------------- |
| More manual work            | Simpler                 |
| Manual routing              | Easy routing            |
| More boilerplate            | Cleaner code            |
| Uses `http` module directly | Built on top of Node.js |

Important:

> Express does not replace Node.js.

Express runs on Node.js.

```text
Node.js
   ↓
Express
   ↓
Your Backend Application
```

---

# 9. What Will Express Handle for Us?

As we continue, Express will help us with:

```text
✓ Routing
✓ Middleware
✓ Request handling
✓ JSON responses
✓ Error handling
✓ API creation
```

For example:

```js
app.get("/todos", ...);
app.post("/todos", ...);
app.put("/todos/:id", ...);
app.delete("/todos/:id", ...);
```

This will eventually become your Todo REST API.

---

# 10. Important Mental Model

Think of it like this:

```text
React
   ↓
Frontend Framework/Library

Node.js
   ↓
Runtime Environment

Express
   ↓
Backend Web Framework
```

And together:

```text
React Frontend
      ↓ HTTP
Express Backend
      ↓
Database
```

---

# What You Should Remember

```text
Express.js
=
A framework built on top of Node.js
```

Its main purpose:

```text
Build servers and APIs easily.
```

Basic Express flow:

```js
const app = express();

app.get("/", (req, res) => {
    res.send("Hello");
});

app.listen(3000);
```

---

# Phase 13 Progress

```text
[x] Step 1 — What is Node.js?
[x] Step 2 — Node.js Environment
[x] Step 3 — npm and package.json
[x] Step 4 — Node.js Modules
[x] Step 5 — Built-in Node.js Modules
[x] Step 6 — Basic HTTP Server
[x] Step 7 — HTTP Fundamentals
[x] Step 8 — Introduction to Express.js

[ ] Step 9 — Express Routing
[ ] Step 10 — Middleware
```

## Next → Step 9: Express Routing

This is where we'll properly learn:

* `app.get()`
* `app.post()`
* `app.put()`
* `app.patch()`
* `app.delete()`
* Route parameters
* Query parameters

-----------------------------------------------------------------------------------------------------------------------------------------

# Step 9: Express Routing

You already saw this:

```js
app.get("/", (req, res) => {
    res.send("Hello World");
});
```

Now let's properly understand **routing**.

---

# 1. What is Routing?

A route determines:

> When a specific HTTP request comes to a specific URL, what should the server do?

For example:

```text
GET /users
```

The server needs to know:

```text
What URL?
→ /users

What method?
→ GET

What should happen?
→ Send users
```

That is routing.

---

# 2. Basic Route Structure

```js
app.METHOD(PATH, HANDLER);
```

Example:

```js
app.get("/users", (req, res) => {
    res.send("Users");
});
```

Breakdown:

```text
app.get
   ↓
HTTP Method

"/users"
   ↓
Route / Path

(req, res) => {}
   ↓
Handler Function
```

---

# 3. GET Route

Used to retrieve data.

```js
app.get("/users", (req, res) => {
    res.send("Get all users");
});
```

Request:

```text
GET /users
```

---

# 4. POST Route

Used to create data.

```js
app.post("/users", (req, res) => {
    res.send("Create user");
});
```

Request:

```text
POST /users
```

---

# 5. PUT Route

Used to update or replace data.

```js
app.put("/users/1", (req, res) => {
    res.send("Update user");
});
```

---

# 6. PATCH Route

Used for partial updates.

```js
app.patch("/users/1", (req, res) => {
    res.send("Partially update user");
});
```

---

# 7. DELETE Route

Used to delete data.

```js
app.delete("/users/1", (req, res) => {
    res.send("Delete user");
});
```

---

# 8. Complete CRUD Route Example

```js
app.get("/todos", (req, res) => {
    res.send("Get all todos");
});

app.post("/todos", (req, res) => {
    res.send("Create todo");
});

app.put("/todos/:id", (req, res) => {
    res.send("Update todo");
});

app.delete("/todos/:id", (req, res) => {
    res.send("Delete todo");
});
```

This follows CRUD:

```text
GET     → Read
POST    → Create
PUT     → Update
DELETE  → Delete
```

---

# 9. Route Parameters

Suppose you have:

```text
/users/1
/users/2
/users/10
```

You don't want to create separate routes for every user.

Instead:

```js
app.get("/users/:id", (req, res) => {
    console.log(req.params);
});
```

The `:id` is a dynamic route parameter.

If the user requests:

```text
GET /users/5
```

Then:

```js
req.params.id
```

is:

```text
5
```

Example:

```js
app.get("/users/:id", (req, res) => {
    const userId = req.params.id;

    res.send(`User ID: ${userId}`);
});
```

---

# 10. Multiple Route Parameters

You can have multiple parameters.

```js
app.get("/users/:userId/posts/:postId", (req, res) => {
    console.log(req.params);
});
```

Request:

```text
/users/10/posts/25
```

Then:

```js
req.params
```

contains conceptually:

```js
{
    userId: "10",
    postId: "25"
}
```

---

# 11. Query Parameters

Query parameters come after `?` in a URL.

Example:

```text
/products?page=2&limit=10
```

In Express:

```js
app.get("/products", (req, res) => {
    console.log(req.query);
});
```

Conceptually:

```js
req.query
```

contains:

```js
{
    page: "2",
    limit: "10"
}
```

---

# 12. Route Params vs Query Params

This distinction is important.

### Route Parameter

Used to identify a specific resource.

```text
/users/5
```

```js
req.params.id
```

---

### Query Parameter

Used for filtering, sorting, pagination, etc.

```text
/users?page=2&limit=10
```

```js
req.query.page
req.query.limit
```

---

# 13. Request Body

For POST requests, data usually comes in the request body.

Example request:

```json
{
    "name": "John",
    "email": "john@example.com"
}
```

In Express:

```js
app.post("/users", (req, res) => {
    console.log(req.body);
});
```

But there is something important:

Express needs middleware to understand JSON request bodies.

We'll learn that in the next topic.

Usually:

```js
app.use(express.json());
```

Then:

```js
req.body
```

works for JSON requests.

---

# 14. Sending Responses

Express provides several ways to respond.

### Text

```js
res.send("Hello");
```

### JSON

```js
res.json({
    message: "Success"
});
```

For APIs, `res.json()` is very common.

Example:

```js
app.get("/users", (req, res) => {
    res.json([
        { id: 1, name: "John" },
        { id: 2, name: "Jane" }
    ]);
});
```

---

# 15. Status Codes in Express

You can set the status before sending a response.

```js
res.status(200).json({
    message: "Success"
});
```

Creating something:

```js
res.status(201).json({
    message: "User created"
});
```

Not found:

```js
res.status(404).json({
    message: "User not found"
});
```

---

# 16. Complete Example

```js
import express from "express";

const app = express();

app.use(express.json());

app.get("/users", (req, res) => {
    res.status(200).json({
        message: "Get all users"
    });
});

app.get("/users/:id", (req, res) => {
    const id = req.params.id;

    res.json({
        message: `Get user ${id}`
    });
});

app.post("/users", (req, res) => {
    const user = req.body;

    res.status(201).json({
        message: "User created",
        user
    });
});

app.listen(3000);
```

This small application already demonstrates:

```text
Routing
HTTP Methods
Route Parameters
Request Body
JSON Responses
Status Codes
```

---

# 17. Mental Model

```text
Client Request
     ↓
HTTP Method + URL
     ↓
Express Route
     ↓
Route Handler
     ↓
Process Request
     ↓
Send Response
```

Example:

```text
POST /users
      ↓
app.post("/users")
      ↓
(req, res) => {}
      ↓
req.body
      ↓
res.status(201).json()
```

---

# Most Important Things to Remember

### Route

```text
A route connects an HTTP request to backend logic.
```

### Route Structure

```js
app.get("/path", handler);
```

### Route Parameters

```text
/users/:id

req.params.id
```

### Query Parameters

```text
/users?page=2

req.query.page
```

### Request Body

```text
POST /users

req.body
```

### JSON Response

```js
res.status(200).json(data);
```

---

# Phase 13 Progress

```text
[x] Step 1 — What is Node.js?
[x] Step 2 — Node.js Environment
[x] Step 3 — npm and package.json
[x] Step 4 — Node.js Modules
[x] Step 5 — Built-in Node.js Modules
[x] Step 6 — Basic HTTP Server
[x] Step 7 — HTTP Fundamentals
[x] Step 8 — Introduction to Express.js
[x] Step 9 — Express Routing

[ ] Step 10 — Express Middleware
```

----------------------------------------------------------------------------------------------------------------------------------------

# Step 10: Express Middleware

This is a very important Express concept. Take your time with this one.

---

# 1. What Is Middleware?

Middleware is a function that runs **between the incoming request and the final response**.

Basic flow:

```text
Client Request
      ↓
Middleware
      ↓
Middleware
      ↓
Route Handler
      ↓
Response
```

The word itself explains it:

> **Middle + software/function = Middleware**

It sits in the middle of the request-response process.

---

# 2. Without Middleware

Imagine this route:

```js
app.get("/users", (req, res) => {
    res.send("Users");
});
```

Flow:

```text
Client
  ↓
Route Handler
  ↓
Response
```

---

# 3. With Middleware

```js
function logger(req, res, next) {
    console.log("Request received");

    next();
}

app.get("/users", logger, (req, res) => {
    res.send("Users");
});
```

Flow:

```text
Client Request
      ↓
logger middleware
      ↓
next()
      ↓
Route Handler
      ↓
Response
```

---

# 4. Middleware Structure

A middleware function usually receives:

```js
(req, res, next)
```

Example:

```js
function middleware(req, res, next) {
    // Do something

    next();
}
```

Let's understand these.

---

## `req`

The request object.

Contains:

```text
URL
Method
Headers
Body
Parameters
```

---

## `res`

The response object.

Used to send a response.

```js
res.send("Hello");
```

---

## `next`

This is the most important part.

```js
next();
```

Means:

> Continue to the next middleware or route handler.

---

# 5. Understanding `next()` Clearly

Look at this:

```js
function logger(req, res, next) {
    console.log("Logger running");

    next();
}

app.get("/users", logger, (req, res) => {
    res.send("Users");
});
```

Request:

```text
GET /users
```

Execution:

```text
1. Request arrives

2. logger() runs

3. Console:
   "Logger running"

4. next() runs

5. Express moves to route handler

6. Response sent
```


> next() passes control to the next middleware/route handler in the stack.

> Call next(err) with an argument → skips to error-handling middleware.

---

# 6. What Happens Without `next()`?

```js
function logger(req, res, next) {
    console.log("Logger running");
}
```

Now:

```text
Request
   ↓
logger()
   ↓
Stops here ❌
```

The route handler never runs.

The request may remain pending because nothing continues the request or sends a response.

---

# 7. Simple Real-Life Analogy

Think of entering an airport.

```text
You
 ↓
Security Check
 ↓
Passport Check
 ↓
Boarding Gate
 ↓
Flight
```

Each checkpoint is like middleware.

```text
Client Request
     ↓
Authentication Middleware
     ↓
Validation Middleware
     ↓
Logging Middleware
     ↓
Route Handler
     ↓
Response
```

---

# 8. Global Middleware

You can apply middleware to every request.

```js
app.use(logger);
```

Example:

```js
import express from "express";

const app = express();

function logger(req, res, next) {
    console.log(req.method, req.url);

    next();
}

app.use(logger);

app.get("/", (req, res) => {
    res.send("Home");
});

app.get("/users", (req, res) => {
    res.send("Users");
});
```

Now the logger runs for:

```text
GET /
GET /users
POST /users
DELETE /users/1
```

Basically, every request that passes through it.

---

# 9. Route-Specific Middleware

You can apply middleware to only one route.

```js
app.get("/users", logger, (req, res) => {
    res.send("Users");
});
```

Now:

```text
GET /users
```

runs the logger.

But:

```text
GET /
```

doesn't use that middleware.

---

# 10. Built-in Middleware: `express.json()`

You already saw this:

```js
app.use(express.json());
```

This is middleware provided by Express.

Its job:

```text
JSON Request
     ↓
express.json()
     ↓
Convert/Parse JSON
     ↓
Available in req.body
```

Example request:

```json
{
    "name": "John"
}
```

Without:

```js
app.use(express.json());
```

Express will not parse a JSON request body for `req.body`.

With it:

```js
app.use(express.json());

app.post("/users", (req, res) => {
    console.log(req.body);
});
```

Output:

```js
{
    name: "John"
}
```

This middleware will be used in almost every Express API.

---

# 11. Multiple Middleware

You can have multiple middleware functions.

```js
function middleware1(req, res, next) {
    console.log("Middleware 1");
    next();
}

function middleware2(req, res, next) {
    console.log("Middleware 2");
    next();
}

app.get(
    "/users",
    middleware1,
    middleware2,
    (req, res) => {
        res.send("Users");
    }
);
```

Flow:

```text
Request
   ↓
Middleware 1
   ↓ next()
Middleware 2
   ↓ next()
Route Handler
   ↓
Response
```

---

# 12. Middleware Order Matters

This is extremely important.

Express executes middleware in order.

Example:

```js
app.use(middleware1);

app.use(middleware2);

app.get("/users", handler);
```

Flow:

```text
Request
   ↓
middleware1
   ↓
middleware2
   ↓
/users handler
```

Order matters because Express processes middleware from top to bottom.

---

# 13. Common Types of Middleware

### 1. Logging Middleware

```text
Request
 ↓
Log Method + URL
 ↓
Continue
```

Example:

```js
function logger(req, res, next) {
    console.log(req.method, req.url);
    next();
}
```

---

### 2. Authentication Middleware

```text
Request
 ↓
Check Token
 ↓
Valid?
 ↓
Yes → Continue
No  → Reject
```

Later:

```text
JWT Middleware
```

will work like this.

---

### 3. Validation Middleware

```text
Request
 ↓
Check Input
 ↓
Valid?
 ↓
Yes → Continue
No  → Send Error
```

Later we will use Zod or Joi.

---

### 4. Error Middleware

Handles application errors.

Conceptually:

```text
Request
   ↓
Something goes wrong
   ↓
Error Middleware
   ↓
Error Response
```

We'll learn this separately.

---

# 14. Middleware Can Stop the Request

Not every middleware calls `next()`.

For example:

```js
function authMiddleware(req, res, next) {
    const token = req.headers.authorization;

    if (!token) {
        return res.status(401).json({
            message: "Unauthorized"
        });
    }

    next();
}
```

Flow if no token:

```text
Request
   ↓
Authentication Middleware
   ↓
No Token
   ↓
401 Response
   ↓
STOP
```

Flow if token exists:

```text
Request
   ↓
Authentication Middleware
   ↓
Token Exists
   ↓
next()
   ↓
Route Handler
```

This is very important.

Middleware has two main choices:

```text
1. next()        → Continue
2. res.send()    → Stop and send response
```

---

# 15. Complete Mental Model

```text
CLIENT REQUEST
      │
      ▼
┌─────────────────┐
│ Middleware 1    │
│ Logger          │
└────────┬────────┘
         │ next()
         ▼
┌─────────────────┐
│ Middleware 2    │
│ Authentication  │
└────────┬────────┘
         │ next()
         ▼
┌─────────────────┐
│ Middleware 3    │
│ Validation      │
└────────┬────────┘
         │ next()
         ▼
┌─────────────────┐
│ Route Handler   │
└────────┬────────┘
         │
         ▼
      RESPONSE
```

---

# The Most Important Concept

Remember this:

```js
function middleware(req, res, next) {
    // Do something

    next();
}
```

And:

```text
next()
=
Go to the next middleware or route handler.
```

But middleware can also stop the request:

```js
res.status(401).json({
    message: "Unauthorized"
});
```

---

# Simple Summary

| Concept     | Meaning                               |
| ----------- | ------------------------------------- |
| Middleware  | Function between request and response |
| `req`       | Incoming request                      |
| `res`       | Outgoing response                     |
| `next()`    | Continue to next middleware           |
| `app.use()` | Register middleware                   |
| Order       | Middleware runs top → bottom          |

---

# Phase 13 Progress

```text
[x] Step 1 — Node.js Basics
[x] Step 2 — Node Environment
[x] Step 3 — npm + package.json
[x] Step 4 — Modules
[x] Step 5 — Built-in Modules
[x] Step 6 — HTTP Server
[x] Step 7 — HTTP Fundamentals
[x] Step 8 — Express Introduction
[x] Step 9 — Express Routing
[x] Step 10 — Middleware

[ ] Step 11 — REST API Design + CRUD
```
----------------------------------------------------------------------------------------------------------------------------------------

# Step 11: REST API Design and CRUD

Now we're connecting everything you've learned:

```text
HTTP Methods
+ Express Routing
+ Request/Response
= REST API
```

---

# 1. What is an API?

API means:

> A way for two applications to communicate.

Example:

```text
React Frontend
      ↓ API Request
Express Backend
      ↓
Database
```

Your frontend asks:

```text
GET /todos
```

Backend responds:

```json
[
  {
    "id": 1,
    "title": "Learn Node.js"
  }
]
```

---

# 2. What is REST?

REST is a common architectural style for designing APIs.

You don't need to memorize the full theory.

For us:

> REST API uses HTTP methods and URLs to perform operations on resources.

A resource can be:

```text
Users
Todos
Products
Orders
Posts
```

---

# 3. REST API URL Design

Suppose we're building a Todo API.

Bad design:

```text
/getTodos
/createTodo
/deleteTodo
/updateTodo
```

REST-style design:

```text
/todos
```

We use the HTTP method to determine the action.

| HTTP Method | Endpoint     | Action        |
| ----------- | ------------ | ------------- |
| GET         | `/todos`     | Get all todos |
| GET         | `/todos/:id` | Get one todo  |
| POST        | `/todos`     | Create todo   |
| PATCH       | `/todos/:id` | Update todo   |
| DELETE      | `/todos/:id` | Delete todo   |

This is cleaner.

---

# 4. What is CRUD?

CRUD represents the four basic database operations:

```text
C → Create
R → Read
U → Update
D → Delete
```

REST maps HTTP methods to CRUD:

| CRUD   | HTTP Method |
| ------ | ----------- |
| Create | POST        |
| Read   | GET         |
| Update | PUT / PATCH |
| Delete | DELETE      |

---

# 5. Example: Todo REST API

Let's imagine we have:

```js
const todos = [
    {
        id: 1,
        title: "Learn React",
        completed: false
    }
];
```

For now, we're using an array instead of a database.

---

# 6. READ — Get All Todos

```js
app.get("/todos", (req, res) => {
    res.status(200).json(todos);
});
```

Request:

```text
GET /todos
```

Response:

```json
[
    {
        "id": 1,
        "title": "Learn React",
        "completed": false
    }
]
```

---

# 7. READ — Get One Todo

```js
app.get("/todos/:id", (req, res) => {
    const id = Number(req.params.id);

    const todo = todos.find(
        (todo) => todo.id === id
    );

    if (!todo) {
        return res.status(404).json({
            message: "Todo not found"
        });
    }

    res.status(200).json(todo);
});
```

Request:

```text
GET /todos/1
```

Important:

```js
req.params.id
```

is usually received as a string.

So:

```js
Number(req.params.id)
```

converts it to a number.

---

# 8. CREATE — Add a Todo

First, JSON middleware:

```js
app.use(express.json());
```

Then:

```js
app.post("/todos", (req, res) => {
    const { title } = req.body;

    const newTodo = {
        id: todos.length + 1,
        title,
        completed: false
    };

    todos.push(newTodo);

    res.status(201).json(newTodo);
});
```

Request:

```text
POST /todos
```

Body:

```json
{
    "title": "Learn Express"
}
```

Response:

```json
{
    "id": 2,
    "title": "Learn Express",
    "completed": false
}
```

---

# 9. UPDATE — Update a Todo

Let's use PATCH.

```js
app.patch("/todos/:id", (req, res) => {
    const id = Number(req.params.id);

    const todo = todos.find(
        (todo) => todo.id === id
    );

    if (!todo) {
        return res.status(404).json({
            message: "Todo not found"
        });
    }

    const { title, completed } = req.body;

    if (title !== undefined) {
        todo.title = title;
    }

    if (completed !== undefined) {
        todo.completed = completed;
    }

    res.status(200).json(todo);
});
```

Request:

```text
PATCH /todos/1
```

Body:

```json
{
    "completed": true
}
```

Only that field changes.

---

# 10. DELETE — Delete a Todo

```js
app.delete("/todos/:id", (req, res) => {
    const id = Number(req.params.id);

    const index = todos.findIndex(
        (todo) => todo.id === id
    );

    if (index === -1) {
        return res.status(404).json({
            message: "Todo not found"
        });
    }

    todos.splice(index, 1);

    res.status(204).send();
});
```

Request:

```text
DELETE /todos/1
```

Response:

```text
204 No Content
```

---

# 11. Complete CRUD Flow

```text
CREATE
POST /todos
     ↓
Create new todo


READ
GET /todos
GET /todos/:id
     ↓
Return todo data


UPDATE
PATCH /todos/:id
     ↓
Update existing todo


DELETE
DELETE /todos/:id
     ↓
Remove todo
```

---

# 12. REST API Naming Rules

A few important conventions:

### Use nouns, not verbs

Good:

```text
/users
/products
/todos
```

Avoid:

```text
/getUsers
/createProduct
/deleteTodo
```

---

### Use plural resource names

Preferred:

```text
/users
/todos
/products
```

Instead of:

```text
/user
/todo
/product
```

---

### Use route parameters for specific resources

```text
/users/:id
/todos/:id
/products/:id
```

---

# 13. Status Codes for CRUD

A practical guide:

| Operation                        | Status |
| -------------------------------- | ------ |
| Successfully get data            | 200    |
| Successfully create data         | 201    |
| Successfully delete with no body | 204    |
| Invalid request                  | 400    |
| Resource not found               | 404    |
| Unexpected server error          | 500    |

Example:

```text
GET /todos
→ 200

POST /todos
→ 201

DELETE /todos/1
→ 204

GET /todos/999
→ 404
```

---

# 14. The Full API Mental Model

```text
CLIENT
  │
  │ HTTP Request
  ▼
EXPRESS ROUTE
  │
  ▼
CONTROLLER LOGIC
  │
  ▼
DATABASE
  │
  ▼
RESPONSE
```

Right now:

```text
Client
  ↓
Express Route
  ↓
Array (temporary data)
  ↓
Response
```

Later:

```text
Client
  ↓
Express Route
  ↓
Controller
  ↓
Database
  ↓
Response
```

---

# What You Should Remember

### REST API

```text
Uses HTTP methods + resource URLs
```

### CRUD

```text
Create → POST
Read   → GET
Update → PUT/PATCH
Delete → DELETE
```

### Example

```text
GET    /todos
GET    /todos/:id
POST   /todos
PATCH  /todos/:id
DELETE /todos/:id
```

---

# Phase 13 Progress

```text
[x] Step 1  — Node.js Basics
[x] Step 2  — Node Environment
[x] Step 3  — npm + package.json
[x] Step 4  — Modules
[x] Step 5  — Built-in Modules
[x] Step 6  — HTTP Server
[x] Step 7  — HTTP Fundamentals
[x] Step 8  — Express Introduction
[x] Step 9  — Express Routing
[x] Step 10 — Middleware
[x] Step 11 — REST API Design + CRUD

[ ] Step 12 — Express Project Structure
```

## Next → Step 12: Organizing an Express Backend

We'll learn how real backend projects separate:

```text
Routes
Controllers
Models
Middleware
Config
```

This is where your backend starts looking like a real-world application.

----------------------------------------------------------------------------------------------------------------------------------------

# Step 12: Express Project Structure

So far, we have written everything in one file:

```text
server.js

- Routes
- Logic
- Data
- Middleware
```

That is okay for small applications.

But real backend projects need better organization.

---

# 1. The Problem With One Large File

Imagine this:

```js
app.get("/users", ...);
app.post("/users", ...);
app.get("/todos", ...);
app.post("/todos", ...);

// Authentication logic
// Database logic
// Validation logic
// Error handling
```

As the application grows:

```text
server.js
   ↓
500 lines
   ↓
2000 lines
   ↓
Very difficult to maintain ❌
```

So we separate responsibilities.

---

# 2. Basic Backend Project Structure

A common Express project structure:

```text
project/
│
├── src/
│   ├── server.js
│   │
│   ├── routes/
│   │   ├── userRoutes.js
│   │   └── todoRoutes.js
│   │
│   ├── controllers/
│   │   ├── userController.js
│   │   └── todoController.js
│   │
│   ├── models/
│   │   ├── User.js
│   │   └── Todo.js
│   │
│   ├── middleware/
│   │   └── authMiddleware.js
│   │
│   └── config/
│       └── db.js
│
├── package.json
└── .env
```

Don't worry about memorizing everything yet.

Let's understand each folder.

---

# 3. `server.js`

This is the entry point of the backend application.

Its main job:

```text
Start Express
↓
Add middleware
↓
Connect routes
↓
Start server
```

Example:

```js
import express from "express";
import userRoutes from "./routes/userRoutes.js";

const app = express();

app.use(express.json());

app.use("/users", userRoutes);

app.listen(3000);
```

Think of `server.js` as the main connection point.

---

# 4. Routes Folder

```text
routes/
```

Routes define:

> Which URL goes to which controller function?

Example:

### `userRoutes.js`

```js
import express from "express";
import {
    getUsers,
    createUser
} from "../controllers/userController.js";

const router = express.Router();

router.get("/", getUsers);

router.post("/", createUser);

export default router;
```

Notice:

```js
const router = express.Router();
```

Instead of:

```js
const app = express();
```

Why?

Because this file handles only a group of related routes.

---

# 5. What is `express.Router()`?

Think of it as a mini Express router.

```text
Express App
    │
    ├── User Router
    │
    ├── Todo Router
    │
    └── Product Router
```

Example:

```js
router.get("/", getUsers);
```

Then in `server.js`:

```js
app.use("/users", userRoutes);
```

Together:

```text
/users + /
=
/users
```

So:

```js
router.get("/", getUsers);
```

becomes:

```text
GET /users
```

---

# 6. Controllers Folder

Controllers contain the actual logic.

```text
Request
   ↓
Route
   ↓
Controller
   ↓
Response
```

Example:

### `userController.js`

```js
export function getUsers(req, res) {
    res.json([
        {
            id: 1,
            name: "John"
        }
    ]);
}

export function createUser(req, res) {
    const user = req.body;

    res.status(201).json({
        message: "User created",
        user
    });
}
```

The controller handles what should happen.

---

# 7. Route vs Controller

This is extremely important.

### Route

Determines:

```text
Which request goes where?
```

Example:

```js
router.get("/", getUsers);
```

---

### Controller

Determines:

```text
What should happen?
```

Example:

```js
function getUsers(req, res) {
    // Logic here
}
```

Mental model:

```text
Route
  ↓
"Where should this request go?"
  ↓
Controller
  ↓
"What should happen?"
```

---

# 8. Models Folder

Models represent your application's data.

Later, when using MongoDB:

```text
models/
    User.js
    Todo.js
```

A model usually handles interaction with the database.

Conceptually:

```text
Controller
    ↓
Model
    ↓
Database
```

Example:

```text
Todo Controller
      ↓
Todo Model
      ↓
MongoDB
```

We will study this properly when we learn databases.

---

# 9. Middleware Folder

Custom middleware goes here.

Example:

```text
middleware/
    authMiddleware.js
```

Later:

```text
Authentication Middleware
Validation Middleware
Error Middleware
```

Example concept:

```js
function authMiddleware(req, res, next) {
    // Check user token

    next();
}
```

---

# 10. Config Folder

Configuration-related files go here.

Example:

```text
config/
    db.js
```

Later this may contain:

```text
Database connection
Application configuration
External service configuration
```

Example:

```text
config/
├── db.js
└── cloud.js
```

---

# 11. `.env` File

Sensitive or environment-specific values should not be hardcoded.

Bad:

```js
const DB_PASSWORD = "mypassword";
```

Better:

```text
.env

PORT=3000
DATABASE_URL=...
JWT_SECRET=...
```

Then your application reads them through environment variables.

We'll learn this properly later.

---

# 12. Complete Request Flow

Let's see how all folders connect.

Suppose the client requests:

```text
GET /users
```

Flow:

```text
CLIENT
   ↓
server.js
   ↓
userRoutes.js
   ↓
userController.js
   ↓
User Model
   ↓
Database
   ↓
Controller
   ↓
Response
   ↓
CLIENT
```

Visual:

```text
React Frontend
      ↓
Express Server
      ↓
Route
      ↓
Controller
      ↓
Model
      ↓
Database
```

---

# 13. Real Example

### `server.js`

```js
import express from "express";
import userRoutes from "./routes/userRoutes.js";

const app = express();

app.use(express.json());

app.use("/users", userRoutes);

app.listen(3000);
```

---

### `routes/userRoutes.js`

```js
import express from "express";
import {
    getUsers,
    createUser
} from "../controllers/userController.js";

const router = express.Router();

router.get("/", getUsers);

router.post("/", createUser);

export default router;
```

---

### `controllers/userController.js`

```js
export function getUsers(req, res) {
    res.json({
        message: "Get users"
    });
}

export function createUser(req, res) {
    res.status(201).json({
        message: "User created"
    });
}
```

---

# 14. Full Flow

When this request happens:

```text
GET /users
```

The execution flow is:

```text
1. Request enters server.js

2. app.use("/users", userRoutes)

3. Request goes to userRoutes.js

4. router.get("/")

5. Calls getUsers controller

6. Controller processes request

7. Controller sends response
```

Visual:

```text
GET /users
    ↓
server.js
    ↓
userRoutes.js
    ↓
getUsers()
    ↓
Response
```

---

# 15. Why This Structure Is Useful

Instead of:

```text
One Huge server.js
       ❌
```

We get:

```text
Routes
   ↓
Controllers
   ↓
Models
   ↓
Database
```

Benefits:

```text
✓ Cleaner code
✓ Easier maintenance
✓ Easier debugging
✓ Better scalability
✓ Easier teamwork
```

---

# The Most Important Distinction

Remember this table:

| Layer       | Responsibility                      |
| ----------- | ----------------------------------- |
| `server.js` | Starts/configures application       |
| Routes      | Maps URL → controller               |
| Controllers | Handles application logic           |
| Models      | Interacts with database             |
| Middleware  | Runs before/around request handling |
| Config      | Application/database configuration  |

---

# Simple Mental Model

```text
Client
  ↓
Middleware
  ↓
Route
  ↓
Controller
  ↓
Model
  ↓
Database
  ↓
Response
```

This structure will become more meaningful as we add databases and authentication.

---

# Phase 13 Progress

```text
[x] Step 1  — Node.js Basics
[x] Step 2  — Node Environment
[x] Step 3  — npm + package.json
[x] Step 4  — Modules
[x] Step 5  — Built-in Modules
[x] Step 6  — HTTP Server
[x] Step 7  — HTTP Fundamentals
[x] Step 8  — Express Introduction
[x] Step 9  — Express Routing
[x] Step 10 — Middleware
[x] Step 11 — REST API Design + CRUD
[x] Step 12 — Express Project Structure

[ ] Step 13 — Error Handling in Express
```

## Next → Step 13: Error Handling

We'll learn:

```text
try/catch
Async errors
Custom error handling
Global error middleware
404 handling
```

This is an essential real-world backend topic.

-----------------------------------------------------------------------------------------------------------------------------------------

# Step 13: Error Handling in Express

Error handling is essential because things can go wrong in every backend application.

For example:

```text
User not found
Invalid input
Database connection failed
Server crashed
Wrong URL
```

Your backend should handle these errors properly instead of crashing.

---

# 1. What Is Error Handling?

Error handling means:

> Detecting errors and sending a proper response to the client.

Bad situation:

```text
Client Request
    ↓
Error happens
    ↓
Server crashes ❌
```

Better:

```text
Client Request
    ↓
Error happens
    ↓
Handle Error
    ↓
Send Proper Response
```

---

# 2. Basic `try...catch`

You already learned `try...catch` in JavaScript.

Example:

```js
try {
    // Code that may cause an error
} catch (error) {
    // Handle the error
}
```

In Express:

```js
app.get("/users", (req, res) => {
    try {
        const users = getUsers();

        res.json(users);
    } catch (error) {
        res.status(500).json({
            message: "Something went wrong"
        });
    }
});
```

---

# 3. Why Not Just Send `error.message`?

You might see:

```js
catch (error) {
    res.status(500).json({
        message: error.message
    });
}
```

During development, this can help.

But in production, exposing internal errors can reveal information about your application.

Better:

```js
catch (error) {
    console.error(error);

    res.status(500).json({
        message: "Internal Server Error"
    });
}
```

---

# 4. Handling "Not Found"

Suppose a user requests:

```text
GET /todos/999
```

But Todo 999 doesn't exist.

```js
app.get("/todos/:id", (req, res) => {
    const id = Number(req.params.id);

    const todo = todos.find(
        (todo) => todo.id === id
    );

    if (!todo) {
        return res.status(404).json({
            message: "Todo not found"
        });
    }

    res.json(todo);
});
```

The important part:

```js
return res.status(404).json(...)
```

The `return` ensures the function stops after sending the response.

---

# 5. Why `return` Is Important

Look at this:

```js
if (!todo) {
    res.status(404).json({
        message: "Todo not found"
    });
}

res.json(todo);
```

If Todo doesn't exist:

```text
404 response sent
       ↓
Code continues
       ↓
Another response tries to send ❌
```

This can cause an error.

Instead:

```js
if (!todo) {
    return res.status(404).json({
        message: "Todo not found"
    });
}

res.json(todo);
```

Flow:

```text
Todo not found
      ↓
Send 404 response
      ↓
return
      ↓
Function stops
```

---

# 6. Handling Async Errors

Backend applications often use asynchronous operations.

For example:

```js
app.get("/users", async (req, res) => {
    try {
        const users = await database.getUsers();

        res.json(users);
    } catch (error) {
        console.error(error);

        res.status(500).json({
            message: "Internal Server Error"
        });
    }
});
```

Flow:

```text
Request
   ↓
try
   ↓
await database operation
   ↓
Success → Response

OR

Error → catch → Error Response
```

---

# 7. The Problem With Repeating `try...catch`

Imagine many routes:

```js
app.get("/users", async () => {
    try {
        // code
    } catch (error) {
        // error handling
    }
});

app.get("/todos", async () => {
    try {
        // code
    } catch (error) {
        // error handling
    }
});

app.post("/products", async () => {
    try {
        // code
    } catch (error) {
        // error handling
    }
});
```

Lots of repetition.

So Express applications often use a **global error handler**.

---

# 8. Global Error Middleware

Normal middleware:

```js
(req, res, next)
```

Error middleware has **four parameters**:

```js
(error, req, res, next)
```

Example:

```js
function errorHandler(error, req, res, next) {
    console.error(error);

    res.status(500).json({
        message: "Internal Server Error"
    });
}
```

Then register it:

```js
app.use(errorHandler);
```

---

# 9. How Does an Error Reach the Error Middleware?

Using:

```js
next(error);
```

Example:

```js
app.get("/users", (req, res, next) => {
    try {
        throw new Error("Database failed");
    } catch (error) {
        next(error);
    }
});
```

Flow:

```text
Route
  ↓
Error happens
  ↓
next(error)
  ↓
Global Error Middleware
  ↓
Send Error Response
```

---

# 10. Complete Example

```js
import express from "express";

const app = express();

app.get("/users", (req, res, next) => {
    try {
        throw new Error("Something failed");
    } catch (error) {
        next(error);
    }
});

function errorHandler(error, req, res, next) {
    console.error(error.message);

    res.status(500).json({
        message: "Internal Server Error"
    });
}

app.use(errorHandler);

app.listen(3000);
```

---

# 11. Why Error Middleware Must Be Last

Express runs middleware in order.

```js
app.use(express.json());

app.use(userRoutes);

app.use(errorHandler);
```

Correct:

```text
Request
   ↓
Routes
   ↓
Error?
   ↓
Error Handler
```

If you place it too early, it may not behave as intended for errors coming from later middleware/routes.

So generally:

> Global error middleware goes near the end of the application setup.

---

# 12. 404 Route Handling

What if the user requests a route that doesn't exist?

Example:

```text
GET /unknown-page
```

You can add a 404 handler:

```js
app.use((req, res) => {
    res.status(404).json({
        message: "Route not found"
    });
});
```

Important: This should come **after your routes**.

Example:

```js
app.use("/users", userRoutes);
app.use("/todos", todoRoutes);

app.use((req, res) => {
    res.status(404).json({
        message: "Route not found"
    });
});
```

---

# 13. 404 Handler vs Error Handler

These are different.

| Situation               | Handler       |
| ----------------------- | ------------- |
| Route doesn't exist     | 404 handler   |
| Something crashes/fails | Error handler |

### 404

```text
GET /wrong-route
       ↓
404 Route Not Found
```

### Server Error

```text
GET /users
       ↓
Database crashes
       ↓
500 Internal Server Error
```

---

# 14. Typical Application Structure

```js
const app = express();

app.use(express.json());

// Routes
app.use("/users", userRoutes);
app.use("/todos", todoRoutes);

// 404 handler
app.use((req, res) => {
    res.status(404).json({
        message: "Route not found"
    });
});

// Global error handler
app.use(errorHandler);
```

Order:

```text
1. Middleware
2. Routes
3. 404 Handler
4. Error Handler
```

---

# 15. Important Status Codes for Errors

| Code | Meaning               |
| ---- | --------------------- |
| 400  | Bad Request           |
| 401  | Unauthorized          |
| 403  | Forbidden             |
| 404  | Not Found             |
| 409  | Conflict              |
| 500  | Internal Server Error |

You'll use these frequently.

---

# Mental Model

```text
CLIENT REQUEST
      ↓
Middleware
      ↓
Route
      ↓
Controller
      ↓
Something goes wrong?
      ↓ YES
next(error)
      ↓
Global Error Handler
      ↓
Error Response
```

If no matching route exists:

```text
Request
   ↓
No Route Found
   ↓
404 Handler
   ↓
404 Response
```

---

# Most Important Things to Remember

### Normal Error Handling

```js
try {
    // risky code
} catch (error) {
    // handle error
}
```

### Send Error to Global Handler

```js
next(error);
```

### Error Middleware

```js
function errorHandler(error, req, res, next) {
    // Handle error
}
```

### 404

```text
Route does not exist
```

### 500

```text
Something unexpected failed on the server
```

----------------------------------------------------------------------------------------------------------------------------------------

# Step 14: Async/Await in Express

You already know `async/await` from JavaScript. Now we need to understand **how it is used in real Express routes**, especially when working with databases.

---

## 1. Why do we need async/await in Express?

Backend operations are often asynchronous:

```text
Database query
API request
File operation
Authentication
Redis
Cloud storage
```

For example:

```js
const users = await User.find();
```

The database may take some time to respond.

So our Express handler becomes:

```js
app.get("/users", async (req, res) => {
    const users = await User.find();

    res.json(users);
});
```

The important part is:

```js
async (req, res) => {
```

and:

```js
await User.find();
```

---

# 2. Basic Express Async Route

Without async operation:

```js
app.get("/users", (req, res) => {
    res.json(users);
});
```

With an asynchronous operation:

```js
app.get("/users", async (req, res) => {
    const users = await getUsers();

    res.json(users);
});
```

Flow:

```text
Request
   ↓
Express Route
   ↓
async function
   ↓
await database/API operation
   ↓
Result
   ↓
Response
```

---

# 3. `async` and `await`

Remember the basic rule:

```js
async function something() {
    const result = await someAsyncOperation();
}
```

`await` means:

> Wait for the Promise to settle before continuing this async function.

It does **not** block the entire Node.js server.

For example:

```js
app.get("/users", async (req, res) => {
    const users = await getUsers();

    res.json(users);
});
```

While the database operation is happening, Node.js can continue handling other work.

---

# 4. Async/Await + try/catch

This is where async/await connects to the previous topic.

```js
app.get("/users", async (req, res) => {
    try {
        const users = await getUsers();

        res.json(users);
    } catch (error) {
        res.status(500).json({
            message: "Failed to get users"
        });
    }
});
```

Flow:

```text
Request
   ↓
try
   ↓
await database
   ↓
 ┌───────────────┐
 │               │
Success        Error
 │               │
 ↓               ↓
res.json()    catch
                 ↓
              Error response
```

---

# 5. Real Database Example

Later, with MongoDB/Mongoose, you'll have something like:

```js
app.get("/todos", async (req, res) => {
    try {
        const todos = await Todo.find();

        res.status(200).json(todos);
    } catch (error) {
        res.status(500).json({
            message: "Failed to fetch todos"
        });
    }
});
```

Here:

```js
await Todo.find();
```

is asynchronous because the application has to communicate with the database.

---

# 6. Async POST Route

Creating data is also asynchronous.

```js
app.post("/todos", async (req, res) => {
    try {
        const todo = await Todo.create({
            title: req.body.title
        });

        res.status(201).json(todo);
    } catch (error) {
        res.status(500).json({
            message: "Failed to create todo"
        });
    }
});
```

Flow:

```text
POST /todos
     ↓
req.body
     ↓
Todo.create()
     ↓
await
     ↓
Database
     ↓
Created Todo
     ↓
201 Response
```

---

# 7. Async Route With Parameters

```js
app.get("/todos/:id", async (req, res) => {
    try {
        const todo = await Todo.findById(req.params.id);

        if (!todo) {
            return res.status(404).json({
                message: "Todo not found"
            });
        }

        res.json(todo);
    } catch (error) {
        res.status(500).json({
            message: "Failed to fetch todo"
        });
    }
});
```

Notice we now have **three possible situations**:

```text
Todo found
    ↓
200


Todo doesn't exist
    ↓
404


Database/query error
    ↓
500
```

That's a very common backend pattern.

---

# 8. Why We Don't Use `.then()` Everywhere

You could write:

```js
Todo.find()
    .then((todos) => {
        res.json(todos);
    })
    .catch((error) => {
        res.status(500).json({
            message: "Error"
        });
    });
```

This works.

But modern backend code commonly uses:

```js
const todos = await Todo.find();
```

because it is easier to read, especially when multiple asynchronous operations are involved.

---

# 9. Multiple `await`s

Suppose we need to:

```text
1. Find user
2. Find user's todos
```

We could write:

```js
app.get("/users/:id/todos", async (req, res) => {
    try {
        const user = await User.findById(req.params.id);

        const todos = await Todo.find({
            userId: user._id
        });

        res.json(todos);
    } catch (error) {
        res.status(500).json({
            message: "Something went wrong"
        });
    }
});
```

This is where async/await becomes extremely useful.

---

# 10. Important: `await` Works Inside `async`

This:

```js
await Todo.find();
```

requires the surrounding function to be:

```js
async
```

So:

```js
app.get("/todos", async (req, res) => {
    const todos = await Todo.find();

    res.json(todos);
});
```

Not:

```js
app.get("/todos", (req, res) => {
    const todos = await Todo.find(); // ❌
});
```

---

# 11. Async Errors and Express

This is an important real-world detail.

An asynchronous operation can reject:

```js
const todos = await Todo.find();
```

So we need to make sure the error reaches our error-handling system.

A straightforward approach while learning is:

```js
app.get("/todos", async (req, res, next) => {
    try {
        const todos = await Todo.find();

        res.json(todos);
    } catch (error) {
        next(error);
    }
});
```

Then our global error middleware handles it:

```js
app.use((error, req, res, next) => {
    console.error(error);

    res.status(500).json({
        message: "Internal Server Error"
    });
});
```

Flow:

```text
Async operation
      ↓
Error
      ↓
catch
      ↓
next(error)
      ↓
Global error middleware
      ↓
500 response
```

This connects directly to what we learned in the previous lesson.

---

# 12. The Pattern You Should Remember

For now, remember this backend pattern:

```js
app.get("/todos", async (req, res, next) => {
    try {
        const todos = await Todo.find();

        res.json(todos);
    } catch (error) {
        next(error);
    }
});
```

It's essentially:

```text
async
  ↓
try
  ↓
await
  ↓
success → response

error
  ↓
catch
  ↓
next(error)
  ↓
global error handler
```

---

# Phase 13 Progress

```text
[x] Node.js Basics
[x] npm + package.json
[x] Modules
[x] HTTP Server
[x] HTTP Fundamentals
[x] Express
[x] Routing
[x] Middleware
[x] REST API + CRUD
[x] Project Structure
[x] Error Handling
[x] Async/Await in Express

[ ] Database Fundamentals
```

## Next → Database Fundamentals

We'll start from the basics:

```text
What is a database?
        ↓
SQL vs NoSQL
        ↓
Relational vs Document Database
        ↓
Tables / Rows
        ↓
Collections / Documents
        ↓
MongoDB vs PostgreSQL
```

Then we'll choose the database for our Todo backend and start actually connecting Express to it.

----------------------------------------------------------------------------------------------------------------------------------------

# Step 15: Database Fundamentals

Before connecting MongoDB or PostgreSQL to Express, let's understand **what a database actually is** and why backend applications need one.

---

## 1. What Is a Database?

A database is a system used to **store and manage data**.

For our Todo application, we currently have something like:

```js
const todos = [
    {
        id: 1,
        title: "Learn Express",
        completed: false
    },
    {
        id: 2,
        title: "Learn MongoDB",
        completed: false
    }
];
```

This works while the server is running.

But what happens if the server restarts?

```text
Server running
     ↓
todos stored in memory
     ↓
Server crashes/restarts
     ↓
Data disappears ❌
```

That's the problem.

A database gives us **persistent storage**.

```text
Express
   ↓
Database
   ↓
Data is stored permanently
```

---

# 2. Why Does a Backend Need a Database?

Imagine a real application.

A Todo application might have:

```text
Users
Todos
Categories
Tasks
Login information
```

An e-commerce application might have:

```text
Users
Products
Orders
Payments
Reviews
```

We can't realistically keep all this information inside JavaScript variables.

Instead:

```text
Client
  ↓
Express API
  ↓
Database
```

The backend communicates with the database whenever it needs to:

```text
Create data
Read data
Update data
Delete data
```

These are the same CRUD operations we already learned.

---

# 3. Database vs JavaScript Variable

### JavaScript variable

```js
const todos = [];
```

Data exists only while the application is running.

### Database

```text
Database
   ↓
Stored data
   ↓
Server restarts
   ↓
Data still exists
```

So the main difference is:

> **Variables store temporary application data. Databases provide persistent data storage.**

---

# 4. What Does a Database Store?

A database stores information in an organized way.

For example, our Todo application might store:

```text
Todo

id
title
completed
createdAt
```

One Todo could look conceptually like:

```text
id: 1
title: "Learn Express"
completed: false
createdAt: ...
```

Another:

```text
id: 2
title: "Learn MongoDB"
completed: false
createdAt: ...
```

The exact way this information is organized depends on the type of database.

---

# 5. Two Major Types of Databases

You'll commonly hear:

```text
SQL
NoSQL
```

Let's understand the difference.

---

# 6. SQL Databases

SQL databases are generally **relational databases**.

Examples:

* PostgreSQL
* MySQL
* MariaDB
* SQLite
* Microsoft SQL Server

They organize data using:

```text
Database
   ↓
Tables
   ↓
Rows
   ↓
Columns
```

For example:

### Users table

| id | name  | email                                         |
| -: | ----- | --------------------------------------------- |
|  1 | John  | [john@example.com](mailto:john@example.com)   |
|  2 | Alice | [alice@example.com](mailto:alice@example.com) |

### Todos table

| id | title         | completed | user_id |
| -: | ------------- | --------- | ------: |
|  1 | Learn Express | false     |       1 |
|  2 | Learn MongoDB | false     |       1 |
|  3 | Learn SQL     | true      |       2 |

Notice something important:

```text
Users
  ↓
user_id
  ↓
Todos
```

Tables can be related to each other.

That's why they're called **relational databases**.

---

# 7. NoSQL Databases

NoSQL databases use different data models.

One popular type is the **document database**.

The most famous example is:

**MongoDB**

Instead of:

```text
Table
 ↓
Rows
 ↓
Columns
```

MongoDB uses:

```text
Database
   ↓
Collections
   ↓
Documents
```

A document can look similar to JavaScript objects:

```js
{
    title: "Learn Express",
    completed: false
}
```

Another:

```js
{
    title: "Learn MongoDB",
    completed: false
}
```

This is one reason MongoDB can feel comfortable to JavaScript developers.

---

# 8. SQL vs NoSQL

Think of them like this:

### SQL

```text
Database
   ↓
Table
   ↓
Rows
   ↓
Columns
```

### MongoDB / Document NoSQL

```text
Database
   ↓
Collection
   ↓
Documents
```

A simplified comparison:

| SQL         | MongoDB         |
| ----------- | --------------- |
| Database    | Database        |
| Table       | Collection      |
| Row         | Document        |
| Column      | Field           |
| SQL queries | MongoDB queries |

---

# 9. What Is a Table?

In a relational database, a **table** stores a particular type of data.

For example:

```text
Users
```

could contain users.

```text
Products
```

could contain products.

```text
Orders
```

could contain orders.

So:

```text
Database
│
├── Users
├── Products
└── Orders
```

Each table contains rows.

---

# 10. What Is a Row?

A row represents **one record**.

For example:

| id | name  | email                                         |
| -: | ----- | --------------------------------------------- |
|  1 | John  | [john@example.com](mailto:john@example.com)   |
|  2 | Alice | [alice@example.com](mailto:alice@example.com) |

The first row represents one user.

The second row represents another user.

So:

> **Row = one record**

---

# 11. What Is a Column?

A column describes a particular piece of information.

For example:

| id | name  | email                                         |
| -: | ----- | --------------------------------------------- |
|  1 | John  | [john@example.com](mailto:john@example.com)   |
|  2 | Alice | [alice@example.com](mailto:alice@example.com) |

Here:

```text
id
name
email
```

are columns.

So:

> **Column = a particular field/property of the records**

---

# 12. MongoDB Collection

MongoDB doesn't use tables.

It uses **collections**.

For example:

```text
Todo Database
     ↓
todos collection
```

The collection could contain:

```js
{
    title: "Learn Express",
    completed: false
}
```

and:

```js
{
    title: "Learn MongoDB",
    completed: false
}
```

Each object is a **document**.

---

# 13. What Is a Document?

A MongoDB document is a record stored in a collection.

For example:

```js
{
    title: "Learn Express",
    completed: false
}
```

Think of it as being similar to a JavaScript object.

A collection can contain many documents:

```text
todos
│
├── Document 1
├── Document 2
├── Document 3
└── Document 4
```

---

# 14. MongoDB Example

Conceptually:

```text
Database: todoApp

Collection: todos

Document:
{
    title: "Learn Express",
    completed: false
}
```

Another document:

```text
{
    title: "Learn MongoDB",
    completed: true
}
```

So the overall structure is:

```text
MongoDB
   ↓
todoApp database
   ↓
todos collection
   ↓
documents
```

---

# 15. Why MongoDB Is Popular With Node.js

One reason is that MongoDB documents look familiar to JavaScript developers.

JavaScript object:

```js
{
    title: "Learn Express",
    completed: false
}
```

MongoDB document:

```js
{
    title: "Learn Express",
    completed: false
}
```

MongoDB actually stores documents using **BSON**, a binary representation related to JSON.

You don't need to worry about BSON yet.

For now remember:

```text
JavaScript object
       ↓
looks similar to
       ↓
MongoDB document
```

---

# 16. PostgreSQL vs MongoDB

Both are excellent databases, but they approach data differently.

### PostgreSQL

```text
Relational
↓
Tables
↓
Rows
↓
Relationships
```

### MongoDB

```text
Document database
↓
Collections
↓
Documents
```

Neither is simply "better."

The appropriate choice depends on the application's requirements.

For our learning project, **MongoDB is a good next step** because it lets us focus on database concepts without introducing SQL and relational modeling at the same time.

---

# 17. Database + Express

Now let's connect everything we've learned.

Our application will eventually look like:

```text
                Client
                  ↓
              HTTP Request
                  ↓
               Express
                  ↓
               Route
                  ↓
             Controller
                  ↓
              Database
                  ↓
               Result
                  ↓
             Controller
                  ↓
             HTTP Response
                  ↓
                Client
```

For example:

```text
GET /todos
    ↓
Express
    ↓
Todo controller
    ↓
MongoDB
    ↓
Get todos
    ↓
JSON response
```

---

# 18. CRUD With a Database

Remember CRUD?

```text
Create
Read
Update
Delete
```

With our Todo API:

### Create

```text
POST /todos
```

Database:

```text
Create new document
```

### Read

```text
GET /todos
```

Database:

```text
Find documents
```

### Update

```text
PUT /todos/:id
```

Database:

```text
Update document
```

### Delete

```text
DELETE /todos/:id
```

Database:

```text
Delete document
```

So our API and database work together.

---

# 19. The Complete Mental Model

Keep this picture in your head:

```text
                 CLIENT
                    ↓
              HTTP Request
                    ↓
                 EXPRESS
                    ↓
                  ROUTE
                    ↓
               CONTROLLER
                    ↓
                DATABASE
                    ↓
              Data Operation
                    ↓
               CONTROLLER
                    ↓
              HTTP Response
                    ↓
                 CLIENT
```

And when something goes wrong:

```text
Database Error
      ↓
catch
      ↓
next(error)
      ↓
Global Error Handler
      ↓
500 Response
```

This connects **Step 14** and **Step 15** together.

---

# What You Should Know Before Moving On

Make sure these terms are clear:

```text
Database
SQL
NoSQL
Relational Database
Document Database
Table
Row
Column
Collection
Document
MongoDB
PostgreSQL
CRUD
```

The most important comparison:

```text
SQL

Database
   ↓
Table
   ↓
Row
   ↓
Column
```

versus:

```text
MongoDB

Database
   ↓
Collection
   ↓
Document
   ↓
Field
```

---

## Next

**Step 16 — MongoDB Fundamentals**

We'll go one level deeper:

```text
MongoDB
   ↓
Install / MongoDB Atlas
   ↓
Create database
   ↓
Create collection
   ↓
Documents
   ↓
MongoDB Compass
   ↓
Connect MongoDB to Express
```

Then we'll replace our temporary in-memory `todos` array with a **real database**.

---------------------------------------------------------------------------------------------------------------------------------------

# Step 16: MongoDB Fundamentals

Now we move from **database theory** to actually understanding MongoDB.

Our goal is:

```text
Express
   ↓
MongoDB
   ↓
todos collection
   ↓
Todo documents
```

---

## 1. What Is MongoDB?

MongoDB is a **NoSQL document database**.

Instead of storing data like:

```text
Table
 ├── Row
 ├── Row
 └── Row
```

MongoDB uses:

```text
Database
   ↓
Collection
   ↓
Document
```

For our Todo application:

```text
todoApp
   ↓
todos
   ↓
documents
```

---

# 2. MongoDB Document

A Todo document could look like:

```js
{
    title: "Learn Express",
    completed: false
}
```

Another:

```js
{
    title: "Learn MongoDB",
    completed: true
}
```

So:

```text
todos collection
       ↓
 ┌───────────────┐
 │ Todo document │
 ├───────────────┤
 │ Todo document │
 ├───────────────┤
 │ Todo document │
 └───────────────┘
```

---

# 3. MongoDB Automatically Gives Documents an ID

When you create a MongoDB document, MongoDB normally gives it an `_id`.

For example:

```js
{
    _id: "some unique id",
    title: "Learn Express",
    completed: false
}
```

The actual `_id` is not normally a simple number like:

```text
1
2
3
```

MongoDB commonly uses an **ObjectId**.

You don't need to memorize how ObjectId works yet.

Just remember:

> `_id` uniquely identifies a document.

---

# 4. Collection

A collection is similar to a table in SQL.

For our project:

```text
Database
   ↓
todoApp
   ↓
todos collection
```

The `todos` collection contains Todo documents.

Example:

```text
todos

Document 1
{
    title: "Learn Express",
    completed: false
}

Document 2
{
    title: "Learn MongoDB",
    completed: false
}
```

---

# 5. Database

A MongoDB database contains collections.

For example:

```text
todoApp
│
├── todos
├── users
└── categories
```

Each collection contains documents.

So the hierarchy is:

```text
MongoDB Server
      ↓
   Database
      ↓
  Collection
      ↓
   Document
      ↓
    Fields
```

---

# 6. What Is a Field?

A field is a property inside a MongoDB document.

Example:

```js
{
    title: "Learn MongoDB",
    completed: false
}
```

Here:

```text
title
completed
```

are fields.

Similar to properties in a JavaScript object.

---

# 7. MongoDB and JavaScript

This is one reason MongoDB is convenient for Node.js developers.

JavaScript:

```js
const todo = {
    title: "Learn Express",
    completed: false
};
```

MongoDB document:

```js
{
    title: "Learn Express",
    completed: false
}
```

The syntax looks very familiar.

But remember:

> A MongoDB document is stored by MongoDB; a JavaScript object exists inside your application.

---

# 8. MongoDB Atlas

There are two common ways you'll encounter MongoDB.

### Local MongoDB

MongoDB runs on your computer.

```text
Your computer
     ↓
MongoDB
```

### MongoDB Atlas

MongoDB runs in the cloud.

```text
Your computer
     ↓
Internet
     ↓
MongoDB Atlas
     ↓
Database
```

For learning backend development, **MongoDB Atlas** is convenient because you don't need to maintain the database server yourself.

---

# 9. MongoDB Compass

Another useful tool is **MongoDB Compass**.

It provides a graphical interface for looking at your database.

Conceptually:

```text
MongoDB
   ↑
   │
Compass
```

You can visually inspect:

```text
Databases
Collections
Documents
Fields
```

For example:

```text
todoApp
   ↓
todos
   ↓
Documents
```

This is useful while learning because you can actually see the data your Express application creates.

---

# 10. Express + MongoDB

Eventually our application will look like this:

```text
             Client
                ↓
             Express
                ↓
              Route
                ↓
           Controller
                ↓
             MongoDB
                ↓
            Collection
                ↓
             Document
```

For example:

```text
GET /todos
     ↓
Express
     ↓
Todo controller
     ↓
MongoDB
     ↓
todos collection
     ↓
return documents
     ↓
JSON response
```

---

# 11. CRUD in MongoDB

The CRUD operations we learned earlier map directly to database operations.

### Create

```text
POST /todos
     ↓
Create document
```

### Read

```text
GET /todos
     ↓
Find documents
```

### Update

```text
PUT /todos/:id
     ↓
Update document
```

### Delete

```text
DELETE /todos/:id
     ↓
Delete document
```

So our previous REST API knowledge is still being used.

---

# 12. MongoDB Is Not the Same as Mongoose

This distinction is **very important**.

You will hear both:

```text
MongoDB
Mongoose
```

They are not the same thing.

### MongoDB

The actual database.

```text
Your application
       ↓
    MongoDB
       ↓
     Data
```

### Mongoose

A Node.js library that helps your application work with MongoDB.

```text
Express
   ↓
Mongoose
   ↓
MongoDB
```

Mongoose provides things like:

```text
Schemas
Models
Validation
Query methods
```

We'll learn those shortly.

---

# 13. Why Use Mongoose?

Suppose we want every Todo to have:

```text
title
completed
```

We can define a structure using a Mongoose schema.

Conceptually:

```text
Todo
│
├── title → String
└── completed → Boolean
```

Then we create a model based on that schema.

```text
Schema
  ↓
Model
  ↓
MongoDB collection
```

We'll build this properly in the next steps.

---

# 14. Important Architecture

Eventually your Todo backend will look something like:

```text
Client
   ↓
Express
   ↓
Route
   ↓
Controller
   ↓
Mongoose Model
   ↓
MongoDB
```

For example:

```text
GET /todos
     ↓
todo route
     ↓
todo controller
     ↓
Todo model
     ↓
MongoDB
     ↓
todos collection
```

This is a very common backend architecture.

------------------------------------------------------------------------------------------------------------------------------------------

# Step 17 — PostgreSQL Basics

Learn what PostgreSQL is and how relational databases work.

Topics:

```text
Database
Tables
Rows
Columns
Primary Keys
Foreign Keys
Relationships
SQL basics
```

Example:

```text
Users Table

id | name  | email
---|-------|----------------
1  | John  | john@email.com
2  | Jane  | jane@email.com
```

The main goal:

> Understand how relational databases store structured data.

---

# Step 18 — Install PostgreSQL + Setup Database

Here you actually install PostgreSQL and create a database.

Example concept:

```text
PostgreSQL Server
      ↓
Create Database
      ↓
my_app_database
      ↓
Create Tables
```

You learn:

```text
How PostgreSQL runs locally
Database creation
Connecting to the database
Basic database management
```

---

# Step 19 — Prisma ORM Introduction

Prisma is an ORM.

ORM means:

> Object Relational Mapper.

Instead of manually writing SQL everywhere:

```sql
SELECT * FROM users;
```

You can use Prisma:

```js
const users = await prisma.user.findMany();
```

Architecture:

```text
Express
   ↓
Prisma ORM
   ↓
PostgreSQL
```

Prisma acts as a bridge between JavaScript and PostgreSQL.

---

# Step 20 — Connect Prisma to PostgreSQL

Now you connect:

```text
Node.js Application
       ↓
Prisma
       ↓
PostgreSQL Database
```

Usually using an environment variable:

```text
DATABASE_URL
```

Conceptually:

```text
DATABASE_URL
       ↓
Prisma Configuration
       ↓
PostgreSQL Connection
```

At this point, your Node.js application can communicate with PostgreSQL.

---

# Step 21 — Database Models / Schema

You define your database structure.

For example, a User:

```text
User

id
name
email
password
```

In Prisma, you define a model conceptually like:

```text
User Model
    ↓
Prisma Schema
    ↓
PostgreSQL Table
```

Another example:

```text
User
  │
  └── has many
          │
          ▼
        Todos
```

This is where you define:

```text
Models
Fields
Data types
Primary keys
Relationships
```

---

# Step 22 — CRUD with Prisma

Now you perform database operations.

CRUD:

```text
Create
Read
Update
Delete
```

Using Prisma:

### Create

```text
Create User
```

### Read

```text
Find Users
```

### Update

```text
Update User
```

### Delete

```text
Delete User
```

Architecture:

```text
Express Controller
       ↓
Prisma Query
       ↓
PostgreSQL
       ↓
Result
       ↓
Response
```

---

# Step 23 — Connect Express + PostgreSQL

This combines everything.

Before:

```text
Express
   ↓
Temporary Array
```

After:

```text
React Frontend
      ↓
Express API
      ↓
Controller
      ↓
Prisma
      ↓
PostgreSQL
```

Example:

```text
GET /users
     ↓
Express Route
     ↓
Controller
     ↓
Prisma
     ↓
PostgreSQL
     ↓
Users returned
     ↓
JSON Response
```

This is where your backend becomes a real database-powered API.

---

# Important Clarification

Your current completed roadmap says:

```text
[x] Step 16 — Introduction to Databases
[x] Step 17 — MongoDB Basics
[x] Step 18 — MongoDB + Mongoose
[x] Step 19 — Mongoose Models and Schemas
[x] Step 20 — Database CRUD Operations
[x] Step 21 — Relationships and Data Modeling
```

That means you have already completed the **MongoDB/Mongoose database path**.

The PostgreSQL/Prisma path is an **alternative database stack**, not something we must complete before Authentication.

-----------------------------------------------------------------------------------------------------------------------------------------

# Authentication Concepts

Authentication is one of the most important parts of backend development.

Before learning **bcrypt** and **JWT**, we need to understand exactly what authentication is and how a login system works.

---

## 1. What is Authentication?

**Authentication = verifying who the user is.**

For example, you enter:

```text
Email: alice@gmail.com
Password: 123456
```

The server checks:

> "Is this really Alice?"

If the credentials are valid:

```text
Authentication successful
```

If not:

```text
Authentication failed
```

### Simple mental model

```text
User
 ↓
"I am Alice"
 ↓
Server
 ↓
"Prove it"
 ↓
Email + Password
 ↓
Server verifies
 ↓
"Yes, you are Alice"
```

---

# 2. Authentication vs Authorization

These two are often confused.

### Authentication

**Who are you?**

```text
Login with email + password
```

### Authorization

**What are you allowed to do?**

For example:

```text
Alice → normal user
Admin → administrator
```

Alice might be allowed to:

```text
GET /todos
POST /todos
DELETE /todos/:id
```

But she might **not** be allowed to:

```text
DELETE /users/:id
```

An admin might be allowed to do that.

So:

```text
Authentication
      ↓
Who are you?
      ↓
Authorization
      ↓
What can you do?
```

---

# 3. Why Do We Need Authentication?

Imagine our Todo API has:

```text
GET /todos
POST /todos
DELETE /todos/:id
```

Without authentication, anyone could potentially access another user's todos.

For example:

```text
User A
  ↓
GET /todos
  ↓
Server
  ↓
All todos
```

That's a problem.

With authentication:

```text
User A
  ↓
Login
  ↓
Authentication
  ↓
Identity established
  ↓
GET /todos
  ↓
Only User A's todos
```

---

# 4. How Does Login Work?

Let's understand the complete flow.

### Step 1 — User registers

```text
POST /register
```

Request:

```json
{
  "email": "alice@gmail.com",
  "password": "mypassword"
}
```

Server creates the user in MongoDB.

But importantly:

**We should never store the plain password.**

We'll learn password hashing with **bcrypt in Step 23**.

---

### Step 2 — User logs in

```text
POST /login
```

Request:

```json
{
  "email": "alice@gmail.com",
  "password": "mypassword"
}
```

Server:

```text
Find user
   ↓
Compare password
   ↓
Password correct?
   ↓
YES
   ↓
Create authentication token
```

We'll learn the token part in **Step 24 — JWT Authentication**.

---

# 5. What Happens After Login?

Suppose the server gives the client a JWT:

```text
JWT TOKEN
```

The client stores it and sends it with future requests.

For example:

```http
GET /todos
Authorization: Bearer <token>
```

The request flow becomes:

```text
React
  ↓
GET /todos
  +
JWT token
  ↓
Express
  ↓
Authentication Middleware
  ↓
Verify JWT
  ↓
Who is this user?
  ↓
Controller
  ↓
MongoDB
  ↓
User's todos
```

This is the architecture we'll build.

---

# 6. What is a Token?

A token is essentially a piece of information that the server can use to recognize an authenticated user.

Think of it like a **temporary access pass**.

Imagine entering an office:

```text
You → show ID → Reception
                  ↓
             Verify identity
                  ↓
             Give access badge
```

Then when you enter different rooms:

```text
You + Badge
     ↓
Room     
     ↓
Access granted
```

JWT works somewhat like that:

```text
Login
 ↓
Server verifies credentials
 ↓
Server creates JWT
 ↓
Client receives JWT
 ↓
Client sends JWT with requests
 ↓
Server verifies JWT
 ↓
Access granted
```

---

# 7. Session vs Token Authentication

There are two common approaches.

## Session-based authentication

The server keeps track of the user's session.

```text
Client
 ↓
Login
 ↓
Server
 ↓
Session created
 ↓
Session ID → Client
```
                            
Later:

```text
Client
 ↓
Session ID
 ↓
Server
 ↓
Find session
 ↓
Identify user
```

---

## Token-based authentication

The server gives the client a token.

```text
Client
 ↓
Login
 ↓
Server
 ↓
JWT
 ↓
Client
```

Later:

```text
Client
 ↓
JWT
 ↓
Server
 ↓
Verify JWT
 ↓
Identify user
```

We'll focus on **JWT authentication** for this roadmap.

---

# 8. Authentication Flow We'll Build

Our final Todo backend will look approximately like this:

```text
                 REGISTER
                    ↓
                  User
                    ↓
                MongoDB
```

Then:

```text
                  LOGIN
                    ↓
             Email + Password
                    ↓
               bcrypt check
                    ↓
                JWT created
                    ↓
                 Client
```

Then protected requests:

```text
Client
  ↓
JWT
  ↓
Express
  ↓
Authentication Middleware
  ↓
JWT verified
  ↓
req.user
  ↓
Controller
  ↓
MongoDB
```

---

# 9. Authentication Middleware

This is going to become very important in **Step 25**.

For example:

```js
app.get("/todos", authenticateUser, getTodos);
```

The middleware:

```text
Request
   ↓
authenticateUser
   ↓
Is JWT valid?
   ↓
   ├── NO → 401 Unauthorized
   │
   └── YES
        ↓
      Controller
        ↓
      Todos
```

The controller doesn't need to worry about how the user was authenticated.

That's the benefit of middleware.

---

# 10. 401 vs 403

You should remember this distinction.

### 401 Unauthorized

Usually means:

> "You haven't successfully authenticated."

Example:

```text
No token
Invalid token
Expired token
```

### 403 Forbidden

Means:

> "I know who you are, but you're not allowed to do this."

Example:

```text
User → tries admin-only endpoint
```

So:

```text
401 → Who are you?
403 → I know who you are, but you're not allowed.
```

---

# 11. The Big Picture

Keep this mental model:

```text
AUTHENTICATION
      ↓
"Who are you?"
      ↓
Login
      ↓
Password verification
      ↓
JWT
      ↓
Authentication Middleware
      ↓
User identified
      ↓
AUTHORIZATION
      ↓
"What are you allowed to do?"
```

And our upcoming steps are:

```text
[→] Step 22 — Authentication Concepts
[ ] Step 23 — Password Hashing with bcrypt
[ ] Step 24 — JWT Authentication
[ ] Step 25 — Authentication Middleware
[ ] Step 26 — Protected Routes
[ ] Step 27 — Authorization
```

### Next

**Step 23 — Password Hashing with bcrypt**

We'll learn **why passwords must never be stored directly**, what hashing means, and how `bcrypt.hash()` and `bcrypt.compare()` work.

-----------------------------------------------------------------------------------------------------------------------------------------

# Step 24 — Password Hashing with bcrypt

Before JWT, we need to understand how passwords are safely stored.

## 1. Never Store Plain Passwords

Bad:

```text
users

email: alice@gmail.com
password: mypassword123
```

If the database is compromised, the actual passwords are exposed.

Instead, we store a **hash**:

```text
email: alice@gmail.com
password: $2b$10$...
```

The original password is not stored.

---

## 2. What Is Hashing?

Hashing converts data into another value.

Conceptually:

```text
Password
   ↓
Hash function
   ↓
Hash
```

For example:

```text
"mypassword"
      ↓
   bcrypt
      ↓
"$2b$10$...."
```

The important idea:

> Hashing is designed to be one-way.

You don't normally take the hash and turn it back into the original password.

---

## 3. Why bcrypt?

**bcrypt** is a password-hashing algorithm designed specifically for storing passwords securely.

Our flow will be:

```text
User enters password
        ↓
      bcrypt
        ↓
      Hash
        ↓
    MongoDB
```

---

## 4. Registration

When a user registers:

```text
POST /register
        ↓
Email + Password
        ↓
Hash password
        ↓
Save user
        ↓
MongoDB
```

Suppose the user enters:

```text
mypassword123
```

We do **not** save:

```text
mypassword123
```

We save its bcrypt hash.

---

## 5. bcrypt.hash()

bcrypt provides:

```js
bcrypt.hash(password, saltRounds)
```

Conceptually:

```js
const hashedPassword = await bcrypt.hash(
    password,
    10
);
```

Here:

```text
password
   ↓
bcrypt.hash()
   ↓
hashedPassword
```

The `10` represents the **cost factor / salt rounds**.

You don't need to memorize the mathematical details yet.

---

## 6. What Is Salt?

bcrypt uses a **salt** when hashing passwords.

This means two users can have the same password but receive different hashes.

For example:

```text
User A:
password → bcrypt → hash A

User B:
password → bcrypt → hash B
```

Even though:

```text
password = password
```

the hashes can be different.

This is an important security property.

---

## 7. Login

Now suppose Alice logs in:

```text
Email: alice@gmail.com
Password: mypassword123
```

The server:

```text
Find user
    ↓
Get stored password hash
    ↓
bcrypt.compare()
    ↓
Password matches?
```

We don't decrypt the stored hash.

Instead, bcrypt compares the entered password against the stored hash.

---

## 8. bcrypt.compare()

Conceptually:

```js
const isValid = await bcrypt.compare(
    password,
    user.password
);
```

Where:

```text
password
    ↓
password entered during login

user.password
    ↓
hash stored in database
```

Result:

```text
true
```

or:

```text
false
```

---

## 9. Complete Authentication Flow

### Registration

```text
User
 ↓
Email + Password
 ↓
bcrypt.hash()
 ↓
Password Hash
 ↓
MongoDB
```

### Login

```text
User
 ↓
Email + Password
 ↓
Find User
 ↓
bcrypt.compare()
 ↓
 ┌─────────────┐
 │             │
Match       No Match
 │             │
 ↓             ↓
Success       401
```

---

## 10. Why We Don't Hash Again and Compare Strings

You might think:

```text
User password
      ↓
Hash again
      ↓
Compare with stored hash
```

But bcrypt uses a salt, so hashing the same password again can produce a different hash.

Instead:

```text
Entered password
       ↓
bcrypt.compare()
       ↓
Stored bcrypt hash
```

bcrypt knows how to perform the comparison correctly.

---

## 11. Important Security Rule

Never return the password hash to the client.

Bad response:

```json
{
  "email": "alice@gmail.com",
  "password": "$2b$10$..."
}
```

Even though it's hashed, there's no reason to expose it.

Better:

```json
{
  "id": "...",
  "email": "alice@gmail.com"
}
```

---

## 12. Registration vs Login

Remember the difference:

### Registration

```text
Plain password
      ↓
bcrypt.hash()
      ↓
Hash
      ↓
Database
```

### Login

```text
Plain password entered
      ↓
bcrypt.compare()
      ↓
Stored hash
      ↓
true / false
```

---

# Mental Model

```text
              REGISTER
                  ↓
             Password
                  ↓
             bcrypt.hash
                  ↓
           Password Hash
                  ↓
              MongoDB


               LOGIN
                  ↓
             Password
                  ↓
           bcrypt.compare
                  ↓
          Stored Password Hash
                  ↓
             true / false
```

The two functions you should remember are:

```js
bcrypt.hash()
```

**Create a password hash.**

```js
bcrypt.compare()
```

**Check a password against its stored hash.**

----------------------------------------------------------------------------------------------------------------------------------------

# Step 25 — JWT Authentication

Now that we understand password hashing, we can move to the next part of authentication: **JWT**.

JWT stands for **JSON Web Token**.

Its main purpose in our application is to let the server recognize a user after they successfully log in.

---

## 1. Why Do We Need JWT?

Suppose Alice logs in:

```text
Email: alice@gmail.com
Password: mypassword
```

The server verifies:

```text
Email exists?
       ↓
Password correct?
       ↓
YES
```

Now the server needs some way to recognize Alice on future requests.

That's where JWT comes in.

```text
Login
  ↓
Credentials verified
  ↓
JWT created
  ↓
Client receives JWT
```

Later:

```text
Client
  ↓
JWT
  ↓
Server
  ↓
Verify JWT
  ↓
Identify user
```

---

# 2. JWT Is a Token

Think of it like an access pass.

```text
Login
   ↓
Identity verified
   ↓
Access pass issued
   ↓
Client keeps the pass
   ↓
Client presents it on future requests
```

In our backend:

```text
Password
   ↓
bcrypt verification
   ↓
JWT
   ↓
Future requests
```

---

# 3. What Does a JWT Look Like?

A JWT usually looks like a long string:

```text
xxxxx.yyyyy.zzzzz
```

There are **three parts**, separated by dots:

```text
Header.Payload.Signature
```

So:

```text
Header
  .
Payload
  .
Signature
```

You don't need to memorize the encoded format.

Just remember the three components.

---

# 4. Header

The header contains information about the token.

Conceptually:

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

It tells the system things such as:

```text
What type of token is this?
Which signing algorithm is being used?
```

---

# 5. Payload

The payload contains **claims**.

For example:

```json
{
  "userId": "123",
  "email": "alice@gmail.com"
}
```

The server can use these claims to identify the user.

You may also see claims such as:

```text
sub
iat
exp
```

For now:

```text
sub → subject / commonly user identifier
iat → issued-at time
exp → expiration time
```

---

# 6. Important: JWT Payload Is Not Secret

This is extremely important.

A JWT payload is **encoded**, not automatically encrypted.

Therefore, don't put sensitive information inside it.

Don't put:

```text
Password
Credit card information
Private secrets
```

A JWT payload might contain:

```text
User ID
Role
Basic identity information
Expiration
```

The signature protects the token's integrity, but it doesn't make the payload confidential.

---

# 7. Signature

The third part is the signature.

Conceptually:

```text
Header
+
Payload
+
Secret
      ↓
Signature
```

The server uses a secret/key to sign the token.

Later, when the token comes back:

```text
JWT
 ↓
Verify signature
 ↓
Valid?
```

If the token has been modified, verification should fail.

---

# 8. Complete JWT Structure

So:

```text
JWT
│
├── Header
│
├── Payload
│
└── Signature
```

Or:

```text
Header.Payload.Signature
```

Think of it as:

```text
Header
→ How the token is structured

Payload
→ Information/claims about the user

Signature
→ Helps verify the token wasn't altered
```

---

# 9. Login Flow With bcrypt + JWT

Now we can combine Step 23 and Step 24.

User logs in:

```text
POST /login
     ↓
Email + Password
     ↓
Find user in MongoDB
     ↓
bcrypt.compare()
     ↓
Password correct?
```

If incorrect:

```text
false
 ↓
401 Unauthorized
```

If correct:

```text
true
 ↓
Create JWT
 ↓
Send JWT to client
```

Complete flow:

```text
             LOGIN
                ↓
        Email + Password
                ↓
          Find User
                ↓
        bcrypt.compare()
                ↓
        ┌───────┴───────┐
        ↓               ↓
      Wrong           Correct
        ↓               ↓
      401          Create JWT
                        ↓
                     Client
```

---

# 10. What Happens on the Next Request?

Suppose Alice wants her todos:

```text
GET /todos
```

The client sends the JWT with the request.

Commonly:

```text
Authorization: Bearer <JWT>
```

The server receives:

```text
Request
   ↓
Authorization header
   ↓
Extract JWT
   ↓
Verify JWT
   ↓
Identify Alice
   ↓
Continue to controller
```

---

# 11. Bearer Token

You will commonly see:

```text
Authorization: Bearer <token>
```

There are two parts:

```text
Authorization
      ↓
Bearer <token>
```

`Bearer` indicates that the client is presenting the token as its credential.

You don't need to manually create this format every time in your application logic; libraries and HTTP clients can help with it.

---

# 12. JWT Verification

The server doesn't simply trust whatever token the client sends.

It verifies it.

```text
Client
  ↓
JWT
  ↓
Server
  ↓
Verify signature
  ↓
Check claims
  ↓
Valid?
```

If valid:

```text
Continue
```

If invalid:

```text
401 Unauthorized
```

---

# 13. JWT Expiration

JWTs can have an expiration time.

For example:

```text
Token created
     ↓
Valid for a certain period
     ↓
Expiration reached
     ↓
Token rejected
```

This is useful because an authentication token shouldn't necessarily remain valid forever.

The `exp` claim represents expiration.

---

# 14. JWT Is Not the Same as Encryption

This is another important distinction.

### Encryption

```text
Readable data
     ↓
Encryption
     ↓
Unreadable data
```

It is designed to provide confidentiality.

### JWT

A standard signed JWT is generally:

```text
Header
+
Payload
+
Signature
```

The payload can be decoded.

The signature is used to verify authenticity/integrity.

Therefore:

> **Don't put secrets or passwords in a JWT payload.**

---

# 15. JWT + Express

Eventually we'll have something like:

```text
Client
  ↓
POST /login
  ↓
Express
  ↓
Controller
  ↓
MongoDB
  ↓
bcrypt
  ↓
JWT created
  ↓
Client
```

Then:

```text
Client
  ↓
GET /todos
  +
JWT
  ↓
Express
  ↓
Authentication Middleware
  ↓
Verify JWT
  ↓
req.user
  ↓
Todo Controller
  ↓
MongoDB
```

The authentication middleware will be covered in **Step 25**.

---

# 16. JWT vs Password

Don't confuse these two.

### Password

Used during login:

```text
Email + Password
       ↓
bcrypt.compare()
```

### JWT

Used after successful login:

```text
JWT
 ↓
Future requests
 ↓
Server identifies user
```

So:

```text
Password
   ↓
Prove identity during login
```

and:

```text
JWT
   ↓
Carry authentication state for subsequent requests
```

---

# 17. The Complete Authentication Picture

At this point:

```text
                 REGISTER
                    ↓
                 Password
                    ↓
               bcrypt.hash()
                    ↓
              Password Hash
                    ↓
                 MongoDB
```

Then:

```text
                   LOGIN
                     ↓
              Email + Password
                     ↓
                MongoDB
                     ↓
             bcrypt.compare()
                     ↓
                 Correct?
                     ↓
                    YES
                     ↓
                Create JWT
                     ↓
                  Client
```

Then:

```text
                PROTECTED REQUEST
                       ↓
                     JWT
                       ↓
                    Express
                       ↓
             Authentication Middleware
                       ↓
                 Verify JWT
                       ↓
                   req.user
                       ↓
                  Controller
                       ↓
                   MongoDB
```

---

# 18. What We Have Completed

```text
[x] Step 22 — Authentication Concepts
[x] Step 23 — Password Hashing with bcrypt
[x] Step 24 — JWT Authentication

[ ] Step 25 — Authentication Middleware
[ ] Step 26 — Protected Routes
[ ] Step 27 — Authorization
```

That is where JWT starts becoming a real part of our Express application.

----------------------------------------------------------------------------------------------------------------------------------------


# Step 26 — Authentication Middleware

Now we take the JWT concept from Step 24 and use it inside **Express middleware**.

The main goal is:

```text
Request
   ↓
Authentication Middleware
   ↓
Is the JWT valid?
   ↓
   ├── No → 401 Unauthorized
   │
   └── Yes
        ↓
     Controller
```

---

## 1. What Is Authentication Middleware?

Middleware is code that runs **between the request and the final route handler**.

We already learned middleware:

```text
Request
   ↓
Middleware
   ↓
Route
   ↓
Response
```

Authentication middleware adds a security check:

```text
Request
   ↓
Authentication Middleware
   ↓
Verify JWT
   ↓
Route
```

---

# 2. Why Do We Need It?

Suppose we have:

```text
GET /todos
```

We don't want just anyone accessing private Todo data.

We want:

```text
GET /todos
     ↓
Is user authenticated?
     ↓
YES → Continue
NO  → 401
```

Instead of putting authentication logic inside every controller, we create reusable middleware.

---

# 3. The Authorization Header

The client commonly sends the JWT in:

```text
Authorization: Bearer <token>
```

For example:

```text
GET /todos

Authorization: Bearer eyJhbGciOi...
```

Express gives us access to request headers through:

```text
req.headers
```

So the authentication middleware needs to:

```text
1. Get Authorization header
2. Extract token
3. Verify token
4. Identify user
5. Continue
```

---

# 4. Authentication Middleware Flow

The complete process is:

```text
Client
   ↓
Request + JWT
   ↓
Express
   ↓
Authentication Middleware
   ↓
Get Authorization header
   ↓
Extract JWT
   ↓
Verify JWT
   ↓
 ┌───────────────┐
 │               │
Invalid         Valid
 │               │
 ↓               ↓
401          Identify user
                 ↓
              req.user
                 ↓
               next()
```

---

# 5. What Happens If There Is No Token?

Suppose the client sends:

```text
GET /todos
```

but doesn't provide:

```text
Authorization: Bearer <token>
```

The middleware should reject the request.

```text
Request
   ↓
No token
   ↓
401 Unauthorized
```

Example response:

```json
{
    "message": "Authentication required"
}
```

---

# 6. What If the Token Is Invalid?

Suppose the client sends a fake or modified token.

```text
Request
   ↓
JWT
   ↓
Verify JWT
   ↓
Invalid
   ↓
401 Unauthorized
```

The controller should **not** run.

That's important.

```text
Invalid JWT
    ↓
401
    ↓
STOP
```

---

# 7. What If the Token Is Valid?

If the JWT is valid:

```text
Request
   ↓
JWT
   ↓
Verify
   ↓
Valid
   ↓
Identify user
   ↓
next()
   ↓
Controller
```

The middleware can attach information about the authenticated user to the request.

For example:

```text
req.user
```

Conceptually:

```text
req.user = {
    userId: "123"
}
```

Now the controller knows which user made the request.

---

# 8. Why `req.user` Is Useful

Imagine:

```text
GET /todos
```

The JWT tells us:

```text
userId = 123
```

The middleware puts that information into:

```text
req.user
```

Then the controller can use:

```text
req.user.userId
```

to find that user's Todo records.

Flow:

```text
JWT
 ↓
Verify
 ↓
userId
 ↓
req.user
 ↓
Controller
 ↓
MongoDB
 ↓
User's todos
```

---

# 9. Middleware Doesn't Send the Successful Response

This is an important concept.

The authentication middleware's job is generally:

```text
Check authentication
```

It isn't responsible for returning the Todo data.

So:

```text
Authentication Middleware
        ↓
"User is valid"
        ↓
next()
        ↓
Todo Controller
        ↓
res.json(todos)
```

The middleware controls whether the request is allowed to continue.

---

# 10. `next()` vs `next(error)`

We learned this in error handling.

### Successful middleware

```text
next()
```

means:

> Continue to the next middleware/handler.

### Error

```text
next(error)
```

means:

> Send the error through Express's error-handling system.

Authentication failures are commonly handled directly with a `401` response.

Unexpected errors can be passed to the global error handler.

---

# 11. Protecting a Route

Suppose we have:

```text
GET /todos
```

We can put authentication middleware before the controller:

```text
GET /todos
    ↓
authenticateUser
    ↓
getTodos
```

So the route becomes:

```text
Request
   ↓
authenticateUser
   ↓
getTodos
```

Without authentication:

```text
Request
   ↓
getTodos
```

That means anyone could potentially reach the controller.

---

# 12. Multiple Protected Routes

We may have:

```text
GET    /todos
POST   /todos
PUT    /todos/:id
DELETE /todos/:id
```

All of them might require authentication.

So:

```text
             authenticateUser
                    ↓
       ┌────────────┼────────────┐
       ↓            ↓            ↓
   GET /todos   POST /todos   DELETE /todos/:id
```

The same middleware can be reused.

This is one of the major benefits of Express middleware.

---

# 13. Authentication Middleware vs Authorization

Don't mix these up.

Authentication middleware asks:

```text
"Who is this user?"
```

Authorization asks:

```text
"Is this user allowed to perform this action?"
```

For example:

```text
JWT
 ↓
Authentication
 ↓
User = Alice
 ↓
Authorization
 ↓
Is Alice an admin?
 ↓
YES/NO
```

We'll cover authorization in **Step 27**.

---

# 14. 401 in Authentication

If authentication fails:

```text
401 Unauthorized
```

Typical cases:

```text
No token
Invalid token
Expired token
Malformed token
```

Flow:

```text
Request
   ↓
Authentication Middleware
   ↓
JWT invalid
   ↓
401
```

The protected controller should not execute.

---

# 15. Authentication Middleware Architecture

Our backend is now becoming:

```text
                    CLIENT
                       ↓
                  HTTP Request
                       ↓
                    EXPRESS
                       ↓
           Authentication Middleware
                       ↓
                  Verify JWT
                       ↓
                    req.user
                       ↓
                    ROUTE
                       ↓
                 CONTROLLER
                       ↓
                   MONGODB
```

This is a very common backend pattern.

---

# 16. Putting Everything Together

We've now connected the previous three steps:

### Step 23 — bcrypt

```text
Password
   ↓
bcrypt.hash()
   ↓
Hash
   ↓
MongoDB
```

### Step 24 — JWT

```text
Login
   ↓
bcrypt.compare()
   ↓
Password correct
   ↓
Create JWT
   ↓
Client
```

### Step 25 — Authentication Middleware

```text
Client
   ↓
JWT
   ↓
Authentication Middleware
   ↓
Verify JWT
   ↓
req.user
   ↓
Controller
```

So the overall authentication system is:

```text
REGISTER
   ↓
Hash password
   ↓
Store user


LOGIN
   ↓
Compare password
   ↓
Create JWT
   ↓
Return JWT


PROTECTED REQUEST
   ↓
Send JWT
   ↓
Authentication Middleware
   ↓
Verify JWT
   ↓
Identify user
   ↓
Controller
```

---

# 17. Current Roadmap

```text
[x] Step 22 — Authentication Concepts
[x] Step 23 — Password Hashing with bcrypt
[x] Step 24 — JWT Authentication
[x] Step 25 — Authentication Middleware

[ ] Step 26 — Protected Routes
[ ] Step 27 — Authorization (Roles/Permissions)
```

---------------------------------------------------------------------------------------------------------------------------------------

# Step 27 — Protected Routes

Now we apply the authentication middleware to **actual routes**.

---

## 1. What is a Protected Route?

A **protected route** is an API route that can only be accessed by an authenticated user.

### Public route

Anyone can access it:

```text
POST /register
POST /login
```

### Protected route

User must have a valid JWT:

```text
GET /todos
POST /todos
PUT /todos/:id
DELETE /todos/:id
```

The basic idea:

```text
Request
   ↓
Authentication Middleware
   ↓
Valid JWT?
   ↓
Yes → Controller
No  → 401 Unauthorized
```

---

# 2. Applying Middleware to a Route

Suppose we have:

```js
router.get("/todos", authenticateUser, getTodos);
```

The order is important:

```text
GET /todos
     ↓
authenticateUser
     ↓
getTodos
```

The controller only runs if authentication succeeds.

---

# 3. What Happens Inside the Middleware?

The middleware verifies the JWT and creates something like:

```js
req.user = {
  id: "123"
};
```

Then:

```js
next();
```

allows the request to continue.

So the controller can access:

```js
req.user.id
```

---

# 4. Example Controller

Imagine we have a Todo collection.

Instead of getting **every user's todos**, we use the authenticated user's ID:

```js
const getTodos = async (req, res) => {
  const todos = await Todo.find({
    userId: req.user.id
  });

  res.json(todos);
};
```

This is very important.

The JWT tells us:

```text
Who is making the request?
        ↓
req.user.id
        ↓
Find that user's data
```

---

# 5. Creating a Todo

When an authenticated user creates a todo:

```js
const createTodo = async (req, res) => {
  const todo = await Todo.create({
    title: req.body.title,
    userId: req.user.id
  });

  res.status(201).json(todo);
};
```

The server assigns the `userId` from the authenticated user.

We should **not trust the client** to tell us which user owns the todo.

For example, don't depend on:

```json
{
  "title": "Learn Node.js",
  "userId": "someone-else"
}
```

Instead:

```text
JWT
 ↓
authenticateUser
 ↓
req.user.id
 ↓
create Todo with that ID
```

---

# 6. Protecting Multiple Routes

We can protect each route:

```js
router.get("/todos", authenticateUser, getTodos);

router.post("/todos", authenticateUser, createTodo);

router.put("/todos/:id", authenticateUser, updateTodo);

router.delete("/todos/:id", authenticateUser, deleteTodo);
```

Now all four routes require authentication.

---

# 7. Router-Level Middleware

If an entire router should be protected, middleware can be applied to the router:

```js
router.use(authenticateUser);
```

Then:

```js
router.get("/todos", getTodos);
router.post("/todos", createTodo);
router.put("/todos/:id", updateTodo);
router.delete("/todos/:id", deleteTodo);
```

All of them automatically use:

```text
authenticateUser
```

This is useful when **most or all routes in that router are protected**.

---

# 8. Important Difference

Authentication protects the **route**.

But we also need to protect the **data**.

For example:

```text
User A
 ↓
GET /todos
 ↓
JWT identifies User A
 ↓
Find todos where userId = User A
```

User A should not receive User B's todos.

So this:

```js
Todo.find()
```

could return everyone's todos.

Instead:

```js
Todo.find({ userId: req.user.id })
```

returns only the authenticated user's todos.

---

# 9. PUT and DELETE Need Ownership Checks

Suppose User A sends:

```text
DELETE /todos/999
```

Even though User A is authenticated, Todo `999` might belong to User B.

So authentication alone isn't enough.

We need to check:

```text
Is the user authenticated?
        ↓
Yes
        ↓
Does this todo belong to this user?
        ↓
Yes → continue
No  → reject
```

The ownership/permission part leads directly into our next topic: **Authorization**.

---

# 10. Complete Flow

A protected request looks like this:

```text
Client
  ↓
GET /todos
Authorization: Bearer JWT
  ↓
Express Router
  ↓
authenticateUser
  ↓
JWT verified
  ↓
req.user = authenticated user
  ↓
getTodos controller
  ↓
Todo.find({ userId: req.user.id })
  ↓
Response
```

If the JWT is missing or invalid:

```text
Client
  ↓
GET /todos
  ↓
authenticateUser
  ↓
Invalid / missing JWT
  ↓
401 Unauthorized
  ↓
Controller never runs
```

---

# Step 27 Complete

The key idea:

> **Protected routes use authentication middleware before the controller, and the authenticated user's identity is used to access their own data.**

### Roadmap

```text
[x] Step 22 — Authentication Concepts
[x] Step 23 — Password Hashing with bcrypt
[x] Step 24 — JWT Authentication
[x] Step 25 — Authentication Middleware
[x] Step 26 — Protected Routes
[ ] Step 27 — Authorization (Roles/Permissions)
```

-----------------------------------------------------------------------------------------------------------------------------------------


# Step 28 — Authorization (Roles & Permissions)

Authentication is done. Now we learn **authorization**.

---

## 1. Authentication vs Authorization

### Authentication

Answers:

> **Who are you?**

Example:

```text
JWT → User ID: 123
```

The server knows the user is logged in.

### Authorization

Answers:

> **What are you allowed to do?**

For example:

```text
User → can view their own todos
Admin → can view/delete all users
```

So:

```text
Authentication → Who?
Authorization  → What can they do?
```

---

# 2. Roles

A **role** is a category assigned to a user.

Common roles:

```text
user
admin
manager
editor
```

A user might have:

```js
{
  id: "123",
  name: "John",
  role: "user"
}
```

An admin might have:

```js
{
  id: "456",
  name: "Admin",
  role: "admin"
}
```

---

# 3. Why Roles Are Needed

Imagine an application has:

```text
GET /users
DELETE /users/:id
```

Normal users shouldn't be able to delete users.

But an admin can.

So:

```text
User
 ↓
GET /users
 ↓
Allowed

User
 ↓
DELETE /users/123
 ↓
Forbidden
```

Whereas:

```text
Admin
 ↓
DELETE /users/123
 ↓
Allowed
```

---

# 4. Authorization Middleware

We already have:

```text
authenticateUser
```

which identifies the user.

Now we can have another middleware:

```text
authorizeAdmin
```

Its job is to check:

```text
Is the authenticated user's role = admin?
```

Flow:

```text
Request
  ↓
authenticateUser
  ↓
Who is the user?
  ↓
authorizeAdmin
  ↓
Is user an admin?
  ↓
Yes → Controller
No  → 403 Forbidden
```

---

# 5. Why 403?

Remember the difference:

### 401 Unauthorized

The user is **not properly authenticated**.

Examples:

```text
No JWT
Invalid JWT
Expired JWT
```

### 403 Forbidden

The user **is authenticated**, but doesn't have permission.

Example:

```text
JWT is valid
User is authenticated
Role = user
Trying to access admin route
```

Result:

```text
403 Forbidden
```

---

# 6. Example

Suppose:

```text
GET /admin/users
```

We want only admins.

The route can use both middleware functions:

```js
router.get(
  "/admin/users",
  authenticateUser,
  authorizeAdmin,
  getAllUsers
);
```

The flow is:

```text
GET /admin/users
       ↓
authenticateUser
       ↓
JWT valid?
       ↓
authorizeAdmin
       ↓
role === "admin"?
       ↓
getAllUsers
```

Both checks must pass.

---

# 7. Where Does the Role Come From?

The user's role can be stored in MongoDB:

```text
User
 ├── name
 ├── email
 ├── password
 └── role
```

For example:

```js
{
  name: "John",
  email: "john@example.com",
  password: "...hashed...",
  role: "user"
}
```

or:

```js
{
  name: "Admin",
  email: "admin@example.com",
  password: "...hashed...",
  role: "admin"
}
```

During authentication, we can make the user's identity and role available to later middleware.

For example:

```js
req.user = {
  id: user.id,
  role: user.role
};
```

Then authorization can check:

```js
req.user.role
```

---

# 8. Role-Based Access

This gives us a simple permission system:

```text
Role      Permission

user      Own data
admin     All users/data
manager   Management features
```

For example:

```text
/admin/users
```

could require:

```text
admin
```

while:

```text
/profile
```

could be available to:

```text
user
admin
manager
```

---

# 9. Authentication + Authorization Together

This is the complete backend security flow we've learned:

```text
Login
  ↓
bcrypt.compare()
  ↓
JWT created
  ↓
Client sends JWT
  ↓
authenticateUser
  ↓
Identify user
  ↓
authorizeUser
  ↓
Check permission
  ↓
Controller
```

So:

```text
bcrypt
   ↓
Password verification

JWT
   ↓
Authentication

Authorization
   ↓
Permissions
```

---

# 10. Authentication vs Authorization — Final Picture

| Concept        | Question                           | Example                           |
| -------------- | ---------------------------------- | --------------------------------- |
| Authentication | Who are you?                       | User ID = 123                     |
| Authorization  | What can you do?                   | Admin can delete users            |
| JWT            | How do we carry identity?          | Bearer token                      |
| bcrypt         | How do we safely verify passwords? | Compare password with hash        |
| 401            | Authentication failed              | Invalid JWT                       |
| 403            | Permission denied                  | Normal user accessing admin route |

---------------------------------------------------------------------------------------------------------------------------------------

# Step 29 — OAuth / Social Login Basics

Now we cover the **OAuth / Social Login** item from your original roadmap.

---

## 1. What is OAuth?

OAuth allows a user to give an application access through another trusted service without giving the application their password.

For example:

```text
Your App
   ↓
"Continue with Google"
   ↓
Google
   ↓
User logs in
   ↓
User gives permission
   ↓
Google sends authorization result
   ↓
Your App
```

Common providers:

```text
Google
GitHub
Microsoft
Apple
```

---

## 2. Why Do We Need OAuth?

Without social login, your application needs its own:

```text
Registration
Login form
Password
Password hashing
Password reset
Email verification
```

With social login:

```text
User
 ↓
Continue with Google
 ↓
Google handles authentication
 ↓
Your application receives the user's identity
```

The user doesn't need to create another password specifically for your application.

---

# 3. OAuth vs JWT

These are **not replacements for each other**.

They solve different parts of the problem.

### JWT

JWT can be used by **your backend to authenticate API requests**.

```text
Login
 ↓
Backend
 ↓
JWT
 ↓
Future API requests
```

### OAuth

OAuth allows your application to use an **external provider**.

```text
Your App
 ↓
Google
 ↓
Google authenticates user
 ↓
Your App receives identity
```

You can actually use both:

```text
Google OAuth
     ↓
User authenticated
     ↓
Your Backend
     ↓
Create/find local user
     ↓
Issue your application's session/JWT
     ↓
React calls your API
```

---

# 4. Important: OAuth vs OpenID Connect

This distinction is important.

**OAuth 2.0** is primarily an **authorization framework**.

It was designed around granting access to resources.

For example:

```text
"Allow this application to access my Google Calendar"
```

For **user authentication / identity**, modern applications commonly use **OpenID Connect (OIDC)** on top of OAuth 2.0.

So when you see:

```text
Sign in with Google
```

the identity/login part is commonly based on **OpenID Connect**.

For your roadmap, you mainly need to understand the OAuth/social-login flow rather than implementing the protocol from scratch.

---

# 5. Basic Google Login Flow

Imagine our Todo application has:

```text
Continue with Google
```

The flow is roughly:

```text
React App
   ↓
Google authorization
   ↓
User logs into Google
   ↓
Google asks for consent
   ↓
Google redirects back
   ↓
Backend verifies the authorization result
   ↓
Find/create user in MongoDB
   ↓
Create application session/JWT
   ↓
User is logged into our application
```

---

# 6. What Does Google Give Us?

After successful authentication, your application can receive identity information such as:

```text
Google user ID
Email
Name
Profile information
```

Your application can then create a local user:

```text
User
 ├── id
 ├── name
 ├── email
 ├── provider
 └── providerId
```

For example:

```text
provider = "google"
providerId = "google-user-id"
```

---

# 7. OAuth User vs Normal User

Your database may contain both.

### Normal registration

```text
email + password
```

Password is stored as a bcrypt hash.

### Google login

```text
Google account
```

Your application doesn't need to store the Google password.

You might have:

```text
User A
provider: local

User B
provider: google
```

---

# 8. OAuth Authorization Code Flow

The common modern flow is roughly:

```text
1. User clicks "Login with Google"
              ↓
2. App redirects to Google
              ↓
3. User authenticates with Google
              ↓
4. Google redirects back with authorization code
              ↓
5. Backend exchanges code with Google
              ↓
6. Backend obtains tokens/identity information
              ↓
7. Backend identifies the user
              ↓
8. Backend creates application session/JWT
```

The important thing to understand is:

> The authorization code is not simply the user's password or your application's JWT.

It is part of the OAuth flow used to securely obtain authorization/tokens from the provider.

---

# 9. Why Redirects Are Used

OAuth normally involves redirects because the user needs to authenticate with the external provider.

Example:

```text
localhost:5173
      ↓
Google
      ↓
localhost:5173/callback
```

The callback URL is where the provider sends the user back after the authorization step.

In production, it might look like:

```text
yourapp.com
     ↓
Google
     ↓
yourapp.com/auth/callback
```

The callback URL must be configured with the provider.

---

# 10. OAuth Security

There are several important security concepts.

### Client ID

Identifies your application to the provider.

```text
CLIENT_ID
```

It is generally okay for the client ID to be visible.

### Client Secret

Used to authenticate your application to the provider.

```text
CLIENT_SECRET
```

This must remain secret.

Never put it in React frontend code.

Store it in backend environment variables:

```text
GOOGLE_CLIENT_ID=...
GOOGLE_CLIENT_SECRET=...
```

---

# 11. OAuth + Our Authentication System

Now combine everything we've learned.

### Traditional login

```text
Email + Password
       ↓
bcrypt.compare()
       ↓
JWT
       ↓
Protected API
```

### Google login

```text
Google
   ↓
OAuth/OIDC
   ↓
User identity
   ↓
Find/create local user
   ↓
JWT/session
   ↓
Protected API
```

After login, both types of users can ultimately use the same protected API:

```text
             ┌── Email/password ──┐
             │                    ↓
User ────────┤                  JWT
             │                    ↓
             └── Google ───────→ API
```

---

# 12. OAuth Does Not Mean You Store Google Passwords

This is one of the most important things to remember.

Your application should **never ask the user for their Google password**.

Instead:

```text
Your App
   ↓
Google
   ↓
Google handles password/authentication
   ↓
Your App receives authorized identity information
```

---

# 13. OAuth vs Authentication vs Authorization

Now we can clearly separate the concepts:

| Concept        | Purpose                                                    |
| -------------- | ---------------------------------------------------------- |
| Authentication | Who is the user?                                           |
| Authorization  | What is the user allowed to do?                            |
| bcrypt         | Safely hash/verify local passwords                         |
| JWT            | Carry authentication information for your API              |
| OAuth          | Delegate authorization to an external provider             |
| OpenID Connect | Identity/authentication layer commonly used with OAuth 2.0 |
| Google/GitHub  | External identity providers                                |

--------------------------------------------------------------------------------------------------------------------------------------

# Step 30 — Environment Variables & Configuration

Now we move to the next item in the original roadmap:

```text
[ ] Environment variables, config per env (dev/staging/prod)
```

---

## 1. What Are Environment Variables?

Environment variables are values stored **outside your source code**.

For example, your application may need:

```text
PORT
DATABASE_URL
JWT_SECRET
GOOGLE_CLIENT_ID
GOOGLE_CLIENT_SECRET
```

Instead of putting them directly in your JavaScript:

```js
const jwtSecret = "my-secret-key";
```

we store them in environment variables.

```text
JWT_SECRET=...
```

Then the application reads the value.

---

## 2. Why Do We Need Them?

Some values should not be hard-coded.

Especially:

```text
Database credentials
API keys
JWT secrets
OAuth client secrets
Passwords
```

For example:

```text
Development
    ↓
Local MongoDB

Production
    ↓
Production MongoDB
```

The application code can stay the same while the configuration changes.

---

# 3. `.env`

A common way to manage environment variables locally is a `.env` file.

Example:

```text
PORT=5000
MONGO_URI=mongodb://localhost:27017/todo
JWT_SECRET=some-secret
```

Your Node.js application can read these values.

Conceptually:

```text
.env
 ↓
Environment variables
 ↓
Node.js application
```

---

# 4. Accessing Variables in Node.js

Node.js provides:

```js
process.env
```

So:

```js
process.env.PORT
```

gets the value of `PORT`.

For example:

```js
const port = process.env.PORT;
```

And:

```js
const mongoUri = process.env.MONGO_URI;
```

---

# 5. Using dotenv

A commonly used package is `dotenv`.

Its job is to load values from `.env` into `process.env`.

Conceptually:

```text
.env
 ↓
dotenv
 ↓
process.env
 ↓
Application
```

Then your application can use:

```js
process.env.JWT_SECRET
```

---

# 6. Never Commit `.env`

Your `.env` file may contain secrets:

```text
MONGO_URI=...
JWT_SECRET=...
GOOGLE_CLIENT_SECRET=...
```

So it should normally be added to `.gitignore`:

```text
.env
```

The idea is:

```text
Source code → Git
Secrets     → Environment
```

---

# 7. Development, Staging, Production

A real application usually has different environments.

### Development

Used while you're building:

```text
development
```

Example:

```text
Database → local database
API → localhost
Debugging → enabled
```

### Staging

Used for testing before production:

```text
staging
```

Example:

```text
Database → staging database
API → staging server
```

### Production

The real application used by customers:

```text
production
```

Example:

```text
Database → production database
API → production server
```

---

# 8. Same Code, Different Configuration

This is the important idea.

You don't want:

```text
development code
staging code
production code
```

Instead:

```text
Same application code
        ↓
Different environment variables
        ↓
Different environment
```

For example:

```text
Development

MONGO_URI → local DB
PORT → 5000
```

and:

```text
Production

MONGO_URI → production DB
PORT → 8080
```

The application itself can remain largely the same.

---

# 9. Configuration File

As the application grows, it is useful to centralize configuration.

For example:

```text
src/
 ├── config/
 │    └── env.js
 ├── controllers/
 ├── routes/
 ├── models/
 └── server.js
```

The configuration layer can read environment variables.

Conceptually:

```text
.env
 ↓
config
 ↓
Application
```

This prevents `process.env.SOMETHING` from being scattered throughout the entire project.

---

# 10. Secrets vs Normal Configuration

Not every environment variable is necessarily a secret.

### Usually secret

```text
JWT_SECRET
DATABASE_PASSWORD
GOOGLE_CLIENT_SECRET
API_KEY
```

### Usually not secret

```text
PORT
NODE_ENV
API_BASE_URL
```

But you should still manage configuration systematically.

---

# 11. `NODE_ENV`

A common environment variable is:

```text
NODE_ENV
```

It can indicate:

```text
development
production
test
```

For example:

```js
if (process.env.NODE_ENV === "production") {
  // production behavior
}
```

This allows the application to behave differently depending on its environment.

---

# 12. Important Security Rule

Environment variables are **not automatically secure just because they are in `.env`**.

You still need to:

* Keep `.env` out of Git.
* Never expose backend secrets to the frontend.
* Use proper secret management in production.
* Rotate compromised secrets.
* Avoid logging secrets.

For example, this is dangerous:

```js
console.log(process.env.JWT_SECRET);
```

because the secret could appear in logs.

---

# 13. How Production Works

Locally:

```text
.env
 ↓
Node.js
```

On a production server, you commonly configure environment variables through the hosting platform/server rather than committing a `.env` file containing real secrets.

```text
Production server
       ↓
Environment variables
       ↓
Node.js
```

So your production secrets don't need to exist in your Git repository.

---

# 14. Overall Configuration Flow

```text
                 ┌── Development
                 │
Environment ─────┼── Staging
Variables        │
                 └── Production
                       ↓
                  Node.js App
                       ↓
                Express / MongoDB
```

The main principle is:

> **Keep configuration and secrets outside your application code so the same codebase can run in different environments safely.**

---

# Phase 13 Progress

```text
[x] Node basics, npm, package.json
[x] Express routing, middleware
[x] REST API design
[x] MongoDB + Mongoose
[x] CRUD operations
[x] JWT + bcrypt
[x] OAuth / social login basics
[x] Environment variables, config per env
[ ] Input validation: Zod/Joi
[ ] File uploads: multer + S3/Cloud storage
[ ] Rate limiting, security headers, CORS edge cases
[ ] Caching: Redis
[ ] Logging/monitoring
```

---------------------------------------------------------------------------------------------------------------------------------------

# Step 31 — Input Validation: Zod / Joi

Now we move to the next item in the original roadmap:

```text
[ ] Input validation: Zod/Joi
```

## 1. What is Input Validation?

Input validation means **checking data received from the client before using it**.

For example, our API has:

```text
POST /users
```

The client sends:

```json
{
  "name": "John",
  "email": "john@example.com",
  "age": 25
}
```

Before saving this data to MongoDB, the backend should check:

```text
Is name present?
Is email valid?
Is age a number?
Is age within the allowed range?
```

---

# 2. Why Do We Need Validation?

Never assume the frontend sends correct data.

A malicious or buggy client could send:

```json
{
  "name": "",
  "email": "hello",
  "age": "abc"
}
```

Or:

```json
{}
```

The backend must validate independently.

The important rule is:

> **Frontend validation improves user experience. Backend validation protects the application.**

---

# 3. Without Validation

Suppose our controller directly does:

```js
const user = await User.create(req.body);
```

We are trusting everything the client sends.

That can cause:

```text
Missing fields
Wrong data types
Invalid email
Unexpected values
Bad database data
```

---

# 4. Schema-Based Validation

Instead, we define what valid data looks like.

For example:

```text
User input

name  → string
email → valid email
age   → number
```

A validation library checks the incoming data against these rules.

Two popular choices are:

```text
Zod
Joi
```

Your roadmap specifically mentions **Zod/Joi**, so you should understand both conceptually.

---

# 5. Zod

Zod lets us define a schema.

For example:

```js
const userSchema = z.object({
  name: z.string().min(2),
  email: z.string().email(),
  age: z.number().min(18)
});
```

This describes valid input:

```text
name
 └── must be string
 └── minimum 2 characters

email
 └── must be string
 └── must be valid email

age
 └── must be number
 └── minimum 18
```

---

# 6. Valid Input

For example:

```json
{
  "name": "John",
  "email": "john@example.com",
  "age": 25
}
```

Validation succeeds.

```text
Request
   ↓
Validation
   ↓
Valid
   ↓
Controller
   ↓
Database
```

---

# 7. Invalid Input

Suppose:

```json
{
  "name": "J",
  "email": "not-an-email",
  "age": 15
}
```

The validation fails.

```text
Request
   ↓
Validation
   ↓
Invalid
   ↓
400 Bad Request
```

The controller shouldn't continue with invalid data.

---

# 8. Validation Middleware

Just like authentication, validation can be middleware.

The request flow becomes:

```text
Client
  ↓
Route
  ↓
Validation Middleware
  ↓
Authentication Middleware
  ↓
Controller
  ↓
Database
```

The exact order depends on what the route needs.

For example:

```text
POST /todos
```

could be:

```text
Request
 ↓
authenticateUser
 ↓
validateTodo
 ↓
createTodo
```

---

# 9. Validate `req.body`

For a Todo:

```json
{
  "title": "Learn Node.js"
}
```

We could define:

```text
title
 └── string
 └── minimum length
 └── required
```

Then the backend only accepts data matching that structure.

---

# 10. Validate More Than `body`

Incoming request data can come from several places.

### Request body

```text
req.body
```

Example:

```text
POST /todos
```

```json
{
  "title": "Learn Express"
}
```

### URL parameters

```text
req.params
```

Example:

```text
GET /todos/123
```

Here:

```text
123
```

is a route parameter.

### Query parameters

```text
req.query
```

Example:

```text
GET /todos?page=2&limit=10
```

So validation can apply to:

```text
req.body
req.params
req.query
```

---

# 11. Validation vs Sanitization

These are related but different.

### Validation

Checks whether data is acceptable.

```text
Is age a number?
```

### Sanitization

Cleans or transforms data.

For example:

```text
"  John  "
```

might become:

```text
"John"
```

Validation asks:

> Is this data valid?

Sanitization asks:

> Can we safely clean/normalize this data?

---

# 12. Joi

Joi solves the same general problem.

You define a schema describing acceptable data.

Conceptually:

```text
Joi schema
   ↓
Incoming request
   ↓
Check data
   ↓
Valid → Controller
Invalid → Error response
```

The syntax differs from Zod, but the fundamental idea is the same.

---

# 13. Why Zod Is Popular in TypeScript Projects

Zod works particularly well with TypeScript because the schema can also be used to infer types.

Conceptually:

```text
Zod Schema
    ↓
Validation
    +
Type information
```

This makes Zod especially useful in modern TypeScript applications.

Joi is also a mature and widely used validation library.

For this roadmap, you don't need to memorize every Zod or Joi method.

The important concept is:

> **Define the expected shape of data, validate incoming requests against that schema, and reject invalid data before the controller/database operation.**

---

# 14. HTTP Status Code

Invalid client input commonly results in:

```text
400 Bad Request
```

For example:

```text
POST /todos

{
  "title": ""
}
```

If `title` is required and cannot be empty:

```text
400 Bad Request
```

The response can contain useful validation information, such as which field failed.

---

# 15. Complete Backend Flow

Our backend is becoming more complete:

```text
Client
  ↓
Request
  ↓
Authentication
  ↓
Authorization
  ↓
Input Validation
  ↓
Controller
  ↓
Mongoose
  ↓
MongoDB
  ↓
Response
```

Not every route needs every step, but these are the major layers we've learned.

---

# Step 31 Complete

The main thing to remember:

```text
Client data
    ↓
Validate
    ↓
Valid → continue
Invalid → 400
```

**Frontend validation is for user experience. Backend validation is a security and data-integrity requirement.**

### Phase 13 Progress

```text
[x] Node basics, npm, package.json
[x] Express routing, middleware
[x] REST API design
[x] MongoDB + Mongoose
[x] CRUD operations
[x] JWT + bcrypt
[x] OAuth / social login
[x] Environment variables / config
[x] Input validation: Zod/Joi
[ ] File uploads: multer + S3/Cloud storage
[ ] Rate limiting, security headers, CORS edge cases
[ ] Caching: Redis
[ ] Logging/monitoring
```
----------------------------------------------------------------------------------------------------------------------------------------


# Step 32 — File Uploads: Multer + Cloud Storage

Now we move to the next item in the original roadmap:

```text
[ ] File uploads: multer + S3/Cloud storage
```

We'll split this into two concepts:

1. **Multer** → receives files in Express
2. **Cloud storage** → stores the files outside your backend server

---

## 1. Why File Uploads Are Different

Normal API requests usually send JSON:

```json
{
  "title": "Learn Node.js"
}
```

But a file is binary data:

```text
Image
PDF
Video
Document
```

So uploading a file commonly uses:

```text
multipart/form-data
```

instead of:

```text
application/json
```

---

# 2. What is Multer?

**Multer** is Express middleware for handling `multipart/form-data`, especially file uploads.

The basic flow is:

```text
React
  ↓
File
  ↓
multipart/form-data
  ↓
Express
  ↓
Multer
  ↓
Controller
```

Multer extracts the uploaded file so your backend can process it.

---

# 3. Example

Suppose our Todo application allows users to upload a profile picture.

The request might contain:

```text
POST /profile/avatar
```

with:

```text
multipart/form-data
```

and:

```text
avatar → profile.jpg
```

Multer processes the upload.

The backend can then access information about the file through the request.

Conceptually:

```text
req.file
```

For multiple files:

```text
req.files
```

---

# 4. What Information Does the Backend Get?

A file upload can contain information such as:

```text
filename
original name
MIME type
size
file location/buffer
```

For example:

```text
profile.jpg
image/jpeg
2 MB
```

The backend can use this information to decide whether the upload is acceptable.

---

# 5. File Validation

Just like normal request data, uploaded files need validation.

For example:

```text
Allowed:
.jpg
.jpeg
.png

Maximum:
5 MB
```

You don't want someone uploading:

```text
10 GB video
.exe file
unexpected file type
```

So the backend should check:

```text
File
 ↓
Size?
 ↓
Type?
 ↓
Allowed?
 ↓
Continue
```

---

# 6. Where Should We Store the File?

This is an important architectural decision.

### Option 1 — Local server

You could save:

```text
uploads/
   profile.jpg
```

on your backend server.

This can work for development or small applications.

But production systems often shouldn't depend on local server storage.

---

# 7. Why Local Storage Can Be a Problem

Imagine your application runs on multiple servers:

```text
Server A
Server B
Server C
```

A user uploads a file to Server A.

The file exists on:

```text
Server A
```

but not necessarily:

```text
Server B
Server C
```

Also, servers can be replaced, restarted, scaled, or redeployed.

So application servers are generally not ideal as permanent file storage.

---

# 8. Cloud Storage

Instead, files can be stored in dedicated object storage.

Examples:

```text
Amazon S3
Cloudinary
Google Cloud Storage
Azure Blob Storage
```

Your backend handles the upload and sends the file to the storage service.

```text
React
  ↓
Express
  ↓
Multer
  ↓
Cloud Storage
  ↓
File URL
```

---

# 9. What Gets Stored in MongoDB?

Usually, you don't put the actual large image/video binary directly into your normal user document.

Instead, you might store metadata:

```text
User
 ├── name
 ├── email
 └── avatarUrl
```

For example:

```text
avatarUrl
→ https://storage-provider/.../profile.jpg
```

The actual file lives in cloud storage.

MongoDB stores the information needed to find it.

---

# 10. Complete Upload Flow

Let's say a user uploads:

```text
profile.jpg
```

The process becomes:

```text
React
   ↓
Select profile.jpg
   ↓
POST /profile/avatar
   ↓
multipart/form-data
   ↓
Authentication
   ↓
Multer
   ↓
Validate file
   ↓
Upload to S3/Cloudinary
   ↓
Receive file URL
   ↓
Save URL in MongoDB
   ↓
Response
```

The response could contain:

```json
{
  "avatarUrl": "https://storage.example/profile.jpg"
}
```

---

# 11. Multer Is Not Cloud Storage

This distinction is important.

```text
Multer
   ↓
Handles the incoming file
```

It does **not** permanently provide cloud storage.

Think:

```text
Multer → receives/processes file

S3/Cloudinary → stores file
```

They solve different problems.

---

# 12. Single vs Multiple Uploads

### Single file

For example:

```text
Profile picture
```

```text
req.file
```

### Multiple files

For example:

```text
Product images
```

```text
req.files
```

The backend can then process each uploaded file.

---

# 13. Security Considerations

File uploads need extra care.

You should consider:

```text
File size limits
Allowed MIME types
Allowed extensions
Authentication
Storage permissions
Filename handling
Malicious files
```

Never blindly trust the filename sent by the client.

For example:

```text
virus.exe
```

shouldn't become acceptable merely because someone renames it:

```text
image.jpg
```

File validation should be handled carefully.

---

# 14. Public vs Private Files

Not every uploaded file should be publicly accessible.

### Public

Examples:

```text
Profile image
Product image
Blog image
```

A public URL may be acceptable.

### Private

Examples:

```text
Invoices
Personal documents
Internal reports
```

These may require controlled access.

The storage system and backend can generate or enforce appropriate access.

---

# 15. Important Architecture

Our backend now looks like:

```text
                    ┌── MongoDB
                    │
React
  ↓                 │
Express → Multer → Cloud Storage
  ↓
Authentication
  ↓
Validation
  ↓
Controller
```

More accurately, the controller coordinates the storage/database operations:

```text
Request
  ↓
Auth
  ↓
Validation
  ↓
Multer
  ↓
Controller
  ├── Cloud Storage
  └── MongoDB
```

---

# Step 32 Complete

Remember the core idea:

> **Multer handles incoming file uploads in Express; cloud storage provides durable storage for the actual files.**

And typically:

```text
Actual file → Cloud Storage

File URL / metadata → MongoDB
```

### Phase 13 Progress

```text
[x] Node basics, npm, package.json
[x] Express routing, middleware
[x] REST API design
[x] MongoDB + Mongoose
[x] CRUD operations
[x] JWT + bcrypt
[x] OAuth / social login
[x] Environment variables / config
[x] Input validation: Zod/Joi
[x] File uploads: Multer + S3/Cloud storage
[ ] Rate limiting, security headers, CORS edge cases
[ ] Caching: Redis
[ ] Logging/monitoring
```

**Next: Step 32 — Rate Limiting, Security Headers, and CORS Edge Cases.**

----------------------------------------------------------------------------------------------------------------------------------------

# Step 33 — Rate Limiting, Security Headers & CORS Edge Cases


# Part 1 — Rate Limiting

## 1. What is Rate Limiting?

Rate limiting restricts how many requests a client can make within a specific period.

For example:

```text
Maximum: 100 requests
Time: 15 minutes
```

If someone exceeds the limit:

```text
Request
Request
Request
Request
...
Too many requests
        ↓
429 Too Many Requests
```

---

## 2. Why Do We Need It?

Without rate limiting:

```text
Attacker / Bot
      ↓
100,000 requests
      ↓
Your API
      ↓
Server overload ❌
```

Rate limiting helps protect against:

```text
Brute-force login attempts
API abuse
Spam
Excessive traffic
Basic denial-of-service attempts
```

---

## 3. Login Rate Limiting

This is especially important:

```text
POST /login
```

Imagine someone repeatedly trying passwords:

```text
password1
password2
password3
password4
...
```

Rate limiting can restrict attempts:

```text
5 login attempts
within 15 minutes
```

After that:

```text
429 Too Many Requests
```

---

## 4. Different Limits for Different Routes

You don't always want one global rule.

For example:

```text
General API
→ 100 requests / 15 minutes

Login
→ 5 requests / 15 minutes

Password reset
→ stricter limit
```

Sensitive endpoints usually need stronger limits.

---

# Part 2 — Security Headers

## 5. What Are HTTP Headers?

HTTP requests and responses contain headers.

Example response:

```text
HTTP Response
   ↓
Headers
   ↓
Body
```

Headers can provide information and security instructions to browsers.

---

## 6. What is Helmet?

Helmet is Express middleware that helps configure security-related HTTP headers.

Conceptually:

```text
Client
  ↓
Express
  ↓
Helmet
  ↓
Security Headers
  ↓
Response
```

Helmet helps apply sensible security protections without manually configuring every header.

---

## 7. Why Security Headers Matter

Browsers need rules about how your application should behave.

Security headers can help reduce risks related to things such as:

```text
Clickjacking
Certain XSS-related risks
MIME type sniffing
Unsafe browser behavior
```

The main concept:

> Security headers tell the browser to follow safer rules when handling your application's content.

---

## 8. Helmet in the Application

Conceptually, Helmet sits near the beginning of middleware:

```text
Request
  ↓
Helmet
  ↓
Other Middleware
  ↓
Routes
```

It applies security-related headers to responses.

---

# Part 3 — CORS

## 9. What is CORS?

CORS means:

**Cross-Origin Resource Sharing**

It controls whether a browser allows a frontend from one origin to access resources from another origin.

For example:

```text
Frontend:
http://localhost:5173

Backend:
http://localhost:5000
```

These are different origins.

So the browser applies CORS rules.

---

## 10. What is an Origin?

An origin is based mainly on:

```text
Protocol
Host
Port
```

For example:

```text
http://localhost:5173
http://localhost:5000
```

Different ports:

```text
Different origins
```

Even though both use:

```text
localhost
```

---

# 11. Why CORS Exists

Imagine any website could freely make browser requests using your logged-in credentials.

That could create security problems.

CORS allows the server to say:

```text
Which origins are allowed to access me?
```

For example:

```text
Frontend:
https://myapp.com

Backend:
https://api.myapp.com
```

The backend can allow:

```text
https://myapp.com
```

---

# 12. Simple CORS Flow

```text
Browser
  ↓
Request from Frontend
  ↓
Backend
  ↓
Is this origin allowed?
  ↓
Yes → Browser allows response
No  → Browser blocks access
```

Important:

> CORS is primarily enforced by browsers.

It is not a replacement for authentication or authorization.

---

# 13. CORS Is Not Authentication

This is extremely important.

Suppose your API allows only:

```text
https://myapp.com
```

That does not mean your API is automatically protected from unauthorized users.

You still need:

```text
Authentication
Authorization
Input validation
```

CORS only controls browser cross-origin access behavior.

---

# 14. Common CORS Problem

Frontend:

```text
http://localhost:5173
```

Backend:

```text
http://localhost:5000
```

React tries:

```text
GET http://localhost:5000/todos
```

Browser:

```text
Different origin detected
        ↓
Check CORS response headers
        ↓
Allowed?
```

If not configured correctly:

```text
CORS Error
```

---

# 15. Preflight Requests

Some requests cause the browser to send an `OPTIONS` request before the actual request.

Example:

```text
Browser
   ↓
OPTIONS request
   ↓
Server responds with allowed rules
   ↓
Browser
   ↓
Actual POST/PUT/DELETE request
```

This is called a:

```text
Preflight request
```

The browser is essentially checking:

```text
Can I make this cross-origin request?
```

---

# 16. When Does Preflight Commonly Happen?

For example, requests using:

```text
Authorization header
```

such as:

```text
Authorization: Bearer JWT
```

may trigger a preflight request.

Also, certain methods and headers can trigger preflight.

This becomes common in authenticated React applications.

---

# 17. CORS + JWT

Our Todo application might look like:

```text
React
http://localhost:5173

        ↓ JWT Request

Express API
http://localhost:5000
```

The backend needs appropriate CORS configuration so the browser allows the frontend to communicate with it.

At the same time:

```text
JWT authentication
```

still verifies the user.

So:

```text
CORS
↓
Can this browser origin access the API?

JWT
↓
Who is making the request?
```

Different responsibilities.

---

# 18. Credentials and CORS

Things become more complex when using browser credentials such as cookies.

For example:

```text
Cookies
Cross-origin requests
```

require careful configuration.

You cannot casually combine:

```text
Allow every origin
+
Credentials
```

A production application should explicitly control trusted origins.

For example:

```text
Development:
http://localhost:5173

Production:
https://myapp.com
```

---

# 19. Development vs Production CORS

Development:

```text
React → localhost:5173
API   → localhost:5000
```

Production:

```text
Frontend → https://myapp.com
Backend  → https://api.myapp.com
```

Your allowed CORS origins may therefore change by environment.

This connects directly to what we learned about:

```text
Environment variables
```

For example conceptually:

```text
Development
→ FRONTEND_URL=http://localhost:5173

Production
→ FRONTEND_URL=https://myapp.com
```

---

# 20. The Three Concepts Together

Let's compare them:

| Feature       | Main Purpose                        |
| ------------- | ----------------------------------- |
| Rate Limiting | Prevent excessive/abusive requests  |
| Helmet        | Add security-related HTTP headers   |
| CORS          | Control browser cross-origin access |

They protect different layers.

```text
Incoming Request
       ↓
Rate Limiting
       ↓
CORS Rules
       ↓
Authentication
       ↓
Authorization
       ↓
Validation
       ↓
Controller
       ↓
Response + Security Headers
```

The exact middleware order can vary depending on the application.

---

# 21. Important Status Codes

### Rate limit exceeded

```text
429 Too Many Requests
```

### Authentication problem

```text
401 Unauthorized
```

### Permission problem

```text
403 Forbidden
```

### Invalid request

```text
400 Bad Request
```

---

# Step 33 Complete

The main things to remember:

### Rate Limiting

```text
Too many requests
      ↓
429
```

### Helmet

```text
Express
   ↓
Security-related HTTP headers
```

### CORS

```text
Frontend origin
      ↓
Backend
      ↓
Browser checks permission
```

And most importantly:

> **CORS is not authentication, and rate limiting is not authorization. Each solves a different security problem.**

---

# Phase 13 Progress

```text
[x] Node basics, npm, package.json
[x] Express routing, middleware
[x] REST API design
[x] MongoDB + Mongoose
[x] CRUD operations
[x] JWT + bcrypt
[x] OAuth / social login basics
[x] Environment variables / config
[x] Input validation: Zod/Joi
[x] File uploads: Multer + S3/Cloud storage
[x] Rate limiting, security headers, CORS edge cases
[ ] Caching: Redis basics
[ ] Logging/monitoring basics
```

**Next: Step 33 — Redis Caching Basics.**

---------------------------------------------------------------------------------------------------------------------------------------

# Step 34 — Redis Caching Basics

Now we move to the next item in the original roadmap:

```text
[ ] Caching: Redis basics
```

---

# 1. What Is Caching?

Caching means temporarily storing frequently used data so it can be retrieved faster.

Without caching:

```text
Client
  ↓
Express
  ↓
MongoDB
  ↓
Get Data
  ↓
Response
```

With caching:

```text
Client
  ↓
Express
  ↓
Redis Cache
  ↓
Data found?
 ├── Yes → Response immediately
 │
 └── No
      ↓
    MongoDB
      ↓
    Save in Redis
      ↓
    Response
```

---

# 2. Why Do We Need Caching?

Database queries take time and consume resources.

Imagine:

```text
10,000 users
      ↓
GET /products
      ↓
MongoDB queried 10,000 times
```

If the product data rarely changes, repeatedly querying MongoDB is unnecessary.

With Redis:

```text
First Request
      ↓
MongoDB
      ↓
Redis Cache

Later Requests
      ↓
Redis
      ↓
Fast Response
```

---

# 3. What Is Redis?

Redis is an in-memory data store.

The important word is:

```text
In-memory
```

This means Redis primarily stores data in memory (RAM), making it extremely fast.

Conceptually:

```text
MongoDB
  ↓
Disk-based database
  ↓
Persistent data


Redis
  ↓
Memory (RAM)
  ↓
Fast temporary data access
```

Redis can be used for more than caching, but caching is one of its most common use cases.

---

# 4. MongoDB vs Redis

They serve different purposes.

| MongoDB                       | Redis                                           |
| ----------------------------- | ----------------------------------------------- |
| Primary database              | Fast data store/cache                           |
| Stores application data       | Often stores temporary/frequently accessed data |
| Persistent storage            | Primarily memory-based access                   |
| User records, todos, products | Cached API results, sessions, counters          |

Think:

```text
MongoDB = Main storage

Redis = Fast temporary access layer
```

Redis does not usually replace your primary database in our application architecture.

---

# 5. Basic Cache Flow

Suppose we have:

```text
GET /products
```

### First request

```text
Client
  ↓
Express
  ↓
Check Redis
  ↓
Not found
  ↓
MongoDB
  ↓
Get products
  ↓
Save products in Redis
  ↓
Send response
```

This is called a:

```text
Cache miss
```

---

### Later request

```text
Client
  ↓
Express
  ↓
Check Redis
  ↓
Data found
  ↓
Send response
```

This is called a:

```text
Cache hit
```

---

# 6. Cache Hit vs Cache Miss

### Cache Hit

Data exists in Redis.

```text
Redis
  ↓
Data found
  ↓
Fast response
```

### Cache Miss

Data does not exist.

```text
Redis
  ↓
No data
  ↓
Query database
  ↓
Store result in Redis
```

Simple rule:

```text
Hit  = Data found in cache
Miss = Data not found in cache
```

---

# 7. Cache Keys

Redis stores data using keys.

Conceptually:

```text
Key                    Value

products               [...]
user:123               {...}
todos:user:123         [...]
```

For example:

```text
todos:user:123
```

could contain cached todos belonging to User 123.

Keys should be organized clearly.

---

# 8. TTL — Time To Live

Cached data should not necessarily live forever.

Redis can assign a:

```text
TTL = Time To Live
```

Example:

```text
products
TTL: 5 minutes
```

Flow:

```text
Save data in Redis
       ↓
5 minutes pass
       ↓
Data expires
       ↓
Next request gets fresh data
```

TTL helps prevent stale data from remaining forever.

---

# 9. The Stale Data Problem

Suppose Redis contains:

```text
Product price: $100
```

But MongoDB is updated:

```text
Product price: $120
```

Redis may still contain:

```text
$100
```

This is called stale cache data.

So caching creates an important problem:

```text
How do we keep cached data updated?
```

---

# 10. Cache Invalidation

Cache invalidation means removing or updating cached data when the original data changes.

Example:

```text
Product updated
      ↓
MongoDB updated
      ↓
Remove old Redis cache
      ↓
Next request
      ↓
Fetch fresh data
```

Conceptually:

```text
Update Database
       ↓
Delete Related Cache
```

Then:

```text
Next GET Request
       ↓
Cache miss
       ↓
Fresh database data
       ↓
Save new cache
```

---

# 11. Example With Todos

Suppose:

```text
GET /todos
```

uses Redis caching.

```text
GET /todos
    ↓
Check Redis
    ↓
Cache hit?
```

### Yes

```text
Return cached todos
```

### No

```text
MongoDB
  ↓
Get todos
  ↓
Store in Redis
  ↓
Return todos
```

Now imagine:

```text
POST /todos
```

creates a new todo.

The cached todo list may now be outdated.

So:

```text
POST /todos
     ↓
MongoDB updated
     ↓
Delete todos cache
```

The same idea applies to:

```text
PUT /todos/:id
DELETE /todos/:id
```

---

# 12. Cache-Aside Pattern

The pattern we described is commonly called:

```text
Cache-Aside
```

The application manages the cache itself.

```text
Application
   ↓
Check Cache
   ↓
Cache hit? → Return data

Cache miss
   ↓
Database
   ↓
Store in cache
   ↓
Return data
```

This is one of the most common caching patterns.

---

# 13. Redis Is Not Only for Caching

Redis can also be used for:

```text
Sessions
Rate limiting
Queues
Pub/Sub messaging
Counters
Temporary tokens
Real-time features
```

For this roadmap, the main focus is:

```text
Redis as a caching layer
```

---

# 14. Where Redis Fits in Our Backend

Our architecture now looks like:

```text
Client
  ↓
Express API
  ↓
Authentication
  ↓
Validation
  ↓
Controller
  ↓
Redis Cache
  ├── Cache Hit → Response
  │
  └── Cache Miss
          ↓
        MongoDB
          ↓
        Response
```

---

# 15. Important Things to Remember

### Redis

```text
Fast in-memory data store
```

### Cache Hit

```text
Data found in Redis
```

### Cache Miss

```text
Data not found
→ Get from database
```

### TTL

```text
Automatically expire cached data
```

### Cache Invalidation

```text
Data changes
↓
Remove/update old cache
```

---

# Mental Model

```text
              Request
                 ↓
             Check Redis
              /       \
             /         \
       Cache Hit     Cache Miss
           ↓             ↓
        Response      MongoDB
                         ↓
                    Store in Redis
                         ↓
                      Response
```

---

# Phase 13 Progress

```text
[x] Node basics, npm, package.json
[x] Express routing, middleware
[x] REST API design
[x] MongoDB + Mongoose
[x] CRUD operations
[x] JWT + bcrypt
[x] OAuth / social login basics
[x] Environment variables / config
[x] Input validation: Zod/Joi
[x] File uploads: Multer + S3/Cloud storage
[x] Rate limiting, security headers, CORS edge cases
[x] Caching: Redis basics
[ ] Logging/monitoring basics
```

---------------------------------------------------------------------------------------------------------------------------------------


# Step 35 — Logging & Monitoring Basics

This is the final major item in the Backend section of your original roadmap:

```text
[ ] Logging/monitoring basics: Winston/Pino, error tracking (Sentry)
```

These concepts help us understand what is happening inside a production backend.

---

# Part 1 — What Is Logging?

## 1. Why Do We Need Logs?

Imagine your production API has an error:

```text
User reports:
"Login is not working."
```

You cannot simply look at the user's browser and immediately know what happened.

You need information from the backend:

```text
What request happened?
What error occurred?
When did it happen?
Which endpoint failed?
```

This information comes from logs.

---

## 2. What Is a Log?

A log is a recorded message about something happening in your application.

For example:

```text
Server started on port 5000

GET /todos - 200

POST /login - 401

Database connection failed
```

Logs help developers understand application behavior.

---

# 3. Basic Logging

You've already seen:

```js
console.log("Server started");
```

and:

```js
console.error(error);
```

These are basic forms of logging.

For learning and small projects:

```text
console.log()
console.error()
```

can be enough.

But production applications usually need a better logging system.

---

# 4. Logging Libraries

Popular Node.js logging libraries include:

```text
Winston
Pino
```

They provide more structured logging.

Instead of:

```text
Something went wrong
```

you can record useful context:

```text
Time: 2026-09-06
Level: ERROR
Route: /login
Message: Database connection failed
```

---

# 5. Log Levels

Logs usually have different severity levels.

Common levels:

```text
DEBUG
INFO
WARN
ERROR
```

---

## DEBUG

Detailed information useful during development.

```text
DEBUG: Checking JWT token
```

---

## INFO

Normal application events.

```text
INFO: Server started
INFO: User logged in
```

---

## WARN

Something unusual happened, but the application can continue.

```text
WARN: Rate limit approaching
```

---

## ERROR

Something failed.

```text
ERROR: Database connection failed
```

---

# 6. Why Log Levels Matter

Imagine thousands of log messages.

Without levels:

```text
Everything mixed together
```

With levels:

```text
INFO
WARN
ERROR
```

You can filter logs.

For example:

```text
Show only ERROR logs
```

This makes debugging production systems much easier.

---

# Part 2 — Winston and Pino

## 7. Winston

Winston is a flexible Node.js logging library.

Conceptually:

```text
Application
     ↓
Winston
     ↓
Formatted Logs
     ↓
Console / File / Logging Service
```

It can send logs to different destinations.

---

## 8. Pino

Pino is another popular Node.js logging library.

It focuses heavily on:

```text
Performance
Structured logging
Fast logging
```

Conceptually:

```text
Application
     ↓
Pino
     ↓
Structured JSON Logs
```

---

# 9. Structured Logging

Instead of writing only:

```text
User login failed
```

structured logging stores information in a predictable format.

Conceptually:

```json
{
  "level": "error",
  "message": "User login failed",
  "userId": "123",
  "timestamp": "..."
}
```

This is easier for logging platforms to search and analyze.

---

# 10. Winston vs Pino

| Winston                      | Pino                               |
| ---------------------------- | ---------------------------------- |
| Flexible and feature-rich    | Performance-focused                |
| Many configuration options   | Lightweight and fast               |
| Popular in Node.js           | Popular in modern Node.js services |
| Supports multiple transports | Structured JSON logging            |

You don't need to master both.

The important thing is understanding:

> Production applications use structured logging instead of relying only on `console.log()`.

---

# Part 3 — Monitoring

## 11. What Is Monitoring?

Logging tells you:

> What happened?

Monitoring tells you:

> How is the application performing right now?

For example:

```text
CPU usage
Memory usage
Response time
Error rate
Request count
Server uptime
```

---

# 12. Example

Imagine your backend normally responds in:

```text
200 ms
```

Suddenly:

```text
5 seconds
```

Monitoring can detect that the application is becoming slow.

```text
Backend
   ↓
Monitoring system
   ↓
Response time increased
   ↓
Alert developers
```

---

# 13. Logs vs Monitoring

| Logging             | Monitoring                      |
| ------------------- | ------------------------------- |
| Records events      | Tracks system health            |
| "What happened?"    | "How is the system performing?" |
| Errors and messages | CPU, memory, latency, uptime    |
| Helps debugging     | Helps detect problems           |

They work together.

---

# Part 4 — Error Tracking

## 14. What Is Error Tracking?

Error tracking focuses specifically on application errors.

For example:

```text
TypeError
Database Error
Unhandled Promise Rejection
Unexpected Server Error
```

An error tracking service collects these errors.

Instead of discovering errors only through users, developers can see:

```text
Error happened
     ↓
Error tracking service captures it
     ↓
Developer receives information
```

---

# 15. Sentry

Sentry is a popular error tracking platform.

Conceptually:

```text
Express Application
       ↓
Error occurs
       ↓
Sentry
       ↓
Error captured
       ↓
Dashboard
       ↓
Developer investigates
```

It can provide information such as:

```text
Error message
Stack trace
When it happened
How often it happens
Application environment
```

---

# 16. Example Production Flow

Imagine:

```text
POST /todos
```

Something fails unexpectedly.

The application flow could be:

```text
Request
   ↓
Controller
   ↓
Database Error
   ↓
Global Error Handler
   ├── Send safe response to client
   │
   ├── Log error with Pino/Winston
   │
   └── Send error information to Sentry
```

The client receives:

```json
{
  "message": "Internal Server Error"
}
```

But developers receive detailed internal information.

This is important because:

> The client should not see sensitive internal errors, but developers still need enough information to debug them.

---

# 17. Development vs Production

### Development

You might use:

```text
console.log()
console.error()
Detailed error messages
```

### Production

You usually want:

```text
Structured logging
Log levels
Centralized logs
Error tracking
Monitoring
Alerts
```

---

# 18. Complete Backend Architecture

Let's look at everything we've learned in Phase 13.

```text
CLIENT
   ↓
RATE LIMITING
   ↓
SECURITY MIDDLEWARE
   ↓
CORS
   ↓
ROUTES
   ↓
AUTHENTICATION
   ↓
AUTHORIZATION
   ↓
INPUT VALIDATION
   ↓
CONTROLLER
   ↓
CACHE (Redis)
   ↓
DATABASE (MongoDB)
   ↓
RESPONSE
```

Supporting production systems:

```text
Application
    ├── Logging (Winston/Pino)
    ├── Error Tracking (Sentry)
    └── Monitoring
```

---

# 19. The Important Mental Model

```text
Something happens
      ↓
Log it
      ↓
Something fails
      ↓
Track the error
      ↓
System becomes unhealthy
      ↓
Monitoring detects it
      ↓
Developers investigate
```

---

# Step 35 Complete

Remember:

### Logging

```text
Record what happens.
```

### Winston / Pino

```text
Structured application logging.
```

### Monitoring

```text
Track application and system health.
```

### Sentry

```text
Track and investigate application errors.
```


---------------------------------------------------------------------------------------------------------------------------------------