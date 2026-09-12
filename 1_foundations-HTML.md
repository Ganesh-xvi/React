# HyperText Markup Language


# 1. What is HTML?

HTML (**HyperText Markup Language**) is used to **structure content** on a webpage.

It doesn’t do logic (that’s JavaScript)
It doesn’t style (that’s CSS)
👉 It just defines *what is what*

Example:

* Heading → `<h1>`
* Paragraph → `<p>`
* Button → `<button>`

---

# 2. Basic Structure of an HTML Page

Every HTML page follows this structure:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>My First Page</title>
  </head>

  <body>
    <h1>Hello World</h1>
    <p>This is my first webpage</p>
  </body>
</html>
```

### Understand this:

* `<!DOCTYPE html>` → tells browser it's HTML5
* `<html>` → root of page
* `<head>` → metadata (not visible)
* `<body>` → everything visible on screen

---

# 3. Most Important Tags 

## Text

```html
<h1>Main Heading</h1>
<h2>Sub Heading</h2>
<p>This is a paragraph</p>
```

---

## Links & Images

```html
<a href="https://google.com">Go to Google</a>

<img src="image.jpg" alt="description">
```

---

## Lists

```html
<ul>
  <li>Item 1</li>
  <li>Item 2</li>
</ul>

<ol>
  <li>First</li>
  <li>Second</li>
</ol>
```

---

## Containers (IMPORTANT)


`<div>` means **division**.

It is a **container** used to group HTML elements together.

Example:

```html
<div>
  <h1>Hello</h1>
  <p>This is my paragraph</p>
</div>
```

Here, the `<div>` groups the `<h1>` and `<p>` together.

Think of it like a **box/container**:

**div = box that holds other elements**

In React, you will use `<div>` **a lot**.

---

## Buttons & Inputs

```html
<button>Click Me</button>

<input type="text" placeholder="Enter name">
```

---

# 4. Attributes

Example:

```html
<a href="https://google.com">Link</a>
<img src="img.jpg" alt="image">
```

* `href`, `src`, `alt` → attributes and 

* '</a>' = clickable link**

---

# 5.Semantic Tags (Important)

👉 Semantic = meaningful

So semantic tags are HTML tags that describe what the content means.

Instead of using only `<div>` elements everywhere, HTML provides **semantic tags** that describe the meaning or purpose of the content.


# Without Semantic Tags (just using `<div>`)

```html
<div>
  <div>My Website</div>
  <div>Home | About</div>
  <div>This is a blog post</div>
  <div>Copyright 2026</div>
</div>
```

👉 Problem:
Everything is just a `<div>`
We don’t know what each part represents.


# With Semantic Tags

```html
<header>Header section</header>
<nav>Navigation</nav>
<section>Main content</section>
<article>Blog post</article>
<footer>Footer</footer>
```

These are called **semantic HTML tags**.

They make your HTML code:

* Easier to read
* Easier to understand
* More organized
* Better for accessibility
* Easier for search engines to understand

## 1. `<header>`

`<header>` represents the **top or introductory part** of a page or section.

It can contain:

* Website logo
* Website name
* Main heading
* Introduction

Example:

```html
<header>
  <h1>My Website</h1>
</header>
```

Think:

**`header` → Top / Introduction**

---

## 2. `<nav>`

`<nav>` represents a **navigation area**.

It usually contains links that help users move around the website.

Example:

```html
<nav>
  <a href="/">Home</a>
  <a href="/about">About</a>
  <a href="/contact">Contact</a>
</nav>
```

Think:

**`nav` → Navigation**

---

## 3. `<section>`

`<section>` represents a **group of related content**.

For example, a profile page might have separate sections for your introduction and skills.

Example:

```html
<section>
  <h2>About Me</h2>
  <p>I am a developer.</p>
</section>

<section>
  <h2>My Skills</h2>
  <p>HTML, CSS, JavaScript</p>
</section>
```

Think:

**`section` → One meaningful part of the page**

---

## 4. `<article>`

`<article>` represents an **independent piece of content**.

For example:

* Blog post
* News article
* Product review
* Forum post

Example:

```html
<article>
  <h2>Learning HTML</h2>
  <p>HTML is the foundation of web development.</p>
</article>
```

Think:

**`article` → Independent content**

---

## 5. `<footer>`

`<footer>` represents the **bottom or closing area** of a page or section.

It can contain:

* Copyright information
* Contact information
* Useful links
* Privacy policy

Example:

```html
<footer>
  <p>© 2026 My Website</p>
</footer>
```

Think:

**`footer` → Bottom / Closing area**

---

## Easy Way to Remember

```text
<header>   → Top / Introduction
<nav>      → Navigation
<section>  → Group of related content
<article>  → Independent content
<footer>   → Bottom / Closing area
```

A typical webpage can be structured like this:

```html
<header>
  <h1>My Website</h1>
</header>

<nav>
  <a href="/">Home</a>
  <a href="/about">About</a>
</nav>

<section>
  <h2>My Blog</h2>

  <article>
    <h3>Learning HTML</h3>
    <p>HTML is the foundation of web development.</p>
  </article>
</section>

<footer>
  <p>© 2026 My Website</p>
</footer>
```

### Important Point

Semantic tags **do not automatically make the website look beautiful**.

They mainly describe **what the content means**.

For example:

```html
<header>...</header>
```

tells us:

> "This is the header."

Later, **CSS** is used to control how that header looks.

So remember:

**HTML → Structure and meaning**

**CSS → Appearance and design**

------------------------------------------------------------------------------------------------------------------------------------------