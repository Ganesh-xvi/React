# Styling

Your roadmap says:

```text
8. Styling

- CSS Modules
- Styled Components
- Tailwind
- Responsive design
- Accessibility basics
```

Before jumping into those tools, we need to understand **why React styling is different from plain CSS**.

---

| Step | Topic                                                     |
| ---- | --------------------------------------------------------- |
| 1    | Styling in React + `className` + CSS Modules introduction |
| 2    | CSS Modules in depth                                      |
| 3    | Conditional/dynamic styling                               |
| 4    | Styled Components                                         |
| 5    | Styled Components in depth                                |
| 6    | Tailwind CSS basics                                       |
| 7    | Tailwind in React + reusable styling                      |
| 8    | Responsive Design                                         |
| 9    | Accessibility basics                                      |
| 10   | **Final Styling Project**                                 |

---


# Step 1 — How Styling Works in React

You already know normal CSS:

```css
button {
    background: blue;
    color: white;
}
```

And HTML:

```html
<button>Click Me</button>
```

In React, we still use CSS.

For example:

```tsx
function App() {
    return (
        <button>Click Me</button>
    );
}
```

Then CSS:

```css
button {
    background: blue;
    color: white;
}
```

So React doesn't replace CSS.

Think:

```text
React
  ↓
HTML structure
  ↓
CSS
  ↓
Appearance
```

---

# 2. Why Do We Need Different Styling Approaches?

Imagine we have:

```text
App
│
├── Navbar
├── ProductCard
├── ProductCard
├── ProductCard
└── Footer
```

Suppose we write:

```css
.card {
    padding: 20px;
}
```

That's fine.

But as the application becomes larger, we might have:

```text
ProductCard
UserCard
OrderCard
ProfileCard
```

and all of them might use:

```css
.card
```

Now styles can accidentally affect components that we didn't intend to change.

This is called a **CSS naming/scope problem**.

---

# 3. Example of the Problem

Imagine:

### ProductCard

```tsx
<div className="card">
    <h2>Laptop</h2>
</div>
```

### UserCard

```tsx
<div className="card">
    <h2>William</h2>
</div>
```

And CSS:

```css
.card {
    padding: 20px;
}
```

Both components receive the same `.card` style.

Sometimes that's useful.

But sometimes we want:

```text
ProductCard
    ↓
Product-specific styles

UserCard
    ↓
User-specific styles
```

That's where different styling approaches become useful.

---

# 4. First: `className`

Before CSS Modules, make sure you understand this React difference.

In HTML:

```html
<div class="card">
```

In React:

```tsx
<div className="card">
```

React uses:

```text
className
```

instead of:

```text
class
```

Example:

```tsx
function ProductCard() {
    return (
        <div className="card">
            <h2>Laptop</h2>
            <p>₹50,000</p>
        </div>
    );
}
```

CSS:

```css
.card {
    padding: 20px;
    border: 1px solid black;
}
```

---

# 5. React Inline Styles

React also allows styles directly inside JSX.

For example:

```tsx
function App() {
    return (
        <h1 style={{ color: "red" }}>
            Hello
        </h1>
    );
}
```

Notice:

```text
style={{ ... }}
```

There are two `{}` because we're putting a JavaScript object inside JSX.

Example:

```tsx
style={{
    color: "red",
    fontSize: "24px"
}}
```

This works.

But we generally don't want to put all our styling directly inside JSX.

For larger applications, separate styling is easier to maintain.

---

# 6. Why CSS Is Still Important

Even though React has different styling approaches, you still need to understand CSS itself.

Your roadmap already covered:

```text
CSS3
├── Box Model
├── Flexbox
└── Grid
```

Those concepts remain important.

Styling tools such as:

```text
CSS Modules
Styled Components
Tailwind
```

are **different ways of writing/organizing styles**.

They don't replace your understanding of:

```text
margin
padding
display
flex
grid
position
width
height
font
color
etc.
```

---

# 7. First Styling Method — CSS Modules

This is the first actual topic in your roadmap.

**CSS Modules** allow CSS to be scoped to a component.

Let's understand that.

Suppose we have:

```text
ProductCard.tsx
ProductCard.module.css
```

Our component:

```tsx
import styles from "./ProductCard.module.css";

function ProductCard() {
    return (
        <div className={styles.card}>
            <h2 className={styles.title}>
                Laptop
            </h2>

            <p className={styles.price}>
                ₹50,000
            </p>
        </div>
    );
}

export default ProductCard;
```

And:

```css
.card {
    padding: 20px;
    border: 1px solid black;
}

.title {
    font-size: 24px;
}

.price {
    font-size: 18px;
}
```

---

# 8. What's Different?

Normal CSS:

```tsx
className="card"
```

CSS:

```css
.card {
}
```

CSS Module:

```tsx
className={styles.card}
```

The CSS file is:

```text
ProductCard.module.css
```

The important difference is:

```text
styles.card
```

---

# 9. Why Is It Called a Module?

Because the CSS is associated with that component.

Think:

```text
ProductCard
│
├── ProductCard.tsx
│
└── ProductCard.module.css
```

The styles are scoped to that component.

Conceptually:

```text
ProductCard
    ↓
its own styles
    ↓
doesn't accidentally clash
with unrelated components
```

That's the main idea.

---

# 10. Compare Normal CSS vs CSS Modules

### Normal CSS

```tsx
<div className="card">
```

```css
.card {
    padding: 20px;
}
```

The class name is global.

---

### CSS Modules

```tsx
<div className={styles.card}>
```

```css
.card {
    padding: 20px;
}
```

The CSS class is scoped through the module.

So we can have:

```text
ProductCard.module.css
    .card

UserCard.module.css
    .card
```

without the same global CSS class causing the same kind of collision.

---

# 11. Important Mental Model

Don't think:

> CSS Modules are a completely different CSS language.

They aren't.

You are still writing:

```css
.card {
    padding: 20px;
}
```

The difference is mainly **how the CSS is scoped and imported into your component**.

Think:

```text
CSS
 ↓
CSS Modules
 ↓
same CSS concepts
+
component-scoped classes
```

---

# 12. Our First CSS Modules Example

Project:

```text
src
│
├── App.tsx
│
└── components
    └── ProductCard
        ├── ProductCard.tsx
        └── ProductCard.module.css
```

### ProductCard.tsx

```tsx
import styles from "./ProductCard.module.css";

function ProductCard() {
    return (
        <div className={styles.card}>
            <h2 className={styles.title}>
                Laptop
            </h2>

            <p className={styles.price}>
                ₹50,000
            </p>

            <button className={styles.button}>
                Add to Cart
            </button>
        </div>
    );
}

export default ProductCard;
```

### ProductCard.module.css

```css
.card {
    padding: 20px;
    border: 1px solid black;
}

.title {
    font-size: 24px;
}

.price {
    font-size: 18px;
}

.button {
    padding: 10px 15px;
}
```

---

# 13. What Is Happening?

This:

```tsx
import styles from "./ProductCard.module.css";
```

imports the CSS Module.

Then:

```tsx
styles.card
```

refers to:

```css
.card
```

And:

```tsx
styles.title
```

refers to:

```css
.title
```

So:

```text
styles.card
     ↓
.card

styles.title
     ↓
.title

styles.button
     ↓
.button
```

---

# 14. One Thing to Notice

We don't write:

```tsx
className="styles.card"
```

That's wrong.

We write:

```tsx
className={styles.card}
```

Because `styles` is a JavaScript object generated from the CSS Module.

Conceptually:

```text
styles
│
├── card
├── title
├── price
└── button
```

So:

```tsx
styles.card
```

means:

> Give me the generated class name for `card`.

---

# 15. What You Need to Understand Today

Don't worry about advanced CSS Modules features yet.

For now, remember just this:

```text
Normal CSS

className="card"
```

versus:

```text
CSS Modules

className={styles.card}
```

And:

```text
Normal CSS
       ↓
global classes

CSS Modules
       ↓
component-scoped classes
```

---

# Phase 8 Progress

```text
## 8. Styling

[ ] CSS Modules
    [x] Why styling approaches exist
    [x] className
    [x] Inline styles
    [x] CSS Modules introduction

[ ] Styled Components
[ ] Tailwind
[ ] Responsive design
[ ] Accessibility
```

### Next → Step 2: CSS Modules in depth

We'll learn **multiple classes, conditional classes, nested component styling, and how CSS Modules actually prevent class-name conflicts**.

-------------------------------------------------------------------------------------------------------------------------------------------


# Step 2 — CSS Modules in Depth

We know the basic idea:

```text
Normal CSS
    ↓
global classes

CSS Modules
    ↓
component-scoped classes
```

Now let's understand how we actually use CSS Modules in a React project.

---

# 1. Basic Structure

We'll use:

```text
src
└── components
    └── ProductCard
        ├── ProductCard.tsx
        └── ProductCard.module.css
```

The important naming convention is:

```text
.module.css
```

For example:

```text
ProductCard.module.css
```

---

# 2. CSS File

`ProductCard.module.css`

```css
.card {
    padding: 20px;
    border: 1px solid black;
}

.title {
    font-size: 24px;
}

.price {
    font-size: 18px;
}

.button {
    padding: 10px;
}
```

Nothing unusual here.

It's still normal CSS.

---

# 3. Import the Module

Inside `ProductCard.tsx`:

```tsx
import styles from "./ProductCard.module.css";
```

Now `styles` represents the CSS classes from that module.

Conceptually:

```text
styles
│
├── card
├── title
├── price
└── button
```

---

# 4. Use the Classes

```tsx
function ProductCard() {
    return (
        <div className={styles.card}>
            <h2 className={styles.title}>
                Laptop
            </h2>

            <p className={styles.price}>
                ₹50,000
            </p>

            <button className={styles.button}>
                Add to Cart
            </button>
        </div>
    );
}
```

The important pattern is:

```tsx
className={styles.card}
```

Not:

```tsx
className="card"
```

---

# 5. Multiple Classes

Sometimes an element needs more than one class.

For example:

```css
.card {
    padding: 20px;
}

.highlight {
    border: 2px solid black;
}
```

We can do:

```tsx
<div className={`${styles.card} ${styles.highlight}`}>
```

So the element receives both:

```text
card
+
highlight
```

This is useful when you want to combine reusable styles.

---

# 6. Conditional Classes

This becomes especially useful in React.

Suppose our product can be:

```text
Available
Out of Stock
```

CSS:

```css
.card {
    padding: 20px;
}

.outOfStock {
    opacity: 0.5;
}
```

React:

```tsx
function ProductCard({ available }: { available: boolean }) {
    return (
        <div
            className={
                available
                    ? styles.card
                    : `${styles.card} ${styles.outOfStock}`
            }
        >
            <h2>Laptop</h2>
        </div>
    );
}
```

If:

```text
available = true
```

we get:

```text
card
```

If:

```text
available = false
```

we get:

```text
card + outOfStock
```

This is one of the places where React's JavaScript logic becomes useful for styling.

---

# 7. Another Simple Example

Imagine a button with two states:

```text
Normal
Disabled
```

CSS:

```css
.button {
    padding: 10px;
}

.disabled {
    opacity: 0.5;
}
```

React:

```tsx
<button
    className={
        disabled
            ? `${styles.button} ${styles.disabled}`
            : styles.button
    }
>
    Submit
</button>
```

So the styling changes based on the React state.

---

# 8. CSS Modules Don't Change CSS

This is important.

You still use:

```css
padding
margin
border
background
color
display
flex
grid
```

CSS Modules only change **how the styles are scoped and used**.

Think:

```text
CSS Modules
     ↓
Normal CSS
     +
Scoped class names
     +
Import into component
```

---

# 9. The Real Benefit: Name Conflicts

Suppose we have:

```text
ProductCard.module.css
```

with:

```css
.title {
    font-size: 24px;
}
```

And:

```text
UserCard.module.css
```

also has:

```css
.title {
    font-size: 18px;
}
```

That's okay.

Each component can use:

```tsx
styles.title
```

because the CSS is scoped to its module.

Conceptually:

```text
ProductCard
    styles.title
        ↓
ProductCard's title style


UserCard
    styles.title
        ↓
UserCard's title style
```

They don't behave like one global `.title` class.

---

# 10. Compare With Normal CSS

### Normal CSS

```text
ProductCard
    ↓
.title

UserCard
    ↓
.title
```

Both `.title` classes are global.

---

### CSS Modules

```text
ProductCard
    ↓
styles.title

UserCard
    ↓
styles.title
```

Each module owns its own class.

That's the main reason CSS Modules are useful in component-based applications.

---

# 11. CSS Module + Props

Now combine this with something you've already learned: **props**.

```tsx
type ProductCardProps = {
    name: string;
    price: number;
};
```

Then:

```tsx
import styles from "./ProductCard.module.css";

function ProductCard({
    name,
    price
}: ProductCardProps) {
    return (
        <div className={styles.card}>
            <h2 className={styles.title}>
                {name}
            </h2>

            <p className={styles.price}>
                ₹{price}
            </p>
        </div>
    );
}

export default ProductCard;
```

Now we have:

```text
Reusable Component
       +
CSS Module
       +
Props
```

This is exactly the kind of component thinking we want in React.

---

# 12. Parent and Child Components

Suppose:

```text
App
│
└── ProductCard
```

`ProductCard` owns:

```text
ProductCard.tsx
ProductCard.module.css
```

The parent doesn't need to know how the card is styled.

That's a good component boundary.

```text
App
 ↓
ProductCard
 ↓
its own styles
```

The component is responsible for itself.

---

# 13. One Important Rule

Don't create one giant CSS Module for the entire application.

Avoid:

```text
src
├── App.tsx
└── styles.module.css
```

with hundreds of classes.

Instead, when appropriate, keep styles close to the component:

```text
components
│
├── Navbar
│   ├── Navbar.tsx
│   └── Navbar.module.css
│
├── ProductCard
│   ├── ProductCard.tsx
│   └── ProductCard.module.css
│
└── Button
    ├── Button.tsx
    └── Button.module.css
```

This makes the code easier to maintain.

---

# 14. Mental Model

Think about a component like a small box:

```text
┌──────────────────────────┐
│ ProductCard              │
│                          │
│ ProductCard.tsx          │
│ ProductCard.module.css   │
│                          │
│ HTML + styling           │
└──────────────────────────┘
```

Another component:

```text
┌──────────────────────────┐
│ UserCard                 │
│                          │
│ UserCard.tsx             │
│ UserCard.module.css      │
│                          │
│ HTML + styling           │
└──────────────────────────┘
```

Each component has its own styling boundary.

---

# 15. What You Should Remember

The most important syntax:

```tsx
import styles from "./Component.module.css";
```

Then:

```tsx
className={styles.card}
```

Multiple classes:

```tsx
className={`${styles.card} ${styles.active}`}
```

Conditional class:

```tsx
className={
    active
        ? styles.active
        : styles.inactive
}
```

And the big idea:

```text
.module.css
     ↓
CSS Module
     ↓
Component-scoped styles
```

---

# Phase 8 Progress

```text
## 8. Styling

[x] Step 1 — Styling in React + CSS Modules introduction

[x] Step 2 — CSS Modules in depth
    [x] Importing modules
    [x] Using styles
    [x] Multiple classes
    [x] Conditional classes
    [x] Props + CSS Modules
    [x] Component-level styling

[ ] Step 3 — Conditional / Dynamic Styling
[ ] Step 4 — Styled Components
[ ] Step 5 — Styled Components in depth
[ ] Step 6 — Tailwind CSS basics
[ ] Step 7 — Tailwind in React
[ ] Step 8 — Responsive Design
[ ] Step 9 — Accessibility
[ ] Step 10 — Final Styling Project
```

**Next → Step 3: Conditional & Dynamic Styling.** We'll use `useState`, props, and conditions to make the UI styles change based on application state.

------------------------------------------------------------------------------------------------------------------------------------------


# Step 3 — Conditional & Dynamic Styling

Now we're going to connect something you already learned:

```text
React state
    +
JavaScript conditions
    +
CSS
```

This is important because real React applications rarely have completely static styling.

For example:

```text
Button
├── Normal
├── Active
├── Disabled
└── Loading
```

The styling can change depending on the state.

---

# 1. What Is Conditional Styling?

Conditional styling means:

> Apply a different CSS class depending on a condition.

Simple example:

```tsx
className={active ? styles.active : styles.inactive}
```

Meaning:

```text
active === true
    ↓
styles.active

active === false
    ↓
styles.inactive
```

---

# 2. Simple Example

Let's create a button that can be active or inactive.

### Button.tsx

```tsx
import { useState } from "react";
import styles from "./Button.module.css";

function Button() {
    const [active, setActive] = useState(false);

    return (
        <button
            className={
                active
                    ? styles.active
                    : styles.inactive
            }
            onClick={() => setActive(!active)}
        >
            {active ? "Active" : "Inactive"}
        </button>
    );
}

export default Button;
```

### Button.module.css

```css
.active {
    background: green;
    color: white;
}

.inactive {
    background: gray;
    color: white;
}
```

---

# 3. Understand the Flow

Initially:

```text
active = false
```

So:

```tsx
className={styles.inactive}
```

The button looks inactive.

When we click:

```tsx
setActive(!active)
```

State changes:

```text
false
 ↓
true
```

React renders again.

Now:

```tsx
className={styles.active}
```

The button gets the active styling.

---

# 4. The Important Pattern

This is the pattern you should remember:

```tsx
condition
    ? classA
    : classB
```

For styling:

```tsx
className={
    condition
        ? styles.active
        : styles.inactive
}
```

This is called the **ternary operator**.

You already learned ternary in JavaScript, so now we're applying it to React styling.

---

# 5. Three States

Real applications often have more than two states.

For example:

```text
Button
├── normal
├── active
└── disabled
```

We can use:

```tsx
className={
    disabled
        ? styles.disabled
        : active
            ? styles.active
            : styles.normal
}
```

It works, but nested ternaries can become difficult to read.

So we can use a cleaner approach.

---

# 6. Build the Class Name First

For example:

```tsx
let buttonClass = styles.normal;

if (disabled) {
    buttonClass = styles.disabled;
} else if (active) {
    buttonClass = styles.active;
}
```

Then:

```tsx
<button className={buttonClass}>
    Submit
</button>
```

This is often easier to understand.

---

# 7. Multiple Classes Dynamically

Sometimes we don't want to choose **one** class.

We want to keep the base class and add another class.

For example:

```text
card
+
active
```

CSS:

```css
.card {
    padding: 20px;
    border: 1px solid black;
}

.active {
    border: 2px solid green;
}
```

React:

```tsx
<div
    className={
        active
            ? `${styles.card} ${styles.active}`
            : styles.card
    }
>
    Product
</div>
```

When active:

```text
card + active
```

When inactive:

```text
card
```

---

# 8. Why Do We Keep the Base Class?

Suppose:

```css
.card {
    padding: 20px;
    border: 1px solid black;
}
```

and:

```css
.active {
    border: 2px solid green;
}
```

If we only apply:

```tsx
styles.active
```

we lose:

```text
padding
border
```

from `.card`.

So we want:

```text
Always:
card

Sometimes:
active
```

That's why:

```tsx
`${styles.card} ${styles.active}`
```

is useful.

---

# 9. Dynamic Styling With Props

Now let's connect this with **props**.

Suppose our component receives:

```tsx
type ButtonProps = {
    primary: boolean;
};
```

Then:

```tsx
function Button({ primary }: ButtonProps) {
    return (
        <button
            className={
                primary
                    ? styles.primary
                    : styles.secondary
            }
        >
            Click Me
        </button>
    );
}
```

Parent:

```tsx
<Button primary={true} />
```

Result:

```text
primary style
```

Another:

```tsx
<Button primary={false} />
```

Result:

```text
secondary style
```

So:

```text
Props
 ↓
Condition
 ↓
CSS class
 ↓
Different appearance
```

---

# 10. Dynamic Styling Is Everywhere

You will see this pattern in real applications.

### Navigation

```text
Home
About
Products
```

Current page:

```text
Home → active
About
Products
```

### Product

```text
Available → normal
Out of stock → disabled
```

### Form

```text
Valid → normal
Invalid → error
```

### Button

```text
Normal
Hover
Active
Disabled
Loading
```

### Dashboard

```text
Success → success style
Warning → warning style
Error → error style
```

---

# 11. State + Styling

You already learned:

```text
useState
```

Now combine it with styling.

```tsx
const [darkMode, setDarkMode] = useState(false);
```

Then:

```tsx
<div
    className={
        darkMode
            ? styles.dark
            : styles.light
    }
>
```

So:

```text
State
 ↓
Condition
 ↓
CSS class
 ↓
UI changes
```

This is a very important React pattern.

---

# 12. CSS `:hover` Is Different

One important distinction.

You don't need React state for simple hover effects.

CSS can handle:

```css
.button:hover {
    transform: scale(1.05);
}
```

You don't need:

```tsx
useState()
```

for this.

So:

```text
Mouse hover
    ↓
CSS :hover
```

is usually better than:

```text
Mouse hover
    ↓
React state
    ↓
className
```

Use React state when the UI state actually matters to the application.

---

# 13. Good vs Bad Usage

### Good

```text
Button is selected
        ↓
React state
        ↓
active class
```

Because selected state is application/UI state.

### Unnecessary

```text
Mouse is hovering
        ↓
React state
        ↓
hover class
```

For a simple hover effect, CSS already provides:

```css
:hover
```

---

# 14. Another Example — Selected Product

Imagine:

```text
Products

[ Laptop ]
[ Phone ]
[ Tablet ]
```

User selects Laptop.

We want:

```text
Laptop → selected style
Phone
Tablet
```

React:

```tsx
const [selected, setSelected] = useState("laptop");
```

Then:

```tsx
<div
    className={
        selected === "laptop"
            ? `${styles.card} ${styles.selected}`
            : styles.card
    }
    onClick={() => setSelected("laptop")}
>
    Laptop
</div>
```

The state determines the styling.

---

# 15. The Big Picture

This is the pattern you should remember:

```text
                 React
                   │
                   ↓
                 State
                   │
                   ↓
               Condition
                   │
                   ↓
               className
                   │
                   ↓
                  CSS
                   │
                   ↓
             Visual change
```

For example:

```text
active = true
     ↓
styles.active
     ↓
green button
```

---

# 16. What We Learned

```text
[x] Conditional classes
[x] Ternary + className
[x] Multiple classes
[x] Dynamic classes
[x] State + styling
[x] Props + styling
[x] CSS :hover vs React state
```

The important syntax:

```tsx
className={
    active
        ? styles.active
        : styles.inactive
}
```

And:

```tsx
className={
    active
        ? `${styles.card} ${styles.active}`
        : styles.card
}
```

---

# Phase 8 Progress

```text
## 8. Styling

[x] Step 1 — Styling in React + CSS Modules
[x] Step 2 — CSS Modules in depth
[x] Step 3 — Conditional & Dynamic Styling

[ ] Step 4 — Styled Components
[ ] Step 5 — Styled Components in depth
[ ] Step 6 — Tailwind CSS basics
[ ] Step 7 — Tailwind in React
[ ] Step 8 — Responsive Design
[ ] Step 9 — Accessibility
[ ] Step 10 — Final Styling Project
```

### Next → Step 4: Styled Components

We'll learn **what Styled Components are, why they exist, how they differ from CSS Modules, and how to style a React component with them**.

------------------------------------------------------------------------------------------------------------------------------------------



# Step 4 — Styled Components

Now we move to the **second styling approach** in your roadmap.

So far:

```text
CSS
 ↓
CSS Modules
 ↓
Component-scoped CSS
```

Now:

```text
Styled Components
 ↓
CSS written inside JavaScript/TypeScript
 ↓
Styled React components
```

---

# 1. What Are Styled Components?

Styled Components is a library that lets you create React components that already have their styles attached.

Instead of:

```text
Button.tsx
Button.module.css
```

you can define the styling directly in your component file.

Think:

```text
Button
  ↓
styling + component
```

---

# 2. Install It

For a React + TypeScript project:

```text
npm install styled-components
npm install -D @types/styled-components
```

The first package provides Styled Components.

The second provides TypeScript support.

---

# 3. Basic Example

Let's create a button.

```tsx
import styled from "styled-components";

const Button = styled.button`
    padding: 10px 20px;
    background: blue;
    color: white;
`;

function App() {
    return (
        <Button>
            Click Me
        </Button>
    );
}

export default App;
```

Look at this:

```tsx
const Button = styled.button`
    ...
`;
```

We're creating a **React component** called `Button`.

---

# 4. Compare With CSS Modules

### CSS Modules

You have two files:

```text
Button.tsx
Button.module.css
```

`Button.module.css`:

```css
.button {
    padding: 10px 20px;
    background: blue;
    color: white;
}
```

Then:

```tsx
<button className={styles.button}>
    Click Me
</button>
```

---

### Styled Components

Everything can be inside:

```text
Button.tsx
```

```tsx
const Button = styled.button`
    padding: 10px 20px;
    background: blue;
    color: white;
`;
```

Then:

```tsx
<Button>Click Me</Button>
```

So the basic difference is:

```text
CSS Modules
Component
   +
CSS file

Styled Components
Component
   +
styles
in same file
```

---

# 5. Why Is It Called "Styled Components"?

Because:

```tsx
const Button = styled.button`
    ...
`;
```

creates a **styled React component**.

You aren't doing:

```tsx
<button className="button">
```

Instead:

```tsx
<Button>
```

The component itself contains the styling.

---

# 6. Different HTML Elements

You can create:

### Styled button

```tsx
const Button = styled.button`
    padding: 10px;
`;
```

### Styled div

```tsx
const Card = styled.div`
    padding: 20px;
`;
```

### Styled heading

```tsx
const Title = styled.h1`
    font-size: 24px;
`;
```

### Styled paragraph

```tsx
const Description = styled.p`
    color: gray;
`;
```

So:

```text
styled.button → button
styled.div    → div
styled.h1     → h1
styled.p      → p
```

---

# 7. A Full Example

```tsx
import styled from "styled-components";

const Card = styled.div`
    padding: 20px;
    border: 1px solid black;
`;

const Title = styled.h2`
    font-size: 24px;
`;

const Price = styled.p`
    font-size: 18px;
`;

const Button = styled.button`
    padding: 10px 15px;
`;

function ProductCard() {
    return (
        <Card>
            <Title>Laptop</Title>

            <Price>
                ₹50,000
            </Price>

            <Button>
                Add to Cart
            </Button>
        </Card>
    );
}

export default ProductCard;
```

Notice:

```text
Card
Title
Price
Button
```

are all React components.

---

# 8. Styled Components + Props

This is where Styled Components becomes very useful.

Suppose we want:

```text
primary button
secondary button
```

We can use props.

```tsx
const Button = styled.button<{ primary: boolean }>`
    padding: 10px 20px;

    background: ${(props) =>
        props.primary ? "blue" : "gray"};

    color: white;
`;
```

Then:

```tsx
<Button primary={true}>
    Save
</Button>
```

and:

```tsx
<Button primary={false}>
    Cancel
</Button>
```

Now the styling changes based on the prop.

---

# 9. Understand the Flow

We have:

```text
<Button primary={true} />
```

The prop is:

```text
primary = true
```

Styled Components checks:

```text
primary?
   ↓
true
   ↓
blue
```

For:

```text
<Button primary={false} />
```

we get:

```text
primary?
   ↓
false
   ↓
gray
```

So:

```text
Props
 ↓
Condition
 ↓
Style
```

This is similar to the conditional styling we just learned.

---

# 10. Styled Components + State

We can also combine it with `useState`.

```tsx
import { useState } from "react";
import styled from "styled-components";

const Button = styled.button<{ active: boolean }>`
    background: ${(props) =>
        props.active ? "green" : "gray"};

    color: white;
`;

function App() {
    const [active, setActive] = useState(false);

    return (
        <Button
            active={active}
            onClick={() => setActive(!active)}
        >
            {active ? "Active" : "Inactive"}
        </Button>
    );
}
```

The flow:

```text
useState
   ↓
active
   ↓
prop
   ↓
styled component
   ↓
different CSS
```

---

# 11. CSS Modules vs Styled Components

This is important for interviews and real projects.

| CSS Modules               | Styled Components                 |
| ------------------------- | --------------------------------- |
| CSS in separate file      | CSS usually inside TSX            |
| `className={styles.card}` | `<Card>`                          |
| Component-scoped styles   | Component-scoped styles           |
| Uses normal CSS           | Uses CSS-in-JS                    |
| Simple and lightweight    | More dynamic styling capabilities |

Neither is automatically "better."

The choice depends on the project and team's conventions.

---

# 12. What Does "CSS-in-JS" Mean?

You may hear this term.

It basically means:

> Writing CSS-related styling inside JavaScript/TypeScript.

For example:

```tsx
const Button = styled.button`
    padding: 10px;
`;
```

The CSS is living inside the TSX file.

So:

```text
CSS Modules

TSX ─────────→ Component
CSS  ─────────→ Styling
```

Whereas:

```text
Styled Components

TSX
 │
 ├── Component
 │
 └── Styling
```

---

# 13. One Important Thing

Styled Components doesn't mean you no longer need to know CSS.

You still write:

```css
padding
margin
display
flex
grid
border
background
color
font-size
```

You're simply writing those CSS rules through the Styled Components system.

So your CSS knowledge remains extremely important.

---

# 14. When Would You Use Styled Components?

It can be useful when you have components whose styles depend heavily on props.

For example:

```text
Button
 ├── primary
 ├── secondary
 ├── danger
 └── disabled
```

Or:

```text
Alert
 ├── success
 ├── warning
 └── error
```

The styling can react directly to component props.

---

# 15. Don't Confuse Styled Components With Inline Styles

These are different.

Inline style:

```tsx
<button
    style={{
        padding: "10px"
    }}
>
    Click
</button>
```

Styled Components:

```tsx
const Button = styled.button`
    padding: 10px;
`;
```

Styled Components gives you a reusable styled component.

So:

```text
Inline style
→ style this particular element

Styled Component
→ create a reusable styled component
```

---

# 16. Mental Model

Think of Styled Components like this:

```text
              styled.button
                    ↓
             Create component
                    ↓
                 Button
                    ↓
             <Button />
```

The CSS travels with the component.

---

# Phase 8 Progress

```text
## 8. Styling

[x] Step 1 — Styling in React + CSS Modules
[x] Step 2 — CSS Modules in depth
[x] Step 3 — Conditional & Dynamic Styling
[x] Step 4 — Styled Components

[ ] Step 5 — Styled Components in depth
[ ] Step 6 — Tailwind CSS basics
[ ] Step 7 — Tailwind in React
[ ] Step 8 — Responsive Design
[ ] Step 9 — Accessibility
[ ] Step 10 — Final Styling Project
```

### Next → Step 5: Styled Components in Depth

We'll cover **reusable styled components, extending styles, props, themes, and when to use CSS Modules vs Styled Components**.

------------------------------------------------------------------------------------------------------------------------------------------



# Step 5 — Styled Components in Depth

Now we'll go a little deeper into **Styled Components**.

The basic idea was:

```text
styled.button
      ↓
creates a styled React component
```

Now let's understand how to make those components **reusable and dynamic**.

---

# 1. Reusable Styled Components

Imagine we have three buttons:

```text
Save
Cancel
Delete
```

We don't want to create completely separate styling for every button.

Instead, create one reusable `Button`:

```tsx
const Button = styled.button`
    padding: 10px 20px;
    border: none;
`;
```

Then:

```tsx
<Button>Save</Button>
<Button>Cancel</Button>
<Button>Delete</Button>
```

One component, many uses.

Think:

```text
Button
  ↓
Reusable styling
  ↓
Save
Cancel
Delete
```

---

# 2. Props Make It Flexible

Now suppose we want different button types.

```text
Save   → primary
Cancel → secondary
Delete → danger
```

We can use a prop.

```tsx
type ButtonProps = {
    variant: "primary" | "secondary" | "danger";
};

const Button = styled.button<ButtonProps>`
    padding: 10px 20px;

    background: ${(props) => {
        if (props.variant === "primary") {
            return "blue";
        }

        if (props.variant === "danger") {
            return "red";
        }

        return "gray";
    }};
`;
```

Then:

```tsx
<Button variant="primary">
    Save
</Button>

<Button variant="secondary">
    Cancel
</Button>

<Button variant="danger">
    Delete
</Button>
```

Now one component supports three styles.

---

# 3. Why Use a Union Type?

Notice:

```tsx
variant: "primary" | "secondary" | "danger";
```

This means only these values are allowed:

```text
primary
secondary
danger
```

So:

```tsx
<Button variant="primary" />
```

is valid.

But:

```tsx
<Button variant="something" />
```

will give a TypeScript error.

This is a nice combination of:

```text
TypeScript
+
Styled Components
```

---

# 4. Base Styles + Variant Styles

Usually we have common styles:

```tsx
const Button = styled.button<ButtonProps>`
    padding: 10px 20px;
    border: none;
    border-radius: 6px;
    cursor: pointer;
`;
```

Then the variant changes only what's different.

Conceptually:

```text
Button
│
├── padding
├── border
├── radius
├── cursor
│
└── variant
     ├── primary
     ├── secondary
     └── danger
```

This is a good component design pattern.

---

# 5. Extending a Styled Component

Suppose we already have:

```tsx
const Button = styled.button`
    padding: 10px 20px;
`;
```

Now we want another button with the same styles but some additional styles.

We can extend it:

```tsx
const DangerButton = styled(Button)`
    background: red;
    color: white;
`;
```

Now:

```text
Button
  ↓
DangerButton
  ↓
inherits Button styles
+
adds danger styles
```

Use it:

```tsx
<DangerButton>
    Delete
</DangerButton>
```

---

# 6. Why Is This Useful?

Imagine:

```text
BaseButton
│
├── PrimaryButton
├── SecondaryButton
└── DangerButton
```

The common styling can live in:

```text
BaseButton
```

while specialized styling can be added to the other components.

This is another form of **component reuse**.

---

# 7. Styled Component as a Normal React Component

This is important.

Once you create:

```tsx
const Button = styled.button`
    padding: 10px;
`;
```

you use it like a normal React component:

```tsx
<Button>
    Save
</Button>
```

You can also pass normal HTML button props:

```tsx
<Button type="submit">
    Save
</Button>
```

or:

```tsx
<Button disabled>
    Save
</Button>
```

So:

```text
Styled Component
       ↓
Still behaves like the underlying HTML element
```

---

# 8. Component Composition

You can also build larger components from smaller styled components.

For example:

```tsx
const Card = styled.div`
    padding: 20px;
`;

const Title = styled.h2`
    font-size: 24px;
`;

const Button = styled.button`
    padding: 10px;
`;
```

Then:

```tsx
function ProductCard() {
    return (
        <Card>
            <Title>Laptop</Title>

            <p>₹50,000</p>

            <Button>
                Add to Cart
            </Button>
        </Card>
    );
}
```

Think:

```text
ProductCard
│
├── Card
├── Title
└── Button
```

This is **composition**, which connects directly to the Component Thinking phase you learned earlier.

---

# 9. Styled Components + `className`

A styled component can also receive `className`.

For example:

```tsx
const Button = styled.button`
    padding: 10px;
`;
```

You can still do:

```tsx
<Button className="extra-style">
    Save
</Button>
```

But generally, if you're using Styled Components, you want most of your styling to stay within the Styled Components system rather than mixing many global classes.

---

# 10. Themes

Styled Components can also support **themes**.

Imagine your application has:

```text
Light theme
Dark theme
```

Instead of manually passing colors everywhere, a theme can provide common design values.

Conceptually:

```text
Theme
│
├── colors
│   ├── background
│   ├── text
│   └── primary
│
├── spacing
│
└── fonts
```

Then components can use those shared values.

This becomes especially useful in larger applications.

We don't need to build a full theme system yet. Just understand the idea.

---

# 11. Why Themes Are Useful

Without a theme:

```text
Button → blue
Navbar → blue
Link → blue
Card → white
Page → white
```

Those values may be repeated throughout the application.

With a theme:

```text
Theme
 ↓
primary = blue
background = white
text = black
```

Components can use the same design values.

If you later change:

```text
primary = purple
```

multiple components can automatically follow the new theme.

---

# 12. CSS Modules vs Styled Components

Now you have enough knowledge to compare them properly.

### CSS Modules

```text
Component
    +
.module.css
```

Example:

```tsx
<div className={styles.card}>
```

Good when you want:

* Familiar CSS
* Separate CSS files
* Component-scoped styles
* A relatively simple styling approach

---

### Styled Components

```text
Component
    +
styled component
```

Example:

```tsx
<Card>
```

Good when you want:

* Styles colocated with components
* Dynamic styles based on props
* Reusable styled components
* Theme support

---

# 13. Don't Think You Must Use Both

In a real project, you usually don't randomly do:

```text
Component A → CSS Modules
Component B → Styled Components
Component C → random inline styles
Component D → another styling system
```

That can become messy.

Usually a project/team chooses a primary styling approach.

For our learning roadmap, we're learning multiple approaches so **you understand how they work and can recognize them in real projects**.

---

# 14. Important Modern Note

Styled Components is useful to know, but you don't need to memorize every API.

The important concepts are:

```text
styled.button
styled.div
styled.h1
      ↓
styled components

props
      ↓
dynamic styles

styled(Component)
      ↓
extend styles

theme
      ↓
shared design values
```

These concepts matter more than memorizing syntax.

---

# 15. Full Example

Here's everything together:

```tsx
import styled from "styled-components";

type ButtonProps = {
    variant: "primary" | "secondary" | "danger";
};

const Card = styled.div`
    padding: 20px;
    border: 1px solid black;
`;

const Title = styled.h2`
    font-size: 24px;
`;

const Button = styled.button<ButtonProps>`
    padding: 10px 20px;
    border: none;

    background: ${(props) => {
        if (props.variant === "primary") {
            return "blue";
        }

        if (props.variant === "danger") {
            return "red";
        }

        return "gray";
    }};

    color: white;
`;

function ProductCard() {
    return (
        <Card>
            <Title>Laptop</Title>

            <p>₹50,000</p>

            <Button variant="primary">
                Add to Cart
            </Button>

            <Button variant="danger">
                Delete
            </Button>
        </Card>
    );
}

export default ProductCard;
```

Notice the architecture:

```text
ProductCard
│
├── Card
│
├── Title
│
└── Button
      │
      └── variant prop
             │
             ├── primary
             └── danger
```

That's a much more realistic use of Styled Components.

---

# 16. What We Learned

```text
[x] Reusable styled components
[x] Props + dynamic styling
[x] TypeScript props
[x] Union types for variants
[x] Extending styled components
[x] Component composition
[x] Themes concept
[x] CSS Modules vs Styled Components
```

The most important mental model:

```text
Reusable Component
        +
      Props
        ↓
Dynamic styling
```

---

# Phase 8 Progress

```text
## 8. Styling

[x] Step 1 — React styling + CSS Modules
[x] Step 2 — CSS Modules in depth
[x] Step 3 — Conditional & Dynamic Styling
[x] Step 4 — Styled Components
[x] Step 5 — Styled Components in depth

[ ] Step 6 — Tailwind CSS basics
[ ] Step 7 — Tailwind in React
[ ] Step 8 — Responsive Design
[ ] Step 9 — Accessibility
[ ] Step 10 — Final Styling Project
```

**Next → Step 6: Tailwind CSS Basics**

We'll first understand **what Tailwind is and why it uses utility classes**, then build a small UI with it.

------------------------------------------------------------------------------------------------------------------------------------------



# Step 6 — Tailwind CSS Basics

Now we move to the **third styling approach** in your roadmap:

```text
CSS Modules
      ↓
Styled Components
      ↓
Tailwind CSS
```

The important thing is to understand **why Tailwind looks so different**.

---

# 1. What Is Tailwind CSS?

Tailwind CSS is a **utility-first CSS framework**.

Instead of creating your own CSS class like:

```css
.card {
    padding: 20px;
    border: 1px solid black;
}
```

you use small utility classes directly in your JSX:

```tsx
<div className="p-5 border">
    Product
</div>
```

Each class represents a small styling rule.

Think:

```text
Tailwind class
      ↓
Small CSS utility
      ↓
Combine utilities
      ↓
Create your design
```

---

# 2. Example

Normal CSS:

```css
.card {
    padding: 20px;
    background: white;
    border: 1px solid black;
}
```

React:

```tsx
<div className="card">
    Product
</div>
```

With Tailwind:

```tsx
<div className="p-5 bg-white border">
    Product
</div>
```

We don't create `.card`.

Instead, we combine Tailwind utilities.

---

# 3. What Does `p-5` Mean?

For example:

```text
p-5
```

means:

```text
p → padding
5 → spacing value
```

So:

```tsx
<div className="p-5">
```

means:

> Add padding.

You will see similar patterns:

```text
p-4   → padding
px-4  → horizontal padding
py-4  → vertical padding

m-4   → margin
mx-4  → horizontal margin
my-4  → vertical margin
```

You don't need to memorize all of them now.

The pattern is what matters.

---

# 4. Text Styling

Tailwind provides utilities for text.

For example:

```tsx
<h1 className="text-2xl">
    Hello
</h1>
```

`text-2xl` controls the text size.

Another:

```tsx
<p className="text-gray-500">
    Description
</p>
```

This controls the text color.

And:

```tsx
<h1 className="font-bold">
    Hello
</h1>
```

makes the text bold.

So:

```text
text-2xl
    ↓
font size

text-gray-500
    ↓
text color

font-bold
    ↓
font weight
```

---

# 5. Combining Classes

This is the core idea of Tailwind.

```tsx
<div className="p-5 bg-white border rounded">
    Product
</div>
```

We have:

```text
p-5
 ↓
padding

bg-white
 ↓
background

border
 ↓
border

rounded
 ↓
border radius
```

Together:

```text
Small utilities
      ↓
Combined
      ↓
Complete design
```

---

# 6. Flexbox

You already learned Flexbox in CSS.

With normal CSS:

```css
.container {
    display: flex;
    justify-content: center;
    align-items: center;
}
```

With Tailwind:

```tsx
<div className="flex justify-center items-center">
```

So:

```text
flex
 ↓
display: flex

justify-center
 ↓
justify-content: center

items-center
 ↓
align-items: center
```

Your existing CSS knowledge makes Tailwind much easier.

---

# 7. Flex Direction

Normal CSS:

```css
.container {
    display: flex;
    flex-direction: column;
}
```

Tailwind:

```tsx
<div className="flex flex-col">
```

So:

```text
flex
+
flex-col
```

means:

```text
display: flex
+
flex-direction: column
```

---

# 8. Gap

Normal CSS:

```css
.container {
    display: flex;
    gap: 20px;
}
```

Tailwind:

```tsx
<div className="flex gap-5">
```

So:

```text
flex
+
gap-5
```

---

# 9. A Complete Card

Let's combine what we've learned.

```tsx
function ProductCard() {
    return (
        <div className="p-5 border rounded">
            <h2 className="text-2xl font-bold">
                Laptop
            </h2>

            <p className="text-gray-500">
                Powerful laptop for development.
            </p>

            <p className="text-xl font-bold">
                ₹50,000
            </p>

            <button className="px-4 py-2 bg-blue-500 text-white rounded">
                Add to Cart
            </button>
        </div>
    );
}
```

Notice that we didn't create:

```text
ProductCard.css
```

The styling is directly in:

```tsx
className="..."
```

---

# 10. Tailwind vs CSS Modules

Let's compare.

### CSS Modules

```tsx
<div className={styles.card}>
```

CSS:

```css
.card {
    padding: 20px;
    border: 1px solid black;
}
```

---

### Tailwind

```tsx
<div className="p-5 border">
```

No custom `.card` class is required.

So:

```text
CSS Modules
     ↓
Create CSS
     ↓
Use class

Tailwind
     ↓
Use existing utilities
     ↓
Combine classes
```

---

# 11. Tailwind vs Styled Components

Styled Components:

```tsx
const Card = styled.div`
    padding: 20px;
    border: 1px solid black;
`;
```

Then:

```tsx
<Card>
```

Tailwind:

```tsx
<div className="p-5 border">
```

So:

```text
Styled Components
    ↓
Create styled component

Tailwind
    ↓
Use utility classes
```

---

# 12. Hover Styling

Tailwind also supports states.

For example:

```tsx
<button className="bg-blue-500 hover:bg-blue-700">
    Save
</button>
```

The important part:

```text
hover:bg-blue-700
```

means:

> When the user hovers over this element, use this style.

So:

```text
Normal
 ↓
bg-blue-500

Hover
 ↓
bg-blue-700
```

This connects directly to the conditional styling topic we learned earlier.

---

# 13. Responsive Styling

Tailwind also makes responsive design convenient.

For example:

```tsx
<div className="text-sm md:text-lg">
    Hello
</div>
```

Conceptually:

```text
Small screen
    ↓
text-sm

Medium screen and above
    ↓
text-lg
```

We'll study responsive design properly later in **Step 8**.

For now, just understand that Tailwind has responsive prefixes.

---

# 14. Don't Try to Memorize Everything

Tailwind has a lot of utility classes.

You don't need to memorize hundreds of them.

You should understand the patterns:

```text
p-4
m-4
flex
grid
gap-4
text-xl
font-bold
bg-*
text-*
rounded
border
```

Once you understand the underlying CSS, Tailwind becomes much easier.

---

# 15. The Important Mental Model

Think about Tailwind like this:

```text
              Tailwind
                  ↓
         Utility classes
                  ↓
       ┌──────────┼──────────┐
       ↓          ↓          ↓
    spacing     colors      layout
       ↓          ↓          ↓
     p-4       bg-blue     flex
     m-4       text-white  grid
     gap-4     border      items-center
```

You combine those small utilities to build the UI.

---

# 16. One Full Example

```tsx
function ProductCard() {
    return (
        <div className="flex flex-col gap-4 p-6 border rounded">
            <h2 className="text-2xl font-bold">
                Laptop
            </h2>

            <p className="text-gray-500">
                Development laptop
            </p>

            <p className="text-xl font-bold">
                ₹50,000
            </p>

            <button className="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-700">
                Add to Cart
            </button>
        </div>
    );
}

export default ProductCard;
```

Break it down:

```text
flex
 ↓
Flexbox

flex-col
 ↓
Column layout

gap-4
 ↓
Space between children

p-6
 ↓
Padding

border
 ↓
Border

rounded
 ↓
Rounded corners

text-2xl
 ↓
Large text

font-bold
 ↓
Bold

bg-blue-500
 ↓
Background

text-white
 ↓
Text color

hover:bg-blue-700
 ↓
Hover background
```

---

# 17. What We Learned

```text
[x] What Tailwind CSS is
[x] Utility-first concept
[x] Spacing utilities
[x] Text utilities
[x] Color utilities
[x] Flexbox utilities
[x] Gap
[x] Hover utilities
[x] Basic responsive prefix
[x] Tailwind vs CSS Modules
[x] Tailwind vs Styled Components
```

The biggest thing to remember:

> **Tailwind doesn't replace CSS knowledge. It gives you ready-made utility classes for common CSS rules.**

---

# Phase 8 Progress

```text
## 8. Styling

[x] Step 1 — React styling + CSS Modules
[x] Step 2 — CSS Modules in depth
[x] Step 3 — Conditional & Dynamic Styling
[x] Step 4 — Styled Components
[x] Step 5 — Styled Components in depth
[x] Step 6 — Tailwind CSS basics

[ ] Step 7 — Tailwind in React
[ ] Step 8 — Responsive Design
[ ] Step 9 — Accessibility
[ ] Step 10 — Final Styling Project
```

**Next → Step 7: Tailwind in React**

We'll build reusable React components with Tailwind and learn how to handle **props, conditional classes, reusable buttons/cards, and component styling**.

> hover means when the mouse pointer is placed over an element.

------------------------------------------------------------------------------------------------------------------------------------------


# Step 7 — Tailwind in React

Now we combine **Tailwind + React concepts** you've already learned:

```text
React
 ├── Components
 ├── Props
 ├── State
 └── Conditional rendering
        +
Tailwind
 └── Utility classes
```

The goal is not just to put Tailwind classes everywhere. We want to learn how to build **reusable React components with Tailwind**.

---

# 1. Tailwind + Components

Let's start with a simple button.

```tsx
function Button() {
    return (
        <button className="px-4 py-2 bg-blue-500 text-white rounded">
            Click Me
        </button>
    );
}
```

Now we can use:

```tsx
<Button />
```

The important idea is:

```text
Button component
       ↓
Tailwind classes
       ↓
Reusable UI
```

---

# 2. Make the Button Reusable With Props

A real application might have:

```text
Save
Cancel
Delete
Edit
```

We don't want four completely different components.

Instead:

```tsx
type ButtonProps = {
    children: React.ReactNode;
};
```

Then:

```tsx
function Button({ children }: ButtonProps) {
    return (
        <button className="px-4 py-2 bg-blue-500 text-white rounded">
            {children}
        </button>
    );
}
```

Now:

```tsx
<Button>Save</Button>

<Button>Cancel</Button>

<Button>Edit</Button>
```

The component is reusable.

---

# 3. Add a Variant

Now let's make the button support different types.

```text
primary
secondary
danger
```

TypeScript:

```tsx
type ButtonProps = {
    children: React.ReactNode;
    variant: "primary" | "secondary" | "danger";
};
```

Then:

```tsx
function Button({ children, variant }: ButtonProps) {
    let classes = "px-4 py-2 rounded text-white";

    if (variant === "primary") {
        classes += " bg-blue-500";
    }

    if (variant === "secondary") {
        classes += " bg-gray-500";
    }

    if (variant === "danger") {
        classes += " bg-red-500";
    }

    return (
        <button className={classes}>
            {children}
        </button>
    );
}
```

Now:

```tsx
<Button variant="primary">
    Save
</Button>

<Button variant="secondary">
    Cancel
</Button>

<Button variant="danger">
    Delete
</Button>
```

---

# 4. Understand What's Happening

This is the same concept we learned with CSS Modules and Styled Components.

```text
Prop
 ↓
variant
 ↓
condition
 ↓
Tailwind classes
 ↓
different appearance
```

For example:

```text
variant="danger"
       ↓
bg-red-500
       ↓
red button
```

---

# 5. Why `children`?

This:

```tsx
<Button>
    Save
</Button>
```

means the text `Save` is passed to the component through:

```tsx
children
```

So:

```tsx
function Button({ children }: ButtonProps)
```

receives:

```text
children = "Save"
```

Then:

```tsx
<button>
    {children}
</button>
```

renders:

```text
Save
```

This is a very important React concept.

---

# 6. Reusable Card

Let's create a reusable card.

```tsx
type CardProps = {
    title: string;
    description: string;
};
```

Component:

```tsx
function Card({ title, description }: CardProps) {
    return (
        <div className="p-5 border rounded">
            <h2 className="text-xl font-bold">
                {title}
            </h2>

            <p className="text-gray-500">
                {description}
            </p>
        </div>
    );
}
```

Use it:

```tsx
<Card
    title="Laptop"
    description="Development laptop"
/>

<Card
    title="Phone"
    description="Android smartphone"
/>
```

Now we have:

```text
Card component
       ↓
Props
       ↓
Different content
```

while the styling stays consistent.

---

# 7. Card With Button

Now let's compose components.

```tsx
function ProductCard() {
    return (
        <div className="p-5 border rounded">
            <h2 className="text-xl font-bold">
                Laptop
            </h2>

            <p className="text-gray-500">
                Development laptop
            </p>

            <Button variant="primary">
                Add to Cart
            </Button>
        </div>
    );
}
```

Architecture:

```text
ProductCard
│
├── title
├── description
└── Button
```

This is **component composition**.

---

# 8. Conditional Styling With State

Now let's use something you've already learned:

```tsx
useState
```

Example:

```tsx
import { useState } from "react";

function LikeButton() {
    const [liked, setLiked] = useState(false);

    return (
        <button
            className={
                liked
                    ? "px-4 py-2 bg-red-500 text-white rounded"
                    : "px-4 py-2 bg-gray-500 text-white rounded"
            }
            onClick={() => setLiked(!liked)}
        >
            {liked ? "Liked" : "Like"}
        </button>
    );
}
```

Initially:

```text
liked = false
```

Button:

```text
gray
Like
```

After clicking:

```text
liked = true
```

Button:

```text
red
Liked
```

---

# 9. Better Approach: Keep Base Classes

Notice we repeated:

```text
px-4 py-2 text-white rounded
```

That's not ideal.

We can keep common classes separately:

```tsx
const baseClasses =
    "px-4 py-2 text-white rounded";
```

Then:

```tsx
const buttonClasses = liked
    ? `${baseClasses} bg-red-500`
    : `${baseClasses} bg-gray-500`;
```

Then:

```tsx
<button className={buttonClasses}>
    {liked ? "Liked" : "Like"}
</button>
```

The idea:

```text
Common classes
      +
Dynamic classes
```

---

# 10. Conditional Classes Are Very Common

You'll see patterns like:

```tsx
className={active ? "bg-blue-500" : "bg-gray-500"}
```

or:

```tsx
className={
    disabled
        ? "opacity-50 cursor-not-allowed"
        : "cursor-pointer"
}
```

or:

```tsx
className={
    selected
        ? "border-blue-500"
        : "border-gray-300"
}
```

The important thing isn't memorizing these exact classes.

It's understanding:

```text
React state/props
       ↓
condition
       ↓
Tailwind class
```

---

# 11. A Practical Example — Tabs

Imagine:

```text
[ Profile ] [ Settings ] [ Security ]
```

We want the selected tab to look different.

```tsx
import { useState } from "react";

function Tabs() {
    const [activeTab, setActiveTab] = useState("profile");

    return (
        <div className="flex gap-4">
            <button
                className={
                    activeTab === "profile"
                        ? "border-b-2 border-blue-500 font-bold"
                        : "text-gray-500"
                }
                onClick={() => setActiveTab("profile")}
            >
                Profile
            </button>

            <button
                className={
                    activeTab === "settings"
                        ? "border-b-2 border-blue-500 font-bold"
                        : "text-gray-500"
                }
                onClick={() => setActiveTab("settings")}
            >
                Settings
            </button>

            <button
                className={
                    activeTab === "security"
                        ? "border-b-2 border-blue-500 font-bold"
                        : "text-gray-500"
                }
                onClick={() => setActiveTab("security")}
            >
                Security
            </button>
        </div>
    );
}
```

Here:

```text
activeTab
    ↓
condition
    ↓
Tailwind classes
    ↓
selected tab appearance
```

This is exactly the kind of thing you'll build in real React applications.

---

# 12. Tailwind + Responsive React

We can also make components responsive.

For example:

```tsx
<div className="flex flex-col md:flex-row">
```

Means:

```text
Small screen
    ↓
column

Medium screen and above
    ↓
row
```

Or:

```tsx
<div className="w-full md:w-1/2">
```

Means:

```text
Small screen
    ↓
100% width

Medium+
    ↓
50% width
```

We'll go much deeper into this in **Step 8**.

---

# 13. Full Example

Let's put the concepts together.

```tsx
import { useState } from "react";

type ButtonProps = {
    children: React.ReactNode;
    variant: "primary" | "secondary" | "danger";
};

function Button({ children, variant }: ButtonProps) {
    let classes =
        "px-4 py-2 rounded text-white";

    if (variant === "primary") {
        classes += " bg-blue-500";
    }

    if (variant === "secondary") {
        classes += " bg-gray-500";
    }

    if (variant === "danger") {
        classes += " bg-red-500";
    }

    return (
        <button className={classes}>
            {children}
        </button>
    );
}

function ProductCard() {
    const [liked, setLiked] = useState(false);

    return (
        <div className="w-full max-w-sm p-5 border rounded">
            <h2 className="text-2xl font-bold">
                Laptop
            </h2>

            <p className="mt-2 text-gray-500">
                Development laptop
            </p>

            <p className="mt-3 text-xl font-bold">
                ₹50,000
            </p>

            <div className="flex gap-3 mt-4">
                <Button variant="primary">
                    Add to Cart
                </Button>

                <button
                    className={
                        liked
                            ? "px-4 py-2 bg-red-500 text-white rounded"
                            : "px-4 py-2 bg-gray-500 text-white rounded"
                    }
                    onClick={() => setLiked(!liked)}
                >
                    {liked ? "Liked" : "Like"}
                </button>
            </div>
        </div>
    );
}

function App() {
    return (
        <div className="p-6">
            <ProductCard />
        </div>
    );
}

export default App;
```

This example contains:

```text
React
├── Component
├── Props
├── State
├── Event handling
└── Conditional rendering

Tailwind
├── Spacing
├── Colors
├── Typography
├── Borders
├── Flexbox
└── Conditional classes
```

---

# 14. One Important Lesson

Don't make this mistake:

```text
Every component
     ↓
100+ Tailwind classes
     ↓
Everything becomes difficult to understand
```

Tailwind is about **composing utilities**, but you should still think about component boundaries.

For example:

```text
Button
Card
Navbar
Modal
Input
ProductCard
```

should be reusable components when they make sense.

Then:

```text
Component
   +
Tailwind
   +
Props
```

gives you reusable UI.

---

# 15. Our Styling Progress

```text
## 8. Styling

[x] Step 1 — React styling + CSS Modules
[x] Step 2 — CSS Modules in depth
[x] Step 3 — Conditional & Dynamic Styling
[x] Step 4 — Styled Components
[x] Step 5 — Styled Components in depth
[x] Step 6 — Tailwind CSS basics
[x] Step 7 — Tailwind in React

[ ] Step 8 — Responsive Design
[ ] Step 9 — Accessibility
[ ] Step 10 — Final Styling Project
```

### Next → Step 8: Responsive Design

We'll learn how to make our React UI work properly on:

```text
Mobile  ↓
Tablet
   ↓
Desktop
```

and understand **breakpoints, responsive Flexbox/Grid, responsive typography, spacing, images, and practical responsive component design**.


------------------------------------------------------------------------------------------------------------------------------------------


# Step 8 — Responsive Design

Now we're learning how to make our React application work properly on **different screen sizes**.

The basic idea is:

```text
Mobile
  ↓
Tablet
  ↓
Desktop
```

A website shouldn't look good only on your laptop.

---

# 1. What Is Responsive Design?

Responsive design means:

> The UI automatically adapts to the available screen size.

For example, on desktop:

```text
┌──────────┬──────────┬──────────┐
│ Product  │ Product  │ Product  │
└──────────┴──────────┴──────────┘
```

On mobile:

```text
┌─────────────────┐
│     Product     │
├─────────────────┤
│     Product     │
├─────────────────┤
│     Product     │
└─────────────────┘
```

Same application.

Different layout.

---

# 2. Why Do We Need Responsive Design?

Users can access your application from:

```text
Phone
Tablet
Laptop
Desktop
Large monitor
```

If you build only for desktop, you might get:

```text
Mobile
 ↓
horizontal scrolling
 ↓
buttons overflowing
 ↓
text too large
 ↓
broken layout
```

Responsive design prevents this.

---

# 3. Mobile First

A very important concept is:

> **Mobile-first design**

Instead of thinking:

```text
Desktop
 ↓
Make it smaller for mobile
```

we generally think:

```text
Mobile
 ↓
Tablet
 ↓
Desktop
```

Start with the smaller screen.

Then add styles for larger screens.

---

# 4. Tailwind Makes This Easy

Remember Tailwind's responsive prefixes.

For example:

```tsx
<div className="text-sm md:text-lg">
```

means:

```text
Mobile
 ↓
text-sm

Medium screen+
 ↓
text-lg
```

The important pattern is:

```text
base class
    +
responsive prefix
```

---

# 5. Common Breakpoint Pattern

You will commonly see:

```text
sm:
md:
lg:
xl:
2xl:
```

For example:

```tsx
<div className="text-sm md:text-lg lg:text-xl">
```

Conceptually:

```text
Small screen
    ↓
text-sm

Medium screen
    ↓
text-lg

Large screen
    ↓
text-xl
```

You don't need to memorize the exact pixel values right now.

Just understand the concept:

```text
base
 ↓
mobile/default

md:
 ↓
medium+

lg:
 ↓
large+
```

---

# 6. Responsive Flexbox

Suppose we have:

```tsx
<div className="flex flex-col md:flex-row">
```

This is very common.

It means:

```text
Mobile
 ↓
column

Desktop/medium+
 ↓
row
```

So mobile:

```text
Product
   ↓
Price
   ↓
Button
```

Desktop:

```text
Product   Price   Button
```

---

# 7. Responsive Grid

Suppose we have products.

On mobile:

```text
┌─────────┐
│ Product │
├─────────┤
│ Product │
├─────────┤
│ Product │
└─────────┘
```

Desktop:

```text
┌─────────┬─────────┬─────────┐
│ Product │ Product │ Product │
└─────────┴─────────┴─────────┘
```

Tailwind:

```tsx
<div className="grid grid-cols-1 md:grid-cols-3">
```

Meaning:

```text
Mobile
 ↓
1 column

Medium+
 ↓
3 columns
```

---

# 8. Responsive Width

You might see:

```tsx
<div className="w-full md:w-1/2">
```

Meaning:

```text
Mobile
 ↓
100% width

Medium+
 ↓
50% width
```

This is useful for forms, cards, layouts, etc.

---

# 9. Responsive Padding

You can also change spacing.

```tsx
<div className="p-4 md:p-8">
```

Meaning:

```text
Mobile
 ↓
padding: 4

Medium+
 ↓
padding: 8
```

So the component gets more breathing room on larger screens.

---

# 10. Responsive Typography

For example:

```tsx
<h1 className="text-2xl md:text-4xl lg:text-5xl">
    Welcome
</h1>
```

Conceptually:

```text
Mobile
 ↓
small heading

Tablet
 ↓
larger heading

Desktop
 ↓
large heading
```

This prevents huge headings from taking over a small mobile screen.

---

# 11. Responsive Navigation

A very common real-world example is a navbar.

Desktop:

```text
Logo    Home   Products   About   Contact
```

Mobile:

```text
Logo                         ☰
```

Usually:

```text
Desktop
 ↓
show navigation links

Mobile
 ↓
show menu button
```

With Tailwind, you can control visibility using responsive classes.

For example:

```tsx
<div className="hidden md:flex">
    ...
</div>
```

Conceptually:

```text
Mobile
 ↓
hidden

Medium+
 ↓
flex
```

And:

```tsx
<button className="md:hidden">
    Menu
</button>
```

means:

```text
Mobile
 ↓
show Menu

Medium+
 ↓
hide Menu
```

---

# 12. Full Responsive Product Layout

Let's build a practical example.

```tsx
function ProductList() {
    return (
        <div className="grid grid-cols-1 gap-4 p-4 md:grid-cols-2 md:p-8 lg:grid-cols-3">
            
            <div className="p-5 border rounded">
                <h2 className="text-xl font-bold">
                    Laptop
                </h2>

                <p className="mt-2 text-gray-500">
                    Development laptop
                </p>

                <button className="px-4 py-2 mt-4 text-white bg-blue-500 rounded">
                    Buy
                </button>
            </div>

            <div className="p-5 border rounded">
                <h2 className="text-xl font-bold">
                    Phone
                </h2>

                <p className="mt-2 text-gray-500">
                    Android smartphone
                </p>

                <button className="px-4 py-2 mt-4 text-white bg-blue-500 rounded">
                    Buy
                </button>
            </div>

            <div className="p-5 border rounded">
                <h2 className="text-xl font-bold">
                    Tablet
                </h2>

                <p className="mt-2 text-gray-500">
                    Portable tablet
                </p>

                <button className="px-4 py-2 mt-4 text-white bg-blue-500 rounded">
                    Buy
                </button>
            </div>

        </div>
    );
}
```

The important part is:

```text
grid-cols-1
    ↓
Mobile: 1 column

md:grid-cols-2
    ↓
Medium: 2 columns

lg:grid-cols-3
    ↓
Large: 3 columns
```

---

# 13. Responsive Component Thinking

Don't think:

> "I need to make the entire website responsive."

Think:

> "How should each component behave at different sizes?"

For example:

```text
Navbar
 ↓
Mobile → menu
Desktop → links

ProductGrid
 ↓
Mobile → 1 column
Tablet → 2 columns
Desktop → 3 columns

ProductCard
 ↓
Mobile → full width
Desktop → fixed/max width

Hero
 ↓
Mobile → column
Desktop → row
```

This is much easier to reason about.

---

# 14. Don't Use Too Many Breakpoints

You don't need:

```text
sm
md
lg
xl
2xl
```

on every element.

For example, this can become unnecessarily complicated:

```tsx
<div className="
    text-sm
    sm:text-base
    md:text-lg
    lg:text-xl
    xl:text-2xl
">
```

Sometimes:

```tsx
<div className="text-base md:text-lg">
```

is enough.

Keep responsive rules simple.

---

# 15. Responsive Design Is Not Only Tailwind

Everything we're doing with Tailwind is based on normal CSS concepts.

In normal CSS you could use:

```css
@media (...) {
    ...
}
```

Tailwind simply gives you convenient utilities for responsive styling.

So remember:

```text
CSS media queries
       ↓
Tailwind responsive utilities
```

Understanding CSS makes Tailwind easier.

---

# 16. Important Responsive Concepts

You should now understand these:

```text
Responsive Design
       ↓
Mobile First
       ↓
Breakpoints
       ↓
Responsive Flexbox
       ↓
Responsive Grid
       ↓
Responsive Width
       ↓
Responsive Spacing
       ↓
Responsive Typography
       ↓
Responsive Visibility
```

---

# 17. Practical Mental Model

When creating a component, ask:

### Question 1

What should it look like on mobile?

```text
Mobile
 ↓
default styles
```

### Question 2

What changes on a larger screen?

```text
md:
lg:
```

### Question 3

Does the layout change?

```text
column → row
1 column → 3 columns
```

### Question 4

Does the size change?

```text
small → large
```

### Question 5

Does something appear/disappear?

```text
menu button
navigation links
```

That's responsive thinking.

---

# Phase 8 Progress

```text
## 8. Styling

[x] Step 1 — React styling + CSS Modules
[x] Step 2 — CSS Modules in depth
[x] Step 3 — Conditional & Dynamic Styling
[x] Step 4 — Styled Components
[x] Step 5 — Styled Components in depth
[x] Step 6 — Tailwind CSS basics
[x] Step 7 — Tailwind in React
[x] Step 8 — Responsive Design

[ ] Step 9 — Accessibility
[ ] Step 10 — Final Styling Project
```

### Next → Step 9: Accessibility

We'll cover **semantic HTML, ARIA, keyboard navigation, focus, accessible forms/buttons, screen readers, and color contrast**—especially how these apply to React.


------------------------------------------------------------------------------------------------------------------------------------------

# Step 9 — Accessibility

Now we move to an important part of frontend development: **making our application usable by as many people as possible**, including people who use keyboards, screen readers, or other assistive technologies.

Your roadmap specifically mentions:

```text
Accessibility basics
├── Semantic HTML
├── ARIA
├── Keyboard navigation
└── Color contrast
```

Let's learn these one by one.

---

# 1. What Is Accessibility?

Accessibility means:

> Designing and developing a website so that people with different abilities can use it effectively.

For example, someone might:

* Use only a keyboard instead of a mouse.
* Use a screen reader.
* Have difficulty seeing certain colors.
* Have difficulty using small or unclear controls.

Think:

```text
Normal UI
   ↓
Works with mouse
```

But accessible UI:

```text
UI
├── Mouse
├── Keyboard
├── Screen reader
└── Assistive technologies
```

---

# 2. Semantic HTML

You already learned semantic HTML.

This is one of the easiest ways to improve accessibility.

Instead of:

```tsx
<div onClick={handleClick}>
    Save
</div>
```

use:

```tsx
<button onClick={handleClick}>
    Save
</button>
```

Why?

Because a real `<button>` already tells the browser:

> "This is an interactive button."

It also gets keyboard behavior automatically.

---

# 3. Don't Use `div` as a Button

This is a common mistake.

Bad:

```tsx
<div onClick={handleClick}>
    Delete
</div>
```

Better:

```tsx
<button onClick={handleClick}>
    Delete
</button>
```

The difference is important.

A `<button>` naturally supports:

```text
Mouse click
Keyboard interaction
Focus
Screen readers
```

Whereas a `<div>` doesn't naturally behave like a button.

---

# 4. Links vs Buttons

Another important distinction.

Use:

```tsx
<a href="/profile">
    Profile
</a>
```

when you're **navigating somewhere**.

Use:

```tsx
<button onClick={handleDelete}>
    Delete
</button>
```

when you're **performing an action**.

Think:

```text
<a>
 ↓
Go somewhere

<button>
 ↓
Do something
```

Don't use a button for navigation just because you like how it looks.

CSS can make either element look however you want.

---

# 5. Forms and Labels

Consider an input:

```tsx
<input type="text" />
```

A user may not know what the input is for.

Better:

```tsx
<label>
    Username

    <input type="text" />
</label>
```

Now the relationship is clear.

Another common approach:

```tsx
<label htmlFor="username">
    Username
</label>

<input
    id="username"
    type="text"
/>
```

Here:

```text
label
 ↓
htmlFor="username"
 ↓
id="username"
 ↓
input
```

The label is associated with the input.

---

# 6. Why Labels Matter

Imagine a screen reader encountering:

```text
Input
```

What is it?

Username?

Email?

Password?

Phone?

But with a label:

```text
Username
Input
```

the purpose is clear.

So whenever you have a form input, think:

```text
Input
 +
Label
```

---

# 7. Keyboard Navigation

Now let's understand keyboard accessibility.

A user should generally be able to navigate interactive elements using:

```text
Tab
Shift + Tab
Enter
Space
```

For example:

```text
Navbar
 ↓
Tab
 ↓
Home
 ↓
Tab
 ↓
Products
 ↓
Tab
 ↓
Contact
```

This should work without requiring a mouse.

---

# 8. Focus

When you press `Tab`, one element becomes focused.

For example:

```text
        ↓
[ Save ]
```

The user needs to visually understand:

> "This is the element currently selected."

This is called **focus**.

Don't remove focus styling without providing another clear indication.

For example, be careful with:

```css
button {
    outline: none;
}
```

If you remove the browser's focus indicator, you should provide a suitable alternative.

---

# 9. Tailwind Focus Styles

Since we're currently learning Tailwind, you can use focus utilities.

For example:

```tsx
<button className="focus:ring-2">
    Save
</button>
```

The idea is:

```text
Normal
 ↓
normal appearance

Focused
 ↓
visible focus indicator
```

You don't need to memorize the exact Tailwind focus utilities yet.

The important concept is:

> **Keyboard users need to see where focus is.**

---

# 10. ARIA

Now we come to **ARIA**.

ARIA stands for:

> Accessible Rich Internet Applications

ARIA provides additional information to assistive technologies when normal HTML isn't enough.

For example:

```tsx
<button aria-label="Close">
    X
</button>
```

A screen reader can understand:

```text
Close
```

instead of just:

```text
X
```

---

# 11. When Should You Use ARIA?

A very important rule:

> **Use semantic HTML first. Use ARIA when necessary.**

For example, don't do this:

```tsx
<div
    role="button"
    tabIndex={0}
>
    Save
</div>
```

if you can simply do:

```tsx
<button>
    Save
</button>
```

The real button already provides the correct semantics and behavior.

Think:

```text
Semantic HTML
      ↓
First choice

ARIA
      ↓
Additional help when needed
```

---

# 12. `aria-label`

A common example is an icon-only button.

Imagine:

```text
[ X ]
```

The `X` might mean:

```text
Close
```

You can write:

```tsx
<button aria-label="Close">
    X
</button>
```

Now assistive technology has a meaningful label.

Another example:

```tsx
<button aria-label="Search">
    🔍
</button>
```

The visual icon is for sighted users.

The accessible label communicates the purpose.

---

# 13. Images and `alt`

Remember HTML:

```html
<img src="laptop.jpg" alt="Laptop">
```

The `alt` text describes the image.

In React:

```tsx
<img
    src="/laptop.jpg"
    alt="Laptop"
/>
```

For a meaningful image:

```text
Image
 ↓
Meaningful alt text
```

For a purely decorative image, you can use:

```tsx
<img
    src="/decoration.jpg"
    alt=""
/>
```

The empty `alt` tells assistive technology that the image is decorative.

---

# 14. Color Contrast

Accessibility isn't only about HTML.

Text needs sufficient contrast against its background.

Bad example:

```text
Light gray text
       +
White background
```

This can be difficult to read.

Better:

```text
Dark text
   +
Light background
```

Think:

```text
Text
 ↓
Contrast
 ↓
Readable
```

Don't communicate important information **only through color**.

For example, avoid:

```text
Red = error
Green = success
```

with no other indication.

Better:

```text
❌ Error: Password is incorrect

✓ Success: Profile updated
```

The text communicates the meaning as well.

---

# 15. Accessible Button Example

Let's combine several concepts.

```tsx
function SaveButton() {
    return (
        <button
            type="button"
            className="px-4 py-2 rounded focus:ring-2"
        >
            Save
        </button>
    );
}
```

We have:

```text
<button>
    ↓
Semantic element

type="button"
    ↓
Explicit button behavior

focus:ring-2
    ↓
Visible focus indication
```

---

# 16. Accessible Form Example

Here's a simple React form:

```tsx
function LoginForm() {
    return (
        <form>
            <div>
                <label htmlFor="email">
                    Email
                </label>

                <input
                    id="email"
                    type="email"
                    className="border p-2"
                />
            </div>

            <div>
                <label htmlFor="password">
                    Password
                </label>

                <input
                    id="password"
                    type="password"
                    className="border p-2"
                />
            </div>

            <button
                type="submit"
                className="px-4 py-2"
            >
                Login
            </button>
        </form>
    );
}
```

The structure is:

```text
form
│
├── label
│    ↓
│   email input
│
├── label
│    ↓
│   password input
│
└── submit button
```

This is much better than using random `<div>` elements for everything.

---

# 17. Accessible Error Message

Suppose the user enters an invalid email.

You might have:

```tsx
<p>
    Please enter a valid email.
</p>
```

You can associate the error with the input:

```tsx
<input
    id="email"
    aria-describedby="email-error"
/>

<p id="email-error">
    Please enter a valid email.
</p>
```

The relationship becomes:

```text
Input
 ↓
aria-describedby
 ↓
email-error
 ↓
Error message
```

This helps assistive technology understand the relationship.

---

# 18. Accessibility + Component Thinking

This is very important for React.

If you create:

```text
Button
Input
Modal
Dropdown
Navbar
Card
```

you should think about accessibility **when creating the component**, not after the entire application is finished.

For example:

```text
Reusable Button
      ↓
semantic <button>
      ↓
keyboard accessible
      ↓
focus visible
      ↓
usable everywhere
```

Then every place using your `Button` benefits from the good implementation.

That's good component architecture.

---

# 19. Accessibility Checklist

When creating a React component, ask:

### Structure

```text
Am I using the correct HTML element?
```

### Keyboard

```text
Can I use it with Tab/Enter/Space?
```

### Focus

```text
Can I clearly see the focused element?
```

### Forms

```text
Does every input have a label?
```

### Images

```text
Does the image need alt text?
```

### ARIA

```text
Do I actually need ARIA?
```

### Color

```text
Is the text readable?
```

### Meaning

```text
Does the UI communicate information without relying only on color?
```

---

# 20. Most Important Rules

If you remember only these, you're already on the right track:

```text
1. Use semantic HTML.

2. Use <button> for actions.

3. Use <a> for navigation.

4. Give form inputs proper labels.

5. Make interactive elements keyboard accessible.

6. Keep focus visible.

7. Use meaningful alt text for meaningful images.

8. Use ARIA only when necessary.

9. Maintain good color contrast.

10. Don't communicate important information using color alone.
```

---

# Phase 8 Progress

```text
## 8. Styling

[x] Step 1 — React styling + CSS Modules
[x] Step 2 — CSS Modules in depth
[x] Step 3 — Conditional & Dynamic Styling
[x] Step 4 — Styled Components
[x] Step 5 — Styled Components in depth
[x] Step 6 — Tailwind CSS basics
[x] Step 7 — Tailwind in React
[x] Step 8 — Responsive Design
[x] Step 9 — Accessibility

[ ] Step 10 — Final Styling Project
```

## Next → Step 10: Final Styling Project

This is the **last step of Phase 8**.

We'll combine everything we've learned:

```text
React
 +
Tailwind
 +
Reusable Components
 +
Responsive Design
 +
Accessibility
```

and build a small **responsive, accessible UI** from scratch.

------------------------------------------------------------------------------------------------------------------------------------------


# Step 10 — Final Styling Project

This is the final step of **Phase 8: Styling**.

We will build a small:

# Responsive Product Dashboard

It will include:

```text
React
├── Components
├── Props
├── State
├── Event Handling
│
Tailwind
├── Layout
├── Flexbox
├── Grid
├── Responsive Design
├── Hover/Focus States
│
Accessibility
├── Semantic HTML
├── Labels
├── Buttons
└── Keyboard Focus
```

---

# 1. Project Structure

We'll keep it simple:

```text
src/
│
├── components/
│   ├── Header.tsx
│   ├── SearchBar.tsx
│   └── ProductCard.tsx
│
├── App.tsx
└── main.tsx
```

---

# 2. What Are We Building?

The UI:

```text
------------------------------------------------
Logo                         Products   About

------------------------------------------------

           Product Store

        [ Search products... ]

------------------------------------------------

[ Product ] [ Product ] [ Product ]

[ Product ] [ Product ] [ Product ]

------------------------------------------------
```

Responsive behavior:

```text
Mobile
↓

1 Product per row


Tablet
↓

2 Products per row


Desktop
↓

3 Products per row
```

---

# 3. Product Data

First, in `App.tsx`, let's create some data.

```tsx
const products = [
    {
        id: 1,
        name: "Laptop",
        description: "Powerful development laptop",
        price: 50000,
    },
    {
        id: 2,
        name: "Phone",
        description: "Modern smartphone",
        price: 30000,
    },
    {
        id: 3,
        name: "Headphones",
        description: "Wireless headphones",
        price: 5000,
    },
    {
        id: 4,
        name: "Keyboard",
        description: "Mechanical keyboard",
        price: 4000,
    },
];
```

Nothing new here.

We already know:

```text
Array
 ↓
map()
 ↓
Render components
```

---

# 4. Header Component

Create:

```text
components/Header.tsx
```

```tsx
function Header() {
    return (
        <header className="flex items-center justify-between px-4 py-4 border-b md:px-8">
            <h1 className="text-xl font-bold">
                Product Store
            </h1>

            <nav>
                <ul className="flex gap-4">
                    <li>
                        <a
                            href="#products"
                            className="hover:underline focus:outline-none focus:ring-2"
                        >
                            Products
                        </a>
                    </li>

                    <li>
                        <a
                            href="#about"
                            className="hover:underline focus:outline-none focus:ring-2"
                        >
                            About
                        </a>
                    </li>
                </ul>
            </nav>
        </header>
    );
}

export default Header;
```

### What did we use?

```text
<header>
 ↓
Semantic HTML

<nav>
 ↓
Navigation area

<ul>
 ↓
List of navigation links

focus:ring
 ↓
Keyboard accessibility
```

---

# 5. SearchBar Component

Create:

```text
components/SearchBar.tsx
```

```tsx
type SearchBarProps = {
    search: string;
    setSearch: (value: string) => void;
};

function SearchBar({
    search,
    setSearch,
}: SearchBarProps) {
    return (
        <div>
            <label
                htmlFor="search"
                className="sr-only"
            >
                Search products
            </label>

            <input
                id="search"
                type="text"
                value={search}
                onChange={(event) =>
                    setSearch(event.target.value)
                }
                placeholder="Search products..."
                className="w-full p-3 border rounded focus:outline-none focus:ring-2"
            />
        </div>
    );
}

export default SearchBar;
```

### Important concepts

```text
Props
 ↓
search

State lives in App
 ↓
passed to SearchBar

User types
 ↓
onChange

setSearch()
 ↓
updates App state
```

Also notice:

```tsx
<label htmlFor="search">
```

This is for accessibility.

---

# 6. ProductCard Component

Create:

```text
components/ProductCard.tsx
```

```tsx
type Product = {
    id: number;
    name: string;
    description: string;
    price: number;
};

type ProductCardProps = {
    product: Product;
};

function ProductCard({
    product,
}: ProductCardProps) {
    return (
        <article className="flex flex-col p-5 border rounded">
            <h2 className="text-xl font-bold">
                {product.name}
            </h2>

            <p className="mt-2 text-gray-600">
                {product.description}
            </p>

            <p className="mt-4 text-lg font-bold">
                ₹{product.price}
            </p>

            <button
                type="button"
                className="px-4 py-2 mt-4 text-white bg-blue-600 rounded hover:bg-blue-700 focus:outline-none focus:ring-2"
            >
                Add to Cart
            </button>
        </article>
    );
}

export default ProductCard;
```

### Why `<article>`?

Each product card represents an independent piece of content.

So:

```text
Product
 ↓
Independent content
 ↓
<article>
```

---

# 7. Main App Component

Now let's connect everything.

## `App.tsx`

```tsx
import { useState } from "react";

import Header from "./components/Header";
import SearchBar from "./components/SearchBar";
import ProductCard from "./components/ProductCard";

const products = [
    {
        id: 1,
        name: "Laptop",
        description: "Powerful development laptop",
        price: 50000,
    },
    {
        id: 2,
        name: "Phone",
        description: "Modern smartphone",
        price: 30000,
    },
    {
        id: 3,
        name: "Headphones",
        description: "Wireless headphones",
        price: 5000,
    },
    {
        id: 4,
        name: "Keyboard",
        description: "Mechanical keyboard",
        price: 4000,
    },
];

function App() {
    const [search, setSearch] = useState("");

    const filteredProducts = products.filter((product) =>
        product.name
            .toLowerCase()
            .includes(search.toLowerCase())
    );

    return (
        <>
            <Header />

            <main className="max-w-6xl p-4 mx-auto md:p-8">
                <section>
                    <h2 className="text-2xl font-bold md:text-4xl">
                        Our Products
                    </h2>

                    <p className="mt-2 text-gray-600">
                        Browse our collection of products.
                    </p>

                    <div className="mt-6">
                        <SearchBar
                            search={search}
                            setSearch={setSearch}
                        />
                    </div>
                </section>

                <section
                    id="products"
                    className="grid grid-cols-1 gap-6 mt-8 md:grid-cols-2 lg:grid-cols-3"
                >
                    {filteredProducts.map((product) => (
                        <ProductCard
                            key={product.id}
                            product={product}
                        />
                    ))}
                </section>
            </main>
        </>
    );
}

export default App;
```

---

# 8. Understand the Complete Data Flow

This is important.

```text
User types in SearchBar
        ↓
onChange event
        ↓
setSearch()
        ↓
App state updates
        ↓
App re-renders
        ↓
filteredProducts updates
        ↓
map()
        ↓
ProductCard components update
```

This combines many React concepts you've already learned.

---

# 9. Responsive Design Used

Look at this:

```tsx
className="
    grid
    grid-cols-1
    gap-6
    md:grid-cols-2
    lg:grid-cols-3
"
```

The behavior:

```text
Mobile
↓
1 column

Tablet
↓
2 columns

Desktop
↓
3 columns
```

Another example:

```tsx
text-2xl md:text-4xl
```

```text
Mobile
↓
smaller heading

Desktop
↓
larger heading
```

---

# 10. Accessibility Used

Let's check what we included.

### Semantic HTML

```tsx
<header>
<nav>
<main>
<section>
<article>
```
| Tag         | Meaning                   |
| ----------- | ------------------------- |
| `<header>`  | Intro/top area            |
| `<nav>`     | Navigation links          |
| `<main>`    | Primary content           |
| `<section>` | Group of related content  |
| `<article>` | Independent content       |
| `<footer>`  | Bottom/footer information |


### Accessible Input

```tsx
<label htmlFor="search">
<input id="search">
```

### Proper Button

```tsx
<button>
```

instead of:

```tsx
<div onClick={...}>
```

### Focus Styles

```text
focus:ring-2
```

### Navigation Links

```tsx
<a href="#products">
```

Correct element for navigation.

---

# 11. Component Architecture

Our application looks like this:

```text
App
│
├── Header
│
├── SearchBar
│
└── ProductCard
      ├── Product 1
      ├── Product 2
      ├── Product 3
      └── Product 4
```

This is proper React component thinking.

Each component has one responsibility.

```text
Header
→ Navigation

SearchBar
→ Search input

ProductCard
→ Display one product

App
→ State + Data + Overall layout
```

---

# 12. Full Project Code

For easier reference:

## `Header.tsx`

```tsx
function Header() {
    return (
        <header className="flex items-center justify-between px-4 py-4 border-b md:px-8">
            <h1 className="text-xl font-bold">
                Product Store
            </h1>

            <nav>
                <ul className="flex gap-4">
                    <li>
                        <a
                            href="#products"
                            className="hover:underline focus:outline-none focus:ring-2"
                        >
                            Products
                        </a>
                    </li>

                    <li>
                        <a href="#about">
                            About
                        </a>
                    </li>
                </ul>
            </nav>
        </header>
    );
}

export default Header;
```

## `SearchBar.tsx`

```tsx
type SearchBarProps = {
    search: string;
    setSearch: (value: string) => void;
};

function SearchBar({
    search,
    setSearch,
}: SearchBarProps) {
    return (
        <div>
            <label
                htmlFor="search"
                className="sr-only"
            >
                Search products
            </label>

            <input
                id="search"
                type="text"
                value={search}
                onChange={(event) =>
                    setSearch(event.target.value)
                }
                placeholder="Search products..."
                className="w-full p-3 border rounded focus:outline-none focus:ring-2"
            />
        </div>
    );
}

export default SearchBar;
```

## `ProductCard.tsx`

```tsx
type Product = {
    id: number;
    name: string;
    description: string;
    price: number;
};

type ProductCardProps = {
    product: Product;
};

function ProductCard({
    product,
}: ProductCardProps) {
    return (
        <article className="flex flex-col p-5 border rounded">
            <h2 className="text-xl font-bold">
                {product.name}
            </h2>

            <p className="mt-2 text-gray-600">
                {product.description}
            </p>

            <p className="mt-4 text-lg font-bold">
                ₹{product.price}
            </p>

            <button
                type="button"
                className="px-4 py-2 mt-4 text-white bg-blue-600 rounded hover:bg-blue-700 focus:outline-none focus:ring-2"
            >
                Add to Cart
            </button>
        </article>
    );
}

export default ProductCard;
```

## `App.tsx`

```tsx
import { useState } from "react";

import Header from "./components/Header";
import SearchBar from "./components/SearchBar";
import ProductCard from "./components/ProductCard";

const products = [
    {
        id: 1,
        name: "Laptop",
        description: "Powerful development laptop",
        price: 50000,
    },
    {
        id: 2,
        name: "Phone",
        description: "Modern smartphone",
        price: 30000,
    },
    {
        id: 3,
        name: "Headphones",
        description: "Wireless headphones",
        price: 5000,
    },
    {
        id: 4,
        name: "Keyboard",
        description: "Mechanical keyboard",
        price: 4000,
    },
];

function App() {
    const [search, setSearch] = useState("");

    const filteredProducts = products.filter((product) =>
        product.name
            .toLowerCase()
            .includes(search.toLowerCase())
    );

    return (
        <>
            <Header />

            <main className="max-w-6xl p-4 mx-auto md:p-8">
                <section>
                    <h2 className="text-2xl font-bold md:text-4xl">
                        Our Products
                    </h2>

                    <p className="mt-2 text-gray-600">
                        Browse our collection.
                    </p>

                    <div className="mt-6">
                        <SearchBar
                            search={search}
                            setSearch={setSearch}
                        />
                    </div>
                </section>

                <section
                    id="products"
                    className="grid grid-cols-1 gap-6 mt-8 md:grid-cols-2 lg:grid-cols-3"
                >
                    {filteredProducts.map((product) => (
                        <ProductCard
                            key={product.id}
                            product={product}
                        />
                    ))}
                </section>
            </main>
        </>
    );
}

export default App;
```

---

# 13. What Did We Learn in Phase 8?

## Styling

```text
[x] CSS basics revision
[x] CSS Modules
[x] Conditional & Dynamic Styling
[x] Styled Components
[x] Styled Components in depth
[x] Tailwind CSS basics
[x] Tailwind in React
[x] Responsive Design
[x] Accessibility
[x] Final Styling Project
```

---

# 14. Styling Phase Complete

Your roadmap section:

```text
## 8. Styling

[x] CSS Modules
[x] Styled Components
[x] Tailwind
[x] Responsive design
[x] Accessibility basics
```

## Phase 8 is now complete. 

You now understand multiple styling approaches and, more importantly, how styling connects with React components, props, state, responsiveness, and accessibility.


# Next Phase → API Integration

According to your roadmap:

```text
## 9. API Integration

[ ] Fetch / Axios
[ ] REST APIs
[ ] Error handling
[ ] Loading states
[ ] React Query / TanStack Query

Build:
Weather App
Blog CRUD App
```

This will connect everything you've learned so far to real backend APIs.

------------------------------------------------------------------------------------------------------------------------------------------