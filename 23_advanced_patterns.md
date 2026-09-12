# Phase 12 — Advanced React / Real-World Architecture

Now we're moving beyond basic React concepts into patterns commonly used in real applications.

## Phase 12 Roadmap

```text
Phase 12 — Advanced React

[ ] Step 1 — Custom Hooks
[ ] Step 2 — Reusable Custom Hook Examples
[ ] Step 3 — Higher-Order Components (HOC)
[ ] Step 4 — Render Props
[ ] Step 5 — Compound Components
[ ] Step 6 — Feature-Based Folder Structure
[ ] Step 7 — Environment Variables
[ ] Step 8 — Error Boundaries
[ ] Step 9 — Production Architecture Concepts
```

---

# Step 1 — Custom Hooks

## 1. What is a Custom Hook?

A Custom Hook is simply:

> A reusable function that contains React hook logic.

You already know hooks like:

```tsx
useState()
useEffect()
useContext()
```

A custom hook allows us to create our own hook.

Example:

```tsx
useCounter()
```

or:

```tsx
useFetch()
```

or:

```tsx
useLocalStorage()
```

---

# 2. Why Do We Need Custom Hooks?

Imagine two components.

### Component A

```tsx
function CounterA() {
    const [count, setCount] = useState(0);

    return (
        <button onClick={() => setCount(count + 1)}>
            {count}
        </button>
    );
}
```

### Component B

```tsx
function CounterB() {
    const [count, setCount] = useState(0);

    return (
        <button onClick={() => setCount(count + 1)}>
            {count}
        </button>
    );
}
```

Both contain similar logic.

Instead of repeating:

```text
useState
+
increase logic
+
decrease logic
```

We can move that logic into a custom hook.

```text
Component A ──┐
              │
              ▼
         useCounter()
              ▲
              │
Component B ──┘
```

---

# 3. Creating Our First Custom Hook

Create:

```text
src/
├── hooks/
│   └── useCounter.ts
```

## `useCounter.ts`

```tsx
import { useState } from "react";

function useCounter() {
    const [count, setCount] = useState(0);

    function increase() {
        setCount((previous) => previous + 1);
    }

    function decrease() {
        setCount((previous) => previous - 1);
    }

    return {
        count,
        increase,
        decrease,
    };
}

export default useCounter;
```

---

# 4. Using the Custom Hook

## `Counter.tsx`

```tsx
import useCounter from "./hooks/useCounter";

function Counter() {
    const {
        count,
        increase,
        decrease,
    } = useCounter();

    return (
        <div>
            <h1>{count}</h1>

            <button onClick={increase}>
                Increase
            </button>

            <button onClick={decrease}>
                Decrease
            </button>
        </div>
    );
}

export default Counter;
```

---

# 5. What Happened Here?

Previously:

```text
Counter Component

useState
setCount
increase logic
decrease logic
UI
```

Now:

```text
useCounter()
│
├── State
├── Increase logic
└── Decrease logic

        ↓

Counter Component
│
└── Only UI
```

This separation is very useful.

---

# 6. Important: Why Does It Start With `use`?

Custom hooks should start with:

```text
use
```

Examples:

```text
useCounter
useFetch
useLocalStorage
useAuth
useTheme
```

Why?

Because React recognizes functions starting with `use` as hooks.

Also, hooks follow React's Rules of Hooks.

---

# 7. Custom Hook Is NOT Global State

This is important.

If you do:

```tsx
const { count } = useCounter();
```

inside Component A, and:

```tsx
const { count } = useCounter();
```

inside Component B:

They do NOT automatically share the same state.

```text
Component A
   ↓
useCounter()
   ↓
Own state


Component B
   ↓
useCounter()
   ↓
Different own state
```

Each hook call creates its own state instance.

This confuses many beginners.

---

# 8. Full Example

```tsx
import { useState } from "react";

function useCounter(initialValue = 0) {
    const [count, setCount] =
        useState(initialValue);

    function increase() {
        setCount((previous) => previous + 1);
    }

    function decrease() {
        setCount((previous) => previous - 1);
    }

    function reset() {
        setCount(initialValue);
    }

    return {
        count,
        increase,
        decrease,
        reset,
    };
}

export default useCounter;
```

Usage:

```tsx
function App() {
    const {
        count,
        increase,
        decrease,
        reset,
    } = useCounter(10);

    return (
        <div>
            <h1>Count: {count}</h1>

            <button onClick={increase}>
                +
            </button>

            <button onClick={decrease}>
                -
            </button>

            <button onClick={reset}>
                Reset
            </button>
        </div>
    );
}
```

Initial value:

```text
useCounter(10)

↓

count = 10
```

---

# 9. The Main Purpose of Custom Hooks

Custom hooks help with:

```text
Reusable Logic
        +
Cleaner Components
        +
Separation of Logic and UI
```

For example:

```text
UI Component
    ↓
Uses
    ↓
Custom Hook
    ↓
Contains Logic
```

---

# Real-World Custom Hooks You May See

```text
useFetch()
useAuth()
useLocalStorage()
useDebounce()
useWindowSize()
useTheme()
useForm()
```

Example architecture:

```text
src/
├── components/
│   └── UserList.tsx
│
├── hooks/
│   ├── useFetch.ts
│   ├── useAuth.ts
│   └── useLocalStorage.ts
│
└── services/
    └── api.ts
```

---

# The Most Important Thing to Remember

```text
Custom Hook

= Reusable React logic
```

Not reusable UI.

```text
Component
↓
Reusable UI


Custom Hook
↓
Reusable Logic
```

---

# Phase 12 Progress

```text
[x] Step 1 — Custom Hooks

[ ] Step 2 — Reusable Custom Hook Examples
[ ] Step 3 — Higher-Order Components
[ ] Step 4 — Render Props
[ ] Step 5 — Compound Components
[ ] Step 6 — Feature-Based Folder Structure
[ ] Step 7 — Environment Variables
[ ] Step 8 — Error Boundaries
[ ] Step 9 — Production Architecture Concepts
```

# Next → Step 2: Real-World Custom Hooks

We'll build and understand useful hooks like:

```text
useLocalStorage()
useFetch()
useDebounce()
```

These are much closer to what you'll encounter in real React projects.

-------------------------------------------------------------------------------------------------------------------------------------------

# Step 2 — Real-World Custom Hooks

Now let's look at custom hooks you may actually encounter in React projects.

We will cover:

```text
1. useLocalStorage()
2. useFetch()
3. useDebounce()
```

---

# 1. `useLocalStorage()`

You already know `useState`:

```tsx
const [name, setName] = useState("");
```

But when the page refreshes:

```text
State disappears.
```

`localStorage` can store data in the browser.

```text
React State
    ↓
Page Refresh
    ↓
Lost


LocalStorage
    ↓
Page Refresh
    ↓
Still Exists
```

---

## Basic Custom Hook

### `useLocalStorage.ts`

```tsx
import { useState } from "react";

function useLocalStorage(
    key: string,
    initialValue: string
) {
    const [value, setValue] = useState(() => {
        const savedValue = localStorage.getItem(key);

        return savedValue || initialValue;
    });

    function setStoredValue(newValue: string) {
        setValue(newValue);

        localStorage.setItem(key, newValue);
    }

    return [value, setStoredValue] as const;
}

export default useLocalStorage;
```

---

## Using It

```tsx
import useLocalStorage from "./hooks/useLocalStorage";

function App() {
    const [name, setName] = useLocalStorage(
        "username",
        ""
    );

    return (
        <div>
            <input
                value={name}
                onChange={(event) =>
                    setName(event.target.value)
                }
            />

            <h1>Hello {name}</h1>
        </div>
    );
}
```

---

## Flow

```text
User types William
       ↓
setName("William")
       ↓
React state updates
       +
localStorage updates
       ↓
Page refreshes
       ↓
"William" is restored
```

---

# 2. `useFetch()`

This is a very common custom hook concept.

Instead of writing API logic inside every component:

```tsx
useEffect(() => {
    fetch(...)
        .then(...)
        .then(...);
}, []);
```

We can move the reusable logic into a hook.

---

## `useFetch.ts`

```tsx
import { useEffect, useState } from "react";

function useFetch<T>(url: string) {
    const [data, setData] = useState<T | null>(null);

    const [loading, setLoading] = useState(true);

    const [error, setError] = useState("");

    useEffect(() => {
        async function fetchData() {
            try {
                const response = await fetch(url);

                if (!response.ok) {
                    throw new Error("Failed to fetch");
                }

                const result = await response.json();

                setData(result);
            } catch {
                setError("Something went wrong");
            } finally {
                setLoading(false);
            }
        }

        fetchData();
    }, [url]);

    return {
        data,
        loading,
        error,
    };
}

export default useFetch;
```

---

# Why `<T>`?

This is TypeScript Generic syntax.

```tsx
useFetch<User[]>("/api/users")
```

Then TypeScript understands:

```text
data = User[]
```

Or:

```tsx
useFetch<Product[]>("/api/products")
```

Then:

```text
data = Product[]
```

Same hook.

Different data types.

Don't worry too much about generics for now—the main focus is the custom hook concept.

---

# Using `useFetch`

```tsx
type User = {
    id: number;
    name: string;
};

function Users() {
    const {
        data,
        loading,
        error,
    } = useFetch<User[]>(
        "https://jsonplaceholder.typicode.com/users"
    );

    if (loading) {
        return <p>Loading...</p>;
    }

    if (error) {
        return <p>{error}</p>;
    }

    return (
        <div>
            {data?.map((user) => (
                <p key={user.id}>
                    {user.name}
                </p>
            ))}
        </div>
    );
}
```

---

## The Separation

Without custom hook:

```text
Users Component
│
├── API logic
├── Loading logic
├── Error logic
├── State
└── UI
```

With custom hook:

```text
useFetch()
│
├── API logic
├── Loading state
├── Error state
└── Data state


Users Component
│
└── UI
```

This makes the component cleaner.

---

# 3. `useDebounce()`

Debouncing is very useful for:

```text
Search inputs
API calls
Autocomplete
Filtering
```

---

## The Problem

Imagine a search input.

User types:

```text
W
Wi
Wil
Will
Willi
William
```

Without debounce:

```text
API Call → W
API Call → Wi
API Call → Wil
API Call → Will
API Call → Willi
API Call → William
```

Too many API calls.

---

## With Debounce

We wait until the user stops typing.

```text
User types
    ↓
Wait 500ms
    ↓
User still typing?
    ↓ Yes
Reset timer
    ↓
User stops typing
    ↓
API Call
```

---

## `useDebounce.ts`

```tsx
import { useEffect, useState } from "react";

function useDebounce<T>(
    value: T,
    delay: number
) {
    const [debouncedValue, setDebouncedValue] =
        useState(value);

    useEffect(() => {
        const timer = setTimeout(() => {
            setDebouncedValue(value);
        }, delay);

        return () => {
            clearTimeout(timer);
        };
    }, [value, delay]);

    return debouncedValue;
}

export default useDebounce;
```

---

# Using It

```tsx
import { useState } from "react";
import useDebounce from "./hooks/useDebounce";

function Search() {
    const [search, setSearch] = useState("");

    const debouncedSearch = useDebounce(
        search,
        500
    );

    return (
        <div>
            <input
                value={search}
                onChange={(event) =>
                    setSearch(event.target.value)
                }
                placeholder="Search..."
            />

            <p>
                Searching for: {debouncedSearch}
            </p>
        </div>
    );
}
```

---

# What Happens?

Suppose the user types quickly:

```text
W → Wi → Wil → Will
```

`search` updates immediately:

```text
search = "Will"
```

But `debouncedSearch` waits:

```text
Wait 500ms
```

After the user stops typing:

```text
debouncedSearch = "Will"
```

Then you can make an API call.

---

# Full Flow

```text
Input
  ↓
search state changes immediately
  ↓
useDebounce(search, 500)
  ↓
Wait 500ms
  ↓
No new typing?
  ↓
Update debounced value
  ↓
Fetch/Search API
```

---

# Comparison of These Hooks

| Hook              | Purpose                     |
| ----------------- | --------------------------- |
| `useLocalStorage` | Persist data in browser     |
| `useFetch`        | Reusable API fetching logic |
| `useDebounce`     | Delay value updates         |

---

# The Bigger Picture

Custom hooks allow us to extract reusable logic.

```text
useState + useEffect + Logic
            ↓
      Custom Hook
            ↓
Reusable in multiple components
```

Examples:

```text
useAuth()
useTheme()
useFetch()
useDebounce()
useLocalStorage()
useWindowSize()
useMediaQuery()
```

---

# Important Distinction

### Component

```text
Reusable UI
```

### Custom Hook

```text
Reusable Logic
```

Example:

```text
useFetch()
    ↓
Reusable Logic


<UserList />
    ↓
Reusable UI
```

---

# What You Should Remember

The three important examples:

```text
useLocalStorage()
↓
Keeps data after refresh


useFetch()
↓
Reusable API logic


useDebounce()
↓
Wait before updating a value
```

---

# Phase 12 Progress

```text
[x] Step 1 — Custom Hooks
[x] Step 2 — Real-World Custom Hooks

[ ] Step 3 — Higher-Order Components (HOC)
[ ] Step 4 — Render Props
[ ] Step 5 — Compound Components
[ ] Step 6 — Feature-Based Folder Structure
[ ] Step 7 — Environment Variables
[ ] Step 8 — Error Boundaries
[ ] Step 9 — Production Architecture Concepts
```

# Next → Step 3: Higher-Order Components (HOC)

This is an older but still important React pattern that you may encounter in existing codebases.

-----------------------------------------------------------------------------------------------------------------------------------------

# Step 3 — Higher-Order Components (HOC)

Higher-Order Components are an older React pattern, but you may still encounter them in existing projects.

---

# 1. What Is a Higher-Order Component?

A Higher-Order Component is:

> A function that takes a component and returns a new enhanced component.

The pattern looks like:

```text
Component
   ↓
HOC
   ↓
Enhanced Component
```

Example:

```tsx
const EnhancedComponent = withSomething(Component);
```

---

# 2. Simple Mental Model

Imagine you have:

```text
User Component
```

You want to add authentication checking.

Instead of putting authentication logic inside every component:

```text
Dashboard
Profile
Settings
```

You create an HOC:

```text
withAuth()
```

Then:

```text
Dashboard ──→ withAuth ──→ Protected Dashboard

Profile ────→ withAuth ──→ Protected Profile

Settings ───→ withAuth ──→ Protected Settings
```

---

# 3. Basic HOC Example

Let's create an HOC called:

```text
withLoading
```

Its job:

```text
If loading = true
    ↓
Show Loading...

Otherwise
    ↓
Show original component
```

---

## `withLoading.tsx`

```tsx
import type { ComponentType } from "react";

type WithLoadingProps = {
    loading: boolean;
};

function withLoading<P>(
    WrappedComponent: ComponentType<P>
) {
    function WithLoadingComponent(
        props: P & WithLoadingProps
    ) {
        const { loading, ...rest } = props;

        if (loading) {
            return <p>Loading...</p>;
        }

        return (
            <WrappedComponent
                {...(rest as P)}
            />
        );
    }

    return WithLoadingComponent;
}

export default withLoading;
```

Don't worry too much about the TypeScript syntax here. Focus on the React pattern.

---

# 4. Create a Normal Component

## `UserList.tsx`

```tsx
type User = {
    id: number;
    name: string;
};

type UserListProps = {
    users: User[];
};

function UserList({
    users,
}: UserListProps) {
    return (
        <div>
            {users.map((user) => (
                <p key={user.id}>
                    {user.name}
                </p>
            ))}
        </div>
    );
}

export default UserList;
```

This is just a normal component.

---

# 5. Wrap It With the HOC

```tsx
import UserList from "./UserList";
import withLoading from "./withLoading";

const UserListWithLoading =
    withLoading(UserList);
```

Now:

```text
UserList
   ↓
withLoading()
   ↓
UserListWithLoading
```

---

# 6. Use the Enhanced Component

```tsx
function App() {
    const users = [
        {
            id: 1,
            name: "William",
        },
        {
            id: 2,
            name: "John",
        },
    ];

    return (
        <UserListWithLoading
            users={users}
            loading={false}
        />
    );
}
```

---

# 7. What Happens?

### When:

```tsx
loading={true}
```

The HOC returns:

```text
Loading...
```

### When:

```tsx
loading={false}
```

The HOC renders:

```tsx
<UserList users={users} />
```

Flow:

```text
UserListWithLoading

        ↓

Is loading true?

    YES         NO
     ↓           ↓

Loading...   UserList
```

---

# 8. Why Use an HOC?

The main purpose is:

```text
Reuse component behavior
```

Instead of repeating logic:

```text
Component A
├── Loading logic
└── UI

Component B
├── Loading logic
└── UI

Component C
├── Loading logic
└── UI
```

We can centralize it:

```text
withLoading()
     │
     ├── Component A
     ├── Component B
     └── Component C
```

---

# 9. Common HOC Examples

You might encounter:

```text
withAuth()
withLoading()
withPermission()
withTheme()
withLogger()
```

Example:

```tsx
const ProtectedDashboard =
    withAuth(Dashboard);
```

Conceptually:

```text
User
 ↓
Dashboard

BUT

withAuth
 ↓
Is user authenticated?

 YES → Dashboard
 NO  → Login page
```

---

# 10. Another Simple Example — `withLogger`

```tsx
import type { ComponentType } from "react";

function withLogger<P>(
    WrappedComponent: ComponentType<P>
) {
    return function WithLogger(props: P) {
        console.log(
            "Rendering component:",
            WrappedComponent.name
        );

        return <WrappedComponent {...props} />;
    };
}

export default withLogger;
```

Usage:

```tsx
const LoggedUserList =
    withLogger(UserList);
```

Now whenever the component renders:

```text
Rendering component: UserList
```

---

# 11. The Most Important Pattern

Remember this:

```tsx
function withSomething(
    WrappedComponent
) {
    return function EnhancedComponent(props) {
        // Extra logic here

        return (
            <WrappedComponent {...props} />
        );
    };
}
```

Mental model:

```text
HOC

Receives:
Component

Adds:
Extra behavior

Returns:
New Component
```

---

# 12. HOC vs Custom Hook

This is important.

| Custom Hook             | HOC                       |
| ----------------------- | ------------------------- |
| Reuses logic            | Enhances/wraps components |
| Used inside a component | Wraps a component         |
| Modern common pattern   | Older pattern, still seen |
| Example: `useFetch()`   | Example: `withAuth()`     |

### Custom Hook

```tsx
function UserList() {
    const { data } = useFetch();

    return (...);
}
```

### HOC

```tsx
const ProtectedUserList =
    withAuth(UserList);
```

---

# 13. HOC vs Component Wrapper

They may look similar.

Normal wrapper:

```tsx
<Auth>
    <Dashboard />
</Auth>
```

HOC:

```tsx
const ProtectedDashboard =
    withAuth(Dashboard);
```

Modern React often prefers:

```text
Custom Hooks
Composition
Context
```

over HOCs for many use cases.

But understanding HOCs is useful because older React projects and libraries may use them.

---

# Real-World Mental Model

```text
HOC = Component Transformer

Input:
<Component />

        ↓

withSomething()

        ↓

Output:
<EnhancedComponent />
```

---

# What You Should Remember

```text
Higher-Order Component

= Function

Takes:
Component

Returns:
New Component
```

Pattern:

```tsx
const EnhancedComponent =
    withSomething(Component);
```

Common examples:

```text
withAuth()
withLoading()
withPermission()
```

---

# Phase 12 Progress

```text
[x] Step 1 — Custom Hooks
[x] Step 2 — Real-World Custom Hooks
[x] Step 3 — Higher-Order Components (HOC)

[ ] Step 4 — Render Props
[ ] Step 5 — Compound Components
[ ] Step 6 — Feature-Based Folder Structure
[ ] Step 7 — Environment Variables
[ ] Step 8 — Error Boundaries
[ ] Step 9 — Production Architecture Concepts
```

# Next → Step 4: Render Props

Render Props is another important older React pattern. It is conceptually related to HOCs, but solves reusable logic in a different way.

-----------------------------------------------------------------------------------------------------------------------------------------

# Step 4 — Render Props

Render Props is another older React pattern for sharing reusable logic between components.

You may not use it often in modern React, but you may encounter it in older codebases.

---

# 1. What Is a Render Prop?

A Render Prop is:

> A component receives a function as a prop and calls that function to decide what UI to render.

The important part is:

```text
Function passed as a prop
        ↓
Component calls that function
        ↓
Function returns UI
```

---

# 2. Simple Example

Imagine we want reusable mouse position logic.

Normally:

```tsx
function MouseTracker() {
    return (
        <div>
            {/* UI */}
        </div>
    );
}
```

But with Render Props:

```tsx
<MouseTracker
    render={(mousePosition) => (
        <p>
            X: {mousePosition.x}
            Y: {mousePosition.y}
        </p>
    )}
/>
```

The parent provides the UI.

The `MouseTracker` component provides the logic.

---

# 3. Basic Mental Model

```text
Render Props Component
│
├── Contains reusable logic
│
└── Calls render function
        │
        ↓
     Returns UI
```

So:

```text
Logic
↓
Reusable Component

UI
↓
Provided through a function
```

---

# 4. Simple Counter Example

Let's create a reusable component.

## `Counter.tsx`

```tsx
import { useState } from "react";

type CounterProps = {
    render: (
        count: number,
        increase: () => void
    ) => React.ReactNode;
};

function Counter({ render }: CounterProps) {
    const [count, setCount] = useState(0);

    function increase() {
        setCount(count + 1);
    }

    return (
        <>
            {render(count, increase)}
        </>
    );
}

export default Counter;
```

---

# 5. What Does This Component Do?

It contains:

```text
State
+
Logic
```

Specifically:

```tsx
const [count, setCount] = useState(0);
```

and:

```tsx
function increase() {
    setCount(count + 1);
}
```

But it does NOT decide the UI.

Instead:

```tsx
render(count, increase)
```

asks the parent:

> "You decide how to display this."

---

# 6. Using the Render Prop

```tsx
import Counter from "./Counter";

function App() {
    return (
        <Counter
            render={(count, increase) => (
                <div>
                    <h1>Count: {count}</h1>

                    <button onClick={increase}>
                        Increase
                    </button>
                </div>
            )}
        />
    );
}
```

---

# 7. What Is Happening?

Step by step:

```text
App
 │
 │ passes a function
 ▼

render={(count, increase) => (...)}

 │
 ▼

Counter receives function

 │
 ▼

Counter manages state

 │
 ▼

Counter calls:

render(count, increase)

 │
 ▼

Function returns UI
```

---

# 8. Same Logic, Different UI

This is the main advantage.

You can reuse the same `Counter` logic but display it differently.

### Example 1

```tsx
<Counter
    render={(count, increase) => (
        <button onClick={increase}>
            Count: {count}
        </button>
    )}
/>
```

### Example 2

```tsx
<Counter
    render={(count, increase) => (
        <div>
            <h1>{count}</h1>

            <button onClick={increase}>
                Add
            </button>
        </div>
    )}
/>
```

Same logic:

```text
Counter state
+
increase function
```

Different UI.

---

# 9. Another Syntax: `children` as a Function

Sometimes Render Props uses `children`.

Instead of:

```tsx
<Counter
    render={(count, increase) => (
        ...
    )}
/>
```

You may see:

```tsx
<Counter>
    {(count, increase) => (
        <div>
            <h1>{count}</h1>

            <button onClick={increase}>
                Increase
            </button>
        </div>
    )}
</Counter>
```

Then the component:

```tsx
type CounterProps = {
    children: (
        count: number,
        increase: () => void
    ) => React.ReactNode;
};

function Counter({
    children,
}: CounterProps) {
    const [count, setCount] = useState(0);

    function increase() {
        setCount(count + 1);
    }

    return (
        <>
            {children(count, increase)}
        </>
    );
}
```

This is also Render Props.

---

# 10. The Important Difference

Normal `children`:

```tsx
<Card>
    <h1>Hello</h1>
</Card>
```

Here `children` is UI.

Render Prop `children`:

```tsx
<Counter>
    {(count) => <h1>{count}</h1>}
</Counter>
```

Here `children` is a function that returns UI.

---

# 11. HOC vs Render Props vs Custom Hooks

This is the important comparison.

| Pattern      | How It Reuses Logic          |
| ------------ | ---------------------------- |
| Custom Hook  | Hook function                |
| HOC          | Wraps component              |
| Render Props | Passes function returning UI |

### Custom Hook

```tsx
const { count, increase } = useCounter();
```

### HOC

```tsx
const EnhancedComponent =
    withCounter(Component);
```

### Render Props

```tsx
<Counter
    render={(count, increase) => (...)}
/>
```

---

# 12. Which One Is Preferred Today?

Modern React generally prefers:

```text
Custom Hooks
```

Why?

Because they are usually simpler and easier to read.

Instead of:

```text
HOC
↓
Component wrapping

Render Props
↓
Nested functions

Custom Hooks
↓
Direct reusable logic
```

Example:

```tsx
function Counter() {
    const {
        count,
        increase,
    } = useCounter();

    return (
        <button onClick={increase}>
            {count}
        </button>
    );
}
```

Very simple.

---

# 13. Why Learn Render Props Then?

Because you may encounter code like this:

```tsx
<DataProvider
    render={(data) => (
        <UserList users={data} />
    )}
/>
```

Or:

```tsx
<MouseTracker>
    {(position) => (
        <p>
            {position.x}, {position.y}
        </p>
    )}
</MouseTracker>
```

You should be able to understand it.

---

# Complete Mental Model

```text
RENDER PROPS

Reusable Component
        │
        ├── State
        ├── Logic
        │
        ▼
Calls Function Prop
        │
        ▼
Parent decides UI
```

---

# What You Should Remember

```text
Render Props

=
A function prop that returns UI
```

Pattern:

```tsx
<Component
    render={(data) => (
        <UI />
    )}
/>
```

The component provides:

```text
Logic + Data
```

The parent provides:

```text
UI
```

---

# Phase 12 Progress

```text
[x] Step 1 — Custom Hooks
[x] Step 2 — Real-World Custom Hooks
[x] Step 3 — Higher-Order Components (HOC)
[x] Step 4 — Render Props

[ ] Step 5 — Compound Components
[ ] Step 6 — Feature-Based Folder Structure
[ ] Step 7 — Environment Variables
[ ] Step 8 — Error Boundaries
[ ] Step 9 — Production Architecture Concepts
```

# Next → Step 5: Compound Components

This is a more modern and useful React pattern used in components like Tabs, Modals, Menus, and Accordions.

----------------------------------------------------------------------------------------------------------------------------------------

# Step 4 — Render Props

Render Props is another older React pattern for sharing reusable logic between components.

You may not use it often in modern React, but you may encounter it in older codebases.

---

# 1. What Is a Render Prop?

A Render Prop is:

> A component receives a function as a prop and calls that function to decide what UI to render.

The important part is:

```text
Function passed as a prop
        ↓
Component calls that function
        ↓
Function returns UI
```

---

# 2. Simple Example

Imagine we want reusable mouse position logic.

Normally:

```tsx
function MouseTracker() {
    return (
        <div>
            {/* UI */}
        </div>
    );
}
```

But with Render Props:

```tsx
<MouseTracker
    render={(mousePosition) => (
        <p>
            X: {mousePosition.x}
            Y: {mousePosition.y}
        </p>
    )}
/>
```

The parent provides the UI.

The `MouseTracker` component provides the logic.

---

# 3. Basic Mental Model

```text
Render Props Component
│
├── Contains reusable logic
│
└── Calls render function
        │
        ↓
     Returns UI
```

So:

```text
Logic
↓
Reusable Component

UI
↓
Provided through a function
```

---

# 4. Simple Counter Example

Let's create a reusable component.

## `Counter.tsx`

```tsx
import { useState } from "react";

type CounterProps = {
    render: (
        count: number,
        increase: () => void
    ) => React.ReactNode;
};

function Counter({ render }: CounterProps) {
    const [count, setCount] = useState(0);

    function increase() {
        setCount(count + 1);
    }

    return (
        <>
            {render(count, increase)}
        </>
    );
}

export default Counter;
```

---

# 5. What Does This Component Do?

It contains:

```text
State
+
Logic
```

Specifically:

```tsx
const [count, setCount] = useState(0);
```

and:

```tsx
function increase() {
    setCount(count + 1);
}
```

But it does NOT decide the UI.

Instead:

```tsx
render(count, increase)
```

asks the parent:

> "You decide how to display this."

---

# 6. Using the Render Prop

```tsx
import Counter from "./Counter";

function App() {
    return (
        <Counter
            render={(count, increase) => (
                <div>
                    <h1>Count: {count}</h1>

                    <button onClick={increase}>
                        Increase
                    </button>
                </div>
            )}
        />
    );
}
```

---

# 7. What Is Happening?

Step by step:

```text
App
 │
 │ passes a function
 ▼

render={(count, increase) => (...)}

 │
 ▼

Counter receives function

 │
 ▼

Counter manages state

 │
 ▼

Counter calls:

render(count, increase)

 │
 ▼

Function returns UI
```

---

# 8. Same Logic, Different UI

This is the main advantage.

You can reuse the same `Counter` logic but display it differently.

### Example 1

```tsx
<Counter
    render={(count, increase) => (
        <button onClick={increase}>
            Count: {count}
        </button>
    )}
/>
```

### Example 2

```tsx
<Counter
    render={(count, increase) => (
        <div>
            <h1>{count}</h1>

            <button onClick={increase}>
                Add
            </button>
        </div>
    )}
/>
```

Same logic:

```text
Counter state
+
increase function
```

Different UI.

---

# 9. Another Syntax: `children` as a Function

Sometimes Render Props uses `children`.

Instead of:

```tsx
<Counter
    render={(count, increase) => (
        ...
    )}
/>
```

You may see:

```tsx
<Counter>
    {(count, increase) => (
        <div>
            <h1>{count}</h1>

            <button onClick={increase}>
                Increase
            </button>
        </div>
    )}
</Counter>
```

Then the component:

```tsx
type CounterProps = {
    children: (
        count: number,
        increase: () => void
    ) => React.ReactNode;
};

function Counter({
    children,
}: CounterProps) {
    const [count, setCount] = useState(0);

    function increase() {
        setCount(count + 1);
    }

    return (
        <>
            {children(count, increase)}
        </>
    );
}
```

This is also Render Props.

---

# 10. The Important Difference

Normal `children`:

```tsx
<Card>
    <h1>Hello</h1>
</Card>
```

Here `children` is UI.

Render Prop `children`:

```tsx
<Counter>
    {(count) => <h1>{count}</h1>}
</Counter>
```

Here `children` is a function that returns UI.

---

# 11. HOC vs Render Props vs Custom Hooks

This is the important comparison.

| Pattern      | How It Reuses Logic          |
| ------------ | ---------------------------- |
| Custom Hook  | Hook function                |
| HOC          | Wraps component              |
| Render Props | Passes function returning UI |

### Custom Hook

```tsx
const { count, increase } = useCounter();
```

### HOC

```tsx
const EnhancedComponent =
    withCounter(Component);
```

### Render Props

```tsx
<Counter
    render={(count, increase) => (...)}
/>
```

---

# 12. Which One Is Preferred Today?

Modern React generally prefers:

```text
Custom Hooks
```

Why?

Because they are usually simpler and easier to read.

Instead of:

```text
HOC
↓
Component wrapping

Render Props
↓
Nested functions

Custom Hooks
↓
Direct reusable logic
```

Example:

```tsx
function Counter() {
    const {
        count,
        increase,
    } = useCounter();

    return (
        <button onClick={increase}>
            {count}
        </button>
    );
}
```

Very simple.

---

# 13. Why Learn Render Props Then?

Because you may encounter code like this:

```tsx
<DataProvider
    render={(data) => (
        <UserList users={data} />
    )}
/>
```

Or:

```tsx
<MouseTracker>
    {(position) => (
        <p>
            {position.x}, {position.y}
        </p>
    )}
</MouseTracker>
```

You should be able to understand it.

---

# Complete Mental Model

```text
RENDER PROPS

Reusable Component
        │
        ├── State
        ├── Logic
        │
        ▼
Calls Function Prop
        │
        ▼
Parent decides UI
```

---

# What You Should Remember

```text
Render Props

=
A function prop that returns UI
```

Pattern:

```tsx
<Component
    render={(data) => (
        <UI />
    )}
/>
```

The component provides:

```text
Logic + Data
```

The parent provides:

```text
UI
```

---

# Phase 12 Progress

```text
[x] Step 1 — Custom Hooks
[x] Step 2 — Real-World Custom Hooks
[x] Step 3 — Higher-Order Components (HOC)
[x] Step 4 — Render Props

[ ] Step 5 — Compound Components
[ ] Step 6 — Feature-Based Folder Structure
[ ] Step 7 — Environment Variables
[ ] Step 8 — Error Boundaries
[ ] Step 9 — Production Architecture Concepts
```

# Next → Step 5: Compound Components

This is a more modern and useful React pattern used in components like Tabs, Modals, Menus, and Accordions.

------------------------------------------------------------------------------------------------------------------------------------------


# Step 5 — Compound Components

Compound Components are a powerful React pattern used for building flexible and reusable UI components.


# First: What is the actual idea?

Think about a simple website navigation.

```text
Navbar
├── Home
├── About
└── Contact
```

All these parts belong to one system.

Similarly, Tabs:

```text
Tabs
├── Profile Button
├── Settings Button
├── Profile Content
└── Settings Content
```

These components are separate, but they need to communicate with each other.

That is the main reason for Compound Components.

---

# Let's use a very simple example: Tabs

We want this:

```text
[ Profile ] [ Settings ]

Profile Content Here
```

When clicking Settings:

```text
[ Profile ] [ Settings ]

Settings Content Here
```

So we have multiple components working together.

---

# Step 1: The Parent Component Holds the State

The most important thing is this:

```tsx
function Tabs() {
    const [activeTab, setActiveTab] = useState("profile");

    // ...
}
```

This means:

```text
Tabs remembers:

Which tab is currently active?
```

Initially:

```text
activeTab = "profile"
```

---

# Step 2: Child Components Need That State

We have separate components:

```text
Tabs
Tabs.Tab
Tabs.Panel
```

The `Tab` component needs to know:

```text
Which tab is active?
How can I change the active tab?
```

The `Panel` component needs to know:

```text
Which tab is active?
```

Example:

```text
Tabs
 │
 │ activeTab = "profile"
 │
 ├── Tab
 │
 └── Panel
```

So all children need access to the same state.

This is where **Context** comes in.

---

# Think of Context Like a Shared Information Box

```text
Tabs Component

Shared Box:
{
    activeTab,
    setActiveTab
}
```

All child components can access this shared box.

```text
             Tabs
               │
        Shared Context
               │
        ┌──────┴──────┐
        ↓             ↓
      Tab          Panel
```

---

# Let's Make It Extremely Simple

## Parent

```tsx
function Tabs({ children }) {
    const [activeTab, setActiveTab] = useState("profile");

    return (
        <TabsContext.Provider
            value={{
                activeTab,
                setActiveTab
            }}
        >
            {children}
        </TabsContext.Provider>
    );
}
```

Don't focus on all syntax.

Just understand:

```text
Tabs owns the state

activeTab
setActiveTab
```

And shares it with its children.

---

# Tab Component

Imagine this:

```tsx
function Tab({ value, children }) {
    const {
        activeTab,
        setActiveTab
    } = useContext(TabsContext);

    return (
        <button
            onClick={() => setActiveTab(value)}
        >
            {children}
        </button>
    );
}
```

Suppose:

```tsx
<Tabs.Tab value="profile">
    Profile
</Tabs.Tab>
```

When clicked:

```text
setActiveTab("profile")
```

The shared state changes.

---

# Panel Component

```tsx
function Panel({ value, children }) {
    const { activeTab } =
        useContext(TabsContext);

    if (activeTab !== value) {
        return null;
    }

    return <div>{children}</div>;
}
```

Suppose:

```tsx
<Tabs.Panel value="profile">
    Profile Content
</Tabs.Panel>
```

It checks:

```text
activeTab === "profile" ?
```

If yes:

```text
Show content
```

If no:

```text
Hide content
```

---

# Now Let's See the Full Flow

Your JSX:

```tsx
<Tabs>

    <Tabs.Tab value="profile">
        Profile
    </Tabs.Tab>

    <Tabs.Tab value="settings">
        Settings
    </Tabs.Tab>


    <Tabs.Panel value="profile">
        Profile Content
    </Tabs.Panel>

    <Tabs.Panel value="settings">
        Settings Content
    </Tabs.Panel>

</Tabs>
```

---

## Initially

Parent state:

```text
activeTab = "profile"
```

Now each panel checks:

### Profile Panel

```text
value = "profile"

activeTab = "profile"

MATCH
↓
Show Profile Content
```

### Settings Panel

```text
value = "settings"

activeTab = "profile"

NOT MATCH
↓
Hide Settings Content
```

---

# User Clicks Settings

This button:

```tsx
<Tabs.Tab value="settings">
    Settings
</Tabs.Tab>
```

runs:

```text
setActiveTab("settings")
```

Now:

```text
activeTab = "settings"
```

React re-renders.

---

### Profile Panel Checks Again

```text
"profile" === "settings"

FALSE

↓
Hide
```

### Settings Panel Checks Again

```text
"settings" === "settings"

TRUE

↓
Show
```

That's literally the whole functionality.

---

# But Where Does "Compound" Come From?

Because we are creating one component system:

```text
Tabs
├── Tabs.Tab
├── Tabs.Panel
```

Instead of unrelated components:

```text
<Tab />
<Panel />
```

We group them together:

```tsx
<Tabs>
    <Tabs.Tab />
    <Tabs.Panel />
</Tabs>
```

Think of it like JavaScript objects:

```js
Tabs.Tab
Tabs.Panel
```

They belong to the `Tabs` system.

---

# Real-Life Analogy

Think about a Car.

```text
Car
├── Car.Engine
├── Car.Wheel
├── Car.Door
└── Car.Seat
```

All parts belong to one system.

Similarly:

```text
Tabs
├── Tabs.Tab
└── Tabs.Panel
```

All components work together as one UI system.

---

# The One-Sentence Definition

> **Compound Components are multiple related components that work together by sharing state, usually through Context.**

---

# Don't Memorize the Complex Code

For now, remember only this:

```text
Parent Component
       │
       │ owns shared state
       ↓
Context
       ↓
Child Components use the shared state
```

And:

```text
<Tabs>
    <Tabs.Tab />
    <Tabs.Panel />
</Tabs>
```

This structure is called the **Compound Component Pattern**.

---


# Next → Step 6: Feature-Based Folder Structure

This is an important real-world topic. We'll learn how to organize a growing React application properly instead of putting everything into one `components` folder.

------------------------------------------------------------------------------------------------------------------------------------------

# Step 6 — Feature-Based Folder Structure

This is a very important real-world React topic.

So far, you may have seen a project structured like this:

```text
src/
├── components/
├── pages/
├── hooks/
├── utils/
└── services/
```

This works for small projects.

But what happens when the application becomes large?

---

# 1. The Problem With Traditional Folder Structure

Imagine an e-commerce application:

```text
src/
├── components/
│   ├── LoginForm.tsx
│   ├── ProductCard.tsx
│   ├── CartItem.tsx
│   ├── UserProfile.tsx
│   └── OrderList.tsx
│
├── hooks/
│   ├── useAuth.ts
│   ├── useCart.ts
│   └── useProducts.ts
│
├── services/
│   ├── authApi.ts
│   ├── productApi.ts
│   └── orderApi.ts
│
├── types/
│   ├── user.ts
│   ├── product.ts
│   └── order.ts
```

Notice the problem.

Everything related to **authentication** is spread across multiple folders.

```text
LoginForm
    → components/

useAuth
    → hooks/

authApi
    → services/

User type
    → types/
```

As the project grows, finding related code becomes harder.

---

# 2. Feature-Based Folder Structure

Instead, we organize files by feature.

Example:

```text
src/
├── features/
│   ├── auth/
│   ├── products/
│   ├── cart/
│   └── orders/
```

Each feature contains everything related to that feature.

---

# 3. Example: Auth Feature

```text
src/
├── features/
│   └── auth/
│       ├── components/
│       │   └── LoginForm.tsx
│       │
│       ├── hooks/
│       │   └── useAuth.ts
│       │
│       ├── services/
│       │   └── authApi.ts
│       │
│       ├── types.ts
│       └── index.ts
```

Now everything related to authentication is in one place.

```text
AUTH FEATURE

auth/
├── LoginForm
├── useAuth
├── authApi
└── types
```

---

# 4. Product Feature

```text
src/
├── features/
│   └── products/
│       ├── components/
│       │   ├── ProductCard.tsx
│       │   └── ProductList.tsx
│       │
│       ├── hooks/
│       │   └── useProducts.ts
│       │
│       ├── services/
│       │   └── productApi.ts
│       │
│       └── types.ts
```

Everything related to products stays together.

---

# 5. Complete Project Structure

A real-world application might look like this:

```text
src/
│
├── app/
│   ├── App.tsx
│   └── routes.tsx
│
├── features/
│   │
│   ├── auth/
│   │   ├── components/
│   │   ├── hooks/
│   │   ├── services/
│   │   ├── types.ts
│   │   └── index.ts
│   │
│   ├── products/
│   │   ├── components/
│   │   ├── hooks/
│   │   ├── services/
│   │   └── types.ts
│   │
│   └── cart/
│       ├── components/
│       ├── hooks/
│       └── types.ts
│
├── components/
│   └── Button.tsx
│
├── hooks/
│   └── useLocalStorage.ts
│
├── utils/
│   └── formatDate.ts
│
└── main.tsx
```

---

# 6. Important Question: Why Do We Still Have a Global `components` Folder?

Because some components are used everywhere.

Example:

```text
Button
Input
Modal
Loader
Navbar
```

These are not specifically related to:

```text
Auth
Products
Cart
```

They are shared components.

So:

```text
src/
├── components/
│   ├── Button.tsx
│   ├── Input.tsx
│   └── Modal.tsx
│
└── features/
    ├── auth/
    ├── products/
    └── cart/
```

---

# 7. Feature-Specific vs Shared Components

This is important.

### Feature-specific component

```text
ProductCard
```

Used only for products.

Place it here:

```text
features/products/components/ProductCard.tsx
```

### Shared component

```text
Button
```

Used everywhere.

Place it here:

```text
components/Button.tsx
```

---

# 8. Simple Rule

Ask yourself:

> "Is this code related to one specific feature?"

### Yes

Put it inside the feature.

```text
features/auth/
features/products/
features/cart/
```

### No, used globally

Put it in shared folders.

```text
components/
hooks/
utils/
```

---

# 9. Real Example

Imagine you're building an application with:

```text
Login
Products
Cart
Orders
```

Traditional structure:

```text
components/
├── LoginForm
├── ProductCard
├── CartItem
└── OrderList
```

Everything is mixed.

Feature-based structure:

```text
features/
├── auth/
│   └── LoginForm
│
├── products/
│   └── ProductCard
│
├── cart/
│   └── CartItem
│
└── orders/
    └── OrderList
```

Much easier to understand.

---

# 10. Why Is This Better?

Suppose your manager says:

> "We need to modify the authentication system."

You immediately go to:

```text
features/auth/
```

Everything related to authentication is there.

```text
features/auth/
├── components
├── hooks
├── services
└── types
```

You don't need to search the entire project.

---

# 11. Feature Folder Mental Model

Think of each feature as a small application.

```text
features/
│
├── auth/
│   ├── UI
│   ├── Logic
│   ├── API
│   └── Types
│
├── products/
│   ├── UI
│   ├── Logic
│   ├── API
│   └── Types
```

Each feature keeps its related code together.

---

# 12. What Is `index.ts`?

You may see:

```text
auth/
├── components/
├── hooks/
└── index.ts
```

Example:

```ts
export { default as LoginForm } from "./components/LoginForm";
export { default as useAuth } from "./hooks/useAuth";
```

Then other files can import:

```ts
import {
    LoginForm,
    useAuth,
} from "@/features/auth";
```

Instead of:

```ts
import LoginForm from
"@/features/auth/components/LoginForm";
```

This is called a **barrel export**.

Don't worry too much about this for now.

---

# 13. Traditional vs Feature-Based

| Traditional           | Feature-Based                |
| --------------------- | ---------------------------- |
| Organize by file type | Organize by business feature |
| Components together   | Feature code together        |
| Harder in large apps  | Easier to scale              |
| `components/`         | `features/auth/`             |
| `hooks/`              | `features/products/hooks/`   |

---

# The Most Important Concept

Traditional structure asks:

```text
What TYPE of file is this?

Component?
Hook?
Service?
```

Feature-based structure asks:

```text
What FEATURE does this belong to?

Auth?
Products?
Cart?
Orders?
```

That is the biggest difference.

---

# What You Should Remember

```text
Feature-Based Architecture

Organize code by feature,
not only by file type.
```

Example:

```text
features/
├── auth/
│   ├── components/
│   ├── hooks/
│   └── services/
│
├── products/
│   ├── components/
│   ├── hooks/
│   └── services/
```

---

# Phase 12 Progress

```text
[x] Step 1 — Custom Hooks
[x] Step 2 — Real-World Custom Hooks
[x] Step 3 — Higher-Order Components
[x] Step 4 — Render Props
[x] Step 5 — Compound Components
[x] Step 6 — Feature-Based Folder Structure

[ ] Step 7 — Environment Variables
[ ] Step 8 — Error Boundaries
[ ] Step 9 — Production Architecture Concepts
```

# Next → Step 7: Environment Variables

This is important for real applications because it teaches how to store API URLs, keys, and environment-specific configuration safely.

---------------------------------------------------------------------------------------------------------------------------------------

# Step 7 — Environment Variables

This is an important real-world React concept.

The main idea is simple:

> **Environment variables let us keep configuration values outside our normal source code.**

For example:

```text
API URL
Environment name
Feature flags
Public configuration
```

---

# 1. The Problem

Imagine you write this directly in your React code:

```tsx
const API_URL =
    "https://api.myapp.com";
```

You might have different environments:

```text
Development
↓
http://localhost:5000


Production
↓
https://api.myapp.com
```

You don't want to manually change your code every time.

Instead:

```text
Environment Variable
        ↓
API URL
        ↓
Application
```

---

# 2. What Is an Environment Variable?

Think of it as a named configuration value.

For example:

```text
API_URL = https://api.myapp.com
```

Then your application reads:

```text
API_URL
```

instead of hardcoding the URL everywhere.

---

# 3. Why Do We Need Them?

Suppose your application uses:

```text
Development API
https://dev-api.example.com

Production API
https://api.example.com
```

You can have:

```text
Development
    ↓
Development environment variable


Production
    ↓
Production environment variable
```

Your React code stays the same.

---

# 4. Vite Example

Since modern React projects commonly use Vite, you'll often see:

```text
.env
```

For example:

```text
VITE_API_URL=https://api.example.com
```

The `VITE_` prefix is important in Vite.

---

# 5. Reading the Variable

In React:

```tsx
const apiUrl =
    import.meta.env.VITE_API_URL;
```

Now:

```text
VITE_API_URL
      ↓
import.meta.env
      ↓
React application
```

For example:

```tsx
fetch(`${apiUrl}/users`);
```

---

# 6. Development Environment

You might have:

```text
.env.development
```

with:

```text
VITE_API_URL=http://localhost:5000
```

So during development:

```text
React
 ↓
VITE_API_URL
 ↓
http://localhost:5000
```

---

# 7. Production Environment

You might have:

```text
.env.production
```

with:

```text
VITE_API_URL=https://api.myapp.com
```

When building for production:

```text
React
 ↓
VITE_API_URL
 ↓
https://api.myapp.com
```

The React code doesn't need to change.

---

# 8. Important Security Point

This is VERY important.

In a frontend React application:

> **Environment variables are NOT automatically secret.**

If you write:

```text
VITE_API_KEY=abc123
```

and use it in your React application, that value can potentially end up in the browser bundle.

A user can inspect the application.

So **never put true secrets in frontend environment variables**, such as:

```text
Database password
Private API secret
Server credentials
Private tokens
```

---

# 9. What Can Be Public?

Things such as:

```text
API base URL
Public configuration
Public service identifiers
Frontend feature flags
```

can often be exposed.

Example:

```text
VITE_API_URL=https://api.example.com
```

That's generally fine if the API URL is meant to be public.

---

# 10. Where Should Secrets Go?

Secrets belong on the backend.

```text
React Frontend
      ↓
Backend API
      ↓
Secret API key
      ↓
External service
```

Not:

```text
React
 ↓
Secret API key
```

Because the browser is controlled by the user.

---

# 11. `.env` Files

You may see:

```text
.env
.env.development
.env.production
```

A project could look like:

```text
project/
│
├── src/
│
├── .env
├── .env.development
├── .env.production
│
├── package.json
└── vite.config.ts
```

---

# 12. `.gitignore`

Usually, projects configure Git so that certain `.env` files aren't committed.

For example:

```text
.env
```

may be included in:

```text
.gitignore
```

Why?

Because developers might accidentally put sensitive configuration there.

But remember:

> **Putting something in `.env` does not magically make it secret.**

Especially in frontend applications.

---

# 13. Full Example

Suppose you have:

```text
.env.development
```

```text
VITE_API_URL=http://localhost:5000
```

And:

```text
.env.production
```

```text
VITE_API_URL=https://api.example.com
```

Your React code:

```tsx
const API_URL =
    import.meta.env.VITE_API_URL;

async function getUsers() {
    const response =
        await fetch(`${API_URL}/users`);

    return response.json();
}
```

Development:

```text
API_URL
   ↓
http://localhost:5000
   ↓
/users
```

Production:

```text
API_URL
   ↓
https://api.example.com
   ↓
/users
```

Same React code.

Different configuration.

---

# 14. Why This Is Useful in Real Projects

You may have:

```text
Development
Staging
Production
```

Each environment could have different:

```text
API URL
Backend URL
Feature settings
Service endpoints
```

Instead of changing source code:

```text
Environment
     ↓
Configuration
     ↓
Application
```

---

# 15. React Environment Variables Mental Model

Remember this:

```text
.env
  ↓
Configuration
  ↓
Build Tool
  ↓
React Application
```

With Vite:

```text
VITE_API_URL
      ↓
import.meta.env.VITE_API_URL
```

---

# 16. One Important Difference

You may see this in old React projects:

```text
REACT_APP_API_URL
```

That's associated with Create React App.

With Vite, you generally see:

```text
VITE_API_URL
```

And access it with:

```text
import.meta.env.VITE_API_URL
```

So don't mix them up.

---

# What You Should Remember

```text
Environment Variable
        =
Configuration outside normal source code
```

Vite:

```text
VITE_API_URL=...
```

Read it:

```text
import.meta.env.VITE_API_URL
```

Main purpose:

```text
Different configuration
for different environments
```

And most importantly:

```text
Frontend .env ≠ Secret storage
```

Never assume a frontend environment variable is private.

---

# Phase 12 Progress

```text
[x] Step 1 — Custom Hooks
[x] Step 2 — Real-World Custom Hooks
[x] Step 3 — Higher-Order Components
[x] Step 4 — Render Props
[x] Step 5 — Compound Components
[x] Step 6 — Feature-Based Folder Structure
[x] Step 7 — Environment Variables

[ ] Step 8 — Error Boundaries
[ ] Step 9 — Production Architecture Concepts
```

# Next → Step 8: Error Boundaries

We'll learn what happens when a React component crashes and how real applications prevent one broken component from taking down the entire UI.

-----------------------------------------------------------------------------------------------------------------------------------------

# Step 8 — Error Boundaries

Error Boundaries are used to prevent one broken React component from crashing the entire application.

---

# 1. The Problem

Imagine this application:

```text
App
│
├── Navbar
├── Sidebar
├── Dashboard
│
└── UserProfile ❌ ERROR
```

Suppose `UserProfile` crashes because of a bug.

Without proper error handling:

```text
UserProfile crashes
      ↓
React application may break
      ↓
User sees broken/blank UI
```

We want something better.

---

# 2. The Solution: Error Boundary

An Error Boundary acts like a safety wall.

```text
App
│
├── Navbar
├── Sidebar
│
└── Error Boundary
       │
       ├── Dashboard
       └── UserProfile ❌
              ↓
         Catch Error
              ↓
       Show fallback UI
```

Instead of the whole application breaking:

```text
Something went wrong.
Please try again.
```

---

# 3. Simple Mental Model

Think of an Error Boundary like `try/catch` for parts of the React UI.

JavaScript:

```text
try
 ↓
Run code

catch
 ↓
Handle error
```

React:

```text
Error Boundary
 ↓
Render components

Error happens
 ↓
Show fallback UI
```

It is not exactly the same as `try/catch`, but the idea is similar.

---

# 4. Example of a Broken Component

```tsx
function BrokenComponent() {
    throw new Error("Something went wrong");

    return <h1>Hello</h1>;
}
```

This component intentionally crashes.

If we render:

```tsx
function App() {
    return (
        <div>
            <h1>My App</h1>

            <BrokenComponent />
        </div>
    );
}
```

We have an error.

---

# 5. Creating an Error Boundary

Traditionally, Error Boundaries use a class component.

```tsx
import React from "react";

type Props = {
    children: React.ReactNode;
};

type State = {
    hasError: boolean;
};

class ErrorBoundary extends React.Component<
    Props,
    State
> {
    state: State = {
        hasError: false,
    };

    static getDerivedStateFromError() {
        return {
            hasError: true,
        };
    }

    componentDidCatch(error: Error) {
        console.error(error);
    }

    render() {
        if (this.state.hasError) {
            return (
                <h1>
                    Something went wrong.
                </h1>
            );
        }

        return this.props.children;
    }
}

export default ErrorBoundary;
```

Don't worry about memorizing this entire code.

Let's understand the idea.

---

# 6. What Does the Error Boundary Do?

Initially:

```text
hasError = false
```

So it renders:

```tsx
this.props.children
```

Meaning:

```text
Render the components inside it.
```

Example:

```tsx
<ErrorBoundary>
    <Dashboard />
</ErrorBoundary>
```

Initially:

```text
ErrorBoundary
      ↓
hasError = false
      ↓
Show Dashboard
```

---

# 7. What Happens When an Error Occurs?

React detects the error.

This runs:

```tsx
getDerivedStateFromError()
```

It changes:

```text
hasError = true
```

Then:

```tsx
if (this.state.hasError) {
    return <h1>Something went wrong.</h1>;
}
```

So instead of the broken component:

```text
Dashboard ❌
```

The user sees:

```text
Something went wrong.
```

---

# 8. Using the Error Boundary

```tsx
import ErrorBoundary from "./ErrorBoundary";
import Dashboard from "./Dashboard";

function App() {
    return (
        <ErrorBoundary>
            <Dashboard />
        </ErrorBoundary>
    );
}
```

Now:

```text
App
 ↓
ErrorBoundary
 ↓
Dashboard
```

If Dashboard crashes:

```text
Dashboard ❌
   ↓
Error Boundary catches it
   ↓
Fallback UI
```

---

# 9. Why Not Wrap Everything?

You can wrap the entire app:

```tsx
<ErrorBoundary>
    <App />
</ErrorBoundary>
```

But real applications often use multiple Error Boundaries.

Example:

```text
App
│
├── Navbar
│
├── ErrorBoundary
│     └── Dashboard
│
├── ErrorBoundary
│     └── UserProfile
│
└── Footer
```

Why?

Suppose `UserProfile` crashes.

Without separate boundaries:

```text
Entire application fallback
```

With separate boundaries:

```text
Navbar still works
Dashboard still works
Footer still works

Only UserProfile shows error
```

This provides a better user experience.

---

# 10. Real-World Example

```tsx
function App() {
    return (
        <div>
            <Navbar />

            <ErrorBoundary>
                <Dashboard />
            </ErrorBoundary>

            <ErrorBoundary>
                <UserProfile />
            </ErrorBoundary>
        </div>
    );
}
```

If:

```text
UserProfile crashes
```

Only:

```text
UserProfile area
```

shows:

```text
Something went wrong.
```

---

# 11. Important Limitation

Error Boundaries do NOT catch every error.

They generally catch errors during React rendering in their child component tree.

They do not automatically handle errors from:

```text
Event handlers
Async code
setTimeout
API requests
```

For example:

```tsx
async function getUsers() {
    const response = await fetch("/api/users");
}
```

For API errors, you still use:

```tsx
try {
    // API request
} catch (error) {
    // Handle API error
}
```

So:

```text
Error Boundary
↓
React rendering errors


try/catch
↓
Async/API errors
```

---

# 12. Error Boundary vs try/catch

| Error Boundary         | try/catch               |
| ---------------------- | ----------------------- |
| React rendering errors | JavaScript errors       |
| Protects UI sections   | Handles code execution  |
| Shows fallback UI      | Handles specific errors |

Example:

```text
Component crashes while rendering
        ↓
Error Boundary


API request fails
        ↓
try/catch
```

---

# 13. Modern Projects

Although React Error Boundaries traditionally require class components, modern projects often use libraries to make error handling easier.

For example, you may encounter libraries that provide reusable Error Boundary components.

But the core React concept remains:

```text
Error Boundary
     ↓
Catch rendering error
     ↓
Show fallback UI
```

---

# Complete Mental Model

```text
NORMAL FLOW

App
 ↓
Component renders
 ↓
Everything works


ERROR FLOW

App
 ↓
Component crashes ❌
 ↓
Error Boundary catches error
 ↓
Fallback UI appears
```

---

# What You Should Remember

```text
Error Boundary

=
A React component that catches rendering errors
in child components and displays fallback UI.
```

Simple usage:

```tsx
<ErrorBoundary>
    <Dashboard />
</ErrorBoundary>
```

If Dashboard crashes:

```text
Instead of:
Broken Application ❌

User sees:
Something went wrong. ✓
```

---

# Phase 12 Progress

```text
[x] Step 1 — Custom Hooks
[x] Step 2 — Real-World Custom Hooks
[x] Step 3 — Higher-Order Components
[x] Step 4 — Render Props
[x] Step 5 — Compound Components
[x] Step 6 — Feature-Based Folder Structure
[x] Step 7 — Environment Variables
[x] Step 8 — Error Boundaries

[ ] Step 9 — Production Architecture Concepts
```

# Next → Step 9: Production Architecture Concepts

This final step will connect everything together and show how a real React application is typically organized: components, features, API services, hooks, error handling, environment variables, and application structure.

------------------------------------------------------------------------------------------------------------------------------------------

# Step 9 — Production Architecture Concepts

This is the final step of Phase 12.

Here we'll connect everything you've learned and see how a **real React application** can be organized.

This is less about writing new React syntax and more about understanding:

> How should a large React project be structured?

---

# 1. The Big Picture

A real application might contain:

```text
React Application
│
├── Pages
├── Features
├── Components
├── Hooks
├── API Services
├── Context
├── Utilities
├── Environment Variables
└── Error Handling
```

Each part has a specific responsibility.

---

# 2. Example Real-World Project

Imagine an e-commerce application.

It has:

```text
Login
Products
Cart
Orders
Profile
```

A possible structure:

```text
src/
│
├── app/
│   ├── App.tsx
│   ├── routes.tsx
│   └── providers.tsx
│
├── features/
│   ├── auth/
│   ├── products/
│   ├── cart/
│   └── orders/
│
├── components/
│   ├── Button.tsx
│   ├── Input.tsx
│   ├── Modal.tsx
│   └── Loader.tsx
│
├── hooks/
│   ├── useLocalStorage.ts
│   └── useDebounce.ts
│
├── services/
│   └── api.ts
│
├── utils/
│   ├── formatDate.ts
│   └── formatCurrency.ts
│
├── types/
│
├── main.tsx
│
└── styles/
```

---

# 3. `main.tsx` — Application Entry Point

This is where React starts.

```text
Browser
   ↓
main.tsx
   ↓
<App />
```

Usually:

```tsx
createRoot(document.getElementById("root")!)
    .render(<App />);
```

Think of it as:

> The starting point of the React application.

---

# 4. `App.tsx` — Main Application

```text
main.tsx
   ↓
App.tsx
   ↓
Routes + Main Layout
```

Example:

```tsx
function App() {
    return (
        <>
            <Navbar />

            <Routes>
                {/* Pages */}
            </Routes>
        </>
    );
}
```

`App.tsx` should usually not contain all your business logic.

It should mostly organize the application.

---

# 5. Features — Business Logic

This is where most application-specific code lives.

```text
features/
│
├── auth/
├── products/
├── cart/
└── orders/
```

Example:

```text
features/products/
│
├── components/
│   ├── ProductCard.tsx
│   └── ProductList.tsx
│
├── hooks/
│   └── useProducts.ts
│
├── services/
│   └── productApi.ts
│
└── types.ts
```

Everything related to products stays together.

---

# 6. Shared Components

Some components are used across the whole application.

```text
components/
├── Button
├── Input
├── Modal
└── Loader
```

These components should generally be reusable.

Example:

```tsx
<Button>
    Save
</Button>
```

The Button shouldn't know anything about:

```text
Products
Cart
Authentication
Orders
```

It should just be a reusable UI component.

---

# 7. API Layer

A common mistake is putting API calls directly inside components.

### Not ideal:

```tsx
function UserList() {
    useEffect(() => {
        fetch("https://api.example.com/users");
    }, []);
}
```

Instead:

```text
Component
    ↓
Hook
    ↓
API Service
    ↓
Backend
```

Example:

### `api.ts`

```tsx
async function getUsers() {
    const response =
        await fetch("/users");

    return response.json();
}
```

Then:

```tsx
function useUsers() {
    // Call getUsers()
}
```

Then:

```tsx
function UserList() {
    const users = useUsers();

    // Display UI
}
```

---

# 8. Separation of Responsibilities

This is very important in real applications.

```text
Component
↓
Responsible for UI


Hook
↓
Responsible for reusable logic


Service
↓
Responsible for API communication


Utility
↓
Responsible for helper functions
```

Example:

```text
ProductList.tsx
      ↓
Displays products


useProducts.ts
      ↓
Fetches/manages product logic


productApi.ts
      ↓
Communicates with backend


formatCurrency.ts
      ↓
Formats price
```

Each file has one clear responsibility.

---

# 9. Complete Data Flow

Imagine the user opens the Products page.

```text
User
 ↓
Product Page
 ↓
ProductList Component
 ↓
useProducts Hook
 ↓
productApi Service
 ↓
Backend API
```

Then the response comes back:

```text
Backend API
 ↓
productApi
 ↓
useProducts
 ↓
ProductList
 ↓
User sees products
```

This separation makes applications easier to maintain.

---

# 10. Environment Variables

Your API configuration might be:

```text
.env
```

```text
VITE_API_URL=https://api.example.com
```

Your API service uses:

```text
import.meta.env.VITE_API_URL
```

Flow:

```text
Environment Variable
        ↓
API Service
        ↓
Backend URL
```

This means you don't hardcode URLs everywhere.

---

# 11. Error Handling

A real application should handle errors at different levels.

### API Error

```text
API request fails
↓
try/catch
↓
Show error message
```

### Component Rendering Error

```text
Component crashes
↓
Error Boundary
↓
Fallback UI
```

So:

```text
API Error
↓
try/catch


React Render Error
↓
Error Boundary
```

---

# 12. Loading States

Real applications must handle:

```text
Loading
Success
Error
```

Example:

```text
API Request
     ↓
Loading...
     ↓
Success → Show Data

OR

Error → Show Error Message
```

This pattern appears everywhere.

Example:

```tsx
if (loading) {
    return <Loader />;
}

if (error) {
    return <ErrorMessage />;
}

return <ProductList />;
```

---

# 13. Application Providers

Sometimes applications have multiple providers.

Example:

```tsx
<AuthProvider>
    <ThemeProvider>
        <App />
    </ThemeProvider>
</AuthProvider>
```

Instead of cluttering `main.tsx`, a project may use:

```text
app/providers.tsx
```

Example:

```tsx
function AppProviders({
    children,
}: {
    children: React.ReactNode;
}) {
    return (
        <AuthProvider>
            <ThemeProvider>
                {children}
            </ThemeProvider>
        </AuthProvider>
    );
}
```

Then:

```tsx
<AppProviders>
    <App />
</AppProviders>
```

This keeps application setup organized.

---

# 14. Real Application Architecture

Here's the complete picture:

```text
                    main.tsx
                       │
                       ↓
                  AppProviders
                       │
                       ↓
                    App.tsx
                       │
                       ↓
                    Routes
                       │
          ┌────────────┼────────────┐
          ↓            ↓            ↓
        Auth       Products        Cart
          │            │            │
          ↓            ↓            ↓
      Components   Components   Components
          │            │
          ↓            ↓
         Hooks        Hooks
          │            │
          ↓            ↓
       Services     Services
          │            │
          └──────┬─────┘
                 ↓
             Backend API
```

---

# 15. The Most Important Real-World Principle

Don't put everything inside one component.

Bad:

```text
ProductPage.tsx

├── API calls
├── useState
├── Business logic
├── Formatting
├── Authentication
├── UI
└── Error handling
```

Better:

```text
ProductPage
     ↓
UI

useProducts
     ↓
Logic

productApi
     ↓
API

formatCurrency
     ↓
Utility
```

---

# 16. Small vs Large Projects

You don't need this complex architecture for every project.

### Small Project

```text
src/
├── components/
├── App.tsx
└── main.tsx
```

Perfectly fine.

### Large Project

```text
src/
├── app/
├── features/
├── components/
├── hooks/
├── services/
├── utils/
└── types/
```

Use architecture based on project complexity.

> Don't over-engineer a small project.

---

# 17. Complete Mental Model

When building a React application, think:

```text
UI
↓
Components

Reusable Logic
↓
Hooks

Feature-specific Code
↓
Features

API Communication
↓
Services

Helper Functions
↓
Utils

Shared Application Setup
↓
App
```

---

# What You Should Remember

The most important architecture flow:

```text
Component
   ↓
Hook
   ↓
Service
   ↓
API
```

And project organization:

```text
src/
├── app/          → Application setup
├── features/     → Business features
├── components/   → Shared UI
├── hooks/        → Shared logic
├── services/     → API communication
├── utils/        → Helper functions
└── types/        → TypeScript types
```

---

# Phase 12 Complete 🎉

```text
PHASE 12 — ADVANCED REACT / REAL-WORLD ARCHITECTURE

[x] Step 1 — Custom Hooks
[x] Step 2 — Real-World Custom Hooks
[x] Step 3 — Higher-Order Components
[x] Step 4 — Render Props
[x] Step 5 — Compound Components
[x] Step 6 — Feature-Based Folder Structure
[x] Step 7 — Environment Variables
[x] Step 8 — Error Boundaries
[x] Step 9 — Production Architecture Concepts
```

## What You've Covered in Phase 12

```text
Custom Hooks
    ↓
Reusable React logic

HOC
    ↓
Component enhancement pattern

Render Props
    ↓
Function provides UI

Compound Components
    ↓
Related components working together

Feature-Based Structure
    ↓
Organize code by feature

Environment Variables
    ↓
Environment configuration

Error Boundaries
    ↓
Protect UI from rendering crashes

Production Architecture
    ↓
Organizing a real application
```

-----------------------------------------------------------------------------------------------------------------------------------------