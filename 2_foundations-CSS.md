# Cascading Style Sheets


## 1. What is CSS?

**CSS = Cascading Style Sheets**

- Right now your pages:

Have structure ✅

But look plain ❌

HTML tells the browser **what something is**.

CSS tells the browser **how it should look**.

For example:

```html
<h1>My Profile</h1>
<p>Hello, my name is William</p>
```

HTML creates:

```text
My Profile
Hello, my name is William
```

CSS can make the heading blue, bigger, centered, etc.

---

## 2. How does CSS work?

A basic CSS rule looks like this:

```css
h1 {
  color: blue;
}
```

There are 3 important parts:

```text
h1       → selector
color    → property
blue     → value
```

So:

**selector → property → value**

The meaning is:

> Find all `<h1>` elements and make their text blue.

---

## 3. Let's try a simple example

HTML:

```html
<h1>My Profile</h1>
<p>Hello, my name is William</p>
```

CSS:

```css
h1 {
  color: blue;
}

p {
  color: gray;
}
```

Now:

* `<h1>` → blue text
* `<p>` → gray text

---

# 4. Basic CSS Properties

### `color`

Changes **text color**.

```css
h1 {
  color: red;
}
```

### `background-color`

Changes **background color**.

```css
h1 {
  background-color: yellow;
}
```

### `font-size`

Changes **text size**.

```css
h1 {
  font-size: 40px;
}
```

### `text-align`

Controls text alignment.

```css
h1 {
  text-align: center;
}
```

You can use:

* `left`
* `center`
* `right`

---

## 5. Important idea

Don't try to memorize everything.

For now, understand this:

```text
CSS
 │
 ├── selector
 │
 └── {
      property: value;
    }
```

Example:

```text
h1
 ↓
color: blue
     ↓
   property + value
```

### Your first CSS basics to learn

We'll go in this order:

**1. `color` → 2. `background-color` → 3. `font-size` → 4. `text-align` → 5. `width` → 6. `height` → 7. `margin` → 8. `padding` → 9. `border`**

Let's use **one small profile card** and apply all 9 CSS properties to it.

### HTML

```html
<div class="profile">
  <h1>William</h1>
  <p>Frontend Developer</p>
  <button>Contact Me</button>
</div>
```

### CSS

```css
.profile {
  color: white;
  background-color: black;
  font-size: 18px;
  text-align: center;
  width: 300px;
  height: 200px;
  margin: 20px;
  padding: 20px;
  border: 2px solid blue;
}
```

Now let's understand **what each one does**:

| CSS                | What it does                         |
| ------------------ | ------------------------------------ |
| `color`            | Changes **text color**               |
| `background-color` | Changes **background color**         |
| `font-size`        | Changes **text size**                |
| `text-align`       | Controls **text alignment**          |
| `width`            | Controls **width**                   |
| `height`           | Controls **height**                  |
| `margin`           | Adds **space outside** the element   |
| `padding`          | Adds **space inside** the element    |
| `border`           | Adds a **border around** the element |

So visually, think of it like:

```text
          margin
      ↓           ↓
   ┌─────────────────────┐  ← border
   │      padding        │
   │                     │
   │       William       │  ← content
   │   Frontend Developer│
   │     Contact Me      │
   │                     │
   └─────────────────────┘
```

The most important thing to understand now is:

**margin = outside space**

**padding = inside space**

And:

**width/height = size**

---

# Box Model

**Basic Styling and Box Model use some of the same CSS properties**.

Let's separate them clearly.

## Think about a real box

Imagine you have a cardboard box containing a phone:

```text
          MARGIN
   ┌───────────────────┐
   │      BORDER       │
   │  ┌─────────────┐  │
   │  │   PADDING   │  │
   │  │   ┌─────┐   │  │
   │  │   │PHONE│   │  │
   │  │   └─────┘   │  │
   │  └─────────────┘  │
   └───────────────────┘
```

### Basic Styling

Basic Styling means:

> **"How do I make my HTML element look different?"**

For example:

```css
h1 {
  color: blue;
  font-size: 30px;
  text-align: center;
  background-color: yellow;
}
```

You're changing the **appearance**.

```text
color          → text appearance
background     → background appearance
font-size      → text size
text-align     → text position
```

---

# Box Model

Box Model means:

> **"How much space does this HTML element occupy, and how is that space divided?"**

The main concepts are:

```text
Content
Padding
Border
Margin
```

For example:

```css
div {
  width: 300px;
  padding: 20px;
  border: 2px solid black;
  margin: 30px;
}
```

Here you're controlling the **box and its spacing**.

---

# The easiest difference

Imagine a button:

```text
┌──────────────────────────┐
│       Contact Me         │
└──────────────────────────┘
```

### Basic Styling

You might say:

> "Make the text white and the background blue."

```text
color
background-color
font-size
text-align
```

You're changing **how it looks**.

### Box Model

You might say:

> "Give the button more space inside and separate it from other elements."

```text
padding
border
margin
width
height
```

You're controlling **the box and its space**.

---

## Why does it feel confusing?

Because properties like `padding`, `margin`, `width`, and `height` were introduced during Basic Styling.

**Basic Styling** is the broad introduction to common CSS properties.

**Box Model** is the deeper concept that explains how an element's **content, padding, border, and margin** work together.

### Remember this:

**Basic Styling = How does it look?**

**Box Model = How does the box take up space?**

---

# Flexbox

**Flexbox = Flexible Box Layout**

It is used to **arrange elements inside a container**.

The easiest way to understand Flexbox is with a simple example.

### Without Flexbox

```html
<div>
  <div>Box 1</div>
  <div>Box 2</div>
  <div>Box 3</div>
</div>
```

Normally, the boxes appear one below another:

```text
Box 1
Box 2
Box 3
```

### With Flexbox

We make the parent `<div>` a Flexbox container:

```css
.container {
  display: flex;
}
```

Now the boxes can appear in a row:

```text
Box 1    Box 2    Box 3
```

---

## The most important idea

In Flexbox, there are two things:

**Parent = Flex container**

**Children = Flex items**

```text
        Parent
     .container
          │
    ┌─────┼─────┐
    ↓     ↓     ↓
  Box 1  Box 2  Box 3
```

When you write:

```css
.container {
  display: flex;
}
```

you are saying:

> "I want to arrange the children of this container using Flexbox."

---

## `justify-content`

This controls how the items are positioned **along the main direction**.

For example:

```css
.container {
  display: flex;
  justify-content: center;
}
```

Result:

```text
       Box 1   Box 2   Box 3
              ↑
            center
```

Common values:

```text
flex-start → beginning
center     → center
flex-end   → end
```

---

## `align-items`

This controls the items in the **other direction**.

```css
.container {
  display: flex;
  align-items: center;
}
```

So you can think:

**`justify-content` → main direction**

**`align-items` → cross direction**

---

### The 3 Flexbox properties you should learn first

```text
display: flex
justify-content
align-items
```

Once these three are clear, Flexbox becomes much easier.

---

# Flexbox — `justify-content`

Now let's understand **`justify-content`** properly.

First:

```css
.container {
  display: flex;
}
```

This puts the items in a row by default:

```text
Box 1   Box 2   Box 3
```

`justify-content` decides **where those items go along the row**.

### 1. `flex-start`

```css
justify-content: flex-start;
```

Items stay at the **start**:

```text
Box 1  Box 2  Box 3
```

### 2. `center`

```css
justify-content: center;
```

Items move to the **center**:

```text
       Box 1  Box 2  Box 3
```

### 3. `flex-end`

```css
justify-content: flex-end;
```

Items move to the **end**:

```text
                 Box 1  Box 2  Box 3
```

### 4. `space-between`

```css
justify-content: space-between;
```

The first item goes to the start, the last goes to the end, and the remaining space is distributed between them:

```text
Box 1          Box 2          Box 3
```

### 5. `space-around`

```css
justify-content: space-around;
```

Space is added **around each item**:

```text
  Box 1       Box 2       Box 3
```

### 6. `space-evenly`

```css
justify-content: space-evenly;
```

The spaces are **equal everywhere**:

```text
   Box 1      Box 2      Box 3
```

### Remember

```text
justify-content = position items along the main direction
```

For the default Flexbox direction:

```text
→ → → → →
```

So `justify-content` controls the **horizontal arrangement**.

Next is **`align-items`**, which controls the other direction.

---

## Flexbox — `align-items`

If `justify-content` controls the items **along the row**, `align-items` controls them in the **other direction**.

Think:

```text
justify-content
→ → → → →

align-items
↑
│
│
↓
```

### Example

```css
.container {
  display: flex;
  align-items: center;
}
```

Suppose the container is tall:

```text
┌─────────────────────────────┐
│                             │
│                             │
│      Box 1  Box 2  Box 3    │
│                             │
│                             │
└─────────────────────────────┘
              ↑
           center
```

The boxes are vertically centered.

### Common values

**`flex-start`** → items go to the top

```text
┌─────────────────────────────┐
│ Box 1  Box 2  Box 3         │
│                             │
│                             │
└─────────────────────────────┘
```

**`center`** → items go to the middle

```text
┌─────────────────────────────┐
│                             │
│ Box 1  Box 2  Box 3         │
│                             │
└─────────────────────────────┘
```

**`flex-end`** → items go to the bottom

```text
┌─────────────────────────────┐
│                             │
│                             │
│ Box 1  Box 2  Box 3         │
└─────────────────────────────┘
```

### The important difference

```text
justify-content → main direction
align-items     → cross direction
```

With the default Flexbox direction:

```text
justify-content → left ↔ right
align-items     → top ↕ bottom
```

So if you want to **center something both horizontally and vertically**, you commonly use:

```css
.container {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

This combination is extremely common in React and normal web development.

---

### `justify-content`

It controls **where the items are positioned along the main axis**.

With the default Flexbox direction (`row`), that means **left ↔ right**.

### `align-items`

It controls **where the items are positioned along the cross axis**.

With the default `row`, that means **top ↕ bottom**.

Think of it like this:

```text
          align-items
              ↕
              │
      Box 1  Box 2  Box 3
              │
              ↕
              
←────── justify-content ──────→
```

So:

**`justify-content` = item positioning horizontally**

**`align-items` = item positioning vertically**

One important correction: **neither one controls the container's direction.**

The property that controls direction is:

```css
flex-direction
```

For example:

```text
flex-direction: row    →  →  →
flex-direction: column
                         ↓
                         ↓
                         ↓
```

So remember:

* `display: flex` → turns on Flexbox
* `flex-direction` → decides **row or column**
* `justify-content` → positions items along the **main axis**
* `align-items` → positions items along the **cross axis**

---

# Flexbox — `flex-direction`

Now let's learn the property that **actually controls the direction**.

### `flex-direction: row`

This is the default.

Items go **left → right**:

```text
Box 1   Box 2   Box 3
  →       →       →
```

```css
.container {
  display: flex;
  flex-direction: row;
}
```

### `flex-direction: column`

Items go **top → bottom**:

```text
Box 1
  ↓
Box 2
  ↓
Box 3
```

```css
.container {
  display: flex;
  flex-direction: column;
}
```

### Why this is important

When you change `flex-direction`, the meaning of the axes changes.

#### `row`

```text
Main axis → → →
Cross axis ↕
```

So:

* `justify-content` → horizontal
* `align-items` → vertical

#### `column`

```text
Main axis ↕
Cross axis → 
```

So:

* `justify-content` → vertical
* `align-items` → horizontal

### Easy rule

Don't memorize "justify = horizontal" and "align = vertical."

Instead remember:

**`flex-direction` decides the main axis.**

**`justify-content` works on the main axis.**

**`align-items` works on the cross axis.**

This is the key idea that makes Flexbox much easier.

---

## Flexbox — Putting Everything Together

Now let's combine the main Flexbox properties you've learned.

```css
.container {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
}
```

Let's understand each one:

```text
display: flex
    ↓
Turn Flexbox on

flex-direction: row
    ↓
Items go left → right

justify-content: center
    ↓
Center items along the main axis

align-items: center
    ↓
Center items along the cross axis
```

So visually:

```text
┌───────────────────────────────┐
│                               │
│       Box 1  Box 2  Box 3     │
│                               │
└───────────────────────────────┘
```

### Now change direction

If we use:

```css
flex-direction: column;
```

The same `justify-content` and `align-items` behave according to the new axes:

```text
┌───────────────────────────────┐
│             Box 1             │
│               ↓               │
│             Box 2             │
│               ↓               │
│             Box 3             │
└───────────────────────────────┘
```

So the key Flexbox concept is:

**`flex-direction` → decides the direction**

**`justify-content` → controls the main axis**

**`align-items` → controls the cross axis**

You now understand the **core of Flexbox**.

Next in your roadmap is **CSS Grid**.

---

# 1. What is Grid?

**CSS Grid is used to arrange elements in rows AND columns.**

Think about a chessboard:

```text
┌───────┬───────┬───────┐
│ Box 1 │ Box 2 │ Box 3 │
├───────┼───────┼───────┤
│ Box 4 │ Box 5 │ Box 6 │
└───────┴───────┴───────┘
```

There are:

* **3 columns**
* **2 rows**

Grid is designed for this kind of layout.

---

## 2. Turn on Grid

HTML:

```html id="grz1s3"
<div class="container">
  <div>Box 1</div>
  <div>Box 2</div>
  <div>Box 3</div>
  <div>Box 4</div>
</div>
```

CSS:

```css id="zcvxw0"
.container {
  display: grid;
}
```

Just like:

```text
display: flex → Flexbox
display: grid → Grid
```

---

## 3. Create columns

Now tell Grid how many columns we want:

```css id="kqu3mj"
.container {
  display: grid;
  grid-template-columns: 1fr 1fr;
}
```

This means:

**Create 2 equal columns.**

Result:

```text
┌──────────┬──────────┐
│  Box 1   │  Box 2   │
├──────────┼──────────┤
│  Box 3   │  Box 4   │
└──────────┴──────────┘
```

`1fr 1fr` means:

```text
1 part + 1 part
```

So both columns get equal space.

---

## 4. Three columns

```css id="8xrlzq"
grid-template-columns: 1fr 1fr 1fr;
```

Result:

```text
┌────────┬────────┬────────┐
│ Box 1  │ Box 2  │ Box 3  │
├────────┼────────┼────────┤
│ Box 4  │ Box 5  │ Box 6  │
└────────┴────────┴────────┘
```

---

## Flexbox vs Grid

This is the main difference to remember:

### Flexbox

Usually focuses on **one direction**:

```text
Box 1 → Box 2 → Box 3
```

Good for:

* Navigation bars
* Buttons in a row
* Aligning items

### Grid

Works with **rows AND columns**:

```text
Box 1  Box 2  Box 3
Box 4  Box 5  Box 6
```

Good for:

* Card layouts
* Dashboards
* Image galleries
* Page layouts

### Easy memory

**Flexbox → one-dimensional**

**Grid → two-dimensional**

------------------------------------------------------------------------------------------------------------------------------------------

# CSS Positioning

## 1. `position: static`

This is the **default**.

```css
.box {
  position: static;
}
```

The element follows the normal document flow.

`top`, `right`, `bottom`, and `left` generally don't move a static element.

---

## 2. `position: relative`

The element **stays in the normal document flow**, but you can offset it.

```css
.box {
  position: relative;
  top: 20px;
  left: 10px;
}
```

Important:

The original space of the element is still preserved.

```text
Normal position
      ↓
   ┌───────┐
   │       │
   └───────┘
        ↘ moved visually
```

### Very important use

`relative` is commonly used as the positioning reference for an absolutely positioned child.

```css
.parent {
  position: relative;
}

.child {
  position: absolute;
  top: 0;
  right: 0;
}
```

---

# 3. `position: absolute`

The element is removed from the normal document flow.

```css
.child {
  position: absolute;
  top: 0;
  right: 0;
}
```

An absolutely positioned element looks for its **nearest positioned ancestor**.

Usually:

```css
.parent {
  position: relative;
}
```

Then:

```text
parent
┌─────────────────────────┐
│                     child
│                        ↓
│                     ┌─────┐
│                     │     │
│                     └─────┘
└─────────────────────────┘
```

This pattern is extremely common for:

* badges
* icons
* dropdowns
* overlays
* notification counters
* buttons positioned inside cards

---

# 4. `position: fixed`

The element is positioned relative to the **viewport**.

```css
.button {
  position: fixed;
  bottom: 20px;
  right: 20px;
}
```

It stays in that position even when the page scrolls.

Common examples:

```text
Chat button
Back-to-top button
Floating action button
Fixed navbar
```

---

# 5. `position: sticky`

`sticky` behaves like a normal element until a scrolling threshold is reached.

```css
header {
  position: sticky;
  top: 0;
}
```

Conceptually:

```text
Before scrolling
        ↓
normal element

Scroll
        ↓
reaches top: 0

        ↓

sticks to top
```

Common example:

```css
.navbar {
  position: sticky;
  top: 0;
}
```

---

# Position Summary

| Position   | Normal flow? | Common use                      |
| ---------- | ------------ | ------------------------------- |
| `static`   | Yes          | Default                         |
| `relative` | Yes          | Offset / positioning reference  |
| `absolute` | No           | Overlay / child positioning     |
| `fixed`    | No           | Fixed to viewport               |
| `sticky`   | Yes          | Sticky elements while scrolling |

The most important relationship to remember:

```text
relative parent
      ↓
absolute child
```

---

# 6. `z-index`

`z-index` controls the **stacking order** of overlapping elements.

Imagine:

```text
Element A
   ↓
Element B
```

If they overlap, `z-index` can determine which appears on top.

```css
.modal {
  z-index: 1000;
}
```

Higher stacking level generally appears above a lower one within the relevant stacking context.

Example:

```css
.box1 {
  position: relative;
  z-index: 1;
}

.box2 {
  position: relative;
  z-index: 2;
}
```

When they overlap:

```text
box2
  ↓
appears above
  ↓
box1
```

### Common mistake

`z-index: 999999` doesn't magically put an element above everything.

**Stacking contexts matter.**

For normal beginner/intermediate work, remember:

```text
position + z-index
        ↓
control overlapping elements
```

---

# 7. CSS Variables

CSS variables let you store reusable values.

Define one:

```css
:root {
  --primary-color: #2563eb;
}
```

Use it:

```css
button {
  background: var(--primary-color);
}
```

Now if you change:

```css
--primary-color
```

every place using it can update.

---

## Multiple Variables

```css
:root {
  --primary-color: #2563eb;
  --text-color: #111827;
  --background-color: #ffffff;
  --spacing: 16px;
}
```

Then:

```css
.card {
  color: var(--text-color);
  background: var(--background-color);
  padding: var(--spacing);
}
```

---

# 8. Variable Fallback

You can provide a fallback:

```css
color: var(--text-color, black);
```

Meaning:

```text
Does --text-color exist?
       ↓
   Yes → use it
   No  → use black
```

---

# 9. Variables + Themes

CSS variables become especially useful for themes.

```css
:root {
  --background: white;
  --text: black;
}
```

Dark theme:

```css
.dark {
  --background: #111;
  --text: white;
}
```

Then:

```css
body {
  background: var(--background);
  color: var(--text);
}
```

You can change the theme by changing the variables rather than rewriting every component.

---

# CSS Foundations — Complete

```text
[x] CSS3
[x] Box model
[x] Flexbox
[x] Grid
[x] position
[x] z-index
[x] CSS variables
```

----------------------------------------------------------------------------------------------------------------------------------------
