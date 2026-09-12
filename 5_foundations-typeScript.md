# 1. TypeScript

We'll follow your roadmap in this order:

1. **Types**
2. **Type inference vs explicit typing**
3. **Interfaces**
4. **Union types**
5. **Intersection types**
6. **Generics**
7. **`tsconfig` basics**
8. **Typing React props/state** — later, when we reach React

And eventually we'll do the planned build:

> **Convert your vanilla JavaScript Todo app to TypeScript.**

---

# Step 1 — What is TypeScript?

You already know JavaScript.

TypeScript is basically:

> **JavaScript + type checking**

- TypeScript is a syntactic superset of JavaScript which adds static typing.

- This basically means that TypeScript adds syntax on top of JavaScript, allowing developers to add types.

For example, in JavaScript:

```javascript
let age = 25;
```

JavaScript doesn't tell you that `age` is supposed to be a number.

In TypeScript, we can explicitly say:

```typescript
let age: number = 25;
```

The `: number` tells TypeScript:

> `age` should contain a number.

So:

```text
JavaScript

age = 25


TypeScript

age: number = 25
      ↑
    type
```

If you later try:

```typescript
age = "William";
```

TypeScript will complain because:

```text
age → number

"William" → string
```

They don't match.

### The main idea

```text
JavaScript
    ↓
You can put different types of values


TypeScript
    ↓
You tell TypeScript what type of value is expected
    ↓
TypeScript checks your code
```

That's the basic reason TypeScript is useful.

------------------------------------------------------------------------------------------------------------------------------------------

# Step 2 — Basic TypeScript Types

TypeScript has types that tell us **what kind of value a variable should contain**.

The main ones we'll start with are:

* `string`
* `number`
* `boolean`
* `array`
* `object`

---

## 1. String

A string is text.

```typescript
let name: string = "William";
```

Here:

```text
name
 ↓
"William"
 ↓
string
```

So TypeScript knows `name` should contain text.

This would be wrong:

```typescript
name = 25;
```

Because `25` is a number, not a string.

---

## 2. Number

```typescript
let age: number = 25;
```

`age` must contain a number.

```typescript
age = 30;       // correct
age = "thirty"; // wrong
```

---

## 3. Boolean

Boolean means:

```text
true
false
```

Example:

```typescript
let isCompleted: boolean = false;
```

Later:

```typescript
isCompleted = true;
```

Both are valid because they are boolean values.

But:

```typescript
isCompleted = "true";
```

is wrong because `"true"` is a string, not a boolean.

---

## 4. Array

Suppose you have an array of names:

```typescript
let names: string[] = ["William", "John", "David"];
```

The `string[]` means:

> This is an array containing strings.

So this is okay:

```typescript
names.push("Peter");
```

But this isn't:

```typescript
names.push(25);
```

because `25` is a number.

You can also have a number array:

```typescript
let numbers: number[] = [10, 20, 30];
```

---

## 5. Object

Remember your Todo object?

```javascript
const todo = {
    title: "Learn React",
    completed: false
};
```

In TypeScript, we can describe its types:

```typescript
const todo: {
    title: string;
    completed: boolean;
} = {
    title: "Learn React",
    completed: false
};
```

We're telling TypeScript:

```text
todo
├── title → string
└── completed → boolean
```

So this would be wrong:

```typescript
completed: "false"
```

because `"false"` is a string, not a boolean.

---

# The important types for now

| Type       | Example               |
| ---------- | --------------------- |
| `string`   | `"Hello"`             |
| `number`   | `25`                  |
| `boolean`  | `true`                |
| `string[]` | `["A", "B"]`          |
| `number[]` | `[1, 2, 3]`           |
| object     | `{ name: "William" }` |

Don't try to memorize everything immediately.

The main idea is simply:

> **A TypeScript type tells us what kind of value is allowed.**

------------------------------------------------------------------------------------------------------------------------------------------

# Step 3 — Type Inference vs Explicit Typing

This is an important TypeScript concept.

There are **two ways** to tell TypeScript about a type.

### 1. Explicit typing

You tell TypeScript the type yourself:

```typescript
let name: string = "William";
```

You explicitly said:

```text
name → string
```

---

### 2. Type inference

TypeScript can figure out the type by looking at the value:

```typescript
let name = "William";
```

TypeScript sees:

```text
"William"
   ↓
string
```

So TypeScript automatically understands:

```text
name → string
```

You don't have to write `: string`.

---

## Another example

```typescript
let age = 25;
```

TypeScript understands:

```text
age → number
```

And:

```typescript
let isCompleted = false;
```

TypeScript understands:

```text
isCompleted → boolean
```

This is called **type inference**.

---

## So what's the difference?

### Explicit typing

```typescript
let age: number = 25;
```

You tell TypeScript:

> `age` is a number.

### Type inference

```typescript
let age = 25;
```

TypeScript says:

> I see `25`, so `age` must be a number.

---

## Why does this matter?

You don't need to write types everywhere.

For example, this is usually unnecessary:

```typescript
let name: string = "William";
let age: number = 25;
let isStudent: boolean = true;
```

You can often simply write:

```typescript
let name = "William";
let age = 25;
let isStudent = true;
```

TypeScript already knows their types.

### Simple rule

> **If TypeScript can clearly understand the type, let it infer it.**

Use explicit types when you need to **clearly define or constrain** something.

---

### One important example

This:

```typescript
let age = 25;
```

means TypeScript knows `age` is a number.

So later:

```typescript
age = "hello";
```

TypeScript will complain.

That's the benefit of type checking even when you don't explicitly write the type.

------------------------------------------------------------------------------------------------------------------------------------------

# Step 4 — Interfaces

This is one of the **most important TypeScript concepts**, especially when you start React.

You already have Todo objects like:

```javascript
const todo = {
  title: "Learn React",
  completed: false
};
```

The question is:

> How can we tell TypeScript exactly what a Todo object should look like?

That's where an **interface** comes in.

---

## What is an interface?

An interface is like a set of rules/blueprint that an object must follow.  

For example:

```typescript
interface Todo {
  title: string;
  completed: boolean;
}
```

We're telling TypeScript:

> A `Todo` must have a `title` that is a string and `completed` that is a boolean.

Think of it like:

```text
Todo
│
├── title     → string
└── completed → boolean
```

---

## Using the interface

Now we can create a Todo:

```typescript
const todo: Todo = {
  title: "Learn React",
  completed: false
};
```

TypeScript checks it against our `Todo` interface.

Everything matches:

```text
title
"Learn React"
    ↓
string ✓

completed
false
    ↓
boolean ✓
```

---

## What if we make a mistake?

For example:

```typescript
const todo: Todo = {
  title: "Learn React",
  completed: "false"
};
```

TypeScript says:

> `completed` should be a boolean, but you provided a string.

Because:

```text
"false" → string ❌

false → boolean ✓
```

---

## What if we forget a property?

Our interface says:

```typescript
interface Todo {
  title: string;
  completed: boolean;
}
```

So this is incomplete:

```typescript
const todo: Todo = {
  title: "Learn React"
};
```

We're missing:

```text
completed
```

TypeScript will complain.

---

# Why do we need interfaces?

Imagine your application has **100 Todo objects**.

Without an interface, you might accidentally create:

```text
Todo 1 → title + completed
Todo 2 → title + completed
Todo 3 → title + complete
Todo 4 → title + isCompleted
```

Now your data is inconsistent.

With an interface:

```typescript
interface Todo {
  title: string;
  completed: boolean;
}
```

TypeScript helps ensure every Todo follows the same structure.

---

## One more important thing

Interfaces can include more properties.

For example, our API Todo has an `id`:

```typescript
interface Todo {
  id: number;
  title: string;
  completed: boolean;
}
```

Now a Todo looks like:

```text
Todo
│
├── id        → number
├── title     → string
└── completed → boolean
```

This is actually very similar to the Todo objects we've been working with.

### Simple definition to remember:

> Interface = rules that define what an object should contain and what type each property should have.
------------------------------------------------------------------------------------------------------------------------------------------

# Step 5 — Union Types

Now we'll learn **Union Types**.

The idea is simple:

> **A union type allows a value to be one of multiple types.**

We use the `|` symbol.

### Example

```typescript id="2mpq7c"
let id: number | string;
```

This means:

> `id` can be a **number OR a string**.

So both are allowed:

```typescript id="4p2s1k"
id = 10;
id = "10";
```

But this isn't:

```typescript id="o6k4jq"
id = true;
```

because `boolean` isn't included.

---

## Why would we need this?

Imagine an API sometimes gives an ID as a number:

```text id="x5xuws"
id: 101
```

and another API gives an ID as a string:

```text id="2cqk4w"
id: "101"
```

You could describe that with:

```typescript id="9t9e5c"
let id: number | string;
```

---

## Another example

```typescript id="j4vr7f"
let value: string | number;
```

Allowed:

```text id="1a8wfg"
"Hello"  ✓
100      ✓
```

Not allowed:

```text id="j0qv9h"
true     ✗
```

---

# Union Types with functions

You can also use them in function parameters:

```typescript id="yxg3fc"
function printId(id: number | string) {
  console.log(id);
}
```

Now we can call:

```typescript id="7m0j1a"
printId(101);
printId("101");
```

Both work.

---

# Union Types vs Interface

Don't confuse these.

### Interface

Describes the **structure of an object**:

```typescript id="3b0p7d"
interface Todo {
  id: number;
  title: string;
  completed: boolean;
}
```

### Union

Allows **multiple possible types**:

```typescript id="a1g5fk"
let id: number | string;
```

Think:

```text id="x6m4wq"
Interface
   ↓
"What should this object look like?"

Union
   ↓
"What types can this value be?"
```

### Simple definition

> **Union = OR**

```text id="n1py3k"
number OR string
```

------------------------------------------------------------------------------------------------------------------------------------------

# Step 6 — Intersection Types

> & can be used to combine two interfaces, but more generally it combines types.  

You already learned **Union Types**:

```typescript
number | string
```

means:

> number **OR** string.

Intersection types are the opposite idea:

```typescript
A & B
```

means:

> A **AND** B.

---

## Simple example

Suppose we have two interfaces:

```typescript
interface Person {
  name: string;
}

interface Employee {
  company: string;
}
```

Now we can combine them:

```typescript
type EmployeePerson = Person & Employee;
```

This means `EmployeePerson` must have **both**:

```text
EmployeePerson
│
├── name     → string
└── company  → string
```

So this is valid:

```typescript
const person: EmployeePerson = {
  name: "William",
  company: "ABC"
};
```

Because it has:

```text
Person  ✓
Employee ✓
```

---

## What if we only provide `name`?

```typescript
const person: EmployeePerson = {
  name: "William"
};
```

TypeScript complains because `company` is missing.

Remember:

```text
Person & Employee
       ↓
   Person AND Employee
       ↓
Both are required
```

---

# Union vs Intersection

This is the important part.

### Union `|`

```typescript
type Value = string | number;
```

Means:

```text
string OR number
```

You can have either one.

### Intersection `&`

```typescript
type EmployeePerson = Person & Employee;
```

Means:

```text
Person AND Employee
```

You need both structures.

---

### Easy way to remember

```text
|  → OR

&  → AND
```

That's really the main thing you need to understand right now.

------------------------------------------------------------------------------------------------------------------------------------------

# Step 7 — Generics

Generics can look complicated at first, but the basic idea is actually simple.

> **Generics allow us to write reusable code that can work with different types.**

Let's start without complicated examples.

### Imagine a function

Suppose we have:

```typescript
function getValue(value: string) {
    return value;
}
```

This function only accepts a `string`.

```text
getValue("Hello")  ✓
getValue(100)      ✗
```

But what if we want the same function to work with **strings, numbers, booleans, or objects**?

We could use a generic.

```typescript
function getValue<T>(value: T) {
    return value;
}
```

Here:

```text
T
↓
Type placeholder
```

It means:

> "I don't know the type yet. You tell me when you use the function."

### Using it with a string

```typescript
getValue<string>("Hello");
```

Now:

```text
T = string
```

### Using it with a number

```typescript
getValue<number>(100);
```

Now:

```text
T = number
```

So the same function can work with different types.

```text
getValue<string>("Hello")
        ↓
      string

getValue<number>(100)
        ↓
      number
```

---

## Why not just use `any`?

You might wonder why we don't simply do:

```typescript
function getValue(value: any) {
    return value;
}
```

`any` basically tells TypeScript:

> "Don't worry about checking this."

That removes much of the benefit of TypeScript.

Generics instead say:

> "The type can be different, but keep track of what that type is."

---

## A real-world example

Imagine an API returns an array of Todos.

We could eventually write something like:

```typescript
function getFirst<T>(items: T[]) {
    return items[0];
}
```

If we give it:

```text
Todo[]
```

TypeScript knows the result is a:

```text
Todo
```

If we give it:

```text
string[]
```

TypeScript knows the result is a:

```text
string
```

Same function, different types.

### The simple definition

> **Generic = reusable code that works with different types while still keeping type safety.**

For now, remember:

```text
<T>
 ↓
"Type will be decided when we use it."
```

------------------------------------------------------------------------------------------------------------------------------------------

# Step 7 — Generics

Generics can look complicated at first, but the basic idea is actually simple.

> **Generics allow us to write reusable code that can work with different types.**

Let's start without complicated examples.

### Imagine a function

Suppose we have:

```typescript
function getValue(value: string) {
    return value;
}
```

This function only accepts a `string`.

```text
getValue("Hello")  ✓
getValue(100)      ✗
```

But what if we want the same function to work with **strings, numbers, booleans, or objects**?

We could use a generic.

```typescript
function getValue<T>(value: T) {
    return value;
}
```

Here:

```text
T
↓
Type placeholder
```

It means:

> "I don't know the type yet. You tell me when you use the function."

### Using it with a string

```typescript
getValue<string>("Hello");
```

Now:

```text
T = string
```

### Using it with a number

```typescript
getValue<number>(100);
```

Now:

```text
T = number
```

So the same function can work with different types.

```text
getValue<string>("Hello")
        ↓
      string

getValue<number>(100)
        ↓
      number
```

---

## Why not just use `any`?

You might wonder why we don't simply do:

```typescript
function getValue(value: any) {
    return value;
}
```

`any` basically tells TypeScript:

> "Don't worry about checking this."

That removes much of the benefit of TypeScript.

Generics instead say:

> "The type can be different, but keep track of what that type is."

---

## A real-world example

Imagine an API returns an array of Todos.

We could eventually write something like:

```typescript
function getFirst<T>(items: T[]) {
    return items[0];
}
```

If we give it:

```text
Todo[]
```

TypeScript knows the result is a:

```text
Todo
```

If we give it:

```text
string[]
```

TypeScript knows the result is a:

```text
string
```

Same function, different types.

### The simple definition

> **Generic = reusable code that works with different types while still keeping type safety.**

For now, remember:

```text
<T>
 ↓
"Type will be decided when we use it."
```

------------------------------------------------------------------------------------------------------------------------------------------

# Step 8 — `tsconfig` Basics

Now we'll learn **`tsconfig.json`**.

Don't worry — this is mostly about **configuration**, not TypeScript syntax.

### What is `tsconfig.json`?

`tsconfig.json` is a configuration file that tells TypeScript:

> **"How should you treat and compile my TypeScript project?"**

Think of it like **settings for TypeScript**.

```text
Your TypeScript files
        ↓
   tsconfig.json
        ↓
TypeScript knows how to handle them
```

---

## Why do we need it?

Suppose your project has:

```text
src/
  app.ts
  todo.ts
  user.ts
```

TypeScript needs to know things like:

* Which files should be checked?
* Which JavaScript version should be generated?
* Where should the output files go?
* How strict should type checking be?

These settings go into:

```text
tsconfig.json
```

---

## Important settings

You don't need to memorize all of them.

### `target`

Tells TypeScript which JavaScript version to generate.

For example:

```text
target → modern JavaScript
```

You can think:

> "What JavaScript version should my TypeScript become?"

---

### `strict`

This is important.

```text
strict → true
```

means:

> **Be strict when checking my TypeScript code.**

For example, TypeScript will catch more potential mistakes.

---

### `rootDir`

Tells TypeScript where your source code is.

Think:

```text
rootDir
   ↓
"Where is my TypeScript code?"
```

---

### `outDir`

Tells TypeScript where to put the generated JavaScript.

Think:

```text
TypeScript
    ↓
compile
    ↓
JavaScript
    ↓
outDir
```

---

# The basic picture

```text
             tsconfig.json
                  ↓
        ┌─────────┼─────────┐
        ↓         ↓         ↓
      target    strict    folders
        ↓         ↓         ↓
   JS version  checking   input/output
```

You don't need to deeply understand every `tsconfig` option right now.

### What you should remember

> **`tsconfig.json` = TypeScript project configuration.**

And that's enough for this step.

---

# TypeScript roadmap progress

You've now covered:

* Types
* Type inference
* Explicit typing
* Interfaces
* Union types
* Intersection types
* Generics
* `tsconfig` basics

The next item in your roadmap is:

**Typing React props/state**

But **React hasn't been introduced yet**.

So we won't jump there.

Instead, we'll do the planned TypeScript build:

## Convert our Vanilla JS Todo App → TypeScript

This will make the TypeScript concepts you've learned actually useful.

------------------------------------------------------------------------------------------------------------------------------------------