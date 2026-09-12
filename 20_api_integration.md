# Phase 9 — API Integration

Styling is complete. Now we move to **API Integration**.

From your roadmap:

```text
## 9. API Integration

[ ] Fetch / Axios
[ ] REST
[ ] Error handling
[ ] Loading states
[ ] React Query / TanStack Query

Build:
- Weather app
- Blog CRUD app
```

We'll take this **step by step**, just like before.

---

# Step 1 — What Is an API?

An API allows our frontend to communicate with another application/server.

Think about our React app:

```text
React Frontend
      ↓
     API
      ↓
Backend / Server
      ↓
   Database
```

For example, our React application might ask:

> "Give me all the products."

The API might respond:

```json
[
  {
    "id": 1,
    "name": "Laptop",
    "price": 50000
  },
  {
    "id": 2,
    "name": "Phone",
    "price": 30000
  }
]
```

React receives this data and displays it.

---

# Step 2 — What Is a REST API?

REST is a common way of designing APIs.

For example, imagine we have a `products` resource.

We might have:

```text
GET     /products
POST    /products
GET     /products/1
PUT     /products/1
DELETE  /products/1
```

Each HTTP method represents an operation.

| Method | Meaning     |
| ------ | ----------- |
| GET    | Get data    |
| POST   | Create data |
| PUT    | Update data |
| DELETE | Delete data |

Think:

```text
GET
 ↓
Give me data

POST
 ↓
Create data

PUT
 ↓
Update data

DELETE
 ↓
Delete data
```

You've actually already used this concept when we worked with the vanilla JavaScript Todo app.

---

# Step 3 — What Is `fetch()`?

`fetch()` is a browser API that allows JavaScript to make HTTP requests.

Simple example:

```tsx
fetch("https://example.com/products");
```

This sends a request to the server.

But `fetch()` is asynchronous.

So we usually use:

```text
fetch()
   ↓
Promise
   ↓
await
   ↓
Response
   ↓
JSON
```

---

# Step 4 — Basic Fetch Example

Let's say an API returns products.

```tsx
const response = await fetch(
    "https://example.com/products"
);
```

Now:

```text
response
```

is the HTTP response.

It isn't directly the JSON data yet.

We need:

```tsx
const data = await response.json();
```

So:

```tsx
const response = await fetch(
    "https://example.com/products"
);

const data = await response.json();

console.log(data);
```

The flow is:

```text
fetch()
  ↓
HTTP Response
  ↓
response.json()
  ↓
JavaScript data
```

---

# Step 5 — Why Two `await`s?

This is something beginners often find confusing.

Look at:

```tsx
const response = await fetch(url);
```

The first `await` waits for the **HTTP response**.

Then:

```tsx
const data = await response.json();
```

The second `await` waits for the response body to be converted into JavaScript data.

So:

```text
await fetch()
       ↓
Response object

await response.json()
       ↓
Actual data
```

---

# Step 6 — API Data in React

Now let's connect this with React.

Suppose the API returns:

```json
[
    {
        "id": 1,
        "name": "Laptop"
    },
    {
        "id": 2,
        "name": "Phone"
    }
]
```

We want:

```text
API
 ↓
React state
 ↓
map()
 ↓
UI
```

So we might have:

```tsx
import { useEffect, useState } from "react";

type Product = {
    id: number;
    name: string;
};

function Products() {
    const [products, setProducts] = useState<Product[]>([]);

    useEffect(() => {
        async function getProducts() {
            const response = await fetch(
                "https://example.com/products"
            );

            const data = await response.json();

            setProducts(data);
        }

        getProducts();
    }, []);

    return (
        <div>
            {products.map((product) => (
                <p key={product.id}>
                    {product.name}
                </p>
            ))}
        </div>
    );
}
```

Now the flow is:

```text
Component renders
       ↓
useEffect runs
       ↓
fetch API
       ↓
receive response
       ↓
response.json()
       ↓
setProducts()
       ↓
React re-renders
       ↓
products.map()
       ↓
UI
```

---

# Step 7 — Why `useEffect`?

You already learned `useEffect`.

We use it here because fetching data is a **side effect**.

We don't want to do this directly during rendering:

```tsx
function Products() {

    fetch(...);

    return (...);
}
```

Because React can render the component many times.

That could cause unnecessary API requests.

Instead:

```tsx
useEffect(() => {
    // fetch data
}, []);
```

The empty dependency array:

```tsx
[]
```

means the effect runs after the initial render.

---

# Step 8 — Real API Example

Let's use a public API so you can understand the complete flow.

We'll use JSONPlaceholder:

```text
https://jsonplaceholder.typicode.com/posts
```

Example response:

```json
[
    {
        "userId": 1,
        "id": 1,
        "title": "..."
    },
    {
        "userId": 1,
        "id": 2,
        "title": "..."
    }
]
```

Our React component:

```tsx
import { useEffect, useState } from "react";

type Post = {
    userId: number;
    id: number;
    title: string;
};

function Posts() {
    const [posts, setPosts] = useState<Post[]>([]);

    useEffect(() => {
        async function getPosts() {
            const response = await fetch(
                "https://jsonplaceholder.typicode.com/posts"
            );

            const data = await response.json();

            setPosts(data);
        }

        getPosts();
    }, []);

    return (
        <div>
            <h1>Posts</h1>

            {posts.map((post) => (
                <article key={post.id}>
                    <h2>{post.title}</h2>
                </article>
            ))}
        </div>
    );
}

export default Posts;
```

---

# Step 9 — Understand the State

Initially:

```tsx
const [posts, setPosts] = useState<Post[]>([]);
```

So:

```text
posts = []
```

The API hasn't returned anything yet.

Then:

```text
fetch()
 ↓
API response
 ↓
data
 ↓
setPosts(data)
```

Now:

```text
posts = [
   post 1,
   post 2,
   post 3,
   ...
]
```

React re-renders.

Then:

```tsx
posts.map(...)
```

creates the UI.

---

# Step 10 — The Important Architecture

For API-based React applications, start thinking like this:

```text
             API
              ↓
           fetch()
              ↓
         Response
              ↓
          JSON data
              ↓
        React State
              ↓
         Components
              ↓
             UI
```

This pattern is extremely important.

---

# Step 11 — But There Is a Problem

Our example works, but it has a problem.

Imagine the API takes 3 seconds.

During those 3 seconds, what should the user see?

Currently:

```text
Posts
```

and then nothing.

That's not a good user experience.

We need:

```text
Loading...
```

This leads directly to the next concept:

# Loading State

We'll have:

```tsx
const [loading, setLoading] = useState(true);
```

Then:

```text
Request starts
    ↓
loading = true
    ↓
"Loading..."
```

When the request finishes:

```text
API response
    ↓
loading = false
    ↓
Show data
```

And there is another problem:

> What if the API request fails?

We need:

```text
Loading
   ↓
Success → show data

Loading
   ↓
Failure → show error
```

So our API state will eventually look like:

```text
┌─────────────────┐
│ Loading         │
│                 │
│ Success         │
│                 │
│ Error           │
└─────────────────┘
```

That is the **next step**.

---

# Phase 9 Progress

```text
## 9. API Integration

[x] Step 1 — What is an API?
[x] Step 2 — REST API basics
[x] Step 3 — fetch()
[x] Step 4 — Fetch API data in React

[ ] Step 5 — Loading states
[ ] Step 6 — Error handling
[ ] Step 7 — POST / PUT / DELETE
[ ] Step 8 — Axios
[ ] Step 9 — API project
[ ] Step 10 — React Query / TanStack Query
```

### Next → Loading States

We'll build the complete pattern:

```text
API Request
    ↓
Loading
    ↓
Success
    ↓
Display data
```

and then add:

```text
Error
 ↓
Display error message
```

This is one of the most important patterns for real React applications.

------------------------------------------------------------------------------------------------------------------------------------------

# Step 5 — Loading States

In the previous step, we fetched data from an API:

```text
API
 ↓
fetch()
 ↓
setPosts(data)
 ↓
Display Posts
```

But there is an important question:

> What happens while the API request is still running?

---

# 1. The Problem Without a Loading State

Look at our previous code:

```tsx
import { useEffect, useState } from "react";

type Post = {
    userId: number;
    id: number;
    title: string;
};

function Posts() {
    const [posts, setPosts] = useState<Post[]>([]);

    useEffect(() => {
        async function getPosts() {
            const response = await fetch(
                "https://jsonplaceholder.typicode.com/posts"
            );

            const data = await response.json();

            setPosts(data);
        }

        getPosts();
    }, []);

    return (
        <div>
            <h1>Posts</h1>

            {posts.map((post) => (
                <article key={post.id}>
                    <h2>{post.title}</h2>
                </article>
            ))}
        </div>
    );
}

export default Posts;
```

Initially:

```text
posts = []
```

The component renders immediately.

Then:

```text
useEffect
   ↓
API request starts
   ↓
Waiting...
   ↓
Waiting...
   ↓
API response arrives
   ↓
setPosts(data)
```

During the waiting time, the user sees an empty page.

---

# 2. The Solution — Loading State

We create another state:

```tsx
const [loading, setLoading] = useState(true);
```

Now we have:

```text
posts
 ↓
Stores API data

loading
 ↓
Stores request status
```

Initially:

```tsx
loading = true
```

Because the API request hasn't finished yet.

---

# 3. Basic Loading State Example

```tsx
import { useEffect, useState } from "react";

type Post = {
    userId: number;
    id: number;
    title: string;
};

function Posts() {
    const [posts, setPosts] = useState<Post[]>([]);
    const [loading, setLoading] = useState(true);

    useEffect(() => {
        async function getPosts() {
            const response = await fetch(
                "https://jsonplaceholder.typicode.com/posts"
            );

            const data = await response.json();

            setPosts(data);
            setLoading(false);
        }

        getPosts();
    }, []);

    if (loading) {
        return <p>Loading...</p>;
    }

    return (
        <div>
            <h1>Posts</h1>

            {posts.map((post) => (
                <article key={post.id}>
                    <h2>{post.title}</h2>
                </article>
            ))}
        </div>
    );
}

export default Posts;
```

---

# 4. Understand the Flow Carefully

Initially:

```text
loading = true
posts = []
```

React renders:

```tsx
if (loading) {
    return <p>Loading...</p>;
}
```

So the user sees:

```text
Loading...
```

---

Then the API finishes:

```tsx
setPosts(data);
```

Now:

```text
posts = API data
```

Then:

```tsx
setLoading(false);
```

Now:

```text
loading = false
```

React re-renders.

This condition:

```tsx
if (loading)
```

is now false.

So React continues to:

```tsx
return (
    <div>
        ...
    </div>
);
```

The posts appear.

---

# 5. Complete Timeline

```text
1. Component mounts
       ↓
2. loading = true
       ↓
3. Show "Loading..."
       ↓
4. useEffect runs
       ↓
5. API request starts
       ↓
6. API returns data
       ↓
7. setPosts(data)
       ↓
8. setLoading(false)
       ↓
9. Component re-renders
       ↓
10. Show Posts
```

---

# 6. Why `loading` Starts as `true`

This is important.

We write:

```tsx
const [loading, setLoading] = useState(true);
```

Why not:

```tsx
useState(false);
```

Because when the component first loads:

```text
API request
   ↓
Not finished yet
```

Therefore:

```text
loading = true
```

After the API finishes:

```text
loading = false
```

So remember:

```text
Request running
↓
loading = true

Request finished
↓
loading = false
```

---

# 7. Another Common Pattern

Instead of returning early:

```tsx
if (loading) {
    return <p>Loading...</p>;
}
```

You can use conditional rendering:

```tsx
return (
    <div>
        {loading ? (
            <p>Loading...</p>
        ) : (
            posts.map((post) => (
                <article key={post.id}>
                    <h2>{post.title}</h2>
                </article>
            ))
        )}
    </div>
);
```

This means:

```text
loading = true
↓
Show Loading...

loading = false
↓
Show Posts
```

Both approaches are valid.

---

# 8. Which Approach Is Easier?

For beginners, I recommend:

```tsx
if (loading) {
    return <p>Loading...</p>;
}
```

Because it's easier to read.

Your component flow becomes:

```text
Is loading?
 ↓
Yes → return Loading

No
 ↓
Show actual UI
```

---

# 9. Loading State Is Not Only for APIs

You can use loading states whenever something takes time.

For example:

```text
API request
File upload
Login request
Form submission
Image loading
Payment processing
```

Example login:

```tsx
const [loading, setLoading] = useState(false);
```

When the user clicks Login:

```text
User clicks Login
      ↓
setLoading(true)
      ↓
Send request
      ↓
Wait
      ↓
Request finished
      ↓
setLoading(false)
```

---

# 10. Button Loading Example

Imagine a Save button.

```tsx
function SaveButton() {
    const [loading, setLoading] = useState(false);

    async function handleSave() {
        setLoading(true);

        await new Promise((resolve) =>
            setTimeout(resolve, 2000)
        );

        setLoading(false);
    }

    return (
        <button
            onClick={handleSave}
            disabled={loading}
        >
            {loading ? "Saving..." : "Save"}
        </button>
    );
}
```

Flow:

```text
Initial

[ Save ]


User clicks Save
      ↓

[ Saving... ]


2 seconds later
      ↓

[ Save ]
```

---

# 11. Why `disabled={loading}`?

When the request is running:

```text
loading = true
```

Then:

```tsx
disabled={true}
```

The button becomes disabled.

This prevents:

```text
Click Save
Click Save
Click Save
Click Save
```

and sending multiple requests accidentally.

This is very common in real applications.

---

# 12. Real API State Pattern

In most applications, API requests eventually have three states:

```text
        API Request
             │
             ▼
        Loading
             │
       ┌─────┴─────┐
       ▼           ▼
    Success       Error
       │           │
       ▼           ▼
   Show Data    Show Error
```

So usually we manage:

```tsx
const [data, setData] = useState([]);
const [loading, setLoading] = useState(true);
const [error, setError] = useState(null);
```

Don't worry about `error` yet.

That's our next step.

---

# 13. Full Example So Far

```tsx
import { useEffect, useState } from "react";

type Post = {
    userId: number;
    id: number;
    title: string;
};

function Posts() {
    const [posts, setPosts] = useState<Post[]>([]);
    const [loading, setLoading] = useState(true);

    useEffect(() => {
        async function getPosts() {
            const response = await fetch(
                "https://jsonplaceholder.typicode.com/posts"
            );

            const data = await response.json();

            setPosts(data);
            setLoading(false);
        }

        getPosts();
    }, []);

    if (loading) {
        return <p>Loading...</p>;
    }

    return (
        <div>
            <h1>Posts</h1>

            {posts.map((post) => (
                <article key={post.id}>
                    <h2>{post.title}</h2>
                </article>
            ))}
        </div>
    );
}

export default Posts;
```

---

# 14. One Small Problem Still Exists

What happens if this fails?

```tsx
const response = await fetch(...);
```

Maybe:

```text
No internet
Server is down
Wrong URL
Network problem
API error
```

Our application currently doesn't handle these situations.

That leads to:

# Step 6 — Error Handling

We'll learn:

```text
API Request
    ↓
Loading
    ↓
Success → Show Data

OR

Error → Show Error Message
```

And we'll use:

```tsx
try {
    // API request
} catch (error) {
    // Handle error
} finally {
    // Always runs
}
```

This will complete the fundamental API request pattern.

------------------------------------------------------------------------------------------------------------------------------------------

# Step 6 — Error Handling

In the previous step, we handled:

```text
Loading
   ↓
Show Loading...
```

Now we need to handle another situation:

```text
API Request
   ↓
Something goes wrong
   ↓
Show Error
```

---

# 1. The Complete API Request States

A real API request usually has three main states:

```text
                API Request
                    │
                    ▼
                Loading
                    │
           ┌────────┴────────┐
           ▼                 ▼
        Success             Error
           │                 │
           ▼                 ▼
       Show Data        Show Error
```

So our React component will usually manage:

```tsx
const [data, setData] = useState([]);
const [loading, setLoading] = useState(true);
const [error, setError] = useState(null);
```

Let's understand each one.

| State     | Purpose                               |
| --------- | ------------------------------------- |
| `data`    | Stores API data                       |
| `loading` | Tracks whether the request is running |
| `error`   | Stores an error if something fails    |

---

# 2. What Is `try...catch`?

JavaScript provides:

```tsx
try {
    // Code that might fail
} catch (error) {
    // Handle the error
}
```

Example:

```tsx
try {
    const response = await fetch("some-url");
} catch (error) {
    console.log(error);
}
```

Think:

```text
try
 ↓
Try to run this code

Something fails?
 ↓
catch
 ↓
Handle the error
```

---

# 3. Adding an Error State

Let's update our component.

```tsx
const [posts, setPosts] = useState<Post[]>([]);
const [loading, setLoading] = useState(true);
const [error, setError] = useState<string | null>(null);
```

Initially:

```text
posts = []
loading = true
error = null
```

`null` means:

> There is currently no error.

---

# 4. Basic Error Handling

```tsx
useEffect(() => {
    async function getPosts() {
        try {
            const response = await fetch(
                "https://jsonplaceholder.typicode.com/posts"
            );

            const data = await response.json();

            setPosts(data);
        } catch (error) {
            setError("Failed to fetch posts");
        }

        setLoading(false);
    }

    getPosts();
}, []);
```

The flow:

```text
try
 ↓
Fetch API

Success?
 ↓
Yes → setPosts(data)

No
 ↓
catch
 ↓
setError(...)
```

Then after either situation:

```tsx
setLoading(false);
```

---

# 5. Why Do We Need `finally`?

Instead of writing:

```tsx
try {
    // fetch
} catch (error) {
    // error
}

setLoading(false);
```

We can use:

```tsx
try {
    // fetch
} catch (error) {
    // handle error
} finally {
    // always runs
}
```

`finally` runs whether the request succeeds or fails.

Example:

```tsx
try {
    // API request
} catch (error) {
    // Handle error
} finally {
    setLoading(false);
}
```

Think:

```text
API Request
    │
    ├── Success
    │      ↓
    │
    └── Error
           ↓

        finally
           ↓
    setLoading(false)
```

This is cleaner.

---

# 6. Complete Example

```tsx
import { useEffect, useState } from "react";

type Post = {
    userId: number;
    id: number;
    title: string;
};

function Posts() {
    const [posts, setPosts] = useState<Post[]>([]);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState<string | null>(null);

    useEffect(() => {
        async function getPosts() {
            try {
                const response = await fetch(
                    "https://jsonplaceholder.typicode.com/posts"
                );

                const data = await response.json();

                setPosts(data);
            } catch (error) {
                setError("Failed to fetch posts");
            } finally {
                setLoading(false);
            }
        }

        getPosts();
    }, []);

    if (loading) {
        return <p>Loading...</p>;
    }

    if (error) {
        return <p>{error}</p>;
    }

    return (
        <div>
            <h1>Posts</h1>

            {posts.map((post) => (
                <article key={post.id}>
                    <h2>{post.title}</h2>
                </article>
            ))}
        </div>
    );
}

export default Posts;
```

---

# 7. Understand the Conditional Rendering

We have:

```tsx
if (loading) {
    return <p>Loading...</p>;
}
```

First priority:

```text
Is the request still running?
```

If yes:

```text
Loading...
```

Then:

```tsx
if (error) {
    return <p>{error}</p>;
}
```

If loading is finished, we check:

```text
Did something fail?
```

If yes:

```text
Failed to fetch posts
```

Otherwise:

```text
Show posts
```

The logic is:

```text
loading?
│
├── Yes → Loading...
│
└── No
     │
     error?
     │
     ├── Yes → Show Error
     │
     └── No → Show Data
```

---

# 8. Important: `fetch()` Does Not Throw for Every Error

This is an important real-world concept.

Suppose the server returns:

```text
404 Not Found
```

or:

```text
500 Internal Server Error
```

You might expect this to automatically go to:

```tsx
catch (error) {
}
```

But `fetch()` does not automatically throw an error for HTTP errors like 404 or 500.

So we should check:

```tsx
if (!response.ok) {
    throw new Error("Failed to fetch posts");
}
```

---

# 9. Better Error Handling

```tsx
try {
    const response = await fetch(
        "https://jsonplaceholder.typicode.com/posts"
    );

    if (!response.ok) {
        throw new Error("Failed to fetch posts");
    }

    const data = await response.json();

    setPosts(data);
} catch (error) {
    setError("Failed to fetch posts");
} finally {
    setLoading(false);
}
```

Now:

```text
Network Error
     ↓
catch

404 Error
     ↓
response.ok = false
     ↓
throw Error
     ↓
catch

500 Error
     ↓
response.ok = false
     ↓
throw Error
     ↓
catch
```

This is a much better pattern.

---

# 10. Full Production-Style Basic Pattern

This is the pattern you should remember:

```tsx
useEffect(() => {
    async function getPosts() {
        try {
            const response = await fetch(
                "https://jsonplaceholder.typicode.com/posts"
            );

            if (!response.ok) {
                throw new Error("Failed to fetch posts");
            }

            const data = await response.json();

            setPosts(data);
        } catch (error) {
            setError("Failed to fetch posts");
        } finally {
            setLoading(false);
        }
    }

    getPosts();
}, []);
```

This is the standard basic flow:

```text
try
 │
 ├── fetch()
 │
 ├── Check response.ok
 │
 ├── Convert JSON
 │
 └── Save data
       │
       ▼
catch
 │
 └── Save error
       │
       ▼
finally
 │
 └── Stop loading
```

---

# 11. TypeScript Error Handling

You might wonder:

```tsx
catch (error) {
```

What is the type of `error`?

In TypeScript, errors can be `unknown`.

A safer pattern is:

```tsx
catch (error) {
    if (error instanceof Error) {
        setError(error.message);
    } else {
        setError("Something went wrong");
    }
}
```

Why?

Because TypeScript doesn't guarantee that everything thrown is an `Error` object.

So:

```text
error instanceof Error
        ↓
Is this a real Error object?
```

If yes:

```tsx
error.message
```

Otherwise:

```tsx
"Something went wrong"
```

---

# 12. Full TypeScript Example

```tsx
import { useEffect, useState } from "react";

type Post = {
    userId: number;
    id: number;
    title: string;
};

function Posts() {
    const [posts, setPosts] = useState<Post[]>([]);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState<string | null>(null);

    useEffect(() => {
        async function getPosts() {
            try {
                const response = await fetch(
                    "https://jsonplaceholder.typicode.com/posts"
                );

                if (!response.ok) {
                    throw new Error("Failed to fetch posts");
                }

                const data: Post[] = await response.json();

                setPosts(data);
            } catch (error) {
                if (error instanceof Error) {
                    setError(error.message);
                } else {
                    setError("Something went wrong");
                }
            } finally {
                setLoading(false);
            }
        }

        getPosts();
    }, []);

    if (loading) {
        return <p>Loading...</p>;
    }

    if (error) {
        return <p>{error}</p>;
    }

    return (
        <div>
            <h1>Posts</h1>

            {posts.map((post) => (
                <article key={post.id}>
                    <h2>{post.title}</h2>
                </article>
            ))}
        </div>
    );
}

export default Posts;
```

---

# 13. The Complete API State

Now you understand the fundamental API pattern:

```tsx
const [data, setData] = useState([]);
const [loading, setLoading] = useState(true);
const [error, setError] = useState(null);
```

Visual flow:

```text
                Component Mounts
                      │
                      ▼
               API Request Starts
                      │
                      ▼
                loading = true
                      │
             ┌────────┴────────┐
             ▼                 ▼
          Success             Error
             │                 │
             ▼                 ▼
        setData(data)      setError(...)
             │                 │
             └────────┬────────┘
                      ▼
               loading = false
                      │
             ┌────────┴────────┐
             ▼                 ▼
         Show Data         Show Error
```

This is one of the most important patterns in React API integration.

---

# 14. Small Improvement: Empty State

There is one more possible situation.

The request succeeds, but:

```text
posts = []
```

No data exists.

You can handle that:

```tsx
if (posts.length === 0) {
    return <p>No posts found.</p>;
}
```

Now your UI can handle four states:

```text
1. Loading
2. Error
3. Empty
4. Success
```

This is common in real applications.

The order usually looks like:

```tsx
if (loading) {
    return <p>Loading...</p>;
}

if (error) {
    return <p>{error}</p>;
}

if (posts.length === 0) {
    return <p>No posts found.</p>;
}

return (
    // Show data
);
```

---

# What You Should Remember

```text
API Integration State

data
 ↓
Actual API data

loading
 ↓
Request in progress

error
 ↓
Something failed
```

And the request pattern:

```text
try
 ↓
fetch()
 ↓
response.ok check
 ↓
response.json()
 ↓
setData()

catch
 ↓
setError()

finally
 ↓
setLoading(false)
```

---

# Phase 9 Progress

```text
## 9. API Integration

[x] Step 1 — What is an API?
[x] Step 2 — REST API basics
[x] Step 3 — fetch()
[x] Step 4 — Fetch API data in React
[x] Step 5 — Loading states
[x] Step 6 — Error handling

[ ] Step 7 — POST / PUT / DELETE
[ ] Step 8 — Axios
[ ] Step 9 — API project
[ ] Step 10 — React Query / TanStack Query
```

## Next → Step 7: POST, PUT, and DELETE Requests

So far, we have only learned:

```text
GET → Get data
```

Next, we'll learn how React sends data to APIs:

```text
POST   → Create data
PUT    → Update data
DELETE → Delete data
```

This will complete the basic CRUD API operations.

------------------------------------------------------------------------------------------------------------------------------------------

# Step 7 — POST, PUT, and DELETE Requests

So far, we learned:

```text
GET
 ↓
Get data from API
```

Now we'll learn the other main CRUD operations.

---

# 1. What Is CRUD?

CRUD means:

```text
C → Create
R → Read
U → Update
D → Delete
```

These usually map to HTTP methods like this:

| CRUD   | HTTP Method | Purpose         |
| ------ | ----------- | --------------- |
| Create | POST        | Create new data |
| Read   | GET         | Get data        |
| Update | PUT / PATCH | Update data     |
| Delete | DELETE      | Delete data     |

Visual flow:

```text
Create → POST
Read   → GET
Update → PUT / PATCH
Delete → DELETE
```

---

# 2. POST — Create Data

Imagine we want to create a new post.

API endpoint:

```text
/posts
```

We send:

```text
POST /posts
```

Unlike GET, POST usually sends data to the server.

For example:

```json
{
    "title": "Learning React",
    "userId": 1
}
```

---

# 3. POST Request with `fetch`

Basic syntax:

```tsx
const response = await fetch(
    "https://jsonplaceholder.typicode.com/posts",
    {
        method: "POST",
        headers: {
            "Content-Type": "application/json",
        },
        body: JSON.stringify({
            title: "Learning React",
            userId: 1,
        }),
    }
);
```

Let's understand each part.

---

## `method: "POST"`

```tsx
method: "POST"
```

Tells the API:

> I want to create new data.

---

## `headers`

```tsx
headers: {
    "Content-Type": "application/json",
}
```

This tells the server:

> The data I am sending is JSON.

---

## `body`

```tsx
body: JSON.stringify({
    title: "Learning React",
    userId: 1,
})
```

We have a JavaScript object:

```tsx
{
    title: "Learning React",
    userId: 1
}
```

But HTTP requests send data as text.

So:

```text
JavaScript Object
       ↓
JSON.stringify()
       ↓
JSON String
       ↓
Send to API
```

---

# 4. POST Complete Example

```tsx
async function createPost() {
    try {
        const response = await fetch(
            "https://jsonplaceholder.typicode.com/posts",
            {
                method: "POST",

                headers: {
                    "Content-Type": "application/json",
                },

                body: JSON.stringify({
                    title: "Learning React",
                    userId: 1,
                }),
            }
        );

        if (!response.ok) {
            throw new Error("Failed to create post");
        }

        const data = await response.json();

        console.log(data);
    } catch (error) {
        console.error(error);
    }
}
```

Flow:

```text
React
 ↓
POST request
 ↓
Send data
 ↓
Server
 ↓
Create data
 ↓
Return created data
```

---

# 5. POST in a React Component

Now let's make it practical.

```tsx
import { useState } from "react";

function CreatePost() {
    const [title, setTitle] = useState("");

    async function handleSubmit(
        event: React.FormEvent<HTMLFormElement>
    ) {
        event.preventDefault();

        try {
            const response = await fetch(
                "https://jsonplaceholder.typicode.com/posts",
                {
                    method: "POST",

                    headers: {
                        "Content-Type": "application/json",
                    },

                    body: JSON.stringify({
                        title: title,
                        userId: 1,
                    }),
                }
            );

            if (!response.ok) {
                throw new Error("Failed to create post");
            }

            const data = await response.json();

            console.log(data);
        } catch (error) {
            console.error(error);
        }
    }

    return (
        <form onSubmit={handleSubmit}>
            <input
                type="text"
                value={title}
                onChange={(event) =>
                    setTitle(event.target.value)
                }
                placeholder="Enter post title"
            />

            <button type="submit">
                Create Post
            </button>
        </form>
    );
}

export default CreatePost;
```

---

# 6. Complete POST Flow

```text
User enters title
        ↓
React State updates
        ↓
User clicks Create Post
        ↓
Form submit
        ↓
handleSubmit()
        ↓
preventDefault()
        ↓
fetch()
        ↓
POST request
        ↓
JSON.stringify(data)
        ↓
Server receives data
        ↓
Server responds
```

---

# 7. PUT — Update Data

Now suppose we already have:

```text
Post ID: 1

Title: Old Title
```

We want:

```text
Post ID: 1

Title: New Title
```

We use:

```text
PUT /posts/1
```

---

# 8. PUT Request

```tsx
async function updatePost() {
    try {
        const response = await fetch(
            "https://jsonplaceholder.typicode.com/posts/1",
            {
                method: "PUT",

                headers: {
                    "Content-Type": "application/json",
                },

                body: JSON.stringify({
                    id: 1,
                    title: "Updated React Post",
                    userId: 1,
                }),
            }
        );

        if (!response.ok) {
            throw new Error("Failed to update post");
        }

        const data = await response.json();

        console.log(data);
    } catch (error) {
        console.error(error);
    }
}
```

Flow:

```text
Existing Data
     ↓
PUT request
     ↓
Updated Data
     ↓
Server
```

---

# 9. PUT vs POST

This is important.

```text
POST
 ↓
Create new resource

PUT
 ↓
Update existing resource
```

Examples:

```text
POST /posts
```

Creates:

```text
New Post
```

While:

```text
PUT /posts/1
```

Updates:

```text
Existing Post with ID 1
```

---

# 10. PATCH vs PUT

You may also see:

```text
PATCH
```

The general difference:

```text
PUT
 ↓
Replace/update the entire resource

PATCH
 ↓
Update only specific fields
```

Example existing user:

```json
{
    "name": "John",
    "email": "john@email.com",
    "age": 25
}
```

With PATCH:

```json
{
    "age": 26
}
```

Only the age is updated.

For now, remember:

```text
PUT / PATCH
 ↓
Update data
```

---

# 11. DELETE — Remove Data

Suppose we want to delete:

```text
Post ID: 1
```

Request:

```text
DELETE /posts/1
```

Code:

```tsx
async function deletePost() {
    try {
        const response = await fetch(
            "https://jsonplaceholder.typicode.com/posts/1",
            {
                method: "DELETE",
            }
        );

        if (!response.ok) {
            throw new Error("Failed to delete post");
        }

        console.log("Post deleted");
    } catch (error) {
        console.error(error);
    }
}
```

Notice:

```text
DELETE
 ↓
Usually doesn't need a request body
```

The ID in the URL tells the server what to delete.

---

# 12. CRUD with URLs

This is a useful pattern to remember:

```text
GET     /posts       → Get all posts

GET     /posts/1     → Get one post

POST    /posts       → Create post

PUT     /posts/1     → Update post

PATCH   /posts/1     → Partially update post

DELETE  /posts/1     → Delete post
```

This is typical REST API design.

---

# 13. Full CRUD Mental Model

Imagine a Todo application.

### Create Todo

```text
POST /todos
```

### Get Todos

```text
GET /todos
```

### Update Todo

```text
PUT /todos/1
```

### Delete Todo

```text
DELETE /todos/1
```

So:

```text
                 TODO API

GET     → Read todos
POST    → Create todo
PUT     → Update todo
DELETE  → Delete todo
```

---

# 14. Important Concept — Updating React State

This is where frontend development becomes interesting.

Suppose you create a new post successfully.

The server responds:

```json
{
    "id": 101,
    "title": "Learning React",
    "userId": 1
}
```

You don't always want to fetch all posts again.

You can update React state directly.

Example:

```tsx
const [posts, setPosts] = useState<Post[]>([]);
```

After POST:

```tsx
const newPost = await response.json();

setPosts((currentPosts) => [
    ...currentPosts,
    newPost,
]);
```

Think:

```text
Current posts
     ↓
[Post 1, Post 2]

New Post
     ↓
Post 3

New state
     ↓
[Post 1, Post 2, Post 3]
```

---

# 15. Updating State After DELETE

Suppose:

```text
Posts:

1
2
3
```

User deletes Post 2.

After the API request succeeds:

```tsx
setPosts((currentPosts) =>
    currentPosts.filter(
        (post) => post.id !== id
    )
);
```

Result:

```text
Before:

[1, 2, 3]

Delete 2

After:

[1, 3]
```

Important:

```text
API request
     ↓
Success
     ↓
Update React state
     ↓
React UI updates
```

---

# 16. Updating State After PUT

Suppose we update Post 1.

The API returns:

```tsx
const updatedPost = await response.json();
```

Then:

```tsx
setPosts((currentPosts) =>
    currentPosts.map((post) =>
        post.id === updatedPost.id
            ? updatedPost
            : post
    )
);
```

Think:

```text
Current posts:

[Post 1, Post 2, Post 3]

Updated Post 1 arrives
       ↓

Replace Post 1

New posts:

[Updated Post 1, Post 2, Post 3]
```

---

# 17. The Most Important Real-World Pattern

Whenever you modify data:

```text
POST
PUT
PATCH
DELETE
```

the usual flow is:

```text
User Action
    ↓
API Request
    ↓
Server Success?
    ↓
Yes
    ↓
Update React State
    ↓
UI Automatically Updates
```

For example:

### POST

```text
API Success
 ↓
Add item to state
```

### DELETE

```text
API Success
 ↓
Remove item from state
```

### UPDATE

```text
API Success
 ↓
Replace item in state
```

---

# 18. CRUD State Examples Together

## Create

```tsx
setPosts((currentPosts) => [
    ...currentPosts,
    newPost,
]);
```

## Update

```tsx
setPosts((currentPosts) =>
    currentPosts.map((post) =>
        post.id === updatedPost.id
            ? updatedPost
            : post
    )
);
```

## Delete

```tsx
setPosts((currentPosts) =>
    currentPosts.filter(
        (post) => post.id !== deletedId
    )
);
```

These three patterns are extremely common in React applications.

---

# 19. Complete CRUD Architecture

```text
                React UI
                    │
                    ▼
              User Action
                    │
        ┌───────────┼───────────┐
        ▼           ▼           ▼
      Create      Update      Delete
        │           │           │
       POST        PUT       DELETE
        │           │           │
        └───────────┼───────────┘
                    ▼
                API Server
                    │
                    ▼
              Success Response
                    │
                    ▼
             Update React State
                    │
                    ▼
                Re-render UI
```

---

# 20. What You Need to Remember

The main HTTP methods:

```text
GET
→ Get data

POST
→ Create data

PUT / PATCH
→ Update data

DELETE
→ Delete data
```

And the `fetch()` structure for sending JSON:

```tsx
fetch(url, {
    method: "POST",

    headers: {
        "Content-Type": "application/json",
    },

    body: JSON.stringify(data),
});
```

Then the React pattern:

```text
API Success
      ↓
Update State
      ↓
React Re-renders
      ↓
UI Updates
```

---

# Phase 9 Progress

```text
## 9. API Integration

[x] Step 1 — What is an API?
[x] Step 2 — REST API basics
[x] Step 3 — fetch()
[x] Step 4 — Fetch API data in React
[x] Step 5 — Loading states
[x] Step 6 — Error handling
[x] Step 7 — POST / PUT / DELETE

[ ] Step 8 — Axios
[ ] Step 9 — API Project
[ ] Step 10 — React Query / TanStack Query
```

# Next → Step 8: Axios

We'll learn why developers sometimes use Axios instead of `fetch()` and compare:

```text
fetch()
vs
Axios
```

Then we'll use Axios inside a React application.

------------------------------------------------------------------------------------------------------------------------------------------


# Step 8 — Axios

So far, we have used the browser's built-in:

```text
fetch()
```

Now let's learn:

```text
Axios
```

Axios is another way to make HTTP requests.

---

# 1. What Is Axios?

Axios is a popular JavaScript library used for making HTTP requests.

We already know:

```text
React
  ↓
fetch()
  ↓
API
```

With Axios:

```text
React
  ↓
Axios
  ↓
API
```

Both do the same main job:

```text
GET data
POST data
PUT data
DELETE data
```

---

# 2. Fetch vs Axios

The biggest difference:

| Fetch                                        | Axios                        |
| -------------------------------------------- | ---------------------------- |
| Built into the browser                       | External library             |
| No installation needed                       | Must install                 |
| Uses `response.json()`                       | Automatically parses JSON    |
| Doesn't reject automatically for HTTP errors | Rejects HTTP error responses |
| Native browser API                           | Third-party library          |

---

# 3. Installing Axios

In a React project, Axios needs to be installed:

```text
npm install axios
```

Then import it:

```tsx
import axios from "axios";
```

Now you can use Axios.

---

# 4. GET Request with Fetch

First, remember our Fetch version:

```tsx
const response = await fetch(
    "https://jsonplaceholder.typicode.com/posts"
);

if (!response.ok) {
    throw new Error("Failed to fetch posts");
}

const data = await response.json();
```

Notice:

```text
fetch()
   ↓
response
   ↓
Check response.ok
   ↓
response.json()
   ↓
data
```

---

# 5. GET Request with Axios

With Axios:

```tsx
const response = await axios.get(
    "https://jsonplaceholder.typicode.com/posts"
);

const data = response.data;
```

That's it.

Axios automatically handles JSON conversion.

Flow:

```text
axios.get()
    ↓
response
    ↓
response.data
    ↓
Actual data
```

---

# 6. Complete Axios GET Example

```tsx
import { useEffect, useState } from "react";
import axios from "axios";

type Post = {
    userId: number;
    id: number;
    title: string;
};

function Posts() {
    const [posts, setPosts] = useState<Post[]>([]);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState<string | null>(null);

    useEffect(() => {
        async function getPosts() {
            try {
                const response = await axios.get<Post[]>(
                    "https://jsonplaceholder.typicode.com/posts"
                );

                setPosts(response.data);
            } catch (error) {
                if (error instanceof Error) {
                    setError(error.message);
                } else {
                    setError("Something went wrong");
                }
            } finally {
                setLoading(false);
            }
        }

        getPosts();
    }, []);

    if (loading) {
        return <p>Loading...</p>;
    }

    if (error) {
        return <p>{error}</p>;
    }

    return (
        <div>
            <h1>Posts</h1>

            {posts.map((post) => (
                <article key={post.id}>
                    <h2>{post.title}</h2>
                </article>
            ))}
        </div>
    );
}

export default Posts;
```

---

# 7. Understand This TypeScript Syntax

You may notice:

```tsx
axios.get<Post[]>(url)
```

The `<Post[]>` tells TypeScript:

> I expect this API to return an array of `Post` objects.

So TypeScript understands:

```tsx
response.data
```

is:

```text
Post[]
```

Instead of:

```text
unknown
```

or poorly typed data.

---

# 8. POST Request with Axios

With Fetch, we wrote:

```tsx
fetch(url, {
    method: "POST",
    headers: {
        "Content-Type": "application/json",
    },
    body: JSON.stringify(data),
});
```

With Axios:

```tsx
axios.post(url, data);
```

Much simpler.

Example:

```tsx
async function createPost() {
    try {
        const response = await axios.post(
            "https://jsonplaceholder.typicode.com/posts",
            {
                title: "Learning Axios",
                userId: 1,
            }
        );

        console.log(response.data);
    } catch (error) {
        console.error(error);
    }
}
```

Axios automatically:

```text
JavaScript Object
       ↓
Convert to JSON
       ↓
Set Content-Type
       ↓
Send Request
```

---

# 9. PUT Request with Axios

```tsx
async function updatePost() {
    try {
        const response = await axios.put(
            "https://jsonplaceholder.typicode.com/posts/1",
            {
                id: 1,
                title: "Updated Post",
                userId: 1,
            }
        );

        console.log(response.data);
    } catch (error) {
        console.error(error);
    }
}
```

Pattern:

```text
axios.put(URL, data)
```

---

# 10. PATCH Request with Axios

```tsx
const response = await axios.patch(
    "https://jsonplaceholder.typicode.com/posts/1",
    {
        title: "Updated Title",
    }
);
```

Pattern:

```text
axios.patch(URL, data)
```

---

# 11. DELETE Request with Axios

```tsx
async function deletePost() {
    try {
        await axios.delete(
            "https://jsonplaceholder.typicode.com/posts/1"
        );

        console.log("Post deleted");
    } catch (error) {
        console.error(error);
    }
}
```

Pattern:

```text
axios.delete(URL)
```

---

# 12. Axios CRUD Summary

```text
GET

axios.get("/posts")


POST

axios.post("/posts", data)


PUT

axios.put("/posts/1", data)


PATCH

axios.patch("/posts/1", data)


DELETE

axios.delete("/posts/1")
```

This is very easy to remember.

---

# 13. Fetch vs Axios — Side by Side

## GET

### Fetch

```tsx
const response = await fetch(url);

if (!response.ok) {
    throw new Error("Request failed");
}

const data = await response.json();
```

### Axios

```tsx
const response = await axios.get(url);

const data = response.data;
```

---

## POST

### Fetch

```tsx
await fetch(url, {
    method: "POST",

    headers: {
        "Content-Type": "application/json",
    },

    body: JSON.stringify(data),
});
```

### Axios

```tsx
await axios.post(url, data);
```

---

# 14. Why Do Developers Use Axios?

Axios provides some convenient features:

```text
Automatic JSON conversion
Better error handling
Interceptors
Request configuration
Base URLs
Timeout configuration
```

The most important advanced feature is:

# Axios Instance

---

# 15. The Problem with Repeating URLs

Imagine your API URL is:

```text
http://localhost:5000/api
```

Without an Axios instance:

```tsx
axios.get("http://localhost:5000/api/posts");

axios.get("http://localhost:5000/api/users");

axios.post("http://localhost:5000/api/posts", data);
```

We are repeating:

```text
http://localhost:5000/api
```

again and again.

---

# 16. Axios Instance

We can create a configured Axios instance.

Example:

```tsx
import axios from "axios";

const api = axios.create({
    baseURL: "http://localhost:5000/api",
});

export default api;
```

Usually, this might be inside:

```text
src/
 └── api/
      └── api.ts
```

Now we can write:

```tsx
import api from "./api/api";

const response = await api.get("/posts");
```

Instead of:

```text
http://localhost:5000/api/posts
```

Axios automatically combines:

```text
baseURL
   +
endpoint
```

Example:

```text
http://localhost:5000/api
          +
       /posts
          ↓
http://localhost:5000/api/posts
```

---

# 17. Why Axios Instances Are Useful

Suppose later your API changes from:

```text
http://localhost:5000/api
```

to:

```text
https://api.mywebsite.com/api
```

Without an Axios instance, you might need to change many files.

With an instance:

```tsx
const api = axios.create({
    baseURL: "https://api.mywebsite.com/api",
});
```

You change it in one place.

---

# 18. Axios Interceptors (Basic Introduction)

Axios can intercept requests and responses.

For example:

```text
Request
  ↓
Interceptor
  ↓
API
```

You can automatically add things like:

```text
Authentication token
Headers
Logging
```

Example concept:

```tsx
api.interceptors.request.use((config) => {
    const token = localStorage.getItem("token");

    if (token) {
        config.headers.Authorization = `Bearer ${token}`;
    }

    return config;
});
```

Then every API request automatically gets:

```text
Authorization: Bearer TOKEN
```

You don't need to manually add the token every time.

We'll revisit this when we learn authentication and backend integration.

For now, just understand the concept.

---

# 19. Fetch or Axios — Which Should You Use?

My recommendation for you:

### Learn both.

You already know Fetch.

Now you understand Axios.

In real projects:

```text
Small/simple project
→ Fetch is perfectly fine

Large application
→ Axios can be convenient

Modern applications
→ Both are commonly used
```

There is no rule saying one is always better.

---

# 20. The Important Thing You Should Remember

Don't focus too much on:

```text
Fetch vs Axios
```

The important thing is understanding the API flow:

```text
React
   ↓
API Request
   ↓
Loading
   ↓
Success / Error
   ↓
Update State
   ↓
UI Updates
```

Whether you use:

```text
fetch()
```

or:

```text
axios
```

the architecture is basically the same.

---

# Quick Summary

```text
FETCH

Built into browser

fetch(url)

response.json()
```

```text
AXIOS

External library

axios.get(url)

response.data
```

CRUD:

```text
GET
axios.get()

POST
axios.post()

PUT
axios.put()

PATCH
axios.patch()

DELETE
axios.delete()
```

---

# Phase 9 Progress

```text
## 9. API Integration

[x] Step 1 — What is an API?
[x] Step 2 — REST API basics
[x] Step 3 — fetch()
[x] Step 4 — Fetch API data in React
[x] Step 5 — Loading states
[x] Step 6 — Error handling
[x] Step 7 — POST / PUT / DELETE
[x] Step 8 — Axios

[ ] Step 9 — API Project
[ ] Step 10 — React Query / TanStack Query
```

# Next Step — Build an API Project

Before moving to React Query, we should build a small project using everything we learned:

```text
Fetch API
Loading state
Error handling
Search
Filter
TypeScript
React state
```

This matches your roadmap:

```text
Build: fetch API data app, search/filter UI
```

Then after completing that project, we'll move to:

# React Query / TanStack Query


------------------------------------------------------------------------------------------------------------------------------------------


# Step 9 — Build an API Project: Posts Search & Filter App

Now we are going to build the project mentioned in your roadmap:

```text
Build: fetch API data app, search/filter UI
```

We will combine everything we learned in API Integration.

---

# What We Are Building

A React + TypeScript application that:

```text
API
 ↓
Fetch Posts
 ↓
Show Loading
 ↓
Handle Errors
 ↓
Display Posts
 ↓
Search Posts
 ↓
Filter Posts by User
```

---

# Features

Our application will have:

```text
✓ Fetch API data
✓ TypeScript types
✓ Loading state
✓ Error handling
✓ Search functionality
✓ Filter functionality
✓ Conditional rendering
✓ Array methods
✓ useState
✓ useEffect
```

---

# Project Structure

For now, keep it simple:

```text
src/
 ├── App.tsx
 └── main.tsx
```

We will put everything inside `App.tsx` first.

Later, we can discuss how to split it into components.

---

# Step 1 — Define the Post Type

Inside `App.tsx`:

```tsx
type Post = {
    userId: number;
    id: number;
    title: string;
    body: string;
};
```

This represents one post from the API.

Example:

```text
{
    userId: 1,
    id: 1,
    title: "...",
    body: "..."
}
```

---

# Step 2 — Create Our States

```tsx
const [posts, setPosts] = useState<Post[]>([]);
const [loading, setLoading] = useState(true);
const [error, setError] = useState<string | null>(null);

const [search, setSearch] = useState("");
const [selectedUser, setSelectedUser] = useState("all");
```

We have five states.

| State          | Purpose                     |
| -------------- | --------------------------- |
| `posts`        | Stores API posts            |
| `loading`      | Tracks API loading          |
| `error`        | Stores error message        |
| `search`       | Stores search input         |
| `selectedUser` | Stores selected user filter |

---

# Step 3 — Fetch API Data

We use `useEffect`.

```tsx
useEffect(() => {
    async function getPosts() {
        try {
            const response = await fetch(
                "https://jsonplaceholder.typicode.com/posts"
            );

            if (!response.ok) {
                throw new Error("Failed to fetch posts");
            }

            const data: Post[] = await response.json();

            setPosts(data);
        } catch (error) {
            if (error instanceof Error) {
                setError(error.message);
            } else {
                setError("Something went wrong");
            }
        } finally {
            setLoading(false);
        }
    }

    getPosts();
}, []);
```

Flow:

```text
Component loads
      ↓
useEffect runs
      ↓
Fetch API
      ↓
Success → Save posts

OR

Error → Save error
      ↓
Finally → Stop loading
```

---

# Step 4 — Search Functionality

We have:

```tsx
const [search, setSearch] = useState("");
```

Input:

```tsx
<input
    type="text"
    value={search}
    onChange={(event) => setSearch(event.target.value)}
    placeholder="Search posts..."
/>
```

Every time the user types:

```text
"r"
 ↓
"re"
 ↓
"rea"
 ↓
"react"
```

React updates:

```text
search
```

Now we filter posts.

```tsx
const searchedPosts = posts.filter((post) =>
    post.title
        .toLowerCase()
        .includes(search.toLowerCase())
);
```

Example:

```text
Search = "react"

Posts:

Learning React      ✓
JavaScript Basics   ✗
React Hooks         ✓
```

---

# Step 5 — Filter by User

We want a dropdown:

```text
All Users
User 1
User 2
User 3
...
```

State:

```tsx
const [selectedUser, setSelectedUser] = useState("all");
```

Dropdown:

```tsx
<select
    value={selectedUser}
    onChange={(event) =>
        setSelectedUser(event.target.value)
    }
>
    <option value="all">All Users</option>

    <option value="1">User 1</option>
    <option value="2">User 2</option>
    <option value="3">User 3</option>
</select>
```

Now filter:

```tsx
const filteredPosts = searchedPosts.filter((post) => {
    if (selectedUser === "all") {
        return true;
    }

    return post.userId === Number(selectedUser);
});
```

Important:

```text
select value
   ↓
Always string

"1"
```

But:

```text
post.userId
   ↓
number

1
```

So we use:

```tsx
Number(selectedUser)
```

---

# Step 6 — Combining Search + Filter

This is important.

We first search:

```tsx
const searchedPosts = posts.filter((post) =>
    post.title
        .toLowerCase()
        .includes(search.toLowerCase())
);
```

Then filter by user:

```tsx
const filteredPosts = searchedPosts.filter((post) => {
    if (selectedUser === "all") {
        return true;
    }

    return post.userId === Number(selectedUser);
});
```

Flow:

```text
All Posts
    ↓
Search
    ↓
Searched Posts
    ↓
User Filter
    ↓
Final Posts
```

---

# Full Code — `App.tsx`

```tsx
import { useEffect, useState } from "react";

type Post = {
    userId: number;
    id: number;
    title: string;
    body: string;
};

function App() {
    const [posts, setPosts] = useState<Post[]>([]);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState<string | null>(null);

    const [search, setSearch] = useState("");
    const [selectedUser, setSelectedUser] = useState("all");

    useEffect(() => {
        async function getPosts() {
            try {
                const response = await fetch(
                    "https://jsonplaceholder.typicode.com/posts"
                );

                if (!response.ok) {
                    throw new Error("Failed to fetch posts");
                }

                const data: Post[] = await response.json();

                setPosts(data);
            } catch (error) {
                if (error instanceof Error) {
                    setError(error.message);
                } else {
                    setError("Something went wrong");
                }
            } finally {
                setLoading(false);
            }
        }

        getPosts();
    }, []);

    const searchedPosts = posts.filter((post) =>
        post.title
            .toLowerCase()
            .includes(search.toLowerCase())
    );

    const filteredPosts = searchedPosts.filter((post) => {
        if (selectedUser === "all") {
            return true;
        }

        return post.userId === Number(selectedUser);
    });

    if (loading) {
        return <p>Loading...</p>;
    }

    if (error) {
        return <p>{error}</p>;
    }

    return (
        <div>
            <h1>Posts</h1>

            <input
                type="text"
                value={search}
                onChange={(event) =>
                    setSearch(event.target.value)
                }
                placeholder="Search posts..."
            />

            <select
                value={selectedUser}
                onChange={(event) =>
                    setSelectedUser(event.target.value)
                }
            >
                <option value="all">All Users</option>

                <option value="1">User 1</option>
                <option value="2">User 2</option>
                <option value="3">User 3</option>
                <option value="4">User 4</option>
                <option value="5">User 5</option>
                <option value="6">User 6</option>
                <option value="7">User 7</option>
                <option value="8">User 8</option>
                <option value="9">User 9</option>
                <option value="10">User 10</option>
            </select>

            <p>
                Showing {filteredPosts.length} posts
            </p>

            {filteredPosts.length === 0 ? (
                <p>No posts found.</p>
            ) : (
                filteredPosts.map((post) => (
                    <article key={post.id}>
                        <h2>{post.title}</h2>
                        <p>{post.body}</p>
                        <small>User ID: {post.userId}</small>
                    </article>
                ))
            )}
        </div>
    );
}

export default App;
```

---

# Complete Application Flow

```text
                Component Mounts
                       ↓
                 loading = true
                       ↓
                Show Loading...
                       ↓
                  Fetch API
                       ↓
              ┌────────┴────────┐
              ↓                 ↓
           Success             Error
              ↓                 ↓
         Save Posts         Save Error
              ↓                 ↓
              └────────┬────────┘
                       ↓
                loading = false
                       ↓
                   Show UI
                       ↓
              User Can Search
                       ↓
              User Can Filter
                       ↓
               Display Results
```

---

# Important Part — Derived Data

Notice we did NOT create state for:

```tsx
const [filteredPosts, setFilteredPosts] = useState([]);
```

Instead:

```tsx
const filteredPosts = ...
```

Why?

Because `filteredPosts` can be calculated from:

```text
posts
+
search
+
selectedUser
```

This is called **derived data**.

```text
posts
search
selectedUser
       ↓
Calculate
       ↓
filteredPosts
```

This is a very important React concept.

Don't store something in state if you can calculate it from existing state.

---

# Project Concepts You Just Used

```text
React:
✓ Components
✓ useState
✓ useEffect
✓ Conditional rendering
✓ Controlled inputs
✓ Lists and keys

TypeScript:
✓ Type definitions
✓ Array types
✓ Union types
✓ Type annotations

JavaScript:
✓ async/await
✓ try/catch/finally
✓ filter()
✓ includes()
✓ toLowerCase()
✓ Number()

API:
✓ GET request
✓ response.ok
✓ JSON parsing
✓ Loading state
✓ Error state
```

---

# Phase 9 Progress

```text
## 9. API Integration

[x] Step 1 — What is an API?
[x] Step 2 — REST API basics
[x] Step 3 — fetch()
[x] Step 4 — Fetch API data in React
[x] Step 5 — Loading states
[x] Step 6 — Error handling
[x] Step 7 — POST / PUT / DELETE
[x] Step 8 — Axios
[x] Step 9 — API Project

[ ] Step 10 — React Query / TanStack Query
```

# Next → Step 10: React Query / TanStack Query

Now comes an important question:

We currently fetch data manually using:

```tsx
useEffect()
+
useState()
+
fetch()
```

But managing:

```text
loading
error
data
refetching
caching
```

manually becomes difficult in large applications.

TanStack Query solves this problem.

Next we'll learn:

```text
Why React Query exists
↓
What problem it solves
↓
How it works compared to useEffect + fetch
```


------------------------------------------------------------------------------------------------------------------------------------------


# Step 10 — React Query / TanStack Query

We have completed the basic API project. Now let's understand **why TanStack Query exists** before learning its code.

---

# 1. The Problem With Manual API Fetching

Until now, we used this pattern:

```tsx
const [posts, setPosts] = useState<Post[]>([]);
const [loading, setLoading] = useState(true);
const [error, setError] = useState<string | null>(null);

useEffect(() => {
    async function getPosts() {
        try {
            const response = await fetch(
                "https://jsonplaceholder.typicode.com/posts"
            );

            if (!response.ok) {
                throw new Error("Failed to fetch posts");
            }

            const data = await response.json();

            setPosts(data);
        } catch (error) {
            setError("Something went wrong");
        } finally {
            setLoading(false);
        }
    }

    getPosts();
}, []);
```

Look at everything we manually manage:

```text
API Data      → posts
Loading       → loading
Error         → error
Fetching      → useEffect
```

For one API request, this is okay.

But imagine a large application.

---

# 2. Imagine a Real Application

Suppose you have:

```text
Dashboard
│
├── Users API
├── Products API
├── Orders API
├── Notifications API
└── Profile API
```

For every API request, you may write:

```text
useState(data)
useState(loading)
useState(error)
useEffect(fetch)
try/catch/finally
```

Again and again.

```text
Component 1
├── data
├── loading
├── error
└── useEffect

Component 2
├── data
├── loading
├── error
└── useEffect

Component 3
├── data
├── loading
├── error
└── useEffect
```

This becomes repetitive.

---

# 3. Another Big Problem — Caching

Imagine this:

```text
User opens Posts page
        ↓
Fetch posts API
        ↓
Receive 100 posts
```

Then the user goes to another page:

```text
Posts → Profile
```

Then comes back:

```text
Profile → Posts
```

Without caching:

```text
Fetch Posts API AGAIN
```

Even though we already have the data.

TanStack Query can cache API data.

```text
First Visit
    ↓
Fetch API
    ↓
Store in Cache

Second Visit
    ↓
Check Cache
    ↓
Use Existing Data
```

---

# 4. What Is TanStack Query?

TanStack Query is a library for managing **server state**.

You may also hear the older name:

```text
React Query
```

The official modern name is:

```text
TanStack Query
```

Think of it like this:

```text
React
  ↓
TanStack Query
  ↓
API
```

It helps manage:

```text
✓ API data
✓ Loading states
✓ Error states
✓ Caching
✓ Refetching
✓ Background updates
✓ Mutations (POST/PUT/DELETE)
```

---

# 5. Important Concept — Server State vs Client State

This is extremely important.

We already know React state:

```tsx
const [count, setCount] = useState(0);
```

This is usually called:

# Client State

Examples:

```text
Dark mode toggle
Modal open/close
Input value
Selected tab
Sidebar open/close
```

This state belongs mainly to your frontend.

---

## Server State

Data coming from an API:

```text
Users
Posts
Products
Orders
Profile data
```

Example:

```text
API
 ↓
Posts
 ↓
React Application
```

This is called server state because the source of truth usually exists on the server/database.

---

# Visual Difference

```text
CLIENT STATE

React Application
     ↓
useState

Example:
isModalOpen
theme
searchInput
```

```text
SERVER STATE

Server / API
     ↓
TanStack Query
     ↓
React Application

Example:
users
posts
products
```

---

# 6. The Main Idea of TanStack Query

Instead of manually writing:

```tsx
useState
+
useEffect
+
fetch
+
loading
+
error
```

TanStack Query provides:

```tsx
useQuery()
```

Think:

```text
useQuery()
    ↓
Fetch API
    ↓
Manage Data
    ↓
Manage Loading
    ↓
Manage Error
    ↓
Cache Data
```

---

# 7. Installing TanStack Query

First install:

```text
npm install @tanstack/react-query
```

---

# 8. Setup QueryClient

TanStack Query needs something called:

```text
QueryClient
```

Think of `QueryClient` as the manager of all API queries and cached data.

Basic architecture:

```text
QueryClient
     ↓
Manages
     ↓
Queries + Cache
```

In `main.tsx`:

```tsx
import React from "react";
import ReactDOM from "react-dom/client";
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";
import App from "./App";

const queryClient = new QueryClient();

ReactDOM.createRoot(
    document.getElementById("root")!
).render(
    <React.StrictMode>
        <QueryClientProvider client={queryClient}>
            <App />
        </QueryClientProvider>
    </React.StrictMode>
);
```

---

# 9. Understand `QueryClientProvider`

We create:

```tsx
const queryClient = new QueryClient();
```

Then:

```tsx
<QueryClientProvider client={queryClient}>
    <App />
</QueryClientProvider>
```

Think of it similarly to a provider:

```text
QueryClientProvider
       │
       ├── App
       │
       ├── Components
       │
       └── useQuery()
```

Any component inside the provider can use TanStack Query.

---

# 10. Your First `useQuery`

Let's fetch posts.

First create a function:

```tsx
async function getPosts(): Promise<Post[]> {
    const response = await fetch(
        "https://jsonplaceholder.typicode.com/posts"
    );

    if (!response.ok) {
        throw new Error("Failed to fetch posts");
    }

    return response.json();
}
```

Then use:

```tsx
const {
    data,
    isLoading,
    error,
} = useQuery({
    queryKey: ["posts"],
    queryFn: getPosts,
});
```

That's the main idea.

---

# 11. Compare Old vs New

## Old Method

```tsx
const [posts, setPosts] = useState<Post[]>([]);
const [loading, setLoading] = useState(true);
const [error, setError] = useState<string | null>(null);

useEffect(() => {
    async function getPosts() {
        try {
            const response = await fetch(url);

            const data = await response.json();

            setPosts(data);
        } catch (error) {
            setError("Failed");
        } finally {
            setLoading(false);
        }
    }

    getPosts();
}, []);
```

---

## TanStack Query

```tsx
const {
    data,
    isLoading,
    error,
} = useQuery({
    queryKey: ["posts"],
    queryFn: getPosts,
});
```

TanStack Query automatically manages:

```text
data
loading
error
cache
refetching
```

---

# 12. Understanding `queryKey`

This:

```tsx
queryKey: ["posts"]
```

is very important.

It gives a unique name to the API data.

Think:

```text
["posts"]
    ↓
Identity of this API data
```

For users:

```tsx
queryKey: ["users"]
```

For products:

```tsx
queryKey: ["products"]
```

TanStack Query uses this key for:

```text
Caching
Refetching
Identifying data
Updating data
```

---

# 13. Understanding `queryFn`

This:

```tsx
queryFn: getPosts
```

means:

> Use this function to fetch the data.

Our function:

```tsx
async function getPosts(): Promise<Post[]> {
    const response = await fetch(
        "https://jsonplaceholder.typicode.com/posts"
    );

    if (!response.ok) {
        throw new Error("Failed to fetch posts");
    }

    return response.json();
}
```

So:

```text
useQuery
   ↓
queryFn
   ↓
getPosts()
   ↓
fetch API
   ↓
Return data
```

---

# 14. Complete Example

## `App.tsx`

```tsx
import { useQuery } from "@tanstack/react-query";

type Post = {
    userId: number;
    id: number;
    title: string;
    body: string;
};

async function getPosts(): Promise<Post[]> {
    const response = await fetch(
        "https://jsonplaceholder.typicode.com/posts"
    );

    if (!response.ok) {
        throw new Error("Failed to fetch posts");
    }

    return response.json();
}

function App() {
    const {
        data: posts,
        isLoading,
        error,
    } = useQuery({
        queryKey: ["posts"],
        queryFn: getPosts,
    });

    if (isLoading) {
        return <p>Loading...</p>;
    }

    if (error) {
        return <p>{error.message}</p>;
    }

    return (
        <div>
            <h1>Posts</h1>

            {posts.map((post) => (
                <article key={post.id}>
                    <h2>{post.title}</h2>
                    <p>{post.body}</p>
                </article>
            ))}
        </div>
    );
}

export default App;
```

---

# 15. Data Flow

```text
Component
   ↓
useQuery()
   ↓
Check Cache
   │
   ├── Data exists?
   │       ↓
   │    Return cached data
   │
   └── No data?
           ↓
        queryFn()
           ↓
        Fetch API
           ↓
        Save in Cache
           ↓
        Return data
```

This caching behavior is one of the major reasons TanStack Query is useful.

---

# 16. Why `data: posts`?

You may notice:

```tsx
const {
    data: posts,
    isLoading,
    error,
} = useQuery(...);
```

This is JavaScript destructuring with renaming.

Normally:

```tsx
const { data } = useQuery(...);
```

But we rename:

```tsx
data: posts
```

Meaning:

```text
data
 ↓ renamed to
posts
```

So now we can write:

```tsx
posts.map(...)
```

instead of:

```tsx
data.map(...)
```

---

# 17. The Important Difference

With `useEffect`, you control everything manually:

```text
useEffect
  ↓
Fetch
  ↓
setData
setLoading
setError
```

With TanStack Query:

```text
useQuery
  ↓
queryFn
  ↓
TanStack Query handles the API state
```

But remember:

TanStack Query does NOT replace `useState`.

You still use `useState` for client state.

Example:

```tsx
const [search, setSearch] = useState("");
const [isModalOpen, setIsModalOpen] = useState(false);
```

Use TanStack Query for:

```text
Server/API data
```

Use `useState` for:

```text
UI/client state
```

---

# 18. Simple Mental Model

```text
useState
   ↓
UI State

useQuery
   ↓
API / Server State
```

Example:

```text
Search input
→ useState

Dark mode
→ useState

Modal open
→ useState

Posts from API
→ useQuery

Users from API
→ useQuery
```

---

# What You Should Remember

The most important things from this step:

```text
TanStack Query
=
Server state management library
```

Main hook:

```tsx
useQuery()
```

Basic structure:

```tsx
const {
    data,
    isLoading,
    error,
} = useQuery({
    queryKey: ["posts"],
    queryFn: getPosts,
});
```

And:

```text
queryKey
↓
Unique identity for cached API data

queryFn
↓
Function that fetches the data
```

---

# Next Step

Now that you understand basic `useQuery`, next we should learn:

# Step 11 — TanStack Query Deep Dive

We'll cover:

```text
staleTime
cacheTime / gcTime
refetching
refetch()
query keys
dependent queries
```

Then after that:

```text
useMutation()
```

for:

```text
POST
PUT
DELETE
```

This will complete the important parts of TanStack Query.


------------------------------------------------------------------------------------------------------------------------------------------

# Step 11 — TanStack Query Deep Dive

Now you know the basic:

```tsx
useQuery({
    queryKey: ["posts"],
    queryFn: getPosts,
});
```

Let's understand what happens behind the scenes and the important features of TanStack Query.

---

# 1. `staleTime`

This is one of the most important concepts.

Imagine you fetch posts:

```text
API
 ↓
Posts received
 ↓
Stored in cache
```

TanStack Query now has the data.

But an important question is:

> How long should TanStack Query consider this data fresh?

That's what `staleTime` controls.

---

## Example

```tsx
useQuery({
    queryKey: ["posts"],
    queryFn: getPosts,
    staleTime: 10000,
});
```

```text
10000 milliseconds
=
10 seconds
```

This means:

```text
For 10 seconds
↓
Data is considered FRESH
```

After 10 seconds:

```text
Data becomes STALE
```

---

# 2. Fresh vs Stale

Think of API data like food.

```text
Fresh Data
↓
Recently fetched
↓
Still trusted
```

After some time:

```text
Stale Data
↓
Old data
↓
May need updating
```

Example timeline:

```text
0 seconds
↓
Fetch API
↓
Data is FRESH

5 seconds
↓
Still FRESH

10 seconds
↓
Data becomes STALE
```

---

# 3. Default `staleTime`

By default:

```text
staleTime = 0
```

That means the data becomes stale immediately after fetching.

Important:

```text
STALE ≠ DELETED
```

The data still exists in the cache.

It simply means:

> This data may need to be checked or refetched.

---

# 4. Example with `staleTime`

```tsx
const {
    data: posts,
    isLoading,
    error,
} = useQuery({
    queryKey: ["posts"],
    queryFn: getPosts,
    staleTime: 1000 * 60,
});
```

This means:

```text
1000 ms
×
60
=
60,000 ms
=
1 minute
```

For one minute:

```text
Posts data
↓
Fresh
```

After one minute:

```text
Posts data
↓
Stale
```

---

# 5. Why Does `staleTime` Matter?

Imagine the user goes:

```text
Posts Page
    ↓
Profile Page
    ↓
Posts Page
```

Without `staleTime`:

```text
Data may be considered stale immediately.
```

With:

```tsx
staleTime: 1000 * 60 * 5
```

```text
For 5 minutes
↓
Use fresh cached data
```

This can reduce unnecessary API requests.

---

# 6. `gcTime` (Previously Called `cacheTime`)

Older tutorials may use:

```text
cacheTime
```

In newer TanStack Query versions:

```text
gcTime
```

GC means:

```text
Garbage Collection
```

`gcTime` controls:

> How long unused cached data stays in memory before TanStack Query removes it.

Example:

```tsx
useQuery({
    queryKey: ["posts"],
    queryFn: getPosts,
    gcTime: 1000 * 60 * 10,
});
```

This means:

```text
Unused cached data
↓
Keep for 10 minutes
↓
Remove from cache
```

---

# 7. `staleTime` vs `gcTime`

This is commonly confusing.

Let's make it simple.

### `staleTime`

Controls:

```text
How long data is considered fresh
```

### `gcTime`

Controls:

```text
How long unused data stays in cache
```

---

## Visual Example

Suppose:

```tsx
staleTime: 1 minute
gcTime: 10 minutes
```

Timeline:

```text
0 min
↓
Fetch API

0–1 min
↓
FRESH

After 1 min
↓
STALE

User leaves component

Cache remains

Up to 10 min unused
↓
TanStack Query removes cache
```

---

# 8. Easy Way to Remember

```text
staleTime
↓
Freshness timer


gcTime
↓
Cache removal timer
```

---

# 9. `refetch()`

Sometimes you want to manually fetch data again.

Example:

```tsx
const {
    data: posts,
    isLoading,
    error,
    refetch,
} = useQuery({
    queryKey: ["posts"],
    queryFn: getPosts,
});
```

Now:

```tsx
<button onClick={() => refetch()}>
    Refresh Posts
</button>
```

When the user clicks:

```text
Refresh Posts
      ↓
refetch()
      ↓
API request runs again
      ↓
New data arrives
      ↓
UI updates
```

---

# 10. Full `refetch()` Example

```tsx
import { useQuery } from "@tanstack/react-query";

type Post = {
    userId: number;
    id: number;
    title: string;
    body: string;
};

async function getPosts(): Promise<Post[]> {
    const response = await fetch(
        "https://jsonplaceholder.typicode.com/posts"
    );

    if (!response.ok) {
        throw new Error("Failed to fetch posts");
    }

    return response.json();
}

function App() {
    const {
        data: posts,
        isLoading,
        error,
        refetch,
    } = useQuery({
        queryKey: ["posts"],
        queryFn: getPosts,
        staleTime: 1000 * 60,
    });

    if (isLoading) {
        return <p>Loading...</p>;
    }

    if (error) {
        return <p>{error.message}</p>;
    }

    return (
        <div>
            <h1>Posts</h1>

            <button onClick={() => refetch()}>
                Refresh Posts
            </button>

            {posts.map((post) => (
                <article key={post.id}>
                    <h2>{post.title}</h2>
                </article>
            ))}
        </div>
    );
}

export default App;
```

---

# 11. `isLoading` vs `isFetching`

This is another important concept.

We know:

```tsx
isLoading
```

But TanStack Query also has:

```tsx
isFetching
```

They are not exactly the same.

---

## `isLoading`

Usually means:

> The initial data is being loaded and we don't have data yet.

Example:

```text
First time opening page
        ↓
Fetch API
        ↓
isLoading = true
        ↓
No data yet
```

---

## `isFetching`

Means:

> Any API request is currently happening.

This includes refetching.

Example:

```text
Posts already displayed
        ↓
User clicks Refresh
        ↓
API request starts again
        ↓
isFetching = true
```

But:

```text
isLoading = false
```

Because we already have data.

---

# Visual Difference

### First API Request

```text
No data exists
↓
Fetching API

isLoading = true
isFetching = true
```

### Refetching

```text
Data already exists
↓
Fetching API again

isLoading = false
isFetching = true
```

This is useful for showing:

```text
Loading...
```

on the first page load, and:

```text
Refreshing...
```

when refreshing existing data.

---

# 12. Example

```tsx
const {
    data: posts,
    isLoading,
    isFetching,
} = useQuery({
    queryKey: ["posts"],
    queryFn: getPosts,
});
```

Then:

```tsx
if (isLoading) {
    return <p>Loading posts...</p>;
}
```

Inside the UI:

```tsx
<button onClick={() => refetch()}>
    {isFetching ? "Refreshing..." : "Refresh"}
</button>
```

Flow:

```text
Initial load
↓
Loading posts...


Data loaded
↓
Refresh


User clicks Refresh
↓
Refreshing...
```

---

# 13. Query Keys — Going Deeper

Previously:

```tsx
queryKey: ["posts"]
```

But query keys can contain multiple values.

Example:

```tsx
queryKey: ["posts", userId]
```

Suppose:

```text
User ID = 1
```

The key becomes:

```text
["posts", 1]
```

For User 2:

```text
["posts", 2]
```

These are different caches.

```text
["posts", 1]
      ↓
User 1 Posts Cache


["posts", 2]
      ↓
User 2 Posts Cache
```

---

# 14. Why Dynamic Query Keys Matter

Imagine:

```text
/posts?userId=1
```

and:

```text
/posts?userId=2
```

These return different data.

So TanStack Query needs different cache identities.

Correct:

```tsx
queryKey: ["posts", userId]
```

Not:

```tsx
queryKey: ["posts"]
```

because that would treat both API results as the same query.

---

# 15. Example with Dynamic User ID

```tsx
function UserPosts({ userId }: { userId: number }) {
    const {
        data: posts,
        isLoading,
    } = useQuery({
        queryKey: ["posts", userId],

        queryFn: async () => {
            const response = await fetch(
                `https://jsonplaceholder.typicode.com/posts?userId=${userId}`
            );

            if (!response.ok) {
                throw new Error("Failed to fetch posts");
            }

            return response.json();
        },
    });

    if (isLoading) {
        return <p>Loading...</p>;
    }

    return (
        <div>
            {posts?.map((post) => (
                <p key={post.id}>
                    {post.title}
                </p>
            ))}
        </div>
    );
}
```

Flow:

```text
userId changes
     ↓
queryKey changes
     ↓
["posts", 1]
        ↓
Fetch/cache User 1 posts


["posts", 2]
        ↓
Fetch/cache User 2 posts
```

---

# 16. Dependent Queries

Sometimes one API request depends on another.

Example:

```text
Get User
   ↓
Get User ID
   ↓
Get User Posts
```

The second query should not run until we have the user ID.

TanStack Query supports this with:

```tsx
enabled
```

Example:

```tsx
const userQuery = useQuery({
    queryKey: ["user"],
    queryFn: getUser,
});

const postsQuery = useQuery({
    queryKey: ["posts", userQuery.data?.id],

    queryFn: () => getPosts(userQuery.data!.id),

    enabled: !!userQuery.data?.id,
});
```

---

# 17. Understanding `enabled`

```tsx
enabled: !!userQuery.data?.id
```

Looks complicated, but the idea is simple.

```text
Do we have user ID?
       ↓
Yes → enabled = true → Run query

No → enabled = false → Don't run query
```

Example:

```text
User API loading
↓
No user ID yet
↓
Posts API does not run


User API finishes
↓
User ID exists
↓
Posts API runs
```

---

# 18. Common Query Options Summary

```tsx
useQuery({
    queryKey: ["posts"],
    queryFn: getPosts,

    staleTime: 1000 * 60,

    gcTime: 1000 * 60 * 10,

    enabled: true,
});
```

Meaning:

| Option      | Purpose                     |
| ----------- | --------------------------- |
| `queryKey`  | Unique identity of query    |
| `queryFn`   | Function that fetches data  |
| `staleTime` | How long data stays fresh   |
| `gcTime`    | How long unused cache stays |
| `enabled`   | Controls whether query runs |

---

# 19. Complete Mental Model

```text
useQuery()
   │
   ├── queryKey
   │      ↓
   │   Identify cache
   │
   ├── queryFn
   │      ↓
   │   Fetch data
   │
   ├── staleTime
   │      ↓
   │   Fresh → Stale
   │
   ├── gcTime
   │      ↓
   │   Keep unused cache
   │
   └── enabled
          ↓
      Run or wait
```

---

# What You Should Remember

The most important concepts:

```text
staleTime
↓
How long data is fresh

gcTime
↓
How long unused cached data remains

refetch()
↓
Manually fetch again

isLoading
↓
Initial loading

isFetching
↓
Any request currently running

queryKey
↓
Unique cache identity

enabled
↓
Controls whether query runs
```

---

# Phase 9 Progress

```text
## 9. API Integration

[x] Step 1 — What is an API?
[x] Step 2 — REST API basics
[x] Step 3 — fetch()
[x] Step 4 — Fetch API data in React
[x] Step 5 — Loading states
[x] Step 6 — Error handling
[x] Step 7 — POST / PUT / DELETE
[x] Step 8 — Axios
[x] Step 9 — API Project
[x] Step 10 — TanStack Query Basics
[x] Step 11 — TanStack Query Deep Dive

[ ] Step 12 — useMutation()
[ ] Step 13 — Final API Integration Project
```

# Next → Step 12: `useMutation()`

`useQuery()` is mainly for:

```text
GET → Read data
```

`useMutation()` is mainly for changing data:

```text
POST   → Create
PUT    → Update
PATCH  → Update partially
DELETE → Delete
```

This is the second major TanStack Query hook.


------------------------------------------------------------------------------------------------------------------------------------------

# Step 12 — TanStack Query `useMutation()`

Until now, we learned:

```text
useQuery()
↓
GET data from API
↓
Read server data
```

Now we learn:

```text
useMutation()
↓
Change server data
```

---

# 1. What Is a Mutation?

A mutation means changing data on the server.

Examples:

```text
POST   → Create data
PUT    → Replace/update data
PATCH  → Partially update data
DELETE → Delete data
```

So:

```text
useQuery()
→ Reading data


useMutation()
→ Changing data
```

---

# 2. Real-World Example

Imagine a Todo application.

You want to:

```text
Get Todos
→ GET
→ useQuery()


Add Todo
→ POST
→ useMutation()


Update Todo
→ PATCH
→ useMutation()


Delete Todo
→ DELETE
→ useMutation()
```

Visual flow:

```text
                 API
                  │
        ┌─────────┴─────────┐
        │                   │
     useQuery          useMutation
        │                   │
     GET data          Change data
```

---

# 3. Basic Structure of `useMutation`

```tsx
const mutation = useMutation({
    mutationFn: createPost,
});
```

The important option is:

```tsx
mutationFn
```

Similar to `queryFn` in `useQuery`.

```text
useQuery
→ queryFn


useMutation
→ mutationFn
```

---

# 4. Creating a POST Request

Let's create a function first.

```tsx
type NewPost = {
    title: string;
    body: string;
    userId: number;
};

async function createPost(post: NewPost) {
    const response = await fetch(
        "https://jsonplaceholder.typicode.com/posts",
        {
            method: "POST",

            headers: {
                "Content-Type": "application/json",
            },

            body: JSON.stringify(post),
        }
    );

    if (!response.ok) {
        throw new Error("Failed to create post");
    }

    return response.json();
}
```

This function:

```text
Receives new post data
        ↓
Makes POST request
        ↓
Returns created post
```

---

# 5. Using `useMutation`

```tsx
import { useMutation } from "@tanstack/react-query";
```

Then:

```tsx
const mutation = useMutation({
    mutationFn: createPost,
});
```

Now TanStack Query gives us mutation information.

For example:

```text
mutation.data
mutation.error
mutation.isPending
mutation.isSuccess
```

---

# 6. Triggering the Mutation

Unlike `useQuery`, mutations do not automatically run.

You manually trigger them.

```tsx
mutation.mutate({
    title: "Learning React Query",
    body: "Understanding useMutation",
    userId: 1,
});
```

Flow:

```text
User clicks button
       ↓
mutate()
       ↓
mutationFn()
       ↓
POST API request
       ↓
Server changes data
```

---

# 7. Complete Basic Example

```tsx
import { useMutation } from "@tanstack/react-query";

type NewPost = {
    title: string;
    body: string;
    userId: number;
};

async function createPost(post: NewPost) {
    const response = await fetch(
        "https://jsonplaceholder.typicode.com/posts",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
            },
            body: JSON.stringify(post),
        }
    );

    if (!response.ok) {
        throw new Error("Failed to create post");
    }

    return response.json();
}

function App() {
    const mutation = useMutation({
        mutationFn: createPost,
    });

    function handleCreatePost() {
        mutation.mutate({
            title: "Learning TanStack Query",
            body: "This is my first mutation",
            userId: 1,
        });
    }

    return (
        <div>
            <button onClick={handleCreatePost}>
                Create Post
            </button>

            {mutation.isPending && (
                <p>Creating post...</p>
            )}

            {mutation.isError && (
                <p>Something went wrong</p>
            )}

            {mutation.isSuccess && (
                <p>Post created successfully!</p>
            )}
        </div>
    );
}

export default App;
```

---

# 8. Mutation States

Just like queries have loading states, mutations have states.

```text
Mutation starts
      ↓
isPending = true
      ↓
      ├── Success → isSuccess = true
      │
      └── Error → isError = true
```

Main states:

| Property    | Meaning                         |
| ----------- | ------------------------------- |
| `isPending` | Mutation is currently running   |
| `isSuccess` | Mutation completed successfully |
| `isError`   | Mutation failed                 |
| `data`      | Returned API data               |
| `error`     | Error information               |

---

# 9. `mutate()` vs `mutateAsync()`

There are two ways to trigger a mutation.

## `mutate()`

```tsx
mutation.mutate(data);
```

This triggers the mutation.

---

## `mutateAsync()`

```tsx
await mutation.mutateAsync(data);
```

This returns a Promise, so you can use `await`.

Example:

```tsx
async function handleCreatePost() {
    try {
        const newPost = await mutation.mutateAsync({
            title: "New Post",
            body: "Hello",
            userId: 1,
        });

        console.log(newPost);
    } catch (error) {
        console.error(error);
    }
}
```

Simple rule:

```text
mutate()
→ Simple mutation


mutateAsync()
→ When you need await / Promise behavior
```

---

# 10. Important Problem After Creating Data

Imagine we have:

```text
Posts List
↓
useQuery(["posts"])
```

Then we create a new post:

```text
POST
↓
useMutation()
```

Question:

> How does the posts list know that new data was added?

The existing cached posts may now be outdated.

This is where:

# Query Invalidation

comes in.

---

# 11. `invalidateQueries()`

TanStack Query provides:

```tsx
queryClient.invalidateQueries()
```

This tells TanStack Query:

> This cached data might be outdated. Please refetch it when appropriate.

First get the `queryClient`.

```tsx
import { useQueryClient } from "@tanstack/react-query";
```

Inside the component:

```tsx
const queryClient = useQueryClient();
```

Then:

```tsx
queryClient.invalidateQueries({
    queryKey: ["posts"],
});
```

This targets:

```text
["posts"]
```

---

# 12. Using `onSuccess`

We usually invalidate after a successful mutation.

```tsx
const mutation = useMutation({
    mutationFn: createPost,

    onSuccess: () => {
        queryClient.invalidateQueries({
            queryKey: ["posts"],
        });
    },
});
```

Flow:

```text
Create Post
    ↓
POST request
    ↓
Success
    ↓
Invalidate ["posts"]
    ↓
Posts query becomes outdated
    ↓
Refetch posts
    ↓
UI updates
```

This is one of the most important patterns in TanStack Query.

---

# 13. Complete Example — Query + Mutation

Let's combine everything.

```tsx
import {
    useMutation,
    useQuery,
    useQueryClient,
} from "@tanstack/react-query";

type Post = {
    userId: number;
    id: number;
    title: string;
    body: string;
};

type NewPost = {
    title: string;
    body: string;
    userId: number;
};

async function getPosts(): Promise<Post[]> {
    const response = await fetch(
        "https://jsonplaceholder.typicode.com/posts"
    );

    if (!response.ok) {
        throw new Error("Failed to fetch posts");
    }

    return response.json();
}

async function createPost(post: NewPost): Promise<Post> {
    const response = await fetch(
        "https://jsonplaceholder.typicode.com/posts",
        {
            method: "POST",

            headers: {
                "Content-Type": "application/json",
            },

            body: JSON.stringify(post),
        }
    );

    if (!response.ok) {
        throw new Error("Failed to create post");
    }

    return response.json();
}

function App() {
    const queryClient = useQueryClient();

    const {
        data: posts,
        isLoading,
        error,
    } = useQuery({
        queryKey: ["posts"],
        queryFn: getPosts,
    });

    const mutation = useMutation({
        mutationFn: createPost,

        onSuccess: () => {
            queryClient.invalidateQueries({
                queryKey: ["posts"],
            });
        },
    });

    function handleCreatePost() {
        mutation.mutate({
            title: "My New Post",
            body: "Learning useMutation",
            userId: 1,
        });
    }

    if (isLoading) {
        return <p>Loading...</p>;
    }

    if (error) {
        return <p>{error.message}</p>;
    }

    return (
        <div>
            <h1>Posts</h1>

            <button
                onClick={handleCreatePost}
                disabled={mutation.isPending}
            >
                {mutation.isPending
                    ? "Creating..."
                    : "Create Post"}
            </button>

            {posts?.map((post) => (
                <article key={post.id}>
                    <h2>{post.title}</h2>
                </article>
            ))}
        </div>
    );
}

export default App;
```

---

# 14. Understanding the Full Flow

```text
PAGE LOAD
    ↓
useQuery()
    ↓
GET /posts
    ↓
Save Posts in Cache
    ↓
Display Posts


USER CLICKS CREATE
    ↓
mutation.mutate()
    ↓
useMutation()
    ↓
POST /posts
    ↓
Success
    ↓
invalidateQueries(["posts"])
    ↓
Posts data becomes outdated
    ↓
Refetch GET /posts
    ↓
Updated UI
```

---

# 15. PUT / PATCH with `useMutation`

The pattern is exactly the same.

Example update function:

```tsx
async function updatePost(post: Post): Promise<Post> {
    const response = await fetch(
        `https://jsonplaceholder.typicode.com/posts/${post.id}`,
        {
            method: "PATCH",

            headers: {
                "Content-Type": "application/json",
            },

            body: JSON.stringify({
                title: post.title,
            }),
        }
    );

    if (!response.ok) {
        throw new Error("Failed to update post");
    }

    return response.json();
}
```

Mutation:

```tsx
const updateMutation = useMutation({
    mutationFn: updatePost,

    onSuccess: () => {
        queryClient.invalidateQueries({
            queryKey: ["posts"],
        });
    },
});
```

Trigger:

```tsx
updateMutation.mutate({
    id: 1,
    userId: 1,
    title: "Updated Title",
    body: "",
});
```

---

# 16. DELETE with `useMutation`

API function:

```tsx
async function deletePost(id: number) {
    const response = await fetch(
        `https://jsonplaceholder.typicode.com/posts/${id}`,
        {
            method: "DELETE",
        }
    );

    if (!response.ok) {
        throw new Error("Failed to delete post");
    }
}
```

Mutation:

```tsx
const deleteMutation = useMutation({
    mutationFn: deletePost,

    onSuccess: () => {
        queryClient.invalidateQueries({
            queryKey: ["posts"],
        });
    },
});
```

Trigger:

```tsx
deleteMutation.mutate(1);
```

Flow:

```text
Delete Button
    ↓
mutate(id)
    ↓
DELETE API
    ↓
Success
    ↓
Invalidate posts cache
    ↓
Refetch posts
```

---

# 17. `onSuccess`, `onError`, `onSettled`

`useMutation` gives lifecycle callbacks.

```tsx
useMutation({
    mutationFn: createPost,

    onSuccess: () => {
        console.log("Success");
    },

    onError: () => {
        console.log("Error");
    },

    onSettled: () => {
        console.log("Always runs");
    },
});
```

Think:

```text
Mutation Starts
      ↓
      ├── Success
      │      ↓
      │   onSuccess
      │
      └── Error
             ↓
           onError

Finally
↓
onSettled
```

Similar to:

```text
try
↓
Success

catch
↓
Error

finally
↓
Always
```

---

# 18. The Most Important Pattern

You should remember this:

```text
GET
↓
useQuery


POST / PUT / PATCH / DELETE
↓
useMutation


After successful mutation
↓
invalidateQueries()
```

This pattern is used constantly in real applications.

---

# 19. `useQuery` vs `useMutation`

| `useQuery`         | `useMutation`                        |
| ------------------ | ------------------------------------ |
| Read data          | Change data                          |
| Usually GET        | POST, PUT, PATCH, DELETE             |
| Runs automatically | Triggered manually                   |
| Uses `queryKey`    | Does not need a query key            |
| Uses `queryFn`     | Uses `mutationFn`                    |
| Cached             | Mutations aren't cached like queries |

---

# Complete Mental Model

```text
SERVER DATA


GET
 │
 ↓
useQuery
 │
 ↓
Cache
 │
 ↓
UI


POST / PUT / PATCH / DELETE
 │
 ↓
useMutation
 │
 ↓
Server Changes
 │
 ↓
onSuccess
 │
 ↓
invalidateQueries()
 │
 ↓
useQuery fetches updated data
 │
 ↓
UI updates
```

---

# What You Should Remember

```text
useQuery()
→ Get server data


useMutation()
→ Change server data


mutationFn
→ Function that performs POST/PUT/DELETE


mutate()
→ Trigger mutation


isPending
→ Mutation is running


onSuccess
→ Runs after successful mutation


invalidateQueries()
→ Mark related cached data as outdated and refetch active queries
```

---

# Phase 9 Progress

```text
## 9. API Integration

[x] Step 1 — What is an API?
[x] Step 2 — REST API basics
[x] Step 3 — fetch()
[x] Step 4 — Fetch API data in React
[x] Step 5 — Loading states
[x] Step 6 — Error handling
[x] Step 7 — POST / PUT / DELETE
[x] Step 8 — Axios
[x] Step 9 — API Project
[x] Step 10 — TanStack Query Basics
[x] Step 11 — TanStack Query Deep Dive
[x] Step 12 — useMutation()

[ ] Step 13 — Final API Integration Project
```

# Next → Step 13: Final API Integration Project

We'll build a proper small application combining:

```text
TanStack Query
useQuery
useMutation
GET
POST
DELETE
Loading states
Error handling
Query invalidation
Search/filter
TypeScript
```

This will complete the **API Integration phase** of your roadmap.


------------------------------------------------------------------------------------------------------------------------------------------


# Step 13 — Final API Integration Project

This is the final project for **Phase 9: API Integration**.

We will combine everything you've learned so far.

## Project: Posts Manager

Features:

```text
✓ Fetch posts with TanStack Query
✓ Loading state
✓ Error handling
✓ Create a post
✓ Delete a post
✓ Search posts
✓ Filter posts
✓ Query invalidation
✓ TypeScript
```

---

# 1. Project Structure

A cleaner structure than putting everything inside `App.tsx`:

```text
src/
├── api/
│   └── posts.ts
├── types/
│   └── post.ts
├── App.tsx
└── main.tsx
```

---

# 2. Create the Type

## `src/types/post.ts`

```tsx
export type Post = {
    userId: number;
    id: number;
    title: string;
    body: string;
};

export type NewPost = {
    title: string;
    body: string;
    userId: number;
};
```

Why separate types?

```text
Post
↓
Existing post from API
↓
Has id


NewPost
↓
Data for creating a post
↓
Doesn't need id
```

---

# 3. Create API Functions

## `src/api/posts.ts`

```tsx
import type { Post, NewPost } from "../types/post";

const API_URL = "https://jsonplaceholder.typicode.com/posts";
```

### GET Posts

```tsx
export async function getPosts(): Promise<Post[]> {
    const response = await fetch(API_URL);

    if (!response.ok) {
        throw new Error("Failed to fetch posts");
    }

    return response.json();
}
```

---

### Create Post

```tsx
export async function createPost(
    post: NewPost
): Promise<Post> {
    const response = await fetch(API_URL, {
        method: "POST",

        headers: {
            "Content-Type": "application/json",
        },

        body: JSON.stringify(post),
    });

    if (!response.ok) {
        throw new Error("Failed to create post");
    }

    return response.json();
}
```

---

### Delete Post

```tsx
export async function deletePost(id: number): Promise<void> {
    const response = await fetch(`${API_URL}/${id}`, {
        method: "DELETE",
    });

    if (!response.ok) {
        throw new Error("Failed to delete post");
    }
}
```

Now our API logic is separated:

```text
api/posts.ts
     ↓
Handles API requests


App.tsx
     ↓
Handles UI
```

This is better architecture.

---

# 4. Setup TanStack Query

## `src/main.tsx`

```tsx
import React from "react";
import ReactDOM from "react-dom/client";
import {
    QueryClient,
    QueryClientProvider,
} from "@tanstack/react-query";

import App from "./App";

const queryClient = new QueryClient();

ReactDOM.createRoot(
    document.getElementById("root")!
).render(
    <React.StrictMode>
        <QueryClientProvider client={queryClient}>
            <App />
        </QueryClientProvider>
    </React.StrictMode>
);
```

Flow:

```text
QueryClient
    ↓
QueryClientProvider
    ↓
App
    ↓
useQuery / useMutation
```

---

# 5. Build the App

## `src/App.tsx`

First imports:

```tsx
import { useState } from "react";

import {
    useMutation,
    useQuery,
    useQueryClient,
} from "@tanstack/react-query";

import {
    getPosts,
    createPost,
    deletePost,
} from "./api/posts";
```

---

# 6. Create Local UI States

```tsx
const [search, setSearch] = useState("");
const [selectedUser, setSelectedUser] = useState("all");

const [title, setTitle] = useState("");
const [body, setBody] = useState("");
```

These are **client/UI states**, so `useState` is correct.

```text
API Posts
→ useQuery


Search input
→ useState


Selected filter
→ useState


Form input
→ useState
```

---

# 7. Fetch Posts with `useQuery`

```tsx
const {
    data: posts,
    isLoading,
    isError,
    error,
} = useQuery({
    queryKey: ["posts"],
    queryFn: getPosts,
});
```

TanStack Query manages:

```text
GET request
Loading
Error
Cache
Server data
```

---

# 8. Create Post Mutation

```tsx
const queryClient = useQueryClient();

const createMutation = useMutation({
    mutationFn: createPost,

    onSuccess: () => {
        queryClient.invalidateQueries({
            queryKey: ["posts"],
        });
    },
});
```

Flow:

```text
Create Post
    ↓
POST API
    ↓
Success
    ↓
Invalidate ["posts"]
    ↓
Refetch posts
```

---

# 9. Delete Post Mutation

```tsx
const deleteMutation = useMutation({
    mutationFn: deletePost,

    onSuccess: () => {
        queryClient.invalidateQueries({
            queryKey: ["posts"],
        });
    },
});
```

Same pattern:

```text
Delete
 ↓
Mutation
 ↓
Success
 ↓
Invalidate posts
 ↓
Refetch
```

---

# 10. Handle Create Form

```tsx
function handleSubmit(
    event: React.FormEvent<HTMLFormElement>
) {
    event.preventDefault();

    if (!title.trim() || !body.trim()) {
        return;
    }

    createMutation.mutate(
        {
            title,
            body,
            userId: 1,
        },
        {
            onSuccess: () => {
                setTitle("");
                setBody("");
            },
        }
    );
}
```

Flow:

```text
Submit Form
    ↓
preventDefault()
    ↓
Validate input
    ↓
mutate()
    ↓
POST API
    ↓
Success
    ↓
Clear inputs
```

---

# 11. Search Functionality

```tsx
const searchedPosts = posts?.filter((post) =>
    post.title
        .toLowerCase()
        .includes(search.toLowerCase())
);
```

---

# 12. Filter by User

```tsx
const filteredPosts = searchedPosts?.filter((post) => {
    if (selectedUser === "all") {
        return true;
    }

    return post.userId === Number(selectedUser);
});
```

Complete data flow:

```text
API Posts
    ↓
Search
    ↓
User Filter
    ↓
Final Posts
```

---

# 13. Full `App.tsx`

```tsx
import { useState } from "react";

import {
    useMutation,
    useQuery,
    useQueryClient,
} from "@tanstack/react-query";

import {
    getPosts,
    createPost,
    deletePost,
} from "./api/posts";

function App() {
    const queryClient = useQueryClient();

    const [search, setSearch] = useState("");
    const [selectedUser, setSelectedUser] = useState("all");

    const [title, setTitle] = useState("");
    const [body, setBody] = useState("");

    const {
        data: posts,
        isLoading,
        isError,
        error,
    } = useQuery({
        queryKey: ["posts"],
        queryFn: getPosts,
    });

    const createMutation = useMutation({
        mutationFn: createPost,

        onSuccess: () => {
            queryClient.invalidateQueries({
                queryKey: ["posts"],
            });
        },
    });

    const deleteMutation = useMutation({
        mutationFn: deletePost,

        onSuccess: () => {
            queryClient.invalidateQueries({
                queryKey: ["posts"],
            });
        },
    });

    function handleSubmit(
        event: React.FormEvent<HTMLFormElement>
    ) {
        event.preventDefault();

        if (!title.trim() || !body.trim()) {
            return;
        }

        createMutation.mutate(
            {
                title,
                body,
                userId: 1,
            },
            {
                onSuccess: () => {
                    setTitle("");
                    setBody("");
                },
            }
        );
    }

    const searchedPosts = posts?.filter((post) =>
        post.title
            .toLowerCase()
            .includes(search.toLowerCase())
    );

    const filteredPosts = searchedPosts?.filter((post) => {
        if (selectedUser === "all") {
            return true;
        }

        return post.userId === Number(selectedUser);
    });

    if (isLoading) {
        return <p>Loading posts...</p>;
    }

    if (isError) {
        return <p>{error.message}</p>;
    }

    return (
        <div>
            <h1>Posts Manager</h1>

            <form onSubmit={handleSubmit}>
                <h2>Create Post</h2>

                <input
                    type="text"
                    placeholder="Title"
                    value={title}
                    onChange={(event) =>
                        setTitle(event.target.value)
                    }
                />

                <br />

                <textarea
                    placeholder="Body"
                    value={body}
                    onChange={(event) =>
                        setBody(event.target.value)
                    }
                />

                <br />

                <button
                    type="submit"
                    disabled={createMutation.isPending}
                >
                    {createMutation.isPending
                        ? "Creating..."
                        : "Create Post"}
                </button>
            </form>

            <hr />

            <h2>Posts</h2>

            <input
                type="text"
                placeholder="Search posts..."
                value={search}
                onChange={(event) =>
                    setSearch(event.target.value)
                }
            />

            <select
                value={selectedUser}
                onChange={(event) =>
                    setSelectedUser(event.target.value)
                }
            >
                <option value="all">All Users</option>

                <option value="1">User 1</option>
                <option value="2">User 2</option>
                <option value="3">User 3</option>
                <option value="4">User 4</option>
                <option value="5">User 5</option>
                <option value="6">User 6</option>
                <option value="7">User 7</option>
                <option value="8">User 8</option>
                <option value="9">User 9</option>
                <option value="10">User 10</option>
            </select>

            <p>
                Showing {filteredPosts?.length ?? 0} posts
            </p>

            {filteredPosts?.length === 0 ? (
                <p>No posts found.</p>
            ) : (
                filteredPosts?.map((post) => (
                    <article key={post.id}>
                        <h3>{post.title}</h3>

                        <p>{post.body}</p>

                        <small>
                            User: {post.userId}
                        </small>

                        <br />

                        <button
                            onClick={() =>
                                deleteMutation.mutate(post.id)
                            }
                            disabled={deleteMutation.isPending}
                        >
                            Delete
                        </button>

                        <hr />
                    </article>
                ))
            )}
        </div>
    );
}

export default App;
```

---

# Important Note About JSONPlaceholder

JSONPlaceholder is a fake API.

So when you:

```text
POST
DELETE
```

it returns successful responses, but the changes are not permanently saved to a real database.

That is expected.

The purpose is learning API integration.

---

# Final Architecture

```text
src/
│
├── api/
│   └── posts.ts
│       ↓
│       API functions
│
├── types/
│   └── post.ts
│       ↓
│       TypeScript types
│
├── App.tsx
│       ↓
│       UI + React logic
│
└── main.tsx
        ↓
        QueryClientProvider
```

---

# Complete Application Flow

```text
                    APPLICATION START
                           ↓
                   QueryClientProvider
                           ↓
                         App
                           ↓
                    useQuery(["posts"])
                           ↓
                        GET API
                           ↓
                       Cache Data
                           ↓
                       Display UI
                           │
          ┌────────────────┼────────────────┐
          ↓                ↓                ↓
        Search           Create           Delete
          ↓                ↓                ↓
      Filter Data      useMutation      useMutation
                           ↓                ↓
                         POST            DELETE
                           ↓                ↓
                         Success        Success
                           ↓                ↓
                    invalidateQueries(["posts"])
                           ↓
                        Refetch
                           ↓
                       Updated UI
```

---

# What You Have Learned in Phase 9

## API Basics

```text
✓ What is an API
✓ REST API
✓ GET
✓ POST
✓ PUT
✓ PATCH
✓ DELETE
✓ fetch()
✓ Axios
```

## React API Integration

```text
✓ useEffect API fetching
✓ Loading states
✓ Error handling
✓ Async/await
✓ Try/catch/finally
```

## TanStack Query

```text
✓ QueryClient
✓ QueryClientProvider
✓ useQuery
✓ queryKey
✓ queryFn
✓ staleTime
✓ gcTime
✓ refetch()
✓ isLoading
✓ isFetching
✓ enabled
✓ Dependent queries
✓ useMutation
✓ mutationFn
✓ mutate()
✓ mutateAsync()
✓ onSuccess
✓ invalidateQueries()
```

---

# Phase 9 Complete

```text
## 9. API Integration

[x] Fetch/Axios
[x] REST APIs
[x] Error handling
[x] Loading states
[x] TanStack Query
[x] useQuery
[x] useMutation
[x] Caching
[x] Query invalidation

BUILD:
[x] Fetch API data
[x] Search/filter UI
[x] Posts Manager with CRUD concepts
```
------------------------------------------------------------------------------------------------------------------------------------------