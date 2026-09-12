# 1. Component Thinking

In React, instead of building the entire webpage as one large component, we **break the UI into smaller components**.

For example, imagine a shopping website:

```text
Website
│
├── Header
├── Navbar
├── ProductList
│   ├── ProductCard
│   ├── ProductCard
│   └── ProductCard
└── Footer
```

Each part can become its own React component.

Think of it like:

**Component = one reusable piece of UI**

---

# 2. Why Do We Need Components?

Imagine putting an entire website inside one `App.tsx`:

```text
App
│
├── Header code
├── Navbar code
├── Product code
├── Cart code
├── Login code
├── Footer code
└── lots and lots of code...
```

As the application becomes bigger, `App.tsx` becomes difficult to understand and maintain.

Instead:

```text
App
│
├── Header
├── Navbar
├── ProductList
├── Cart
└── Footer
```

Now each component has a specific responsibility.

---

# 3. Component = Function

You've already learned this in React Basics.

A React component can be a function:

```tsx
function Header() {
    return <h1>My Website</h1>;
}
```

And:

```tsx
function Footer() {
    return <p>© 2026 My Website</p>;
}
```

Then `App` can use them:

```tsx
function App() {
    return (
        <div>
            <Header />
            <Footer />
        </div>
    );
}
```

So:

```text
App
│
├── Header
│
└── Footer
```

---

# 4. Think of Components Like LEGO Blocks

This is a very useful way to understand Component Thinking.

Imagine building a LEGO house.

You don't create the entire house as **one giant LEGO piece**.

You use smaller pieces:

```text
House
│
├── Door
├── Window
├── Roof
└── Wall
```

React works similarly:

```text
Website
│
├── Header
├── Button
├── Card
├── Form
└── Footer
```

Each component is a building block.

---

# 5. What is a Reusable Component?

Suppose we create:

```tsx
function Button() {
    return <button>Click Me</button>;
}
```

We can use it multiple times:

```tsx
function App() {
    return (
        <div>
            <Button />
            <Button />
            <Button />
        </div>
    );
}
```

The same component is being reused:

```text
Button
  ↓
Button
  ↓
Button
```

This is one of the main benefits of components.

---

# 6. Components Don't Have to Be Huge

A component can be very small.

For example:

```tsx
function Welcome() {
    return <h1>Welcome!</h1>;
}
```

Or it can contain many elements:

```tsx
function UserProfile() {
    return (
        <div>
            <h2>William</h2>
            <p>Developer</p>
            <button>Follow</button>
        </div>
    );
}
```

Both are components.

The important thing is that the component represents **one meaningful part of the UI**.

---

# 7. Component Responsibility

A good component usually has a clear responsibility.

For example:

```text
Header
 ↓
Handles header UI

ProductCard
 ↓
Displays one product

LoginForm
 ↓
Handles login form UI

TodoItem
 ↓
Displays one Todo
```

Avoid creating a component where its purpose is unclear.

Think:

> **What is this component responsible for?**

---

# 8. Our Todo App Already Uses Component Thinking

We actually started doing this in our Todo project.

Remember:

```text
App
│
└── TodoItem
```

Originally, we had:

```tsx
{todos.map((todo) => (
    <li>{todo.title}</li>
))}
```

Everything was inside `App`.

Then we created:

```text
TodoItem.tsx
```

Now:

```text
App
│
└── TodoItem
```

That is **Component Thinking**.

`App` handles the Todo list.

`TodoItem` handles the UI for one Todo.

---

# 9. Why Did We Create `TodoItem`?

Suppose we have:

```text
Todo 1
Todo 2
Todo 3
Todo 4
```

Each Todo needs:

```text
Title
Complete
Delete
```

Instead of writing the same UI repeatedly:

```text
Todo 1 UI
Todo 2 UI
Todo 3 UI
Todo 4 UI
```

we create one component:

```text
TodoItem
```

Then React can reuse it:

```text
TodoItem
   ↓
Todo 1

TodoItem
   ↓
Todo 2

TodoItem
   ↓
Todo 3

TodoItem
   ↓
Todo 4
```

This is much cleaner.

---

# 10. Component Tree

React applications are usually organized as a **component tree**.

For our Todo app:

```text
App
│
├── Input
├── Add Button
│
└── TodoItem
    │
    ├── Todo 1
    ├── Todo 2
    └── Todo 3
```

For a larger application:

```text
App
│
├── Header
│   ├── Logo
│   └── Navigation
│
├── Main
│   ├── Sidebar
│   └── ProductList
│       ├── ProductCard
│       ├── ProductCard
│       └── ProductCard
│
└── Footer
```

This tree helps us understand:

> Which component contains which component?

---

# 11. Parent and Child Components

This is another important concept.

If:

```tsx
function App() {
    return <Header />;
}
```

Then:

```text
App
 ↓
Header
```

`App` is the **parent**.

`Header` is the **child**.

Similarly:

```text
App
 ↓
TodoItem
```

`App` is the parent.

`TodoItem` is the child.

---

# 12. Parent → Child

We already learned that a parent can send information to a child using **Props**.

For example:

```text
App
 │
 │ name = "William"
 ↓
UserProfile
```

The child receives that information.

So:

**Parent → Child = Props**

This is something you already learned in the Todo app.

---

# 13. Reusable Components + Props

Here's where Component Thinking becomes powerful.

We can create one `ProductCard`:

```tsx
function ProductCard({ name }: { name: string }) {
    return <h2>{name}</h2>;
}
```

Then:

```tsx
<ProductCard name="Laptop" />
<ProductCard name="Phone" />
<ProductCard name="Keyboard" />
```

Same component.

Different data.

```text
ProductCard
    │
    ├── Laptop
    ├── Phone
    └── Keyboard
```

So:

**Component = reusable UI structure**

**Props = data given to that component**

---

# 14. Component Thinking in One Picture

```text
                 App
                  │
        ┌─────────┼─────────┐
        ↓         ↓         ↓
      Header    Main      Footer
                  │
             ┌────┴────┐
             ↓         ↓
          Sidebar   ProductList
                       │
              ┌────────┼────────┐
              ↓        ↓        ↓
          Product    Product   Product
           Card       Card      Card
```

Each component has a job.

---

# 15. Easy Way to Remember

Think:

```text
Component
   ↓
Reusable UI building block
```

```text
Props
   ↓
Data passed from parent to child
```

```text
Component Tree
   ↓
Shows how components are connected
```

```text
Component Thinking
   ↓
Break a large UI into smaller meaningful pieces
```

---

# What We Will Learn Next

Your **Component Thinking** section has three main topics:

```text
1. Reusable Components + Composition
           ↓
2. Props Drilling Problem
           ↓
3. Thinking in React
```

We've just covered the basic idea of **reusable components**.

Next we'll learn **Composition**, which is how we combine smaller components together to build larger UI components.

-------------------------------------------------------------------------------------------------------------------------------------------

# Composition

Now we move to the next concept:

> **Component Composition**

Composition sounds complicated, but the idea is simple.

---

# 1. What is Composition?

**Composition means building a bigger component by combining smaller components.**

For example:

```text
Small Components
      ↓
combine them
      ↓
Bigger Component
```

Imagine a webpage:

```text
App
│
├── Header
├── Sidebar
├── MainContent
└── Footer
```

`App` is composed of these smaller components.

That's **composition**.

---

# 2. Simple Example

We have three components:

```tsx
function Header() {
    return <h1>My Website</h1>;
}

function Content() {
    return <p>Welcome to my website.</p>;
}

function Footer() {
    return <p>Copyright 2026</p>;
}
```

Now combine them inside `App`:

```tsx
function App() {
    return (
        <div>
            <Header />
            <Content />
            <Footer />
        </div>
    );
}
```

So:

```text
App
│
├── Header
├── Content
└── Footer
```

`App` is **composed of** Header, Content, and Footer.

---

# 3. Why Do We Use Composition?

Imagine `App` contains everything:

```text
App
│
├── Header code
├── Sidebar code
├── Product code
├── Cart code
├── Login code
├── Footer code
└── lots of code
```

It becomes difficult to manage.

Instead:

```text
App
│
├── Header
├── Sidebar
├── ProductList
├── Cart
└── Footer
```

Each part has its own component.

This makes the application:

* Easier to understand
* Easier to maintain
* Easier to reuse
* Easier to modify

---

# 4. Composition + Props

Composition becomes even more useful when combined with Props.

For example:

```tsx
function UserCard({ name }: { name: string }) {
    return <h2>{name}</h2>;
}
```

Then `App` composes multiple `UserCard`s:

```tsx
function App() {
    return (
        <div>
            <UserCard name="William" />
            <UserCard name="John" />
            <UserCard name="David" />
        </div>
    );
}
```

The structure is:

```text
App
│
├── UserCard
│     └── William
│
├── UserCard
│     └── John
│
└── UserCard
      └── David
```

Same component.

Different Props.

---

# 5. Composition in Our Todo App

We already did this without calling it composition.

Our application:

```text
App
│
└── TodoItem
```

`App` creates the Todo list.

`TodoItem` represents one Todo.

```text
App
│
├── TodoItem → Learn React
├── TodoItem → Learn TypeScript
└── TodoItem → Learn Node.js
```

So `App` is composed of multiple `TodoItem` components.

---

# 6. A Real-World Example

Imagine an e-commerce page.

Instead of:

```text
App
└── Everything
```

we break it down:

```text
App
│
├── Header
│   ├── Logo
│   └── Navbar
│
├── ProductPage
│   ├── ProductImage
│   ├── ProductInfo
│   └── AddToCartButton
│
└── Footer
```

Each smaller component is combined to create the complete page.

That's composition.

---

# 7. `children` — An Important Part of Composition

React gives us a special Prop called:

```text
children
```

It represents the content placed **inside a component**.

For example:

```tsx
function Card({ children }: { children: React.ReactNode }) {
    return (
        <div>
            {children}
        </div>
    );
}
```

We can use it like:

```tsx
<Card>
    <h2>Learn React</h2>
    <p>React is a JavaScript library.</p>
</Card>
```

The content:

```text
<h2>Learn React</h2>
<p>React is a JavaScript library.</p>
```

becomes the `children` of `Card`.

Think:

```text
Card
│
└── children
    ├── h2
    └── p
```

---

# 8. Why is `children` Useful?

It lets us create a **reusable container**.

For example:

```text
Card
```

doesn't need to know exactly what content it will contain.

We can put different things inside:

```tsx
<Card>
    <h2>Profile</h2>
</Card>
```

or:

```tsx
<Card>
    <h2>Product</h2>
    <p>₹999</p>
</Card>
```

or:

```tsx
<Card>
    <button>Buy Now</button>
</Card>
```

Same `Card` component.

Different content.

---

# 9. Composition vs Props

These two concepts are related but different.

### Props

Used to pass **data**:

```text
Parent
  ↓
name="William"
  ↓
Child
```

### Composition

Used to combine **components/UI**:

```text
Parent
  ↓
Child Component
  ↓
Another Component
```

And `children` is one of the main tools React gives us for composition.

---

# Easy Way to Remember

```text
Component
   ↓
Small reusable UI piece
```

```text
Composition
   ↓
Combine small components
   ↓
Create bigger UI
```

```text
Props
   ↓
Pass data to components
```

```text
children
   ↓
Put content/components inside another component
```

### One simple sentence:

**Composition = building a bigger React UI by putting smaller components together.**

Next → **Step 3: Props Drilling Problem** — we'll see what happens when data has to travel through several components just to reach one component.

-------------------------------------------------------------------------------------------------------------------------------------------


# Component Thinking — Step 3: Props Drilling

Now we come to an important React problem:

> **Props Drilling**

Don't worry about the name. The concept is simple.

---

# 1. What is Props Drilling?

**Props drilling means passing data through components that don't actually need that data, just so another deeper component can receive it.**

For example:

```text id="3q7m2x"
App
 ↓
Component A
 ↓
Component B
 ↓
Component C
```

Suppose `App` has some data:

```text id="8k4p1z"
name = "William"
```

But only `Component C` needs it.

Still, we have to pass it through A and B:

```text id="a6n9r3"
App
 │
 │ name
 ↓
Component A
 │
 │ name
 ↓
Component B
 │
 │ name
 ↓
Component C
```

Component A and B don't even use `name`.

They are just **passing it along**.

That's Props Drilling.

---

# 2. Simple Example

Suppose we have:

```tsx id="n4c8v2"
function App() {
    const name = "William";

    return <Parent name={name} />;
}
```

Then:

```tsx id="r7m3k9"
function Parent({ name }: { name: string }) {
    return <Child name={name} />;
}
```

Then:

```tsx id="w2q6p8"
function Child({ name }: { name: string }) {
    return <h1>Hello {name}</h1>;
}
```

The flow is:

```text id="s5j8d1"
App
 │
 │ name
 ↓
Parent
 │
 │ name
 ↓
Child
```

Only `Child` actually needs `name`.

But `Parent` has to receive it and pass it again.

---

# 3. Why Can This Become a Problem?

Imagine the component tree becomes much bigger:

```text id="v8m2q5"
App
 ↓
Page
 ↓
Layout
 ↓
Main
 ↓
Profile
 ↓
UserInfo
 ↓
UserName
```

Now imagine `App` has:

```text id="k6r3x9"
userName
```

And `UserName` needs it.

We may end up doing:

```text id="g2p7w4"
App
 ↓ userName
Page
 ↓ userName
Layout
 ↓ userName
Main
 ↓ userName
Profile
 ↓ userName
UserInfo
 ↓ userName
UserName
```

That's a lot of unnecessary passing.

---

# 4. Important Point

Props themselves are **not bad**.

We use Props all the time in React.

For example:

```text id="p4x8m2"
App
 ↓
TodoItem
```

Passing:

```text id="j9r5q1"
todo={todo}
```

is perfectly normal.

The problem happens when we have:

```text id="y6c3v8"
A → B → C → D → E
```

and we're passing the same data through **A, B, C, and D**, even though only E needs it.

---

# 5. Our Todo App Example

Imagine our Todo app becomes:

```text id="x8k2m4"
App
 ↓
TodoList
 ↓
TodoItem
```

`App` owns:

```text id="v3p7n9"
todos
```

`TodoItem` needs one Todo.

So:

```text id="q5m8r1"
App
 │
 │ todos
 ↓
TodoList
 │
 │ todo
 ↓
TodoItem
```

That's not necessarily a problem.

`TodoList` is directly involved in displaying the list, so passing data through it is reasonable.

---

# 6. When Props Drilling Becomes Painful

Imagine:

```text id="c7x2m5"
App
 ↓
Dashboard
 ↓
Sidebar
 ↓
Profile
 ↓
UserName
```

Only `UserName` needs:

```text id="n4v8q2"
userName
```

But every component has to receive and pass it:

```text id="m8p3z6"
App
 ↓
userName
Dashboard
 ↓
userName
Sidebar
 ↓
userName
Profile
 ↓
userName
UserName
```

This becomes harder to maintain.

If you rename or change the data, you may have to modify many components.

---

# 7. How Do We Solve Props Drilling?

Later in React, you'll learn:

**Context API**

It allows components deeper in the tree to access shared data without manually passing Props through every level.

Instead of:

```text id="r7k2m5"
App
 ↓
A
 ↓
B
 ↓
C
 ↓
D
```

passing the same data everywhere, Context can provide the data to the components that need it.

Conceptually:

```text id="z5q8n2"
        App
         │
      Context
         │
    ┌────┼────┐
    ↓    ↓    ↓
    A    C    D
         ↑
      gets data
```

We'll learn Context properly later in **State Management**.

---

# 8. Don't Use Context for Everything

This is important.

Just because Context exists doesn't mean:

> "Never use Props."

Props are still the normal way to pass data from parent to child.

Use Props when the relationship is simple:

```text id="e2m7q9"
Parent
  ↓
Child
```

Props drilling becomes a concern when data has to travel through many unnecessary intermediate components.

---

# Easy Way to Remember

```text id="u6k3p8"
Props
 ↓
Normal way to pass data
```

```text id="h4m9x2"
Props Drilling
 ↓
Passing data through many components
 ↓
even though intermediate components don't need it
```

```text id="v7q2n5"
Context
 ↓
One solution for shared/deeply needed data
```

### One sentence:

**Props drilling = passing Props through components just to reach a component deeper in the tree.**

Next → **Step 4: Thinking in React** — this is probably the most important part of Component Thinking because it teaches you **how to decide what components you should create in the first place.**


-------------------------------------------------------------------------------------------------------------------------------------------

# Component Thinking — Step 4: Thinking in React

This is the **most important concept** in this section.

"Thinking in React" means:

> **Before writing code, look at the UI and decide how you should break it into components and how the data should flow between them.**

---

# 1. Don't Start With Code

Suppose you need to build this:

```text
Todo App

[ Learn React          ] [Add]

☐ Learn HTML       [Delete]
☑ Learn CSS        [Delete]
☐ Learn React      [Delete]
```

Don't immediately start writing `App.tsx`.

First, look at the UI and ask:

> What are the different parts?

We can identify:

```text
Todo App
│
├── TodoForm
│   ├── Input
│   └── Add Button
│
└── TodoList
    ├── TodoItem
    ├── TodoItem
    └── TodoItem
```

Now we have a component structure.

---

# 2. Start With the UI

A useful process is:

```text
UI
 ↓
Break into components
 ↓
Decide what data is needed
 ↓
Decide where state should live
 ↓
Pass data using Props
 ↓
Write the components
```

This is the basic idea behind **Thinking in React**.

---

# 3. Example: Product Page

Imagine this UI:

```text
--------------------------------
        My Shopping App
--------------------------------

       Laptop
       ₹50,000

       [Add to Cart]

--------------------------------
       Footer
--------------------------------
```

We can break it into:

```text
App
│
├── Header
├── Product
│   ├── ProductName
│   ├── ProductPrice
│   └── AddToCartButton
│
└── Footer
```

We don't necessarily need a component for every single HTML element.

For example, this:

```text
<h1>Laptop</h1>
```

doesn't automatically need to become:

```text
LaptopTitle.tsx
```

That would be unnecessary.

Instead, group things based on **meaning and responsibility**.

---

# 4. How Do We Decide Components?

Ask:

### Question 1

> Is this a separate meaningful part of the UI?

If yes, it might be a component.

For example:

```text
Header
Footer
Sidebar
ProductCard
TodoItem
LoginForm
```

---

### Question 2

> Will I reuse this UI?

If yes, a component is often useful.

For example:

```text
Button
Card
Modal
ProductCard
TodoItem
```

---

### Question 3

> Does this part have its own responsibility?

For example:

```text
LoginForm
```

has the responsibility of handling login UI.

```text
ProductCard
```

has the responsibility of displaying a product.

```text
TodoItem
```

has the responsibility of displaying one Todo.

---

# 5. Don't Over-Create Components

This is also important.

You don't need to create:

```text
App
 ↓
Heading
 ↓
Paragraph
 ↓
Button
```

just because these are different HTML elements.

For example:

```tsx id="n6p3k8"
function Welcome() {
    return (
        <div>
            <h1>Welcome</h1>
            <p>Learn React</p>
            <button>Start</button>
        </div>
    );
}
```

This can perfectly be **one component**.

You create additional components when there is a good reason.

---

# 6. Where Should State Live?

This is one of the most important questions in React.

Suppose:

```text
App
│
├── SearchBox
└── ProductList
```

The user searches for:

```text
Laptop
```

Both `SearchBox` and `ProductList` need to know the search value.

So where should the state live?

Usually, we move the state to their **common parent**:

```text
       App
        │
     search
     /    \
    ↓      ↓
SearchBox ProductList
```

This is called **lifting state up**.

We'll learn this concept more deeply when we work with state and component communication.

---

# 7. Data Flow

React generally follows:

```text
Parent
   ↓
Props
   ↓
Child
```

For example:

```text
App
 │
 │ todo
 ↓
TodoItem
```

And if the child needs to tell the parent something:

```text
Child
 ↓
callback function
 ↓
Parent
 ↓
state update
```

We already did this:

```text
TodoItem
   ↓
onToggle(todo.id)
   ↓
App
   ↓
setTodos()
```

---

# 8. Thinking in React — Our Todo Example

Let's apply everything we've learned.

### Step 1 — Look at UI

```text
[Input] [Add]

Todo 1 [Delete]
Todo 2 [Delete]
Todo 3 [Delete]
```

### Step 2 — Break it into components

```text
App
│
├── TodoForm
│
└── TodoList
    │
    ├── TodoItem
    ├── TodoItem
    └── TodoItem
```

### Step 3 — Identify data

We need:

```text
todos
todoText
```

### Step 4 — Decide where state lives

`App` is the common parent:

```text
App
│
├── TodoForm
└── TodoList
```

So `App` can own the state.

### Step 5 — Pass data

```text
App
 │
 ├── TodoForm
 │      ↑
 │   todoText
 │
 └── TodoList
        ↓
      todos
        ↓
    TodoItem
```

### Step 6 — Handle events

```text
TodoForm
   ↓
Add Todo
   ↓
App
   ↓
setTodos()
```

And:

```text
TodoItem
   ↓
Toggle/Delete
   ↓
App
   ↓
setTodos()
```

---

# 9. The Main Idea

Thinking in React is basically asking these questions:

```text
1. What does my UI look like?
            ↓
2. What components should I create?
            ↓
3. What data do I have?
            ↓
4. Where should the state live?
            ↓
5. Which component needs which data?
            ↓
6. How should components communicate?
```

---

# Easy Way to Remember

```text
Thinking in React
       ↓
Look at UI
       ↓
Break UI into components
       ↓
Identify data
       ↓
Place state
       ↓
Pass Props
       ↓
Handle events
```

## Component Thinking — Completed

You've now covered all three topics from your roadmap:

```text
Reusable Components + Composition     
Props Drilling                        
Thinking in React                     
```

-------------------------------------------------------------------------------------------------------------------------------------------