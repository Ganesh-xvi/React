# Our React Basics order will be:

**Step 1 — Component Architecture**

* What is a component?
* Functional components
* How components fit together

**Step 2 — JSX**

* JSX
* JSX expressions
* JSX vs HTML

**Step 3 — Props**

* Passing props
* Receiving props
* TypeScript props

**Step 4 — `useState`**

* What is state?
* Creating state
* Updating state
* How React re-renders

**Step 5 — Event Handling**

* `onClick`
* `onChange`
* `onSubmit`
* Event objects
* Passing functions to events

**Step 6 — Conditional Rendering**

* `if`
* Ternary
* `&&`
* Showing/hiding UI

**Step 7 — Lists + Keys**

* `.map()`
* Rendering arrays
* Why `key` is needed

**Step 8 — Build**

* Counter app
* Todo app with TypeScript

------------------------------------------------------------------------------------------------------------------------------------------


# Step 1 — Components

Now we officially start **React**.

The first important concept is:

> **A React application is built from components.**

## What is a component?

A component is basically a **reusable piece of UI**.

For example, imagine a Todo application:

```text
Todo App
│
├── Header
├── TodoInput
├── TodoList
│   ├── TodoItem
│   ├── TodoItem
│   └── TodoItem
└── Footer
```

Each of these can be a React component.

---

## A simple component

```tsx
function Welcome() {
    return <h1>Hello World</h1>;
}
```

Here:

```text
function Welcome()
       ↓
   React Component
```

And:

```tsx
<h1>Hello World</h1>
```

is JSX.

So a component usually does two things:

1. **Contains UI logic**
2. **Returns JSX**

---

## Why use components?

Imagine you have 20 Todo items.

Instead of writing the same HTML 20 times, you create one component:

```tsx
function TodoItem() {
    return <li>Learn React</li>;
}
```

Then React can use that component wherever needed.

Think:

```text
TodoItem component
       ↓
   reusable
       ↓
Todo 1
Todo 2
Todo 3
Todo 4
...
```

---

# Component names

React components normally start with a **capital letter**.

Correct:

```tsx
function TodoItem() {
    return <li>Learn React</li>;
}
```

Not:

```tsx
function todoItem() {
```

The capital letter helps React distinguish a component from a normal HTML element.

Think:

```text
<div>       → HTML element

<TodoItem>  → React component
```

---

# Components can contain other components

For example:

```tsx
function TodoItem() {
    return <li>Learn React</li>;
}

function TodoList() {
    return (
        <ul>
            <TodoItem />
            <TodoItem />
        </ul>
    );
}
```

Here:

```text
TodoList
   ↓
   ├── TodoItem
   └── TodoItem
```

So components can be **combined to build larger components**.

---

## The important idea

Don't think of React as:

> "One huge HTML page."

Think of it as:

> **"A collection of small reusable components."**

For our Todo app, we'll eventually have something like:

```text
App
│
├── TodoInput
└── TodoList
      │
      ├── TodoItem
      ├── TodoItem
      └── TodoItem
```

That's the basic idea behind **component architecture**.

------------------------------------------------------------------------------------------------------------------------------------------


# React Step 1.2 — Why Do We Need Components?

You now understand:

> **A functional component is a function that returns JSX.**

Now the next question is:

**Why not just put everything inside one function?**

Imagine our Todo app has:

```text
Todo App
├── Header
├── Input
├── Add Button
├── Todo List
└── Todo Item
```

We could put everything into one huge component, but that becomes difficult to manage.

Instead, we split it:

```text
App
│
├── Header
├── TodoInput
└── TodoList
      │
      ├── TodoItem
      ├── TodoItem
      └── TodoItem
```

Each component has a specific responsibility.

---

## Example

### Header component

```tsx
function Header() {
    return <h1>My Todo App</h1>;
}
```

### TodoInput component

```tsx
function TodoInput() {
    return <input />;
}
```

### TodoItem component

```tsx
function TodoItem() {
    return <li>Learn React</li>;
}
```

### App component

The `App` component can bring them together:

```tsx
function App() {
    return (
        <div>
            <Header />
            <TodoInput />
            <TodoItem />
        </div>
    );
}
```

Think of `App` as the **parent**:

```text
App
│
├── Header
├── TodoInput
└── TodoItem
```

---

## Why is this better?

Suppose you have 100 Todo items.

You don't want to manually write 100 different pieces of UI.

Instead:

```text
TodoItem component
       ↓
Reusable
       ↓
Todo 1
Todo 2
Todo 3
...
Todo 100
```

Later, **props** will allow each `TodoItem` to receive different data.

For example:

```text
TodoItem
   ↓
"Learn React"

TodoItem
   ↓
"Learn TypeScript"

TodoItem
   ↓
"Build Todo App"
```

Same component, different data.

We'll learn that when we reach **Props**.

### So remember these three things:

**1. Component = reusable UI piece**

**2. Functional component = function that returns JSX**

**3. Components can be combined to build a complete application**

Next, we'll move to **JSX properly**, including how JavaScript works inside JSX.

------------------------------------------------------------------------------------------------------------------------------------------

# React Step 2 — JSX

You already know the basic idea:

> **JSX lets us write HTML-like UI inside JavaScript/TypeScript.**

Now let's understand how it actually works.

---

## 1. Normal JavaScript

You can create a variable:

```javascript
const name = "William";
```

You can also return something:

```javascript
function welcome() {
    return "Hello";
}
```

That's normal JavaScript.

---

## 2. JSX

With React:

```tsx
function Welcome() {
    return <h1>Hello World</h1>;
}
```

This:

```text
<h1>Hello World</h1>
```

is JSX.

It looks like HTML, but it's inside a TypeScript/JavaScript function.

---

# 3. JavaScript inside JSX

This is one of the most important JSX concepts.

We use `{ }` to put JavaScript expressions inside JSX.

For example:

```tsx
function Welcome() {
    const name = "William";

    return <h1>Hello {name}</h1>;
}
```

Here:

```text
name = "William"
```

and:

```text
{name}
```

means:

> "Put the value of the JavaScript variable `name` here."

The result is:

```text
Hello William
```

---

## Another example

```tsx
function Todo() {
    const title = "Learn React";
    const completed = false;

    return (
        <div>
            <h2>{title}</h2>
            <p>{completed}</p>
        </div>
    );
}
```

React displays:

```text
Learn React
false
```

So think:

```text
JSX
 │
 ├── normal UI
 │
 └── { JavaScript expression }
```

---

# 4. JSX can use expressions

For example:

```tsx
function App() {
    const a = 10;
    const b = 20;

    return <h1>{a + b}</h1>;
}
```

Result:

```text
30
```

Because:

```text
{a + b}
    ↓
JavaScript expression
    ↓
30
```

---

# 5. JSX vs HTML

JSX looks like HTML, but there are some differences.

For example, in HTML you might write:

```html
<div class="todo">
```

In JSX, we use:

```tsx
<div className="todo">
```

So:

```text
HTML → class
JSX  → className
```

Another example is inline styling, which we'll learn later.

---

# 6. JSX must return one parent

This won't work:

```tsx
function App() {
    return (
        <h1>Hello</h1>
        <p>Welcome</p>
    );
}
```

Because the component is trying to return two separate elements.

We can put them inside a parent:

```tsx
function App() {
    return (
        <div>
            <h1>Hello</h1>
            <p>Welcome</p>
        </div>
    );
}
```

Now:

```text
div
│
├── h1
└── p
```

There is one parent.

Later you'll also learn **Fragments**, which allow us to avoid adding an unnecessary `<div>`.

---

# The most important JSX rule for now

When you see:

```tsx
<h1>Hello {name}</h1>
```

read it as:

```text
HTML-like UI
      +
JavaScript expression
      ↓
      JSX
```

And remember:

> **`{ }` lets us put JavaScript expressions inside JSX.**

Next we'll learn **JSX attributes and how JSX handles things like `className`, events, and dynamic values**.


------------------------------------------------------------------------------------------------------------------------------------------

# React Step 2.2 — JSX Attributes

Now let's understand how we give information to HTML elements inside JSX.

In normal HTML, you already know:

```html
<input id="todoInput" placeholder="Enter todo">
```

JSX also has attributes, but there are some differences.

---

## 1. `className`

In HTML:

```html
<div class="todo">
```

In JSX:

```tsx
<div className="todo">
```

Why?

Because `class` has a special meaning in JavaScript, so React uses:

```text
className
```

instead.

---

## 2. Normal values

You can write:

```tsx
<input placeholder="Enter todo" />
```

Here `"Enter todo"` is a fixed value.

But what if the value is stored in a variable?

```tsx
function TodoInput() {
    const placeholderText = "Enter your todo";

    return <input placeholder={placeholderText} />;
}
```

Notice:

```tsx
placeholder={placeholderText}
```

We use `{}` because we're putting a JavaScript variable into JSX.

Think:

```text
"Enter your todo"
       ↓
fixed value

{placeholderText}
       ↓
JavaScript value
```

---

## 3. Boolean values

Suppose:

```tsx
const isCompleted = true;
```

We can use:

```tsx
<input disabled={isCompleted} />
```

Because `{isCompleted}` is JavaScript.

If:

```text
isCompleted = true
```

the input is disabled.

If:

```text
isCompleted = false
```

the input isn't disabled.

---

# 4. JSX and JavaScript expressions

You can use expressions inside `{}`.

For example:

```tsx
const name = "William";

return <h1>Hello {name}</h1>;
```

Or:

```tsx
const completed = true;

return <p>{completed ? "Done" : "Pending"}</p>;
```

Or:

```tsx
const count = 5;

return <p>Total: {count + 1}</p>;
```

So `{}` is very important in JSX.

---

# 5. Self-closing elements

In HTML you might write:

```html
<input>
```

In JSX, elements without children are commonly written as:

```tsx
<input />
```

Similarly:

```tsx
<img />
<button>Click</button>
```

Notice:

```text
<input />
   ↑
self-closing
```

But:

```tsx
<button>Click</button>
```

has content, so it has an opening and closing tag.

---

# JSX summary

You now know:

```text
JSX
│
├── HTML-like elements
│
├── { JavaScript expressions }
│
├── className instead of class
│
├── Dynamic attributes using {}
│
└── Self-closing elements use />
```

For example:

```tsx
function TodoInput() {
    const placeholder = "Enter todo";

    return (
        <input
            className="todo-input"
            placeholder={placeholder}
        />
    );
}
```

Here we have:

```text
className       → JSX attribute
placeholder     → JSX attribute
{placeholder}   → JavaScript variable
<input />       → JSX element
```

Next we'll move to **Props**, which is how one React component sends data to another.


------------------------------------------------------------------------------------------------------------------------------------------

# React Step 3 — Props

Now we move to **Props**.

You can think of props as:

> **Data passed from a parent component to a child component.**

This is very important in React.

---

## 1. Why do we need props?

Suppose we have this component:

```tsx
function TodoItem() {
    return <li>Learn React</li>;
}
```

It always displays:

```text
Learn React
```

But our Todo app needs different Todo items:

```text
Learn React
Learn TypeScript
Build Todo App
Learn Node.js
```

We don't want to create a separate component for every Todo.

Instead, we pass the Todo information into the component using **props**.

---

## 2. Parent → Child

Imagine:

```text
App
 ↓
TodoItem
```

The parent `App` can give data to `TodoItem`.

For example:

```tsx
function App() {
    return <TodoItem title="Learn React" />;
}
```

Here:

```text
title="Learn React"
```

is a **prop**.

Think:

```text
App
 │
 │ title = "Learn React"
 ↓
TodoItem
```

---

## 3. Receiving the prop

The child component receives it:

```tsx
function TodoItem(props) {
    return <li>{props.title}</li>;
}
```

Now:

```text
props.title
     ↓
"Learn React"
```

So React displays:

```text
Learn React
```

---

# 4. Props are like function parameters

This connection is important because you already understand functions.

Normal JavaScript:

```javascript
function greet(name) {
    return "Hello " + name;
}
```

We call:

```javascript
greet("William");
```

The function receives:

```text
name → "William"
```

React is similar:

```tsx
function TodoItem(props) {
    return <li>{props.title}</li>;
}
```

And:

```tsx
<TodoItem title="Learn React" />
```

The component receives:

```text
props.title → "Learn React"
```

So you can think:

```text
Function parameter
       ≈
Component props
```

---

# 5. Multiple props

We can send more than one:

```tsx
function App() {
    return (
        <TodoItem
            title="Learn React"
            completed={false}
        />
    );
}
```

The child receives:

```tsx
function TodoItem(props) {
    return (
        <li>
            {props.title}
            {props.completed ? " Done" : " Pending"}
        </li>
    );
}
```

So:

```text
props
│
├── title → "Learn React"
└── completed → false
```

---

# 6. TypeScript Props

Because we're learning **React + TypeScript**, we need to type our props.

We can create:

```tsx
interface TodoItemProps {
    title: string;
    completed: boolean;
}
```

Then:

```tsx
function TodoItem(props: TodoItemProps) {
    return <li>{props.title}</li>;
}
```

Now TypeScript knows exactly what `TodoItem` expects.

```text
TodoItemProps
│
├── title → string
└── completed → boolean
```

If someone tries:

```tsx
<TodoItem title={100} completed={false} />
```

TypeScript will complain because:

```text
title expects string
100 is number
```

---

# The important flow

```text
Parent component
       │
       │ props
       ↓
Child component
       │
       ↓
Uses the data
```

For our Todo app:

```text
App
 │
 │ title
 │ completed
 ↓
TodoItem
```

---


## Think about a normal function

You already know this:

```javascript
function greet(name) {
    console.log(name);
}
```

If we do:

```javascript
greet("William");
```

The value `"William"` goes **into** the function.

```text
"William"
    ↓
 greet(name)
    ↓
 name = "William"
```

Correct?

---

# React does something similar

Suppose we have a component:

```jsx
function TodoItem() {
    return <h2>Learn React</h2>;
}
```

This component always shows:

```text
Learn React
```

But what if we want it to show different text?

We can give the component a value:

```jsx
<TodoItem title="Learn React" />
```

Here:

```text
title="Learn React"
```

is called a **prop**.

Think of it like:

```text
TodoItem
   ↑
   │
title = "Learn React"
```

---

# How does TodoItem receive it?

We write:

```jsx
function TodoItem(props) {
    return <h2>{props.title}</h2>;
}
```

Now the flow is:

```text
<TodoItem title="Learn React" />
                ↓
              props
                ↓
props.title = "Learn React"
                ↓
        <h2>Learn React</h2>
```

That's it.

---

# Compare it with a normal function

### Normal function

```javascript
function greet(name) {
    return name;
}

greet("William");
```

Flow:

```text
"William"
   ↓
name
```

- **const, let, and var** declarations usually end with ;.

- **Function and class** declarations do not end with ;.

- But **function/class** expressions assigned to variables do end with ;.


```
const x = 5;                    // declaration → ;
let y = 10;                     // declaration → ;

function foo() {}               // function declaration → no ;
class Foo {}                    // class declaration → no ;

const foo = function() {};      // function expression → ;
const Foo = class {};           // class expression → ;
const add = (a, b) => a + b;    // arrow = expression → ;
```


### React component

```jsx
function TodoItem(props) {
    return <h2>{props.title}</h2>;
}

<TodoItem title="Learn React" />
```

Flow:

```text
"Learn React"
      ↓
  props.title
      ↓
    JSX
```

So **props are basically information given to a component**.

---

# Why do we need this?

Imagine our Todo app has three todos:

```text
Learn React
Learn TypeScript
Learn Node.js
```

We can reuse the same `TodoItem` component:

```jsx
<TodoItem title="Learn React" />

<TodoItem title="Learn TypeScript" />

<TodoItem title="Learn Node.js" />
```

Same component:

```text
TodoItem
   ↓
   ├── Learn React
   ├── Learn TypeScript
   └── Learn Node.js
```

The component doesn't change.

**Only the prop changes.**

---

## One sentence to remember

> **Props are values that we pass into a React component.**

Don't worry about TypeScript props yet.

First make sure this flow is clear:

```text
<TodoItem title="Learn React" />
              ↓
            props
              ↓
       props.title
              ↓
       "Learn React"
```

---

### Remember this definition:

> **Props are data passed from a parent component to a child component.**

And the connection to what you already know:

> **Props are similar to function parameters, but they are used to pass data into React components.**

Next we'll learn **`useState`**, which is one of the most important concepts in React.


------------------------------------------------------------------------------------------------------------------------------------------

# Step 4 — `useState`

This is one of the **most important React concepts**.

## First: What is state?

State is **data that can change while your application is running**, and when it changes, React updates the UI.

For example, a counter:

```text
Count: 0

[ + ]
```

Click `+`:

```text
Count: 1
```

Click again:

```text
Count: 2
```

The `count` is **state** because its value changes.

---

## Normal JavaScript variable

You might think we can simply do:

```javascript id="u8nmbh"
let count = 0;

count = count + 1;
```

The value changes.

But React needs to **know that the value changed so it can update the screen**.

That's where `useState` comes in.

---

# `useState`

In React:

```tsx id="x8j5s7"
const [count, setCount] = useState(0);
```

There are two things here:

```text id="rlx6u8"
count
   ↓
current value

setCount
   ↓
function used to change the value
```

And:

```text id="4j3c6f"
useState(0)
       ↓
initial value = 0
```

So initially:

```text id="d7b2nj"
count = 0
```

---

## When we change it

We use:

```tsx id="s1zqsp"
setCount(count + 1);
```

The flow is:

```text id="6y2rjh"
count = 0
   ↓
setCount(1)
   ↓
React knows state changed
   ↓
React updates the UI
   ↓
count = 1
```

---

# Why two names?

This:

```tsx id="3c1w8m"
const [count, setCount] = useState(0);
```

is using **array destructuring**, which you already learned in JavaScript.

`useState(0)` gives us two things:

```text id="m50a3v"
[
    current value,
    update function
]
```

So:

```text id="7p4k9q"
count
 ↓
current value

setCount
 ↓
change/update function
```

---

## Simple counter component

```tsx id="9f4r4q"
function Counter() {
    const [count, setCount] = useState(0);

    return (
        <div>
            <p>Count: {count}</p>

            <button onClick={() => setCount(count + 1)}>
                +
            </button>
        </div>
    );
}
```

Don't worry about `onClick` yet. **We'll learn event handling as its own step**, exactly as your roadmap says.

For now, focus only on:

```tsx id="o3w9pk"
const [count, setCount] = useState(0);
```

### Remember:

> **`count` = current state value**

> **`setCount` = function that changes the state**

> **`0` = initial value**

Next, we'll go deeper into **how `useState` actually changes the UI**, before moving to event handling.


------------------------------------------------------------------------------------------------------------------------------------------

# React Step 4.2 — How `useState` Updates the UI

Let's understand the most important part of `useState`.

You have:

```tsx
const [count, setCount] = useState(0);
```

Initially:

```text
count = 0
```

React displays:

```text
Count: 0
```

---

## Now we change the state

Suppose we call:

```tsx
setCount(1);
```

React sees:

```text
Old state
count = 0

      ↓

setCount(1)

      ↓

New state
count = 1
```

React then **re-renders the component**.

So the JSX:

```tsx
<p>Count: {count}</p>
```

now becomes:

```text
Count: 1
```

---

# The important flow

```text
User action
    ↓
setCount(1)
    ↓
State changes
    ↓
React re-renders component
    ↓
UI shows new value
```

This is the main reason we use `useState`.

---

## Why not just use a normal variable?

Imagine:

```tsx
function Counter() {
    let count = 0;

    count = count + 1;

    return <p>{count}</p>;
}
```

You might think:

> "The value changed, so why doesn't React update?"

Because a normal variable doesn't tell React:

> **"My data changed. Please update the UI."**

`setCount()` does.

```text
Normal variable
      ↓
value changes
      ↓
React doesn't automatically know

useState
      ↓
setCount()
      ↓
React knows
      ↓
re-render
```

---

# One more important thing

Don't directly change the state variable.

Don't do:

```tsx
count = count + 1;
```

Use:

```tsx
setCount(count + 1);
```

Because `setCount` is the function React gives us to update that state.

---

## Think of `useState` as a connection

```text
        State
          ↓
      count = 0
          ↓
        JSX
          ↓
      Count: 0

          ↑
          │
      setCount()
          │
          └── changes state
```

So the key idea is:

> **State stores changing data, and the state setter tells React to update the UI when that data changes.**

> **setCount tells React that we want to change the state**

Next we'll learn **state with different types** — strings, booleans, and arrays — because this will connect directly to our Todo app.


------------------------------------------------------------------------------------------------------------------------------------------

# React Step 4.3 — `useState` with Different Data Types

`useState` doesn't only work with numbers.

You can store **strings, booleans, arrays, and objects** in state.

The important thing is:

> The initial value tells TypeScript what type of state you're creating.

---

## 1. Number state

Our counter:

```tsx
const [count, setCount] = useState(0);
```

TypeScript understands:

```text
count → number
setCount → changes number
```

So:

```tsx
setCount(5);
```

is valid.

But:

```tsx
setCount("hello");
```

would be an error.

---

## 2. String state

For example, an input:

```tsx
const [name, setName] = useState("");
```

Because the initial value is:

```text
""
↓
string
```

So:

```tsx
setName("William");
```

is valid.

You could imagine:

```text
name = "William"
```

---

## 3. Boolean state

For something that is either true or false:

```tsx
const [completed, setCompleted] = useState(false);
```

Now:

```text
completed → boolean
```

So:

```tsx
setCompleted(true);
```

or:

```tsx
setCompleted(false);
```

---

# 4. Array state

This is especially important for our Todo app.

Suppose we want to store multiple Todos.

```tsx
const [todos, setTodos] = useState<Todo[]>([]);
```

The important part is:

```text
Todo[]
```

which means:

> **an array containing Todo objects.**

So:

```text
todos
 ↓
[
   Todo,
   Todo,
   Todo
]
```

---

## Why do we use `<Todo[]>`?

Remember our TypeScript interface:

```tsx
interface Todo {
    id: number;
    title: string;
    completed: boolean;
}
```

We tell React:

```tsx
useState<Todo[]>([])
```

meaning:

```text
State
 ↓
array
 ↓
only Todo objects
```

So this is valid:

```tsx
setTodos([
    {
        id: 1,
        title: "Learn React",
        completed: false
    }
]);
```

But putting a string into the array would be a TypeScript error.

---

# 5. Object state

We can also store an object:

```tsx
const [user, setUser] = useState({
    name: "",
    age: 0
});
```

TypeScript can infer:

```text
user
├── name → string
└── age → number
```

---

# The important connection to our Todo app

Our Todo app needs to keep track of **many Todos**.

So eventually we'll have:

```tsx
const [todos, setTodos] = useState<Todo[]>([]);
```

Think of it as:

```text
todos
  ↓
┌─────────────────────┐
│ Todo 1              │
│ Todo 2              │
│ Todo 3              │
└─────────────────────┘
```

And when we add a Todo:

```text
Old state
[]
 ↓
add Todo
 ↓
New state
[Todo 1]
 ↓
React re-renders
 ↓
Todo appears on screen
```

That's going to be the heart of our React Todo app.

---

### Quick memory rule

```text
useState(0)       → number

useState("")      → string

useState(false)   → boolean

useState<Todo[]>([]) →  array
```

---

- The important part is `Todo[]`.

### Example 1 — Todo array

```tsx
const [todos, setTodos] = useState<Todo[]>([]);
```

Means:

```text
todos → array of Todo objects
```

---

### Example 2 — String array

If you want an array of strings:

```tsx
const [names, setNames] = useState<string[]>([]);
```

Means:

```text
names → ["William", "John", "David"]
```

---

### Example 3 — Number array

```tsx
const [numbers, setNumbers] = useState<number[]>([]);
```

Means:

```text
numbers → [10, 20, 30]
```

---

### Example 4 — Boolean array

```tsx
const [values, setValues] = useState<boolean[]>([]);
```

Means:

```text
values → [true, false, true]
```

---

### So the pattern is:

```text
useState<Type[]>([])
        ↑
      type
```

For example:

```text
string[]    → array of strings
number[]    → array of numbers
boolean[]   → array of booleans
Todo[]      → array of Todos
User[]      → array of Users
```

And `[]` means the **initial array is empty**.

So:

```tsx
useState<Todo[]>([])
```

Next we'll learn **how to update state correctly**, especially arrays, because that's essential for adding and deleting Todos.


------------------------------------------------------------------------------------------------------------------------------------------

# React Step 4.4 — Updating State Correctly

Now we'll learn an important rule:

> **Don't directly modify React state. Use the setter function.**

Let's use our Todo array.

```tsx
const [todos, setTodos] = useState<Todo[]>([]);
```

Initially:

```text
todos = []
```

---

## Adding a Todo

Suppose we have:

```text
Todo 1
```

We want:

```text
Todo 1
Todo 2
```

A common mistake is trying to directly modify the array.

Instead, we create a **new array** containing the old Todos plus the new Todo.

Conceptually:

```text
Old todos
[Todo 1]

        +

New Todo
[Todo 2]

        ↓

New state
[Todo 1, Todo 2]
```

In JavaScript, the spread operator helps us do this:

```tsx
setTodos([...todos, newTodo]);
```

You already learned the spread operator in JavaScript.

Here:

```text
...todos
```

means:

> Take all the existing Todos.

Then:

```text
newTodo
```

adds the new Todo.

So:

```text
[todos..., newTodo]
```

becomes:

```text
[Todo 1, Todo 2]
```

---

# Why not modify the existing array?

Avoid doing:

```tsx
todos.push(newTodo);
```

The problem is that you're modifying the existing state directly.

Instead:

```tsx
setTodos([...todos, newTodo]);
```

creates a new array and tells React:

> "Here is the new state."

---

# Deleting a Todo

Suppose:

```text
todos
├── Todo 1
├── Todo 2
└── Todo 3
```

We want to delete Todo 2.

We can create a new array containing everything **except Todo 2**.

That's where `.filter()` comes in:

```tsx
setTodos(todos.filter(todo => todo.id !== id));
```

Think about the flow:

```text
Todo 1 → keep
Todo 2 → remove
Todo 3 → keep

        ↓

[Todo 1, Todo 3]
```

Again, we're creating a new array.

---

# Changing a Todo

Suppose Todo 2 changes from:

```text
completed = false
```

to:

```text
completed = true
```

We can use `.map()` to create a new array with the changed Todo.

Conceptually:

```text
Todo 1 → unchanged
Todo 2 → changed
Todo 3 → unchanged
```

This is another important React pattern.

---

# The main rule

Remember this:

```text
❌ Directly modify state

✅ Use the state setter
```

For arrays:

```text
Add    → create a new array
Delete → create a filtered array
Update → create a mapped array
```

This is why your JavaScript knowledge of:

* spread `...`
* `filter()`
* `map()`

is very useful in React.

---

## Todo app flow

Eventually our Todo app will work like:

```text
User clicks Add
      ↓
Create new Todo
      ↓
setTodos([...todos, newTodo])
      ↓
React re-renders
      ↓
Todo list updates
```

And for delete:

```text
User clicks Delete
      ↓
filter out Todo
      ↓
setTodos(newArray)
      ↓
React re-renders
      ↓
Todo disappears
```

Now we've covered the core of **`useState`**.

Next, exactly as your roadmap specifies, we'll move to **Step 5 — Event Handling**: `onClick`, `onChange`, `onSubmit`, and React event objects.

------------------------------------------------------------------------------------------------------------------------------------------

# React Step 5 — Event Handling

You already know event handling from JavaScript:

```javascript
button.addEventListener("click", () => {
    // do something
});
```

React handles events in a similar way, but the syntax is different.

---

# 1. `onClick`

In normal JavaScript:

```javascript
button.addEventListener("click", () => {
    console.log("Clicked");
});
```

In React:

```tsx
<button onClick={() => {
    console.log("Clicked");
}}>
    Click
</button>
```

The main difference is:

```text
JavaScript
addEventListener("click", ...)

React
onClick={...}
```

---

# 2. Why `onClick` has `{}`

Remember JSX?

We use `{}` to put JavaScript inside JSX.

So:

```tsx
<button onClick={handleClick}>
```

means:

> "Use the JavaScript function `handleClick` when this button is clicked."

For example:

```tsx
function handleClick() {
    console.log("Button clicked");
}

function App() {
    return (
        <button onClick={handleClick}>
            Click
        </button>
    );
}
```

Flow:

```text
User clicks button
       ↓
onClick
       ↓
handleClick()
       ↓
"Button clicked"
```

---

# 3. Don't call the function immediately

This is important.

Correct:

```tsx
<button onClick={handleClick}>
```

We are saying:

> When clicked, run `handleClick`.

But:

```tsx
<button onClick={handleClick()}>
```

means:

> Run `handleClick` immediately while rendering.

So remember:

```text
onClick={handleClick}
       ↓
give React the function

onClick={handleClick()}
       ↓
call the function immediately
```

---

# 4. `onClick` with `useState`

Now we can connect what we just learned.

```tsx
function Counter() {
    const [count, setCount] = useState(0);

    function handleClick() {
        setCount(count + 1);
    }

    return (
        <div>
            <p>Count: {count}</p>

            <button onClick={handleClick}>
                +
            </button>
        </div>
    );
}
```

Flow:

```text
User clicks +
      ↓
onClick
      ↓
handleClick()
      ↓
setCount(count + 1)
      ↓
State changes
      ↓
React re-renders
      ↓
Count increases
```

This is the connection between:

**Event Handling + State**

---

# 5. `onChange`

For an input, we commonly use `onChange`.

```tsx
function App() {
    function handleChange() {
        console.log("Input changed");
    }

    return (
        <input onChange={handleChange} />
    );
}
```

Every time the input value changes, React calls `handleChange`.

Flow:

```text
User types
   ↓
onChange
   ↓
handleChange()
```

---

# 6. Getting the input value

This is where the event object becomes useful.

```tsx
function handleChange(event) {
    console.log(event.target.value);
}
```

If the user types:

```text
Learn React
```

then:

```text
event.target.value
        ↓
"Learn React"
```

So this is similar to what you did in vanilla JavaScript:

```javascript
todoInput.value
```

but React gives us the event information.

---

# 7. `onSubmit`

For forms, React uses:

```tsx
<form onSubmit={handleSubmit}>
```

Example:

```tsx
function handleSubmit(event) {
    event.preventDefault();

    console.log("Form submitted");
}
```

The flow:

```text
User submits form
       ↓
onSubmit
       ↓
handleSubmit()
       ↓
preventDefault()
       ↓
Your React logic
```

You'll use this a lot when building forms.

---

# The three important events for now

| React event | Used for        |
| ----------- | --------------- |
| `onClick`   | Button clicks   |
| `onChange`  | Input changes   |
| `onSubmit`  | Form submission |

And you already know the underlying idea from JavaScript:

```text
JavaScript
addEventListener
      ↓
React
event props
```

Next we'll focus specifically on **the React event object and TypeScript**, so you understand what `event` actually is.


------------------------------------------------------------------------------------------------------------------------------------------

# React Step 5.2 — Event Object + TypeScript

You already understand:

```tsx
<button onClick={handleClick}>
```

Now let's understand this:

```tsx
function handleClick(event) {
}
```

What exactly is `event`?

## 1. What is the event object?

When something happens in the browser, React gives your function information about that event.

For example, when you click a button:

```text
User clicks button
      ↓
React detects click
      ↓
React calls handleClick()
      ↓
React gives information about the click
      ↓
event
```

That information is the **event object**.

---

## 2. Example

```tsx
function handleClick(event) {
    console.log(event);
}
```

When you click the button, `event` contains information about that click.

For example, you can access things such as:

```text
event.target
event.currentTarget
```

---

# 3. `event.target`

Suppose we have:

```tsx
<button onClick={handleClick}>
    Delete
</button>
```

When the user clicks the button:

```text
event.target
      ↓
the element that was clicked
      ↓
button
```

So:

```tsx
function handleClick(event) {
    console.log(event.target);
}
```

gives you the clicked element.

This is similar to what you saw in your vanilla JS Todo app.

---

# 4. `event.preventDefault()`

You already used this concept in JavaScript.

For a form:

```tsx
function handleSubmit(event) {
    event.preventDefault();
}
```

Normally, submitting an HTML form can cause the browser's default behavior.

`preventDefault()` says:

> Don't perform the browser's default action.

Then React can handle the form itself.

---

# 5. TypeScript event types

Now because we're using TypeScript, we can tell TypeScript what kind of event we're receiving.

For a button click:

```tsx
function handleClick(event: React.MouseEvent<HTMLButtonElement>) {
}
```

Don't worry about memorizing this yet.

Break it down:

```text
React.MouseEvent
       ↓
mouse event

<HTMLButtonElement>
       ↓
event came from a button
```

So:

> This function receives a mouse event from an HTML button.

---

## Input event

For an input:

```tsx
function handleChange(event: React.ChangeEvent<HTMLInputElement>) {
}
```

Meaning:

```text
React.ChangeEvent
       ↓
change event

<HTMLInputElement>
       ↓
event came from an input
```

Then:

```tsx
event.target.value
```

is the input's value.

---

## Form event

For a form:

```tsx
function handleSubmit(event: React.FormEvent<HTMLFormElement>) {
    event.preventDefault();
}
```

Meaning:

```text
React.FormEvent
       ↓
form event

<HTMLFormElement>
       ↓
event came from a form
```

---

# But remember something important

You **don't always need to explicitly write these types**.

When the function is directly used in JSX, TypeScript can often infer the event type.

For example:

```tsx
<button onClick={(event) => {
    console.log(event);
}}>
    Delete
</button>
```

TypeScript can understand that `event` is a mouse event.

So don't start memorizing:

```text
React.MouseEvent<HTMLButtonElement>
React.ChangeEvent<HTMLInputElement>
React.FormEvent<HTMLFormElement>
```

yet.

Just understand **what they mean**.

---

# Connect this with your old Todo app

You previously had:

```javascript
deleteButton.addEventListener("click", (event) => {
    event.stopPropagation();
    todoItem.remove();
});
```

React will eventually look more like:

```tsx
<button onClick={handleDelete}>
    Delete
</button>
```

And:

```text
Click
 ↓
onClick
 ↓
handleDelete
 ↓
update React state
 ↓
React removes Todo from UI
```

Notice the important difference:

### Vanilla JS

You directly did:

```text
todoItem.remove()
```

### React

You normally change the **state**:

```text
setTodos(...)
      ↓
React re-renders
      ↓
Todo disappears
```

That difference will become very important when we build the React Todo app.

Next: **Conditional Rendering** — how React decides whether to show something or not.

------------------------------------------------------------------------------------------------------------------------------------------

# React Step 6 — Conditional Rendering

Now we move to the next topic in your roadmap:

> **Conditional Rendering**

You already know conditions in JavaScript:

```javascript
if (completed) {
    // do something
}
```

In React, we use conditions to decide **what should appear on the screen**.

---

## 1. Simple example

Suppose we have:

```tsx
const isLoggedIn = true;
```

We want:

```text
If logged in → "Welcome"
If not       → "Please login"
```

We can use a ternary:

```tsx
function App() {
    const isLoggedIn = true;

    return (
        <h1>
            {isLoggedIn ? "Welcome" : "Please login"}
        </h1>
    );
}
```

The important part is:

```text
condition ? valueIfTrue : valueIfFalse
```

So:

```text
isLoggedIn
    ↓
  true?
  /   \
yes    no
 ↓      ↓
Welcome Please login
```

---

# 2. Using `&&`

Sometimes we only want to show something when a condition is true.

For example:

```tsx
function App() {
    const hasTodos = true;

    return (
        <div>
            {hasTodos && <p>You have todos.</p>}
        </div>
    );
}
```

> "If the left side is true, show the right side." 

If:

```text
hasTodos = true
```

React shows:

```text
You have todos.
```

If:

```text
hasTodos = false
```

React doesn't show the paragraph.

Think:

```text
true && something
      ↓
show something

false && something
       ↓
show nothing
```

---

# 3. Using `if`

We can also use a normal `if` before returning JSX.

```tsx
function Message() {
    const isLoggedIn = false;

    if (!isLoggedIn) {
        return <p>Please login.</p>;
    }

    return <p>Welcome!</p>;
}
```

Flow:

```text
isLoggedIn?
    ↓
 false
    ↓
"Please login."
```

This is useful when the logic is more complicated.

---

# 4. Todo example

This is where conditional rendering becomes useful for our Todo app.

Suppose:

```text
completed = true
```

We might show:

```text
Learn React ✓
```

And if:

```text
completed = false
```

we might show:

```text
Learn React
```

For example:

```tsx
function TodoItem() {
    const completed = true;

    return (
        <li>
            Learn React
            {completed && " ✓"}
        </li>
    );
}
```

If `completed` is true:

```text
Learn React ✓
```

If false:

```text
Learn React
```

---

# 5. Conditional rendering + state

This becomes even more powerful when combined with `useState`.

For example:

```tsx
const [completed, setCompleted] = useState(false);
```

Then the UI can depend on the state:

```text
completed = false
       ↓
"Pending"

       ↓ user clicks

completed = true
       ↓
"Completed"
```

So:

```text
Event
  ↓
setCompleted()
  ↓
State changes
  ↓
Conditional rendering
  ↓
UI changes
```

This is the React pattern you'll use constantly.

---

## Imagine a light switch

There are only two states:

```text
OFF
ON
```

We can represent that with a boolean:

```tsx
const [isOn, setIsOn] = useState(false);
```

Initially:

```text
isOn = false
```

So the UI can show:

```text
Light is OFF
```

---

## Now we want to change it

We have a button:

```tsx
<button onClick={() => setIsOn(true)}>
    Turn On
</button>
```

When the user clicks the button:

```text
User clicks button
       ↓
setIsOn(true)
       ↓
isOn changes
false → true
       ↓
React re-renders
       ↓
UI changes
```

Now React sees:

```text
isOn = true
```

So we can conditionally show:

```tsx
{isOn ? "Light is ON" : "Light is OFF"}
```

The screen becomes:

```text
Light is ON
```

---

# Now connect the pieces

This is the part I want you to understand:

```text
1. State
   ↓
isOn = false

2. User clicks
   ↓
3. Event handler runs
   ↓
setIsOn(true)

4. State changes
   ↓
isOn = true

5. React renders again
   ↓
6. Condition is checked
   ↓
isOn ? "ON" : "OFF"

7. UI shows
   ↓
Light is ON
```

That's what I meant by:

```text
Event
  ↓
setIsOn()
  ↓
State changes
  ↓
Conditional rendering
  ↓
UI changes
```

---

# Now use the same idea for Todo

Remember your Todo has:

```text
completed
```

It can be:

```text
false → not completed
true  → completed
```

We could have:

```tsx
const [completed, setCompleted] = useState(false);
```

Initially:

```text
completed = false
```

So:

```tsx
{completed ? "Completed" : "Pending"}
```

shows:

```text
Pending
```

Then the user clicks a button:

```tsx
<button onClick={() => setCompleted(true)}>
    Complete
</button>
```

Flow:

```text
User clicks Complete
        ↓
setCompleted(true)
        ↓
completed: false → true
        ↓
React re-renders
        ↓
completed ? "Completed" : "Pending"
        ↓
Completed
```

---

## The key idea

**`useState` stores the current situation.**

**An event changes that situation.**

**Conditional rendering looks at the current situation and decides what to display.**

So:

```text
STATE
"What is the current situation?"

EVENT
"What did the user do?"

SETTER
"Change the situation."

CONDITIONAL RENDERING
"Based on the situation, what should I show?"
```

This is a very common React pattern.

> State stores a value that can change, and conditional rendering uses that value to decide what to show.

##### So remember:

- **state** = stores changing data

- **Conditional rendering** = decides what to display based on data

- They can work together, but they are two different concepts.

---

# Three ways to remember

### `if`

Use when you have more complicated logic.

```text
if (condition) {
    ...
}
```

### Ternary

Use when you want **one thing or another**.

```text
condition ? A : B
```

### `&&`

Use when you want to show something **only when true**.

```text
condition && A
```

---

## One important idea

Conditional rendering doesn't mean React is creating a completely different application.

It means:

> **Based on the current state/data, React decides what JSX should be displayed.**

Next we'll learn the final React Basics concept: **Lists + Keys**, which we'll use to display all our Todo items.


------------------------------------------------------------------------------------------------------------------------------------------

# React Step 7 — Lists + Keys

You already know JavaScript `.map()`, so this should feel familiar.

## 1. Why do we need lists?

Imagine our Todo app has:

```text
Learn React
Learn TypeScript
Learn Node.js
```

We don't want to manually write:

```tsx
<TodoItem />
<TodoItem />
<TodoItem />
```

Instead, we keep the Todos in an array:

```text
todos
│
├── Todo 1
├── Todo 2
└── Todo 3
```

Then React can create a component for each Todo.

---

# 2. Using `.map()`

Suppose:

```tsx id="yr0l4x"
const todos = [
    { id: 1, title: "Learn React" },
    { id: 2, title: "Learn TypeScript" },
    { id: 3, title: "Learn Node.js" }
];
```

We can do:

```tsx id="e8xhlb"
function TodoList() {
    return (
        <ul>
            {todos.map(todo => (
                <li>{todo.title}</li>
            ))}
        </ul>
    );
}
```

`.map()` goes through each Todo:

```text id="fdl2id"
Todo 1 → <li>Learn React</li>

Todo 2 → <li>Learn TypeScript</li>

Todo 3 → <li>Learn Node.js</li>
```

React then displays:

```text id="m7v28u"
Learn React
Learn TypeScript
Learn Node.js
```

---

# 3. What is `key`?

You'll notice React will warn us that each list item needs a **key**.

So we write:

```tsx id="x40cax"
{todos.map(todo => (
    <li key={todo.id}>
        {todo.title}
    </li>
))}
```

Here:

```text id="6f5ecj"
key={todo.id}
```

gives each Todo a unique identity.

Think:

```text id="qj0jrr"
Todo 1 → key = 1
Todo 2 → key = 2
Todo 3 → key = 3
```

---

# 4. Why does React need a key?

Imagine we have:

```text id="6db8j1"
1 → Learn React
2 → Learn TypeScript
3 → Learn Node.js
```

Then we delete Todo 2.

Now:

```text id="f4v8o1"
1 → Learn React
3 → Learn Node.js
```

React needs to understand **which item was removed and which items remain**.

The keys help React identify the items.

So:

> **A key gives each item in a React list a stable identity.**

---

# 5. Why use `id`?

Our Todo already has:

```text id="r7p6fi"
id: 1
id: 2
id: 3
```

Those IDs are unique, so they're good keys:

```tsx id="jj9qwv"
key={todo.id}
```

Avoid using the array index as a key when you have a proper unique ID.

For example, prefer:

```tsx id="h6v9h0"
key={todo.id}
```

over:

```tsx id="5ot9uq"
key={index}
```

when your data has stable IDs.

---

# Connect this with our Todo app

Eventually we'll have:

```tsx id="7c6qkq"
const [todos, setTodos] = useState<Todo[]>([]);
```

Then:

```text id="d0n6l3"
todos
│
├── Todo 1
├── Todo 2
└── Todo 3
```

And our component will render them:

```text id="y8h6dr"
todos.map()
     ↓
TodoItem
     ↓
TodoItem
     ↓
TodoItem
```

Each one gets:

```text id="6i9g8n"
key={todo.id}
```

---

# The whole idea

```text id="5w4q3j"
Array of Todos
      ↓
    .map()
      ↓
Create TodoItem for each Todo
      ↓
key={todo.id}
      ↓
React knows each item's identity
```

### Remember these two things:

**`.map()`**

> Creates UI for every item in an array.

**`key`**

> Gives each rendered item a unique/stable identity.

---

## React Basics is now complete

You've covered:

1. Component architecture
2. JSX
3. Functional components
4. Props
5. `useState`
6. Event handling
7. Conditional rendering
8. Lists + keys

Next, as your roadmap says, we'll **build the Counter App first**, then move to the **React + TypeScript Todo App**.

------------------------------------------------------------------------------------------------------------------------------------------