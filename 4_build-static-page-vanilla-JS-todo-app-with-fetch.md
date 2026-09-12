# Build: static page, vanilla JS todo app with fetch.


> We'll build it in small parts so you understand **HTML → CSS → JS → DOM → Fetch**.


# Todo App — Step 1: Understand what we are building

Our final app will look roughly like:

```text
Todo App

[ Enter todo...             ] [Add]

☐ Learn HTML          [Delete]
☐ Learn JavaScript    [Delete]
☐ Learn React         [Delete]
```

The user should be able to:

1. Add a todo
2. Display todos
3. Mark a todo as completed
4. Delete a todo
5. Get/save todo data using an API with `fetch`

---

# Step 2: Create the project

Create a folder:

```text
todo-app
```

Inside it, create:

```text
todo-app
│
├── index.html
├── style.css
└── script.js
```

You already know these concepts:

* `index.html` → HTML structure
* `style.css` → styling
* `script.js` → JavaScript logic

For now, **don't worry about React**. This is plain/vanilla JavaScript.

---

# Step 3: Start with HTML

Open `index.html`.

We'll first create only the basic structure:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Todo App</title>
  <link rel="stylesheet" href="style.css">
</head>

<body>

  <h1>Todo App</h1>

  <input id="todoInput" type="text" placeholder="Enter todo">

  <button id="addButton">Add</button>

  <ul id="todoList"></ul>

  <script src="script.js"></script>
</body>
</html>
```

Don't worry about understanding every line yet.

The important part is:

```text
Input
  ↓
User types todo

Button
  ↓
User clicks Add

<ul>
  ↓
Todos will appear here
```

---

# Step 4: Connect JavaScript

In `script.js`:

```javascript
const todoInput = document.getElementById("todoInput");
const addButton = document.getElementById("addButton");
const todoList = document.getElementById("todoList");
```

You already learned DOM.

So what are we doing here?

```text
document
   ↓
find HTML element
   ↓
store it in JavaScript variable
```

For example:

```javascript
document.getElementById("todoInput")
```

means:

> Find the HTML element whose ID is `todoInput`.

---

# Step 5: Test the button

Now let's make the button respond to a click.

```javascript
addButton.addEventListener("click", () => {
  console.log("Add button clicked");
});
```

Open your webpage.

Click **Add**.

Then check the browser console.

You should see:

```text
Add button clicked
```

This is our **first small milestone**.

---

# Don't add Fetch yet

This is important.

We learned `fetch`, but **don't immediately put Fetch into the project**.

First build the basic flow:

```text
HTML
 ↓
DOM
 ↓
Button click
 ↓
Get input
 ↓
Create todo
 ↓
Display todo
```

Then we'll add:

```text
Fetch
 ↓
API
 ↓
Get todos
 ↓
Display todos
```

---

# Our project roadmap

We'll build it in this order:

### Phase 1 — HTML

Create:

* Input
* Add button
* Todo list

### Phase 2 — DOM

Learn how JavaScript gets those elements.

### Phase 3 — Add Todo

```text
User types:
"Learn JavaScript"

       ↓

Click Add

       ↓

JavaScript gets the input

       ↓

Todo appears on screen
```

### Phase 4 — Delete

Add a Delete button to each todo.

### Phase 5 — Complete

Click a todo → mark it completed.

### Phase 6 — Fetch

Connect the application to a Todo API.

### Phase 7 — Async/Await

Use what you just learned:

```text
fetch()
  ↓
await
  ↓
response
  ↓
JSON
  ↓
todos
```

### Phase 8 — Improve the app

We'll clean up the code and understand **why** we wrote each part.

---

## Your first task

Create these three files:

```text
todo-app
├── index.html
├── style.css
└── script.js
```

Then put the HTML above into `index.html`.

-------------------------------------------------------------------------------------------------------------------------------------------

# Step 2 — Get the user's input

You already have:

```javascript
const todoInput = document.getElementById("todoInput");
const addButton = document.getElementById("addButton");
const todoList = document.getElementById("todoList");
```

Now update your button click:

```javascript
addButton.addEventListener("click", () => {
  const todoText = todoInput.value;

  console.log(todoText);
});
```

### What happens?

Suppose you type:

```text
Learn JavaScript
```

and click **Add**.

This:

```javascript
todoInput.value
```

gets whatever the user typed.

So:

```text
Input
   ↓
"Learn JavaScript"
   ↓
todoText
```

Then:

```javascript
console.log(todoText);
```

prints:

```text
Learn JavaScript
```

### Your task

Run the app and test:

1. Type `Learn HTML`
2. Click **Add**
3. Check the browser console
4. Type `Learn JavaScript`
5. Click **Add**

You should see both values in the console.

-------------------------------------------------------------------------------------------------------------------------------------------

Now let's make the todo **appear on the webpage** instead of only showing it in the console.

# Step 3 — Display the Todo

Currently we have:

```text
User types
    ↓
todoInput.value
    ↓
JavaScript gets the text
    ↓
console.log()
```

We want:

```text
User types
    ↓
Click Add
    ↓
JavaScript gets the text
    ↓
Create a list item
    ↓
Display it on the page
```

## Add this inside your click function

```javascript
addButton.addEventListener("click", () => {
  const todoText = todoInput.value;

  const todoItem = document.createElement("li");

  todoItem.textContent = todoText;

  todoList.appendChild(todoItem);
});
```

### Let's understand each line

#### 1. Get the input

```javascript
const todoText = todoInput.value;
```

If you type:

```text
Learn JavaScript
```

then:

```text
todoText → "Learn JavaScript"
```

#### 2. Create an `<li>`

```javascript
const todoItem = document.createElement("li");
```

JavaScript creates:

```html
<li></li>
```

#### 3. Put the text inside it

```javascript
todoItem.textContent = todoText;
```

Now it becomes:

```html
<li>Learn JavaScript</li>
```

#### 4. Put it inside the `<ul>`

```javascript
todoList.appendChild(todoItem);
```

Your HTML originally had:

```html
<ul id="todoList"></ul>
```

After clicking Add:

```html
<ul id="todoList">
  <li>Learn JavaScript</li>
</ul>
```

---

## Test it

Type:

```text
Learn HTML
```

Click **Add**.

Then:

```text
Learn JavaScript
```

Click **Add**.

You should see:

```text
Todo App

[ Enter todo... ] [Add]

• Learn HTML
• Learn JavaScript
```

### One more thing

You may notice that after clicking **Add**, the text still remains inside the input.

-------------------------------------------------------------------------------------------------------------------------------------------

# Step 4 — Clear the Input

Currently:

```text
Type todo
   ↓
Click Add
   ↓
Todo appears
   ↓
Text is still in the input ❌
```

We want:

```text
Type todo
   ↓
Click Add
   ↓
Todo appears
   ↓
Input becomes empty ✅
```

Add this line at the end of your click function:

```javascript
todoInput.value = "";
```

So your function becomes:

```javascript
addButton.addEventListener("click", () => {
  const todoText = todoInput.value;

  const todoItem = document.createElement("li");

  todoItem.textContent = todoText;

  todoList.appendChild(todoItem);

  todoInput.value = "";
});
```

### What does this mean?

Remember:

```javascript
todoInput.value
```

gets the value from the input.

But we can also **change** the value:

```javascript
todoInput.value = "";
```

`""` means an empty string.

So:

```text
Before:
[ Learn JavaScript ]

After clicking Add:
[                 ]
```

## Test it

Type:

```text
Learn HTML
```

Click **Add**.

You should get:

```text
• Learn HTML
```

and the input should become empty.

------------------------------------------------------------------------------------------------------------------------------------------

# Step 5 — Delete a Todo

Currently we have:

```text
• Learn HTML
• Learn JavaScript
```

We want:

```text
• Learn HTML       [Delete]
• Learn JavaScript [Delete]
```

And when the user clicks **Delete**, that todo should disappear.

---

## Step 1: Create the Delete button

Inside your existing `addButton` click function, after creating the `li`, add:

```javascript
const deleteButton = document.createElement("button");

deleteButton.textContent = "Delete";
```

Now JavaScript creates:

```html
<button>Delete</button>
```

---

## Step 2: Add the button to the todo

We already have:

```javascript
todoItem.textContent = todoText;
```

Then add:

```javascript
todoItem.appendChild(deleteButton);
```

Now each todo looks like:

```text
Learn HTML [Delete]
```

---

## Step 3: Make Delete work

Add:

```javascript
deleteButton.addEventListener("click", () => {
  todoItem.remove();
});
```

This means:

> When this Delete button is clicked, remove this todo item.

---

## Your complete function

Your code should now look like:

```javascript
addButton.addEventListener("click", () => {
  const todoText = todoInput.value;

  const todoItem = document.createElement("li");

  todoItem.textContent = todoText;

  const deleteButton = document.createElement("button");

  deleteButton.textContent = "Delete";

  todoItem.appendChild(deleteButton);

  deleteButton.addEventListener("click", () => {
    todoItem.remove();
  });

  todoList.appendChild(todoItem);

  todoInput.value = "";
});
```

### Understand the flow

```text
Click Add
   ↓
Get input
   ↓
Create <li>
   ↓
Put todo text inside
   ↓
Create Delete button
   ↓
Put Delete button inside <li>
   ↓
Display <li>
```

Then:

```text
Click Delete
   ↓
todoItem.remove()
   ↓
Todo disappears
```

### Test it

Add:

```text
Learn HTML
Learn CSS
Learn JavaScript
```

You should have three todos, each with its own **Delete** button.

------------------------------------------------------------------------------------------------------------------------------------------


# Step 6 — Mark a Todo as Completed

We want this behavior:

```text
Before:

Learn HTML       [Delete]
Learn JavaScript [Delete]


Click "Learn HTML"


After:

Learn HTML ✓     [Delete]
Learn JavaScript [Delete]
```

We'll use the DOM event you already learned.

## Step 1: Add a click event to the Todo

After creating `todoItem`, add:

```javascript
todoItem.addEventListener("click", () => {
  todoItem.style.textDecoration = "line-through";
});
```

Now when you click the todo:

```text
Learn HTML
    ↓
Click
    ↓
Learn HTML
───────────
```

The `line-through` means the todo is completed.

---

## Step 2: But there is a small problem

Right now, clicking **Delete** will also trigger the todo's click event because the Delete button is inside the `<li>`.

So let's prevent that.

Change your Delete event to:

```javascript
deleteButton.addEventListener("click", (event) => {
  event.stopPropagation();
  todoItem.remove();
});
```

### What is `event.stopPropagation()`?

It means:

> "Don't pass this click event to the parent element."

So:

```text
Todo <li>
│
├── Todo text → click → mark completed
│
└── Delete button → click → delete only
```

---

Now let's understand **event propagation** using the parent/child relationship you understood.

### Your HTML

```text
<li>                    ← Parent
│
├── Learn JavaScript
│
└── [Delete]            ← Child
```

You click **Delete**.

The click happens on the **button first**.

But the browser can then let that event move upward to its parent:

```text
[Delete]
   ↓
  <li>
   ↓
parent
```

This movement is called **event propagation**.

---

### Why is this a problem in our Todo app?

We have two click events.

The `<li>` has:

```javascript
todoItem.addEventListener("click", () => {
  todoItem.style.textDecoration = "line-through";
});
```

Meaning:

> If the `<li>` is clicked → mark the todo as completed.

The button has:

```javascript
deleteButton.addEventListener("click", () => {
  todoItem.remove();
});
```

Meaning:

> If Delete is clicked → remove the todo.

Now imagine:

```text
<li>
  Learn JavaScript
  [Delete]
</li>
```

You click **Delete**.

The button's click happens:

```text
[Delete]
   ↓
Delete it
```

But the click can also propagate to the parent:

```text
[Delete]
   ↓
<li>
   ↓
Mark as completed
```

So the browser could effectively trigger **both handlers**.

---

# This is where `stopPropagation()` comes in

We tell the button:

> "Handle this click, but don't let the click continue to the parent."

```javascript
deleteButton.addEventListener("click", (event) => {
  event.stopPropagation();

  todoItem.remove();
});
```

Now:

```text
Click Delete
     ↓
[Delete button]
     ↓
stopPropagation()
     ↓
STOP
     ✕
   <li>
```

Then:

```text
todoItem.remove()
      ↓
Todo disappears
```

### So remember these three things

**Parent/Child:**

```text
<li> → parent
<button> → child
```

**Propagation:**

```text
Child click → Parent can receive the click
```

**`stopPropagation()`:**

```text
Stop the click from reaching the parent
```

That's the whole idea. You don't need to memorize anything more about it right now.

---

## Your current code

Your function should now look like:

```javascript
addButton.addEventListener("click", () => {
  const todoText = todoInput.value;

  const todoItem = document.createElement("li");

  todoItem.textContent = todoText;

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

  todoInput.value = "";
});
```

## Test

Add:

```text
Learn HTML
Learn CSS
Learn JavaScript
```

Then:

* Click **Learn HTML** → it should get a line through it.
* Click **Learn CSS** → it should get a line through it.
* Click **Delete** → that todo should disappear.

At this point, you have a working basic **vanilla JavaScript Todo app**.

------------------------------------------------------------------------------------------------------------------------------------------

# Step 7 — Connect Todo App to an API

Until now, our todos exist **only in the browser**.

If you refresh the page:

```text
Refresh
  ↓
All todos disappear
```

Why?

Because we are only creating them in the DOM. We aren't storing them anywhere.

Now we'll use an **API** to get todo data.

---

## First understand the flow

Our app will communicate with a server:

```text
Todo App
   ↓
fetch()
   ↓
API / Server
   ↓
Todo data
   ↓
JavaScript
   ↓
DOM
   ↓
Display todos
```

We're going to use the public **JSONPlaceholder** API for learning.

It provides sample todo data.

---

# Step 1 — Create a function to get todos

In `script.js`, create:

```javascript
async function getTodos() {

}
```

You already know what `async` means:

> This function can work with asynchronous operations and returns a Promise.

---

# Step 2 — Use `fetch()`

Inside the function:

```javascript
async function getTodos() {
  const response = await fetch("https://jsonplaceholder.typicode.com/todos");

  console.log(response);
}
```

Now call the function:

```javascript
getTodos();
```

So:

```text
getTodos()
   ↓
fetch()
   ↓
API request
   ↓
Server response
   ↓
response
```

---

# Step 3 — Get the actual data

Right now `response` is the **HTTP response**, not the actual todo data.

We need:

```javascript
const data = await response.json();
```

So:

```javascript
async function getTodos() {
  const response = await fetch("https://jsonplaceholder.typicode.com/todos");

  const data = await response.json();

  console.log(data);
}

getTodos();
```

Now check your browser console.

You should see an array containing todo objects, something like:

```text
[
  {
    userId: 1,
    id: 1,
    title: "delectus aut autem",
    completed: false
  },
  {
    userId: 1,
    id: 2,
    title: "quis ut nam facilis",
    completed: false
  }
]
```

Notice something important:

**This is exactly what we learned earlier about arrays and objects.**

```text
API response
    ↓
Array
    ↓
Objects
    ↓
Each object has:
    userId
    id
    title
    completed
```

---

# Step 4 — Don't display everything yet

For now, just make sure you can see the API data in your console.

Your task:

1. Add `getTodos()`
2. Use `fetch()`
3. Use `await response.json()`
4. `console.log(data)`
5. Refresh the page
6. Check the browser console

Once you see the array of todos, we'll take that API data and **display it inside our Todo app using `map()` and the DOM**.

------------------------------------------------------------------------------------------------------------------------------------------

# Step 9 — Display API Todos

We already have:

```javascript
async function getTodos() {
  const response = await fetch("https://jsonplaceholder.typicode.com/todos");

  const todos = await response.json();

  console.log(todos);
}

getTodos();
```

Instead of only doing:

```javascript
console.log(todos);
```

we'll display them in our `<ul>`.

---

## Step 1: Create a function to display one Todo

Add this:

```javascript
function displayTodo(todo) {
  const todoItem = document.createElement("li");

  todoItem.textContent = todo.title;

  todoList.appendChild(todoItem);
}
```

Remember what we did earlier?

```javascript
const todoItem = document.createElement("li");
```

We create an `<li>`.

Then:

```javascript
todoItem.textContent = todo.title;
```

The API gives us something like:

```text
todo.title
    ↓
"delectus aut autem"
```

So our `<li>` becomes:

```html
<li>delectus aut autem</li>
```

Then:

```javascript
todoList.appendChild(todoItem);
```

puts it inside our `<ul>`.

---

# Step 2: Use the function for every API Todo

Now change:

```javascript
console.log(todos);
```

to:

```javascript
todos.forEach(todo => {
  displayTodo(todo);
});
```

So your Fetch code becomes:

```javascript
async function getTodos() {
  const response = await fetch("https://jsonplaceholder.typicode.com/todos");

  const todos = await response.json();

  todos.forEach(todo => {
    displayTodo(todo);
  });
}

getTodos();
```

---

# What is happening?

The API gives us:

```text
todos
 ↓
[Todo 1, Todo 2, Todo 3, ...]
```

Then:

```javascript
todos.forEach(todo => {
  displayTodo(todo);
});
```

means:

> Go through every Todo and display it.

So:

```text
Todo 1 → displayTodo()
Todo 2 → displayTodo()
Todo 3 → displayTodo()
Todo 4 → displayTodo()
...
```

And the browser shows them:

```text
Todo App

• delectus aut autem
• quis ut nam facilis et officia qui
• fugiat veniam minus
• et porro tempora
...
```

---

## One important thing

Our **Add Todo** functionality and **API Todo** functionality are currently separate.

That's okay.

We're learning them separately first:

```text
                    Todo App
                       │
          ┌────────────┴────────────┐
          ↓                         ↓
     Add Todo                   Fetch API
          ↓                         ↓
    User creates              Server gives
       Todo                      Todos
```
------------------------------------------------------------------------------------------------------------------------------------------

## Step 10 — Add API Todo's `completed` status

Our API Todo contains:

```text
title
completed
```

For example:

```text
title → "delectus aut autem"
completed → false
```

or:

```text
title → "some todo"
completed → true
```

We want our UI to reflect that.

### Currently

Our function is:

```javascript
function displayTodo(todo) {
  const todoItem = document.createElement("li");

  todoItem.textContent = todo.title;

  todoList.appendChild(todoItem);
}
```

This only displays the title.

We can check whether the Todo is completed:

```javascript
if (todo.completed) {
  todoItem.style.textDecoration = "line-through";
}
```

So the function becomes:

```javascript
function displayTodo(todo) {
  const todoItem = document.createElement("li");

  todoItem.textContent = todo.title;

  if (todo.completed) {
    todoItem.style.textDecoration = "line-through";
  }

  todoList.appendChild(todoItem);
}
```

### What does this mean?

Suppose the API gives:

```text
completed: false
```

Then:

```text
Todo
────
No line-through
```

If the API gives:

```text
completed: true
```

Then:

```text
Todo
────────
Line-through
```

So we're using the `if` condition you learned earlier:

```text
todo.completed
      ↓
   true?
   /   \
 yes    no
 ↓       ↓
line    nothing
```

This is a good example of how the things you've learned connect together:

**Object → `todo.completed`**

**`if` → check the value**

**DOM → change the Todo's appearance**

------------------------------------------------------------------------------------------------------------------------------------------

Right now API todos only show their title:

```text
• Todo 1
• Todo 2
• Todo 3
```

We want:

```text
• Todo 1              [Delete]
• Todo 2              [Delete]
• Todo 3              [Delete]
```

And clicking a Todo should mark it completed.

## Step 11 — Add Delete button to API Todos

Our current function is:

```javascript
function displayTodo(todo) {
  const todoItem = document.createElement("li");

  todoItem.textContent = todo.title;

  if (todo.completed) {
    todoItem.style.textDecoration = "line-through";
  }

  todoList.appendChild(todoItem);
}
```

Now add the Delete button:

```javascript
function displayTodo(todo) {
  const todoItem = document.createElement("li");

  todoItem.textContent = todo.title;

  if (todo.completed) {
    todoItem.style.textDecoration = "line-through";
  }

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

### What are we doing?

Same thing we already did earlier.

```text
API Todo
   ↓
Create <li>
   ↓
Put todo.title inside
   ↓
Create Delete button
   ↓
Put Delete button inside <li>
   ↓
Display it
```

And:

```javascript
deleteButton.addEventListener("click", (event) => {
  event.stopPropagation();
  todoItem.remove();
});
```

means:

```text
Click Delete
     ↓
Stop click from reaching <li>
     ↓
Remove the Todo
```

---

## Step 12 — Make API Todo clickable

Now add this before the Delete button:

```javascript
todoItem.addEventListener("click", () => {
  todoItem.style.textDecoration = "line-through";
});
```

Now the API Todo behaves like the Todo we created ourselves:

```text
Click Todo
   ↓
Line-through


Click Delete
   ↓
Todo disappears
```

So our `displayTodo()` now handles:

* Displaying the API Todo
* Checking `completed`
* Marking it completed
* Creating Delete button
* Deleting the Todo
* Preventing Delete click from triggering the Todo click

### Important

We are **not actually updating the API's `completed` value or deleting the Todo from the server yet**.

We're only changing what we see in the browser.

------------------------------------------------------------------------------------------------------------------------------------------

Right now, we have two places that create a Todo:

1. When you click **Add**
2. When you get Todos from the **API**

Both are doing almost the same thing.

Instead of writing the same logic twice, we'll create **one reusable function**.

# Step 13 — Create one `createTodo()` function

We already have:

```javascript
function displayTodo(todo) {
  ...
}
```

Let's make this function responsible for creating and displaying a Todo.

It will receive a Todo object like:

```text
todo
├── title
└── completed
```

Then it creates the `<li>`, Delete button, click behavior, etc.

Your function should be:

```javascript
function displayTodo(todo) {
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

Now this one function handles **all Todo display logic**.

---

# Step 14 — Change the Add button

Previously, your Add button was manually creating the `<li>`:

```javascript
addButton.addEventListener("click", () => {
  const todoText = todoInput.value;

  const todoItem = document.createElement("li");

  // lots of code...
});
```

Now we can make it much simpler.

```javascript
addButton.addEventListener("click", () => {
  const todoText = todoInput.value;

  const todo = {
    title: todoText,
    completed: false
  };

  displayTodo(todo);

  todoInput.value = "";
});
```

Look at what we're doing:

```text
User types
   ↓
"Learn JavaScript"
   ↓
Create Todo object
   ↓
displayTodo(todo)
   ↓
Todo appears
```

---

# Why are we creating an object?

Because API Todos already have this structure:

```javascript
{
  title: "...",
  completed: false
}
```

So we make our own Todo look the same.

Our new Todo:

```javascript
const todo = {
  title: todoText,
  completed: false
};
```

API Todo:

```javascript
{
  title: "delectus aut autem",
  completed: false
}
```

Now both can use:

```javascript
displayTodo(todo);
```

That's the important idea.

---

# The architecture is now cleaner

```text
                    displayTodo()
                         ↑
                  ┌──────┴──────┐
                  │             │
              Add button      API
                  │             │
             create object   get object
                  │             │
                  └──────┬──────┘
                         ↓
                    displayTodo()
                         ↓
                    Show Todo
```

This is why functions are useful:

> **Write the logic once, reuse it whenever you need it.**

------------------------------------------------------------------------------------------------------------------------------------------

# Step 14 — Complete `script.js`

At this point, your `script.js` should look like this:

```javascript
const todoInput = document.getElementById("todoInput");
const addButton = document.getElementById("addButton");
const todoList = document.getElementById("todoList");

function displayTodo(todo) {
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

addButton.addEventListener("click", () => {
  const todoText = todoInput.value;

  const todo = {
    title: todoText,
    completed: false
  };

  displayTodo(todo);

  todoInput.value = "";
});

async function getTodos() {
  const response = await fetch(
    "https://jsonplaceholder.typicode.com/todos"
  );

  const todos = await response.json();

  todos.forEach(todo => {
    displayTodo(todo);
  });
}

getTodos();
```

Now let's **not memorize this**. Let's understand the flow.

---

# 1. First, we find our HTML elements

```javascript
const todoInput = document.getElementById("todoInput");
const addButton = document.getElementById("addButton");
const todoList = document.getElementById("todoList");
```

Remember DOM?

```text
HTML
 ↓
document.getElementById()
 ↓
JavaScript gets the element
```

So JavaScript now knows about:

```text
Input
Button
Todo list
```

---

# 2. `displayTodo()` is our main function

```javascript
function displayTodo(todo) {
```

This function's job is:

> **Take one Todo and display it on the webpage.**

It doesn't care where the Todo came from.

It could come from:

```text
User
 ↓
Add button
```

or:

```text
API
 ↓
Fetch
```

Both can call:

```javascript
displayTodo(todo);
```

---

# 3. Add button creates a Todo

When the user clicks Add:

```javascript
addButton.addEventListener("click", () => {
```

We get the input:

```javascript
const todoText = todoInput.value;
```

Then create an object:

```javascript
const todo = {
  title: todoText,
  completed: false
};
```

For example, if you type:

```text
Learn React
```

we create:

```text
todo
├── title → "Learn React"
└── completed → false
```

Then:

```javascript
displayTodo(todo);
```

displays it.

---

# 4. API also uses the same function

The API gives us many Todo objects.

```javascript
todos.forEach(todo => {
  displayTodo(todo);
});
```

So:

```text
API
 ↓
Todo 1 → displayTodo()
Todo 2 → displayTodo()
Todo 3 → displayTodo()
...
```

This is why we created `displayTodo()`.

---

# 5. Fetch flow

This part:

```javascript
async function getTodos() {
```

means:

> We're creating an asynchronous function.

Then:

```javascript
const response = await fetch(...);
```

means:

```text
Ask API
 ↓
Wait for response
```

Then:

```javascript
const todos = await response.json();
```

means:

```text
Response
 ↓
Convert to JavaScript data
 ↓
todos
```

Then:

```javascript
todos.forEach(...)
```

goes through every Todo.

---

# 6. Final flow of the application

Your application now works like this:

```text
                    Todo App
                       │
          ┌────────────┴────────────┐
          ↓                         ↓
     User clicks Add             getTodos()
          ↓                         ↓
     Get input                    fetch()
          ↓                         ↓
    Create Todo object           API response
          ↓                         ↓
          └────────────┬────────────┘
                       ↓
                  displayTodo()
                       ↓
                 Create <li>
                       ↓
                Display Todo
                       │
             ┌─────────┴─────────┐
             ↓                   ↓
        Click Todo          Click Delete
             ↓                   ↓
        line-through          remove()
```

### You have now built a real vanilla JavaScript Todo app.

And you've used almost everything you've learned:

* Variables
* Objects
* Functions
* Arrow functions
* `if`
* Arrays
* `forEach`
* DOM
* `addEventListener`
* `createElement`
* `appendChild`
* `textContent`
* `async`
* `await`
* Promises
* `fetch`
* JSON
* `stopPropagation()`

There is **one important improvement left**: our app currently fetches 200 todos from the API, which is too many for a simple Todo app, and our Add/Delete/Complete actions don't persist to the server.

------------------------------------------------------------------------------------------------------------------------------------------
## Step 15 — Don't Load 200 Todos

Right now, JSONPlaceholder gives us **200 Todo items**.

That's too many for our small app.

Let's display only the first **10**.

Currently:

```javascript
todos.forEach(todo => {
  displayTodo(todo);
});
```

This means:

> Go through **every** Todo and display it.

We can use the array method you learned: **`slice()`**.

Change it to:

```javascript
todos.slice(0, 10).forEach(todo => {
  displayTodo(todo);
});
```

### What does `slice(0, 10)` mean?

Suppose we have:

```text
Todo 1
Todo 2
Todo 3
...
Todo 200
```

Then:

```javascript
todos.slice(0, 10)
```

takes:

```text
Todo 1
Todo 2
Todo 3
...
Todo 10
```

Then `forEach()` displays those 10.

So:

```text
todos
  ↓
slice(0, 10)
  ↓
first 10 todos
  ↓
forEach()
  ↓
displayTodo()
```

### Why are we doing this?

Not because `slice()` is required for Fetch.

We're doing it to make the application easier to work with while learning.

---

## One important thing to notice

Our current app has a limitation:

If you do:

```text
Add Todo
   ↓
Learn React
```

and then refresh the page:

```text
Refresh
   ↓
Learn React disappears
```

Why?

Because our Add button only changes the **browser page**.

It doesn't send the new Todo to the server.

Similarly:

```text
Delete
   ↓
Todo disappears from screen
```

but it isn't actually deleted from the API.

So our next step is to understand **HTTP methods**:

* `GET` → get data
* `POST` → create data
* `PUT/PATCH` → update data
* `DELETE` → delete data

This will make your Fetch knowledge much more practical.
------------------------------------------------------------------------------------------------------------------------------------------

## Step 16 — HTTP Methods

Now we'll understand how the frontend communicates with an API.

Think of an API like a **server that stores data**.

Your Todo app can tell the server what it wants to do.

### 1. `GET` → Get data

We already used this:

```javascript
fetch("https://jsonplaceholder.typicode.com/todos");
```

This means:

> "Give me the Todos."

```text
App → GET → Server
App ← Todos ← Server
```

---

### 2. `POST` → Create data

When you click **Add**, we could send the new Todo to the server.

```text
App → POST → Server
        ↓
   Create Todo
        ↓
App ← Response ← Server
```

For example:

```text
title: "Learn React"
completed: false
```

---

### 3. `PATCH` → Update data

Suppose you click a Todo and mark it completed.

We could tell the server:

```text
App → PATCH → Server
        ↓
 completed = true
```

So `PATCH` means:

> **Update part of existing data.**

---

### 4. `DELETE` → Delete data

When you click Delete:

```text
App → DELETE → Server
          ↓
      Remove Todo
```

So `DELETE` means:

> **Delete existing data.**

---

# The four you should remember

| Method   | Meaning     |
| -------- | ----------- |
| `GET`    | Get data    |
| `POST`   | Create data |
| `PATCH`  | Update data |
| `DELETE` | Delete data |

Think about your Todo app:

```text
GET
 ↓
Show Todos

POST
 ↓
Add Todo

PATCH
 ↓
Complete Todo

DELETE
 ↓
Delete Todo
```

### Important distinction

Currently our app does this:

```text
Click Add
   ↓
Change browser UI
```

We want eventually:

```text
Click Add
   ↓
POST request
   ↓
Server
   ↓
Create Todo
   ↓
Update UI
```

That's the next improvement we'll make.
------------------------------------------------------------------------------------------------------------------------------------------

## Step 17 — POST: Send a New Todo to the API

Now we'll connect our **Add button** to the API.

Currently, when you click Add:

```text
Click Add
   ↓
Create Todo object
   ↓
Display on screen
```

We want:

```text
Click Add
   ↓
Create Todo object
   ↓
Send it to API using POST
   ↓
API responds
   ↓
Display Todo
```

### 1. Create a POST function

Add this function:

```javascript
async function addTodo(todo) {
  const response = await fetch("https://jsonplaceholder.typicode.com/todos", {
    method: "POST",
    headers: {
      "Content-Type": "application/json"
    },
    body: JSON.stringify(todo)
  });

  const newTodo = await response.json();

  return newTodo;
}
```

Don't worry about all the new parts yet. Let's understand them one by one.

### `method: "POST"`

```text
POST
 ↓
Tell the API:
"I want to create new data."
```

### `body`

```javascript
body: JSON.stringify(todo)
```

This sends our Todo to the server.

Our JavaScript object:

```text
{
  title: "Learn React",
  completed: false
}
```

gets converted into JSON to send over the network.

### `await`

```javascript
const response = await fetch(...)
```

means:

> Wait for the server's response.

Then:

```javascript
const newTodo = await response.json();
```

means:

> Convert the server's response into JavaScript data.

---

## 2. Use it when clicking Add

Change your Add button to:

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

Now the flow is:

```text
User types "Learn React"
          ↓
Create Todo object
          ↓
       addTodo()
          ↓
        POST
          ↓
        API
          ↓
   Server response
          ↓
     newTodo
          ↓
    displayTodo()
```

### One important note

JSONPlaceholder is a **fake practice API**. It simulates creating the Todo and returns a response, but it does **not permanently save your new Todo**.

So if you refresh the page, your newly added Todo will disappear.

That's expected.

The purpose here is to learn how a real frontend would communicate with an API.

------------------------------------------------------------------------------------------------------------------------------------------

Your code does:

```text
Click Add
   ↓
POST request
   ↓
API doesn't respond
   ↓
Error
```

We don't want our application to fail silently.

## Step 18 — Add `try...catch`

You already learned `try`, `catch`, and `finally`.

We'll use `try...catch` here.

Change `addTodo()` to:

```javascript
async function addTodo(todo) {
    try {
        const response = await fetch("https://jsonplaceholder.typicode.com/todos", {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(todo)
        });

        const newTodo = await response.json();

        return newTodo;
    } catch (error) {
        console.log(error);
    }
}
```

### What does this mean?

```text
try
 ↓
Try to send Todo
 ↓
Success?
 ├── Yes → get response
 │
 └── No → catch
            ↓
         show error
```

So:

```javascript
try {
   // code that might fail
}
```

means:

> Try this code.

And:

```javascript
catch (error) {
   // handle the problem
}
```

means:

> If something goes wrong, handle the error here.

---

## One more improvement

We should also check whether the API response was successful.

Add:

```javascript
if (!response.ok) {
    throw new Error("Failed to add todo");
}
```

So:

```javascript
async function addTodo(todo) {
    try {
        const response = await fetch("https://jsonplaceholder.typicode.com/todos", {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(todo)
        });

        if (!response.ok) {
            throw new Error("Failed to add todo");
        }

        const newTodo = await response.json();

        return newTodo;
    } catch (error) {
        console.log(error);
    }
}
```

### Why `response.ok`?

The server can respond even when the request wasn't successful.

`response.ok` tells us whether the HTTP response was successful.

Think:

```text
response.ok
    ↓
true  → continue
false → throw error
```

For now, don't worry about HTTP status codes like `200`, `201`, `404`, etc. We'll learn those separately.

### The important flow

```text
addTodo()
    ↓
try
    ↓
fetch()
    ↓
response
    ↓
response.ok?
   /     \
 true    false
  ↓        ↓
JSON     error
  ↓        ↓
return   catch
```

This makes your API code safer and is a common pattern you'll use in real applications.

------------------------------------------------------------------------------------------------------------------------------------------

## Step 19 — What happens when the API fails?

We added `try...catch` inside `addTodo()`.

But there's one problem with our current code.

If `addTodo()` fails:

```javascript
catch (error) {
    console.log(error);
}
```

then the function doesn't return a Todo.

But our Add button still does:

```javascript
displayTodo(newTodo);
```

So `newTodo` could be `undefined`.

### We want this flow instead:

```text
User clicks Add
      ↓
addTodo()
      ↓
API request
   ↙       ↘
Success    Error
   ↓         ↓
Todo       catch
   ↓         ↓
display    show error
```

We can handle this in the Add button.

Change:

```javascript
const newTodo = await addTodo(todo);

displayTodo(newTodo);
```

to:

```javascript
const newTodo = await addTodo(todo);

if (newTodo) {
    displayTodo(newTodo);
}
```

### What does this mean?

```text
newTodo exists?
   ↓
  YES → displayTodo()
  NO  → don't display
```

So we're protecting the application from trying to display something that wasn't successfully returned.

---

### But there's another improvement

Instead of only:

```javascript
console.log(error);
```

we could show the user an error message.

For now, though, **don't change anything else**.

The important concept here is:

> **When working with APIs, always consider both success and failure.**

You've now learned the basic **CRUD communication flow**:

```text
GET     → Get Todos
POST    → Add Todo
PATCH   → Update Todo
DELETE  → Delete Todo
```
------------------------------------------------------------------------------------------------------------------------------------------

## Step 20 — DELETE Todo from the API

Currently, when you click **Delete**, we only do this:

```javascript
todoItem.remove();
```

That removes the Todo **from the webpage**, but not from the API.

We want:

```text
Click Delete
    ↓
Send DELETE request to API
    ↓
API deletes Todo
    ↓
Remove Todo from webpage
```

### 1. Create a `deleteTodo()` function

Add:

```javascript
async function deleteTodo(id) {
    const response = await fetch(
        `https://jsonplaceholder.typicode.com/todos/${id}`,
        {
            method: "DELETE"
        }
    );

    return response.ok;
}
```

The important part is:

```javascript
method: "DELETE"
```

This tells the API:

> **Delete this Todo.**

And:

```text
/todos/${id}
```

means we're telling the API **which Todo** to delete.

For example, if the Todo has:

```text
id = 5
```

the request goes to:

```text
/todos/5
```

---

## 2. Use it in our Delete button

Previously we had:

```javascript
deleteButton.addEventListener("click", (event) => {
    event.stopPropagation();
    todoItem.remove();
});
```

Now we need to make it asynchronous:

```javascript
deleteButton.addEventListener("click", async (event) => {
    event.stopPropagation();

    const deleted = await deleteTodo(todo.id);

    if (deleted) {
        todoItem.remove();
    }
});
```

### Understand the flow

Suppose we click Delete on Todo `id = 5`.

```text
Click Delete
     ↓
todo.id = 5
     ↓
deleteTodo(5)
     ↓
DELETE /todos/5
     ↓
API responds
     ↓
deleted = true
     ↓
todoItem.remove()
```

So now we're not immediately removing the Todo.

We're first asking the API to delete it.

---

### One important change

This only works for **API Todos**, because API Todos have an `id`.

For example:

```text
{
  id: 5,
  title: "Learn JavaScript",
  completed: false
}
```

Our newly created Todo also gets an `id` from the API after the POST request, so it can use the same Delete logic.

That's why earlier we did:

```javascript
const newTodo = await addTodo(todo);
displayTodo(newTodo);
```

The API gives us the newly created Todo **with an ID**.

------------------------------------------------------------------------------------------------------------------------------------------

Now let's move to the next important operation: **PATCH — updating a Todo when it is completed.**

## Step 21 — Update Todo with PATCH

Currently, when you click a Todo:

```javascript
todoItem.addEventListener("click", () => {
    todoItem.style.textDecoration = "line-through";
});
```

It only changes the **screen**.

The API still has:

```text
completed: false
```

We want to update the API too.

### First, understand the flow

When you click a Todo:

```text
Click Todo
    ↓
PATCH request
    ↓
API
    ↓
completed = true
    ↓
Line-through on screen
```

---

## 1. Create `completeTodo()`

Add this function:

```javascript
async function completeTodo(id) {
    const response = await fetch(
        `https://jsonplaceholder.typicode.com/todos/${id}`,
        {
            method: "PATCH",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify({
                completed: true
            })
        }
    );

    return response.ok;
}
```

### What is different from DELETE?

With DELETE:

```text
DELETE /todos/5
```

We're saying:

> Delete Todo 5.

With PATCH:

```text
PATCH /todos/5
```

we're saying:

> Update Todo 5.

And:

```javascript
body: JSON.stringify({
    completed: true
})
```

means:

> Change its `completed` value to `true`.

---

## 2. Use it when the Todo is clicked

Change your existing click listener:

```javascript
todoItem.addEventListener("click", () => {
    todoItem.style.textDecoration = "line-through";
});
```

to:

```javascript
todoItem.addEventListener("click", async () => {
    const completed = await completeTodo(todo.id);

    if (completed) {
        todoItem.style.textDecoration = "line-through";
    }
});
```

Now the flow is:

```text
User clicks Todo
       ↓
completeTodo(todo.id)
       ↓
PATCH request
       ↓
API
       ↓
completed = true
       ↓
response.ok
       ↓
line-through
```

### Notice the pattern

You are now seeing the same structure repeatedly:

**POST**

```text
Add → API → display
```

**DELETE**

```text
Delete → API → remove from UI
```

**PATCH**

```text
Complete → API → update UI
```

This is the basic pattern of how a frontend communicates with a backend API.

------------------------------------------------------------------------------------------------------------------------------------------

## Step 22 — Empty Todo Validation

Currently, the user can click **Add** without typing anything.

That could create:

```text
[Delete]
```

with no Todo text.

We should prevent that.

In your Add button:

```javascript id="bh2ybc"
addButton.addEventListener("click", async () => {
    const todoText = todoInput.value;

    if (!todoText) {
        return;
    }

    const todo = {
        title: todoText,
        completed: false
    };

    const newTodo = await addTodo(todo);

    if (newTodo) {
        displayTodo(newTodo);
    }

    todoInput.value = "";
});
```

### What does this do?

This:

```javascript id="7j4c7x"
if (!todoText) {
    return;
}
```

means:

> If the input is empty, stop the function.

So:

```text
User clicks Add
       ↓
Is input empty?
   ↙          ↘
 YES          NO
  ↓            ↓
return       Create Todo
               ↓
             POST
               ↓
             API
```

### Why is this useful?

This is called **input validation**.

Before sending data to an API, we should check that the data is valid.

For example:

```text
Empty input       → Don't send
"Learn React"     → Send
"Learn JavaScript" → Send
```

---

### Your Todo app now has the complete basic flow

```text
                    TODO APP
                       │
       ┌───────────────┼───────────────┐
       ↓               ↓               ↓
      GET             POST            PATCH
       ↓               ↓               ↓
  Load Todos       Add Todo       Complete Todo
       │               │               │
       └───────────────┼───────────────┘
                       ↓
                    Display
                       │
                       ↓
                    DELETE
                       ↓
                 Remove Todo
```

You've now covered the main concepts needed for the **vanilla JavaScript Todo + Fetch** project.

The next useful step is to **review the complete project from HTML → CSS → JavaScript → API**, rather than adding more features.

------------------------------------------------------------------------------------------------------------------------------------------










