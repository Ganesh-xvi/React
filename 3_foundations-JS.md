# JavaScript ES6+


### JavaScript learning order

1.  [What is JavaScript?](#what-is-javascript)

2.  [Variables](#variables)
    - [let](#2-let)
    - [const](#1-const)
    - [var](#3-var)

3.  [Data Types](#data-types)

4.  [Operators](#operators)
    - [Arithmetic](#1-arithmetic-operators)
    - [Comparison](#2-comparison-operators)
    - [Logical](#3-logical-operators)

5.  [Conditions](#if--else)
    - [if](#1-simple-if)
    - [else if](#3-else-if)
    - [else](#2-if--else)

6.  [Ternary Operator](#ternary-operator)

7.  [Loops](#loops)
    - [for](#1-for-loop)
    - [while](#3-while-loop)
    - [for...of](#4for-of)

8.  [Functions](#functions)
    - [Parameters](#2-function-with-parameters)
    - [Arguments](#2-function-with-parameters)
    - [return](#3-function-with-return)

9.  [Default Parameters](#default-parameters)

10. [Arrays](#arrays)

11. [Objects](#objects)

12. [Array Methods](#array-methods)
    - [forEach](#1-foreach)
    - [map](#2-map)
    - [filter](#3-filter)
    - [reduce](#4-reduce)
    - [find](#5-find)
    - [some](#6-some)
    - [every](#7-every)
    - [includes](#8-includes)

13. [ES6+ Features](#es6-features)
    - [Arrow Functions](#arrow-functions)
    - [Destructuring](#destructuring)
    - [Spread / Rest](#spread-and-rest-)
    - [Template Literals](#template-literals)
    - [Optional Chaining](#1-optional-chaining-)
    - [Nullish Coalescing](#4-nullish-coalescing-)

14. [this Keyword](#this)

15. [Closures](#closures)

16. [Promises](#promises)

17. [async / await](#async--await)

18. [try / catch / finally](#javascript--lesson-9-try--catch--finally)

19. [JSON](#json)

20. [Modules](#modules)
    - [import](#2-import)
    - [export](#1-export)

21. [DOM](#dom)

22. [Fetch / API](#fetch-api)

------------------------------------------------------------------------------------------------------------------------------------------

# What is JavaScript?

**JavaScript (JS)** is a programming language used to add **behavior and logic** to a webpage.

Remember what we learned:

* **HTML** → structure
* **CSS** → appearance
* **JavaScript** → behavior/logic

### Example

HTML:

```html
<button>Click Me</button>
```

HTML creates the button.

CSS can make it look nice.

JavaScript can make something happen when you click it:

```text
Click Me
   ↓
User clicks
   ↓
JavaScript runs
   ↓
Something happens
```

For example:

> Click the button → show "Hello William"

---

# Where does JavaScript run?

In a web browser such as Chrome, Edge, or Firefox.

The browser has a **JavaScript engine** that reads and executes JavaScript.

So:

```text
HTML
 ↓
Structure

CSS
 ↓
Appearance

JavaScript
 ↓
Logic / Behavior
```

---

# Your first JavaScript

```javascript
console.log("Hello World");
```

`console.log()` means: 

- const is used to declare/create a variable whose binding cannot be reassigned.

> Print something to the browser's developer console.

So this:

```javascript
console.log("Hello World");
```

produces:

```text
Hello World
```

in the console.

---

# JavaScript can store information

For example:

```javascript
const name = "William";
```

Here:

* `const` → creates a variable
* `name` → variable name
* `"William"` → value

You can then use it:

```javascript
console.log(name);
```

Result:

```text
William
```

------------------------------------------------------------------------------------------------------------------------------------------

# Variables

A **variable** is a place where we store a value.

For example:

```javascript
const name = "William";
```

Think of it like a box:

```text
name
 ↓
┌─────────┐
│ William │
└─────────┘
```

JavaScript mainly has **3 ways** to create variables:

* `const`
* `let`
* `var`


For modern JavaScript, focus mainly on **`const` and `let`**.

---

## 1. `const`

Use `const` when the variable **should not be reassigned**.

```javascript
const name = "William";
```

You can use it:

```javascript
console.log(name);
```

Output:

```text
William
```

But you cannot do:

```javascript
name = "John";
```

because `name` was created with `const`.

Think:

**`const` → value cannot be reassigned**

---

## 2. `let`

Use `let` when the value **needs to change**.

```javascript
let age = 25;
```

Later:

```javascript
age = 26;
```

That's allowed.

Think:

**`let` → value can change**

---

## 3. `var`

`var` is the older way of declaring variables.

```javascript
var name = "William";
```

You may still see it in older JavaScript code, but in modern JavaScript, we generally use:

**`const` and `let`**

---

## Simple comparison

| Keyword | Can value change? | Modern JS   |
| ------- | ----------------- | ----------- |
| `const` | No reassignment   | Yes         |
| `let`   | Yes               | Yes         |
| `var`   | Yes               | Older style |

### Easy rule

When writing JavaScript:

**Start with `const`.**

If you know the value needs to change, use **`let`**.

```javascript
const name = "William";
let age = 25;

age = 26;
```

------------------------------------------------------------------------------------------------------------------------------------------

# Data types

A **data type** tells JavaScript **what kind of value** you are storing.

For example:

```javascript
const name = "William";
```

`"William"` is a **string**.

Let's look at the important types.

---

## 1. String

A **string** is text.

```javascript
const name = "William";
const city = "Chennai";
```

You can use:

```text
"Hello"
'Hello'
```

Both represent text.

Think:

**String → text**

---

## 2. Number

Numbers are used for numerical values.

```javascript
const age = 25;
const price = 100;
const temperature = 30.5;
```

Think:

**Number → numeric value**

---

## 3. Boolean

A Boolean has only **two values**:

```javascript
const isLoggedIn = true;
const isAdmin = false;
```

Think:

**Boolean → true or false**

This is very useful for conditions.

For example:

```text
isLoggedIn = true
       ↓
User is logged in
```

---

## 4. Undefined

A variable can exist without having a value.

```javascript
let name;
```

Its value is:

```text
undefined
```

Think:

**undefined → value hasn't been assigned**

---

## 5. Null

`null` means **intentionally empty**.

```javascript
const selectedUser = null;
```

Think:

**null → intentionally no value**

---

## 6. Array

An array stores **multiple values**.

```javascript
const skills = ["HTML", "CSS", "JavaScript"];
```

Think:

```text
skills
  ↓
┌─────────────────────────┐
│ HTML │ CSS │ JavaScript │
└─────────────────────────┘
```

We'll learn arrays in much more detail later.

---

## 7. Object

An object stores information as **key-value pairs**.

```javascript
const user = {
  name: "William",
  age: 25
};
```

Think:

```text
user
 ├── name → William
 └── age  → 25
```

Objects are extremely important in React.

---

## The important ones to remember

```text
String     → "William"
Number     → 25
Boolean    → true / false
Undefined  → no value assigned
Null       → intentionally empty
Array      → multiple values
Object     → related information
```

------------------------------------------------------------------------------------------------------------------------------------------

# Operators

**Operators** are symbols that tell JavaScript to perform an operation.

We'll start with the most common ones.

## 1. Arithmetic Operators

These are used for calculations.

```javascript
const a = 10;
const b = 5;
```

### Addition `+`

```javascript
a + b
```

Result:

```text
15
```

### Subtraction `-`

```javascript
a - b
```

Result:

```text
5
```

### Multiplication `*`

```javascript
a * b
```

Result:

```text
50
```

### Division `/`

```javascript
a / b
```

Result:

```text
2
```

### Remainder `%`

```javascript
a % b
```

Result:

```text
0
```

`%` gives you the **remainder** after division.

For example:

```javascript
10 % 3
```

Result:

```text
1
```

---

# 2. Comparison Operators

These are used to **compare values**.

### `===` Equal

```javascript
10 === 10
```

Result:

```text
true
```

But:

```javascript
10 === 5
```

Result:

```text
false
```

### `!==` Not equal

```javascript
10 !== 5
```

Result:

```text
true
```

### `>` Greater than

```javascript
10 > 5
```

Result:

```text
true
```

### `<` Less than

```javascript
10 < 5
```

Result:

```text
false
```

### `>=` Greater than or equal

```javascript
10 >= 10
```

Result:

```text
true
```

### `<=` Less than or equal

```javascript
5 <= 10
```

Result:

```text
true
```

---

# 3. Logical Operators

These are useful when you have **multiple conditions**.

### `&&` AND

Both conditions must be true.

```javascript
age > 18 && isLoggedIn === true
```

Think:

```text
Condition 1 AND Condition 2
       ↓            ↓
      true         true
           ↓
          true
```

### `||` OR

At least one condition must be true.

```javascript
age > 18 || isAdmin === true
```

### `!` NOT

Reverses true/false.

```javascript
!true
```

Result:

```text
false
```

---

## The important operators for now

```text
+   -   *   /   %
=== !==
>   <   >=  <=
&&  ||  !
```

One important thing to remember:

**`=` is assignment**

```javascript
const age = 25;
```

You're putting `25` into `age`.

**`===` is comparison**

```javascript
age === 25
```

You're asking:

> "Is age equal to 25?"

---

# `if` / `else`

Now we use the comparison operators to make **decisions**.

Think of it like:

> **If something is true → do this. Otherwise → do that.**

---

## 1. Simple `if`

```javascript id="ev0j1k"
const age = 20;

if (age >= 18) {
  console.log("You can vote");
}
```

Here:

```text id="4n5l3j"
age >= 18
   ↓
Is it true?
   ↓
Yes
   ↓
"You can vote"
```

If the condition is `true`, JavaScript executes the code inside `{ }`.

---

## 2. `if` + `else`

```javascript id="t2m0qm"
const age = 15;

if (age >= 18){console.log("You can vote");} 
else {console.log("You cannot vote");}
```

Since `15 >= 18` is false:

```text id="h6pmw6"
You cannot vote
```

Think:

```text id="t4k1fv"
        condition
            ↓
       ┌────┴────┐
      true      false
       ↓          ↓
      if         else
```

---

## 3. `else if`

Sometimes we have **more than two possibilities**.

```javascript id="5s5v4q"
const marks = 75;

if (marks >= 90) {
  console.log("A");
} else if (marks >= 60) {
  console.log("B");
} else {
  console.log("C");
}
```

Here JavaScript checks from **top to bottom**:

```text id="jz1f2q"
marks >= 90?
     ↓ No

marks >= 60?
     ↓ Yes

"B"
```

---

## 4. Using `&&`

You can combine conditions.

```javascript id="bq8kxh"
const age = 25;
const isLoggedIn = true;

if (age >= 18 && isLoggedIn === true) {
  console.log("Access granted");
}
```

Both conditions need to be true.

---

## 5. Using `||`

At least one condition needs to be true.

```javascript id="3g5hkm"
const isAdmin = false;
const isOwner = true;

if (isAdmin || isOwner) {
  console.log("Access granted");
}
```

Since `isOwner` is `true`, the condition succeeds.

---

### Easy way to remember

```text id="h1by23"
if       → if this is true
else if  → otherwise, check this
else     → if nothing above is true
```

And remember:

**`if` / `else` = decision making in JavaScript.**

------------------------------------------------------------------------------------------------------------------------------------------

# Ternary Operator

For example:

```javascript
const age = 20;

if (age >= 18) {
  console.log("Adult");
} else {
  console.log("Minor");
}
```

This works perfectly.

But if your condition is very simple, JavaScript gives us a shorter way: the **ternary operator**.

---

## 1. Basic syntax

```javascript
condition ? valueIfTrue : valueIfFalse
```

Think:

```text
condition
    ↓
  true? ─── Yes → first value
    │
    └─────── No  → second value
```

---

## 2. Example

The previous `if / else`:

```javascript
const age = 20;

if (age >= 18) {
  console.log("Adult");
} else {
  console.log("Minor");
}
```

Can be written as:

```javascript
const age = 20;

const result = age >= 18 ? "Adult" : "Minor";

console.log(result);
```

Because:

```text
age >= 18
   ↓
 true
   ↓
"Adult"
```

So:

```text
condition ? true result : false result
```

---

## 3. Another example

```javascript
const isLoggedIn = true;

const message = isLoggedIn ? "Welcome" : "Please login";
```

Since `isLoggedIn` is `true`:

```text
message → "Welcome"
```

If it were `false`:

```text
message → "Please login"
```

---

## 4. Easy way to remember

Ternary has **three parts**, which is why it is called *ternary*:

```javascript
condition ? true : false
```

For example:

```javascript
age >= 18 ? "Adult" : "Minor"
```

Read it like English:

> **Is age greater than or equal to 18? If yes, Adult; otherwise, Minor.**

---

## `if/else` vs ternary

### `if/else`

```javascript
if (age >= 18) {
  result = "Adult";
} else {
  result = "Minor";
}
```

### Ternary

```javascript
result = age >= 18 ? "Adult" : "Minor";
```

### Important

Use ternary when the decision is **simple**.

Don't try to replace every complicated `if/else` with ternary.

### Remember

```text
if / else
→ good for larger/multiple pieces of logic

ternary
→ good for simple true/false choices
```

------------------------------------------------------------------------------------------------------------------------------------------

# Loops

A **loop** is used when you want JavaScript to **repeat something**.

For example, instead of writing:

```text
Print Hello
Print Hello
Print Hello
Print Hello
Print Hello
```

you can use a loop to repeat it 5 times.

---

## 1. `for` loop

The most common loop to start with is the `for` loop.

```javascript
for (let i = 0; i < 5; i++) {
  console.log("Hello");
}
```
> i++ - (i = i + 1)

This prints:

```text
Hello
Hello
Hello
Hello
Hello
```

### Understand the three parts

```text
for (let i = 0; i < 5; i++)
     │          │      │
     │          │      └── increase i
     │          └───────── condition
     └──────────────────── starting value
```

> i++ - (i = i + 1)

Let's break it down:

### `let i = 0`

Start counting from `0`.

### `i < 5`

Keep running while `i` is less than `5`.

### `i++`

Increase `i` by `1` after each loop.

So:

```text
i = 0 → run
i = 1 → run
i = 2 → run
i = 3 → run
i = 4 → run
i = 5 → stop
```

---

# 2. Loop through an array

This is where loops become very useful.

```javascript
const skills = ["HTML", "CSS", "JavaScript"];
```

You can loop through the skills:

```javascript
for (let i = 0; i < skills.length; i++) {
  console.log(skills[i]);
}
```

Output:

```text
HTML
CSS
JavaScript
```

Here:

**`.length`** tells us how many items are in the array.

```text
skills.length
     ↓
     3
```

And:

```text
skills[0] → HTML
skills[1] → CSS
skills[2] → JavaScript
```

---

# 3. `while` loop

Another type of loop is `while`.

```javascript
let i = 0;

while (i < 5) {
  console.log(i);
  i++;
}
```

It means:

> **While this condition is true, keep running.**

---

# 4.for...of`

You already know the normal `for` loop:

```javascript
const skills = ["HTML", "CSS", "JavaScript"];

for (let i = 0; i < skills.length; i++) {
  console.log(skills[i]);
}
```

Output:

```text
HTML
CSS
JavaScript
```

`for...of` gives us a simpler way to go through the values of an array.

---

## 1. Basic `for...of`

```javascript
const skills = ["HTML", "CSS", "JavaScript"];

for (const skill of skills) {
  console.log(skill);
}
```

Output:

```text
HTML
CSS
JavaScript
```

Read it like:

> **For each `skill` of `skills`, run this code.**

---

## 2. How it works

Given:

```javascript
const skills = ["HTML", "CSS", "JavaScript"];
```

The loop does:

```text
First:
skill → "HTML"

Second:
skill → "CSS"

Third:
skill → "JavaScript"
```

So:

```javascript
for (const skill of skills)
```

means:

```text
Take each value from skills
        ↓
Put it into skill
        ↓
Run the code
```

---

## 3. Difference between `for` and `for...of`

### Normal `for`

```javascript
for (let i = 0; i < skills.length; i++) {
  console.log(skills[i]);
}
```

You work with the **index**:

```text
0 → HTML
1 → CSS
2 → JavaScript
```

### `for...of`

```javascript
for (const skill of skills) {
  console.log(skill);
}
```

You directly get the **value**:

```text
HTML
CSS
JavaScript
```

### `for...in` 

`for...in` is mainly used to **loop through the keys (property names) of an object**.

### Example

```javascript
const user = {
  name: "William",
  age: 25,
  city: "Chennai"
};

for (let key in user) {
  console.log(key);
}
```

Output:

```text
name
age
city
```

Here:

* `key` → gets the property name
* `user` → the object we're looping through

### Get both key and value

```javascript
for (let key in user) {
  console.log(key, user[key]);
}
```

Output:

```text
name William
age 25
city Chennai
```

The important part is:

```javascript
user[key]
```

If `key` is `"name"`:

```javascript
user["name"]
```

gives:

```text
William
```

### `for...in` vs `for...of`

This is important:

**`for...in` → object keys**

```javascript
const user = {
  name: "William",
  age: 25
};

for (let key in user) {
  console.log(key);
}
```

Output:

```text
name
age
```

**`for...of` → values of an iterable like an array**

```javascript
const skills = ["HTML", "CSS", "JavaScript"];

for (let skill of skills) {
  console.log(skill);
}
```

Output:

```text
HTML
CSS
JavaScript
```

So remember:

> **`for...in` → keys**
> **`for...of` → values**


So remember:

```text
for
→ gives you index

for...of
→ gives you value
```

---

## 4. `for...of` with an array of objects

This is very useful for API data.

```javascript
const users = [
  { name: "William", age: 25 },
  { name: "John", age: 30 }
];

for (const user of users) {
  console.log(user.name);
}
```

Output:

```text
William
John
```

Because each `user` is an object:

```text
First user
→ { name: "William", age: 25 }

Second user
→ { name: "John", age: 30 }
```

---

## One important point

Don't confuse:

```javascript
for...of
```

with:

```javascript
for...in
```

For now, remember:

```text
for...of
→ values

for...in
→ keys / property names
```

For arrays, **`for...of`** is generally the one you'll want when you simply need each value.

### Simple memory trick

**`of` → values**

```javascript
for (const item of items)
```
---

## What you need to remember now

There are several types of loops, but start with these:

```text
for    → repeat a known number of times
while  → repeat while a condition is true
```

Later, when we learn **arrays**, you'll learn easier ways to loop through arrays, such as:

`for...of`, `forEach()`, `map()`, and more.


------------------------------------------------------------------------------------------------------------------------------------------

# Default Parameters

You already know that functions can receive **parameters**.

For example:

```javascript
function greet(name) {
  console.log("Hello " + name);
}

greet("William");
```

Output:

```text
Hello William
```

But what happens if we don't provide a value?

```javascript
greet();
```

The result would be:

```text
Hello undefined
```

That's where **default parameters** are useful.

---

## 1. What is a default parameter?

A default parameter gives a parameter a **default value** if the caller doesn't provide one.

```javascript
function greet(name = "William") {
  console.log("Hello " + name);
}
```

Now:

```javascript
greet();
```

Output:

```text
Hello William
```

But if you provide a value:

```javascript
greet("John");
```

Output:

```text
Hello John
```

So:

```text
Value provided?
      ↓
    Yes → use provided value
      ↓
    No → use default value
```

---

## 2. Another example

```javascript
function calculatePrice(price, tax = 10) {
  return price + tax;
}
```

If you do:

```javascript
calculatePrice(100);
```

JavaScript uses:

```text
price = 100
tax = 10
```

Result:

```text
110
```

But you can provide your own tax:

```javascript
calculatePrice(100, 20);
```

Result:

```text
120
```

---

## 3. Important point

The default value is used when the parameter is **not provided** (or is `undefined`).

```javascript
function greet(name = "William") {
  console.log(name);
}
```

```javascript
greet();
```

→ `William`

```javascript
greet("John");
```

→ `John`

---

## Simple memory trick

```javascript
function greet(name = "William")
```

means:

> "If you don't give me `name`, I'll use `William`."

That's all you need to understand for now.

------------------------------------------------------------------------------------------------------------------------------------------


# Functions

A **function** is a reusable block of code that performs a task.

Think of it like a **machine**:

```text
Input → Function → Output
```

For example:

```text
2 + 3 → add function → 5
```

## 1. Creating a function

```javascript
function greet() {
  console.log("Hello William");
}
```

Here we created a function called `greet`.

But the function doesn't run just because we created it.

We need to **call** it:

```javascript
greet();
```

Output:

```text
Hello William
```

---

## 2. Function with parameters

A function can receive information.

```javascript
function greet(name) {
  console.log("Hello " + name);
}
```

Here `name` is a **parameter**.

We can call it:

```javascript
greet("William");
```

Output:

```text
Hello William
```

Or:

```javascript
greet("John");
```

Output:

```text
Hello John
```

So:

```text
function greet(name)
              ↑
          parameter
```

And:

```text
greet("William")
      ↑
    argument
```

**Parameter** = variable defined by the function.

**Argument** = actual value you pass to the function.


- Parameter is name and Argument is willam and function is greet

---

# 3. Function with `return`

A function can return a value.

```javascript
function add(a, b) {
  return a + b;
}
```

Now:

```javascript
const result = add(10, 5);
```

The function calculates:

```text
10 + 5
 ↓
15
```

So:

```text
result = 15
```

`return` means:

> **Send the result back from the function.**

---

# 4. Why functions are useful

Without a function:

```javascript
console.log(10 + 5);
console.log(20 + 5);
console.log(30 + 5);
```

With a function:

```javascript
function addFive(number) {
  return number + 5;
}
```

Now you can reuse it:

```text
addFive(10) → 15
addFive(20) → 25
addFive(30) → 35
```

That's the main idea:

**Function = reusable piece of code.**

---

## One important distinction

You've previously seen this:

```javascript
function add(a, b) {
  return a + b;
}
```

That's a **normal function**.

Later we'll learn:

```javascript
const add = (a, b) => a + b;
```

That's an **arrow function**.

Arrow functions are part of **ES6+**, so we'll cover them after you understand the JavaScript fundamentals.

------------------------------------------------------------------------------------------------------------------------------------------

# Arrays

An **array** is used to store **multiples in one variable**.

For example, instead of:

```javascript
const skill1 = "HTML";
const skill2 = "CSS";
const skill3 = "JavaScript";
```

we can use an array:

```javascript
const skills = ["HTML", "CSS", "JavaScript"];
```

Think of it like a list:

```text
skills
  ↓
┌────────┬────────┬────────────┐
│  HTML  │  CSS   │ JavaScript │
└────────┴────────┴────────────┘
```

## 1. Accessing array items

Arrays use an **index**.

Important: JavaScript starts counting from **0**.

```text
HTML       → index 0
CSS        → index 1
JavaScript → index 2
```

So:

```javascript
skills[0]
```

gives:

```text
HTML
```

And:

```javascript
skills[2]
```

gives:

```text
JavaScript
```

---

## 2. `.length`

`.length` tells you how many items are in the array.

```javascript
const skills = ["HTML", "CSS", "JavaScript"];

console.log(skills.length);
```

Result:

```text
3
```

---

## 3. Adding an item

`push()` adds an item to the **end**.

```javascript
skills.push("React");
```

Now:

```text
HTML
CSS
JavaScript
React
```

---

## 4. Removing the last item

`pop()` removes the **last item**.

```javascript
skills.pop();
```

Now:

```text
HTML
CSS
JavaScript
```

---

## 5. Arrays can contain different types

For example:

```javascript
const data = ["William", 25, true];
```

But usually, we keep related data together:

```javascript
const skills = ["HTML", "CSS", "JavaScript"];
```

---

## 6. Array + loop

You can combine what we learned earlier:

```javascript
const skills = ["HTML", "CSS", "JavaScript"];

for (let i = 0; i < skills.length; i++) {
  console.log(skills[i]);
}
```

Output:

```text
HTML
CSS
JavaScript
```

This is important because later we'll learn **array methods** such as `map()`, `filter()`, and `forEach()`, which are heavily used in React.

### Remember

**Array = collection/list of values**

```text
skills[0] → first item
skills[1] → second item
skills[2] → third item
```

**Index starts at `0`.**

------------------------------------------------------------------------------------------------------------------------------------------

# Objects

An **object** is used to store related information together using **key-value pairs**.

For example, a person has a name, age, and skills.

Instead of:

```javascript
const name = "William";
const age = 25;
const city = "Chennai";
```

We can group them:

```javascript
const user = {
  name: "William",
  age: 25,
  city: "Chennai"
};
```

Think of it like:

```text
user
 ├── name → William
 ├── age  → 25
 └── city → Chennai
```

Here:

* `name` → **key**
* `"William"` → **value**
* `age` → **key**
* `25` → **value**

---

## 1. Accessing object values

You can use **dot notation**:

```javascript
user.name
```

Result:

```text
William
```

And:

```javascript
user.age
```

Result:

```text
25
```

---

## 2. Changing a value

Objects created with `const` can still have their properties changed.

```javascript
user.age = 26;
```

Now:

```text
age → 26
```

This is different from reassigning the whole variable.

---

## 3. Adding a new property

You can add another property:

```javascript
user.job = "Developer";
```

Now:

```text
user
 ├── name → William
 ├── age  → 26
 ├── city → Chennai
 └── job  → Developer
```

---

## 4. Object with an array

Objects can contain arrays:

```javascript
const user = {
  name: "William",
  skills: ["HTML", "CSS", "JavaScript"]
};
```

You can access the array:

```javascript
user.skills
```

And a specific skill:

```javascript
user.skills[0]
```

Result:

```text
HTML
```

---

## 5. Array of objects

This is **very important for React**.

You can have multiple users:

```javascript
const users = [
  {
    name: "William",
    age: 25
  },
  {
    name: "John",
    age: 30
  }
];
```

Think:

```text
users
 ├── User 1
 │    ├── name → William
 │    └── age  → 25
 │
 └── User 2
      ├── name → John
      └── age  → 30
```

Later, when you learn `map()`, you'll frequently work with **arrays of objects** like this.

### Remember

**Array → list of values**

```text
["HTML", "CSS", "JavaScript"]
```

**Object → related information using key-value pairs**

```text
{
  name: "William",
  age: 25
}
```

------------------------------------------------------------------------------------------------------------------------------------------

# Arrow Functions

Now we're entering the **ES6+** part of your roadmap.

You already learned normal functions:

```javascript id="xgkm4g"
function add(a, b) {
  return a + b;
}
```

An **arrow function** is another way to write a function.

```javascript id="0x4y0s"
const add = (a, b) => {
  return a + b;
};
```

Both do the same thing.

---

## 1. Normal function vs Arrow function

### Normal function

```javascript id="b0w4go"
function add(a, b) {
  return a + b;
}
```

### Arrow function

```javascript id="t0f0fr"
const add = (a, b) => {
  return a + b;
};
```

Think:

```text id="bubt78"
function add(a, b) { ... }

          ↓ becomes

const add = (a, b) => { ... }
```

---

## 2. Arrow function with one parameter

If there is only **one parameter**, parentheses can be removed:

```javascript id="o1r7p8"
const greet = name => {
  console.log("Hello " + name);
};
```

Call it:

```javascript id="b4fsgg"
greet("William");
```

Result:

```text id="b6o8wq"
Hello William
```

---

## 3. Short arrow function

If the function only returns one expression, you can make it shorter.

Instead of:

```javascript id="f4p1g0"
const add = (a, b) => {
  return a + b;
};
```

You can write:

```javascript id="ujf5gj"
const add = (a, b) => a + b;
```

The result is the same.

```text id="z1u6d4"
add(10, 5)
   ↓
  15
```

---

## Why are arrow functions important in React?

You'll see them **everywhere** in React.

For example:

```javascript id="7jpmnj"
const handleClick = () => {
  console.log("Button clicked");
};
```

And later with array methods:

```javascript id="q1g9o7"
skills.map(skill => ...)
```

So it's important to become comfortable with this syntax.

### Remember

```text id="g8vknh"
Normal:

function add(a, b) {
  return a + b;
}


Arrow:

const add = (a, b) => a + b;
```

**Arrow function = another way to write a function.**

------------------------------------------------------------------------------------------------------------------------------------------

# Destructuring

**Destructuring** means taking values out of an **array or object** and putting them into variables.

It sounds complicated, but the idea is simple.

---

## 1. Array destructuring

Suppose we have:

```javascript
const skills = ["HTML", "CSS", "JavaScript"];
```

Normally, we access them like:

```javascript
skills[0]
skills[1]
skills[2]
```

With destructuring:

```javascript
const [first, second, third] = skills;
```

Now:

```text
first  → HTML
second → CSS
third  → JavaScript
```

So:

```javascript
console.log(first);
```

gives:

```text
HTML
```

### Think of it like:

```text
["HTML", "CSS", "JavaScript"]
     ↓       ↓        ↓
   first   second    third
```

---

# 2. Object destructuring

Suppose we have:

```javascript
const user = {
  name: "William",
  age: 25,
  city: "Chennai"
};
```

Normally:

```javascript
user.name
user.age
user.city
```

With destructuring:

```javascript
const { name, age, city } = user;
```

Now you can directly use:

```javascript
console.log(name);
console.log(age);
console.log(city);
```

Result:

```text
William
25
Chennai
```

### Think:

```text
user
 ├── name → William
 ├── age  → 25
 └── city → Chennai

       ↓ destructuring

name
age
city
```

---

# 3. Why is this important in React?

You'll see destructuring very often with **props**.

For example:

```javascript
function User({ name, age }) {
  return <p>{name} - {age}</p>;
}
```

Here:

```text
{name, age}
```

is destructuring the props object.

So don't worry if this feels new. You'll get lots of practice with it in React.

### Remember

**Array:**

```javascript
const [a, b] = array;
```

**Object:**

```javascript
const { name, age } = user;
```

The main idea:

> **Destructuring = take values out and put them into variables.**

------------------------------------------------------------------------------------------------------------------------------------------

# Spread and Rest (`...`)

The `...` operator is called **spread** or **rest**, depending on how you use it.

The same `...` symbol has **two different purposes**.

## 1. Spread — "spread out" values

Suppose:

```javascript
const skills = ["HTML", "CSS"];
```

You can create another array and copy those values:

```javascript
const newSkills = [...skills, "JavaScript"];
```

Result:

```text
newSkills
→ ["HTML", "CSS", "JavaScript"]
```

Think:

```text
skills
   ↓
...skills
   ↓
HTML   CSS
   ↓
HTML   CSS   JavaScript
```

So:

**Spread = take the values out and spread/copy them.**

---

## 2. Spread with objects

```javascript
const user = {
  name: "William",
  age: 25
};
```

You can create another object:

```javascript
const newUser = {
  ...user,
  city: "Chennai"
};
```

Result:

```text
newUser
├── name → William
├── age  → 25
└── city → Chennai
```

This is **very common in React** when updating state.

---

# 3. Rest — "collect" values

Rest does the opposite.

```javascript
function add(...numbers) {
  console.log(numbers);
}
```

If we call:

```javascript
add(10, 20, 30);
```

The `...numbers` collects all the values into an array:

```text
numbers
→ [10, 20, 30]
```

So:

**Rest = collect multiple values into one variable.**

---

## Easy way to remember

```text
Spread → spread values OUT

Rest   → collect values IN
```

Both use:

```text
...
```

The **context tells you whether it is spread or rest**.

For now, just remember:

**`...` before an existing array/object → usually spread**

**`...` in a function parameter → rest**

> Spread is used to copy/spread values from an existing array into a new array.

- Spread is not only for copying arrays. It can also copy/spread objects.

> Rest collects multiple values into an array.

------------------------------------------------------------------------------------------------------------------------------------------

# Template Literals

Template literals are a cleaner way to create strings, especially when you want to put **variables inside text**.

---

## 1. Normal string

You already know:

```javascript
const name = "William";

console.log("Hello " + name);
```

Output:

```text
Hello William
```

This uses `+` to join strings.

---

## 2. Template literal

Instead, we can use **backticks**:

```javascript
const name = "William";

console.log(`Hello ${name}`);
```

Output:

```text
Hello William
```

Notice:

```text
"Hello " + name
```

becomes:

```text
`Hello ${name}`
```

---

## 3. `${}`

This part:

```javascript
${name}
```

means:

> Put the value of `name` here.

Example:

```javascript
const name = "William";
const age = 25;

console.log(`My name is ${name} and I am ${age} years old.`);
```

Output:

```text
My name is William and I am 25 years old.
```

---

## 4. You can use expressions too

You can put JavaScript expressions inside `${}`.

```javascript
const a = 10;
const b = 20;

console.log(`Total: ${a + b}`);
```

Output:

```text
Total: 30
```

---

## 5. Multiple lines

Template literals are also useful when you need multiple lines.

```javascript
const message = `
Hello William
Welcome to JavaScript
Keep learning
`;
```

You don't need to use `\n` for every new line.

---

# Why are template literals useful in our Todo app?

Later, when we work with todo data, we might have:

```javascript
const todo = "Learn JavaScript";
```

We can create text like:

```javascript
const message = `Todo: ${todo}`;
```

Result:

```text
Todo: Learn JavaScript
```

---

## Remember

### Normal string:

```javascript
"Hello " + name
```

### Template literal:

```javascript
`Hello ${name}`
```

The important things are:

```text
Backticks → ` `
Variable   → ${variable}
```

**Template literal = easier way to build strings with variables and expressions.**

------------------------------------------------------------------------------------------------------------------------------------------

# Optional Chaining `?.` and Nullish Coalescing `??`

These two are often used together, but they solve **different problems**.

---

# 1. Optional Chaining `?.`

Imagine we have an object:

```javascript
const user = {
  name: "William",
  address: {
    city: "Chennai"
  }
};
```

We can access:

```javascript
user.address.city
```

Result:

```text
Chennai
```

But what if `address` doesn't exist?

```javascript
const user = {
  name: "William"
};
```

This would cause an error:

```javascript
user.address.city
```

Because:

```text
user
 ↓
address
 ↓
❌ doesn't exist
 ↓
city → error
```

### Optional chaining solves this:

```javascript
user.address?.city
```

It means:

> "If `address` exists, give me `city`. If it doesn't exist, don't throw an error."

The result would be:

```text
undefined
```

---

# 2. Another example

```javascript
const user = {
  name: "William"
};

console.log(user.address?.city);
```

Instead of an error:

```text
undefined
```

So:

**`?.` = safely access something that might not exist.**

---

# 3. Optional chaining with functions

It can also check whether a function exists.

```javascript
user.login?.();
```

Meaning:

> If `login` exists and is callable, execute it.

Otherwise, don't throw an error.

---

# 4. Nullish Coalescing `??`

Now suppose we have:

```javascript
const username = null;
```

We want a default value if `username` doesn't exist.

We can use:

```javascript
const name = username ?? "Guest";
```

Result:

```text
Guest
```

Think:

```text
username
   ↓
null?
   ↓
Yes
   ↓
use "Guest"
```

---

# 5. What values does `??` check?

`??` uses the value on the right only when the left side is:

```text
null
undefined
```

Example:

```javascript
const name = undefined ?? "Guest";
```

Result:

```text
Guest
```

But:

```javascript
const name = "William" ?? "Guest";
```

Result:

```text
William
```

---

# 6. Important difference between `??` and `||`

This is important.

With `||`:

```javascript
const value = 0 || 100;
```

Result:

```text
100
```

Because `0` is considered falsy.

With `??`:

```javascript
const value = 0 ?? 100;
```

Result:

```text
0
```

Because `0` is **not** `null` or `undefined`.

So:

```text
|| → checks for falsy values

?? → checks only null or undefined
```

---

# 7. Using both together

This is very common:

```javascript
const city = user.address?.city ?? "Unknown";
```

Read it as:

> "Try to get the user's city. If the address or city doesn't exist, use `Unknown`."

Flow:

```text
user
 ↓
address?
 ↓
city?
 ↓
exists → use city
doesn't exist → "Unknown"
```

---

## Remember this

### `?.`

**Safely access something that might not exist.**

```javascript
user.address?.city
```

### `??`

**Use a fallback when the value is `null` or `undefined`.**

```javascript
city ?? "Unknown"
```

Together:

```javascript
user.address?.city ?? "Unknown"
```

------------------------------------------------------------------------------------------------------------------------------------------

# Array Methods

Now we'll learn one of the **most important parts of JavaScript for React**.

You already know arrays:

```javascript
const skills = ["HTML", "CSS", "JavaScript"];
```

JavaScript gives us methods to work with these arrays.

We'll learn:

1. `forEach()`
2. `map()`
3. `filter()`
4. `reduce()`

Let's start with **`forEach()`**.

---

# 1. `forEach()`

`forEach()` means:

> **Go through every item in the array and do something.**

Example:

```javascript
const skills = ["HTML", "CSS", "JavaScript"];

skills.forEach(skill => {
  console.log(skill);
});
```

Output:

```text
HTML
CSS
JavaScript
```

Think:

```text
skills
  ↓
HTML        → console.log()
CSS         → console.log()
JavaScript  → console.log()
```

So `forEach()` is useful when you simply want to **perform an action for every item**.

> Go through each item in an array, one by one, and perform some action.

---

# 2. `map()`

`map()` is different.

It goes through every item and **creates a new array**.

Example:

```javascript
const numbers = [1, 2, 3];

const doubled = numbers.map(number => number * 2);
```

Result:

```text
numbers
→ [1, 2, 3]

doubled
→ [2, 4, 6]
```

Think:

```text
1 → 2
2 → 4
3 → 6
```

So:

**`map()` = transform every item into something new.**

This is **very important in React**, because you'll use `map()` to display lists of components.

---

# 3. `filter()`

`filter()` means:

> **Keep only the items that satisfy a condition.**

Example:

```javascript
const numbers = [1, 2, 3, 4, 5];

const evenNumbers = numbers.filter(number => number % 2 === 0);
```

Result:

```text
evenNumbers
→ [2, 4]
```

Think:

```text
1 → ❌
2 → ✅
3 → ❌
4 → ✅
5 → ❌
```

So:

**`filter()` = select certain items.**

---

# 4. `reduce()`

`reduce()` is used to **combine many values into one value**.

Example:

```javascript
const numbers = [1, 2, 3, 4];

const total = numbers.reduce((sum, number) => {
  return sum + number;
}, 0);
```

Result:

```text
total
→ 10
```

Think:

```text
1 + 2 + 3 + 4
      ↓
     10
```

So:

**`reduce()` = reduce many values into one result.**

---

# The important difference

This is worth remembering:

```text
forEach()
→ Do something with every item

map()
→ Transform every item → new array

filter()
→ Keep some items → new array

reduce()
→ Combine items → one result
```

### Simple example

Suppose:

```javascript
const numbers = [1, 2, 3, 4, 5];
```

```text
forEach → print each number

map     → [2, 4, 6, 8, 10]

filter  → [2, 4]

reduce  → 15
```

For React, **`map()` is especially important**. We'll come back to it when we start React.


---

## 5. `find()`

`find()` means:

> **Find the first item that matches a condition.**

Example:

```javascript
const numbers = [10, 20, 30, 40];

const result = numbers.find(number => number > 20);
```

Result:

```text
30
```

Why?

```text
10 → ❌
20 → ❌
30 → ✅ ← STOP
40 → not checked
```

So remember:

**`find()` → returns the first matching item.**

---

## 6. `some()`

`some()` asks:

> **Does at least one item match the condition?**

Example:

```javascript
const numbers = [1, 3, 5, 8];

const result = numbers.some(number => number % 2 === 0);
```

Result:

```text
true
```

Because `8` is even.

Think:

```text
1 → ❌
3 → ❌
5 → ❌
8 → ✅

At least ONE? → true
```

So:

**`some()` → "Is there at least one?"**

---

## 7. `every()`

`every()` asks:

> **Do ALL items match the condition?**

Example:

```javascript
const numbers = [2, 4, 6, 8];

const result = numbers.every(number => number % 2 === 0);
```

Result:

```text
true
```

Because every number is even.

But:

```javascript
const numbers = [2, 4, 7, 8];

const result = numbers.every(number => number % 2 === 0);
```

Result:

```text
false
```

Because `7` is not even.

Think:

```text
2 → ✅
4 → ✅
7 → ❌

EVERY? → false
```

So:

**`every()` → "Do all of them satisfy the condition?"**

---

# 8. `includes()`

`includes()` asks:

> **Does this array contain this value?**

Example:

```javascript
const skills = ["HTML", "CSS", "JavaScript"];

const result = skills.includes("JavaScript");
```

Result:

```text
true
```

Because `"JavaScript"` exists in the array.

If:

```javascript
const result = skills.includes("Python");
```

Result:

```text
false
```

---

# Important difference

Remember these four like this:

```text
find()
→ Give me the first matching item

some()
→ Is at least one item matching?

every()
→ Are all items matching?

includes()
→ Does this exact value exist?
```

### Example

Given:

```javascript
const numbers = [10, 20, 30, 40];
```

```text
find(number > 20)
→ 30

some(number > 35)
→ true

every(number > 5)
→ true

includes(30)
→ true
```

These methods are **very commonly used in real JavaScript and React applications**, so make sure you understand the difference rather than just memorizing the names.

------------------------------------------------------------------------------------------------------------------------------------------

# `this`

`this` is confusing at first, so don't try to memorize complicated rules.

The simplest way to start is:

> **`this` refers to the object that is calling the function.**

### Example

```javascript
const user = {
  name: "William",

  greet() {
    console.log(this.name);
  }
};

user.greet();
```

Output:

```text
William
```

Why?

We called:

```javascript
user.greet();
```

So inside `greet()`:

```javascript
this
```

refers to:

```text
user
```

Therefore:

```javascript
this.name
```

means:

```javascript
user.name
```

---

### Think of it like this

```text
user
 ├── name → "William"
 │
 └── greet()
       ↓
     this
       ↓
     user
```

So:

```javascript
this.name
```

→ `"William"`

---

# Another example

```javascript
const person = {
  name: "John",

  sayName() {
    console.log(this.name);
  }
};

person.sayName();
```

Output:

```text
John
```

Because:

```text
person.sayName()
       ↓
     this
       ↓
    person
```

---

# Important: `this` is not the function itself

For example:

```javascript
const user = {
  name: "William",

  greet() {
    console.log(this);
  }
};
```

When you call:

```javascript
user.greet();
```

`this` refers to the `user` object.

So conceptually:

```text
this
 ↓
{
  name: "William",
  greet: ...
}
```

---

# One important difference: Arrow functions

Arrow functions behave differently with `this`.

For now, remember:

```javascript
const user = {
  name: "William",

  greet: () => {
    console.log(this.name);
  }
};
```

You **shouldn't use an arrow function here** if you expect `this` to refer to `user`.

Instead:

```javascript
const user = {
  name: "William",

  greet() {
    console.log(this.name);
  }
};
```

For normal object methods, this is the easier pattern to understand.

---

## The simple rule for now

When you see:

```javascript
object.method();
```

inside `method()`:

```javascript
this
```

usually refers to:

```javascript
object
```

Example:

```javascript
user.greet();
```

```text
user → this
```

That's the foundation of `this`. There are more cases—especially with regular functions, arrow functions, classes, and event handlers.

------------------------------------------------------------------------------------------------------------------------------------------

# Closures

Closures are one of the more confusing JavaScript concepts, so let's keep it simple.

First, remember **scope**.

## 1. Scope

A variable created inside a function normally belongs to that function.

```javascript
function greet() {
  const name = "William";

  console.log(name);
}
```

You can use `name` inside `greet()`.

But outside:

```javascript
console.log(name);
```

you cannot access it.

Think:

```text
greet()
│
└── name
    ↓
  accessible here

outside
    ↓
❌ cannot access name
```

---

# 2. Now the interesting part: Closure

A **closure** happens when an inner function remembers variables from its outer function, even after the outer function has finished.

Example:

```javascript
function outer() {
  const name = "William";

  function inner() {
    console.log(name);
  }

  return inner;
}
```

Now:

```javascript
const result = outer();
```

The `outer()` function has finished.

Normally you might think:

```text
outer() finished
     ↓
name should disappear
```

But:

```javascript
result();
```

still prints:

```text
William
```

Why?

Because the inner function **remembers `name`**.

That's a closure.

---

# 3. Think of it like a backpack

Imagine the inner function is carrying a backpack.

```text
outer()
│
├── name = "William"
│
└── inner()
       │
       └── 🎒 remembers name
```

When `outer()` finishes:

```text
outer() finishes
      ↓
inner function still exists
      ↓
inner remembers name
      ↓
"William"
```

That "remembering" is the important idea behind **closures**.

---

# 4. A more useful example

Closures are often used to keep data private.

```javascript
function counter() {
  let count = 0;

  return function () {
    count++;
    console.log(count);
  };
}
```

Now:

```javascript
const increment = counter();
```

Every time we call:

```javascript
increment();
```

we get:

```text
1
```

Again:

```javascript
increment();
```

gives:

```text
2
```

And:

```javascript
increment();
```

gives:

```text
3
```

Why doesn't `count` reset to `0`?

Because the returned function **remembers the `count` variable**.

```text
counter()
   ↓
count = 0
   ↓
returns function
   ↓
function remembers count
   ↓
increment()
   ↓
count = 1
   ↓
increment()
   ↓
count = 2
   ↓
increment()
   ↓
count = 3
```

---

# 5. Why is this called a closure?

Because the inner function "closes over" the variables from its surrounding scope.

So the easiest definition is:

> **A closure is a function that remembers variables from the scope where it was created.**

Don't worry about memorizing the technical definition yet.

Just remember:

```text
Inner function
     +
Outer function's variables
     +
Remembers them
     ↓
Closure
```

Functions and callbacks often **capture variables from their surrounding scope**. Understanding closures will become especially useful when we get into React.

------------------------------------------------------------------------------------------------------------------------------------------

# Promises

A **Promise** is used when JavaScript has to wait for something that will finish **later**.

A common example is getting data from an API.

Imagine:

```text
You → Ask server for data
             ↓
          Server
             ↓
       Takes some time
             ↓
       Data comes back
```

JavaScript doesn't want to freeze the whole application while waiting.

A Promise represents:

> **"I don't have the result yet, but I'll give you the result when it's ready."**

---

## 1. Promise has 3 states

A Promise can be:

```text
Pending   → still waiting
Fulfilled → completed successfully
Rejected  → something went wrong
```

Think:

```text
          Promise
             │
       ┌─────┴─────┐
       ↓           ↓
   Fulfilled    Rejected
   success       error
```

---

## 2. Simple Promise

```javascript id="qym5tb"
const promise = new Promise((resolve, reject) => {
  resolve("Success");
});
```

Here:

* `resolve()` → operation succeeded
* `reject()` → operation failed

---

## 3. Using `.then()`

> .then = run this function once the promise before it finishes successfully, using its result.

To get the successful result:

```javascript id="y3grn4"
promise.then(result => {
  console.log(result);
});
```

Output:

```text id="i4q5q3"
Success
```

Think:

```text
Promise
   ↓
completed
   ↓
.then()
   ↓
get result
```

---

## 4. Handling errors with `.catch()`

```javascript id="6m1brp"
promise
  .then(result => {
    console.log(result);
  })
  .catch(error => {
    console.log(error);
  });
```

If something goes wrong, `.catch()` handles the error.

---

# Why do we need Promises?

Suppose you request user data from a server:

```text id="x9qj1v"
Your website
     ↓
   Request
     ↓
   Server
     ↓
  Waiting...
     ↓
   Response
     ↓
Your website
```

The response doesn't come immediately.

Promises allow JavaScript to handle this **asynchronous work**.

You'll see Promises when working with:

* APIs
* Fetch
* Database requests
* File operations
* Timers
* React applications

---

# Promise + API

Later you'll write something like:

```javascript id="w3lqcf"
fetch("https://example.com/users")
  .then(response => response.json())
  .then(data => {
    console.log(data);
  })
  .catch(error => {
    console.log(error);
  });
```

Don't worry about understanding all of that yet.

The important concept is:

**Promise = a value that will be available later.**


# In this code:

```javascript
const promise = new Promise((resolve, reject) => {
  resolve("Success");
});
```

`new` means:

> **Create a new object from something.**

Here, `Promise` is a built-in JavaScript object/type, and:

```javascript
new Promise(...)
```

creates a **new Promise object**.

Think of it like:

```text
Promise
   ↓
new Promise()
   ↓
creates a new Promise
```

### Simple example

You can also see `new` with other JavaScript objects:

```javascript
const date = new Date();
```

This creates a new `Date` object.

So for now, remember:

**`new` → create a new object/instance.**

And:

```javascript
new Promise(...)
```

→ **creates a new Promise.**

------------------------------------------------------------------------------------------------------------------------------------------

# `async` / `await`

`async` and `await` are used to work with **Promises** more easily.

You already learned:

```text
Promise → something that will finish later
```

`async/await` gives us a cleaner way to wait for that result.

---

## 1. `async`

When you put `async` before a function:

```javascript id="c5u1vp"
async function getData() {
  // code
}
```

you are saying:

> "This function works with asynchronous operations."

An `async` function always returns a **Promise**.

---

## 2. `await`

`await` means:

> **Wait for the Promise to finish and give me the result.**

Example:

```javascript id="90qj7w"
async function getData() {
  const result = await promise;
  console.log(result);
}
```

Think:

```text id="dbk5dl"
await promise
     ↓
Wait for Promise
     ↓
Promise finishes
     ↓
Get the result
```

---

# 3. `async/await` with `fetch`

This is where you'll see it commonly used.

```javascript id="x3l8g7"
async function getUsers() {
  const response = await fetch("https://example.com/users");
  const data = await response.json();

  console.log(data);
}
```

There are two things happening:

```text id="c2k3tq"
fetch()
  ↓
wait for server response
  ↓
response

response.json()
  ↓
wait for data conversion
  ↓
data
```

---

# 4. Handling errors

With `async/await`, we commonly use `try/catch`.

```javascript id="p72x0p"
async function getUsers() {
  try {
    const response = await fetch("https://example.com/users");
    const data = await response.json();

    console.log(data);
  } catch (error) {
    console.log(error);
  }
}
```

Think:

```text id="aq2y0h"
try
 ↓
Try to get the data

Success → use the data

Error
 ↓
catch
 ↓
handle the error
```

---

# Promise vs async/await

### Promise style

```javascript id="6u5yvs"
fetch(url)
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.log(error));
```

### Async/await style

```javascript id="1q7vst"
async function getData() {
  try {
    const response = await fetch(url);
    const data = await response.json();

    console.log(data);
  } catch (error) {
    console.log(error);
  }
}
```

Both are working with **Promises**.

`async/await` is generally easier to read, especially when there are multiple asynchronous steps.

### Remember

**`async` → makes a function asynchronous**

**`await` → waits for a Promise result**

**`try/catch` → handles errors**

---

Absolutely. Let's focus **only on `async` and `await`** and make it very simple.

## First: Why do we need `async` and `await`?

Imagine you order food.

```text
You → Order food
       ↓
    Wait...
       ↓
    Food arrives
       ↓
    You eat
```

While the food is being prepared, you don't want the whole restaurant to stop working.

JavaScript works similarly. Some operations take time:

* Calling an API
* Getting data from a server
* Reading a file
* Database operations

These are called **asynchronous operations**.

---

# 1. What does `async` mean?

When you write:

```javascript
async function getData() {
}
```

`async` tells JavaScript:

> **"This function will perform asynchronous work and will return a Promise."**

For example:

```javascript
async function getData() {
  return "Hello";
}
```

Even though we return `"Hello"`, the function actually returns a **Promise**.

Think:

```text
async function
      ↓
returns a Promise
```

That's the first thing to remember.

---

# 2. What does `await` mean?

`await` means:

> **"Wait for this Promise to finish, then give me its result."**

Example:

```javascript
async function getData() {
  const result = await somePromise;
}
```

Think:

```text
somePromise
     ↓
   await
     ↓
wait for result
     ↓
result available
```

---

# 3. Real example with `fetch`

Suppose we want to get users from a server:

```javascript
async function getUsers() {
  const response = await fetch("https://example.com/users");

  console.log(response);
}
```

Let's understand this line:

```javascript
const response = await fetch(...);
```

### Step 1

```javascript
fetch(...)
```

asks the server for data.

### Step 2

`fetch()` returns a **Promise**.

```text
fetch()
  ↓
Promise
```

### Step 3

`await` waits for that Promise to complete.

```text
fetch()
  ↓
Promise
  ↓
await
  ↓
server response
```

### Step 4

The response is stored in:

```javascript
const response
```

So:

```text
fetch() → Promise → await → response
```

---

# 4. Why must `await` be inside `async`?

Usually, when using `await` inside a function, that function needs to be marked `async`.

Correct:

```javascript
async function getUsers() {
  const response = await fetch(url);
}
```

Think:

```text
async
 ↓
"This function can use await"
```

Then:

```text
await
 ↓
"Wait for this Promise"
```

---

# 5. The easiest way to remember

Think about these two words:

### `async`

**"This function does asynchronous work."**

### `await`

**"Wait for this Promise result."**

So:

```javascript
async function getUsers() {
  const response = await fetch(url);
}
```

means:

> **"This function is asynchronous. Inside it, wait for the fetch operation to finish, then continue."**

---

# 6. One important point

`await` does **not** mean:

> "Stop the entire JavaScript application."

It means the **async function pauses at that point** until the Promise settles, while JavaScript can continue handling other work.

For example:

```text
getUsers()
   ↓
await fetch()
   ↓
waiting for server...

Meanwhile JavaScript can handle:
- button clicks
- animations
- other tasks
```

Once the server responds:

```text
fetch completes
     ↓
await gets the result
     ↓
getUsers() continues
```

---

## Final picture

```text
async function
      ↓
function can work with asynchronous operations
      ↓
    await
      ↓
wait for a Promise
      ↓
Promise finishes
      ↓
get the result
      ↓
continue the function
```

### One-line memory trick:

**`async` = "this function works asynchronously"**

**`await` = "wait for this Promise's result"**

And this is why you often see them together:

```javascript
async function getData() {
  const response = await fetch(url);
}
```

------------------------------------------------------------------------------------------------------------------------------------------

# `try / catch / finally`

These are used for **error handling**.

When JavaScript runs code, sometimes something goes wrong.

For example:

```javascript
const user = null;

console.log(user.name);
```

This causes an error because `user` is `null`.

We don't always want an error to completely break our application.

That's where `try`, `catch`, and `finally` come in.

---

## 1. `try`

`try` means:

> **Try to run this code.**

```javascript
try {
  console.log("Hello");
}
```

If everything works, the code runs normally.

---

## 2. `catch`

`catch` means:

> **If an error happens inside `try`, handle it here.**

Example:

```javascript
try {
  const user = null;

  console.log(user.name);
} catch (error) {
  console.log("Something went wrong");
}
```

Instead of your application stopping at the error, the `catch` block runs.

Think:

```text
try
 ↓
Run code
 ↓
Error?
 ├── No → continue
 │
 └── Yes → catch
```

---

## 3. What is `error`?

In:

```javascript
catch (error) {
  console.log(error);
}
```

`error` contains information about what went wrong.

You can do:

```javascript
catch (error) {
  console.log(error.message);
}
```

For example, you might get an error message explaining that something was accessed incorrectly.

---

# 4. `finally`

`finally` means:

> **Run this code whether there is an error or not.**

Example:

```javascript
try {
  console.log("Trying");
} catch (error) {
  console.log("Error");
} finally {
  console.log("Finished");
}
```

If there is no error:

```text
Trying
Finished
```

If there is an error:

```text
Error
Finished
```

So `finally` **always runs** after the `try`/`catch` process.

---

# 5. Complete example

```javascript
try {
  const result = 10 / 2;

  console.log(result);
} catch (error) {
  console.log("Something went wrong");
} finally {
  console.log("Operation finished");
}
```

Output:

```text
5
Operation finished
```

There was no error, so `catch` didn't run.

But `finally` still ran.

---

# 6. Why do we need this with Fetch?

This is very important for our Todo app.

When we call an API, the request could fail.

We can write:

```javascript
async function getTodos() {
  try {
    const response = await fetch(url);
    const data = await response.json();

    console.log(data);
  } catch (error) {
    console.log("Failed to get todos");
  } finally {
    console.log("Request finished");
  }
}
```

The flow is:

```text
fetch API
   ↓
   try
   ↓
Success?
 ├── Yes → get data
 │
 └── No → catch → handle error
              ↓
           finally
              ↓
        request finished
```

---

## Remember these three

```text
try
→ Try the code

catch
→ Handle the error

finally
→ Run this regardless of success/error
```

------------------------------------------------------------------------------------------------------------------------------------------

# JSON

JSON is very important when working with **APIs**.

## 1. What is JSON?

**JSON = JavaScript Object Notation**

It is a common format used to **send and receive data** between applications and servers.

For example, an API might send:

```json
{
  "name": "William",
  "age": 25,
  "skills": ["HTML", "CSS", "JavaScript"]
}
```

It looks very similar to a JavaScript object, but JSON is actually **text/data format**.

---

## 2. JSON vs JavaScript Object

JavaScript object:

```javascript
const user = {
  name: "William",
  age: 25
};
```

JSON:

```json
{
  "name": "William",
  "age": 25
}
```

Notice the important difference:

In JSON, property names use **double quotes**:

```text
"name"
"age"
```

---

# 3. Why do we need JSON?

Imagine your JavaScript application wants user data from a server:

```text
Your application
      ↓
    Request
      ↓
    Server
      ↓
   JSON data
      ↓
Your application
```

The server might send:

```json
{
  "name": "William",
  "age": 25
}
```

JavaScript can then convert that JSON into a JavaScript value that it can work with.

---

# 4. `JSON.parse()`

`JSON.parse()` converts **JSON text → JavaScript value**.

Example:

```javascript
const jsonData = '{"name":"William","age":25}';

const user = JSON.parse(jsonData);
```

Now:

```javascript
user.name
```

gives:

```text
William
```

Think:

```text
JSON text
   ↓
JSON.parse()
   ↓
JavaScript object
```

---

# 5. `JSON.stringify()`

This does the opposite.

It converts:

**JavaScript value → JSON text**

Example:

```javascript
const user = {
  name: "William",
  age: 25
};

const jsonData = JSON.stringify(user);
```

Now `jsonData` is JSON text.

Think:

```text
JavaScript object
      ↓
JSON.stringify()
      ↓
JSON text
```

---

# 6. How this connects to Fetch

Earlier we wrote:

```javascript
const response = await fetch(url);

const data = await response.json();
```

This:

```javascript
response.json()
```

takes the JSON response from the server and converts it into a JavaScript value.

So:

```text
Server
  ↓
JSON
  ↓
response.json()
  ↓
JavaScript array/object
  ↓
Use it in your application
```

After:

```javascript
const data = await response.json();
```

`data` becomes a **JavaScript array containing objects**.

Then you can use:

```text
data.map(...)
data.filter(...)
data.find(...)
```

which connects directly to the array methods we already learned.

### Remember

```text
JSON.parse()
→ JSON → JavaScript

JSON.stringify()
→ JavaScript → JSON

response.json()
→ API response → JavaScript
```

------------------------------------------------------------------------------------------------------------------------------------------

# Modules

**Modules** allow you to split JavaScript code into **different files**.

Instead of putting everything into one huge file:

```text
app.js
 ├── user code
 ├── product code
 ├── API code
 └── utility code
```

you can separate them:

```text
user.js
product.js
api.js
utils.js
app.js
```

This makes your code easier to manage.

---

## 1. `export`

Suppose you have a file called `math.js`:

```javascript id="u2cxgs"
export function add(a, b) {
  return a + b;
}
```

`export` means:

> **Make this function available to other files.**

---

## 2. `import`

Now in another file:

```javascript id="h2c9iq"
import { add } from "./math.js";
```

`import` means:

> **Bring something from another file into this file.**

Then you can use it:

```javascript id="r0x5t5"
const result = add(10, 5);
```

Result:

```text id="y4v6r4"
15
```

---

## Think of it like this

```text id="jlv2hl"
math.js
   │
   │ export
   ↓
  add()
   │
   │ import
   ↓
app.js
```

So:

**`export` → send something out**

**`import` → bring something in**

---

## 3. Exporting a variable

You can export variables too:

```javascript id="x5b3d4"
export const name = "William";
```

Then:

```javascript id="n7n0dl"
import { name } from "./user.js";
```

---

## Why modules are important in React

React applications are usually split into many files:

```text
src/
 ├── App.jsx
 ├── components/
 │    ├── Header.jsx
 │    ├── Button.jsx
 │    └── Todo.jsx
 └── utils/
      └── helper.js
```

One component can be exported:

```text id="7oyz5x"
Button.jsx
    ↓
 export
    ↓
App.jsx
    ↓
 import
```

You'll use `import` and `export` constantly in React.

### Remember

```text
export → make available
import → use it in another file
```
------------------------------------------------------------------------------------------------------------------------------------------

# DOM

**DOM = Document Object Model**

The DOM is how JavaScript **sees and interacts with your HTML page**.

Suppose your HTML is:

```html id="s7nqkv"
<h1 id="title">Hello</h1>
<button id="button">Click Me</button>
```

The browser converts this HTML into a structure that JavaScript can interact with:

```text id="z5zmbz"
Document
  │
  ├── h1
  │    └── "Hello"
  │
  └── button
       └── "Click Me"
```

That's the **DOM**.

---

## 1. Find an HTML element

JavaScript can find an element using its `id`:

```javascript id="09pgq2"
const title = document.getElementById("title");
```

Now `title` refers to:

```html id="ngqg1p"
<h1 id="title">Hello</h1>
```

---

## 2. Change the content

You can change the text:

```javascript id="1w8v2q"
title.textContent = "Hello William";
```

Before:

```text id="m0y5so"
Hello
```

After:

```text id="l8mm2s"
Hello William
```

So JavaScript changed the HTML content.

---

## 3. Listen for a button click

HTML:

```html id="2uz7x1"
<button id="button">Click Me</button>
```

JavaScript:

```javascript id="1s4w2j"
const button = document.getElementById("button");

button.addEventListener("click", () => {
  console.log("Button clicked");
});
```

Now:

```text id="k0a4rh"
User clicks button
       ↓
JavaScript detects click
       ↓
"Button clicked"
```

---

## 4. Change styles

JavaScript can also change styles:

```javascript id="8i9s3h"
title.style.color = "blue";
```

The heading becomes blue.

---

### HTML input:


**If you are getting a value from an HTML `<input>` element, you use `.value`.**

For example:

```html
<input id="nameInput" type="text">
```

JavaScript:

```javascript
const nameInput = document.getElementById("nameInput");

console.log(nameInput.value);
```

If the user typed `William`:

```text
nameInput       → HTML input element
nameInput.value → "William"
```

But **not every HTML element uses `.value`**.

For example, with:

```html
<h1 id="title">Hello</h1>
```

you would use:

```javascript
const title = document.getElementById("title");

console.log(title.textContent);
```

So remember:

* `<input>` → `.value`
* `<textarea>` → `.value`
* `<select>` → `.value`
* `<h1>`, `<p>`, `<div>` → usually `.textContent`

And this:

```javascript
const name = "William";
console.log(name);
```

does **not** need `.value` because `name` already contains the value.


---

# Why DOM is important

Before React, you often manipulate HTML directly using the DOM.

For example:

```text id="k5j9jp"
HTML
 ↓
DOM
 ↓
JavaScript
 ↓
Change HTML
```

React changes the way we work with the UI. Instead of manually manipulating the DOM for every update, React manages UI updates based on **state and components**.

So understanding the DOM is still useful because it helps you understand **what React is doing for you**.

### Remember

**DOM = JavaScript's way of interacting with the HTML page.**

You can:

* Find elements
* Change text
* Change styles
* Listen for events
* Add/remove elements

---

The easiest way to understand DOM is:

> **DOM is the connection between your HTML page and JavaScript.**

### Step 1: You have HTML

```html
<h1 id="title">Hello</h1>
<button id="btn">Click Me</button>
```

When the browser loads this HTML, it creates a **DOM representation** of the page.

Think of it like:

```text
HTML
  ↓
Browser
  ↓
DOM
  ↓
JavaScript can access it
```

### Step 2: JavaScript finds the HTML element

```javascript
const title = document.getElementById("title");
```

This means:

> "JavaScript, find the HTML element whose id is `title`."

So `title` now refers to:

```html
<h1 id="title">Hello</h1>
```

### Step 3: JavaScript can change it

```javascript
title.textContent = "Hello William";
```

Before:

```text
Hello
```

After:

```text
Hello William
```

So the basic idea is:

```text
HTML
<h1 id="title">Hello</h1>
        ↓
JavaScript finds it
        ↓
document.getElementById("title")
        ↓
JavaScript changes it
        ↓
Hello William
```

### Another example: Button

HTML:

```html
<button id="btn">Click Me</button>
```

JavaScript:

```javascript
const button = document.getElementById("btn");
```

Now JavaScript has access to the button.

Then:

```javascript
button.addEventListener("click", () => {
  console.log("Button clicked");
});
```

This means:

> "When this button is clicked, run this code."

```text
User clicks button
       ↓
DOM detects the click
       ↓
JavaScript runs the function
       ↓
"Button clicked"
```

### The 3 things you should understand first

Don't worry about everything in DOM yet.

Just remember:

**1. `document`** → represents the webpage

**2. `getElementById()`** → finds an HTML element

**3. `addEventListener()`** → waits for an event like a click

Once these three are clear, DOM will become much easier.

------------------------------------------------------------------------------------------------------------------------------------------

# Fetch API

Now we'll learn **Fetch**, which is very important because it allows JavaScript to communicate with a **server/API**.

Think of it like this:

```text
Your Website
     ↓
   Fetch
     ↓
   Server
     ↓
   Data
     ↓
Your Website
```

## 1. What is an API?

An **API** allows your application to communicate with another system.

For example, your frontend might ask a server:

> "Give me the list of users."

The server sends the data back.

---

## 2. Using `fetch()`

JavaScript provides `fetch()` for making requests.

```javascript id="m9x8v3"
fetch("https://example.com/users");
```

This means:

> "Send a request to this URL."

`fetch()` returns a **Promise**, which connects directly to what we learned earlier.

---

## 3. Using `async/await`

A common way to use Fetch is:

```javascript id="0k9y2s"
async function getUsers() {
  const response = await fetch("https://example.com/users");
  const data = await response.json();

  console.log(data);
}
```

There are two important steps:

```text id="l9v3xk"
fetch()
   ↓
Server response
   ↓
response.json()
   ↓
Actual data
```

### Why `response.json()`?

The server sends a response.

`response.json()` converts the response into JavaScript data that you can work with.

For example, the server might send:

```text id="9u6w3e"
[
  {
    name: "William",
    age: 25
  },
  {
    name: "John",
    age: 30
  }
]
```

After `response.json()`, JavaScript can treat that as an **array of objects**.

---

## 4. Error handling

Usually we use `try/catch`:

```javascript id="j1f6u0"
async function getUsers() {
  try {
    const response = await fetch("https://example.com/users");
    const data = await response.json();

    console.log(data);
  } catch (error) {
    console.log(error);
  }
}
```

Think:

```text id="1k6m8g"
try
 ↓
Request data
 ↓
Success? → use data

Error?
 ↓
catch
 ↓
handle error
```

---

# Your roadmap connection

You originally planned:

> **Build: vanilla JS todo app with fetch**
------------------------------------------------------------------------------------------------------------------------------------------