# Phase 7 — State Management

Now we start the next section of your roadmap:

```text
## 7. State Management

- [ ] Context API
- [ ] Redux Toolkit
- [ ] Form state/validation: React Hook Form + Zod

Build:
auth state app
cart system
validated signup form
```

We'll **not jump into Redux yet**.

First, we need to understand **why state management is needed**.

---

# Step 1 — Why Do We Need State Management?

You already know `useState`.

For example:

```tsx
const [count, setCount] = useState(0);
```

The state belongs to that component.

```text
App
 ↓
count
```

That's easy.

But imagine our application becomes bigger:

```text
App
│
├── Navbar
│
├── Home
│
├── Products
│
└── Dashboard
```

Suppose the user logs in.

We want many components to know:

```text
user = William
logged in = true
```

For example:

```text
Navbar
  ↓
Show "William"

Home
  ↓
Welcome William

Dashboard
  ↓
Show William's dashboard

Profile
  ↓
Show William's profile
```

Now the same state is needed by many components.

---

# Step 2 — The Problem

Suppose the state is here:

```text
App
 ↓
user
```

But `Profile` needs the user.

Maybe the structure is:

```text
App
 ↓
Dashboard
 ↓
Profile
```

We could pass it:

```text
App
 ↓ user
Dashboard
 ↓ user
Profile
```

But what if the structure becomes:

```text
App
 ↓
Dashboard
 ↓
Layout
 ↓
Sidebar
 ↓
Profile
```

We might have to pass `user` through components that don't even need it.

That's called **prop drilling**.

---

# Step 3 — What Is Prop Drilling?

Imagine:

```text
App
 │
 │ user
 ↓
Dashboard
 │
 │ user
 ↓
Layout
 │
 │ user
 ↓
Sidebar
 │
 │ user
 ↓
Profile
```

Only `Profile` actually needs `user`.

But we have to pass it through:

```text
Dashboard
Layout
Sidebar
```

This becomes annoying.

That's the problem we're trying to solve.

---

# Step 4 — Context API

React provides **Context API**.

Context allows us to make some state available to multiple components without passing it manually through every component.

Think:

```text
Without Context:

App
 ↓
Dashboard
 ↓
Layout
 ↓
Sidebar
 ↓
Profile

user must travel ↓ ↓ ↓ ↓


With Context:

        User Context
        ↓    ↓    ↓
      Navbar Profile Dashboard
```

Components can access the shared state directly.

---

# Step 5 — Simple Real-World Example

Imagine an authentication state:

```text
UserContext

user:
    name: "William"
    loggedIn: true
```

Then:

```text
Navbar
   ↓
UserContext
   ↓
"Welcome William"
```

And:

```text
Profile
   ↓
UserContext
   ↓
"William's Profile"
```

And:

```text
Dashboard
   ↓
UserContext
   ↓
"Welcome back, William"
```

We don't need:

```text
App → Dashboard → Profile
```

to manually pass the user.

---

# Step 6 — Context Is Not a Replacement for `useState`

This is important.

You might think:

> "Context replaces useState."

No.

They have different jobs.

### `useState`

Manages state:

```text
count
search
user
cart
```

### Context

Makes shared data available to components.

So they often work together:

```text
useState
   ↓
stores state

Context
   ↓
shares state
```

For example:

```text
UserProvider
    ↓
useState()
    ↓
user state
    ↓
Context
    ↓
many components
```

---

# Step 7 — Our Next Build

We're going to build a simple **authentication state app**.

Something like:

```text
-------------------------
        My App
-------------------------

Navbar

Hello, William

[ Logout ]
```

When the user isn't logged in:

```text
-------------------------
        My App
-------------------------

Navbar

Please log in

[ Login ]
```

The state will be shared between components.

---

# Step 8 — Our Learning Order

We'll learn Context API gradually:

```text
Step 1
Why state management?
        ↓
Step 2
Prop drilling
        ↓
Step 3
Create Context
        ↓
Step 4
Provider
        ↓
Step 5
useContext
        ↓
Step 6
Create Auth Context
        ↓
Step 7
Login / Logout
        ↓
Step 8
Build auth state app
```

---

# The Main Idea

For now, remember only this:

```text
useState
   ↓
stores state
```

and:

```text
Context
   ↓
shares state between components
```

So:

```text
          Shared State
               ↓
            Context
          ↙    ↓    ↘
      Navbar  Home  Profile
```

**Next → Step 2: We'll understand Prop Drilling with a small example before creating Context.**

-------------------------------------------------------------------------------------------------------------------------------------------

# Step 2 — Prop Drilling

Before learning Context API, we need to clearly understand the problem it solves.

That problem is **prop drilling**.

---

## 1. First, Remember Props

You already learned that props allow a parent to send data to a child.

Example:

```tsx
function App() {
    const name = "William";

    return <Profile name={name} />;
}
```

Then:

```tsx
function Profile({ name }) {
    return <h1>Hello {name}</h1>;
}
```

The flow is:

```text
App
 ↓
name
 ↓
Profile
```

That's perfectly fine.

---

# 2. The Problem Starts With Multiple Levels

Imagine this component structure:

```text
App
 ↓
Dashboard
 ↓
Profile
```

`App` has the user's name.

`Profile` needs it.

So we pass it:

```text
App
 ↓ name
Dashboard
 ↓ name
Profile
```

No big problem yet.

---

# 3. What If There Are More Components?

Now imagine:

```text
App
 ↓
Dashboard
 ↓
Layout
 ↓
Sidebar
 ↓
Profile
```

But only `Profile` needs the user's name.

Still, we have to pass the prop through every component:

```text
App
 ↓ name
Dashboard
 ↓ name
Layout
 ↓ name
Sidebar
 ↓ name
Profile
```

The intermediate components don't even use `name`.

They are just **passing it along**.

That's prop drilling.

---

# 4. Simple Example

Imagine:

```tsx
function App() {
    const user = "William";

    return <Dashboard user={user} />;
}
```

Then:

```tsx
function Dashboard({ user }) {
    return <Profile user={user} />;
}
```

Then:

```tsx
function Profile({ user }) {
    return <h1>Hello {user}</h1>;
}
```

Notice:

`Dashboard` doesn't actually need `user`.

It only receives it so that it can send it to `Profile`.

That's the problem.

---

# 5. Visualize It

```text
App
 │
 │ user
 ↓
Dashboard
 │
 │ user
 ↓
Profile
```

The data is traveling through `Dashboard`.

But:

```text
Dashboard
     ↓
doesn't need user
```

It is just a middleman.

---

# 6. With More Levels

Imagine:

```text
App
 │
 │ user
 ↓
Dashboard
 │
 │ user
 ↓
Layout
 │
 │ user
 ↓
Sidebar
 │
 │ user
 ↓
Profile
```

Now imagine you add another component:

```text
App
 ↓
Dashboard
 ↓
Layout
 ↓
Sidebar
 ↓
UserSection
 ↓
Profile
```

Now the prop has to travel through even more components.

This becomes difficult to maintain.

---

# 7. Why Is This a Problem?

There are three main issues.

### 1. Lots of unnecessary props

```tsx
<Dashboard user={user} />
```

```tsx
<Layout user={user} />
```

```tsx
<Sidebar user={user} />
```

Most of these components don't need `user`.

---

### 2. Components become harder to maintain

Suppose you rename:

```text
user
```

to:

```text
currentUser
```

You may have to update many components.

---

### 3. Deep component trees become messy

Instead of:

```text
App
 ↓
Profile
```

you might have:

```text
App
 ↓
A
 ↓
B
 ↓
C
 ↓
D
 ↓
Profile
```

and the prop gets passed through every level.

---

# 8. This Is Where Context Helps

Instead of:

```text
App
 ↓ user
A
 ↓ user
B
 ↓ user
C
 ↓ user
Profile
```

we can use Context:

```text
          User Context
          ↓    ↓    ↓
         App   A    Profile
```

`Profile` can access the shared user directly.

The middle components don't need to receive or forward it.

---

# 9. Important: Don't Avoid Props Completely

This does **not** mean:

> "Props are bad."

Props are extremely important in React.

Use props when:

```text
Parent
  ↓
Child
```

needs to communicate.

For example:

```text
ProductList
    ↓
ProductCard
```

Passing:

```text
product
```

as a prop makes perfect sense.

The problem is mainly when data needs to travel through **many unrelated intermediate components**.

---

# 10. Props vs Context

A simple way to think about it:

### Props

```text
Parent
  ↓
Child
```

Good for direct component relationships.

### Context

```text
Shared data
 ↓   ↓   ↓
A   B   C
```

Useful when many components need the same data.

---

# 11. Our Authentication Example

We're going to use this exact problem in our build.

Imagine:

```text
App
│
├── Navbar
│
├── Home
│
└── Dashboard
     │
     └── Profile
```

We want all of these to know:

```text
user
```

Without Context:

```text
App
 │
 │ user
 ├────────→ Navbar
 │
 └→ Dashboard
       │
       │ user
       ↓
     Profile
```

With Context:

```text
           UserContext
          ↙     ↓     ↘
      Navbar   Home   Profile
```

Much cleaner.

---

# 12. One Important Rule

Don't immediately think:

> "Many components = Context."

That's not always true.

If the relationship is simple:

```text
Parent → Child
```

just use props.

Use Context when some data is genuinely **shared across a component tree** and passing it through many layers becomes inconvenient.

---

## Progress

```text
Phase 7 — State Management

Context API

[x] Why state management?
[x] Prop drilling
[ ] Create Context
[ ] Provider
[ ] useContext
[ ] Auth Context
[ ] Login / Logout
[ ] Build auth state app
```

**Next → Step 3: We'll create our first Context from scratch and see exactly what `createContext()` does.**

-------------------------------------------------------------------------------------------------------------------------------------------

# Step 3 — Create Our First Context

Now we solve the **prop drilling problem** using React Context.

We'll keep it very simple first. We are **not** building authentication yet.

Our goal is only to understand:

```text
createContext()
```

---

# 1. What Are We Trying to Do?

Suppose we have:

```text
App
│
├── Navbar
│
└── Profile
```

We want both `Navbar` and `Profile` to access the same value:

```text
user = "William"
```

Instead of:

```text
App
 ↓ user
Navbar

App
 ↓ user
Profile
```

we'll create a shared Context.

---

# 2. Create a Context

Create a new file:

```text
src
│
├── App.tsx
├── Navbar.tsx
├── Profile.tsx
└── UserContext.tsx
```

Open:

```text
UserContext.tsx
```

and write:

```tsx
import { createContext } from "react";

const UserContext = createContext("");

export default UserContext;
```

That's our first Context.

---

# 3. What Does `createContext()` Do?

This:

```tsx
createContext("")
```

creates a **Context object**.

Think of it as creating a shared place for data.

```text
UserContext
    ↓
shared data
```

The empty string:

```tsx
""
```

is the **default value**.

So:

```tsx
const UserContext = createContext("");
```

means:

> Create a Context whose default value is an empty string.

---

# 4. Context Does Not Automatically Contain Our User

This is important.

When we write:

```tsx
const UserContext = createContext("");
```

we haven't actually stored:

```text
William
```

yet.

We've only created the **Context**.

Think:

```text
createContext()
      ↓
creates the container
```

Later we'll put data into it using a **Provider**.

---

# 5. Context Has Two Important Parts

You'll commonly see:

```text
Context
   │
   ├── Provider
   │
   └── Consumer
```

In modern React, instead of manually using a Consumer, we commonly use:

```text
useContext()
```

So our learning flow is:

```text
createContext()
      ↓
Provider
      ↓
useContext()
      ↓
Component gets shared data
```

---

# 6. Create the Provider

Let's modify `UserContext.tsx`.

```tsx
import { createContext } from "react";

const UserContext = createContext("");

export default UserContext;
```

The Context itself already gives us:

```tsx
UserContext.Provider
```

We don't need to create another component yet.

---

# 7. Use the Provider

Open `App.tsx`.

```tsx
import UserContext from "./UserContext";
import Navbar from "./Navbar";
import Profile from "./Profile";

function App() {
    return (
        <UserContext.Provider value="William">
            <Navbar />
            <Profile />
        </UserContext.Provider>
    );
}

export default App;
```

Now something important has happened.

We have:

```text
UserContext.Provider
        │
        │ value = "William"
        ↓
 ┌──────┴──────┐
 ↓             ↓
Navbar       Profile
```

Both components are inside the Provider.

Therefore, they can access the value.

---

# 8. What Is a Provider?

Look at:

```tsx
<UserContext.Provider value="William">
```

The Provider says:

> "Any component inside me can access this Context value."

So:

```text
Provider
   ↓
Children
   ↓
can access Context
```

---

# 9. What Does `value` Mean?

Here:

```tsx
<UserContext.Provider value="William">
```

we are providing:

```text
William
```

through the Context.

We could provide other values too.

For example:

```tsx
value="John"
```

or:

```tsx
value={user}
```

We'll eventually provide an object containing authentication information.

---

# 10. Access the Context

Now let's make `Profile.tsx` read the value.

We use:

```tsx
useContext()
```

```tsx
import { useContext } from "react";
import UserContext from "./UserContext";

function Profile() {
    const user = useContext(UserContext);

    return <h1>Hello {user}</h1>;
}

export default Profile;
```

Now:

```text
UserContext.Provider
       ↓
value = "William"
       ↓
useContext(UserContext)
       ↓
user = "William"
```

The screen shows:

```text
Hello William
```

---

# 11. Navbar Can Also Access It

`Navbar.tsx`:

```tsx
import { useContext } from "react";
import UserContext from "./UserContext";

function Navbar() {
    const user = useContext(UserContext);

    return <nav>Welcome, {user}</nav>;
}

export default Navbar;
```

Now both components use the same Context:

```text
              UserContext
                   │
            value = William
              ↙         ↘
          Navbar       Profile
             ↓            ↓
      Welcome, William  Hello William
```

---

# 12. Full Example

Our files:

```text
src
│
├── App.tsx
├── Navbar.tsx
├── Profile.tsx
└── UserContext.tsx
```

### `UserContext.tsx`

```tsx
import { createContext } from "react";

const UserContext = createContext("");

export default UserContext;
```

### `App.tsx`

```tsx
import UserContext from "./UserContext";
import Navbar from "./Navbar";
import Profile from "./Profile";

function App() {
    return (
        <UserContext.Provider value="William">
            <Navbar />
            <Profile />
        </UserContext.Provider>
    );
}

export default App;
```

### `Navbar.tsx`

```tsx
import { useContext } from "react";
import UserContext from "./UserContext";

function Navbar() {
    const user = useContext(UserContext);

    return <nav>Welcome, {user}</nav>;
}

export default Navbar;
```

### `Profile.tsx`

```tsx
import { useContext } from "react";
import UserContext from "./UserContext";

function Profile() {
    const user = useContext(UserContext);

    return <h1>Hello {user}</h1>;
}

export default Profile;
```

---

# 13. Look at the Difference

### Before Context

We would have:

```text
App
│
├── user
│
├── Navbar
│     ↑
│     user prop
│
└── Profile
      ↑
      user prop
```

With Context:

```text
       UserContext
            │
            │ William
            ↓
     Provider
       /    \
      ↓      ↓
  Navbar   Profile
```

No need to pass `user` through props.

---

# 14. Three New Things

You just learned three important pieces:

### `createContext()`

Creates the Context.

```tsx
const UserContext = createContext("");
```

### `Provider`

Provides a value.

```tsx
<UserContext.Provider value="William">
```

### `useContext()`

Reads the value.

```tsx
const user = useContext(UserContext);
```

The complete flow:

```text
createContext()
      ↓
Context created
      ↓
Provider
      ↓
value provided
      ↓
useContext()
      ↓
component receives value
```

---

# 15. One Very Important Point

The component must be **inside the Provider** to receive the Provider's value.

For example:

```text
UserContext.Provider
│
├── Navbar       ✅
│
└── Profile      ✅
```

Both can access:

```text
William
```

But:

```text
UserContext.Provider
│
└── Navbar       ✅

Profile          ❌
```

`Profile` is outside the Provider.

It won't receive `"William"` from that Provider.

---

# Current Progress

```text
Phase 7 — State Management

Context API

[x] Why state management?
[x] Prop drilling
[x] createContext()
[x] Provider
[x] useContext()
[ ] Combine Context + useState
[ ] Auth Context
[ ] Login / Logout
[ ] Build auth state app
```

The next important step is **combining `useState` with Context**.

That's where Context becomes much more useful:

```text
useState
   ↓
stores changing data

Context
   ↓
shares that data
```

**Next → Step 4: Context + `useState` — we'll make the user value actually change instead of hard-coding `"William"`.**


-------------------------------------------------------------------------------------------------------------------------------------------

# Step 4 — Context + `useState`

Now we're going to combine two things you already know:

```text
useState
+
Context
```

This is where Context becomes really useful.

---

# 1. What We Have Right Now

Currently, we have:

```tsx
<UserContext.Provider value="William">
```

The problem is that `"William"` is **hard-coded**.

It never changes.

We want something like:

```text
Not logged in
     ↓
User clicks Login
     ↓
user = "William"
     ↓
UI changes
```

So we need **state**.

---

# 2. `useState` Stores the Changing Data

We can write:

```tsx
const [user, setUser] = useState("");
```

Now:

```text
user
 ↓
current value

setUser()
 ↓
changes the value
```

For example:

```text
Initial:
user = ""

After login:
setUser("William")

Result:
user = "William"
```

---

# 3. Why Add Context?

`useState` can store the user:

```text
useState
   ↓
user
```

But we want multiple components to access that user:

```text
Navbar
Profile
Dashboard
```

So:

```text
useState
   ↓
user
   ↓
Context
   ↓
Navbar / Profile / Dashboard
```

This is the important relationship.

---

# 4. Move the State Into the Context

Instead of putting the state in `App.tsx`, we'll create a reusable provider.

Our files become:

```text id="7n0h0p"
src
│
├── App.tsx
├── Navbar.tsx
├── Profile.tsx
└── UserContext.tsx
```

---

# 5. `UserContext.tsx`

We'll create a component called `UserProvider`.

```tsx id="1whu9q"
import { createContext, useState } from "react";

const UserContext = createContext(null);

function UserProvider({ children }) {
    const [user, setUser] = useState("");

    return (
        <UserContext.Provider value={{ user, setUser }}>
            {children}
        </UserContext.Provider>
    );
}

export { UserContext, UserProvider };
```

There is a lot happening here, so let's break it down.

---

# 6. `useState`

We have:

```tsx id="i9blt5"
const [user, setUser] = useState("");
```

Initially:

```text id="ljq0c9"
user = ""
```

When we call:

```tsx id="f5r8pn"
setUser("William");
```

it becomes:

```text id="y5j4t2"
user = "William"
```

---

# 7. The Provider

We then provide both:

```text id="qkqf7m"
user
setUser
```

through Context:

```tsx id="2bhh6q"
<UserContext.Provider value={{ user, setUser }}>
```

This is important.

We're not just sharing the value:

```text
William
```

We're sharing:

```text
user
+
setUser
```

So components can both:

```text
read the user
```

and:

```text
change the user
```

---

# 8. What Is `children`?

You may see this:

```tsx id="n2i8ly"
function UserProvider({ children })
```

`children` means:

> Whatever we put inside `UserProvider`.

For example:

```tsx id="s1s2xh"
<UserProvider>
    <Navbar />
    <Profile />
</UserProvider>
```

Then:

```text id="z8q1da"
children
   ↓
Navbar
Profile
```

So this:

```tsx id="6h3b7c"
{children}
```

renders those components.

---

# 9. Use the Provider in `App.tsx`

Now:

```tsx id="9z4pl4"
import { UserProvider } from "./UserContext";
import Navbar from "./Navbar";
import Profile from "./Profile";

function App() {
    return (
        <UserProvider>
            <Navbar />
            <Profile />
        </UserProvider>
    );
}

export default App;
```

Our structure is now:

```text id="b2w7q4"
UserProvider
      │
      ├── Navbar
      │
      └── Profile
```

Both can access the shared state.

---

# 10. Read the User in `Navbar`

```tsx id="m50e1p"
import { useContext } from "react";
import { UserContext } from "./UserContext";

function Navbar() {
    const { user } = useContext(UserContext);

    return (
        <nav>
            {user ? `Welcome, ${user}` : "Not logged in"}
        </nav>
    );
}

export default Navbar;
```

Now:

```text id="0k6q6k"
user = ""
```

shows:

```text
Not logged in
```

If:

```text id="yyaj5r"
user = "William"
```

it shows:

```text
Welcome, William
```

---

# 11. Login From `Profile`

We can also get `setUser`:

```tsx id="9e2g72"
import { useContext } from "react";
import { UserContext } from "./UserContext";

function Profile() {
    const { user, setUser } = useContext(UserContext);

    return (
        <div>
            {user ? (
                <h1>Hello {user}</h1>
            ) : (
                <button onClick={() => setUser("William")}>
                    Login
                </button>
            )}
        </div>
    );
}

export default Profile;
```

Now the interesting part happens.

---

# 12. Click Login

Initially:

```text id="g3fdzo"
user = ""
```

The screen:

```text id="ajh6lb"
Navbar

Not logged in

[ Login ]
```

User clicks:

```text id="k1g4kz"
Login
```

This runs:

```tsx id="xq0czq"
setUser("William")
```

Then:

```text id="53f4pv"
user
 ↓
"William"
```

Because the state changed, React re-renders the components using that state.

---

# 13. The UI Changes

Navbar receives the new Context value:

```text id="bdy7s8"
user = "William"
```

So:

```text id="qgsg5s"
Not logged in
```

becomes:

```text id="gh6f6j"
Welcome, William
```

And Profile changes:

```text id="b8v2ml"
[ Login ]
```

to:

```text id="3i5l6q"
Hello William
```

The complete flow is:

```text id="x0o9rj"
Click Login
     ↓
setUser("William")
     ↓
useState changes
     ↓
Context value changes
     ↓
Components using Context re-render
     ↓
UI changes
```

---

# 14. This Is the Important Concept

We now have:

```text id="k9t1xj"
             UserProvider
                  │
          ┌───────┴───────┐
          ↓               ↓
        Navbar          Profile
          ↑               ↑
          └──── user ─────┘
                  ↑
              useState
```

So:

**`useState` manages the state.**

**Context shares the state.**

---

# 15. Full Example

### `UserContext.tsx`

```tsx id="0m1i0p"
import { createContext, useState } from "react";

const UserContext = createContext(null);

function UserProvider({ children }) {
    const [user, setUser] = useState("");

    return (
        <UserContext.Provider value={{ user, setUser }}>
            {children}
        </UserContext.Provider>
    );
}

export { UserContext, UserProvider };
```

### `App.tsx`

```tsx id="c2zq0y"
import { UserProvider } from "./UserContext";
import Navbar from "./Navbar";
import Profile from "./Profile";

function App() {
    return (
        <UserProvider>
            <Navbar />
            <Profile />
        </UserProvider>
    );
}

export default App;
```

### `Navbar.tsx`

```tsx id="1qkz3f"
import { useContext } from "react";
import { UserContext } from "./UserContext";

function Navbar() {
    const { user } = useContext(UserContext);

    return (
        <nav>
            {user ? `Welcome, ${user}` : "Not logged in"}
        </nav>
    );
}

export default Navbar;
```

### `Profile.tsx`

```tsx id="m8l1d7"
import { useContext } from "react";
import { UserContext } from "./UserContext";

function Profile() {
    const { user, setUser } = useContext(UserContext);

    return (
        <div>
            {user ? (
                <h1>Hello {user}</h1>
            ) : (
                <button onClick={() => setUser("William")}>
                    Login
                </button>
            )}
        </div>
    );
}

export default Profile;
```

---

# 16. What Did We Add?

Previously:

```text id="3q9u8a"
Context
 ↓
static value
```

Now:

```text id="c8w0g3"
Context
 ↓
useState
 ↓
changing value
```

That's a major step.

---

# 17. Our Progress

```text id="ukj7o2"
Phase 7 — State Management

Context API

[x] Why state management?
[x] Prop drilling
[x] createContext()
[x] Provider
[x] useContext()
[x] Context + useState
[ ] Auth Context
[ ] Login / Logout
[ ] Build auth state app
```

**`useState` → Provider → Context → `useContext()` → component**

We are now ready for the more realistic version.

**Next → Step 5: Build an `AuthContext` with `login()` and `logout()` instead of directly calling `setUser()` from components.**


-------------------------------------------------------------------------------------------------------------------------------------------

# Step 5 — Auth Context

Now we'll improve what we built in the previous step.

Previously, `Profile` directly did this:

```tsx
setUser("William")
```

That works, but it's not a good structure for a real application.

Instead, we want:

```text
Profile
   ↓
login()
   ↓
AuthContext
   ↓
setUser()
```

And for logout:

```text
Profile
   ↓
logout()
   ↓
AuthContext
   ↓
setUser(null)
```

This keeps the authentication logic inside the Auth Context.

---

# 1. What Is Auth Context?

**Auth** means **Authentication**.

Authentication is about things like:

```text
Is the user logged in?
Who is the user?
Login
Logout
```

So our Context will manage:

```text
user
login()
logout()
```

Think:

```text
              AuthContext
             /     |      \
            ↓      ↓       ↓
          user   login()  logout()
```

Any component that needs authentication information can access it.

---

# 2. Our File Structure

We'll now have:

```text
src
│
├── App.tsx
│
├── AuthContext.tsx
│
├── Navbar.tsx
│
└── Profile.tsx
```

No new folder is required yet.

---

# 3. Create `AuthContext.tsx`

```tsx
import { createContext, useState } from "react";

const AuthContext = createContext(null);

function AuthProvider({ children }) {
    const [user, setUser] = useState(null);

    function login() {
        setUser("William");
    }

    function logout() {
        setUser(null);
    }

    return (
        <AuthContext.Provider value={{ user, login, logout }}>
            {children}
        </AuthContext.Provider>
    );
}

export { AuthContext, AuthProvider };
```

Let's understand this carefully.

---

# 4. Our State

```tsx
const [user, setUser] = useState(null);
```

Initially:

```text
user = null
```

That means:

```text
Not logged in
```

After login:

```text
user = "William"
```

After logout:

```text
user = null
```

So:

```text
null
 ↓
Not logged in
```

and:

```text
"William"
 ↓
Logged in
```

---

# 5. Create `login()`

Instead of allowing components to directly call:

```tsx
setUser("William")
```

we create:

```tsx
function login() {
    setUser("William");
}
```

Now the component doesn't need to know **how** login works.

It simply says:

```text
login()
```

The Auth Context handles the actual state change.

---

# 6. Create `logout()`

Same idea:

```tsx
function logout() {
    setUser(null);
}
```

So:

```text
login()
 ↓
user = "William"
```

and:

```text
logout()
 ↓
user = null
```

---

# 7. Provide Everything

Look at:

```tsx
<AuthContext.Provider
    value={{ user, login, logout }}
>
```

We're sharing three things:

```text
user
login
logout
```

So components can:

```text
read user
call login()
call logout()
```

---

# 8. Update `App.tsx`

Now we wrap our application with `AuthProvider`.

```tsx
import { AuthProvider } from "./AuthContext";
import Navbar from "./Navbar";
import Profile from "./Profile";

function App() {
    return (
        <AuthProvider>
            <Navbar />
            <Profile />
        </AuthProvider>
    );
}

export default App;
```

The structure is:

```text
AuthProvider
│
├── Navbar
│
└── Profile
```

Both components can access AuthContext.

---

# 9. Update `Navbar.tsx`

```tsx
import { useContext } from "react";
import { AuthContext } from "./AuthContext";

function Navbar() {
    const { user } = useContext(AuthContext);

    return (
        <nav>
            {user ? `Welcome, ${user}` : "Not logged in"}
        </nav>
    );
}

export default Navbar;
```

The Navbar only needs:

```text
user
```

It doesn't need:

```text
login()
logout()
```

So we don't use them here.

---

# 10. Update `Profile.tsx`

```tsx
import { useContext } from "react";
import { AuthContext } from "./AuthContext";

function Profile() {
    const { user, login, logout } = useContext(AuthContext);

    return (
        <div>
            {user ? (
                <div>
                    <h1>Hello {user}</h1>

                    <button onClick={logout}>
                        Logout
                    </button>
                </div>
            ) : (
                <button onClick={login}>
                    Login
                </button>
            )}
        </div>
    );
}

export default Profile;
```

Now Profile doesn't directly use:

```text
setUser()
```

Instead it uses:

```text
login()
logout()
```

---

# 11. Let's Follow the Login Flow

Initially:

```text
user = null
```

UI:

```text
Navbar
Not logged in

[ Login ]
```

User clicks:

```text
Login
```

This runs:

```text
login()
```

Inside `AuthContext`:

```text
login()
   ↓
setUser("William")
```

State becomes:

```text
user = "William"
```

React re-renders the components using that state.

Navbar becomes:

```text
Welcome, William
```

Profile becomes:

```text
Hello William

[ Logout ]
```

---

# 12. Logout Flow

User clicks:

```text
Logout
```

This runs:

```text
logout()
```

Inside AuthContext:

```text
logout()
   ↓
setUser(null)
```

State becomes:

```text
user = null
```

The UI goes back to:

```text
Not logged in

[ Login ]
```

---

# 13. The Complete Flow

### Login

```text
User clicks Login
       ↓
     login()
       ↓
setUser("William")
       ↓
 user changes
       ↓
 Context updates
       ↓
 components re-render
       ↓
 UI changes
```

### Logout

```text
User clicks Logout
       ↓
    logout()
       ↓
 setUser(null)
       ↓
 user changes
       ↓
 Context updates
       ↓
 components re-render
       ↓
 UI changes
```

---

# 14. Why Is This Better?

Previously:

```text
Profile
   ↓
setUser("William")
```

Now:

```text
Profile
   ↓
login()
   ↓
AuthContext
   ↓
setUser("William")
```

This gives us separation of responsibility.

### Profile

Handles the UI:

```text
Show Login button
Show Logout button
```

### AuthContext

Handles authentication state:

```text
user
login()
logout()
```

That's a much better architecture.

---

# 15. Full Code

### `AuthContext.tsx`

```tsx
import { createContext, useState } from "react";

const AuthContext = createContext(null);

function AuthProvider({ children }) {
    const [user, setUser] = useState(null);

    function login() {
        setUser("William");
    }

    function logout() {
        setUser(null);
    }

    return (
        <AuthContext.Provider
            value={{ user, login, logout }}
        >
            {children}
        </AuthContext.Provider>
    );
}

export { AuthContext, AuthProvider };
```

### `App.tsx`

```tsx
import { AuthProvider } from "./AuthContext";
import Navbar from "./Navbar";
import Profile from "./Profile";

function App() {
    return (
        <AuthProvider>
            <Navbar />
            <Profile />
        </AuthProvider>
    );
}

export default App;
```

### `Navbar.tsx`

```tsx
import { useContext } from "react";
import { AuthContext } from "./AuthContext";

function Navbar() {
    const { user } = useContext(AuthContext);

    return (
        <nav>
            {user ? `Welcome, ${user}` : "Not logged in"}
        </nav>
    );
}

export default Navbar;
```

### `Profile.tsx`

```tsx
import { useContext } from "react";
import { AuthContext } from "./AuthContext";

function Profile() {
    const { user, login, logout } = useContext(AuthContext);

    return (
        <div>
            {user ? (
                <div>
                    <h1>Hello {user}</h1>

                    <button onClick={logout}>
                        Logout
                    </button>
                </div>
            ) : (
                <button onClick={login}>
                    Login
                </button>
            )}
        </div>
    );
}

export default Profile;
```

---

# 16. The Architecture We Have Now

```text
                 AuthProvider
                      │
                AuthContext
                      │
             ┌────────┴────────┐
             ↓                 ↓
          Navbar            Profile
             │                 │
          user            user/login/logout
```

And inside the Provider:

```text
AuthProvider
     │
     ├── useState
     │      ↓
     │     user
     │
     ├── login()
     │      ↓
     │   setUser()
     │
     └── logout()
            ↓
         setUser()
```

This is the basic pattern you'll see frequently in React applications.

---

# Phase 7 Progress

```text
## 7. State Management

Context API

[x] Why state management?
[x] Prop drilling
[x] createContext()
[x] Provider
[x] useContext()
[x] Context + useState
[x] Auth Context
[x] Login / Logout
[ ] Build auth state app
```

**Next → Step 6: We'll turn this into the actual `Auth State App` from your roadmap, including protected UI and multiple components using the same authentication state.**


-------------------------------------------------------------------------------------------------------------------------------------------

# Step 6 — Build: Auth State App

Now we will **build the project required by your roadmap**:

> **Build: auth state app**

We already learned the individual pieces. Now we're going to put them together into one small React application.

The goal is to understand **how Context API is actually used in a project**, not just individual examples.

---

# 1. What Are We Building?

Our app will have:

```text
My App
│
├── Navbar
│
├── Home
│
└── Profile
```

Authentication state:

```text
Logged out
    ↓
[ Login ]
    ↓
Logged in
    ↓
Welcome, William
[ Logout ]
```

And the important part:

```text
AuthContext
     ↓
  user state
     ↓
 ┌───┴────┐
 ↓        ↓
Navbar  Profile
```

Both `Navbar` and `Profile` will use the **same authentication state**.

---

# 2. Create the Files

Your `src` folder:

```text
src
│
├── App.tsx
│
├── AuthContext.tsx
│
├── Navbar.tsx
│
├── Home.tsx
└── Profile.tsx
```

You don't need any additional folders yet.

---

# 3. `AuthContext.tsx`

This is the most important file.

```tsx
import { createContext, useState } from "react";

const AuthContext = createContext(null);

function AuthProvider({ children }) {
    const [user, setUser] = useState(null);

    function login() {
        setUser("William");
    }

    function logout() {
        setUser(null);
    }

    return (
        <AuthContext.Provider
            value={{
                user,
                login,
                logout
            }}
        >
            {children}
        </AuthContext.Provider>
    );
}

export { AuthContext, AuthProvider };
```

Let's understand the responsibility of this file:

```text
AuthContext.tsx
│
├── user
├── login()
└── logout()
```

It owns the authentication state.

---

# 4. `Navbar.tsx`

The Navbar only needs to know whether the user is logged in.

```tsx
import { useContext } from "react";
import { AuthContext } from "./AuthContext";

function Navbar() {
    const { user } = useContext(AuthContext);

    return (
        <nav>
            <h2>My App</h2>

            {user ? (
                <p>Welcome, {user}</p>
            ) : (
                <p>Guest</p>
            )}
        </nav>
    );
}

export default Navbar;
```

Notice something important.

Navbar does **not** have:

```text
user prop
```

We're getting the user directly from Context.

---

# 5. `Home.tsx`

```tsx
function Home() {
    return (
        <div>
            <h1>Home</h1>
            <p>Welcome to the application.</p>
        </div>
    );
}

export default Home;
```

Home doesn't need authentication state, so it doesn't use Context.

That's completely fine.

**Not every component needs Context.**

---

# 6. `Profile.tsx`

Now Profile will handle Login and Logout.

```tsx
import { useContext } from "react";
import { AuthContext } from "./AuthContext";

function Profile() {
    const { user, login, logout } = useContext(AuthContext);

    return (
        <div>
            <h1>Profile</h1>

            {user ? (
                <div>
                    <p>Hello, {user}</p>

                    <button onClick={logout}>
                        Logout
                    </button>
                </div>
            ) : (
                <div>
                    <p>You are not logged in.</p>

                    <button onClick={login}>
                        Login
                    </button>
                </div>
            )}
        </div>
    );
}

export default Profile;
```

Now Profile can:

```text
read user
   ↓
login()
   ↓
logout()
```

without receiving any props from `App`.

---

# 7. `App.tsx`

Now we connect everything.

```tsx
import { AuthProvider } from "./AuthContext";

import Navbar from "./Navbar";
import Home from "./Home";
import Profile from "./Profile";

function App() {
    return (
        <AuthProvider>

            <Navbar />

            <Home />

            <Profile />

        </AuthProvider>
    );
}

export default App;
```

The structure is:

```text
AuthProvider
│
├── Navbar
│
├── Home
│
└── Profile
```

---

# 8. What Happens When the App Starts?

Initially:

```tsx
const [user, setUser] = useState(null);
```

So:

```text
user = null
```

Navbar:

```text
My App
Guest
```

Profile:

```text
Profile

You are not logged in.

[ Login ]
```

---

# 9. Click Login

User clicks:

```text
[ Login ]
```

Profile calls:

```tsx
login()
```

The function exists inside `AuthContext`:

```tsx
function login() {
    setUser("William");
}
```

So:

```text
Login button
     ↓
login()
     ↓
setUser("William")
     ↓
user changes
```

---

# 10. Context Updates

The Provider was giving:

```tsx
value={{
    user,
    login,
    logout
}}
```

Before:

```text
user = null
```

After:

```text
user = "William"
```

So components using this Context receive the new value.

---

# 11. Navbar Changes Automatically

Before:

```text
My App
Guest
```

After:

```text
My App
Welcome, William
```

---

# 12. Profile Changes Automatically

Before:

```text
Profile

You are not logged in.

[ Login ]
```

After:

```text
Profile

Hello, William

[ Logout ]
```

We didn't manually tell Navbar to update.

We didn't pass a new prop.

The state changed, and React re-rendered the components that use that state.

---

# 13. Click Logout

Now the user clicks:

```text
[ Logout ]
```

Profile calls:

```tsx
logout()
```

AuthContext:

```tsx
function logout() {
    setUser(null);
}
```

So:

```text
Logout
 ↓
logout()
 ↓
setUser(null)
 ↓
user = null
 ↓
Context updates
 ↓
Navbar + Profile re-render
```

The UI goes back to:

```text
My App
Guest

Profile

You are not logged in.

[ Login ]
```

---

# 14. The Entire Data Flow

This is the most important thing to understand:

```text
                    AuthProvider
                         │
                         │
                      useState
                         │
                         ↓
                       user
                         │
             ┌───────────┴───────────┐
             ↓                       ↓
          Navbar                  Profile
             │                       │
          useContext()           useContext()
                                     │
                                  login()
                                     │
                                  logout()
```

When Login happens:

```text
Profile
  ↓
login()
  ↓
setUser("William")
  ↓
Context value changes
  ↓
Navbar + Profile update
```

---

# 15. Why Is This Better Than Props?

Without Context:

```text
App
 │
 │ user
 ↓
Navbar
```

and:

```text
App
 │
 │ user
 ↓
Profile
```

For a deeper tree:

```text
App
 ↓ user
Dashboard
 ↓ user
Layout
 ↓ user
Profile
```

With Context:

```text
       AuthProvider
            │
      AuthContext
       /         \
      ↓           ↓
  Navbar       Profile
```

No unnecessary prop passing.

---

# 16. One Important Real-World Point

Our login is obviously not a real login system.

We're simply doing:

```tsx
setUser("William");
```

A real application might do:

```text
Login form
    ↓
API request
    ↓
Backend verifies credentials
    ↓
User information returned
    ↓
AuthContext stores user
    ↓
Application knows user is logged in
```

We'll learn backend authentication much later in your roadmap.

For now, we're learning **state management architecture**.

---

# 17. Full Project Code

Your project is now:

```text
src
│
├── App.tsx
├── AuthContext.tsx
├── Navbar.tsx
├── Home.tsx
└── Profile.tsx
```

### `AuthContext.tsx`

```tsx
import { createContext, useState } from "react";

const AuthContext = createContext(null);

function AuthProvider({ children }) {
    const [user, setUser] = useState(null);

    function login() {
        setUser("William");
    }

    function logout() {
        setUser(null);
    }

    return (
        <AuthContext.Provider
            value={{
                user,
                login,
                logout
            }}
        >
            {children}
        </AuthContext.Provider>
    );
}

export { AuthContext, AuthProvider };
```

### `App.tsx`

```tsx
import { AuthProvider } from "./AuthContext";

import Navbar from "./Navbar";
import Home from "./Home";
import Profile from "./Profile";

function App() {
    return (
        <AuthProvider>
            <Navbar />
            <Home />
            <Profile />
        </AuthProvider>
    );
}

export default App;
```

### `Navbar.tsx`

```tsx
import { useContext } from "react";
import { AuthContext } from "./AuthContext";

function Navbar() {
    const { user } = useContext(AuthContext);

    return (
        <nav>
            <h2>My App</h2>

            {user ? (
                <p>Welcome, {user}</p>
            ) : (
                <p>Guest</p>
            )}
        </nav>
    );
}

export default Navbar;
```

### `Home.tsx`

```tsx
function Home() {
    return (
        <div>
            <h1>Home</h1>
            <p>Welcome to the application.</p>
        </div>
    );
}

export default Home;
```

### `Profile.tsx`

```tsx
import { useContext } from "react";
import { AuthContext } from "./AuthContext";

function Profile() {
    const { user, login, logout } = useContext(AuthContext);

    return (
        <div>
            <h1>Profile</h1>

            {user ? (
                <div>
                    <p>Hello, {user}</p>

                    <button onClick={logout}>
                        Logout
                    </button>
                </div>
            ) : (
                <div>
                    <p>You are not logged in.</p>

                    <button onClick={login}>
                        Login
                    </button>
                </div>
            )}
        </div>
    );
}

export default Profile;
```

---

# 18. Phase 7 Progress

We've now completed the **Context API portion** of the roadmap:

```text
## 7. State Management

### Context API

[x] Why state management?
[x] Prop drilling
[x] createContext()
[x] Provider
[x] useContext()
[x] Context + useState
[x] Auth Context
[x] Login / Logout
[x] Build: auth state app
```

The next item in your roadmap is:

```text
[ ] Redux Toolkit
```

But before jumping into Redux, we'll first understand **when Context API is enough and why Redux Toolkit exists**.

**Next → Step 7: Context API vs Redux Toolkit — why do we need Redux when we already have Context?**


-------------------------------------------------------------------------------------------------------------------------------------------


# Step 7 — Context API vs Redux Toolkit

Before we start Redux Toolkit, we need to understand **why Redux exists**.

You might be thinking:

> "We already have Context API. Why do we need Redux?"

That's exactly what we need to understand now.

---

# 1. What Problem Did Context Solve?

We started with **prop drilling**.

```text
App
 ↓ user
Dashboard
 ↓ user
Layout
 ↓ user
Profile
```

Context solved this:

```text
        AuthContext
       /     |      \
      ↓      ↓       ↓
   Navbar  Profile  Dashboard
```

So Context is very useful for **sharing data**.

---

# 2. Is Context Bad?

No.

Context is a React feature and is perfectly good for many situations.

For example:

```text
Theme
 ↓
light / dark
```

or:

```text
Authentication
 ↓
user
```

or:

```text
Language
 ↓
English / Tamil
```

These are common Context use cases.

---

# 3. Then Why Redux?

Imagine a much larger application.

For example, an e-commerce application:

```text
                    App
                     │
       ┌─────────────┼─────────────┐
       ↓             ↓             ↓
    Navbar        Products       Profile
       │             │
       ↓             ↓
     Cart         Product
       │
       ↓
   Checkout
```

Now we might have many different pieces of global state:

```text
user
cart
products
orders
notifications
filters
UI state
permissions
```

And these states may interact with each other.

Managing all of this with many Contexts can become harder.

---

# 4. Multiple Contexts

You might end up with:

```text
AuthContext
CartContext
ProductContext
NotificationContext
ThemeContext
```

And your application could look like:

```text
AuthProvider
   ↓
CartProvider
   ↓
ProductProvider
   ↓
NotificationProvider
   ↓
ThemeProvider
   ↓
App
```

This can become difficult to organize.

---

# 5. Redux Gives Us a Central Store

Redux uses a central **store**.

Think:

```text
                 Redux Store
                     │
       ┌─────────────┼─────────────┐
       ↓             ↓             ↓
      auth          cart        products
       │             │             │
       ↓             ↓             ↓
     user          items        product list
```

Instead of having many separate Contexts, Redux provides a centralized state-management system.

---

# 6. Context vs Redux

Think of them like this:

### Context

Main purpose:

```text
Share data
```

### Redux

Main purpose:

```text
Manage complex application state
```

This is a simplified mental model, but it's a useful one when you're learning.

---

# 7. Context Can Also Manage State

Remember our example:

```text
AuthProvider
     ↓
useState
     ↓
user
```

Context can absolutely manage changing state.

So:

```text
Context
+
useState
```

is enough for many applications.

You don't automatically need Redux.

---

# 8. When Context Is Enough

For example:

```text
Small application
```

with:

```text
user
theme
language
```

Context may be perfectly sufficient.

Example:

```text
AuthContext
ThemeContext
```

That's not a problem.

---

# 9. When Redux Becomes Useful

Imagine:

```text
Large application
```

with:

```text
User
Cart
Products
Orders
Notifications
Filters
Search
UI state
Permissions
```

and many components are reading and updating these values.

A dedicated state-management library can make the state flow more structured and predictable.

That's where Redux Toolkit becomes useful.

---

# 10. The Most Important Redux Idea

Redux has a central store.

The basic flow is:

```text
Component
   ↓
dispatch()
   ↓
Action
   ↓
Reducer
   ↓
Store changes
   ↓
Component receives new state
```

Remember this flow.

We'll use it constantly.

---

# 11. Example

Suppose our cart is:

```text
cart = []
```

User clicks:

```text
Add to Cart
```

The component dispatches an action:

```text
dispatch(addToCart(product))
```

Then:

```text
Component
   ↓
dispatch
   ↓
addToCart
   ↓
Reducer
   ↓
cart updated
```

The store might become:

```text
cart = [
    Laptop
]
```

Any component that needs the cart can read it.

---

# 12. Redux Is Not Another React Hook

This is important because you've been learning React Hooks.

Redux Toolkit is **not a React Hook**.

It is a **state-management library**.

However, when using Redux with React, you'll commonly use hooks such as:

```text
useSelector()
useDispatch()
```

These are React-Redux hooks used to interact with the Redux store.

So don't confuse:

```text
useState
useEffect
useRef
```

with:

```text
Redux Toolkit
```

They are different concepts.

---

# 13. Context vs Redux — Simple Comparison

| Context API                  | Redux Toolkit                   |
| ---------------------------- | ------------------------------- |
| Built into React             | External library                |
| Good for shared values       | Good for complex global state   |
| Simple to set up             | More structured                 |
| Often uses `useState`        | Uses store/reducers/actions     |
| Good for small/medium needs  | Useful for larger state systems |
| No extra package for Context | Requires Redux packages         |

---

# 14. Don't Think Redux Replaces `useState`

Redux does not mean:

> "Never use `useState` again."

You can still use local state.

For example:

```text
Search input
   ↓
useState
```

while:

```text
Cart
   ↓
Redux
```

So an application can use both:

```text
Local UI state
    ↓
useState

Global application state
    ↓
Redux
```

---

# 15. A Useful Rule

Think about state in two categories.

### Local state

Only one component or a small component area needs it.

Example:

```text
Is dropdown open?
Is modal open?
Input value?
```

Use:

```text
useState
```

### Shared/global state

Many unrelated parts of the application need it.

Example:

```text
Logged-in user
Shopping cart
Global notifications
```

Use something like:

```text
Context
or
Redux Toolkit
```

depending on the complexity.

---

# 16. Our Learning Path

We're now ready to actually learn Redux Toolkit.

We'll do it gradually:

```text
Step 7
Context vs Redux
        ↓
Step 8
Install Redux Toolkit
        ↓
Step 9
Create Redux Store
        ↓
Step 10
Create Slice
        ↓
Step 11
Actions + Reducers
        ↓
Step 12
useSelector()
        ↓
Step 13
useDispatch()
        ↓
Step 14
Build Counter with Redux
        ↓
Step 15
Build Cart System
```

The **Cart System** is the project required by your roadmap.

---

# Phase 7 Progress

```text
## 7. State Management

### Redux Toolkit

[x] Why Redux?
[x] Context vs Redux
[ ] Redux Store
[ ] Slice
[ ] Actions
[ ] Reducers
[ ] useSelector()
[ ] useDispatch()
[ ] Build: Cart System
```

**Next → Step 8: Set up Redux Toolkit and create our first Redux Store.**


-------------------------------------------------------------------------------------------------------------------------------------------

# Step 8 — Redux Toolkit: Install and Create the Store

Now we start **Redux Toolkit**.

Our goal in this step is very small:

> Understand what the **Redux Store** is and create one.

We are **not** creating a slice or actions yet. Those come next.

---

# 1. What Are We Building?

Eventually, we'll build:

```text
React App
   ↓
Redux Store
   ↓
Cart
   ↓
Products
```

For now, we'll only create:

```text
Redux Store
```

Think of the store as the **central place where Redux keeps application state**.

---

# 2. Install Redux Toolkit

Since you're using Vite + React + TypeScript, install:

```text
@reduxjs/toolkit
react-redux
```

You need both:

```text
@reduxjs/toolkit
    ↓
Redux tools

react-redux
    ↓
Connect Redux with React
```

After installation, your project will have these packages available.

---

# 3. Create the Redux Folder

Now create:

```text
src
│
├── App.tsx
│
└── store
    └── store.ts
```

So:

```text
store
  ↓
contains Redux configuration
```

---

# 4. Create `store.ts`

Inside:

```text
src/store/store.ts
```

write:

```tsx
import { configureStore } from "@reduxjs/toolkit";

export const store = configureStore({
    reducer: {}
});
```

That's our first Redux Store.

---

# 5. What Is `configureStore()`?

This:

```text
configureStore()
```

creates the Redux store.

Think:

```text
configureStore()
       ↓
   Redux Store
```

The store will eventually contain things like:

```text
Store
│
├── auth
├── cart
├── products
└── notifications
```

But currently we don't have any of those.

---

# 6. What Is `reducer`?

We currently have:

```tsx
reducer: {}
```

Don't worry about this yet.

A reducer is responsible for describing **how a particular piece of Redux state changes**.

Later we'll have:

```text
Store
│
├── auth → authReducer
├── cart → cartReducer
└── products → productReducer
```

For now:

```text
reducer: {}
```

means:

> We haven't added any state slices yet.

We'll add our first slice in the next step.

---

# 7. Redux Store vs `useState`

You already know:

```tsx
const [count, setCount] = useState(0);
```

This state belongs to a component.

```text
Component
   ↓
useState
   ↓
count
```

Redux is different:

```text
Redux Store
   ↓
global application state
```

For example:

```text
Redux Store
│
├── cart
├── user
└── products
```

Many components can access this state.

---

# 8. We Need to Connect Redux to React

Creating the store isn't enough.

React needs to know:

> "This is the Redux store my application should use."

For that, React Redux gives us:

```text
Provider
```

This is similar to what you learned with Context.

Remember Context:

```text
AuthProvider
    ↓
components
```

Redux:

```text
Provider
    ↓
Redux Store
    ↓
components
```

---

# 9. Update `main.tsx`

Your Vite project already has:

```text
src/main.tsx
```

You'll modify it.

```tsx
import React from "react";
import ReactDOM from "react-dom/client";

import App from "./App";
import { Provider } from "react-redux";
import { store } from "./store/store";

ReactDOM.createRoot(document.getElementById("root")!).render(
    <React.StrictMode>
        <Provider store={store}>
            <App />
        </Provider>
    </React.StrictMode>
);
```

The important part is:

```text
<Provider store={store}>
    <App />
</Provider>
```

---

# 10. What Is Happening Here?

We created:

```text
store.ts
    ↓
Redux Store
```

Then:

```text
main.tsx
    ↓
Provider
    ↓
store
```

Then:

```text
Provider
    ↓
App
```

So the structure is:

```text
Provider
   │
   │ Redux Store
   ↓
  App
   │
   ├── Navbar
   ├── Home
   └── Products
```

Every component inside the Provider can interact with Redux.

---

# 11. Context Provider vs Redux Provider

This might look familiar.

Context:

```text
<AuthProvider>
    <App />
</AuthProvider>
```

Redux:

```text
<Provider store={store}>
    <App />
</Provider>
```

The idea is similar:

```text
Provider
   ↓
makes something available
   ↓
child components
```

But they are different systems.

### Context

```text
React Context API
```

### Redux

```text
Redux Store
+
React Redux Provider
```

---

# 12. Our Current Project

Your project can now look like:

```text
src
│
├── App.tsx
│
├── main.tsx
│
└── store
    └── store.ts
```

### `store.ts`

```tsx
import { configureStore } from "@reduxjs/toolkit";

export const store = configureStore({
    reducer: {}
});
```

### `main.tsx`

```tsx
import React from "react";
import ReactDOM from "react-dom/client";

import App from "./App";
import { Provider } from "react-redux";
import { store } from "./store/store";

ReactDOM.createRoot(document.getElementById("root")!).render(
    <React.StrictMode>
        <Provider store={store}>
            <App />
        </Provider>
    </React.StrictMode>
);
```

---

# 13. What We Have NOT Learned Yet

Don't worry about these yet:

```text
❌ Slice
❌ createSlice()
❌ Action
❌ Reducer logic
❌ useSelector()
❌ useDispatch()
```

We'll learn each separately.

For now:

```text
configureStore()
       ↓
creates Store

Provider
       ↓
makes Store available to React
```

That's all you need from this step.

---

# 14. The Big Picture

Eventually, our Redux application will work like this:

```text
                 Redux Store
                     │
          ┌──────────┼──────────┐
          ↓          ↓          ↓
        auth        cart      products
          │          │          │
          ↓          ↓          ↓
       Navbar     CartPage   Products
```

And when something changes:

```text
Component
    ↓
dispatch()
    ↓
Action
    ↓
Reducer
    ↓
Redux Store
    ↓
UI updates
```

We'll learn this flow one piece at a time.

---

# Phase 7 Progress

```text
## 7. State Management

### Redux Toolkit

[x] Why Redux?
[x] Context vs Redux
[x] Install Redux Toolkit
[x] Create Redux Store
[x] Provider
[ ] Slice
[ ] Actions
[ ] Reducers
[ ] useSelector()
[ ] useDispatch()
[ ] Build: Counter with Redux
[ ] Build: Cart System
```

**Next → Step 9: `createSlice()` — we'll create our first Redux state, a simple counter, and understand what a slice actually contains.**

-------------------------------------------------------------------------------------------------------------------------------------------

# Step 9 — Redux Toolkit: `createSlice()`

Now we create our **first actual Redux state**.

Until now, we only created the store:

```text
configureStore()
      ↓
Redux Store
```

But our store currently has no useful state:

```text
Store
└── nothing yet
```

Now we'll add a **counter slice**.

---

# 1. What Is a Slice?

A **slice** is one piece of your Redux state along with the logic for changing that state.

For example, a large application might have:

```text
Redux Store
│
├── auth
├── cart
├── products
└── notifications
```

Each of these can be a slice.

For our example:

```text
Redux Store
│
└── counter
```

So:

```text
counter slice
    ↓
counter state
    ↓
logic for changing counter
```

---

# 2. Create the Counter Slice

Our folder becomes:

```text
src
│
├── App.tsx
├── main.tsx
│
└── store
    ├── store.ts
    └── counterSlice.ts
```

Create:

```text
src/store/counterSlice.ts
```

---

# 3. Write the Slice

```tsx id="kq2v9w"
import { createSlice } from "@reduxjs/toolkit";

const counterSlice = createSlice({
    name: "counter",

    initialState: {
        value: 0
    },

    reducers: {
        increment(state) {
            state.value += 1;
        },

        decrement(state) {
            state.value -= 1;
        }
    }
});

export const { increment, decrement } = counterSlice.actions;

export default counterSlice.reducer;
```

There are several new things here.

Let's take them one by one.

---

# 4. `createSlice()`

This:

```tsx id="8qv6hm"
createSlice()
```

creates a Redux slice.

Think:

```text
createSlice()
      ↓
counter slice
```

---

# 5. `name`

We have:

```tsx id="at6a7u"
name: "counter"
```

This gives our slice a name.

So:

```text
counterSlice
     ↓
name = counter
```

---

# 6. `initialState`

We have:

```tsx id="w6r6i1"
initialState: {
    value: 0
}
```

This is the initial Redux state.

So when the application starts:

```text
counter
   ↓
value = 0
```

Similar to:

```tsx id="8z4fpp"
const [count, setCount] = useState(0);
```

But now the state belongs to Redux.

---

# 7. `reducers`

This is one of the most important parts.

We have:

```tsx id="5i1g2x"
reducers: {
    increment(state) {
        state.value += 1;
    },

    decrement(state) {
        state.value -= 1;
    }
}
```

These functions describe **how our state can change**.

We have two:

```text
increment
decrement
```

So:

```text
increment()
   ↓
value + 1
```

and:

```text
decrement()
   ↓
value - 1
```

---

# 8. Think of Reducers as State-Changing Rules

Our state:

```text
value = 0
```

Reducer:

```text
increment
```

Rule:

```text
value + 1
```

Another reducer:

```text
decrement
```

Rule:

```text
value - 1
```

So:

```text
State
 ↓
Reducer
 ↓
New State
```

---

# 9. Export the Actions

We have:

```tsx id="u4q9jo"
export const { increment, decrement } = counterSlice.actions;
```

This gives us functions/actions we can dispatch from components.

So:

```text
increment
decrement
```

become available outside this file.

Later we'll do:

```text
dispatch(increment())
```

and:

```text
dispatch(decrement())
```

---

# 10. Export the Reducer

At the bottom:

```tsx id="g7j7br"
export default counterSlice.reducer;
```

This exports the reducer for the **Redux Store**.

Remember:

```text
Slice
│
├── state
├── reducers
│
└── reducer
       ↓
     Store
```

---

# 11. Connect the Slice to the Store

Now open:

```text
src/store/store.ts
```

Previously we had:

```tsx id="v9q5hi"
import { configureStore } from "@reduxjs/toolkit";

export const store = configureStore({
    reducer: {}
});
```

Change it to:

```tsx id="5d5f4v"
import { configureStore } from "@reduxjs/toolkit";

import counterReducer from "./counterSlice";

export const store = configureStore({
    reducer: {
        counter: counterReducer
    }
});
```

Now our Redux Store has a `counter` section.

---

# 12. Visualize the Store

Before:

```text
Redux Store
│
└── nothing
```

After:

```text
Redux Store
│
└── counter
     │
     └── value: 0
```

So when we write:

```tsx id="amw93c"
counter: counterReducer
```

we're telling Redux:

> "The `counter` part of the store is managed by `counterReducer`."

---

# 13. Our Redux Architecture

We now have:

```text
counterSlice.ts
│
├── initialState
│      ↓
│   value: 0
│
├── increment()
│
└── decrement()
       ↓
    reducer
       ↓
Redux Store
```

---

# 14. Compare With `useState`

You already know:

```tsx id="u5yqqh"
const [count, setCount] = useState(0);
```

With Redux:

```text
initialState
   ↓
value = 0
```

Then:

```text
setCount(count + 1)
```

roughly corresponds conceptually to:

```text
dispatch(increment())
```

But the Redux flow is more structured:

```text
Component
   ↓
dispatch(increment())
   ↓
increment action
   ↓
reducer
   ↓
state changes
```

---

# 15. One Strange-Looking Thing

You might notice:

```tsx id="l3p9jk"
increment(state) {
    state.value += 1;
}
```

It looks like we're directly modifying state.

Normally in Redux, you should **not mutate state directly**.

But Redux Toolkit uses **Immer** internally, which allows this simpler syntax while producing the correct immutable state update behind the scenes.

So for now, remember:

```tsx id="g6g8w4"
state.value += 1;
```

is valid inside a Redux Toolkit reducer.

---

# 16. But We Still Can't See the Counter

At this point:

```text
Redux Store
    ↓
counter
    ↓
value = 0
```

exists.

But our React component isn't reading it yet.

That's the next step.

We need:

```text
useSelector()
```

to **read Redux state**.

And:

```text
useDispatch()
```

to **send actions**.

---

# 17. Current Project

```text
src
│
├── App.tsx
│
├── main.tsx
│
└── store
    ├── store.ts
    └── counterSlice.ts
```

### `counterSlice.ts`

```tsx id="6l9r3x"
import { createSlice } from "@reduxjs/toolkit";

const counterSlice = createSlice({
    name: "counter",

    initialState: {
        value: 0
    },

    reducers: {
        increment(state) {
            state.value += 1;
        },

        decrement(state) {
            state.value -= 1;
        }
    }
});

export const { increment, decrement } = counterSlice.actions;

export default counterSlice.reducer;
```

### `store.ts`

```tsx id="n3y0kl"
import { configureStore } from "@reduxjs/toolkit";

import counterReducer from "./counterSlice";

export const store = configureStore({
    reducer: {
        counter: counterReducer
    }
});
```

### `main.tsx`

We already connected the store:

```tsx id="m55gak"
<Provider store={store}>
    <App />
</Provider>
```

---

# 18. The Redux Flow So Far

```text
                Redux Store
                     │
                     ↓
                  counter
                     │
                     ↓
                  value: 0

counterSlice
│
├── initialState
│
├── increment
│
└── decrement
```

We're almost ready to interact with it from React.

---

```text
- Slice = Defines the state and how it changes

- Store = Holds all Redux state

- Provider = Gives the Store to React components

--- 

counterSlice.ts
        ↓
defines the counter

store.ts
        ↓
puts counter into the global Redux store

main.tsx
        ↓
makes that store available to React

```


# Phase 7 Progress

```text
### Redux Toolkit

[x] Why Redux?
[x] Context vs Redux
[x] Install Redux Toolkit
[x] Create Redux Store
[x] Provider
[x] createSlice()
[x] initialState
[x] reducers
[x] actions
[ ] useSelector()
[ ] useDispatch()
[ ] Build: Counter with Redux
[ ] Build: Cart System
```

**Next → Step 10: `useSelector()` — we'll read `counter.value` from the Redux Store and display it on the screen.**


-------------------------------------------------------------------------------------------------------------------------------------------

# Step 10 — `useSelector()`

Now we'll connect our React component to the Redux Store and **read the counter value**.

We currently have:

```text
Redux Store
    ↓
counter
    ↓
value: 0
```

But our React component doesn't know about this value yet.

That's what `useSelector()` is for.

---

# 1. What Does `useSelector()` Do?

`useSelector()` is used to **read data from the Redux Store**.

Think:

```text
Redux Store
    ↓
useSelector()
    ↓
React Component
```

For our counter:

```text
Redux Store
    ↓
counter.value
    ↓
useSelector()
    ↓
Counter component
    ↓
Display 0
```

---

# 2. Create `Counter.tsx`

Our structure becomes:

```text
src
│
├── App.tsx
├── main.tsx
│
├── Counter.tsx
│
└── store
    ├── store.ts
    └── counterSlice.ts
```

Create:

```text
src/Counter.tsx
```

---

# 3. Import `useSelector`

Inside `Counter.tsx`:

```tsx
import { useSelector } from "react-redux";
```

Then:

```tsx
function Counter() {
    const count = useSelector((state) => state.counter.value);

    return (
        <div>
            <h1>{count}</h1>
        </div>
    );
}

export default Counter;
```

Let's understand the important line:

```tsx
const count = useSelector((state) => state.counter.value);
```

---

# 4. What Is `state`?

Here:

```tsx
(state) => state.counter.value
```

`state` represents the **entire Redux Store state**.

Our store currently looks like:

```text
state
│
└── counter
     │
     └── value: 0
```

So:

```text
state.counter
```

gives:

```text
{
    value: 0
}
```

And:

```text
state.counter.value
```

gives:

```text
0
```

Therefore:

```tsx
const count = useSelector(
    (state) => state.counter.value
);
```

means:

> "Go to the Redux Store and give me the counter's value."

---

# 5. Why `counter`?

Look at our `store.ts`:

```tsx
reducer: {
    counter: counterReducer
}
```

We named this part:

```text
counter
```

Therefore:

```text
state.counter
```

exists.

And our slice's state is:

```text
value
```

Therefore:

```text
state.counter.value
```

---

# 6. Visualize It

Our Redux Store:

```text
┌─────────────────────┐
│     Redux Store     │
│                     │
│  counter            │
│    └── value: 0     │
│                     │
└─────────────────────┘
```

`useSelector()`:

```text
useSelector()
      ↓
state.counter.value
      ↓
      0
```

Then:

```tsx
<h1>{count}</h1>
```

displays:

```text
0
```

---

# 7. Add Counter to `App.tsx`

Now update `App.tsx`:

```tsx
import Counter from "./Counter";

function App() {
    return (
        <div>
            <Counter />
        </div>
    );
}

export default App;
```

Remember, `Provider` is already in `main.tsx`:

```text
Provider
   ↓
 App
   ↓
Counter
```

So Counter has access to Redux.

---

# 8. Full Flow

When the application starts:

```text
Redux Store
     ↓
counter
     ↓
value = 0
```

Counter:

```text
useSelector()
     ↓
state.counter.value
     ↓
0
```

Then:

```text
<h1>0</h1>
```

appears on the screen.

---

# 9. Why Does `useSelector()` Matter?

Imagine the Redux Store has:

```text
user
cart
products
notifications
```

A component doesn't need to receive everything.

It can select only what it needs.

For example:

```text
Navbar
   ↓
user
```

```text
Cart
   ↓
cart
```

```text
Products
   ↓
products
```

This is why it's called:

**`useSelector()`**

You're selecting a specific piece of Redux state.

---

# 10. `useSelector()` Does Not Change State

This is important.

`useSelector()` is for:

```text
READ
```

Not:

```text
CHANGE
```

So:

```text
useSelector()
    ↓
read state
```

To change state, we'll use:

```text
useDispatch()
```

That's our next step.

---

# 11. Current Project

```text
src
│
├── App.tsx
├── main.tsx
├── Counter.tsx
│
└── store
    ├── store.ts
    └── counterSlice.ts
```

### `Counter.tsx`

```tsx
import { useSelector } from "react-redux";

function Counter() {
    const count = useSelector((state) => state.counter.value);

    return (
        <div>
            <h1>{count}</h1>
        </div>
    );
}

export default Counter;
```

### `App.tsx`

```tsx
import Counter from "./Counter";

function App() {
    return (
        <div>
            <Counter />
        </div>
    );
}

export default App;
```

---

# 12. One TypeScript Issue

Because our project is TypeScript, your editor may complain about:

```tsx
state
```

because TypeScript doesn't know the type of our Redux Store yet.

That's normal.

We'll properly type the Redux Store shortly.

For learning the Redux concept, focus on this:

```text
state.counter.value
```

---

# 13. Redux Flow So Far

We now understand:

```text
                    Redux Store
                        │
                        ↓
                     counter
                        │
                        ↓
                      value
                        │
                        ↓
                  useSelector()
                        │
                        ↓
                    Counter
                        │
                        ↓
                     <h1>
```

But currently the number can't change from the UI.

Why?

Because we haven't connected our actions to the component yet.

We already created:

```text
increment()
decrement()
```

Now we need a way to **dispatch** those actions.

---

# Phase 7 Progress

```text
## 7. State Management

### Redux Toolkit

[x] Why Redux?
[x] Context vs Redux
[x] Install Redux Toolkit
[x] Create Redux Store
[x] Provider
[x] createSlice()
[x] initialState
[x] reducers
[x] actions
[x] useSelector()
[ ] useDispatch()
[ ] Build: Counter with Redux
[ ] Build: Cart System
```

**Next → Step 11: `useDispatch()` — we'll connect the Increment and Decrement buttons to our Redux actions and make the counter actually work.**


-------------------------------------------------------------------------------------------------------------------------------------------

# Step 11 — `useDispatch()`

Now we have learned how to **read** Redux state with `useSelector()`.

Next, we need to learn how to **change** Redux state.

That's what `useDispatch()` is for.

---

# 1. `useSelector()` vs `useDispatch()`

Remember:

```text
useSelector()
     ↓
READ Redux state
```

And:

```text
useDispatch()
     ↓
SEND an action
```

So:

```text
READ  → useSelector()
CHANGE → useDispatch()
```

This is one of the most important things to remember.

---

# 2. Our Current Redux State

We have:

```text
Redux Store
│
└── counter
     │
     └── value: 0
```

And our slice already has:

```text
increment
decrement
```

So we have the ability to change the state.

We just need to trigger those actions from React.

---

# 3. Update `Counter.tsx`

Currently:

```tsx
import { useSelector } from "react-redux";

function Counter() {
    const count = useSelector((state) => state.counter.value);

    return (
        <div>
            <h1>{count}</h1>
        </div>
    );
}

export default Counter;
```

Now we'll add `useDispatch()`.

```tsx
import { useDispatch, useSelector } from "react-redux";
import { increment, decrement } from "./store/counterSlice";

function Counter() {
    const count = useSelector((state) => state.counter.value);

    const dispatch = useDispatch();

    return (
        <div>
            <h1>{count}</h1>

            <button onClick={() => dispatch(increment())}>
                Increment
            </button>

            <button onClick={() => dispatch(decrement())}>
                Decrement
            </button>
        </div>
    );
}

export default Counter;
```

Now our counter is connected.

---

# 4. Understand `useDispatch()`

This:

```tsx
const dispatch = useDispatch();
```

gives us a function called:

```text
dispatch
```

We use it to send Redux actions.

For example:

```tsx
dispatch(increment())
```

means:

> Send the `increment` action to Redux.

And:

```tsx
dispatch(decrement())
```

means:

> Send the `decrement` action to Redux.

---

# 5. What Happens When We Click Increment?

User clicks:

```text
[ Increment ]
```

React executes:

```tsx
dispatch(increment())
```

Then:

```text
Component
    ↓
dispatch()
    ↓
increment action
    ↓
counter reducer
    ↓
state.value += 1
    ↓
Redux Store updates
```

Initially:

```text
value = 0
```

After clicking:

```text
value = 1
```

---

# 6. Why Does the Screen Update?

Remember this line:

```tsx
const count = useSelector(
    (state) => state.counter.value
);
```

The component is watching:

```text
state.counter.value
```

When Redux changes:

```text
0 → 1
```

the component gets the new value.

So:

```text
Redux state changes
       ↓
useSelector gets new value
       ↓
Component re-renders
       ↓
UI shows 1
```

---

# 7. Click Again

Current:

```text
value = 1
```

Click:

```text
[ Increment ]
```

Flow:

```text
dispatch(increment())
        ↓
reducer
        ↓
value += 1
        ↓
value = 2
```

Screen:

```text
2
```

Again:

```text
3
```

And so on.

---

# 8. What About Decrement?

Click:

```text
[ Decrement ]
```

We dispatch:

```tsx
dispatch(decrement())
```

Reducer:

```tsx
decrement(state) {
    state.value -= 1;
}
```

So:

```text
3
 ↓
2
```

---

# 9. The Complete Redux Flow

This is the flow you should memorize:

```text
             User clicks button
                     ↓
               dispatch()
                     ↓
                  Action
                     ↓
                 Reducer
                     ↓
               State changes
                     ↓
                Redux Store
                     ↓
               useSelector()
                     ↓
               Component
                     ↓
                UI updates
```

For our counter:

```text
Click Increment
      ↓
dispatch(increment())
      ↓
increment action
      ↓
increment reducer
      ↓
value + 1
      ↓
Redux Store
      ↓
useSelector()
      ↓
Counter displays new value
```

---

# 10. Why Do We Need the Action?

You might wonder why we can't simply do:

```text
setCount(count + 1)
```

like `useState`.

Redux intentionally separates the responsibilities.

```text
Component
   ↓
"Something happened"
   ↓
Action
   ↓
Reducer decides how state changes
```

The component doesn't directly manage the Redux state.

---

# 11. What Does `increment()` Actually Represent?

From our slice:

```tsx
reducers: {
    increment(state) {
        state.value += 1;
    }
}
```

Redux Toolkit creates an action for us.

So:

```tsx
increment()
```

represents:

```text
"Increment the counter."
```

Then:

```tsx
dispatch(increment())
```

sends that instruction to Redux.

---

# 12. Don't Confuse These Three Things

This is important.

### `increment`

The action creator:

```text
increment
```

### `increment()`

Creates an action:

```text
increment()
```

### `dispatch(increment())`

Sends that action to Redux:

```text
dispatch
   ↓
increment()
```

So the normal pattern is:

```tsx
dispatch(increment());
```

---

# 13. Our Files Now

```text
src
│
├── App.tsx
├── main.tsx
├── Counter.tsx
│
└── store
    ├── store.ts
    └── counterSlice.ts
```

The important relationship:

```text
counterSlice.ts
       │
       ├── increment
       ├── decrement
       └── reducer
                ↓
             store.ts
                ↓
              Store
                ↓
           Provider
                ↓
           Counter.tsx
                │
          ┌─────┴─────┐
          ↓           ↓
    useSelector   useDispatch
       READ          CHANGE
```

---

# 14. Full `Counter.tsx`

```tsx
import { useDispatch, useSelector } from "react-redux";
import { increment, decrement } from "./store/counterSlice";

function Counter() {
    const count = useSelector((state) => state.counter.value);

    const dispatch = useDispatch();

    return (
        <div>
            <h1>{count}</h1>

            <button onClick={() => dispatch(increment())}>
                Increment
            </button>

            <button onClick={() => dispatch(decrement())}>
                Decrement
            </button>
        </div>
    );
}

export default Counter;
```

---

# 15. We Have Now Completed the Basic Redux Cycle

You now know:

```text
[x] Store
[x] Slice
[x] Initial state
[x] Reducer
[x] Action
[x] useSelector()
[x] useDispatch()
```

And the entire cycle:

```text
UI
 ↓
dispatch(action)
 ↓
reducer
 ↓
Redux state
 ↓
useSelector
 ↓
UI
```

This is the foundation of Redux Toolkit.

---

# Phase 7 Progress

```text
## 7. State Management

### Context API

[x] Context API
[x] Auth state app

### Redux Toolkit

[x] Why Redux?
[x] Context vs Redux
[x] Install Redux Toolkit
[x] Redux Store
[x] Provider
[x] createSlice()
[x] initialState
[x] reducers
[x] actions
[x] useSelector()
[x] useDispatch()
[x] Counter with Redux

[ ] Type the Redux Store properly with TypeScript
[ ] Build Cart System
```

**Next → Step 12: TypeScript + Redux Toolkit — we'll fix the TypeScript types properly (`RootState`, `AppDispatch`) before moving to the Cart System.**

-------------------------------------------------------------------------------------------------------------------------------------------

# Step 12 — TypeScript + Redux Toolkit

Now we'll make our Redux code **properly typed with TypeScript**.

So far, we used:

```tsx
const count = useSelector((state) => state.counter.value);
```

But TypeScript doesn't know what `state` is.

We'll fix that now.

---

# 1. Why Do We Need Types?

Our Redux Store looks like:

```text
Redux Store
│
└── counter
     │
     └── value: number
```

We want TypeScript to understand this automatically.

Then if we write:

```tsx
state.counter.value
```

TypeScript knows:

```text
state       → Redux Store state
counter     → counter state
value       → number
```

---

# 2. Update `store.ts`

Open:

```text
src/store/store.ts
```

Currently:

```tsx
import { configureStore } from "@reduxjs/toolkit";

import counterReducer from "./counterSlice";

export const store = configureStore({
    reducer: {
        counter: counterReducer
    }
});
```

Add these types:

```tsx
import { configureStore } from "@reduxjs/toolkit";

import counterReducer from "./counterSlice";

export const store = configureStore({
    reducer: {
        counter: counterReducer
    }
});

export type RootState = ReturnType<typeof store.getState>;

export type AppDispatch = typeof store.dispatch;
```

---

# 3. What Is `RootState`?

This:

```tsx
export type RootState = ReturnType<typeof store.getState>;
```

creates a TypeScript type representing the **entire Redux state**.

Our store is:

```text
Store
│
└── counter
     └── value
```

Therefore `RootState` understands:

```text
RootState
   ↓
counter
   ↓
value
```

So:

```tsx
state: RootState
```

means:

> `state` represents the complete Redux state.

---

# 4. What Is `AppDispatch`?

This:

```tsx
export type AppDispatch = typeof store.dispatch;
```

gets the type of Redux's `dispatch` function.

So TypeScript understands:

```text
dispatch
   ↓
Redux actions
```

This becomes useful when our application gets more complex.

---

# 5. Type `useSelector()`

Now update `Counter.tsx`.

```tsx
import { useSelector } from "react-redux";

import type { RootState } from "./store/store";

function Counter() {
    const count = useSelector(
        (state: RootState) => state.counter.value
    );

    return (
        <div>
            <h1>{count}</h1>
        </div>
    );
}

export default Counter;
```

Now TypeScript knows exactly what `state` is.

---

# 6. Type `useDispatch()`

We also want a typed dispatch.

Update the imports:

```tsx
import { useDispatch, useSelector } from "react-redux";

import type { RootState, AppDispatch } from "./store/store";

import {
    increment,
    decrement
} from "./store/counterSlice";
```

Then:

```tsx
const dispatch = useDispatch<AppDispatch>();
```

So the complete component becomes:

```tsx
import { useDispatch, useSelector } from "react-redux";

import type { RootState, AppDispatch } from "./store/store";

import {
    increment,
    decrement
} from "./store/counterSlice";

function Counter() {
    const count = useSelector(
        (state: RootState) => state.counter.value
    );

    const dispatch = useDispatch<AppDispatch>();

    return (
        <div>
            <h1>{count}</h1>

            <button onClick={() => dispatch(increment())}>
                Increment
            </button>

            <button onClick={() => dispatch(decrement())}>
                Decrement
            </button>
        </div>
    );
}

export default Counter;
```

---

# 7. But There Is a Better Approach

Typing this everywhere:

```tsx
useSelector((state: RootState) => ...)
```

and:

```tsx
useDispatch<AppDispatch>()
```

can become repetitive.

So Redux Toolkit applications commonly create **typed hooks**.

We'll create:

```text
src/store/hooks.ts
```

---

# 8. Create `hooks.ts`

```tsx
import { useDispatch, useSelector } from "react-redux";

import type { RootState, AppDispatch } from "./store";

export const useAppSelector = useSelector.withTypes<RootState>();

export const useAppDispatch = useDispatch.withTypes<AppDispatch>();
```

Now we have:

```text
useAppSelector
useAppDispatch
```

These are our application's typed Redux hooks.

---

# 9. Why Create These?

Instead of:

```tsx
useSelector(
    (state: RootState) => state.counter.value
);
```

we can write:

```tsx
useAppSelector(
    (state) => state.counter.value
);
```

TypeScript already knows `state`.

And instead of:

```tsx
useDispatch<AppDispatch>();
```

we can write:

```tsx
useAppDispatch();
```

Much cleaner.

---

# 10. Update `Counter.tsx`

Now our Counter becomes:

```tsx
import {
    useAppSelector,
    useAppDispatch
} from "./store/hooks";

import {
    increment,
    decrement
} from "./store/counterSlice";

function Counter() {
    const count = useAppSelector(
        (state) => state.counter.value
    );

    const dispatch = useAppDispatch();

    return (
        <div>
            <h1>{count}</h1>

            <button onClick={() => dispatch(increment())}>
                Increment
            </button>

            <button onClick={() => dispatch(decrement())}>
                Decrement
            </button>
        </div>
    );
}

export default Counter;
```

This is the approach we'll use going forward.

---

# 11. Understand the Architecture

Our Redux folder is now:

```text
store
│
├── store.ts
│
├── hooks.ts
│
└── counterSlice.ts
```

Responsibilities:

```text
store.ts
   ↓
Creates Redux Store
   ↓
Defines RootState + AppDispatch


hooks.ts
   ↓
Creates typed Redux hooks


counterSlice.ts
   ↓
Owns counter state
   ↓
Owns counter reducers/actions
```

---

# 12. Full Redux Flow Now

```text
                     Redux Store
                         │
                         ↓
                      counter
                         │
                         ↓
                       value
                         │
                ┌────────┴────────┐
                ↓                 ↓
        useAppSelector      useAppDispatch
             │                   │
             │                   ↓
             │              dispatch()
             │                   │
             │                   ↓
             │              increment()
             │                   │
             │                   ↓
             │                reducer
             │                   │
             └───────────←────────┘
```

---

# 13. Why We Learned This Before Cart

Our roadmap says:

```text
Redux Toolkit
      ↓
Build: Cart System
```

A cart will have more realistic state:

```text
cart
│
├── items
│
├── addToCart
├── removeFromCart
├── increaseQuantity
└── decreaseQuantity
```

And we'll want TypeScript to understand all of it.

So learning the typing now prevents us from carrying confusing code into the Cart project.

---

# 14. Important Things to Remember

For Redux Toolkit + TypeScript:

```text
store.ts
    ↓
RootState
AppDispatch
```

```text
hooks.ts
    ↓
useAppSelector
useAppDispatch
```

```text
slice.ts
    ↓
state
reducers
actions
```

And components use:

```text
useAppSelector()
     ↓
READ
```

and:

```text
useAppDispatch()
     ↓
CHANGE
```

---

# Phase 7 Progress

```text
## 7. State Management

### Context API

[x] Context API
[x] Auth state app

### Redux Toolkit

[x] Why Redux?
[x] Context vs Redux
[x] Install Redux Toolkit
[x] Redux Store
[x] Provider
[x] createSlice()
[x] initialState
[x] reducers
[x] actions
[x] useSelector()
[x] useDispatch()
[x] Counter with Redux
[x] TypeScript + Redux Toolkit
```

### Next → Step 13: Build the Cart System

This is the **actual project from your roadmap**.

We'll create products, add them to the cart, remove them, and change quantities. This will let you see why Redux Toolkit is useful in a real application rather than just a counter.

-------------------------------------------------------------------------------------------------------------------------------------------