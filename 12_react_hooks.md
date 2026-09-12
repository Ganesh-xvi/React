# React Hooks

Your roadmap has:

```text
React Hooks
│
├── useState
├── useEffect
├── useRef
├── useMemo
└── useCallback
```

We'll learn them **one by one**, with small examples.

---

# React Hooks — Step 1: `Hook`

A **Hook** is a special React function that lets a component use React features.

For example:

```tsx
useState()
```

lets a component have **state**.

```tsx
useEffect()
```

lets a component perform **side effects**.

```tsx
useRef()
```

lets a component keep a value/reference without causing a re-render.

So you can think:

```text
Hook
 ↓
React feature inside a component
```

---

# 2. Why Are They Called "Hooks"?

The word **Hook** basically means that the function lets your component **"hook into" React's features**.

For example:

```text
Component
    ↓
useState
    ↓
React State
```

---

# 3. Important Rule

Hooks are normally called:

**inside a React component**

For example:

```tsx
function App() {
    const [count, setCount] = useState(0);

    return <h1>{count}</h1>;
}
```

Not like this:

```tsx
const [count, setCount] = useState(0);

function App() {
    ...
}
```

The Hook belongs inside the component.

---

# 4. First Hook — `useState`

You've already used it:

```tsx
const [count, setCount] = useState(0);
```

It gives us two things:

```text
count
 ↓
current state value

setCount
 ↓
function to change the state
```

Example:

```text
count = 0
   ↓
click
   ↓
setCount(1)
   ↓
count = 1
   ↓
React updates UI
```

---

# 5. Why Can't We Just Use a Normal Variable?

You might wonder:

```tsx
let count = 0;
```

Why not use that?

Because changing a normal variable does **not tell React to update the UI**.

For example:

```text
normal variable
      ↓
value changes
      ↓
React doesn't know
      ↓
UI doesn't automatically update
```

With state:

```text
setCount()
    ↓
state changes
    ↓
React knows
    ↓
component re-renders
    ↓
UI updates
```

That's the main reason we use `useState`.

---

# 6. Simple Example

Let's create a small counter:

```tsx
import { useState } from "react";

function Counter() {
    const [count, setCount] = useState(0);

    return (
        <div>
            <h1>{count}</h1>

            <button onClick={() => setCount(count + 1)}>
                Increase
            </button>
        </div>
    );
}

export default Counter;
```

The flow is:

```text
Initial state
count = 0

       ↓

Click Increase

       ↓

setCount(count + 1)

       ↓

count = 1

       ↓

React re-renders

       ↓

Screen shows 1
```

Click again:

```text
1 → 2 → 3 → 4 → ...
```

---

# 7. `useState` Can Store Different Types

It isn't only for numbers.

### String

```tsx
const [name, setName] = useState("");
```

### Boolean

```tsx
const [completed, setCompleted] = useState(false);
```

### Array

```tsx
const [todos, setTodos] = useState<Todo[]>([]);
```

### Object

```tsx
const [user, setUser] = useState({
    name: "",
    age: 0
});
```

So:

```text
useState
│
├── number
├── string
├── boolean
├── array
└── object
```

---

# 8. `useState` + TypeScript

This is especially important because you're learning React with TypeScript.

For a simple value, TypeScript can usually understand the type automatically:

```tsx
const [count, setCount] = useState(0);
```

TypeScript understands:

```text
count → number
```

For an array of Todos, we explicitly tell it:

```tsx
const [todos, setTodos] = useState<Todo[]>([]);
```

Meaning:

```text
todos
 ↓
array
 ↓
containing Todo objects
```

---

# 9. The Pattern to Remember

Whenever you see:

```tsx
const [value, setValue] = useState(initialValue);
```

read it as:

> **`value` is the current state, and `setValue` changes that state.**

Examples:

```text
count / setCount
name / setName
todos / setTodos
completed / setCompleted
```

The naming convention is usually:

```text
value
set + Value
```

---

# 10. One Important Concept

When you call:

```tsx
setCount(count + 1);
```

you are **not directly changing** `count`.

You're telling React:

> "I want the state to become this new value."

Then React updates the state and re-renders the component.

So think:

```text
setState()
   ↓
Request state change
   ↓
React updates state
   ↓
React re-renders
   ↓
UI reflects new state
```

---

## `useState` Summary

```text
useState
   ↓
Allows a component to have state
   ↓
State changes with setter function
   ↓
React re-renders
   ↓
UI updates
```

You already know the basic `useState` syntax from our Todo app. Now we've clarified **why it exists and how React uses it**.

### Next: `useEffect`

`useEffect` is used when your component needs to **do something after rendering**, such as fetching API data, setting up a timer, or reacting to a state/prop change.

-------------------------------------------------------------------------------------------------------------------------------------------

# React Hooks — Step 2: `useEffect`

Now we'll learn the second important Hook:

**`useEffect`**

If `useState` is used to **store data**, `useEffect` is mainly used to **perform something because the component rendered or because some value changed**.

> useEffect is used to perform side effects — things that happen outside the normal UI rendering.

---

# 1. What is `useEffect`?

`useEffect` lets you run some code **after React renders the component**.

For example:

```text id="m1q7vz"
Component renders
       ↓
React updates the screen
       ↓
useEffect runs
```

Think:

**`useEffect` → "After rendering, do this."**

---

# 2. Why Do We Need `useEffect`?

Some actions are not simply displaying UI.

For example:

* Fetch data from an API
* Start a timer
* Set up an event listener
* Update something outside React
* Save something to local storage

These are called **side effects**.

Example:

```text id="x4k9pa"
React component
      ↓
renders UI
      ↓
fetch data from API
      ↓
receive data
      ↓
update state
      ↓
React renders again
```

---

# 3. Basic Syntax

First import it:

```tsx id="7h2mqa"
import { useEffect } from "react";
```

Then inside the component:

```tsx id="8v5k3n"
useEffect(() => {
    console.log("Component rendered");
});
```

So:

```tsx id="1w6p9c"
function App() {
    useEffect(() => {
        console.log("Component rendered");
    });

    return <h1>Hello</h1>;
}
```

The function inside `useEffect` contains the work we want React to perform.

---

# 4. What Happens?

When `App` renders:

```text id="j5n8qx"
App renders
   ↓
<h1>Hello</h1>
   ↓
Browser updates
   ↓
useEffect runs
   ↓
"Component rendered"
```

---

# 5. The Dependency Array

This is the most important part of `useEffect`.

You will commonly see:

```tsx id="m3f7ka"
useEffect(() => {
    console.log("Hello");
}, []);
```

The `[]` is called the **dependency array**.

It controls **when the effect should run**.

---

# 6. Empty Dependency Array `[]`

```tsx id="n8r2x5"
useEffect(() => {
    console.log("Component loaded");
}, []);
```

The empty array means:

> Run this effect when the component is initially mounted.

Think:

```text id="j2v6s9"
Component starts
      ↓
useEffect runs
      ↓
Done
```

This is commonly used for things like:

```text id="x6k3m1"
Fetch initial API data
Load initial data
Set up something when component starts
```

---

# 7. Dependency Example

Suppose we have:

```tsx id="f8q4w2"
const [count, setCount] = useState(0);
```

And:

```tsx id="z7m1p5"
useEffect(() => {
    console.log("Count changed");
}, [count]);
```

Now React watches:

```text id="u5k8r3"
count
```

When `count` changes:

```text id="b4n7x2"
count = 0
    ↓
click
    ↓
count = 1
    ↓
useEffect runs
```

Again:

```text id="w2q9m6"
count = 1
    ↓
click
    ↓
count = 2
    ↓
useEffect runs
```

So:

**`[count]` means the effect depends on `count`.**

---

# 8. No Dependency Array vs Empty Array

This distinction is important.

### No dependency array

```tsx id="a8m4q2"
useEffect(() => {
    console.log("Effect");
});
```

Runs after **every render**.

```text id="p6x1v9"
Render
 ↓
Effect

Render
 ↓
Effect

Render
 ↓
Effect
```

---

### Empty dependency array

```tsx id="c5r8k2"
useEffect(() => {
    console.log("Effect");
}, []);
```

Runs when the component is initially mounted.

```text id="q7m3x5"
Component starts
 ↓
Effect
```

---

### Dependency `[count]`

```tsx id="v9k2n6"
useEffect(() => {
    console.log("Effect");
}, [count]);
```

Runs when `count` changes.

```text id="h3q8m1"
count changes
    ↓
Effect
```

---

# 9. API Example

This is where `useEffect` becomes very useful.

Suppose we want to load Todos from an API when our component starts.

The idea is:

```text id="s4m8q2"
App starts
   ↓
useEffect
   ↓
fetch API
   ↓
receive Todos
   ↓
setTodos()
   ↓
React updates UI
```

Conceptually:

```tsx id="j6n2p4"
useEffect(() => {
    fetch("API URL")
        .then(response => response.json())
        .then(data => {
            setTodos(data);
        });
}, []);
```

The `[]` means:

> Fetch the initial data when the component starts.

You already learned `fetch()` and `response.json()` in JavaScript, so `useEffect` is basically giving us a place to perform that API operation in a React component.

---

# 10. `useEffect` + `useState`

These two Hooks often work together.

For example:

```text id="n8q4w7"
useEffect
   ↓
Fetch API
   ↓
Data received
   ↓
setTodos(data)
   ↓
useState changes
   ↓
React re-renders
   ↓
Todos displayed
```

So:

**`useState` → stores the data**

**`useEffect` → performs the action that gets/updates the data**

---

# 11. Simple Real-Life Example

Imagine your React component is a person entering a room.

```text id="y7p3m9"
Component enters room
       ↓
useEffect
       ↓
"Now that I'm here, do this."
```

For example:

> "When this page opens, fetch the Todos."

That's why `useEffect` is commonly used for API calls.

---

# 12. Important: `useEffect` Is Not for Everything

Don't think:

> "Whenever I need to run a function, use `useEffect`."

No.

For example, clicking a button:

```tsx id="q2x7m5"
<button onClick={addTodo}>
    Add
</button>
```

does **not** need `useEffect`.

The user directly caused the action.

```text id="k8v3r1"
User click
   ↓
onClick
   ↓
addTodo()
```

`useEffect` is more about reacting to **rendering or changes in dependencies**.

---

# Easy Way to Remember

```text id="f6m2q8"
useState
   ↓
Store state
```

```text id="r9k4v1"
useEffect
   ↓
Perform a side effect
   ↓
after rendering / when dependencies change
```

Examples:

```text id="u3n7p5"
API request
Timer
Event listener
Local storage
External systems
```

---

# One Important Picture

```text id="w2j8m4"
             Component
                 ↓
              Render
                 ↓
             UI updates
                 ↓
             useEffect
                 ↓
          Perform side effect
                 ↓
          Maybe update state
                 ↓
              Re-render
```

### `useEffect` in one sentence:

**`useEffect` lets a React component perform side-effect work after rendering, optionally when specific values change.**

Next → **`useRef`**, where we'll learn how to keep a value or directly access a DOM element without causing a re-render.

------------------------------------------------------------------------------------------------------------------------------------------

# React Hooks — Step 2: `useEffect`

Now we'll learn the second important Hook:

**`useEffect`**

If `useState` is used to **store data**, `useEffect` is mainly used to **perform something because the component rendered or because some value changed**.

---

# 1. What is `useEffect`?

`useEffect` lets you run some code **after React renders the component**.

For example:

```text id="m1q7vz"
Component renders
       ↓
React updates the screen
       ↓
useEffect runs
```

Think:

**`useEffect` → "After rendering, do this."**

---

# 2. Why Do We Need `useEffect`?

Some actions are not simply displaying UI.

For example:

* Fetch data from an API
* Start a timer
* Set up an event listener
* Update something outside React
* Save something to local storage

These are called **side effects**.

Example:

```text id="x4k9pa"
React component
      ↓
renders UI
      ↓
fetch data from API
      ↓
receive data
      ↓
update state
      ↓
React renders again
```

---

# 3. Basic Syntax

First import it:

```tsx id="7h2mqa"
import { useEffect } from "react";
```

Then inside the component:

```tsx id="8v5k3n"
useEffect(() => {
    console.log("Component rendered");
});
```

So:

```tsx id="1w6p9c"
function App() {
    useEffect(() => {
        console.log("Component rendered");
    });

    return <h1>Hello</h1>;
}
```

The function inside `useEffect` contains the work we want React to perform.

---

# 4. What Happens?

When `App` renders:

```text id="j5n8qx"
App renders
   ↓
<h1>Hello</h1>
   ↓
Browser updates
   ↓
useEffect runs
   ↓
"Component rendered"
```

---

# 5. The Dependency Array

This is the most important part of `useEffect`.

You will commonly see:

```tsx id="m3f7ka"
useEffect(() => {
    console.log("Hello");
}, []);
```

The `[]` is called the **dependency array**.

It controls **when the effect should run**.

---

# 6. Empty Dependency Array `[]`

```tsx id="n8r2x5"
useEffect(() => {
    console.log("Component loaded");
}, []);
```

The empty array means:

> Run this effect when the component is initially mounted.

Think:

```text id="j2v6s9"
Component starts
      ↓
useEffect runs
      ↓
Done
```

This is commonly used for things like:

```text id="x6k3m1"
Fetch initial API data
Load initial data
Set up something when component starts
```

---

# 7. Dependency Example

Suppose we have:

```tsx id="f8q4w2"
const [count, setCount] = useState(0);
```

And:

```tsx id="z7m1p5"
useEffect(() => {
    console.log("Count changed");
}, [count]);
```

Now React watches:

```text id="u5k8r3"
count
```

When `count` changes:

```text id="b4n7x2"
count = 0
    ↓
click
    ↓
count = 1
    ↓
useEffect runs
```

Again:

```text id="w2q9m6"
count = 1
    ↓
click
    ↓
count = 2
    ↓
useEffect runs
```

So:

**`[count]` means the effect depends on `count`.**

---

# 8. No Dependency Array vs Empty Array

This distinction is important.

### No dependency array

```tsx id="a8m4q2"
useEffect(() => {
    console.log("Effect");
});
```

Runs after **every render**.

```text id="p6x1v9"
Render
 ↓
Effect

Render
 ↓
Effect

Render
 ↓
Effect
```

---

### Empty dependency array

```tsx id="c5r8k2"
useEffect(() => {
    console.log("Effect");
}, []);
```

Runs when the component is initially mounted.

```text id="q7m3x5"
Component starts
 ↓
Effect
```

---

### Dependency `[count]`

```tsx id="v9k2n6"
useEffect(() => {
    console.log("Effect");
}, [count]);
```

Runs when `count` changes.

```text id="h3q8m1"
count changes
    ↓
Effect
```

---

# 9. API Example

This is where `useEffect` becomes very useful.

Suppose we want to load Todos from an API when our component starts.

The idea is:

```text id="s4m8q2"
App starts
   ↓
useEffect
   ↓
fetch API
   ↓
receive Todos
   ↓
setTodos()
   ↓
React updates UI
```

Conceptually:

```tsx id="j6n2p4"
useEffect(() => {
    fetch("API URL")
        .then(response => response.json())
        .then(data => {
            setTodos(data);
        });
}, []);
```

The `[]` means:

> Fetch the initial data when the component starts.

You already learned `fetch()` and `response.json()` in JavaScript, so `useEffect` is basically giving us a place to perform that API operation in a React component.

---

# 10. `useEffect` + `useState`

These two Hooks often work together.

For example:

```text id="n8q4w7"
useEffect
   ↓
Fetch API
   ↓
Data received
   ↓
setTodos(data)
   ↓
useState changes
   ↓
React re-renders
   ↓
Todos displayed
```

So:

**`useState` → stores the data**

**`useEffect` → performs the action that gets/updates the data**

---

# 11. Simple Real-Life Example

Imagine your React component is a person entering a room.

```text id="y7p3m9"
Component enters room
       ↓
useEffect
       ↓
"Now that I'm here, do this."
```

For example:

> "When this page opens, fetch the Todos."

That's why `useEffect` is commonly used for API calls.

---

# 12. Important: `useEffect` Is Not for Everything

Don't think:

> "Whenever I need to run a function, use `useEffect`."

No.

For example, clicking a button:

```tsx id="q2x7m5"
<button onClick={addTodo}>
    Add
</button>
```

does **not** need `useEffect`.

The user directly caused the action.

```text id="k8v3r1"
User click
   ↓
onClick
   ↓
addTodo()
```

`useEffect` is more about reacting to **rendering or changes in dependencies**.

---

# Easy Way to Remember

```text id="f6m2q8"
useState
   ↓
Store state
```

```text id="r9k4v1"
useEffect
   ↓
Perform a side effect
   ↓
after rendering / when dependencies change
```

Examples:

```text id="u3n7p5"
API request
Timer
Event listener
Local storage
External systems
```

---

# One Important Picture

```text id="w2j8m4"
             Component
                 ↓
              Render
                 ↓
             UI updates
                 ↓
             useEffect
                 ↓
          Perform side effect
                 ↓
          Maybe update state
                 ↓
              Re-render
```

### `useEffect` in one sentence:

**`useEffect` lets a React component perform side-effect work after rendering, optionally when specific values change.**

Next → **`useRef`**, where we'll learn how to keep a value or directly access a DOM element without causing a re-render.


------------------------------------------------------------------------------------------------------------------------------------------

# React Hooks — Step 3: `useRef`

Now let's learn **`useRef`**.

This one is a little different from `useState`.

The easiest way to understand it is:

> **`useRef` lets you store a value that can change without causing the component to re-render.**

It is also commonly used to **access a DOM element directly**.

---

# 1. Basic Syntax

First import it:

```tsx
import { useRef } from "react";
```

Then inside the component:

```tsx
const valueRef = useRef(0);
```

This creates a ref.

You access the stored value using:

```tsx
valueRef.current
```

So:

```text id="9f5r2m"
valueRef
   ↓
.current
   ↓
stored value
```

---

# 2. `useRef` vs `useState`

This is the most important comparison.

### `useState`

```tsx
const [count, setCount] = useState(0);
```

When you do:

```tsx
setCount(1);
```

React re-renders the component.

```text id="j8v3q6"
setCount()
    ↓
State changes
    ↓
Re-render
    ↓
UI updates
```

### `useRef`

```tsx
const countRef = useRef(0);
```

When you do:

```tsx
countRef.current = 1;
```

React **does not re-render**.

```text id="w4k7p2"
ref.current changes
      ↓
No re-render
```

So remember:

```text id="m6x2r8"
useState
 ↓
Change value
 ↓
Re-render

useRef
 ↓
Change value
 ↓
No re-render
```

---

# 3. Why Would We Want That?

Sometimes we need to remember something, but changing it doesn't need to update the screen.

For example:

```text id="q7n3m5"
Previous value
Timer ID
DOM element
Some internal value
```

We can store these using `useRef`.

---

# 4. `useRef` to Access an Input

This is one of the most common examples.

Suppose we have:

```tsx
<input />
```

Normally, React renders the input.

But sometimes we want to directly access that input from JavaScript.

For example:

> When the page opens, automatically put the cursor inside the input.

We can use `useRef`.

---

# 5. Create the Ref

```tsx
const inputRef = useRef<HTMLInputElement>(null);
```

Because you're using TypeScript, we tell TypeScript:

```text id="b8m4q2"
inputRef
   ↓
HTMLInputElement
```

The initial value is:

```text id="h3v7n9"
null
```

because when the component first starts, the input hasn't been connected to the ref yet.

---

# 6. Connect Ref to Input

We use:

```tsx
<input ref={inputRef} />
```

Now:

```text id="v5k2p8"
inputRef
   ↓
<input>
```

React connects the ref to the actual DOM element.

---

# 7. Access the Input

Now we can do:

```tsx
inputRef.current?.focus();
```

This means:

> If the input exists, focus it.

So the cursor goes into the input.

---

# 8. Complete Example

```tsx
import { useEffect, useRef } from "react";

function App() {
    const inputRef = useRef<HTMLInputElement>(null);

    useEffect(() => {
        inputRef.current?.focus();
    }, []);

    return (
        <div>
            <h1>Todo App</h1>

            <input ref={inputRef} />

            <button>Add</button>
        </div>
    );
}

export default App;
```

When the component loads:

```text id="e7m3q9"
App renders
    ↓
<input> created
    ↓
inputRef connects to input
    ↓
useEffect runs
    ↓
inputRef.current
    ↓
focus()
    ↓
Cursor appears in input
```

---

# 9. Why `HTMLInputElement`?

You asked about things like:

```tsx
as HTMLButtonElement
```

earlier.

Here we're using:

```tsx
useRef<HTMLInputElement>(null)
```

`HTMLInputElement` is the TypeScript type for an HTML `<input>` element.

There are many DOM element types:

```text
<input>       → HTMLInputElement
<button>      → HTMLButtonElement
<div>         → HTMLDivElement
<form>        → HTMLFormElement
<select>      → HTMLSelectElement
<textarea>    → HTMLTextAreaElement
<img>         → HTMLImageElement
<a>           → HTMLAnchorElement
```

You don't need to memorize all of them.

TypeScript/your editor can help you find the appropriate type when needed.

---

# 10. Another Important Difference

Suppose we have:

```tsx
const [name, setName] = useState("");
```

If:

```tsx
setName("William");
```

React re-renders.

But:

```tsx
const nameRef = useRef("");

nameRef.current = "William";
```

React does **not** re-render.

So if you want the **screen to update**, use state.

If you simply need to **remember something without triggering a render**, a ref can be appropriate.

---

# 11. Easy Example

Imagine you have:

```text id="w9k4p2"
Timer
```

You might want to remember the timer ID:

```text id="t3m8q6"
timerRef.current = timer ID
```

Changing that value doesn't need to update the UI.

So `useRef` is useful.

---

# 12. The Two Main Uses of `useRef`

### Use 1 — Access DOM elements

```text id="r8m3v5"
useRef
 ↓
<input>
 ↓
focus()
```

### Use 2 — Store a value without re-rendering

```text id="k6q2n9"
useRef
 ↓
.current
 ↓
remember a value
 ↓
no re-render
```

---

# Easy Way to Remember

```text id="v3m7x2"
useState
   ↓
Data needed for UI
   ↓
Changing it → re-render
```

```text id="j8p4k1"
useRef
   ↓
Value/DOM reference
   ↓
Changing it → no re-render
```

### One sentence:

**`useRef` is mainly used to hold a mutable value across renders or to access a DOM element directly, without causing a re-render when the ref changes.**

Next → **`useMemo`**, where we'll learn how React can avoid repeating expensive calculations unnecessarily.


------------------------------------------------------------------------------------------------------------------------------------------


# React Hooks — Step 4: `useMemo`

Now let's learn **`useMemo`**.

This Hook is mainly about **performance**.

The basic idea is:

> **`useMemo` remembers the result of a calculation so React doesn't have to calculate it again unnecessarily.**

---

# 1. The Problem

Suppose we have a calculation:

```text id="j8f2m4"
numbers
   ↓
calculate something
   ↓
result
```

For example:

```tsx id="k4n7p2"
const total = calculateTotal(numbers);
```

Every time the component re-renders, this calculation runs again.

Even if `numbers` didn't change.

```text id="v3m8q1"
Component re-renders
       ↓
calculateTotal()
       ↓
same result
```

If the calculation is expensive, this can waste time.

---

# 2. What Does `useMemo` Do?

`useMemo` remembers the calculated result.

```tsx id="p6r2x8"
const total = useMemo(() => {
    return calculateTotal(numbers);
}, [numbers]);
```

Now React thinks:

> "I already calculated this. Has `numbers` changed?"

If `numbers` **hasn't changed**, React can reuse the previous result.

```text id="n5k3w7"
numbers unchanged
      ↓
useMemo
      ↓
use previous result
```

If `numbers` changes:

```text id="q2m8v4"
numbers changed
      ↓
calculate again
      ↓
save new result
```

---

# 3. Basic Syntax

```tsx id="z7x4m1"
const result = useMemo(() => {
    return someCalculation();
}, [dependency]);
```

There are three important parts:

```text id="a6p9k3"
useMemo
  │
  ├── calculation
  │
  └── dependencies
```

---

# 4. Simple Example

Suppose:

```tsx id="x3n8q5"
const [count, setCount] = useState(0);
const [name, setName] = useState("");
```

We have a calculation based on `count`:

```tsx id="r7m2v9"
const doubled = useMemo(() => {
    return count * 2;
}, [count]);
```

Now:

```text id="s8k4p2"
count = 5
   ↓
doubled = 10
```

If `name` changes:

```text id="v5n9q1"
name changes
   ↓
component re-renders
   ↓
count didn't change
   ↓
useMemo can reuse 10
```

If `count` changes:

```text id="m3x7k8"
count changes
   ↓
useMemo calculates again
```

---

# 5. Why Not Just Use a Normal Variable?

You might ask:

> Why do we need `useMemo`? Why not just write `const doubled = count * 2`?

For a simple calculation like:

```tsx id="h2q6w9"
const doubled = count * 2;
```

**You should just use the normal calculation.**

You don't need `useMemo`.

This is very important.

---

# 6. `useMemo` Is for Expensive Calculations

Imagine we have:

```text id="p8m4r2"
10 items
 ↓
easy calculation
```

No problem.

But imagine:

```text id="q6v2n8"
1,000,000 items
       ↓
complex calculation
       ↓
expensive
```

We don't want to repeat that expensive calculation every time the component renders if the relevant data hasn't changed.

That's where `useMemo` can help.

---

# 7. Real Example: Filtering Todos

Imagine our Todo app has:

```text id="c5m9x3"
todos
│
├── Learn HTML
├── Learn CSS
├── Learn React
└── Learn TypeScript
```

We want to show only completed Todos.

We could calculate:

```tsx id="u7k2p5"
const completedTodos = todos.filter(
    todo => todo.completed
);
```

If the list is very large and the filtering is expensive, we could use:

```tsx id="w4n8q1"
const completedTodos = useMemo(() => {
    return todos.filter(todo => todo.completed);
}, [todos]);
```

Now:

```text id="s2m6v9"
todos changes
     ↓
filter again
```

But if some unrelated state changes:

```text id="r8q3k5"
unrelated state changes
     ↓
component re-renders
     ↓
todos didn't change
     ↓
reuse previous filtered result
```

---

# 8. `useMemo` Does NOT Stop Re-rendering

This is important.

Some beginners think:

> "`useMemo` prevents my component from re-rendering."

No.

The component can still re-render.

`useMemo` only helps avoid **repeating a calculation**.

Think:

```text id="g4p7m2"
Component re-renders
        ↓
useMemo checks dependencies
        ↓
Dependency changed?
   ↙             ↘
 Yes             No
  ↓               ↓
Calculate      Use previous
again           result
```

---

# 9. Compare the Hooks We've Learned

Now we have:

### `useState`

```text id="a5k8q2"
Store state
   ↓
State changes
   ↓
Re-render
```

### `useEffect`

```text id="m3v7n1"
Perform side effect
   ↓
After render / dependency changes
```

### `useRef`

```text id="x8q2p6"
Store value/reference
   ↓
Changing it doesn't re-render
```

### `useMemo`

```text id="j4n9r3"
Remember calculation result
   ↓
Avoid unnecessary recalculation
```

---

# 10. Easy Way to Remember

```text id="q7m2v5"
useState
→ Store UI state
```

```text id="n4k8x1"
useEffect
→ Perform side effects
```

```text id="p3r6m9"
useRef
→ Remember value / access DOM
```

```text id="w8q2j4"
useMemo
→ Remember calculation result
```

### One sentence:

**`useMemo` caches the result of a calculation and recalculates it when its dependencies change.**

---

# Important Rule

Don't use `useMemo` everywhere.

For example:

```tsx id="z6m3p8"
const fullName = firstName + " " + lastName;
```

No need for `useMemo`.

That's a tiny calculation.

Use it when there is a **real performance reason** to avoid an expensive recalculation.

---

Next → **`useCallback`**. This one is closely related to `useMemo`, but instead of remembering a **calculated value**, it remembers a **function**.


------------------------------------------------------------------------------------------------------------------------------------------


# React Hooks — Step 5: `useCallback`

Now we'll learn the last Hook in our core Hooks list:

**`useCallback`**

This is closely related to `useMemo`, so we'll compare them carefully.

---

# 1. What is `useCallback`?

`useCallback` is used to **remember a function** between renders.

Think:

```text
useMemo
   ↓
Remember a value/result

useCallback
   ↓
Remember a function
```

---

# 2. The Problem

Remember that React components can re-render.

Suppose we have:

```tsx
function App() {
    const handleClick = () => {
        console.log("Clicked");
    };

    return <Button onClick={handleClick} />;
}
```

Every time `App` renders, a **new function** is created:

```text
Render 1
 ↓
handleClick → Function A

Render 2
 ↓
handleClick → Function B

Render 3
 ↓
handleClick → Function C
```

Even though the function does exactly the same thing.

Usually, this is completely fine.

But sometimes we want React to keep the **same function reference** between renders.

That's where `useCallback` comes in.

---

# 3. Basic Syntax

```tsx
const handleClick = useCallback(() => {
    console.log("Clicked");
}, []);
```

The empty dependency array means:

> Keep the same function reference unless the dependencies change.

---

# 4. Dependency Example

Suppose our function uses `count`:

```tsx
const handleClick = useCallback(() => {
    console.log(count);
}, [count]);
```

Now React watches:

```text
count
```

If `count` doesn't change:

```text
Render
 ↓
same function reference
```

If `count` changes:

```text
count changes
 ↓
new function is created
```

So:

```text
Dependencies unchanged
        ↓
same function

Dependencies changed
        ↓
new function
```

---

# 5. Why Does Function Identity Matter?

This becomes important when passing functions to **child components**.

Imagine:

```text
App
 ↓
Button
```

`App` passes:

```text
handleClick
```

to `Button`.

If `App` re-renders, the function may be recreated.

```text
App re-renders
     ↓
new handleClick function
     ↓
Button receives a different function
```

In certain optimized React applications, this can cause unnecessary work.

`useCallback` can help keep the function reference stable.

---

# 6. `useCallback` + `React.memo`

This is where `useCallback` is commonly useful.

Imagine:

```text
App
 ↓
Button
```

The `Button` component is optimized using `React.memo`.

If the props haven't changed, React can skip unnecessary rendering of `Button`.

But if we create a new function every time:

```text
App renders
 ↓
new handleClick
 ↓
Button receives new function
 ↓
Button may render again
```

With `useCallback`:

```text
App renders
 ↓
same handleClick
 ↓
Button receives same function
 ↓
React can potentially skip Button render
```

So `useCallback` is mostly useful in **performance optimization scenarios**.

---

# 7. Important: Don't Use `useCallback` Everywhere

This is very important.

You might think:

> "I should put every function inside `useCallback`."

No.

For normal functions:

```tsx
function addTodo() {
    ...
}
```

there is usually no reason to use `useCallback`.

Just use the function normally.

`useCallback` is mainly useful when **function identity matters**, especially when passing callbacks to memoized child components or when a function is itself a dependency of another Hook.

---

# 8. `useMemo` vs `useCallback`

This is the easiest way to understand the difference.

### `useMemo`

Remembers the **result**:

```tsx
const total = useMemo(() => {
    return calculateTotal(items);
}, [items]);
```

Think:

```text
Calculation
   ↓
Result
   ↓
Remember result
```

---

### `useCallback`

Remembers the **function**:

```tsx
const handleClick = useCallback(() => {
    doSomething();
}, []);
```

Think:

```text
Function
   ↓
Remember function
```

---

# 9. Simple Comparison

```text
useMemo
   ↓
"What is the result?"

useCallback
   ↓
"What is the function?"
```

Or:

```text
useMemo     → remembers a value
useCallback → remembers a function
```

---

# 10. Example Together

Suppose we have:

```tsx
const filteredTodos = useMemo(() => {
    return todos.filter(todo => todo.completed);
}, [todos]);

const handleDelete = useCallback((id: number) => {
    setTodos(currentTodos =>
        currentTodos.filter(todo => todo.id !== id)
    );
}, []);
```

Here:

```text
filteredTodos
     ↓
useMemo
     ↓
remember calculated result
```

And:

```text
handleDelete
     ↓
useCallback
     ↓
remember function
```

---

# 11. Our Four Main Hooks

We have now covered all five Hooks from our roadmap.

### `useState`

```text
Store state
```

Example:

```text
todos
count
name
```

---

### `useEffect`

```text
Perform side effects
```

Example:

```text
API request
timer
external system
```

---

### `useRef`

```text
Store a value without causing re-render
or access a DOM element
```

Example:

```text
inputRef
timerRef
```

---

### `useMemo`

```text
Remember a calculated value
```

Example:

```text
filteredTodos
totalPrice
```

---

### `useCallback`

```text
Remember a function
```

Example:

```text
handleDelete
handleClick
handleSubmit
```

---

# 12. Final Picture

```text
                 React Hooks
                     │
       ┌─────────────┼─────────────┐
       │             │             │
   useState       useEffect      useRef
       │             │             │
   Store state    Side effects   Value / DOM
       
       ┌───────────────────────────┐
       │                           │
    useMemo                   useCallback
       │                           │
   Remember value             Remember function
```

### Easy memory trick

**State → store**

**Effect → do**

**Ref → remember/access**

**Memo → remember result**

**Callback → remember function**

---

## React Hooks — Core: Completed

Your original roadmap was:

```text
React Hooks — Core

useState      
useEffect     
useRef        
useMemo       
useCallback   
```

------------------------------------------------------------------------------------------------------------------------------------------


# `React.memo`

`React.memo` is a **performance optimization** for a React component.

The simple idea is:

> **If a component receives the same Props as before, React can skip rendering that component again.**

---

# 1. Normal Component

Suppose we have:

```tsx
function Child({ name }: { name: string }) {
    console.log("Child rendered");

    return <h1>Hello {name}</h1>;
}
```

And:

```tsx
function App() {
    const [count, setCount] = useState(0);

    return (
        <div>
            <button onClick={() => setCount(count + 1)}>
                {count}
            </button>

            <Child name="William" />
        </div>
    );
}
```

When `count` changes:

```text
count changes
     ↓
App re-renders
     ↓
Child also renders
```

Even though:

```text
Child's name = "William"
```

didn't change.

---

# 2. Using `React.memo`

We can wrap the child:

```tsx
const Child = React.memo(function Child({ name }: { name: string }) {
    console.log("Child rendered");

    return <h1>Hello {name}</h1>;
});
```

Now React checks the Props.

```text
App re-renders
      ↓
React.memo checks Child Props
      ↓
Did Props change?
   ↙          ↘
 No           Yes
 ↓             ↓
Skip          Render
```

So if:

```text
name = "William"
```

was before and is still:

```text
name = "William"
```

React can skip rendering `Child`.

---

# 3. Now `useCallback` Makes Sense

This connects directly to your previous question.

Suppose:

```tsx
function App() {
    const [count, setCount] = useState(0);

    const handleClick = useCallback(() => {
        console.log("Clicked");
    }, []);

    return (
        <div>
            <button onClick={() => setCount(count + 1)}>
                {count}
            </button>

            <Child onClick={handleClick} />
        </div>
    );
}
```

And:

```tsx
const Child = React.memo(function Child({ onClick }) {
    console.log("Child rendered");

    return <button onClick={onClick}>Click</button>;
});
```

Now:

```text
count changes
     ↓
App re-renders
     ↓
useCallback
     ↓
same handleClick reference
     ↓
React.memo checks Child Props
     ↓
Props haven't changed
     ↓
Child skips re-render
```

That's where **`useCallback` + `React.memo`** can work together.

---

# 4. Without `useCallback`

Suppose we don't use it:

```tsx
const handleClick = () => {
    console.log("Clicked");
};
```

Every time `App` renders:

```text
Render 1
 ↓
Function A

Render 2
 ↓
Function B

Render 3
 ↓
Function C
```

Even though the function does the same thing, these are different function references.

So `React.memo` sees:

```text
Previous onClick → Function A
Current onClick  → Function B
```

and thinks:

> "The Props changed."

Therefore the child can re-render.

---

# 5. With `useCallback`

```tsx
const handleClick = useCallback(() => {
    console.log("Clicked");
}, []);
```

Now:

```text
Render 1
 ↓
Function A

Render 2
 ↓
Function A

Render 3
 ↓
Function A
```

Same function reference.

So:

```text
React.memo
    ↓
Props same
    ↓
Skip Child render
```

---

# 6. Very Important

Don't think:

```text
React.memo = always prevent re-render
```

That's not exactly correct.

`React.memo` says:

> **"If the Props are the same, React can skip rendering this component."**

If the Props change:

```text
Props changed
   ↓
Child renders
```

And if the component has its **own state** that changes, it can still re-render.

---

# 7. Easy Comparison

Now you can understand these two together:

### `React.memo`

```text
Checks component Props
        ↓
Props same?
        ↓
Potentially skip child render
```

### `useCallback`

```text
Remember a function
        ↓
Function reference stays the same
        ↓
React.memo can see that the function Prop hasn't changed
```

So:

```text
useCallback
    +
React.memo
    ↓
Can reduce unnecessary child re-renders
```

---

## One-line memory trick

**`React.memo` → "Don't re-render this child if its Props haven't changed."**

**`useCallback` → "Keep this function reference the same if its dependencies haven't changed."**

And importantly, **neither one is something you need to use everywhere**. They're mainly optimization tools when unnecessary rendering is actually a concern.


------------------------------------------------------------------------------------------------------------------------------------------