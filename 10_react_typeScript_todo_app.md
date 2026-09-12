# Todo App with TypeScript

Now we're moving to the second project in your roadmap:

> **Build: Todo App (React + TypeScript)**

You've already built a Todo app with vanilla JavaScript, so this time we'll rebuild the same idea using **React + TypeScript**.

This is useful because you'll see exactly how the approach changes.

## What we'll build

Our Todo app will have:

```text id="b8x4hy"
Todo App

[ Enter todo... ] [Add]

☐ Learn React       [Delete]
☑ Learn TypeScript  [Delete]
☐ Build project     [Delete]
```

We'll support:

* Add Todo
* Display Todos
* Mark Todo as completed
* Delete Todo
* TypeScript types
* React state
* Props
* Event handling
* Lists + keys

---

# How we'll build it

We'll do it in small steps:

### Step 1 — Create the Todo type

We'll define what a Todo looks like:

```text id="v4p3je"
Todo
├── id
├── title
└── completed
```

### Step 2 — Create Todo state

```text id="k9gq0f"
todos
   ↓
array of Todo objects
```

### Step 3 — Create input state

Store what the user is typing.

### Step 4 — Display the Todo list

Use `.map()` and `key`.

### Step 5 — Add a Todo

Connect the input, button, and state.

### Step 6 — Create `TodoItem` component

This is where we'll practice **Props** properly.

### Step 7 — Complete a Todo

Use state to change `completed`.

### Step 8 — Delete a Todo

Remove the Todo from the state.

### Step 9 — Final cleanup

Put everything together and make sure you understand the complete flow.

---

## Important difference from your old Todo app

In your vanilla JS Todo app, you were doing things like:

```text id="w6g4s3"
document.createElement()
        ↓
appendChild()
        ↓
element.remove()
```

In React, we'll mostly do:

```text id="q5xq6m"
State
 ↓
JSX
 ↓
React creates/updates the UI
```

For example, instead of manually removing an `<li>`:

```text id="j8v8bx"
todoItem.remove()
```

we'll remove the Todo from the `todos` state:

```text id="a3qz0m"
setTodos(...)
     ↓
React re-renders
     ↓
Todo disappears
```

This is the **big React mindset change**.

We'll start with **Step 1 — Create the Todo Type**.

-------------------------------------------------------------------------------------------------------------------------------------------

# Todo App — Step 1: Clean `App.tsx`

Open:

```text
src/App.tsx
```

Vite gives you some default React code there.

For our learning project, remove the default code and keep a simple component:

```tsx
function App() {
    return (
        <div>
            <h1>Todo App</h1>
        </div>
    );
}

export default App;
```

### What we have now

```text
App
 ↓
<h1>Todo App</h1>
 ↓
Browser
```

That's all for this step.

We are **not adding state, input, buttons, or Todo types yet**.

Once you understand this, the next step is:

**Step 2 — Create the `Todo` TypeScript interface.**

-------------------------------------------------------------------------------------------------------------------------------------------

# Todo App — Step 2: Create the `Todo` Type

Now we'll tell **TypeScript what a Todo should contain**.

Open:

```text
src/App.tsx
```

Above the `App` function, add:

```tsx id="q3r7ka"
interface Todo {
    id: number;
    title: string;
    completed: boolean;
}
```

So your file will look like:

```tsx id="z0q7cz"
interface Todo {
    id: number;
    title: string;
    completed: boolean;
}

function App() {
    return (
        <div>
            <h1>Todo App</h1>
        </div>
    );
}

export default App;
```

### What does this mean?

We're defining the **shape of a Todo**:

```text id="x9f2k8"
Todo
│
├── id
│    └── number
│
├── title
│    └── string
│
└── completed
     └── boolean
```

For example, this is a valid Todo:

```text id="c8m3nv"
id: 1
title: "Learn React"
completed: false
```

But TypeScript won't accept something like:

```text id="3p6z1x"
id: "one"
```

because `id` must be a **number**.

### Why are we doing this?

Later we'll have many Todos:

```text id="n6r5s1"
todos
 ↓
Todo[]
```

TypeScript will then know exactly what every item inside `todos` looks like.

**Next → Step 3: Create the `todos` state using `useState<Todo[]>([])`.**


-------------------------------------------------------------------------------------------------------------------------------------------


# Todo App — Step 3: Create the Todo State

Now we need a place to **store all our Todos**.

We already created the `Todo` type:

```tsx id="6eg0ec"
interface Todo {
    id: number;
    title: string;
    completed: boolean;
}
```

Now inside `App`, add:

```tsx id="kl7d5k"
const [todos, setTodos] = useState<Todo[]>([]);
```

But first, because we're using `useState`, import it at the top:

```tsx id="3h8c1x"
import { useState } from "react";
```

Your `App.tsx` should now look like:

```tsx id="a6yq8m"
import { useState } from "react";

interface Todo {
    id: number;
    title: string;
    completed: boolean;
}

function App() {
    const [todos, setTodos] = useState<Todo[]>([]);

    return (
        <div>
            <h1>Todo App</h1>
        </div>
    );
}

export default App;
```

## Understand this line

```tsx id="c2d9tq"
const [todos, setTodos] = useState<Todo[]>([]);
```

Break it into three parts:

### `todos`

The **current Todo list**.

Initially:

```text id="j9x7py"
todos = []
```

### `setTodos`

The function we will use later to **add, delete, or update Todos**.

### `Todo[]`

This is TypeScript.

It means:

> `todos` is an array of `Todo` objects.

So eventually:

```text id="x5t2vd"
todos
 ↓
[
    Todo 1,
    Todo 2,
    Todo 3
]
```

---

## Compare with our Counter

Counter:

```tsx id="w8d6ks"
const [count, setCount] = useState(0);
```

Todo:

```tsx id="h7v3qa"
const [todos, setTodos] = useState<Todo[]>([]);
```

Same React pattern:

```text id="e1x5zr"
[current value, setter] = useState(initial value)
```

Only the data is different.

### Current status

```text id="y9a2xc"
Step 1 Clean App.tsx
Step 2 Todo interface
Step 3 Todo state
```

**Next → Step 4: Create the input state**, which will store what the user types into the Todo input.



-------------------------------------------------------------------------------------------------------------------------------------------


# Todo App — Step 4: Create Input State

Now we need to handle the **text the user types** into the input box.

For example:

```text
[ Learn React                  ]
```

We need to store `"Learn React"` somewhere.

That's what our second `useState` is for.

## 1. Create `todoText` state

Inside `App`, add:

```tsx id="6p3f2r"
const [todoText, setTodoText] = useState("");
```

Now we have **two states**:

```tsx id="4p8k1m"
const [todos, setTodos] = useState<Todo[]>([]);

const [todoText, setTodoText] = useState("");
```

### First state

```text id="0m8r5x"
todos
 ↓
stores the Todo list
```

Initially:

```text id="k2z9fw"
[]
```

### Second state

```text id="w6q3nc"
todoText
 ↓
stores what the user is currently typing
```

Initially:

```text id="j1v7ha"
""
```

---

## 2. Why do we need separate state?

Imagine the user types:

```text
Learn React
```

We want:

```text
todoText = "Learn React"
```

Then when they click **Add**, we can use that value to create a Todo:

```text
todoText
   ↓
"Learn React"
   ↓
Create Todo
   ↓
Add to todos
```

So we have:

```text id="x9j2kq"
Input
  ↓
todoText
  ↓
Add button
  ↓
todos
```

---

## Important distinction

Don't confuse these two:

```text
todoText
```

and

```text
todos
```

They are different.

```text
todoText
   ↓
What the user is typing RIGHT NOW

todos
   ↓
All Todos that have been added
```

For example, while the user is typing:

```text
todoText = "Learn React"

todos = []
```

After clicking Add:

```text
todoText = ""

todos = [
    {
        id: 1,
        title: "Learn React",
        completed: false
    }
]
```

That's the reason we need **two separate states**.

### Current progress

```text
Step 1 Clean App.tsx
Step 2 Todo interface
Step 3 Todo state
Step 4 Input state
```

**Next → Step 5: Create the input and Add button in JSX, then connect the input to `todoText`.**


-------------------------------------------------------------------------------------------------------------------------------------------

# Todo App — Step 5: Create Input + Add Button

Now we'll create the **actual UI** for the user.

We already have:

```tsx
const [todoText, setTodoText] = useState("");
```

This stores what the user types.

Now inside the `return`, add an input and button:

```tsx
return (
    <div>
        <h1>Todo App</h1>

        <input type="text" />

        <button>Add</button>
    </div>
);
```

The browser will show:

```text
Todo App

[              ] [Add]
```

---

## But our input isn't connected to state yet

We want:

```text
User types
   ↓
input
   ↓
todoText
```

So we use `onChange`.

```tsx
<input
    type="text"
    onChange={(event) => setTodoText(event.target.value)}
/>
```

Now every time the user types:

```text
User types "R"
        ↓
onChange
        ↓
setTodoText("R")

User types "Re"
        ↓
onChange
        ↓
setTodoText("Re")

User types "React"
        ↓
onChange
        ↓
setTodoText("React")
```

So eventually:

```text
todoText = "React"
```

---

# Complete Step 5

Your `App.tsx` should now be:

```tsx
import { useState } from "react";

interface Todo {
    id: number;
    title: string;
    completed: boolean;
}

function App() {
    const [todos, setTodos] = useState<Todo[]>([]);
    const [todoText, setTodoText] = useState("");

    return (
        <div>
            <h1>Todo App</h1>

            <input
                type="text"
                onChange={(event) => setTodoText(event.target.value)}
            />

            <button>Add</button>
        </div>
    );
}

export default App;
```

### Understand the flow

```text
User types
    ↓
onChange
    ↓
event.target.value
    ↓
setTodoText()
    ↓
todoText contains the typed text
```

For example:

```text
Input: Learn React

todoText:
"Learn React"
```

**The Add button doesn't do anything yet.** That's intentional.

Next → **Step 6: Handle the Add button and create our first Todo object.**


-------------------------------------------------------------------------------------------------------------------------------------------


# Todo App — Step 7: Display Todos with `.map()`

Now we have successfully added the Todo to our **state**.

But the browser still only shows:

```text
Todo App

[ Learn React ] [Add]
```

Why?

Because we haven't told React:

> "Take the `todos` array and display each Todo."

That's what `.map()` does.

---

## 1. Display the `todos`

Inside the JSX, below the Add button:

```tsx
<ul>
    {todos.map((todo) => (
        <li key={todo.id}>
            {todo.title}
        </li>
    ))}
</ul>
```

### What is happening?

Suppose our state contains:

```text
todos = [
    {
        id: 1,
        title: "Learn React",
        completed: false
    },
    {
        id: 2,
        title: "Learn TypeScript",
        completed: false
    }
]
```

`.map()` goes through each Todo:

```text
Todo 1
   ↓
<li>Learn React</li>

Todo 2
   ↓
<li>Learn TypeScript</li>
```

The browser displays:

```text
Todo App

[ Learn Node.js ] [Add]

• Learn React
• Learn TypeScript
```

---

## 2. Why `key={todo.id}`?

Remember our Lists + Keys lesson.

```tsx
<li key={todo.id}>
```

This tells React:

> This particular list item belongs to this particular Todo.

Since every Todo has its own `id`:

```text
Todo 1 → key = 1
Todo 2 → key = 2
Todo 3 → key = 3
```

---

# Full Code So Far

Here is the **complete code from Steps 1–7**:

```tsx
import { useState } from "react";

interface Todo {
    id: number;
    title: string;
    completed: boolean;
}

function App() {
    const [todos, setTodos] = useState<Todo[]>([]);
    const [todoText, setTodoText] = useState("");

    function addTodo() {
        const newTodo: Todo = {
            id: Date.now(),
            title: todoText,
            completed: false
        };

        setTodos([...todos, newTodo]);
    }

    return (
        <div>
            <h1>Todo App</h1>

            <input
                type="text"
                onChange={(event) => setTodoText(event.target.value)}
            />

            <button onClick={addTodo}>Add</button>

            <ul>
                {todos.map((todo) => (
                    <li key={todo.id}>
                        {todo.title}
                    </li>
                ))}
            </ul>
        </div>
    );
}

export default App;
```

## Understand the complete flow

```text
User types
    ↓
onChange
    ↓
setTodoText()
    ↓
todoText contains the text

User clicks Add
    ↓
addTodo()
    ↓
Create newTodo
    ↓
setTodos([...todos, newTodo])
    ↓
todos state changes
    ↓
React re-renders
    ↓
todos.map()
    ↓
<li> is created for each Todo
    ↓
Todo appears on screen
```

### Current progress

```text
Step 1  Clean App.tsx
Step 2  Todo interface
Step 3  Todo state
Step 4  Input state
Step 5  Input + Add button
Step 6  Create and add Todo
Step 7  Display Todo list
```

Next is **Step 8 — Create a separate `TodoItem` component**. This is where we'll properly learn **Props** by passing a Todo from `App` to `TodoItem`.



-------------------------------------------------------------------------------------------------------------------------------------------


# Todo App — Step 8: Create `TodoItem` Component

Now we're going to learn **Props** properly.

Until now, `App` is doing everything:

```text
App
 ├── Input
 ├── Add button
 └── Todo list
```

As the application gets bigger, we don't want one component doing everything.

So we'll create a separate component:

```text
App
 │
 ├── Input
 ├── Add button
 │
 └── TodoItem
       ├── Todo 1
       ├── Todo 2
       └── Todo 3
```

## 1. Create a new file

Inside `src`, create:

```text id="4v8xq3"
TodoItem.tsx
```

Your structure becomes:

```text id="m2p7sa"
src/
├── App.tsx
├── TodoItem.tsx
└── main.tsx
```

---

## 2. Create the component

Inside `TodoItem.tsx`:

```tsx id="s1h4y9"
interface TodoItemProps {
    todo: {
        id: number;
        title: string;
        completed: boolean;
    };
}

function TodoItem({ todo }: TodoItemProps) {
    return (
        <li>
            {todo.title}
        </li>
    );
}

export default TodoItem;
```

Don't worry about the syntax yet. The important idea is:

> `TodoItem` receives a Todo from `App`.

---

# 3. What are Props?

Think of Props as **information passed from a parent component to a child component**.

Here:

```text id="p9g2wv"
App
 ↓
TodoItem
```

`App` is the **parent**.

`TodoItem` is the **child**.

We pass:

```text id="w3m8n2"
todo
 ↓
TodoItem
```

So:

```text id="0j8y4v"
App
 │
 │ todo
 ↓
TodoItem
```

---

# 4. Use `TodoItem` in `App`

At the top of `App.tsx`, import it:

```tsx id="6q3w5e"
import TodoItem from "./TodoItem";
```

Then change this:

```tsx id="6g8p1j"
<li key={todo.id}>
    {todo.title}
</li>
```

to:

```tsx id="9k2v7x"
<TodoItem key={todo.id} todo={todo} />
```

So your `.map()` becomes:

```tsx id="3x7m1a"
{todos.map((todo) => (
    <TodoItem key={todo.id} todo={todo} />
))}
```

---

# 5. Understand this line

```tsx id="h7x2q4"
<TodoItem key={todo.id} todo={todo} />
```

There are **two different things** here:

```text id="f5s8k2"
key={todo.id}
```

This is for **React's list tracking**.

And:

```text id="a2m9q6"
todo={todo}
```

This is a **Prop** we're passing to the child component.

So:

```text id="h0r5s9"
App
 │
 │ todo={todo}
 ↓
TodoItem
 │
 ↓
todo.title
```

---

# Full Code Now

### `TodoItem.tsx`

```tsx id="j7q3m1"
interface TodoItemProps {
    todo: {
        id: number;
        title: string;
        completed: boolean;
    };
}

function TodoItem({ todo }: TodoItemProps) {
    return (
        <li>
            {todo.title}
        </li>
    );
}

export default TodoItem;
```

### `App.tsx`

```tsx id="q2m8v5"
import { useState } from "react";
import TodoItem from "./TodoItem";

interface Todo {
    id: number;
    title: string;
    completed: boolean;
}

function App() {
    const [todos, setTodos] = useState<Todo[]>([]);
    const [todoText, setTodoText] = useState("");

    function addTodo() {
        const newTodo: Todo = {
            id: Date.now(),
            title: todoText,
            completed: false
        };

        setTodos([...todos, newTodo]);
    }

    return (
        <div>
            <h1>Todo App</h1>

            <input
                type="text"
                onChange={(event) => setTodoText(event.target.value)}
            />

            <button onClick={addTodo}>Add</button>

            <ul>
                {todos.map((todo) => (
                    <TodoItem key={todo.id} todo={todo} />
                ))}
            </ul>
        </div>
    );
}

export default App;
```

---

## The important concept

Before:

```text id="n6f4c2"
App
 ↓
<li>{todo.title}</li>
```

Now:

```text id="s8k3w1"
App
 ↓
TodoItem
 ↓
todo.title
```

We're **breaking our UI into reusable components**.

And Props are the way the parent gives information to the child.

Next → **Step 9: Add completion functionality**, where `TodoItem` will need to tell `App` that the user clicked the Todo.



-------------------------------------------------------------------------------------------------------------------------------------------


# Todo App — Step 9: Props + Completion

Now we'll make the Todo clickable.

The goal:

```text
Learn React
```

When clicked:

```text
~~Learn React~~
```

The important part is that **`TodoItem` is the child**, but the `todos` state is inside **`App`**.

So we need a way for the child to tell the parent:

> "The user clicked this Todo."

---

## 1. Why can't `TodoItem` directly change `todos`?

Our state is here:

```text id="r4m7f1"
App
 └── todos state
```

But `TodoItem` is here:

```text id="a9v2k6"
TodoItem
```

The relationship is:

```text id="j6s3p8"
App
 ↓
TodoItem
```

The parent sends data **down** using Props.

But when the child wants to notify the parent, we can also pass a **function as a prop**.

---

# 2. Create a completion function in `App`

Inside `App`:

```tsx id="z4m8q2"
function toggleTodo(id: number) {
    setTodos(
        todos.map((todo) =>
            todo.id === id
                ? { ...todo, completed: !todo.completed }
                : todo
        )
    );
}
```

Don't worry about the whole line yet.

The basic idea is:

```text id="y8n3c5"
toggleTodo(id)
      ↓
Find the Todo
      ↓
Change completed
      ↓
setTodos()
      ↓
React updates UI
```

---

# 3. Pass the function to `TodoItem`

Change:

```tsx id="h6k2p1"
<TodoItem key={todo.id} todo={todo} />
```

to:

```tsx id="3r9v5m"
<TodoItem
    key={todo.id}
    todo={todo}
    onToggle={toggleTodo}
/>
```

Now we're passing **two Props**:

```text id="v1q8s6"
todo
 ↓
Todo information

onToggle
 ↓
Function to toggle the Todo
```

---

# 4. Receive the function in `TodoItem`

Update the Props:

```tsx id="s3m7x2"
interface TodoItemProps {
    todo: {
        id: number;
        title: string;
        completed: boolean;
    };

    onToggle: (id: number) => void;
}
```

Now `TodoItem` receives:

```tsx id="j5v9q4"
function TodoItem({ todo, onToggle }: TodoItemProps) {
```

And we can use it when the `<li>` is clicked:

```tsx id="x8k2m6"
<li onClick={() => onToggle(todo.id)}>
    {todo.title}
</li>
```

---

# 5. Complete flow

This is the important part.

```text id="q2f7n9"
User clicks Todo
       ↓
TodoItem
       ↓
onToggle(todo.id)
       ↓
toggleTodo(id) in App
       ↓
setTodos(...)
       ↓
Todo state changes
       ↓
React re-renders
       ↓
TodoItem receives updated todo
       ↓
UI changes
```

So the data flow is:

```text id="a6k3w8"
        App
         │
         │ todo
         │ onToggle
         ↓
     TodoItem
         │
         │ user clicks
         ↓
      onToggle()
         │
         ↓
        App
```

This is a **very important React pattern**:

> **Data goes down through Props, and child components can communicate back up by calling functions passed through Props.**

---

## One more change: show completed Todo

Inside `TodoItem`, we can use conditional styling:

```tsx id="k9p3w7"
<li
    onClick={() => onToggle(todo.id)}
    style={{
        textDecoration: todo.completed
            ? "line-through"
            : "none"
    }}
>
    {todo.title}
</li>
```

Now:

```text id="x5m2q8"
completed = false
      ↓
Learn React

click
  ↓
completed = true
  ↓
~~Learn React~~
```

### Current progress

```text id="m8q4z1"
Step 1  Clean project
Step 2  Todo interface
Step 3  Todo state
Step 4  Input state
Step 5  Input + Add
Step 6  Create Todo
Step 7  Display list
Step 8  TodoItem + Props
Step 9  Complete Todo + callback Props
```

Next → **Step 10: Delete Todo**, where we'll use the same parent/child communication pattern to remove a Todo.



-------------------------------------------------------------------------------------------------------------------------------------------


# Todo App — Step 10: Delete Todo

Now that `toggleTodo` is clear, deleting will be easier because the **same parent → child function pattern** is used.

## 1. Create `deleteTodo` in `App.tsx`

The `todos` state belongs to `App`, so `App` should handle deleting.

```tsx id="q8n4m2"
function deleteTodo(id: number) {
    setTodos(todos.filter((todo) => todo.id !== id));
}
```

Read it simply:

> Keep every Todo **except** the Todo whose ID was clicked.

For example:

```text id="d6k2p9"
Before:

Todo 1
Todo 2
Todo 3

deleteTodo(2)

After:

Todo 1
Todo 3
```

---

## 2. Pass `deleteTodo` to `TodoItem`

```tsx id="z4m7x1"
<TodoItem
    key={todo.id}
    todo={todo}
    onToggle={toggleTodo}
    onDelete={deleteTodo}
/>
```

Now `TodoItem` has two functions:

```text id="v2h8q5"
onToggle
   ↓
complete / uncomplete Todo

onDelete
   ↓
delete Todo
```

---

## 3. Add the Delete button

Inside `TodoItem.tsx`:

```tsx id="n5k3r7"
<button onClick={() => onDelete(todo.id)}>
    Delete
</button>
```

When the user clicks Delete:

```text id="j8f2w6"
Click Delete
     ↓
onDelete(todo.id)
     ↓
deleteTodo(id) in App
     ↓
setTodos(...)
     ↓
Todo is removed from state
     ↓
React re-renders
     ↓
Todo disappears
```

---

# Full Code

### `src/App.tsx`

```tsx id="x6p3m9"
import { useState } from "react";
import TodoItem from "./TodoItem";

interface Todo {
    id: number;
    title: string;
    completed: boolean;
}

function App() {
    const [todos, setTodos] = useState<Todo[]>([]);
    const [todoText, setTodoText] = useState("");

    function addTodo() {
        if (todoText.trim() === "") {
            return;
        }

        const newTodo: Todo = {
            id: Date.now(),
            title: todoText,
            completed: false
        };

        setTodos([...todos, newTodo]);
        setTodoText("");
    }

    function toggleTodo(id: number) {
        setTodos(
            todos.map((todo) =>
                todo.id === id
                    ? { ...todo, completed: !todo.completed }
                    : todo
            )
        );
    }

    function deleteTodo(id: number) {
        setTodos(todos.filter((todo) => todo.id !== id));
    }

    return (
        <div>
            <h1>Todo App</h1>

            <input
                type="text"
                value={todoText}
                onChange={(event) => setTodoText(event.target.value)}
            />

            <button onClick={addTodo}>Add</button>

            <ul>
                {todos.map((todo) => (
                    <TodoItem
                        key={todo.id}
                        todo={todo}
                        onToggle={toggleTodo}
                        onDelete={deleteTodo}
                    />
                ))}
            </ul>
        </div>
    );
}

export default App;
```

### `src/TodoItem.tsx`

```tsx id="r9k4v2"
interface Todo {
    id: number;
    title: string;
    completed: boolean;
}

interface TodoItemProps {
    todo: Todo;
    onToggle: (id: number) => void;
    onDelete: (id: number) => void;
}

function TodoItem({
    todo,
    onToggle,
    onDelete
}: TodoItemProps) {
    return (
        <li>
            <span
                onClick={() => onToggle(todo.id)}
                style={{
                    textDecoration: todo.completed
                        ? "line-through"
                        : "none"
                }}
            >
                {todo.title}
            </span>

            <button onClick={() => onDelete(todo.id)}>
                Delete
            </button>
        </li>
    );
}

export default TodoItem;
```

## The important flow

You now have **three actions**:

```text id="w7m2q5"
Add
 ↓
addTodo()
 ↓
setTodos()
```

```text id="c4n8y1"
Click Todo
 ↓
onToggle(todo.id)
 ↓
toggleTodo(id)
 ↓
setTodos()
```

```text id="p6r3k9"
Click Delete
 ↓
onDelete(todo.id)
 ↓
deleteTodo(id)
 ↓
setTodos()
```

Notice something important:

**`TodoItem` doesn't directly change `todos`.**

`App` owns the state:

```text id="u2k7m4"
App
 └── todos
```

`TodoItem` only tells `App` what happened:

```text id="e8q5v1"
TodoItem
   ↓
"User clicked this Todo"
   ↓
App changes state
   ↓
React updates UI
```

That's the main **Props + State + Event Handling** pattern in this Todo app.

Next we'll do a **final cleanup/review of the Todo App**, including why `value={todoText}` is used and the complete data flow.


-------------------------------------------------------------------------------------------------------------------------------------------