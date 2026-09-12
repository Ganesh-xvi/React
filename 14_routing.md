
# 6. React Routing

---

# 1. What is Routing?

Imagine a website has different pages:

```text
Home
About
Products
Contact
```

Each page can have a URL:

```text
/           → Home
/about      → About
/products   → Products
/contact    → Contact
```

**Routing** is the process of deciding:

> "When the user goes to this URL, which page/component should React display?"

---

# 2. Without Routing

Imagine our React app only has:

```text
App
 ↓
Home
```

If you want an About page, you might think:

```text
App
 ↓
if something
   ↓
Home
or
About
```

But managing many pages this way becomes difficult.

We want:

```text
URL
 ↓
Router
 ↓
Correct component
```

For example:

```text
/about
   ↓
Router
   ↓
About component
```

---

# 3. What is React Router?

**React Router** is a library used to handle routing in React applications.

It lets us define:

```text
URL → Component
```

For example:

```text
/        → Home
/about   → About
/contact → Contact
```

So the basic idea is:

```text
Browser URL
     ↓
React Router
     ↓
Matching route
     ↓
React component
```

---

# 4. Why Do We Need It?

Suppose we build an e-commerce application.

We might have:

```text
/
/products
/products/101
/products/102
/cart
/login
/profile
```

We don't want to put all of these pages into one giant component.

Instead:

```text
Route
 ↓
Component
```

For example:

```text
/products
     ↓
ProductsPage

/products/101
     ↓
ProductDetailsPage

/cart
     ↓
CartPage
```

This keeps the application organized.

---

# 5. Important: React Is Still One Application

This is something beginners often misunderstand.

If you go from:

```text
/
```

to:

```text
/about
```

the browser doesn't necessarily load a completely new HTML page from the server.

Instead, React Router can change what React displays.

Think:

```text
Same React App
      ↓
URL changes
      ↓
Router detects URL
      ↓
Different component displayed
```

This is why React applications are often called **Single Page Applications (SPAs)**.

---

# 6. Our First Routing Example

Imagine we have three components:

```text
Home
About
Contact
```

And these routes:

```text
/        → Home
/about   → About
/contact → Contact
```

The structure is:

```text
React App
   │
   └── Router
       │
       ├── /        → Home
       ├── /about   → About
       └── /contact → Contact
```

---

# 7. Install React Router

For our project, we'll use the current React Router package.

Install:

```text
npm install react-router-dom
```

Then we can create our routes.

---

# 8. The Main Components We'll Learn

Before we write the full application, understand these names:

```text
BrowserRouter
Routes
Route
Link
```

They each have a job.

### `BrowserRouter`

Provides routing functionality to the React application.

Think:

```text
BrowserRouter
     ↓
Enables routing
```

---

### `Routes`

Contains your routes.

Think:

```text
Routes
  ↓
All route definitions
```

---

### `Route`

Defines one URL and the component associated with it.

Think:

```text
Route
 ↓
URL → Component
```

---

### `Link`

Used to navigate between routes without using a normal browser page reload.

Think:

```text
Link
 ↓
Navigate to another route
```

---

# 9. Example Structure

Conceptually:

```text
BrowserRouter
      ↓
   Routes
      ↓
 ┌────┼────────┐
 ↓    ↓        ↓
Route Route   Route
 /    /about  /contact
 ↓      ↓       ↓
Home   About   Contact
```

That's the foundation of React Router.

---

# 10. `Link` vs `<a>`

You already learned HTML:

```html
<a href="/about">About</a>
```

React Router gives us:

```tsx
<Link to="/about">About</Link>
```

The purpose is similar:

```text
Click About
     ↓
Go to /about
```

But React Router's `Link` is designed for navigation inside the React application.

For internal React routes, we'll generally use:

```text
<Link>
```

rather than a normal:

```text
<a>
```

---

# 11. Our Learning Order

We won't jump directly into dynamic routes.

We'll learn routing in this order:

```text
Step 1
What is routing                    

Step 2
Create BrowserRouter

Step 3
Create Routes and Route

Step 4
Create Home / About / Contact

Step 5
Navigate using Link

Step 6
Understand URL parameters

Step 7
Dynamic routes

Step 8
Nested routes

Step 9
Build multi-page app
```

---

# The Main Idea

Remember just this for now:

```text
URL
 ↓
React Router
 ↓
Route
 ↓
Component
```

Example:

```text
/about
   ↓
React Router
   ↓
About component
   ↓
About page appears
```

**Next → Step 2: We'll create our first React Router setup and make `/`, `/about`, and `/contact` work.**


-------------------------------------------------------------------------------------------------------------------------------------------


# Step 2 — Create Our First React Router Setup

Now we'll actually set up routing.

Our goal is very simple:

```text
/          → Home
/about     → About
/contact   → Contact
```

---

# 1. Install React Router

If you haven't already installed it:

```text
npm install react-router-dom
```

We need this package because React itself doesn't provide browser routing.

---

# 2. Our Files

We'll keep the project simple for now.

```text
src
│
├── App.tsx
├── Home.tsx
├── About.tsx
├── Contact.tsx
└── main.tsx
```

We're creating **three components**:

```text
Home
About
Contact
```

---

# 3. Create `Home.tsx`

```tsx
function Home() {
    return <h1>Home Page</h1>;
}

export default Home;
```

This is just a normal React functional component.

---

# 4. Create `About.tsx`

```tsx
function About() {
    return <h1>About Page</h1>;
}

export default About;
```

---

# 5. Create `Contact.tsx`

```tsx
function Contact() {
    return <h1>Contact Page</h1>;
}

export default Contact;
```

Now we have:

```text
Home.tsx
   ↓
Home component

About.tsx
   ↓
About component

Contact.tsx
   ↓
Contact component
```

---

# 6. Create the Router

Now open `App.tsx`.

We'll import:

```tsx
import { BrowserRouter, Routes, Route } from "react-router-dom";
```

Then:

```tsx
function App() {
    return (
        <BrowserRouter>
            <Routes>
                <Route path="/" element={<Home />} />
                <Route path="/about" element={<About />} />
                <Route path="/contact" element={<Contact />} />
            </Routes>
        </BrowserRouter>
    );
}
```

And import our components:

```tsx
import Home from "./Home";
import About from "./About";
import Contact from "./Contact";
```

---

# 7. Full `App.tsx`

```tsx
import { BrowserRouter, Routes, Route } from "react-router-dom";

import Home from "./Home";
import About from "./About";
import Contact from "./Contact";

function App() {
    return (
        <BrowserRouter>
            <Routes>
                <Route path="/" element={<Home />} />
                <Route path="/about" element={<About />} />
                <Route path="/contact" element={<Contact />} />
            </Routes>
        </BrowserRouter>
    );
}

export default App;
```

---

# 8. Understand `BrowserRouter`

Look at:

```tsx
<BrowserRouter>
    ...
</BrowserRouter>
```

This provides the routing system to our application.

Think:

```text
BrowserRouter
      ↓
React application
      ↓
Routing available
```

Without it, our routes won't work this way.

---

# 9. Understand `Routes`

Inside:

```tsx
<BrowserRouter>
    <Routes>
        ...
    </Routes>
</BrowserRouter>
```

`Routes` contains our route definitions.

Think:

```text
BrowserRouter
     ↓
   Routes
     ↓
All available routes
```

---

# 10. Understand `Route`

Now look at:

```tsx
<Route
    path="/about"
    element={<About />}
/>
```

This means:

> When the URL is `/about`, display the `About` component.

So:

```text
/about
   ↓
Route
   ↓
<About />
```

Similarly:

```tsx
<Route path="/" element={<Home />} />
```

means:

```text
/
 ↓
Home
```

And:

```tsx
<Route path="/contact" element={<Contact />} />
```

means:

```text
/contact
    ↓
Contact
```

---

# 11. Test It

Start your React application.

Then manually change the URL.

### Home

```text
http://localhost:5173/
```

You should see:

```text
Home Page
```

### About

```text
http://localhost:5173/about
```

You should see:

```text
About Page
```

### Contact

```text
http://localhost:5173/contact
```

You should see:

```text
Contact Page
```

---

# 12. What Just Happened?

We didn't create three separate applications.

We have:

```text
One React App
     ↓
BrowserRouter
     ↓
URL
     ↓
Routes
     ↓
Correct component
```

For example:

```text
URL = /about

      ↓

<Route path="/about">

      ↓

<About />

      ↓

About Page
```

---

# 13. One Important Thing

Right now, if you're on:

```text
/about
```

and want to go to:

```text
/contact
```

you could manually type the URL.

But users shouldn't have to do that.

We need navigation links.

That's what **`Link`** is for.

---

## Current Progress

```text
React Router

[x] Understand routing
[x] Install React Router
[x] BrowserRouter
[x] Routes
[x] Route
[ ] Link navigation
[ ] Dynamic routes
[ ] Nested routes
[ ] Multi-page app
```

**Next → Step 3: `Link` — we'll create a navigation bar so you can click Home, About, and Contact instead of manually changing the URL.**

-------------------------------------------------------------------------------------------------------------------------------------------

# Step 3 — Navigation with `Link`

Our routes are working now:

```text
/          → Home
/about     → About
/contact   → Contact
```

But currently, we have to **type the URL manually**.

Now we'll create navigation links.

---

# 1. What is `Link`?

React Router provides a component called:

```tsx
<Link>
```

It lets the user move from one route to another.

For example:

```tsx
<Link to="/about">About</Link>
```

Think:

```text
Click "About"
      ↓
Go to /about
      ↓
About component appears
```

---

# 2. Import `Link`

In `App.tsx`:

```tsx
import {
    BrowserRouter,
    Routes,
    Route,
    Link
} from "react-router-dom";
```

We now have:

```text
BrowserRouter → enables routing
Routes        → contains routes
Route         → defines URL → component
Link          → navigation
```

---

# 3. Create a Navigation Bar

Inside `BrowserRouter`, before `Routes`:

```tsx
<nav>
    <Link to="/">Home</Link>
    <Link to="/about">About</Link>
    <Link to="/contact">Contact</Link>
</nav>
```

So our structure becomes:

```text
BrowserRouter
│
├── Navigation
│   ├── Home
│   ├── About
│   └── Contact
│
└── Routes
    ├── /
    ├── /about
    └── /contact
```

---

# 4. Full `App.tsx`

```tsx
import {
    BrowserRouter,
    Routes,
    Route,
    Link
} from "react-router-dom";

import Home from "./Home";
import About from "./About";
import Contact from "./Contact";

function App() {
    return (
        <BrowserRouter>

            <nav>
                <Link to="/">Home</Link>
                <Link to="/about">About</Link>
                <Link to="/contact">Contact</Link>
            </nav>

            <Routes>
                <Route path="/" element={<Home />} />
                <Route path="/about" element={<About />} />
                <Route path="/contact" element={<Contact />} />
            </Routes>

        </BrowserRouter>
    );
}

export default App;
```

---

# 5. What Happens Now?

When the application starts:

```text
Home | About | Contact
```

If you click:

```text
About
```

React Router changes the URL:

```text
/about
```

Then:

```text
/about
   ↓
matching Route
   ↓
<About />
   ↓
About Page
```

If you click:

```text
Contact
```

then:

```text
/contact
   ↓
<Route path="/contact">
   ↓
<Contact />
```

---

# 6. `Link` vs HTML `<a>`

You already know HTML:

```html
<a href="/about">About</a>
```

React Router uses:

```tsx
<Link to="/about">About</Link>
```

The important difference is:

```text
<a>
 ↓
Normal browser navigation

<Link>
 ↓
React Router navigation
```

For navigation **inside our React application**, we'll generally use `Link`.

---

# 7. Why Not Use `href`?

Don't write:

```tsx
<Link href="/about">
```

`Link` uses:

```tsx
<Link to="/about">
```

So remember:

```text
HTML <a>       → href
React Router   → to
```

---

# 8. Our Routing Flow

We now have the complete basic routing flow:

```text
User clicks Link
       ↓
<Link to="/about">
       ↓
URL changes
       ↓
React Router checks Routes
       ↓
Finds /about
       ↓
Renders About
```

---

# 9. One Important Observation

Notice something interesting.

Our navigation stays visible:

```text
Home | About | Contact
```

while the page content changes:

```text
Home Page
```

then:

```text
About Page
```

then:

```text
Contact Page
```

That's because the navigation is **outside `<Routes>`**.

Our structure is:

```text
BrowserRouter
│
├── Navigation       ← stays visible
│
└── Routes           ← changes based on URL
```

This pattern will become very useful when we build layouts.

---

## Current Progress

```text
React Router

[x] What is routing
[x] BrowserRouter
[x] Routes
[x] Route
[x] Link navigation
[ ] Dynamic routes
[ ] Nested routes
[ ] Multi-page app
```

### Next → Step 4: Dynamic Routes

We'll learn how to handle URLs like:

```text
/products/1
/products/2
/products/3
```

with **one component** instead of creating a separate route for every product.

-------------------------------------------------------------------------------------------------------------------------------------------

# Step 4 — Dynamic Routes

Now we move to **Dynamic Routes**.

So far, we have fixed routes:

```text
/          → Home
/about     → About
/contact   → Contact
```

But real applications need URLs where part of the URL changes.

For example, an e-commerce app:

```text
/products/1
/products/2
/products/3
```

We don't want to create:

```text
/products/1
/products/2
/products/3
...
```

Instead, we create **one dynamic route**.

---

# 1. What Is a Dynamic Route?

A dynamic route contains a **variable part**.

For example:

```text
/products/:id
```

Here:

```text
/products/
```

is fixed.

And:

```text
:id
```

is dynamic.

So this one route can match:

```text
/products/1
/products/2
/products/100
/products/500
```

Think:

```text
/products/:id
          ↑
       changes
```

---

# 2. Create a Product Component

Create:

```text
Product.tsx
```

For now:

```tsx
function Product() {
    return <h1>Product Details</h1>;
}

export default Product;
```

---

# 3. Create the Dynamic Route

In `App.tsx`, add:

```tsx
<Route path="/products/:id" element={<Product />} />
```

And import:

```tsx
import Product from "./Product";
```

Now our routes look like:

```text
/                  → Home
/about             → About
/contact           → Contact
/products/:id      → Product
```

---

# 4. Test It

Open:

```text
/products/1
```

You should see:

```text
Product Details
```

Now change the URL:

```text
/products/25
```

You'll still see:

```text
Product Details
```

That's because both URLs match:

```text
/products/:id
```

---

# 5. But Where Did `1` Go?

This is the important question.

If the URL is:

```text
/products/25
```

React Router needs to give our component access to:

```text
25
```

We use a Hook called:

```text
useParams
```

This is provided by React Router.

---

# 6. Use `useParams`

Update `Product.tsx`:

```tsx
import { useParams } from "react-router-dom";

function Product() {
    const { id } = useParams();

    return <h1>Product ID: {id}</h1>;
}

export default Product;
```

Now:

```text
/products/25
```

will display:

```text
Product ID: 25
```

And:

```text
/products/100
```

will display:

```text
Product ID: 100
```

---

# 7. Understand the Flow

This is the most important part:

```text
URL

/products/25
       ↓
Route
       ↓
/products/:id
       ↓
id = 25
       ↓
useParams()
       ↓
Product component
       ↓
Product ID: 25
```

So `:id` is the **parameter**, and `useParams()` reads it.

---

# 8. Why Is It Called `id`?

Because we wrote:

```tsx
path="/products/:id"
```

The name after `:` becomes the parameter name.

For example:

```tsx
path="/products/:productId"
```

Then:

```tsx
const { productId } = useParams();
```

Or:

```tsx
path="/users/:userId"
```

Then:

```tsx
const { userId } = useParams();
```

So:

```text
:id
 ↓
id

:userId
 ↓
userId

:productId
 ↓
productId
```

---

# 9. Real-World Example

Imagine an API has products:

```text
Product 1
Product 2
Product 3
```

When the user clicks Product 2:

```text
/products/2
```

React Router gets:

```text
id = 2
```

Then we can use that ID to request the product:

```text
/products/2
       ↓
useParams()
       ↓
id = 2
       ↓
fetch product 2
       ↓
display product details
```

This is how product-detail pages commonly work.

---

# 10. `useParams` Is a React Router Hook

You asked earlier whether something belongs to React Hooks.

Here we have:

```text
React Hooks
   ↓
useState
useEffect
useRef
useMemo
useCallback
```

And React Router has its own hooks, such as:

```text
React Router Hooks
   ↓
useParams
useNavigate
useLocation
```

So don't confuse them.

`useParams` is **not a React core Hook** like `useState`.

> **useParams** that allows you to access dynamic values (parameters) from the current URL

It's a **React Router Hook**.

---

# 11. One More Example

Suppose we create:

```tsx
<Route path="/users/:userId" element={<User />} />
```

Then:

```text
/users/10
```

gives:

```text
userId = 10
```

And:

```text
/users/500
```

gives:

```text
userId = 500
```

One route handles all of them.

---

# Current Progress

```text
React Router

[x] What is routing
[x] BrowserRouter
[x] Routes
[x] Route
[x] Link navigation
[x] Dynamic routes
[x] useParams
[ ] Nested routes
[ ] Multi-page app
```

### Next → Step 5: Nested Routes

We'll learn how to create a structure like:

```text
/dashboard
    │
    ├── /dashboard/profile
    ├── /dashboard/settings
    └── /dashboard/orders
```

and understand why **nested routes** are useful.

-------------------------------------------------------------------------------------------------------------------------------------------


# Step 5 — Nested Routes

Now we are going to learn **Nested Routes**.


# 1. What Are Nested Routes?

Suppose we have a Dashboard:

```text
/dashboard
```

Inside the Dashboard, we have:

```text
/dashboard/profile
/dashboard/settings
/dashboard/orders
```

These are called **nested routes** because:

```text
/dashboard
    ↓
    ├── profile
    ├── settings
    └── orders
```

Think of it as:

> A page inside another page.

---

# 2. Why Do We Need Nested Routes?

Imagine the Dashboard has a common layout:

```text
----------------------------
        Dashboard
----------------------------
Profile | Settings | Orders
----------------------------

       Page Content

----------------------------
```

When you click:

```text
Profile
```

only the content below the navigation should change.

The Dashboard layout should remain.

So:

```text
Dashboard layout
      ↓
    stays
      ↓
Child route changes
```

That's the main purpose of nested routes.

---

# 3. Our File Structure

Let's create:

```text
src
│
├── App.tsx
│
├── Home.tsx
│
├── Dashboard.tsx
├── Profile.tsx
├── Settings.tsx
└── Orders.tsx
```

---

# 4. `Home.tsx`

```tsx
function Home() {
    return <h1>Home Page</h1>;
}

export default Home;
```

---

# 5. `Profile.tsx`

```tsx
function Profile() {
    return <h2>Profile Page</h2>;
}

export default Profile;
```

---

# 6. `Settings.tsx`

```tsx
function Settings() {
    return <h2>Settings Page</h2>;
}

export default Settings;
```

---

# 7. `Orders.tsx`

```tsx
function Orders() {
    return <h2>Orders Page</h2>;
}

export default Orders;
```

---

# 8. The Important Part — `Dashboard.tsx`

This is where nested routing becomes interesting.

```tsx
import { Link, Outlet } from "react-router-dom";

function Dashboard() {
    return (
        <div>
            <h1>Dashboard</h1>

            <nav>
                <Link to="/dashboard/profile">Profile</Link>
                <Link to="/dashboard/settings">Settings</Link>
                <Link to="/dashboard/orders">Orders</Link>
            </nav>

            <hr />

            <Outlet />
        </div>
    );
}

export default Dashboard;
```

There are two important things here:

```text
Link
Outlet
```

We already know `Link`.

Now let's understand `Outlet`.

---

# 9. What Is `Outlet`?

`Outlet` tells React Router:

> "Render the matching child route here."

? **<hr />** is an HTML tag used to create a horizontal line on a webpage.

For example:

```tsx
<Outlet />
```

Imagine it as an empty space:

```text
Dashboard
----------------
Profile Settings Orders
----------------

       ↓
    <Outlet />

       ↓

Child page appears here
```

---

# 10. Create the Routes

Now `App.tsx`:

```tsx
import {
    BrowserRouter,
    Routes,
    Route
} from "react-router-dom";

import Home from "./Home";
import Dashboard from "./Dashboard";
import Profile from "./Profile";
import Settings from "./Settings";
import Orders from "./Orders";

function App() {
    return (
        <BrowserRouter>
            <Routes>

                <Route path="/" element={<Home />} />

                <Route path="/dashboard" element={<Dashboard />}>

                    <Route
                        path="profile"
                        element={<Profile />}
                    />

                    <Route
                        path="settings"
                        element={<Settings />}
                    />

                    <Route
                        path="orders"
                        element={<Orders />}
                    />

                </Route>

            </Routes>
        </BrowserRouter>
    );
}

export default App;
```

---

# 11. Look Carefully at This Part

We have:

```tsx
<Route path="/dashboard" element={<Dashboard />}>
```

Inside it:

```tsx
<Route path="profile" element={<Profile />} />
<Route path="settings" element={<Settings />} />
<Route path="orders" element={<Orders />} />
```

This means:

```text
/dashboard
    │
    ├── profile
    ├── settings
    └── orders
```

React Router combines them:

```text
/dashboard + profile
        ↓
/dashboard/profile
```

and:

```text
/dashboard + settings
        ↓
/dashboard/settings
```

and:

```text
/dashboard + orders
        ↓
/dashboard/orders
```

---

# 12. Why Don't We Write `/dashboard/profile`?

Notice we wrote:

```tsx
path="profile"
```

not:

```tsx
path="/dashboard/profile"
```

Because this is a **child route**.

React Router automatically understands:

```text
Parent:
 /dashboard

Child:
 profile

Result:
 /dashboard/profile
```

---

# 13. The Most Important Part — `Outlet`

Let's say the user visits:

```text
/dashboard/profile
```

React Router does this:

```text
/dashboard
     ↓
Dashboard component
     ↓
matches "profile"
     ↓
Profile component
```

But where does `Profile` appear?

Here:

```tsx
<Outlet />
```

So the final UI is:

```text
Dashboard
----------------
Profile Settings Orders
----------------

Profile Page
```

The `Dashboard` component remains.

Only the `<Outlet />` content changes.

---

# 14. Click Settings

User clicks:

```text
Settings
```

URL becomes:

```text
/dashboard/settings
```

React Router:

```text
/dashboard
      ↓
Dashboard
      ↓
child = settings
      ↓
Settings
      ↓
Outlet
```

Result:

```text
Dashboard
----------------
Profile Settings Orders
----------------

Settings Page
```

---

# 15. Click Orders

URL:

```text
/dashboard/orders
```

Result:

```text
Dashboard
----------------
Profile Settings Orders
----------------

Orders Page
```

Again:

**Dashboard stays.**

The child content changes.

---

# 16. Complete Project Structure

So our project is:

```text
src
│
├── App.tsx
│
├── Home.tsx
│
├── Dashboard.tsx
├── Profile.tsx
├── Settings.tsx
└── Orders.tsx
```

---

# 17. Complete Code

### `App.tsx`

```tsx
import {
    BrowserRouter,
    Routes,
    Route
} from "react-router-dom";

import Home from "./Home";
import Dashboard from "./Dashboard";
import Profile from "./Profile";
import Settings from "./Settings";
import Orders from "./Orders";

function App() {
    return (
        <BrowserRouter>
            <Routes>

                <Route path="/" element={<Home />} />

                <Route path="/dashboard" element={<Dashboard />}>

                    <Route
                        path="profile"
                        element={<Profile />}
                    />

                    <Route
                        path="settings"
                        element={<Settings />}
                    />

                    <Route
                        path="orders"
                        element={<Orders />}
                    />

                </Route>

            </Routes>
        </BrowserRouter>
    );
}

export default App;
```

### `Home.tsx`

```tsx
function Home() {
    return <h1>Home Page</h1>;
}

export default Home;
```

### `Dashboard.tsx`

```tsx
import { Link, Outlet } from "react-router-dom";

function Dashboard() {
    return (
        <div>
            <h1>Dashboard</h1>

            <nav>
                <Link to="/dashboard/profile">Profile</Link>
                <Link to="/dashboard/settings">Settings</Link>
                <Link to="/dashboard/orders">Orders</Link>
            </nav>

            <hr />

            <Outlet />
        </div>
    );
}

export default Dashboard;
```

### `Profile.tsx`

```tsx
function Profile() {
    return <h2>Profile Page</h2>;
}

export default Profile;
```

### `Settings.tsx`

```tsx
function Settings() {
    return <h2>Settings Page</h2>;
}

export default Settings;
```

### `Orders.tsx`

```tsx
function Orders() {
    return <h2>Orders Page</h2>;
}

export default Orders;
```

---

# 18. Test These URLs

Start your application and test:

```text
/
```

Result:

```text
Home Page
```

Then:

```text
/dashboard
```

You'll see:

```text
Dashboard
Profile Settings Orders
```

Then:

```text
/dashboard/profile
```

You'll see:

```text
Dashboard
Profile Settings Orders

Profile Page
```

Then:

```text
/dashboard/settings
```

You'll see:

```text
Dashboard
Profile Settings Orders

Settings Page
```

Then:

```text
/dashboard/orders
```

You'll see:

```text
Dashboard
Profile Settings Orders

Orders Page
```

---

# 19. Understand the Difference

### Normal route

```text
/about
```

```text
Route
 ↓
About
```

### Dynamic route

```text
/products/:id
```

```text
/products/25
       ↓
useParams()
       ↓
id = 25
```

### Nested route

```text
/dashboard
      ↓
Dashboard
      ↓
Outlet
      ↓
Profile / Settings / Orders
```

---

# 20. The Three Things to Remember

At this point, React Router has three major concepts:

```text
Route
 ↓
URL → Component
```

```text
Dynamic Route
 ↓
URL → Parameter → Component
```

```text
Nested Route
 ↓
Parent → Outlet → Child
```

And `Link` is how we navigate between them.

---

## Phase 6 Progress

```text
## 6. Routing

[x] React Router
[x] Basic routes
[x] Link navigation
[x] Dynamic routes
[x] useParams
[x] Nested routes
[ ] Multi-page app
```

### Next → Step 6: Build the **Multi-Page App**

We'll combine everything we've learned:

```text
Home
Products
Product Details
Dashboard
 ├── Profile
 ├── Settings
 └── Orders
```

That will complete the **Routing phase** of your roadmap.


-------------------------------------------------------------------------------------------------------------------------------------------