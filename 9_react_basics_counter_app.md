## React Project 1 — Counter App

- For the **React + TypeScript Counter learning path** we're following :

### Counter App — 7 Steps

1. **Clean the default Vite `App.tsx`**

   * Remove the starter code.

2. **Create the Counter UI**

   * Display `Count: 0`
   * Add `+` and `-` buttons.

3. **Add `useState`**

   * Store the counter value.
   * Example: `count = 0`.

4. **Add `onClick` event handling**

   * `+` button increases the count.
   * `-` button decreases the count.

5. **Understand state → UI update**

   * `setCount()` changes state.
   * React re-renders the component.

6. **Add conditional rendering**

   * For example:
   * Even → `"Even"`
   * Odd → `"Odd"`

7. **Final Counter App**

   * Put everything together and make sure you understand the complete flow.

### Complete flow

```text
App.tsx
   ↓
UI
   ↓
useState
   ↓
onClick
   ↓
setCount()
   ↓
State changes
   ↓
React re-renders
   ↓
Conditional rendering
   ↓
Updated UI
```

- We'll do **one step at a time**, just like we did with the Todo app.


We'll first set up the React + TypeScript project using **Vite**, then I'll explain each important file in simple terms before we write any React code.

-------------------------------------------------------------------------------------------------------------------------------------------
### Step 1 - setup 

#### Option 1 - Your folder is empty

If you **already created a folder**, you have two options.

Open the terminal **inside that folder**, then run:

```text
npm create vite@latest .
```

The `.` means:

> **Create the Vite project in the current folder.**

---

#### option 2 - Open your terminal

Go to the folder where you want to create your React project.

Then run:

```text
npm create vite@latest
```

Vite will ask you a few questions.

Choose:

```text
Project name: react-counter
Framework: React
Variant: TypeScript
Linter    → ESLint
```
> ESLint is useful because it will help us catch common coding mistakes while we're learning React.

Then go into the project:

```text
cd react-counter
```

Install the required packages:

```text
npm install
```

Then start the development server:

```text
npm run dev
```

You'll get a local address in the terminal, usually something like:

```text
http://localhost:5173/
```

Open that address in your browser.

### Your flow is:

```text
npm create vite@latest
        ↓
react-counter
        ↓
React
        ↓
TypeScript
        ↓
npm install
        ↓
npm run dev
        ↓
Browser
```

##### Project Files

- **node_modules** – installed dependencies
- **public** – static assets served as-is
- **src** – your source code
- **.gitignore** – files git should ignore
- **eslint.config** – linting rules
- **index.html** – app entry HTML
- **package.json** – project deps/scripts
- **package-lock.json** – exact dependency versions
- **README.md** – project docs
- **tsconfig.app.json** – TS config for app code
- **tsconfig.json** – base TS config
- **tsconfig.node.json** – TS config for node/build scripts
- **vite.config.ts** – Vite build config

**Don't change `App.tsx` yet.**

First create the project and make sure the default Vite/React page opens in your browser. Then tell me **done**, and we'll inspect `App.tsx` together.

-------------------------------------------------------------------------------------------------------------------------------------------

# Step 2 — Understand `App.tsx`

Now that the React + TypeScript project is running, let's look at the most important file first:

```text
src/
└── App.tsx
```

Open `App.tsx`.

You will see some default Vite code. **Don't worry about all of it.** We are going to understand the important idea first.

## What is `App.tsx`?

`App.tsx` contains a React component called `App`.

Conceptually:

```text
App.tsx
   ↓
App component
   ↓
returns JSX
   ↓
browser displays it
```

For example, if we have:

```tsx
function App() {
    return <h1>Hello React</h1>;
}
```

Then the browser displays:

```text
Hello React
```

---

## Why is it called `App`?

`App` is simply the name of our component.

Remember what we learned:

> A React functional component is a function that returns JSX.

So:

```tsx
function App() {
    return <h1>Hello React</h1>;
}
```

is:

```text
function
   ↓
App component
   ↓
returns JSX
```

---

## What about `main.tsx`?

There is another important file:

```text
src/
├── App.tsx
└── main.tsx
```

`main.tsx` is responsible for **starting React and rendering the `App` component**.

Think of it like this:

```text
main.tsx
   ↓
starts React
   ↓
App
   ↓
JSX
   ↓
Browser
```

So you can think:

**`main.tsx` = starts the React application**

**`App.tsx` = contains our main component**

---

# Our Counter App

We are going to put most of our Counter learning inside:

```text
src/App.tsx
```

Eventually the flow will be:

```text
main.tsx
    ↓
App.tsx
    ↓
Counter UI
    ↓
useState
    ↓
Events
    ↓
UI updates
```

### For now

Don't create another file or component.

We'll keep the Counter inside `App.tsx` so you can focus on understanding React rather than project structure.

**Next:** We'll clean the default `App.tsx` and create our first simple Counter UI.

-------------------------------------------------------------------------------------------------------------------------------------------

# Step 3 — Add `useState`

Right now, our counter might just display:

```text
Count: 0
```

But we want the `0` to be **changeable**.

That's where `useState` comes in.

Inside `App.tsx`, we use:

```tsx
const [count, setCount] = useState(0);
```

Let's break this down:

```text
count
  ↓
current counter value

setCount
  ↓
function used to change counter value

0
  ↓
initial value
```

So when the app starts:

```text
count = 0
```

---

### Then use `count` in JSX

Instead of writing:

```tsx
<h1>Count: 0</h1>
```

we use:

```tsx
<h1>Count: {count}</h1>
```

Remember our JSX lesson:

```text
{count}
   ↓
JavaScript value inside JSX
```

So the UI becomes:

```text
Count: 0
```

---

### Important

At this step, **don't add the button functionality yet**.

We are only doing:

```text
useState
   ↓
store count
   ↓
display count
```

The `+` and `-` buttons will become functional in **Step 4 — Event Handling**.

Once you have added `useState` and the app still shows `Count: 0`, tell me **done**, and we'll move to Step 4.

-------------------------------------------------------------------------------------------------------------------------------------------

# Step 4 — Event Handling

Now we'll make the **`+` button actually work**.

You already learned event handling, so let's connect it with `useState`.

We have:

```tsx
const [count, setCount] = useState(0);
```

Remember:

```text
count
  ↓
current value

setCount
  ↓
changes the value
```

## Make the `+` button work

We use React's `onClick`:

```tsx
<button onClick={() => setCount(count + 1)}>
    +
</button>
```

Read it like this:

> **When the user clicks `+`, increase `count` by 1.**

### The flow

Initially:

```text
count = 0
```

User clicks `+`:

```text
onClick
   ↓
setCount(count + 1)
   ↓
setCount(1)
   ↓
count = 1
   ↓
React updates the UI
```

Now the screen shows:

```text
Count: 1
```

Click again:

```text
count = 1
   ↓
setCount(2)
   ↓
Count: 2
```

---

## Now the `-` button

We do the opposite:

```tsx
<button onClick={() => setCount(count - 1)}>
    -
</button>
```

So:

```text
+ → count + 1

- → count - 1
```

---

## The important connection

You have now combined **two React concepts**:

```text
useState
   ↓
stores count
   ↓
onClick
   ↓
calls setCount()
   ↓
state changes
   ↓
React updates UI
```

This is the main pattern I wanted you to understand from the Counter App.

### Your Counter currently has:

```text
Count: 0

[ - ]  [ + ]
```

Click `+` → `1`

Click `+` → `2`

Click `-` → `1`

Don't worry about conditional rendering yet.

**Step 5 will be understanding this state → UI update flow in the actual Counter App**, so you can explain what is happening rather than just making it work.


-------------------------------------------------------------------------------------------------------------------------------------------

# Step 5 — Understand State → UI Update

Your Counter is working now. The important thing is to understand **why the number on the screen changes**.

We have:

```tsx
const [count, setCount] = useState(0);
```

At the beginning:

```text
count = 0
```

And our JSX has:

```tsx
<h1>Count: {count}</h1>
```

So React displays:

```text
Count: 0
```

---

## When you click `+`

You have:

```tsx
<button onClick={() => setCount(count + 1)}>
    +
</button>
```

Suppose `count` is `0`.

The click happens:

```text
User clicks +
      ↓
onClick runs
      ↓
setCount(count + 1)
      ↓
setCount(1)
      ↓
count becomes 1
```

Now React **renders the component again**.

The JSX is evaluated again:

```tsx
<h1>Count: {count}</h1>
```

But now:

```text
count = 1
```

So React displays:

```text
Count: 1
```

---

## Why is this important?

React's basic idea is:

> **When state changes, React re-renders the component so the UI reflects the new state.**

Think:

```text
State
count = 0
   ↓
UI
Count: 0

       ↓ setCount(1)

State
count = 1
   ↓
React re-renders
   ↓
UI
Count: 1
```

That's why we don't manually do something like:

```text
change the text on the <h1>
```

Instead, we change the **state**, and React takes care of updating the UI.

---

## This is the pattern you should remember

```text
User action
    ↓
Event
    ↓
State setter
    ↓
State changes
    ↓
React re-renders
    ↓
UI reflects new state
```

For our Counter:

```text
Click +
  ↓
onClick
  ↓
setCount(count + 1)
  ↓
count changes
  ↓
React re-renders
  ↓
Count increases
```

You have now understood the **core React cycle**.

Next is **Step 6 — Conditional Rendering**, where we'll make the Counter display something different depending on whether the count is even or odd.


-------------------------------------------------------------------------------------------------------------------------------------------

# Step 6 — Conditional Rendering in the Counter

Now we'll use the **conditional rendering** concept we already learned.

We'll make the Counter display:

```text
Count: 2
Even
```

or:

```text
Count: 3
Odd
```

## 1. We already have `count`

```tsx id="y3y4iy"
const [count, setCount] = useState(0);
```

The value changes:

```text
0 → 1 → 2 → 3 → 4
```

---

## 2. Check whether the number is even or odd

In JavaScript, you can use `%`:

```text id="i5e31b"
2 % 2 = 0  → Even
3 % 2 = 1  → Odd
4 % 2 = 0  → Even
```

So inside JSX, we can use a ternary:

```tsx id="6igxjv"
<p>{count % 2 === 0 ? "Even" : "Odd"}</p>
```

Read it as:

> If `count` divided by 2 has a remainder of `0`, show `"Even"`, otherwise show `"Odd"`.

---

## 3. Complete flow

Suppose:

```text id="17oxu5"
count = 0
```

React checks:

```text id="q3o0xw"
0 % 2 === 0
      ↓
true
      ↓
Even
```

Screen:

```text id="1l4qxr"
Count: 0
Even
```

Click `+`:

```text id="k1t7qt"
count = 1
```

React checks:

```text id="e8kqvb"
1 % 2 === 0
      ↓
false
      ↓
Odd
```

Screen:

```text id="g5b8ga"
Count: 1
Odd
```

Click `+` again:

```text id="wz0qkf"
count = 2
```

Screen:

```text id="xw2gqa"
Count: 2
Even
```

---

# What have we combined?

Our Counter now uses almost everything from React Basics:

```text id="g4t7or"
Functional Component
       ↓
JSX
       ↓
useState
       ↓
Event Handling
       ↓
State changes
       ↓
Conditional Rendering
       ↓
UI updates
```

This is exactly why we built the Counter App.

You aren't just learning individual topics anymore — you're seeing how they **work together**.

### Step 7 is the final step

We'll do a **complete Counter App review** and make sure you can explain the entire code yourself before moving to the next project: **React + TypeScript Todo App**.


-------------------------------------------------------------------------------------------------------------------------------------------

# Step 7 — Complete Counter App Review

Now let's put everything together and make sure you understand **what each part is doing**.

Your Counter App has these concepts:

```text
Component
JSX
useState
Event handling
State update
Conditional rendering
```

## 1. Component

Everything is inside our `App` functional component.

```tsx
function App() {
    ...
}
```

Remember:

> A functional component is a function that returns JSX.

---

## 2. State

Inside the component:

```tsx
const [count, setCount] = useState(0);
```

Meaning:

```text
count
  ↓
current value

setCount
  ↓
function to change count

0
  ↓
initial value
```

---

## 3. Display the count

```tsx
<h1>Count: {count}</h1>
```

`{count}` means:

> Put the current JavaScript value of `count` into the JSX.

---

## 4. Increase the count

```tsx
<button onClick={() => setCount(count + 1)}>
    +
</button>
```

Flow:

```text
Click +
 ↓
onClick
 ↓
setCount(count + 1)
 ↓
count changes
 ↓
React re-renders
 ↓
UI updates
```

---

## 5. Decrease the count

```tsx
<button onClick={() => setCount(count - 1)}>
    -
</button>
```

Same idea:

```text
Click -
 ↓
setCount(count - 1)
 ↓
count decreases
 ↓
React re-renders
 ↓
UI updates
```

---

## 6. Conditional rendering

We can display whether the number is even or odd:

```tsx
<p>
    {count % 2 === 0 ? "Even" : "Odd"}
</p>
```

So:

```text
count = 4
 ↓
Even

count = 5
 ↓
Odd
```

---

# The complete mental picture

This is the most important thing to remember:

```text
                App
                 ↓
              useState
                 ↓
            count = 0
                 ↓
              JSX
                 ↓
        ┌───────────────┐
        │   Count: 0    │
        │               │
        │  [-]    [+]   │
        │               │
        │     Even      │
        └───────────────┘
                 ↑
                 │
             User clicks
                 │
              onClick
                 │
             setCount()
                 │
           state changes
                 │
          React re-renders
                 │
                 └──────→ UI updates
```

### Before moving on, make sure you can answer these:

1. **What is `App`?**
2. **What does `useState(0)` do?**
3. **What is `count`?**
4. **What is `setCount`?**
5. **What does `onClick` do?**
6. **Why does the UI change when `setCount()` is called?**
7. **What is conditional rendering?**
8. **Why do we use `{count}` inside JSX?**

If you understand these, **our Counter App is complete**.

The next project from your roadmap is **React + TypeScript Todo App**.

------------------------------------------------------------------------------------------------------------------------------------------