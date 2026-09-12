#  Performance Optimization

## Step 1: Why Do React Components Re-render?

Before learning `React.memo`, `useMemo`, and `useCallback`, you need to understand one thing:

> **When and why does a React component re-render?**

---

# 1. What Is a Re-render?

A re-render means React runs the component function again.

Example:

```tsx
function Counter() {
    const [count, setCount] = useState(0);

    console.log("Component rendered");

    return (
        <div>
            <p>{count}</p>

            <button onClick={() => setCount(count + 1)}>
                Increase
            </button>
        </div>
    );
}
```

When you click:

```text
Increase
   ↓
setCount()
   ↓
State changes
   ↓
Component function runs again
   ↓
React updates the UI if needed
```

---

# 2. Main Reasons Components Re-render

There are three major reasons.

## Reason 1 — State Changes

```tsx
const [count, setCount] = useState(0);
```

When:

```tsx
setCount(1);
```

The component re-renders.

```text
State change
    ↓
Re-render
```

---

## Reason 2 — Props Change

Example:

```tsx
function Child({ name }: { name: string }) {
    console.log("Child rendered");

    return <h1>{name}</h1>;
}
```

Parent:

```tsx
<Child name="William" />
```

If the prop changes:

```text
William
   ↓
John
```

React re-renders the child.

```text
Props change
    ↓
Re-render
```

---

## Reason 3 — Parent Re-renders

This is very important.

Example:

```tsx
function Parent() {
    const [count, setCount] = useState(0);

    return (
        <div>
            <button onClick={() => setCount(count + 1)}>
                Increase
            </button>

            <Child />
        </div>
    );
}
```

Child:

```tsx
function Child() {
    console.log("Child rendered");

    return <h1>Hello</h1>;
}
```

When you click Increase:

```text
Parent state changes
       ↓
Parent re-renders
       ↓
Child also re-renders
```

Even though `Child` doesn't use `count`.

This is one of the main reasons performance optimization exists.

---

# 3. Example to Test It

```tsx
import { useState } from "react";

function Child() {
    console.log("Child rendered");

    return <h2>I am Child</h2>;
}

function App() {
    const [count, setCount] = useState(0);

    console.log("App rendered");

    return (
        <div>
            <h1>{count}</h1>

            <button
                onClick={() => setCount(count + 1)}
            >
                Increase
            </button>

            <Child />
        </div>
    );
}

export default App;
```

Open the browser console.

Initially:

```text
App rendered
Child rendered
```

Click Increase:

```text
App rendered
Child rendered
```

Again:

```text
App rendered
Child rendered
```

The child keeps rendering because its parent renders.

---

# 4. Important: Re-render ≠ DOM Update

This is extremely important.

A React re-render does **not automatically mean** the real browser DOM changes.

```text
State changes
    ↓
React component function runs again
    ↓
React compares old result vs new result
    ↓
Only necessary DOM changes are applied
```

React does not blindly rebuild the entire page every time.

---

# 5. Visual Flow

```text
STATE CHANGE

setCount()
    ↓
Component Re-renders
    ↓
React creates new UI representation
    ↓
Compare with previous UI
    ↓
Are there changes?
    ↓
Yes → Update necessary DOM parts
No  → DOM stays unchanged
```

---

# 6. Re-renders Are Normal

This is important because beginners often think:

> "Every re-render is bad."

No.

Re-rendering is a normal part of React.

```text
State changes
↓
React re-renders
↓
UI stays updated
```

The problem only comes when:

```text
Too many unnecessary re-renders
+
Expensive calculations
+
Large component trees
+
Heavy components
```

---

# 7. Example of an Unnecessary Re-render

Imagine:

```tsx
function App() {
    const [count, setCount] = useState(0);

    return (
        <>
            <Counter count={count} />

            <HeavyComponent />
        </>
    );
}
```

Every time `count` changes:

```text
App re-renders
    ↓
Counter re-renders
    ↓
HeavyComponent also re-renders
```

But `HeavyComponent` may not even need `count`.

This is where we can optimize.

---

# 8. The Three Tools We Will Learn

React gives us:

```text
React.memo
useMemo
useCallback
```

But they solve different problems.

---

## `React.memo`

Prevents unnecessary component re-renders.

```text
Parent re-renders
       ↓
Child props unchanged?
       ↓
React.memo
       ↓
Skip child re-render
```

---

## `useMemo`

Memorizes a calculated value.

```text
Expensive calculation
       ↓
useMemo
       ↓
Reuse previous result
```

---

## `useCallback`

Memorizes a function reference.

```text
Function created
       ↓
useCallback
       ↓
Reuse previous function
```

---

# 9. Simple Comparison

| Tool          | Optimizes              |
| ------------- | ---------------------- |
| `React.memo`  | Component re-rendering |
| `useMemo`     | Calculated values      |
| `useCallback` | Function references    |

---

# 10. When NOT to Optimize

Don't immediately wrap everything with:

```tsx
React.memo
useMemo
useCallback
```

That can actually make code harder to read.

Use them when there is a real reason:

```text
Expensive component
Large list
Expensive calculation
Unnecessary child renders
Performance problem
```

---

# Key Takeaway

React components mainly re-render when:

```text
1. State changes
2. Props change
3. Parent re-renders
```

And remember:

```text
Re-render
≠
DOM update
```

React re-renders the component first, then decides what actually needs to change in the DOM.

---

# Phase 10 Progress

```text
## Performance Optimization

[x] Step 1 — Why Components Re-render

[ ] Step 2 — React.memo
[ ] Step 3 — useMemo
[ ] Step 4 — useCallback
[ ] Step 5 — Lazy Loading
[ ] Step 6 — Suspense
[ ] Step 7 — Error Boundaries
```

## Next → Step 2: `React.memo`

We'll see exactly how to stop a child component from unnecessarily re-rendering when its parent re-renders.

-----------------------------------------------------------------------------------------------------------------------------------------

# Step 2 — `React.memo`

Now we know that when a parent component re-renders, its children normally re-render too.

`React.memo` helps prevent unnecessary child re-renders.

---

# 1. The Problem

Look at this example:

```tsx
import { useState } from "react";

function Child() {
    console.log("Child rendered");

    return <h2>I am Child</h2>;
}

function App() {
    const [count, setCount] = useState(0);

    console.log("App rendered");

    return (
        <div>
            <h1>{count}</h1>

            <button onClick={() => setCount(count + 1)}>
                Increase
            </button>

            <Child />
        </div>
    );
}

export default App;
```

When you click the button:

```text
count changes
    ↓
App re-renders
    ↓
Child re-renders
```

Even though `Child` doesn't use `count`.

---

# 2. Solution: `React.memo`

We can wrap the child component with `memo`.

```tsx
import { memo, useState } from "react";

const Child = memo(function Child() {
    console.log("Child rendered");

    return <h2>I am Child</h2>;
});
```

Full example:

```tsx
import { memo, useState } from "react";

const Child = memo(function Child() {
    console.log("Child rendered");

    return <h2>I am Child</h2>;
});

function App() {
    const [count, setCount] = useState(0);

    console.log("App rendered");

    return (
        <div>
            <h1>{count}</h1>

            <button onClick={() => setCount(count + 1)}>
                Increase
            </button>

            <Child />
        </div>
    );
}

export default App;
```

---

# 3. What Happens Now?

Initial render:

```text
App rendered
Child rendered
```

Click Increase:

```text
App rendered
```

Notice:

```text
Child rendered
```

doesn't appear again.

Why?

Because `React.memo` tells React:

> If this component's props have not changed, don't re-render it unnecessarily.

---

# 4. Main Rule of `React.memo`

```text
Parent re-renders
      ↓
Check Child props
      ↓
Props changed?
   /         \
 Yes         No
 ↓            ↓
Re-render    Skip re-render
```

---

# 5. Example With Props

```tsx
import { memo, useState } from "react";

type ChildProps = {
    name: string;
};

const Child = memo(function Child({ name }: ChildProps) {
    console.log("Child rendered");

    return <h2>Hello {name}</h2>;
});

function App() {
    const [count, setCount] = useState(0);

    return (
        <div>
            <h1>Count: {count}</h1>

            <button onClick={() => setCount(count + 1)}>
                Increase
            </button>

            <Child name="William" />
        </div>
    );
}
```

When `count` changes:

```text
App re-renders
      ↓
Child props = name "William"
      ↓
Same as before
      ↓
React.memo skips Child re-render
```

---

# 6. When Props Change

Now let's make `name` state.

```tsx
import { memo, useState } from "react";

type ChildProps = {
    name: string;
};

const Child = memo(function Child({ name }: ChildProps) {
    console.log("Child rendered");

    return <h2>Hello {name}</h2>;
});

function App() {
    const [count, setCount] = useState(0);
    const [name, setName] = useState("William");

    return (
        <div>
            <h1>Count: {count}</h1>

            <button onClick={() => setCount(count + 1)}>
                Increase Count
            </button>

            <button onClick={() => setName("John")}>
                Change Name
            </button>

            <Child name={name} />
        </div>
    );
}
```

### Clicking Increase Count

```text
App re-renders
      ↓
name prop is still "William"
      ↓
Child does NOT re-render
```

### Clicking Change Name

```text
App re-renders
      ↓
name changes
      ↓
Child receives new props
      ↓
Child re-renders
```

---

# 7. Important: `React.memo` Checks Props

This is the core concept.

```text
React.memo
     ↓
Checks previous props
     ↓
Checks new props
     ↓
Same?
→ Skip render

Different?
→ Re-render
```

---

# 8. What About Objects and Functions?

This is where things become interesting.

Look:

```tsx
<Child user={{ name: "William" }} />
```

You might think the value is always the same.

But every parent render creates a new object:

```text
Previous render:

{ name: "William" }


New render:

{ name: "William" }
```

They look identical, but JavaScript sees them as different references.

```text
Object A !== Object B
```

Therefore:

```text
React.memo sees new prop reference
        ↓
Child re-renders
```

The same problem happens with functions.

```tsx
<Child onClick={() => console.log("Hello")} />
```

Every render creates a new function.

This is why `useMemo` and `useCallback` exist.

We'll learn those next.

---

# 9. Simple Example

## Primitive Props

```tsx
<Child name="William" />
```

Usually easy for React.memo to compare:

```text
"William" === "William"
```

---

## Object Props

```tsx
<Child user={{ name: "William" }} />
```

Different object reference on each render:

```text
Previous object !== New object
```

---

## Function Props

```tsx
<Child onClick={() => console.log("Hello")} />
```

Different function reference on each render:

```text
Previous function !== New function
```

---

# 10. When Should You Use `React.memo`?

Good situations:

```text
✓ Heavy/expensive components
✓ Large lists
✓ Child renders unnecessarily
✓ Props usually stay the same
```

Example:

```text
Dashboard
│
├── Header
├── Sidebar
├── HeavyChart
├── LargeTable
└── Footer
```

If only a small state changes, you may not want every expensive component to re-render.

---

# 11. When NOT to Use It

Don't wrap every component:

```tsx
memo(Component1);
memo(Component2);
memo(Component3);
memo(Component4);
```

That is unnecessary.

`React.memo` itself has a cost because React must compare props.

Use it when there is an actual unnecessary rendering problem.

---

# 12. Important Limitation

`React.memo` does NOT prevent re-rendering caused by the component's own state.

Example:

```tsx
const Child = memo(function Child() {
    const [count, setCount] = useState(0);

    return (
        <button onClick={() => setCount(count + 1)}>
            {count}
        </button>
    );
});
```

When the child's own state changes:

```text
Child state changes
      ↓
Child re-renders
```

`memo` cannot stop that.

It mainly helps with parent-triggered renders when props remain unchanged.

---

# Final Mental Model

```text
WITHOUT React.memo

Parent re-renders
      ↓
Child re-renders
      ↓
Even if props are unchanged


WITH React.memo

Parent re-renders
      ↓
Compare Child props
      ↓
Props unchanged?
      ↓
YES
      ↓
Skip Child re-render
```

---

# What You Should Remember

```text
React.memo
↓
Memoizes a component

Main purpose
↓
Prevent unnecessary re-renders

Works by
↓
Comparing props

Same props
↓
Skip render

Changed props
↓
Re-render
```

---

# Phase 10 Progress

```text
[x] Step 1 — Why Components Re-render
[x] Step 2 — React.memo

[ ] Step 3 — useMemo
[ ] Step 4 — useCallback
[ ] Step 5 — Lazy Loading
[ ] Step 6 — Suspense
[ ] Step 7 — Error Boundaries
```

# Next → Step 3: `useMemo`

This will explain how to prevent expensive calculations from running again on every re-render.

------------------------------------------------------------------------------------------------------------------------------------------

# Step 3 — `useMemo`

Now we learned:

```text
React.memo
↓
Memoizes a COMPONENT
```

Next:

```text
useMemo
↓
Memoizes a VALUE / CALCULATION
```

---

# 1. The Problem

Every time a component re-renders, everything inside the component function runs again.

Example:

```tsx
function App() {
    const [count, setCount] = useState(0);

    const result = expensiveCalculation();

    return (
        <div>
            <h1>{count}</h1>

            <button onClick={() => setCount(count + 1)}>
                Increase
            </button>

            <p>{result}</p>
        </div>
    );
}
```

When you click Increase:

```text
count changes
    ↓
App re-renders
    ↓
expensiveCalculation() runs again
```

Even if the calculation has nothing to do with `count`.

---

# 2. What Is `useMemo`?

`useMemo` remembers the result of a calculation.

Basic syntax:

```tsx
const value = useMemo(() => {
    return calculation();
}, []);
```

Think:

```text
Calculate value
     ↓
Store result
     ↓
Component re-renders
     ↓
Reuse previous result
```

---

# 3. Simple Example

Without `useMemo`:

```tsx
function App() {
    const [count, setCount] = useState(0);

    const doubledNumber = 10 * 2;

    return (
        <div>
            <p>{count}</p>
            <p>{doubledNumber}</p>

            <button onClick={() => setCount(count + 1)}>
                Increase
            </button>
        </div>
    );
}
```

Technically:

```text
Component re-renders
↓
10 * 2 calculates again
```

But this calculation is tiny, so `useMemo` is unnecessary.

---

# 4. Real Example — Expensive Calculation

Imagine a large array:

```tsx
const numbers = [1, 2, 3, 4, 5];
```

We calculate:

```tsx
const total = numbers.reduce(
    (sum, number) => sum + number,
    0
);
```

Now imagine millions of numbers.

Every re-render:

```text
Component re-renders
        ↓
Loop through millions of items again
```

This can become expensive.

---

# 5. Using `useMemo`

```tsx
import { useMemo, useState } from "react";

function App() {
    const [count, setCount] = useState(0);

    const numbers = [1, 2, 3, 4, 5];

    const total = useMemo(() => {
        console.log("Calculating total");

        return numbers.reduce(
            (sum, number) => sum + number,
            0
        );
    }, []);

    return (
        <div>
            <h1>Count: {count}</h1>
            <h2>Total: {total}</h2>

            <button onClick={() => setCount(count + 1)}>
                Increase Count
            </button>
        </div>
    );
}
```

Now:

### Initial render

```text
Calculating total
```

### Click Increase Count

```text
App re-renders

BUT

Calculating total
↓
Does not run again
```

Because:

```tsx
[], // dependencies
```

Nothing changed.

---

# 6. Dependency Array

`useMemo` works similarly to `useEffect`.

```tsx
useMemo(() => {
    return calculation();
}, [dependencies]);
```

React checks dependencies.

```text
Dependencies changed?
      │
   YES ↓
Recalculate value

   NO
      ↓
Reuse previous value
```

---

# 7. Example With Dependency

```tsx
const [number, setNumber] = useState(5);
```

```tsx
const doubledNumber = useMemo(() => {
    console.log("Calculating...");

    return number * 2;
}, [number]);
```

Now:

```text
number changes
    ↓
Dependency changed
    ↓
Calculate again
```

But if another state changes:

```tsx
const [count, setCount] = useState(0);
```

Then:

```text
count changes
    ↓
Component re-renders
    ↓
number did NOT change
    ↓
Reuse previous doubledNumber
```

---

# 8. Full Example

```tsx
import { useMemo, useState } from "react";

function App() {
    const [count, setCount] = useState(0);
    const [number, setNumber] = useState(5);

    const doubledNumber = useMemo(() => {
        console.log("Calculating doubled number");

        return number * 2;
    }, [number]);

    return (
        <div>
            <h1>Count: {count}</h1>

            <button onClick={() => setCount(count + 1)}>
                Increase Count
            </button>

            <hr />

            <h1>Number: {number}</h1>

            <button onClick={() => setNumber(number + 1)}>
                Increase Number
            </button>

            <h2>Doubled: {doubledNumber}</h2>
        </div>
    );
}

export default App;
```

---

# 9. Test the Behavior

## Click "Increase Count"

```text
count changes
    ↓
Component re-renders
    ↓
number unchanged
    ↓
useMemo returns previous value
```

Console:

```text
Calculating doubled number
```

Does NOT appear again.

---

## Click "Increase Number"

```text
number changes
    ↓
Component re-renders
    ↓
Dependency changed
    ↓
useMemo calculates again
```

Console:

```text
Calculating doubled number
```

appears again.

---

# 10. `useMemo` Is NOT Just for Expensive Calculations

Another important use case is maintaining object/array references.

Example:

```tsx
const user = {
    name: "William",
};
```

Every component render creates a new object.

```text
Render 1
user → Object A

Render 2
user → Object B

Object A !== Object B
```

This can cause problems with `React.memo`.

---

# 11. Object Example

Without `useMemo`:

```tsx
const user = {
    name: "William",
};

<Child user={user} />
```

Even if the name looks the same:

```text
Parent re-renders
    ↓
New object created
    ↓
Child receives new object reference
    ↓
React.memo thinks props changed
    ↓
Child re-renders
```

With `useMemo`:

```tsx
const user = useMemo(() => {
    return {
        name: "William",
    };
}, []);
```

Now:

```text
Parent re-renders
    ↓
Same object reference reused
    ↓
React.memo sees same prop
    ↓
Child can skip re-render
```

---

# 12. `useMemo` + `React.memo`

These two often work together.

```text
React.memo
↓
Checks props


useMemo
↓
Keeps object/value reference stable
```

Example:

```tsx
import { memo, useMemo, useState } from "react";

type ChildProps = {
    user: {
        name: string;
    };
};

const Child = memo(function Child({ user }: ChildProps) {
    console.log("Child rendered");

    return <h2>{user.name}</h2>;
});

function App() {
    const [count, setCount] = useState(0);

    const user = useMemo(() => {
        return {
            name: "William",
        };
    }, []);

    return (
        <div>
            <h1>{count}</h1>

            <button onClick={() => setCount(count + 1)}>
                Increase
            </button>

            <Child user={user} />
        </div>
    );
}
```

Now:

```text
Count changes
    ↓
Parent re-renders
    ↓
user reference remains the same
    ↓
React.memo sees unchanged props
    ↓
Child skips re-render
```

---

# 13. Important Warning

Don't do this everywhere:

```tsx
const name = useMemo(() => "William", []);
```

This is pointless.

Also:

```tsx
const total = useMemo(() => 2 + 2, []);
```

Also pointless.

`useMemo` itself has overhead.

React must:

```text
Store value
Compare dependencies
Manage memoization
```

For simple calculations, just calculate normally.

---

# 14. When Should You Use `useMemo`?

Good situations:

```text
✓ Expensive calculations
✓ Filtering large lists
✓ Sorting large data
✓ Complex transformations
✓ Maintaining object references
✓ Maintaining array references for memoized children
```

Example:

```tsx
const filteredUsers = useMemo(() => {
    return users.filter((user) =>
        user.name.includes(search)
    );
}, [users, search]);
```

This recalculates only when:

```text
users changes
OR
search changes
```

---

# 15. `useMemo` vs `React.memo`

| `useMemo`              | `React.memo`                   |
| ---------------------- | ------------------------------ |
| Memoizes a value       | Memoizes a component           |
| Prevents recalculation | Prevents unnecessary re-render |
| Hook                   | Higher-order component utility |
| Used inside component  | Wraps component                |

---

# 16. Easy Mental Model

```text
React.memo
    ↓
"Don't re-render this component
if props are unchanged"


useMemo
    ↓
"Don't recalculate this value
if dependencies are unchanged"
```

---

# Important Note

`useMemo` is a performance optimization.

It should not be used to make your application logic work correctly.

Your code should work perfectly without it.

Then use `useMemo` when you have a performance reason.

---

# What You Should Remember

```text
useMemo
↓
Memorizes a calculated value

Dependencies unchanged
↓
Reuse old value

Dependencies changed
↓
Calculate new value
```

Syntax:

```tsx
const value = useMemo(() => {
    return calculation();
}, [dependencies]);
```

---

# Phase 10 Progress

```text
[x] Step 1 — Why Components Re-render
[x] Step 2 — React.memo
[x] Step 3 — useMemo

[ ] Step 4 — useCallback
[ ] Step 5 — Lazy Loading
[ ] Step 6 — Suspense
[ ] Step 7 — Error Boundaries
```

# Next → Step 4: `useCallback`

This is closely related to `React.memo` and solves the function reference problem.

-----------------------------------------------------------------------------------------------------------------------------------------

# Step 3 — `useMemo`

Now we learned:

```text
React.memo
↓
Memoizes a COMPONENT
```

Next:

```text
useMemo
↓
Memoizes a VALUE / CALCULATION
```

---

# 1. The Problem

Every time a component re-renders, everything inside the component function runs again.

Example:

```tsx
function App() {
    const [count, setCount] = useState(0);

    const result = expensiveCalculation();

    return (
        <div>
            <h1>{count}</h1>

            <button onClick={() => setCount(count + 1)}>
                Increase
            </button>

            <p>{result}</p>
        </div>
    );
}
```

When you click Increase:

```text
count changes
    ↓
App re-renders
    ↓
expensiveCalculation() runs again
```

Even if the calculation has nothing to do with `count`.

---

# 2. What Is `useMemo`?

`useMemo` remembers the result of a calculation.

Basic syntax:

```tsx
const value = useMemo(() => {
    return calculation();
}, []);
```

Think:

```text
Calculate value
     ↓
Store result
     ↓
Component re-renders
     ↓
Reuse previous result
```

---

# 3. Simple Example

Without `useMemo`:

```tsx
function App() {
    const [count, setCount] = useState(0);

    const doubledNumber = 10 * 2;

    return (
        <div>
            <p>{count}</p>
            <p>{doubledNumber}</p>

            <button onClick={() => setCount(count + 1)}>
                Increase
            </button>
        </div>
    );
}
```

Technically:

```text
Component re-renders
↓
10 * 2 calculates again
```

But this calculation is tiny, so `useMemo` is unnecessary.

---

# 4. Real Example — Expensive Calculation

Imagine a large array:

```tsx
const numbers = [1, 2, 3, 4, 5];
```

We calculate:

```tsx
const total = numbers.reduce(
    (sum, number) => sum + number,
    0
);
```

Now imagine millions of numbers.

Every re-render:

```text
Component re-renders
        ↓
Loop through millions of items again
```

This can become expensive.

---

# 5. Using `useMemo`

```tsx
import { useMemo, useState } from "react";

function App() {
    const [count, setCount] = useState(0);

    const numbers = [1, 2, 3, 4, 5];

    const total = useMemo(() => {
        console.log("Calculating total");

        return numbers.reduce(
            (sum, number) => sum + number,
            0
        );
    }, []);

    return (
        <div>
            <h1>Count: {count}</h1>
            <h2>Total: {total}</h2>

            <button onClick={() => setCount(count + 1)}>
                Increase Count
            </button>
        </div>
    );
}
```

Now:

### Initial render

```text
Calculating total
```

### Click Increase Count

```text
App re-renders

BUT

Calculating total
↓
Does not run again
```

Because:

```tsx
[], // dependencies
```

Nothing changed.

---

# 6. Dependency Array

`useMemo` works similarly to `useEffect`.

```tsx
useMemo(() => {
    return calculation();
}, [dependencies]);
```

React checks dependencies.

```text
Dependencies changed?
      │
   YES ↓
Recalculate value

   NO
      ↓
Reuse previous value
```

---

# 7. Example With Dependency

```tsx
const [number, setNumber] = useState(5);
```

```tsx
const doubledNumber = useMemo(() => {
    console.log("Calculating...");

    return number * 2;
}, [number]);
```

Now:

```text
number changes
    ↓
Dependency changed
    ↓
Calculate again
```

But if another state changes:

```tsx
const [count, setCount] = useState(0);
```

Then:

```text
count changes
    ↓
Component re-renders
    ↓
number did NOT change
    ↓
Reuse previous doubledNumber
```

---

# 8. Full Example

```tsx
import { useMemo, useState } from "react";

function App() {
    const [count, setCount] = useState(0);
    const [number, setNumber] = useState(5);

    const doubledNumber = useMemo(() => {
        console.log("Calculating doubled number");

        return number * 2;
    }, [number]);

    return (
        <div>
            <h1>Count: {count}</h1>

            <button onClick={() => setCount(count + 1)}>
                Increase Count
            </button>

            <hr />

            <h1>Number: {number}</h1>

            <button onClick={() => setNumber(number + 1)}>
                Increase Number
            </button>

            <h2>Doubled: {doubledNumber}</h2>
        </div>
    );
}

export default App;
```

---

# 9. Test the Behavior

## Click "Increase Count"

```text
count changes
    ↓
Component re-renders
    ↓
number unchanged
    ↓
useMemo returns previous value
```

Console:

```text
Calculating doubled number
```

Does NOT appear again.

---

## Click "Increase Number"

```text
number changes
    ↓
Component re-renders
    ↓
Dependency changed
    ↓
useMemo calculates again
```

Console:

```text
Calculating doubled number
```

appears again.

---

# 10. `useMemo` Is NOT Just for Expensive Calculations

Another important use case is maintaining object/array references.

Example:

```tsx
const user = {
    name: "William",
};
```

Every component render creates a new object.

```text
Render 1
user → Object A

Render 2
user → Object B

Object A !== Object B
```

This can cause problems with `React.memo`.

---

# 11. Object Example

Without `useMemo`:

```tsx
const user = {
    name: "William",
};

<Child user={user} />
```

Even if the name looks the same:

```text
Parent re-renders
    ↓
New object created
    ↓
Child receives new object reference
    ↓
React.memo thinks props changed
    ↓
Child re-renders
```

With `useMemo`:

```tsx
const user = useMemo(() => {
    return {
        name: "William",
    };
}, []);
```

Now:

```text
Parent re-renders
    ↓
Same object reference reused
    ↓
React.memo sees same prop
    ↓
Child can skip re-render
```

---

# 12. `useMemo` + `React.memo`

These two often work together.

```text
React.memo
↓
Checks props


useMemo
↓
Keeps object/value reference stable
```

Example:

```tsx
import { memo, useMemo, useState } from "react";

type ChildProps = {
    user: {
        name: string;
    };
};

const Child = memo(function Child({ user }: ChildProps) {
    console.log("Child rendered");

    return <h2>{user.name}</h2>;
});

function App() {
    const [count, setCount] = useState(0);

    const user = useMemo(() => {
        return {
            name: "William",
        };
    }, []);

    return (
        <div>
            <h1>{count}</h1>

            <button onClick={() => setCount(count + 1)}>
                Increase
            </button>

            <Child user={user} />
        </div>
    );
}
```

Now:

```text
Count changes
    ↓
Parent re-renders
    ↓
user reference remains the same
    ↓
React.memo sees unchanged props
    ↓
Child skips re-render
```

---

# 13. Important Warning

Don't do this everywhere:

```tsx
const name = useMemo(() => "William", []);
```

This is pointless.

Also:

```tsx
const total = useMemo(() => 2 + 2, []);
```

Also pointless.

`useMemo` itself has overhead.

React must:

```text
Store value
Compare dependencies
Manage memoization
```

For simple calculations, just calculate normally.

---

# 14. When Should You Use `useMemo`?

Good situations:

```text
✓ Expensive calculations
✓ Filtering large lists
✓ Sorting large data
✓ Complex transformations
✓ Maintaining object references
✓ Maintaining array references for memoized children
```

Example:

```tsx
const filteredUsers = useMemo(() => {
    return users.filter((user) =>
        user.name.includes(search)
    );
}, [users, search]);
```

This recalculates only when:

```text
users changes
OR
search changes
```

---

# 15. `useMemo` vs `React.memo`

| `useMemo`              | `React.memo`                   |
| ---------------------- | ------------------------------ |
| Memoizes a value       | Memoizes a component           |
| Prevents recalculation | Prevents unnecessary re-render |
| Hook                   | Higher-order component utility |
| Used inside component  | Wraps component                |

---

# 16. Easy Mental Model

```text
React.memo
    ↓
"Don't re-render this component
if props are unchanged"


useMemo
    ↓
"Don't recalculate this value
if dependencies are unchanged"
```

---

# Important Note

`useMemo` is a performance optimization.

It should not be used to make your application logic work correctly.

Your code should work perfectly without it.

Then use `useMemo` when you have a performance reason.

---

# What You Should Remember

```text
useMemo
↓
Memorizes a calculated value

Dependencies unchanged
↓
Reuse old value

Dependencies changed
↓
Calculate new value
```

Syntax:

```tsx
const value = useMemo(() => {
    return calculation();
}, [dependencies]);
```

---

# Phase 10 Progress

```text
[x] Step 1 — Why Components Re-render
[x] Step 2 — React.memo
[x] Step 3 — useMemo

[ ] Step 4 — useCallback
[ ] Step 5 — Lazy Loading
[ ] Step 6 — Suspense
[ ] Step 7 — Error Boundaries
```

# Next → Step 4: `useCallback`

This is closely related to `React.memo` and solves the function reference problem.

------------------------------------------------------------------------------------------------------------------------------------------

# Step 4 — `useCallback`

Now we have:

```text
React.memo
↓
Memoizes a component

useMemo
↓
Memoizes a value

useCallback
↓
Memoizes a function
```

---

# 1. The Problem: Functions Are Recreated on Every Render

Look at this:

```tsx
function App() {
    const [count, setCount] = useState(0);

    function handleClick() {
        console.log("Hello");
    }

    return (
        <div>
            <h1>{count}</h1>

            <button onClick={() => setCount(count + 1)}>
                Increase
            </button>

            <Child onClick={handleClick} />
        </div>
    );
}
```

Every time `App` re-renders:

```text
App render 1
→ handleClick = Function A

App render 2
→ handleClick = Function B

Function A !== Function B
```

Even though the function does exactly the same thing.

---

# 2. Why Is This a Problem?

Imagine `Child` uses `React.memo`.

```tsx
const Child = memo(function Child({ onClick }) {
    console.log("Child rendered");

    return <button onClick={onClick}>Click</button>;
});
```

You might expect `Child` not to re-render.

But:

```text
Parent re-renders
      ↓
New handleClick function created
      ↓
Child receives new function reference
      ↓
React.memo thinks props changed
      ↓
Child re-renders
```

So `React.memo` cannot help here.

---

# 3. Solution: `useCallback`

`useCallback` remembers the function reference.

```tsx
const handleClick = useCallback(() => {
    console.log("Hello");
}, []);
```

Now:

```text
Parent re-renders
      ↓
Dependencies unchanged
      ↓
Same function reference reused
```

---

# 4. Basic Syntax

```tsx
const functionName = useCallback(() => {
    // function logic
}, [dependencies]);
```

Compare with `useMemo`:

```tsx
const value = useMemo(() => {
    return calculation();
}, [dependencies]);
```

Very similar.

---

# 5. Full Example

```tsx
import { memo, useCallback, useState } from "react";

type ChildProps = {
    onClick: () => void;
};

const Child = memo(function Child({ onClick }: ChildProps) {
    console.log("Child rendered");

    return (
        <button onClick={onClick}>
            Child Button
        </button>
    );
});

function App() {
    const [count, setCount] = useState(0);

    const handleClick = useCallback(() => {
        console.log("Hello from Child");
    }, []);

    console.log("App rendered");

    return (
        <div>
            <h1>Count: {count}</h1>

            <button onClick={() => setCount(count + 1)}>
                Increase Count
            </button>

            <Child onClick={handleClick} />
        </div>
    );
}

export default App;
```

---

# 6. What Happens?

### Initial Render

```text
App rendered
Child rendered
```

### Click "Increase Count"

Without `useCallback`:

```text
App rendered
Child rendered
```

With `useCallback`:

```text
App rendered
```

Why?

```text
App re-renders
      ↓
handleClick reference stays the same
      ↓
Child props remain unchanged
      ↓
React.memo skips Child render
```

---

# 7. Dependency Array

Just like `useMemo`, `useCallback` has dependencies.

```tsx
const handleClick = useCallback(() => {
    console.log(count);
}, [count]);
```

Now if `count` changes:

```text
count changes
    ↓
Dependency changed
    ↓
New function created
```

If `count` does not change:

```text
Dependency unchanged
    ↓
Same function reused
```

---

# 8. Example With Dependencies

```tsx
function App() {
    const [count, setCount] = useState(0);
    const [name, setName] = useState("William");

    const handleClick = useCallback(() => {
        console.log(name);
    }, [name]);

    return (
        <div>
            <button onClick={() => setCount(count + 1)}>
                Increase Count
            </button>

            <button onClick={() => setName("John")}>
                Change Name
            </button>

            <Child onClick={handleClick} />
        </div>
    );
}
```

### Increase Count

```text
count changes
↓
App re-renders
↓
name unchanged
↓
handleClick remains the same
```

### Change Name

```text
name changes
↓
App re-renders
↓
Dependency changed
↓
New handleClick created
```

---

# 9. `useCallback` + `React.memo`

This is the most common use case.

```text
React.memo
↓
Prevents child re-render if props stay the same


BUT

Functions normally get new references


useCallback
↓
Keeps function reference stable
```

Together:

```text
Parent re-renders
       ↓
useCallback keeps same function
       ↓
React.memo compares props
       ↓
Props unchanged
       ↓
Child skips render
```

---

# 10. `useCallback` vs `useMemo`

This is a common interview question.

| `useMemo`                       | `useCallback`                       |
| ------------------------------- | ----------------------------------- |
| Memoizes a value                | Memoizes a function                 |
| Stores calculation result       | Stores function reference           |
| Used for expensive calculations | Used for stable function references |

Example:

```tsx
const total = useMemo(() => {
    return calculateTotal();
}, [items]);
```

```tsx
const handleClick = useCallback(() => {
    console.log("Clicked");
}, []);
```

---

# 11. Interesting Fact

You can think of:

```tsx
useCallback(fn, dependencies)
```

as roughly:

```tsx
useMemo(() => fn, dependencies)
```

So conceptually:

```text
useMemo
→ Memoizes a returned value

useCallback
→ Memoizes the function itself
```

---

# 12. When Should You Use `useCallback`?

Good situations:

```text
✓ Passing functions to React.memo components
✓ Preventing unnecessary child re-renders
✓ Function used as a dependency in another hook
✓ Performance-sensitive components
```

---

# 13. When NOT to Use It

Don't do this everywhere:

```tsx
const handleA = useCallback(() => {}, []);
const handleB = useCallback(() => {}, []);
const handleC = useCallback(() => {}, []);
```

If you're not passing functions to memoized children or solving a dependency/performance problem, normal functions are usually fine.

```tsx
function handleClick() {
    console.log("Hello");
}
```

Simple and readable.

---

# 14. Complete Mental Model

```text
NORMAL FUNCTION

Parent render 1
→ Function A

Parent render 2
→ Function B

A !== B


useCallback

Parent render 1
→ Function A

Parent render 2
→ Function A

Dependencies unchanged
↓
Same reference
```

---

# 15. The Three Optimization Tools Together

```text
React.memo
    ↓
Memoizes COMPONENT


useMemo
    ↓
Memoizes VALUE


useCallback
    ↓
Memoizes FUNCTION
```

### Example:

```tsx
const Child = memo(function Child() {
    // Component memoization
});

const filteredUsers = useMemo(() => {
    return users.filter(...);
}, [users]);

const handleDelete = useCallback(() => {
    // Function
}, []);
```

--


# What You Should Remember

```text
useCallback
↓
Keeps a function reference stable

Dependencies unchanged
↓
Reuse same function

Dependencies changed
↓
Create new function
```

The most important combination:

```text
React.memo + useCallback
```

This helps prevent unnecessary child re-renders when passing functions as props.

---

# Phase 10 Progress

```text
[x] Step 1 — Why Components Re-render
[x] Step 2 — React.memo
[x] Step 3 — useMemo
[x] Step 4 — useCallback

[ ] Step 5 — Lazy Loading
[ ] Step 6 — Suspense
[ ] Step 7 — Error Boundaries
```

# Next → Step 5: Lazy Loading

We will learn how React can load components only when they are actually needed, reducing the initial bundle size.

------------------------------------------------------------------------------------------------------------------------------------------

# Step 5 — Lazy Loading

So far, we optimized:

```text
React.memo   → Component rendering
useMemo      → Expensive values/calculations
useCallback  → Function references
```

Now we'll optimize something different:

```text
Lazy Loading
↓
Controls WHEN a component is loaded
```

---

# 1. The Problem: Everything Loads at Once

Imagine your application:

```text
App
├── Home
├── Dashboard
├── Settings
├── Profile
└── Admin Panel
```

Normally, if you import components like this:

```tsx
import Home from "./Home";
import Dashboard from "./Dashboard";
import Settings from "./Settings";
import Profile from "./Profile";
import Admin from "./Admin";
```

Your application bundle may include all these components.

Even if the user only visits:

```text
Home Page
```

They may still download JavaScript for:

```text
Dashboard
Settings
Profile
Admin
```

That can increase the initial bundle size.

---

# 2. What Is Lazy Loading?

Lazy loading means:

> Load something only when it is needed.

Instead of:

```text
Application starts
       ↓
Load everything
```

We do:

```text
Application starts
       ↓
Load only necessary code
       ↓
User needs another component
       ↓
Load that component
```

---

# 3. React.lazy()

React provides:

```tsx
lazy()
```

Example:

```tsx
import { lazy } from "react";

const Settings = lazy(() => import("./Settings"));
```

This means:

> Don't load `Settings` immediately. Load it when React needs it.

---

# 4. Normal Import vs Lazy Import

## Normal Import

```tsx
import Settings from "./Settings";
```

```text
App starts
   ↓
Settings code loads immediately
```

---

## Lazy Import

```tsx
const Settings = lazy(() => import("./Settings"));
```

```text
App starts
   ↓
Settings code is not loaded yet
   ↓
React needs Settings
   ↓
Settings code loads
```

---

# 5. Basic Example

## `HeavyComponent.tsx`

```tsx
function HeavyComponent() {
    return (
        <div>
            <h1>Heavy Component</h1>
            <p>This component is loaded lazily.</p>
        </div>
    );
}

export default HeavyComponent;
```

Notice:

```tsx
export default HeavyComponent;
```

We'll come back to why this matters.

---

## `App.tsx`

```tsx
import { lazy, useState } from "react";

const HeavyComponent = lazy(
    () => import("./HeavyComponent")
);

function App() {
    const [showComponent, setShowComponent] = useState(false);

    return (
        <div>
            <button
                onClick={() => setShowComponent(true)}
            >
                Load Component
            </button>

            {showComponent && <HeavyComponent />}
        </div>
    );
}

export default App;
```

But this code has a problem.

React doesn't know what to show while the component is loading.

This is where `Suspense` comes in.

---

# 6. Lazy Loading Requires `Suspense`

We wrap lazy components with:

```tsx
<Suspense fallback={...}>
```

Full example:

```tsx
import {
    lazy,
    Suspense,
    useState,
} from "react";

const HeavyComponent = lazy(
    () => import("./HeavyComponent")
);

function App() {
    const [showComponent, setShowComponent] = useState(false);

    return (
        <div>
            <button
                onClick={() => setShowComponent(true)}
            >
                Load Component
            </button>

            <Suspense fallback={<p>Loading...</p>}>
                {showComponent && <HeavyComponent />}
            </Suspense>
        </div>
    );
}

export default App;
```

Flow:

```text
User clicks button
       ↓
showComponent = true
       ↓
React needs HeavyComponent
       ↓
Download component code
       ↓
Show "Loading..."
       ↓
Component finishes loading
       ↓
Display HeavyComponent
```

---

# 7. Why Use Lazy Loading?

Imagine a large application:

```text
Application
│
├── Home Page
├── Dashboard
├── Analytics
│     ├── Charts
│     └── Reports
│
├── Settings
└── Admin Panel
```

Without lazy loading:

```text
Initial Load
↓
Download everything
```

With lazy loading:

```text
Initial Load
↓
Download Home


User opens Dashboard
↓
Download Dashboard


User opens Admin
↓
Download Admin
```

This reduces the initial JavaScript bundle.

---

# 8. Real-World Example: Pages

Lazy loading is commonly used with routes.

Without lazy loading:

```tsx
import Home from "./pages/Home";
import Dashboard from "./pages/Dashboard";
import Settings from "./pages/Settings";
```

All page code can be included in the initial bundle.

With lazy loading:

```tsx
import { lazy } from "react";

const Home = lazy(() => import("./pages/Home"));

const Dashboard = lazy(
    () => import("./pages/Dashboard")
);

const Settings = lazy(
    () => import("./pages/Settings")
);
```

Now pages can load only when needed.

This is very common in large React applications.

---

# 9. Dynamic Import

This syntax:

```tsx
import("./HeavyComponent")
```

is called a:

```text
Dynamic Import
```

Compare:

### Static Import

```tsx
import HeavyComponent from "./HeavyComponent";
```

Loaded as part of the normal module loading process.

### Dynamic Import

```tsx
import("./HeavyComponent");
```

Returns a Promise.

Conceptually:

```text
Request component
      ↓
Promise
      ↓
Component downloaded
      ↓
Promise resolves
```

React's `lazy()` works with this dynamic import.

---

# 10. Why Default Export Matters

Usually React.lazy expects a module with a default export.

This works:

```tsx
function HeavyComponent() {
    return <h1>Hello</h1>;
}

export default HeavyComponent;
```

Then:

```tsx
const HeavyComponent = lazy(
    () => import("./HeavyComponent")
);
```

---

# 11. What If You Use Named Exports?

Example:

```tsx
export function HeavyComponent() {
    return <h1>Hello</h1>;
}
```

Then this won't directly work:

```tsx
const HeavyComponent = lazy(
    () => import("./HeavyComponent")
);
```

Because React.lazy expects:

```text
default export
```

There are ways to handle named exports, but for now remember:

```text
React.lazy()
↓
Usually works directly with default exports
```

---

# 12. Important: Lazy Loading Is Not for Everything

Don't do this:

```tsx
const Button = lazy(() => import("./Button"));
const Input = lazy(() => import("./Input"));
const Header = lazy(() => import("./Header"));
```

For tiny components, lazy loading can create unnecessary complexity.

Use it for:

```text
✓ Pages/routes
✓ Large components
✓ Heavy charts
✓ Admin dashboards
✓ Rich text editors
✓ Large third-party libraries
```

---

# 13. Mental Model

## Without Lazy Loading

```text
USER OPENS APP

Download:

████████████████████████

Everything
```

## With Lazy Loading

```text
USER OPENS APP

Download:

█████

Essential code only


Later...

User needs feature
       ↓
Download feature code
```

---

# 14. Lazy Loading vs React.memo

These are completely different.

| Feature       | Purpose                                 |
| ------------- | --------------------------------------- |
| `React.memo`  | Prevent unnecessary re-render           |
| `useMemo`     | Prevent unnecessary calculation         |
| `useCallback` | Prevent unnecessary function recreation |
| `lazy()`      | Load component code only when needed    |

---

# 15. Key Takeaway

```text
React.lazy()
↓
Lazy loads a component

Dynamic import()
↓
Loads module when needed

Suspense
↓
Shows fallback while loading
```

Basic pattern:

```tsx
import { lazy, Suspense } from "react";

const Component = lazy(
    () => import("./Component")
);

function App() {
    return (
        <Suspense fallback={<p>Loading...</p>}>
            <Component />
        </Suspense>
    );
}
```

---

# Phase 10 Progress

```text
[x] Step 1 — Why Components Re-render
[x] Step 2 — React.memo
[x] Step 3 — useMemo
[x] Step 4 — useCallback
[x] Step 5 — Lazy Loading

[ ] Step 6 — Suspense
[ ] Step 7 — Error Boundaries
```

# Next → Step 6: `Suspense`

We already briefly used `Suspense` here. Next we'll understand exactly what it does and how fallback loading boundaries work.

-----------------------------------------------------------------------------------------------------------------------------------------

# Step 6 — `Suspense`

We already used `Suspense` with lazy loading:

```tsx
<Suspense fallback={<p>Loading...</p>}>
    <HeavyComponent />
</Suspense>
```

Now let's understand exactly what it does.

---

# 1. What Is Suspense?

`Suspense` allows React to show a fallback UI while something is not ready yet.

```text
Component not ready
       ↓
Show fallback
       ↓
Component becomes ready
       ↓
Show component
```

The most common use for now:

```text
Lazy-loaded components
```

---

# 2. Basic Syntax

```tsx
<Suspense fallback={<p>Loading...</p>}>
    <Component />
</Suspense>
```

The `fallback` is what the user sees while waiting.

Examples:

```tsx
fallback={<p>Loading...</p>}
```

```tsx
fallback={<Spinner />}
```

```tsx
fallback={<h1>Please wait...</h1>}
```

---

# 3. Why Is Suspense Needed?

Consider this:

```tsx
const Dashboard = lazy(
    () => import("./Dashboard")
);
```

The Dashboard component is loaded asynchronously.

```text
React needs Dashboard
       ↓
Dashboard code is downloading
       ↓
What should the user see?
```

React needs an answer.

That's what `Suspense` provides:

```tsx
<Suspense fallback={<p>Loading Dashboard...</p>}>
    <Dashboard />
</Suspense>
```

---

# 4. Complete Example

## `Dashboard.tsx`

```tsx
function Dashboard() {
    return (
        <div>
            <h1>Dashboard</h1>
            <p>Welcome to your dashboard.</p>
        </div>
    );
}

export default Dashboard;
```

---

## `App.tsx`

```tsx
import { lazy, Suspense, useState } from "react";

const Dashboard = lazy(
    () => import("./Dashboard")
);

function App() {
    const [showDashboard, setShowDashboard] = useState(false);

    return (
        <div>
            <button
                onClick={() => setShowDashboard(true)}
            >
                Open Dashboard
            </button>

            <Suspense
                fallback={<p>Loading Dashboard...</p>}
            >
                {showDashboard && <Dashboard />}
            </Suspense>
        </div>
    );
}

export default App;
```

---

# 5. Flow

When the application first loads:

```text
App loaded
│
├── Button visible
│
└── Dashboard not loaded yet
```

User clicks:

```text
Open Dashboard
       ↓
showDashboard = true
       ↓
React needs Dashboard
       ↓
Start downloading Dashboard code
       ↓
Show fallback
       ↓
Loading Dashboard...
       ↓
Dashboard downloaded
       ↓
Show Dashboard
```

---

# 6. Suspense Is a Boundary

Think of `Suspense` as a loading boundary.

```text
App
│
├── Header
│
├── Suspense Boundary
│      │
│      └── Dashboard
│
└── Footer
```

If Dashboard is loading:

```text
Header remains visible

Suspense area:
Loading...

Footer remains visible
```

Only the area inside the Suspense boundary is affected.

---

# 7. Example With Multiple Components

```tsx
<Suspense fallback={<p>Loading...</p>}>
    <Dashboard />
    <Analytics />
</Suspense>
```

Both are inside one loading boundary.

If one of them is still loading, the fallback may be shown for that boundary.

---

# 8. Separate Suspense Boundaries

You can also use multiple boundaries:

```tsx
<div>
    <Suspense fallback={<p>Loading Dashboard...</p>}>
        <Dashboard />
    </Suspense>

    <Suspense fallback={<p>Loading Analytics...</p>}>
        <Analytics />
    </Suspense>
</div>
```

Now each section handles its loading separately.

```text
Dashboard
    ↓
Own loading state


Analytics
    ↓
Own loading state
```

This can create a better user experience in larger applications.

---

# 9. Suspense vs Normal Loading State

Before Suspense, you might manually do:

```tsx
if (isLoading) {
    return <p>Loading...</p>;
}
```

This is common with API requests.

But `Suspense` is different.

### Manual loading:

```text
You control the loading state yourself.
```

### Suspense:

```text
React handles the waiting boundary.
```

For our current learning:

```text
useQuery
↓
API data loading


Suspense
↓
Lazy component loading
```

Don't confuse them.

---

# 10. Important Difference

```tsx
const {
    data,
    isLoading,
} = useQuery(...);
```

Here:

```text
Waiting for API DATA
```

While:

```tsx
<Suspense fallback={<p>Loading...</p>}>
```

Commonly:

```text
Waiting for COMPONENT CODE
```

---

# 11. Lazy + Suspense Relationship

They usually work together:

```text
lazy()
   ↓
Component loads asynchronously
   ↓
Suspense
   ↓
Shows fallback while waiting
```

Example:

```tsx
const Settings = lazy(
    () => import("./Settings")
);
```

```tsx
<Suspense fallback={<p>Loading Settings...</p>}>
    <Settings />
</Suspense>
```

---

# 12. Real-World Example

Imagine an admin dashboard:

```text
Admin Dashboard
│
├── Sidebar
├── Header
│
├── Analytics
│     └── Heavy charts
│
├── Reports
│
└── Settings
```

You may lazy load the heavy sections:

```text
Initial Load
│
├── Sidebar ✓
├── Header ✓
│
└── Analytics ✗ Not loaded yet
```

When the user opens Analytics:

```text
Analytics requested
       ↓
Show spinner
       ↓
Load charts
       ↓
Display Analytics
```

This improves the initial application loading performance.

---

# 13. Suspense Is Not Only for Lazy Loading

Technically, modern React can use Suspense in more advanced situations.

But for your current roadmap, remember:

```text
Main use case:
React.lazy + Suspense
```

That's enough for now.

---

# 14. Mental Model

```text
Suspense
↓
"React, while this part is waiting,
show this fallback UI."
```

Syntax:

```tsx
<Suspense fallback={<Loading />}>
    <SomethingThatMayNotBeReady />
</Suspense>
```

---

# 15. `lazy()` vs `Suspense`

| `lazy()`                    | `Suspense`                            |
| --------------------------- | ------------------------------------- |
| Loads component dynamically | Handles waiting UI                    |
| Controls what loads later   | Controls what user sees while waiting |
| Uses dynamic `import()`     | Uses `fallback`                       |

Together:

```text
lazy()
   ↓
Load component later

Suspense
   ↓
Show loading UI while waiting
```

---

# Key Takeaway

```text
Suspense
↓
A loading boundary

fallback
↓
What users see while waiting

Most common usage for now
↓
React.lazy + Suspense
```

Basic pattern:

```tsx
import { lazy, Suspense } from "react";

const Component = lazy(
    () => import("./Component")
);

function App() {
    return (
        <Suspense fallback={<p>Loading...</p>}>
            <Component />
        </Suspense>
    );
}
```

---

# Phase 10 Progress

```text
[x] Step 1 — Why Components Re-render
[x] Step 2 — React.memo
[x] Step 3 — useMemo
[x] Step 4 — useCallback
[x] Step 5 — Lazy Loading
[x] Step 6 — Suspense

[ ] Step 7 — Error Boundaries
```

# Next → Step 7: Error Boundaries

This is the final topic in **Performance Optimization**.

-----------------------------------------------------------------------------------------------------------------------------------------

# Step 7 — Error Boundaries

This is the final topic in **Phase 10: Performance Optimization**.

---

# 1. What Problem Do Error Boundaries Solve?

Imagine your application:

```text
App
│
├── Header
├── Dashboard
│   ├── Chart
│   └── Statistics
├── Sidebar
└── Footer
```

Now imagine the `Chart` component crashes because of an error.

Without proper error handling:

```text
Chart crashes
    ↓
React application may break
    ↓
User sees a broken/blank UI
```

We want to handle this gracefully.

---

# 2. What Is an Error Boundary?

An Error Boundary is a React component that catches errors in its child component tree.

```text
Error happens
    ↓
Error Boundary catches it
    ↓
Show fallback UI
```

Example:

```text
Instead of:

💥 Application crashed


Show:

Something went wrong.
Please try again.
```

---

# 3. Visual Example

```text
App
│
├── Header
│
├── ErrorBoundary
│      │
│      ├── Dashboard
│      │
│      └── Chart 💥 Error
│
└── Footer
```

The Error Boundary catches the error from inside its children.

```text
Header still works
Footer still works

Dashboard area:
"Something went wrong"
```

---

# 4. Traditional Error Boundary

Historically, Error Boundaries are created using a class component.

```tsx
import { Component, type ReactNode } from "react";

type Props = {
    children: ReactNode;
};

type State = {
    hasError: boolean;
};

class ErrorBoundary extends Component<Props, State> {
    state: State = {
        hasError: false,
    };

    static getDerivedStateFromError(): State {
        return {
            hasError: true,
        };
    }

    componentDidCatch(error: Error) {
        console.error("Error caught:", error);
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

Don't worry about understanding every class component detail.

The important part is the behavior.

---

# 5. How to Use It

Imagine this component can crash:

## `BuggyComponent.tsx`

```tsx
function BuggyComponent() {
    throw new Error("Something went wrong!");

    return <h1>Hello</h1>;
}

export default BuggyComponent;
```

Now wrap it.

## `App.tsx`

```tsx
import ErrorBoundary from "./ErrorBoundary";
import BuggyComponent from "./BuggyComponent";

function App() {
    return (
        <div>
            <h1>My Application</h1>

            <ErrorBoundary>
                <BuggyComponent />
            </ErrorBoundary>
        </div>
    );
}

export default App;
```

Flow:

```text
BuggyComponent renders
       ↓
Error happens
       ↓
ErrorBoundary catches error
       ↓
hasError = true
       ↓
Show fallback UI
```

Instead of crashing the entire app.

---

# 6. Why `componentDidCatch`?

This method allows us to handle or log the error.

```tsx
componentDidCatch(error: Error) {
    console.error(error);
}
```

In a real application:

```text
Error happens
    ↓
Error Boundary catches it
    ↓
Send error to monitoring service
```

For example:

```text
Sentry
Datadog
Other error monitoring tools
```

---

# 7. Where Should You Put Error Boundaries?

You can put one around the entire application:

```tsx
<ErrorBoundary>
    <App />
</ErrorBoundary>
```

But a better approach in larger applications is to use multiple boundaries.

Example:

```text
App
│
├── Header
│
├── ErrorBoundary
│      └── Dashboard
│
├── ErrorBoundary
│      └── Analytics
│
└── Footer
```

Now if Analytics crashes:

```text
Analytics → Fallback UI

Header → Still working
Dashboard → Still working
Footer → Still working
```

This is called using error boundaries strategically.

---

# 8. Error Boundaries Do NOT Catch Everything

This is important.

Error Boundaries generally catch errors during:

```text
✓ Rendering
✓ Lifecycle methods
✓ Child component tree rendering
```

They do NOT automatically catch errors inside:

```text
✗ Event handlers
✗ Async functions
✗ setTimeout
✗ API requests
```

Example:

```tsx
function App() {
    function handleClick() {
        throw new Error("Button error");
    }

    return (
        <button onClick={handleClick}>
            Click
        </button>
    );
}
```

For this, use normal:

```tsx
try {
    // code
} catch (error) {
    // handle error
}
```

---

# 9. Error Boundary vs try/catch

| Error Boundary          | try/catch                       |
| ----------------------- | ------------------------------- |
| React rendering errors  | JavaScript errors               |
| Component crashes       | Functions/API calls             |
| Shows fallback UI       | Handles logic errors            |
| Protects component tree | Handles specific code execution |

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

# 10. Error Boundary + Suspense

These can work together.

```tsx
<ErrorBoundary>
    <Suspense fallback={<p>Loading...</p>}>
        <Dashboard />
    </Suspense>
</ErrorBoundary>
```

Now we handle two situations.

```text
Dashboard loading
        ↓
Suspense
        ↓
Loading...


Dashboard crashes
        ↓
ErrorBoundary
        ↓
Something went wrong
```

Very common architecture:

```text
ErrorBoundary
      │
      └── Suspense
             │
             └── Lazy Component
```

---

# 11. Modern React Approach

In modern React projects, developers often use libraries such as `react-error-boundary`.

It provides easier Error Boundary handling with function-based React applications.

Conceptually:

```text
Traditional React
↓
Class Error Boundary


Modern applications
↓
react-error-boundary library
```

But understanding the core React Error Boundary concept is more important first.

---

# 12. Complete Mental Model

```text
Component Tree

App
 │
 ├── Header
 │
 └── ErrorBoundary
        │
        ├── Component A
        │
        └── Component B 💥
                 ↓
             Error caught
                 ↓
        ErrorBoundary fallback
```

---

# 13. Important Summary

```text
Error Boundary
↓
Catches rendering errors

Purpose
↓
Prevent part of the UI from completely crashing

Fallback
↓
Shows user-friendly error UI

Does NOT handle
↓
Event handlers
Async/API errors
```

---

# Phase 10 Complete

```text
## Performance Optimization

[x] Why React components re-render
[x] React.memo
[x] useMemo
[x] useCallback
[x] Lazy loading
[x] Suspense
[x] Error Boundaries
```

---

# Complete Performance Optimization Mental Map

```text
PERFORMANCE OPTIMIZATION
│
├── Re-rendering
│     ├── Why components re-render
│     └── React.memo
│
├── Memoization
│     ├── useMemo → Values
│     └── useCallback → Functions
│
├── Code Splitting
│     ├── React.lazy
│     └── Suspense
│
└── Error Handling
      └── Error Boundaries
```

## Next According to Your Roadmap

# Phase 11 — Testing

```text
[ ] Jest
[ ] React Testing Library
[ ] Basic E2E testing
    ├── Playwright
    OR
    └── Cypress
```

-----------------------------------------------------------------------------------------------------------------------------------------

## `react-error-boundary`

`react-error-boundary` is a library that helps React applications handle errors gracefully.

Instead of your entire app crashing when a component throws an error, you can show a fallback UI.

### Install

```bash
npm install react-error-boundary
```

---

## Basic Example

```tsx
import { ErrorBoundary } from "react-error-boundary";

function ErrorFallback({ error }: { error: Error }) {
  return (
    <div>
      <h2>Something went wrong</h2>
      <p>{error.message}</p>
    </div>
  );
}

function App() {
  return (
    <ErrorBoundary FallbackComponent={ErrorFallback}>
      <MyComponent />
    </ErrorBoundary>
  );
}
```

### Flow

```text
MyComponent crashes
       ↓
ErrorBoundary catches error
       ↓
ErrorFallback is displayed
```

---

## Why use it?

Imagine:

```text
App
 ├── Navbar
 ├── Posts
 └── Sidebar
```

If `Posts` crashes:

Without Error Boundary:

```text
Entire app may break ❌
```

With Error Boundary around Posts:

```text
Navbar works ✅
Posts → Error message shown ⚠️
Sidebar works ✅
```

Example:

```tsx
<ErrorBoundary FallbackComponent={ErrorFallback}>
  <Posts />
</ErrorBoundary>
```

---

## Important difference

### `try/catch`

Used mainly for:

* Async functions
* API calls
* Promises

```tsx
try {
  await fetchData();
} catch (error) {
  console.log(error);
}
```

### `ErrorBoundary`

Used for:

* React component rendering errors
* Errors inside component lifecycle/render tree

```tsx
<ErrorBoundary>
  <Component />
</ErrorBoundary>
```

---

## Simple mental model

```text
React Query → handles server/API errors

try/catch → handles async operation errors

react-error-boundary → handles React UI/component crashes
```

This is commonly used together with React Query in production applications.

-----------------------------------------------------------------------------------------------------------------------------------------




