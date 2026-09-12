# Step 6 — Build the Multi-Page App

Now we'll combine everything we learned in **React Routing** into one small application.

---

# 1. What Are We Building?

Our app will have:

```text
Home
Products
Product Details
Dashboard
    ├── Profile
    ├── Settings
    └── Orders
```

And the URLs will be:

```text
/                       → Home
/products               → Products
/products/:id           → Product Details

/dashboard              → Dashboard
/dashboard/profile      → Profile
/dashboard/settings     → Settings
/dashboard/orders       → Orders
```

So we'll use:

* Basic routes
* `Link`
* Dynamic routes
* `useParams`
* Nested routes
* `Outlet`

---

# 2. Create the Files

Your `src` folder will look like:

```text
src
│
├── App.tsx
│
├── Home.tsx
├── Products.tsx
├── ProductDetails.tsx
│
├── Dashboard.tsx
├── Profile.tsx
├── Settings.tsx
└── Orders.tsx
```

Don't create any additional folders yet.

---

# 3. `Home.tsx`

```tsx
function Home() {
    return (
        <div>
            <h1>Home Page</h1>
            <p>Welcome to our application.</p>
        </div>
    );
}

export default Home;
```

---

# 4. `Products.tsx`

Here we'll display a few products.

```tsx
import { Link } from "react-router-dom";

function Products() {
    return (
        <div>
            <h1>Products</h1>

            <div>
                <h2>Laptop</h2>
                <Link to="/products/1">
                    View Laptop
                </Link>
            </div>

            <div>
                <h2>Phone</h2>
                <Link to="/products/2">
                    View Phone
                </Link>
            </div>

            <div>
                <h2>Headphones</h2>
                <Link to="/products/3">
                    View Headphones
                </Link>
            </div>
        </div>
    );
}

export default Products;
```

Notice:

```text
Laptop → /products/1
Phone → /products/2
Headphones → /products/3
```

We're using **one dynamic route** for all three products.

---

# 5. `ProductDetails.tsx`

Now we need to read the product ID.

```tsx
import { useParams } from "react-router-dom";

function ProductDetails() {
    const { id } = useParams();

    return (
        <div>
            <h1>Product Details</h1>
            <p>Product ID: {id}</p>
        </div>
    );
}

export default ProductDetails;
```

If the user clicks Laptop:

```text
/products/1
```

we get:

```text
id = 1
```

If they click Phone:

```text
/products/2
```

we get:

```text
id = 2
```

---

# 6. `Dashboard.tsx`

Our Dashboard will be the parent route.

```tsx
import { Link, Outlet } from "react-router-dom";

function Dashboard() {
    return (
        <div>
            <h1>Dashboard</h1>

            <nav>
                <Link to="/dashboard/profile">
                    Profile
                </Link>

                <Link to="/dashboard/settings">
                    Settings
                </Link>

                <Link to="/dashboard/orders">
                    Orders
                </Link>
            </nav>

            <hr />

            <Outlet />
        </div>
    );
}

export default Dashboard;
```

Remember:

```text
Outlet
 ↓
Child route appears here
```

---

# 7. `Profile.tsx`

```tsx
function Profile() {
    return <h2>Profile Page</h2>;
}

export default Profile;
```

---

# 8. `Settings.tsx`

```tsx
function Settings() {
    return <h2>Settings Page</h2>;
}

export default Settings;
```

---

# 9. `Orders.tsx`

```tsx
function Orders() {
    return <h2>Orders Page</h2>;
}

export default Orders;
```

---

# 10. Now the Important File — `App.tsx`

This connects everything.

```tsx
import {
    BrowserRouter,
    Routes,
    Route,
    Link
} from "react-router-dom";

import Home from "./Home";
import Products from "./Products";
import ProductDetails from "./ProductDetails";

import Dashboard from "./Dashboard";
import Profile from "./Profile";
import Settings from "./Settings";
import Orders from "./Orders";

function App() {
    return (
        <BrowserRouter>

            <nav>
                <Link to="/">Home</Link>

                <Link to="/products">
                    Products
                </Link>

                <Link to="/dashboard">
                    Dashboard
                </Link>
            </nav>

            <hr />

            <Routes>

                <Route
                    path="/"
                    element={<Home />}
                />

                <Route
                    path="/products"
                    element={<Products />}
                />

                <Route
                    path="/products/:id"
                    element={<ProductDetails />}
                />

                <Route
                    path="/dashboard"
                    element={<Dashboard />}
                >
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

# 11. Understand the Complete Route Structure

Our application now looks like:

```text
BrowserRouter
│
├── Navigation
│
└── Routes
    │
    ├── /
    │    └── Home
    │
    ├── /products
    │    └── Products
    │
    ├── /products/:id
    │    └── ProductDetails
    │
    └── /dashboard
         │
         └── Dashboard
              │
              └── Outlet
                   │
                   ├── profile
                   │    └── Profile
                   │
                   ├── settings
                   │    └── Settings
                   │
                   └── orders
                        └── Orders
```

This is basically everything from Phase 6.

---

# 12. Test the Application

### Home

```text
/
```

Shows:

```text
Home Page
```

---

### Products

Click:

```text
Products
```

URL:

```text
/products
```

Shows:

```text
Products

Laptop
View Laptop

Phone
View Phone

Headphones
View Headphones
```

---

### Product Details

Click:

```text
View Laptop
```

URL:

```text
/products/1
```

Shows:

```text
Product Details

Product ID: 1
```

Click Phone:

```text
/products/2
```

Shows:

```text
Product ID: 2
```

That's our **dynamic route**.

---

# 13. Dashboard

Click:

```text
Dashboard
```

URL:

```text
/dashboard
```

Shows:

```text
Dashboard

Profile
Settings
Orders
```

Click Profile:

```text
/dashboard/profile
```

Result:

```text
Dashboard

Profile Settings Orders

Profile Page
```

Click Settings:

```text
/dashboard/settings
```

Result:

```text
Dashboard

Profile Settings Orders

Settings Page
```

Click Orders:

```text
/dashboard/orders
```

Result:

```text
Dashboard

Profile Settings Orders

Orders Page
```

That's our **nested routing**.

---

# 14. What We Have Learned

Let's connect each concept to the application.

### Basic Route

```tsx
<Route path="/products" element={<Products />} />
```

Means:

```text
/products
    ↓
Products
```

### Dynamic Route

```tsx
<Route path="/products/:id" element={<ProductDetails />} />
```

Means:

```text
/products/1
/products/2
/products/3
...
```

all use:

```text
ProductDetails
```

---

### `useParams`

```tsx
const { id } = useParams();
```

Gets:

```text
/products/25
       ↓
id = 25
```

---

### Nested Route

```tsx
<Route path="/dashboard" element={<Dashboard />}>
```

with:

```tsx
<Route path="profile" element={<Profile />} />
```

creates:

```text
/dashboard/profile
```

---

### `Outlet`

Inside `Dashboard`:

```tsx
<Outlet />
```

means:

```text
Child route
    ↓
render here
```

---

### `Link`

```tsx
<Link to="/products">
    Products
</Link>
```

lets the user navigate to:

```text
/products
```

---

# 15. Phase 6 Is Complete

Your roadmap says:

```text
## 6. Routing

[x] React Router
[x] Dynamic routes
[x] Nested routes

Build: multi-page app.
```

We've completed all of it.

The important mental model to remember is:

```text
                 React Router
                      │
          ┌───────────┼───────────┐
          ↓           ↓           ↓
       Normal      Dynamic      Nested
       Route        Route        Route
          ↓           ↓           ↓
       /about    /products/:id   /dashboard
                    ↓               ↓
               useParams()       Outlet
```

-------------------------------------------------------------------------------------------------------------------------------------------