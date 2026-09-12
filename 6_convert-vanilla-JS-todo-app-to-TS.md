| #  | Topic          | What we'll learn                |
| -- | -------------- | ------------------------------- |
| 1  | Project Setup  | `.ts` files & TypeScript setup  |
| 2  | Variables      | Types & type inference          |
| 3  | Interface      | Create `Todo` interface         |
| 4  | Arrays         | Type Todo arrays                |
| 5  | Functions      | Type parameters & returns       |
| 6  | DOM            | Type HTML elements              |
| 7  | Events         | Type click events               |
| 8  | Fetch          | Type API data                   |
| 9  | Async/Await    | Type async functions            |
| 10 | Error Handling | Handle API errors               |
| 11 | Final App      | Complete Todo app in TypeScript |

------------------------------------------------------------------------------------------------------------------------------------------

# TypeScript Project — Convert Our Todo App

We'll take the **same Todo app you already built** and convert it from JavaScript to TypeScript.

We won't change the functionality yet. Our first goal is simply:

> **Make the existing Todo code work with TypeScript.**

---

## Step 1 — JavaScript vs TypeScript

Your JavaScript file was:

```text
script.js
```

We'll eventually have:

```text
script.ts
```

The important difference is:

```text
.js  → JavaScript
.ts  → TypeScript
```

---

## Step 2 — Start with our Todo object

In JavaScript, we had:

```javascript
const todo = {
    title: todoText,
    completed: false
};
```

TypeScript can understand this automatically.

But now we want to create a proper **Todo interface**.

```typescript
interface Todo {
    id: number;
    title: string;
    completed: boolean;
}
```

This is the blueprint for our Todo.

Think of it as:

```text
Todo
│
├── id        → number
├── title     → string
└── completed → boolean
```

So whenever we say:

```typescript
Todo
```

TypeScript knows exactly what a Todo should contain.

---

## Step 3 — Use the interface

Now we can create a Todo:

```typescript
const todo: Todo = {
    id: 1,
    title: "Learn TypeScript",
    completed: false
};
```

TypeScript checks it:

```text
id
1
↓
number ✓

title
"Learn TypeScript"
↓
string ✓

completed
false
↓
boolean ✓
```

Everything matches.

---

## One important difference from our POST code

Earlier, when we created a new Todo, we didn't provide an `id`:

```javascript
const todo = {
    title: todoText,
    completed: false
};
```

That's because we expected the **API to give us the ID**.

Our interface currently says `id` is required.

We'll handle that properly when we convert the full Todo app.

**Don't worry about changing anything yet.**

The first thing I want you to understand is this:

> `interface Todo` describes what a Todo looks like, and `Todo` can then be used as a type.

Next we'll convert the **HTML elements** such as `todoInput`, `addButton`, and `todoList` to TypeScript.

------------------------------------------------------------------------------------------------------------------------------------------

# Step 2 — Type the HTML Elements

In JavaScript, we had:

```javascript
const todoInput = document.getElementById("todoInput");
const addButton = document.getElementById("addButton");
const todoList = document.getElementById("todoList");
```

In TypeScript, there's an important question:

> **What type of HTML element is each one?**

---

## 1. `todoInput`

Our HTML is something like:

```html
<input id="todoInput">
```

So this is an **HTMLInputElement**.

We can write:

```typescript
const todoInput = document.getElementById("todoInput") as HTMLInputElement;
```

The important part is:

```text
HTMLInputElement
```

It tells TypeScript:

> `todoInput` is an HTML input element.

That's useful because we use:

```typescript
todoInput.value
```

and TypeScript knows that an input has a `value`.

---

## 2. `addButton`

Our HTML:

```html
<button id="addButton">Add</button>
```

So we can tell TypeScript:

```typescript
const addButton = document.getElementById("addButton") as HTMLButtonElement;
```

Now TypeScript knows:

```text
addButton
   ↓
HTMLButtonElement
```

---

## 3. `todoList`

Our HTML:

```html
<ul id="todoList"></ul>
```

This is an unordered list.

So:

```typescript
const todoList = document.getElementById("todoList") as HTMLUListElement;
```

Now TypeScript knows:

```text
todoList
   ↓
HTMLUListElement
```

---

# Why are we doing this?

Remember, TypeScript wants to know **what type something is**.

Previously:

```javascript
todoInput.value
```

JavaScript doesn't care much about the element's type.

TypeScript wants to know:

```text
todoInput
   ↓
HTMLInputElement
   ↓
has .value
```

Similarly:

```text
addButton
   ↓
HTMLButtonElement
   ↓
button properties/events
```

and:

```text
todoList
   ↓
HTMLUListElement
   ↓
list properties
```

---

## The three lines

```typescript
const todoInput = document.getElementById("todoInput") as HTMLInputElement;

const addButton = document.getElementById("addButton") as HTMLButtonElement;

const todoList = document.getElementById("todoList") as HTMLUListElement;
```

The new thing here is `as`.

```text
as HTMLInputElement
```

means:

> **Treat this element as an HTML input element.**

For now, just remember that.

---

There are **many** DOM element types in TypeScript. You don't need to memorize all of them. The important thing is to know the common ones you'll actually use.

### Common HTML element types

| HTML         | TypeScript type        |
| ------------ | ---------------------- |
| `<input>`    | `HTMLInputElement`     |
| `<button>`   | `HTMLButtonElement`    |
| `<ul>`       | `HTMLUListElement`     |
| `<ol>`       | `HTMLOListElement`     |
| `<li>`       | `HTMLLIElement`        |
| `<div>`      | `HTMLDivElement`       |
| `<p>`        | `HTMLParagraphElement` |
| `<h1>`       | `HTMLHeadingElement`   |
| `<form>`     | `HTMLFormElement`      |
| `<textarea>` | `HTMLTextAreaElement`  |
| `<select>`   | `HTMLSelectElement`    |
| `<option>`   | `HTMLOptionElement`    |
| `<a>`        | `HTMLAnchorElement`    |
| `<img>`      | `HTMLImageElement`     |
| `<table>`    | `HTMLTableElement`     |

There are more, but **you don't need to learn them all now**.

### Notice the pattern

The HTML tag:

```text
<button>
```

becomes:

```text
HTMLButtonElement
```

The HTML tag:

```text
<input>
```

becomes:

```text
HTMLInputElement
```

The HTML tag:

```text
<div>
```

becomes:

```text
HTMLDivElement
```

So you can often recognize the type from the HTML element name.

### One important thing

You will also see:

```typescript
HTMLElement
```

This is a **general HTML element type**.

For example, if you don't need anything specific to an input, button, etc., `HTMLElement` can be enough.

Think of it like:

```text
HTMLElement
    │
    ├── HTMLInputElement
    ├── HTMLButtonElement
    ├── HTMLDivElement
    ├── HTMLParagraphElement
    └── ...
```

So don't try to memorize 50+ types.

For our Todo project, remember these:

**`HTMLInputElement` → input**

**`HTMLButtonElement` → button**

**`HTMLUListElement` → ul**

**`HTMLLIElement` → li**

**`HTMLElement` → general HTML element**

------------------------------------------------------------------------------------------------------------------------------------------

# Step 3 — Type the `displayTodo()` Function

Now let's convert the function you already understand.

In JavaScript, we had:

```javascript
function displayTodo(todo) {
```

The problem is:

> TypeScript doesn't know what `todo` is.

We already created our interface:

```typescript
interface Todo {
    id: number;
    title: string;
    completed: boolean;
}
```

So we can tell TypeScript:

```typescript
function displayTodo(todo: Todo) {
```

Now TypeScript knows:

```text
todo
 ↓
Todo
 ↓
├── id → number
├── title → string
└── completed → boolean
```

---

## Why is this useful?

Inside the function we have:

```typescript
todo.title
```

TypeScript knows:

```text
todo.title → string
```

And:

```typescript
todo.completed
```

TypeScript knows:

```text
todo.completed → boolean
```

And:

```typescript
todo.id
```

TypeScript knows:

```text
todo.id → number
```

So TypeScript can catch mistakes.

For example:

```typescript
todo.title = 100;
```

would be an error because:

```text
todo.title
   ↓
string
```

but you're trying to give it:

```text
100
 ↓
number
```

---

## Our function now starts like this

```typescript
function displayTodo(todo: Todo) {
    const todoItem = document.createElement("li");

    todoItem.textContent = todo.title;

    if (todo.completed) {
        todoItem.style.textDecoration = "line-through";
    }
}
```

Notice something important:

We didn't have to change the actual logic.

We mainly added:

```text
: Todo
```

That's one of the main benefits of TypeScript.

> **Your JavaScript logic stays almost the same; TypeScript adds type safety around it.**

------------------------------------------------------------------------------------------------------------------------------------------

# Step 4 — TypeScript Can Infer `todoItem`

Look at this:

```typescript
const todoItem = document.createElement("li");
```

Do we need to write:

```typescript
const todoItem: HTMLLIElement = ...
```

No.

TypeScript can figure it out automatically.

Because we're creating:

```text
<li>
```

TypeScript understands:

```text
todoItem
   ↓
HTMLLIElement
```

This is **type inference**, which we learned earlier.

---

### Same thing with our other elements

When we write:

```typescript
const deleteButton = document.createElement("button");
```

TypeScript automatically knows:

```text
deleteButton
     ↓
HTMLButtonElement
```

And:

```typescript
const todoItem = document.createElement("li");
```

becomes:

```text
todoItem
    ↓
HTMLLIElement
```

So we don't need to explicitly write the types.

---

## This is a good example of inference

Earlier we learned:

```typescript
let age = 25;
```

TypeScript understands:

```text
age → number
```

Now:

```typescript
const todoItem = document.createElement("li");
```

TypeScript understands:

```text
todoItem → HTMLLIElement
```

Same concept.

---

## Why is this useful?

Because TypeScript now knows which properties and methods are available.

For example:

```typescript
todoItem.textContent
```

TypeScript knows `todoItem` is an `HTMLLIElement`.

And:

```typescript
deleteButton.textContent
```

TypeScript knows `deleteButton` is an `HTMLButtonElement`.

You **don't need to manually type every variable**.

### Important rule

> **Use explicit types when they help define something; let TypeScript infer types when it can clearly figure them out.**

We've now converted:

* `Todo` → interface
* HTML elements → appropriate types
* `displayTodo(todo)` → `todo: Todo`
* `createElement()` → TypeScript inference

------------------------------------------------------------------------------------------------------------------------------------------

# Step 5 — TypeScript + Event Listeners

Now we'll look at the event listeners in your Todo app.

You already understand this JavaScript:

```javascript
todoItem.addEventListener("click", () => {
    todoItem.style.textDecoration = "line-through";
});
```

In TypeScript, **this can stay almost exactly the same**.

Why?

Because TypeScript already knows that `todoItem` is an `HTMLLIElement`, and `addEventListener()` knows what kind of event we're listening for.

---

## Delete button

You had:

```javascript
deleteButton.addEventListener("click", (event) => {
    event.stopPropagation();
    todoItem.remove();
});
```

In TypeScript, this also works:

```typescript
deleteButton.addEventListener("click", (event) => {
    event.stopPropagation();
    todoItem.remove();
});
```

You might wonder:

> **What type is `event`?**

Because this is a `"click"` event, TypeScript can infer the event type automatically.

So you don't normally need to write:

```typescript
(event: MouseEvent)
```

TypeScript can figure it out.

---

## Why is this useful?

Remember **type inference**?

Here it happens again:

```typescript
deleteButton.addEventListener("click", (event) => {
```

TypeScript sees:

```text
"click"
  ↓
Mouse event
  ↓
event is MouseEvent
```

So TypeScript knows that:

```typescript
event.stopPropagation();
```

is valid.

---

## Compare JavaScript and TypeScript

### JavaScript

```javascript
deleteButton.addEventListener("click", (event) => {
    event.stopPropagation();
    todoItem.remove();
});
```

### TypeScript

```typescript
deleteButton.addEventListener("click", (event) => {
    event.stopPropagation();
    todoItem.remove();
});
```

**Exactly the same.**

That's an important point:

> **You don't add TypeScript types everywhere. TypeScript often figures them out for you.**

---

### One thing to remember

If TypeScript **cannot figure out the type**, then we can explicitly provide it.

But when the type is obvious from something like `addEventListener("click", ...)`, TypeScript usually handles it automatically.

So our `displayTodo()` is now mostly converted:

```typescript
function displayTodo(todo: Todo) {
    const todoItem = document.createElement("li");

    todoItem.textContent = todo.title;

    if (todo.completed) {
        todoItem.style.textDecoration = "line-through";
    }

    todoItem.addEventListener("click", () => {
        todoItem.style.textDecoration = "line-through";
    });

    const deleteButton = document.createElement("button");

    deleteButton.textContent = "Delete";

    todoItem.appendChild(deleteButton);

    deleteButton.addEventListener("click", (event) => {
        event.stopPropagation();
        todoItem.remove();
    });

    todoList.appendChild(todoItem);
}
```

The main TypeScript change here was:

```typescript
todo: Todo
```

The rest can remain almost the same because TypeScript is **inferring the types for us**.

------------------------------------------------------------------------------------------------------------------------------------------

# Step 6 — Convert `addTodo()` to TypeScript

Now we'll convert the API function you already understand.

Our JavaScript was:

```javascript
async function addTodo(todo) {
    const response = await fetch(...);
    const newTodo = await response.json();

    return newTodo;
}
```

The first thing we can type is the `todo` parameter.

We already have:

```typescript
interface Todo {
    id: number;
    title: string;
    completed: boolean;
}
```

So we could write:

```typescript
async function addTodo(todo: Todo) {
```

But there's a problem.

When we're **creating** a new Todo, we don't have an `id` yet.

Remember our flow:

```text
User enters Todo
      ↓
Create Todo
      ↓
POST to API
      ↓
API gives us an ID
```

So this object:

```typescript
{
    title: "Learn TypeScript",
    completed: false
}
```

doesn't have an `id`.

But our `Todo` interface requires:

```typescript
id: number;
```

---

# Create a separate type

This is where TypeScript becomes useful.

We can create a type for a **new Todo**:

```typescript
type NewTodo = {
    title: string;
    completed: boolean;
};
```

Now we have two concepts:

```text
Todo
├── id
├── title
└── completed

NewTodo
├── title
└── completed
```

So:

```text
Todo     → Todo we already have
NewTodo  → Todo we are creating
```

Then our function becomes:

```typescript
async function addTodo(todo: NewTodo) {
```

Now TypeScript knows exactly what we're sending to the API.

---

## What about the response?

The API gives us the completed Todo including its ID.

So we can say:

```typescript
async function addTodo(todo: NewTodo): Promise<Todo> {
```

Don't worry if `Promise<Todo>` looks unfamiliar.

You already learned `async` and `await`.

Think of it simply as:

```text
Promise<Todo>
      ↓
"This function will eventually give me a Todo."
```

So:

```typescript
async function addTodo(todo: NewTodo): Promise<Todo> {
```

means:

> This function receives a `NewTodo` and eventually returns a `Todo`.

---

## The full function

```typescript
async function addTodo(todo: NewTodo): Promise<Todo> {
    const response = await fetch(
        "https://jsonplaceholder.typicode.com/todos",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(todo)
        }
    );

    const newTodo: Todo = await response.json();

    return newTodo;
}
```

### Look at the flow

```text
NewTodo
   ↓
addTodo()
   ↓
POST API
   ↓
API creates Todo
   ↓
Todo with id
   ↓
Promise<Todo>
```

This is one of the major benefits of TypeScript with APIs:

> We can tell TypeScript what data we're sending **and** what data we're expecting back.

### One thing to remember

You don't need to memorize `Promise<Todo>` yet.

Just understand:

```text
NewTodo
   ↓
What we SEND

Todo
   ↓
What we RECEIVE
```
---
## 1. What is `Promise<Todo>`?

First forget TypeScript for a moment.

You already learned:

```javascript
const newTodo = await addTodo(todo);
```

You understood that `await` means:

> Wait for the API result.

The important thing is that an `async` function **returns a Promise**.

For example:

```typescript
async function addTodo() {
    // API request
}
```

The function doesn't immediately give you the Todo.

It gives you a **Promise**, which means:

> "I will give you the result later."

Think of it like ordering food:

```text
You order food
      ↓
Restaurant says:
"Wait, your food is coming"
      ↓
Promise
      ↓
Food arrives
      ↓
Todo
```

So:

```typescript
Promise<Todo>
```

means:

> **A Promise that will eventually give us a `Todo`.**

---

### Why `Todo` inside `< >`?

We learned that generics use `< >`.

For example:

```typescript
Promise<string>
```

means:

> A Promise that eventually gives a string.

```typescript
Promise<number>
```

means:

> A Promise that eventually gives a number.

And:

```typescript
Promise<Todo>
```

means:

> A Promise that eventually gives a Todo.

So:

```typescript
async function addTodo(todo: NewTodo): Promise<Todo>
```

means:

```text
Input
 ↓
NewTodo

Function
 ↓
wait for API

Result
 ↓
Todo
```

That's all you need to understand for now.

---

# 2. Why `type NewTodo` instead of `interface NewTodo`?

This is an excellent question.

We could actually use **either**.

For example:

```typescript
interface NewTodo {
    title: string;
    completed: boolean;
}
```

This would work perfectly.

Or:

```typescript
type NewTodo = {
    title: string;
    completed: boolean;
};
```

This also works.

For this simple object, **there isn't a big practical difference**.

---

## So why did we use `type`?

Because TypeScript has two ways to describe structures:

### `interface`

Usually very common for describing the **shape of objects**.

```typescript
interface Todo {
    id: number;
    title: string;
    completed: boolean;
}
```

### `type`

More flexible. It can describe objects **and** things like unions, intersections, etc.

For example:

```typescript
type ID = number | string;
```

You can't use an interface like that.

Remember our earlier lessons:

```text
Union
number | string

Intersection
Person & Employee
```

Those are commonly created with `type`.

------------------------------------------------------------------------------------------------------------------------------------------

# Step 7 — Convert the Add Button to TypeScript

Now we'll connect everything we've learned.

Our JavaScript was:

```javascript
addButton.addEventListener("click", async () => {
    const todoText = todoInput.value;

    const todo = {
        title: todoText,
        completed: false
    };

    const newTodo = await addTodo(todo);

    displayTodo(newTodo);

    todoInput.value = "";
});
```

The good news is: **almost nothing needs to change.**

Why?

Because TypeScript can infer most of the types.

---

## 1. Get the input

```typescript
const todoText = todoInput.value;
```

We already told TypeScript:

```typescript
todoInput → HTMLInputElement
```

Therefore TypeScript knows:

```text
todoInput.value
       ↓
     string
```

So:

```text
todoText → string
```

We don't need:

```typescript
const todoText: string = ...
```

TypeScript already knows it.

---

## 2. Create the New Todo

Now:

```typescript
const todo = {
    title: todoText,
    completed: false
};
```

TypeScript sees:

```text
title
  ↓
string

completed
  ↓
boolean
```

So it understands this is compatible with our `NewTodo` type:

```typescript
type NewTodo = {
    title: string;
    completed: boolean;
};
```

---

## 3. Send it to `addTodo()`

```typescript
const newTodo = await addTodo(todo);
```

Remember our function:

```typescript
async function addTodo(todo: NewTodo): Promise<Todo>
```

So TypeScript checks:

```text
todo
 ↓
NewTodo ✓
```

And because `addTodo()` returns:

```text
Promise<Todo>
```

after `await`:

```text
newTodo
   ↓
Todo
```

That's very useful.

TypeScript now knows that `newTodo` has:

```text
id        → number
title     → string
completed → boolean
```

---

## 4. Display the Todo

So this:

```typescript
displayTodo(newTodo);
```

works because:

```text
newTodo
   ↓
Todo
   ↓
displayTodo(todo: Todo)
```

Everything matches.

---

# The complete flow

```text
User types
    ↓
todoInput.value
    ↓
string
    ↓
NewTodo
    ↓
addTodo()
    ↓
Promise<Todo>
    ↓
await
    ↓
Todo
    ↓
displayTodo()
```

This is the important TypeScript flow we're building.

### Your Add button can remain:

```typescript
addButton.addEventListener("click", async () => {
    const todoText = todoInput.value;

    const todo = {
        title: todoText,
        completed: false
    };

    const newTodo = await addTodo(todo);

    displayTodo(newTodo);

    todoInput.value = "";
});
```

Notice how little TypeScript code we actually added.

That's because **TypeScript's type inference is doing a lot of the work for us.**

------------------------------------------------------------------------------------------------------------------------------------------

# Step 8 — Put the TypeScript Todo App Together

Now let's see how all the pieces connect.

Don't try to memorize this whole file. The goal is to **recognize the TypeScript parts you just learned**.

```typescript
interface Todo {
    id: number;
    title: string;
    completed: boolean;
}

type NewTodo = {
    title: string;
    completed: boolean;
};

const todoInput = document.getElementById("todoInput") as HTMLInputElement;
const addButton = document.getElementById("addButton") as HTMLButtonElement;
const todoList = document.getElementById("todoList") as HTMLUListElement;

function displayTodo(todo: Todo) {
    const todoItem = document.createElement("li");

    todoItem.textContent = todo.title;

    if (todo.completed) {
        todoItem.style.textDecoration = "line-through";
    }

    todoItem.addEventListener("click", () => {
        todoItem.style.textDecoration = "line-through";
    });

    const deleteButton = document.createElement("button");

    deleteButton.textContent = "Delete";

    todoItem.appendChild(deleteButton);

    deleteButton.addEventListener("click", (event) => {
        event.stopPropagation();
        todoItem.remove();
    });

    todoList.appendChild(todoItem);
}

async function addTodo(todo: NewTodo): Promise<Todo> {
    const response = await fetch(
        "https://jsonplaceholder.typicode.com/todos",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(todo)
        }
    );

    const newTodo: Todo = await response.json();

    return newTodo;
}

addButton.addEventListener("click", async () => {
    const todoText = todoInput.value;

    const todo = {
        title: todoText,
        completed: false
    };

    const newTodo = await addTodo(todo);

    displayTodo(newTodo);

    todoInput.value = "";
});
```

## Now look only at the TypeScript parts

### 1. Interface

```typescript
interface Todo {
    id: number;
    title: string;
    completed: boolean;
}
```

**Blueprint for a Todo.**

---

### 2. New Todo type

```typescript
type NewTodo = {
    title: string;
    completed: boolean;
};
```

**Blueprint for a Todo before it gets an ID.**

---

### 3. HTML element types

```typescript
as HTMLInputElement
```

```typescript
as HTMLButtonElement
```

```typescript
as HTMLUListElement
```

**Tell TypeScript what kind of HTML element we're working with.**

---

### 4. Function parameter type

```typescript
function displayTodo(todo: Todo)
```

Means:

> `displayTodo()` expects a Todo.

---

### 5. Function return type

```typescript
async function addTodo(todo: NewTodo): Promise<Todo>
```

Means:

> This function receives a `NewTodo` and eventually returns a `Todo`.

---

### 6. API response type

```typescript
const newTodo: Todo = await response.json();
```

Means:

> We expect the API response to be a Todo.

---

# The biggest thing you should understand

Our JavaScript application already worked.

TypeScript didn't completely change the application.

Instead, we added **information about our data**:

```text
Todo
 ↓
id → number
title → string
completed → boolean
```

And TypeScript checks that information throughout our application.

```text
             Todo interface
                  ↓
        ┌─────────┼─────────┐
        ↓         ↓         ↓
 displayTodo   API response  todo data
        ↓         ↓         ↓
        └─────────┼─────────┘
                  ↓
           TypeScript checks
```

So you have now completed the **TypeScript Todo conversion** at a basic level.

------------------------------------------------------------------------------------------------------------------------------------------
