# Full Stack Integration


# Step 1 — Connect React Frontend to Express Backend

Before this phase, you had two separate applications:

```text id="7v4p2n"
React Frontend
      ↓
   Browser
```

and:

```text id="w2k8xs"
Express Backend
      ↓
   Database
```

Now we connect them.

---

# 1. Full-Stack Architecture

The basic architecture becomes:

```text id="6v5k9a"
┌─────────────────┐
│  React Frontend │
│                 │
│   Browser       │
└────────┬────────┘
         │
         │ HTTP Request
         ↓
┌─────────────────┐
│ Express Backend │
│                 │
│   REST API      │
└────────┬────────┘
         │
         │ Database Query
         ↓
┌─────────────────┐
│    Database     │
└─────────────────┘
```

For example:

```text id="k4q8m1"
React
  ↓
GET /api/todos
  ↓
Express
  ↓
MongoDB
  ↓
Todos
  ↓
Express
  ↓
React
```

---

# 2. Frontend and Backend Are Separate

A common misconception is that React directly talks to MongoDB.

It should **not**.

Bad:

```text id="q8z3pw"
React
  ↓
MongoDB ❌
```

Correct:

```text id="n6r2tx"
React
  ↓
Express API
  ↓
MongoDB
```

The backend is the middle layer.

---

# 3. Why Do We Need the Backend?

The backend handles things such as:

```text id="6m4j2c"
Authentication
Authorization
Validation
Business logic
Database access
Secrets
API keys
Security
```

For example, suppose your application has:

```text id="w8h1z7"
OPENAI_API_KEY
DATABASE_URL
JWT_SECRET
```

These should stay on the backend.

They should **not** be placed in React's client-side code.

---

# 4. Example Backend Endpoint

Suppose Express has:

```text id="p3j7kc"
GET /api/todos
```

The backend might do:

```text id="9d5x1w"
GET /api/todos
      ↓
Express Route
      ↓
Controller
      ↓
Database
      ↓
Return JSON
```

Response:

```json id="f5v2n8"
[
  {
    "id": 1,
    "title": "Learn Node.js"
  },
  {
    "id": 2,
    "title": "Build API"
  }
]
```

---

# 5. React Calls the API

React can use `fetch()`:

```js id="j7c3qa"
const response = await fetch("http://localhost:5000/api/todos");

const todos = await response.json();
```

The flow is:

```text id="d2n9xf"
React
  ↓
fetch()
  ↓
http://localhost:5000/api/todos
  ↓
Express
  ↓
Database
  ↓
JSON Response
  ↓
React
```

---

# 6. GET Request

For reading data:

```js id="v8m4cx"
const response = await fetch(
  "http://localhost:5000/api/todos"
);

const data = await response.json();
```

Backend:

```text id="s6z2kp"
GET /api/todos
```

---

# 7. POST Request

For creating a todo:

```js id="q1y8mb"
await fetch("http://localhost:5000/api/todos", {
  method: "POST",
  headers: {
    "Content-Type": "application/json"
  },
  body: JSON.stringify({
    title: "Learn Express"
  })
});
```

Flow:

```text id="f9w3hs"
React
 ↓
POST /api/todos
 ↓
Express
 ↓
Validate data
 ↓
Database
 ↓
Created Todo
 ↓
React
```

---

# 8. PUT/PATCH

Updating data:

```text id="v1a7dn"
React
 ↓
PATCH /api/todos/123
 ↓
Express
 ↓
Database
 ↓
Updated Todo
 ↓
React
```

---

# 9. DELETE

Deleting data:

```text id="b6x2kr"
React
 ↓
DELETE /api/todos/123
 ↓
Express
 ↓
Database
 ↓
Success
 ↓
React
```

---

# 10. Development Environment

During development, you might have:

```text id="k2m9zs"
React
http://localhost:5173
```

and:

```text id="u4p7ca"
Express
http://localhost:5000
```

Notice:

```text id="z9r1fx"
5173 ≠ 5000
```

They are different origins.

That leads directly to our next concept:

```text id="f7w2nm"
CORS
```

---

# 11. What Is CORS?

CORS means:

```text id="q6v9ka"
Cross-Origin Resource Sharing
```

The browser has security rules controlling which origins can communicate with a server.

Your frontend:

```text id="j3k8px"
http://localhost:5173
```

tries to call:

```text id="m4x2qb"
http://localhost:5000
```

These are different origins.

The browser may block the request unless the backend allows the frontend origin.

---

# 12. Simple CORS Mental Model

```text id="a5r7nd"
React
http://localhost:5173
        │
        │ Request
        ↓
Express
http://localhost:5000
        │
        ↓
"Is this frontend allowed?"
        │
     ┌──┴──┐
    YES    NO
     ↓      ↓
  Response  Block
```

---

# 13. Express CORS

Express applications commonly use the `cors` middleware.

Conceptually:

```js id="r8t2vz"
app.use(cors());
```

This allows cross-origin requests according to the middleware's configuration.

In production, you generally don't want to blindly allow every origin.

You can configure the allowed frontend origin.

Conceptually:

```text id="s1k6qy"
Allowed Origin:
https://myfrontend.com
```

rather than:

```text id="w4p9jc"
Every website ❌
```

---

# 14. Development vs Production

Development:

```text id="y7q3mb"
React
localhost:5173

Express
localhost:5000
```

Production:

```text id="p5c8nx"
React
https://myapp.com

Express
https://api.myapp.com
```

Then:

```text id="h2v6ra"
React
  ↓
https://api.myapp.com
  ↓
Express
  ↓
Database
```

---

# 15. Environment Variables

You don't want to hardcode API URLs everywhere.

Instead, React can use a frontend environment variable.

For Vite:

```text id="m3x8qc"
VITE_API_URL
```

Example:

```text id="n8k4ys"
VITE_API_URL=http://localhost:5000
```

Then:

```js id="d5r2wf"
const API_URL = import.meta.env.VITE_API_URL;
```

And:

```js id="j1q7cv"
fetch(`${API_URL}/api/todos`);
```

When deploying:

```text id="s3h9mx"
VITE_API_URL=https://api.myapp.com
```

The code doesn't need to change.

---

# 16. Important Security Rule

Frontend environment variables are **not secrets**.

For example:

```text id="f6k2ra"
VITE_API_URL
```

is fine.

But don't put:

```text id="x9c4mz"
DATABASE_PASSWORD
JWT_SECRET
OPENAI_API_KEY
```

into React's environment variables.

Why?

Because frontend code is delivered to the user's browser.

```text id="z7p1nd"
Frontend
 ↓
Browser
 ↓
User can inspect it
```

Backend secrets stay on the server.

---

# 17. Complete Request Flow

Now combine everything:

```text id="e3m7qx"
                    BROWSER

             React Application
                    │
                    │ fetch()
                    ↓
             API Request
                    │
                    ↓
             CORS Check
                    │
                    ↓
              Express API
                    │
                    ↓
             Authentication
                    │
                    ↓
              Authorization
                    │
                    ↓
               Controller
                    │
                    ↓
                Database
                    │
                    ↓
              JSON Response
                    │
                    ↓
                 React
```

This is the basic full-stack connection.

---

# 18. Example: Full Todo Request

User clicks:

```text id="v6x2pa"
"Add Todo"
```

React:

```text id="b3r9kc"
POST /api/todos
```

Express:

```text id="m7q1zd"
Receive request
      ↓
Check authentication
      ↓
Validate todo
      ↓
Save to MongoDB
      ↓
Return created todo
```

React:

```text id="c4n8ys"
Receive JSON
      ↓
Update React state
      ↓
UI shows new todo
```

Complete:

```text id="s2x7vm"
USER
 ↓
REACT
 ↓
HTTP
 ↓
EXPRESS
 ↓
AUTH
 ↓
VALIDATION
 ↓
DATABASE
 ↓
EXPRESS
 ↓
JSON
 ↓
REACT
 ↓
UI
```

---

# 19. The Big Picture

You've already learned each piece separately:

```text id="k9r3wp"
React
   +
Express
   +
MongoDB
   +
JWT
   +
REST API
```

Now you're connecting them:

```text id="n4v7zs"
             FULL STACK APP

        ┌───────────────┐
        │     React     │
        │   Frontend    │
        └───────┬───────┘
                │
              HTTP
                │
        ┌───────▼───────┐
        │    Express    │
        │    Backend    │
        └───────┬───────┘
                │
             Mongoose
                │
        ┌───────▼───────┐
        │    MongoDB     │
        │    Database    │
        └───────────────┘
```

This is the foundation of a real full-stack application.

---

### Phase 14 Progress

```text id="q8m2vx"
[x] Connect React frontend to Express backend
[ ] CORS handling, protected routes
[ ] CI/CD basics: GitHub Actions
[ ] Deployment: frontend, backend, database
```

---------------------------------------------------------------------------------------------------------------


# CORS Handling + Protected Routes


# Part 1 — CORS

## 1. Why CORS Exists

Suppose your frontend runs on:

```text
http://localhost:5173
```

and your backend runs on:

```text
http://localhost:5000
```

The browser sees these as different origins.

```text
React
localhost:5173
     ↓
     ↓ HTTP request
     ↓
Express
localhost:5000
```

The browser has security rules that control this cross-origin communication.

That's where CORS comes in.

---

# 2. Express CORS

In Express, we commonly use the `cors` middleware.

Conceptually:

```js
import cors from "cors";

app.use(cors());
```

This allows cross-origin requests.

But in production, it's better to specify which frontend is allowed.

For example:

```js
app.use(
  cors({
    origin: "http://localhost:5173"
  })
);
```

Now the backend is saying:

```text
"This frontend is allowed to communicate with me."
```

---

# 3. Production Example

Development:

```text
Frontend
http://localhost:5173

Backend
http://localhost:5000
```

Production:

```text
Frontend
https://myapp.com

Backend
https://api.myapp.com
```

CORS configuration would allow:

```text
https://myapp.com
```

rather than allowing every website.

---

# 4. CORS Is a Browser Security Mechanism

Important distinction:

```text
CORS ≠ Authentication
CORS ≠ Authorization
```

CORS answers:

> "Is this browser origin allowed to make this cross-origin request?"

Authentication answers:

> "Who is this user?"

Authorization answers:

> "Is this user allowed to perform this action?"

These are separate security layers.

---

# Part 2 — Protected Routes

Now let's connect this with the authentication system you already learned.

Suppose your backend has:

```text
GET /api/todos
```

You don't want every person on the internet to access everyone's todos.

You want:

```text
Logged-in user
     ↓
GET /api/todos
     ↓
Their todos
```

This is a **protected route**.

---

# 5. Public vs Protected Routes

### Public route

Anyone can access it:

```text
GET /api/products
```

or:

```text
POST /api/auth/login
```

### Protected route

User must be authenticated:

```text
GET /api/todos
POST /api/todos
PATCH /api/todos/:id
DELETE /api/todos/:id
```

Conceptually:

```text
Public
   ↓
Anyone


Protected
   ↓
Authenticated user only
```

---

# 6. JWT Comes Into the Picture

You already learned JWT authentication.

The flow is:

```text
React
 ↓
Login
 ↓
Express
 ↓
Check email/password
 ↓
Create JWT
 ↓
React receives authentication information
```

Then later:

```text
React
 ↓
Request protected API
 ↓
Send authentication information
 ↓
Express
 ↓
Verify JWT
 ↓
Allow / Reject
```

---

# 7. Protected Route Middleware

Express middleware can protect routes.

Conceptually:

```js
router.get(
  "/todos",
  authMiddleware,
  getTodos
);
```

The flow becomes:

```text
GET /api/todos
       ↓
authMiddleware
       ↓
JWT valid?
   ┌───┴───┐
  YES      NO
   ↓        ↓
Controller  401
   ↓
Database
```

---

# 8. What Does `authMiddleware` Do?

Its job is roughly:

```text
1. Get authentication token
2. Check whether it exists
3. Verify the JWT
4. Extract user information
5. Attach user information to req
6. Continue to controller
```

Conceptually:

```js
req.user = decodedUser;
```

Then the controller knows:

```text
"This request belongs to user 123."
```

---

# 9. Why Attach `req.user`?

Imagine:

```text
GET /api/todos
```

The controller needs to know whose todos to retrieve.

The middleware can provide:

```text
req.user.id
```

Then:

```text
Find todos
WHERE userId = req.user.id
```

So:

```text
JWT
 ↓
Middleware
 ↓
req.user
 ↓
Controller
 ↓
User's data
```

---

# 10. Protected Todo Example

Suppose MongoDB contains:

```text
Todo A
userId → user123

Todo B
userId → user456
```

User `user123` sends:

```text
GET /api/todos
```

JWT tells the backend:

```text
userId = user123
```

Backend queries:

```text
Find todos where:

userId = user123
```

Result:

```text
Todo A
```

It must **not** return:

```text
Todo B ❌
```

This is authorization/data isolation.

---

# 11. Authentication vs Authorization

This is worth reinforcing because it is extremely important in full-stack applications.

### Authentication

```text
Who are you?
```

Example:

```text
JWT says:
userId = user123
```

### Authorization

```text
Are you allowed to do this?
```

Example:

```text
user123 tries to delete user456's todo
```

Backend should say:

```text
403 Forbidden
```

or otherwise deny the action.

---

# 12. Complete Protected Request

```text
React
  ↓
GET /api/todos
  ↓
Authentication Middleware
  ↓
Read JWT
  ↓
Verify JWT
  ↓
Get userId
  ↓
Controller
  ↓
Query MongoDB using userId
  ↓
Return only user's data
  ↓
React
```

---

# 13. What If JWT Is Missing?

Request:

```text
GET /api/todos
```

without authentication.

Middleware:

```text
Token?
 ↓
NO
```

Response:

```text
401 Unauthorized
```

Flow:

```text
Request
 ↓
Auth Middleware
 ↓
No valid authentication
 ↓
401
```

---

# 14. What If JWT Is Invalid?

```text
Request
 ↓
JWT exists
 ↓
Verify JWT
 ↓
Invalid
 ↓
401 Unauthorized
```

For example:

```text
Expired token
Malformed token
Wrong signature
```

The request should not reach the protected controller.

---

# 15. What If User Is Authenticated but Not Allowed?

Example:

```text
Admin-only endpoint
```

User is authenticated:

```text
JWT ✓
```

But role:

```text
user
```

Required:

```text
admin
```

Then:

```text
403 Forbidden
```

Flow:

```text
JWT valid
   ↓
Authentication ✓
   ↓
Authorization ✗
   ↓
403 Forbidden
```

---

# 16. CORS + Authentication Together

Now combine both:

```text
                    React
                      ↓
                HTTP Request
                      ↓
                    CORS
                      ↓
             Authentication
                      ↓
              Authorization
                      ↓
                Controller
                      ↓
                  MongoDB
                      ↓
                 Response
                      ↓
                    React
```

Each layer has a different responsibility.

```text
CORS
↓
Cross-origin browser permission

Authentication
↓
Identify user

Authorization
↓
Check permission

Controller
↓
Perform business logic

Database
↓
Store/retrieve data
```

---

# 17. Common Mistake

Don't think:

```text
CORS enabled
=
API is secure
```

No.

This:

```js
app.use(cors());
```

doesn't authenticate users.

You still need:

```text
Authentication
+
Authorization
+
Validation
+
Proper database access control
```

---

# 18. Another Common Mistake

Don't trust the frontend.

For example, React sends:

```json
{
  "userId": "user123"
}
```

The backend should **not** blindly trust:

```text
"userId": "user123"
```

A malicious user could change it to:

```json
{
  "userId": "user456"
}
```

Instead, derive the user identity from the authenticated token:

```text
JWT
 ↓
Verified userId
 ↓
req.user.id
```

Then use that value.

---

# 19. Full-Stack Security Picture

Your application now looks like:

```text
┌──────────────────┐
│      React       │
│    Frontend      │
└────────┬─────────┘
         │
         │ HTTP
         ↓
┌──────────────────┐
│       CORS       │
└────────┬─────────┘
         ↓
┌──────────────────┐
│ Authentication   │
│      JWT         │
└────────┬─────────┘
         ↓
┌──────────────────┐
│  Authorization   │
│ roles/ownership  │
└────────┬─────────┘
         ↓
┌──────────────────┐
│    Controller    │
└────────┬─────────┘
         ↓
┌──────────────────┐
│     MongoDB      │
└──────────────────┘
```

---

# 20. Example Full Request

User logs into your Todo application.

```text
Login
 ↓
JWT issued
```

User then opens their todos:

```text
React
 ↓
GET /api/todos
 ↓
CORS check
 ↓
JWT verification
 ↓
req.user.id = user123
 ↓
Find todos belonging to user123
 ↓
Return todos
 ↓
React displays them
```

Then user deletes a todo:

```text
React
 ↓
DELETE /api/todos/abc
 ↓
JWT verification
 ↓
Find todo abc
 ↓
Check todo.userId === req.user.id
 ↓
Allowed?
 ├── YES → Delete
 └── NO → 403
```

That's a proper protected full-stack operation.

---

# Key Things to Remember

### CORS

```text
Controls which browser origins
can make cross-origin requests.
```

### Authentication

```text
Identifies the user.
```

### Authorization

```text
Checks what the user is allowed to do.
```

### Protected Route

```text
A route that requires authentication
before its controller can execute.
```

### `req.user`

```text
Contains trusted user information
after successful authentication.
```

### 401

```text
Unauthenticated / invalid authentication.
```

### 403

```text
Authenticated, but not permitted.
```

---

# Phase 14 Progress

```text
[x] Connect React frontend to Express backend
[x] CORS handling + protected routes
[ ] CI/CD basics: GitHub Actions
[ ] Deployment: frontend, backend, database
```

---------------------------------------------------------------------------------------------------------------

# Full Stack Integration

## Step 3 — CI/CD Basics with GitHub Actions

Now we move to **CI/CD**, the next item in your original roadmap.

---

## 1. What is CI/CD?

### CI = Continuous Integration

Whenever you push code to GitHub, your project can automatically:

```text
Push code
   ↓
GitHub Actions
   ↓
Install dependencies
   ↓
Run tests
   ↓
Build project
   ↓
Success / Failure
```

So instead of manually checking:

> "Did my new code break anything?"

GitHub checks it automatically.

---

### CD = Continuous Delivery / Deployment

After CI succeeds, CD can automatically deliver/deploy your application.

For example:

```text
Developer pushes code
        ↓
CI
 ├── Install
 ├── Test
 └── Build
        ↓
Success
        ↓
CD
        ↓
Deploy application
```

So:

**CI → check the code**

**CD → deliver/deploy the code**

---

# 2. What is GitHub Actions?

GitHub Actions is GitHub's automation system.

You create a workflow file:

```text
.github/
└── workflows/
    └── ci.yml
```

GitHub reads this file and executes the instructions automatically.

---

# 3. Basic Workflow

A simple Node/React CI workflow looks like this:

```yaml
name: CI

on:
  push:
    branches: [main]

  pull_request:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: 20

      - name: Install dependencies
        run: npm ci

      - name: Run tests
        run: npm test

      - name: Build
        run: npm run build
```

Don't worry about memorizing this.

Let's understand the important pieces.

---

# 4. `name`

```yaml
name: CI
```

Just gives the workflow a name.

GitHub will show:

```text
CI
```

in the Actions section.

---

# 5. `on`

```yaml
on:
  push:
    branches: [main]

  pull_request:
    branches: [main]
```

This determines **when the workflow runs**.

For example:

```text
push to main
      ↓
run workflow
```

or:

```text
Pull Request → main
      ↓
run workflow
```

This is useful because every PR can automatically be checked before merging.

---

# 6. `jobs`

```yaml
jobs:
  build:
```

A workflow contains one or more **jobs**.

For example:

```text
Workflow
│
├── frontend
│
├── backend
│
└── tests
```

For now, we're keeping it simple with one job.

---

# 7. Runner

```yaml
runs-on: ubuntu-latest
```

GitHub needs a computer to execute your commands.

GitHub provides a temporary machine called a **runner**.

Here we're saying:

> Run this job on the latest Ubuntu environment.

---

# 8. Checkout Code

```yaml
- name: Checkout code
  uses: actions/checkout@v4
```

The runner starts as a fresh environment.

It needs your repository's code.

`checkout` downloads/checks out your GitHub repository into the runner.

Conceptually:

```text
GitHub Repository
       ↓
   checkout
       ↓
GitHub Runner
       ↓
Your project files
```

---

# 9. Setup Node

```yaml
- name: Setup Node
  uses: actions/setup-node@v4
  with:
    node-version: 20
```

Your project uses Node.js, so the runner needs Node installed.

This tells GitHub:

```text
Use Node.js 20
```

---

# 10. Install Dependencies

```yaml
- name: Install dependencies
  run: npm ci
```

This installs the packages from your project.

For CI, `npm ci` is commonly preferred because it performs a clean, reproducible installation based on the lockfile.

Conceptually:

```text
package-lock.json
       ↓
     npm ci
       ↓
node_modules
```

---

# 11. Run Tests

```yaml
- name: Run tests
  run: npm test
```

GitHub runs your project's tests.

If tests fail:

```text
npm test
   ↓
❌ failure
   ↓
Workflow fails
```

If they pass:

```text
npm test
   ↓
✅ success
```

---

# 12. Build

```yaml
- name: Build
  run: npm run build
```

This checks whether your application can successfully build.

For a Vite React application, for example:

```text
npm run build
      ↓
Vite builds React app
      ↓
dist/
```

If the build fails, GitHub reports the workflow as failed.

---

# 13. Why CI is Useful

Imagine you have a team.

Developer A changes:

```text
login code
```

Developer B changes:

```text
todo API
```

Developer C changes:

```text
React UI
```

Someone creates a Pull Request.

GitHub automatically does:

```text
Pull Request
     ↓
GitHub Actions
     ↓
Install
     ↓
Tests
     ↓
Build
     ↓
✅ Everything works
```

Now the team has more confidence merging the code.

---

# 14. Secrets

This is extremely important.

Never put things like this directly into your workflow:

```yaml
OPENAI_API_KEY: "real-secret-key"
```

or:

```yaml
MONGO_URI: "mongodb+srv://username:password..."
```

Instead, sensitive values should be stored using **GitHub Actions Secrets**.

Conceptually:

```text
GitHub Secrets
     │
     ├── MONGO_URI
     ├── JWT_SECRET
     └── OPENAI_API_KEY
              ↓
       GitHub Actions
```

The same principle you've already learned applies:

> **Secrets belong on the backend/server environment, not in frontend code.**

---

# 15. CI vs CD

Keep this simple:

| CI                           | CD                                    |
| ---------------------------- | ------------------------------------- |
| Checks code                  | Delivers/deploys code                 |
| Runs tests                   | Deploys application                   |
| Runs build                   | Publishes new version                 |
| Usually triggered by push/PR | Usually triggered after successful CI |

Typical production flow:

```text
Developer
    ↓
git push
    ↓
GitHub
    ↓
CI
 ├── install
 ├── test
 └── build
    ↓
✅
    ↓
CD
    ↓
Deploy
    ↓
Live application
```

---

# 16. Your Full Stack Project

Eventually your project will look something like:

```text
React Frontend
      │
      ↓
Express Backend
      │
      ↓
MongoDB
```

And CI/CD can automate:

```text
GitHub
  │
  ├── Frontend tests/build
  │
  ├── Backend tests
  │
  └── Deployment
```

You can have separate workflows for frontend and backend, or organize multiple jobs in one workflow.

For now, the important thing is understanding the **CI/CD concept and GitHub Actions workflow structure**.

---

## Step 3 Complete

Your Phase 14 progress is now:

```text
[x] Connect React frontend to Express backend
[x] CORS handling + protected routes
[x] CI/CD basics: GitHub Actions
[ ] Deployment: frontend, backend, database
```


---------------------------------------------------------------------------------------------------------------


# Phase 14 — Full Stack Integration

## Step 4 — Deployment

This is the **last step of Phase 14** in your original roadmap.

The goal is to take your local full-stack application:

```text
React
Express
MongoDB
```

and make it available online.

---

## 1. What is Deployment?

Until now, you might have:

```text
Your PC
│
├── React → localhost:5173
├── Express → localhost:5000
└── MongoDB
```

Only your computer can access it.

Deployment means putting these parts on internet-accessible services:

```text
                    INTERNET
                       │
          ┌────────────┴────────────┐
          ↓                         ↓
   React Frontend             Express Backend
          │                         │
          │                         ↓
          │                      MongoDB
          │
       Browser
```

---

# 2. Frontend Deployment

Your React application can be deployed to services such as:

* Vercel
* Netlify

For example:

```text
React project
     ↓
Build
     ↓
dist/
     ↓
Vercel / Netlify
     ↓
https://your-app...
```

The hosting service serves the built frontend to users.

---

# 3. Backend Deployment

Your Express server also needs to run somewhere online.

Services from your roadmap include:

* Render
* Railway

For example:

```text
Express project
      ↓
Deploy
      ↓
Cloud server
      ↓
https://api.your-app...
```

Your Express application is now running continuously on a remote server.

---

# 4. Database Deployment

Your local MongoDB shouldn't be used as the production database.

Instead, you can use a hosted database such as:

**MongoDB Atlas**

Architecture:

```text
React
  ↓
Internet
  ↓
Express on Render/Railway
  ↓
MongoDB Atlas
```

The backend connects to Atlas using a connection string.

---

# 5. Environment Variables

This is one of the most important parts of deployment.

Locally you might have:

```env
PORT=5000
MONGO_URI=your_database_connection
JWT_SECRET=your_secret
```

In production, these values are configured through the hosting platform's environment-variable settings.

You **don't** commit `.env` containing secrets to GitHub.

---

# 6. Frontend Environment Variables

Your React frontend needs to know where the production backend is.

For example:

```env
VITE_API_URL=https://your-backend...
```

Then:

```js
fetch(`${import.meta.env.VITE_API_URL}/api/todos`);
```

So during development:

```text
React
 ↓
http://localhost:5000
```

Production:

```text
React
 ↓
https://your-backend...
```

The frontend code can remain essentially the same.

Important:

> `VITE_*` variables are exposed to the frontend, so **never put secrets** such as JWT secrets, database passwords, or private API keys there.

---

# 7. Production Architecture

After deployment, your application might look like:

```text
                  USER
                   │
                   ↓
             React Frontend
              Vercel/Netlify
                   │
                   │ HTTPS
                   ↓
             Express Backend
              Render/Railway
                   │
          ┌────────┴────────┐
          ↓                 ↓
      MongoDB Atlas      External APIs
```

Authentication:

```text
User
 ↓
React Login
 ↓
Express /api/auth/login
 ↓
MongoDB
 ↓
JWT
 ↓
React
 ↓
Protected API requests
```

---

# 8. CORS Changes in Production

Earlier we learned:

```text
React:   localhost:5173
Express: localhost:5000
```

These are different origins.

After deployment they might become:

```text
Frontend:
https://mytodo.vercel.app

Backend:
https://mytodo-api.onrender.com
```

They are still different origins.

So your Express server should allow the **actual frontend origin**.

Conceptually:

```js
app.use(cors({
  origin: "https://mytodo.vercel.app"
}));
```

Don't blindly use:

```js
origin: "*"
```

for an application that needs controlled authenticated access.

---

# 9. Deployment Flow

Your complete development workflow becomes:

```text
Write code
   ↓
Git commit
   ↓
Git push
   ↓
GitHub
   ↓
GitHub Actions
   ↓
Tests
   ↓
Build
   ↓
Deployment
   ↓
Live application
```

This connects the two things we just learned:

**CI/CD + deployment**

---

# 10. Development vs Production

### Development

```text
React
localhost:5173

Express
localhost:5000

MongoDB
local/Atlas
```

### Production

```text
React
Vercel/Netlify

Express
Render/Railway

MongoDB
Atlas
```

The architecture stays the same.

Only the environments change.

---

# 11. What Happens When You Push a New Version?

Suppose your application is already live.

You change your React UI:

```text
Make change
   ↓
git commit
   ↓
git push
   ↓
GitHub
   ↓
CI checks
   ↓
Build succeeds
   ↓
Deployment
   ↓
New version goes live
```

This is the basic idea behind modern continuous deployment.

---

# 12. Your Phase 14 Build

The original roadmap says:

> **Build: full CRUD app, auth, deployed live, CI pipeline**

So the final project should eventually have:

```text
Frontend
├── React
├── Routing
├── Authentication UI
└── CRUD UI

Backend
├── Express
├── Authentication
├── Authorization
├── Validation
├── CRUD
└── API

Database
└── MongoDB

Infrastructure
├── GitHub
├── GitHub Actions
├── Frontend hosting
├── Backend hosting
└── MongoDB Atlas
```

And the final flow:

```text
                    ┌──────────────┐
                    │    GitHub    │
                    └──────┬───────┘
                           │
                      CI/CD Pipeline
                           │
             ┌─────────────┴─────────────┐
             ↓                           ↓
       React Frontend              Express Backend
       Vercel/Netlify              Render/Railway
                                         │
                                         ↓
                                  MongoDB Atlas
```

---

# Phase 14 Complete

```text
[x] Connect React frontend to Express backend
[x] CORS handling + protected routes
[x] CI/CD basics: GitHub Actions
[x] Deployment: frontend, backend, database
```

**Phase 14 — Full Stack Integration: COMPLETE**

---------------------------------------------------------------------------------------------------------------