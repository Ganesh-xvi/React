# Phase 11 — Testing

Now we move to the next phase of your roadmap.

## Topics

```text
1. What is Testing?
2. Why do we test applications?
3. Types of testing
   ├── Unit Testing
   ├── Integration Testing
   └── End-to-End (E2E) Testing
4. Jest
5. React Testing Library
6. Playwright or Cypress
```

---

# Step 1 — What Is Testing?

Testing means checking whether your application behaves as expected.

Example:

You have a function:

```tsx
function add(a: number, b: number) {
    return a + b;
}
```

You expect:

```text
add(2, 3)
↓
5
```

A test checks that expectation automatically.

```text
Expected: 5
Actual:   5

✓ Test Passed
```

---

# Why Do We Need Testing?

Imagine you have an application with:

```text
Login
Dashboard
Cart
Payment
Profile
```

You change something in the login code.

That change might accidentally break:

```text
Authentication
Protected routes
User profile
```

Testing helps catch these problems.

```text
You change code
      ↓
Run tests
      ↓
Something broke?
   ↓        ↓
  Yes       No
   ↓         ↓
 Fix it    Deploy
```

---

# Manual Testing vs Automated Testing

## Manual Testing

You manually open the application:

```text
Open browser
↓
Click Login
↓
Enter username
↓
Enter password
↓
Click Submit
↓
Check result
```

---

## Automated Testing

Code does it automatically.

```text
Run test
↓
Test clicks button
↓
Test enters data
↓
Test checks result
```

This is faster and repeatable.

---

# The 3 Main Types of Testing

## 1. Unit Testing

Tests a small piece of code.

```text
Function
Component logic
Utility function
```

Example:

```tsx
add(2, 3)
```

Expected:

```text
5
```

---

## 2. Integration Testing

Tests multiple parts working together.

Example:

```text
Login Form
    +
Validation
    +
API call
```

We test whether they work together correctly.

---

## 3. End-to-End Testing (E2E)

Tests the application like a real user.

Example:

```text
Open website
↓
Click Login
↓
Enter email
↓
Enter password
↓
Click Submit
↓
Dashboard appears
```

This tests the complete flow.

---

# Simple Comparison

| Testing Type     | Tests                   |
| ---------------- | ----------------------- |
| Unit Test        | Small individual pieces |
| Integration Test | Multiple parts together |
| E2E Test         | Complete user flow      |

---

# Real-World Example

Imagine a shopping website.

### Unit Test

```text
calculateTotal()
```

Does it calculate correctly?

### Integration Test

```text
Cart + Checkout Form + Total
```

Do they work together?

### E2E Test

```text
User opens website
↓
Adds product
↓
Goes to cart
↓
Completes checkout
```

Does the complete user journey work?

---

# Testing Tools We Will Learn

```text
Jest
↓
Test runner and testing framework


React Testing Library
↓
Tests React components from the user's perspective


Playwright
↓
E2E browser testing
```

For your roadmap, I recommend:

```text
Jest
+
React Testing Library
+
Playwright
```

---

# Mental Model

```text
TESTING
│
├── Unit Testing
│     └── Jest
│
├── React Component Testing
│     └── React Testing Library
│
└── Full Application Testing
      └── Playwright
```

---

# Important Mindset

Testing is not mainly about testing implementation details.

Good testing asks:

> "Does the application behave correctly for the user?"

For example, instead of testing:

```text
Did useState get called?
```

We prefer:

```text
Did clicking the button increase the counter?
```

---

# Phase 11 Progress

```text
[x] Step 1 — Testing Fundamentals

[ ] Step 2 — Setup Jest
[ ] Step 3 — First Unit Test
[ ] Step 4 — React Testing Library
[ ] Step 5 — Testing User Interactions
[ ] Step 6 — Mocking API Calls
[ ] Step 7 — E2E Testing with Playwright
```

## Next → Step 2: Setting up Jest and writing your first test.
# Phase 11 — Testing

Now we move to the next phase of your roadmap.

## Topics

```text
1. What is Testing?
2. Why do we test applications?
3. Types of testing
   ├── Unit Testing
   ├── Integration Testing
   └── End-to-End (E2E) Testing
4. Jest
5. React Testing Library
6. Playwright or Cypress
```

---

# Step 1 — What Is Testing?

Testing means checking whether your application behaves as expected.

Example:

You have a function:

```tsx
function add(a: number, b: number) {
    return a + b;
}
```

You expect:

```text
add(2, 3)
↓
5
```

A test checks that expectation automatically.

```text
Expected: 5
Actual:   5

✓ Test Passed
```

---

# Why Do We Need Testing?

Imagine you have an application with:

```text
Login
Dashboard
Cart
Payment
Profile
```

You change something in the login code.

That change might accidentally break:

```text
Authentication
Protected routes
User profile
```

Testing helps catch these problems.

```text
You change code
      ↓
Run tests
      ↓
Something broke?
   ↓        ↓
  Yes       No
   ↓         ↓
 Fix it    Deploy
```

---

# Manual Testing vs Automated Testing

## Manual Testing

You manually open the application:

```text
Open browser
↓
Click Login
↓
Enter username
↓
Enter password
↓
Click Submit
↓
Check result
```

---

## Automated Testing

Code does it automatically.

```text
Run test
↓
Test clicks button
↓
Test enters data
↓
Test checks result
```

This is faster and repeatable.

---

# The 3 Main Types of Testing

## 1. Unit Testing

Tests a small piece of code.

```text
Function
Component logic
Utility function
```

Example:

```tsx
add(2, 3)
```

Expected:

```text
5
```

---

## 2. Integration Testing

Tests multiple parts working together.

Example:

```text
Login Form
    +
Validation
    +
API call
```

We test whether they work together correctly.

---

## 3. End-to-End Testing (E2E)

Tests the application like a real user.

Example:

```text
Open website
↓
Click Login
↓
Enter email
↓
Enter password
↓
Click Submit
↓
Dashboard appears
```

This tests the complete flow.

---

# Simple Comparison

| Testing Type     | Tests                   |
| ---------------- | ----------------------- |
| Unit Test        | Small individual pieces |
| Integration Test | Multiple parts together |
| E2E Test         | Complete user flow      |

---

# Real-World Example

Imagine a shopping website.

### Unit Test

```text
calculateTotal()
```

Does it calculate correctly?

### Integration Test

```text
Cart + Checkout Form + Total
```

Do they work together?

### E2E Test

```text
User opens website
↓
Adds product
↓
Goes to cart
↓
Completes checkout
```

Does the complete user journey work?

---

# Testing Tools We Will Learn

```text
Jest
↓
Test runner and testing framework


React Testing Library
↓
Tests React components from the user's perspective


Playwright
↓
E2E browser testing
```

For your roadmap, I recommend:

```text
Jest
+
React Testing Library
+
Playwright
```

---

# Mental Model

```text
TESTING
│
├── Unit Testing
│     └── Jest
│
├── React Component Testing
│     └── React Testing Library
│
└── Full Application Testing
      └── Playwright
```

---

# Important Mindset

Testing is not mainly about testing implementation details.

Good testing asks:

> "Does the application behave correctly for the user?"

For example, instead of testing:

```text
Did useState get called?
```

We prefer:

```text
Did clicking the button increase the counter?
```

---

# Phase 11 Progress

```text
[x] Step 1 — Testing Fundamentals

[ ] Step 2 — Setup Jest
[ ] Step 3 — First Unit Test
[ ] Step 4 — React Testing Library
[ ] Step 5 — Testing User Interactions
[ ] Step 6 — Mocking API Calls
[ ] Step 7 — E2E Testing with Playwright
```

## Next → Step 2: Setting up Jest and writing your first test.

-------------------------------------------------------------------------------------------------------------------------------------------


# Step 2 — Setting Up Jest

Before writing tests, let's understand what Jest does.

## What Is Jest?

Jest is a JavaScript testing framework.

It provides:

```text
✓ Test runner
✓ Assertions
✓ Mocking
✓ Test organization
```

The basic flow:

```text
Write test
    ↓
Run Jest
    ↓
Jest executes test
    ↓
Compare expected vs actual result
    ↓
Pass / Fail
```

---

# 1. Install Jest

For a TypeScript project:

```bash
npm install -D jest ts-jest @types/jest
```

What are these?

| Package       | Purpose                             |
| ------------- | ----------------------------------- |
| `jest`        | Testing framework                   |
| `ts-jest`     | Allows Jest to work with TypeScript |
| `@types/jest` | TypeScript types for Jest           |

---

# 2. Create Jest Configuration

Create:

```text
jest.config.js
```

Basic configuration:

```js
module.exports = {
    preset: "ts-jest",
    testEnvironment: "node",
};
```

For now, don't worry too much about this.

Just understand:

```text
ts-jest
↓
Allows TypeScript testing


testEnvironment: "node"
↓
Tests run in a Node.js environment
```

Later, when testing React components, we will use:

```text
jsdom
```

because React components need a browser-like environment.

---

# 3. Add a Test Script

Open:

```text
package.json
```

Add:

```json
{
    "scripts": {
        "test": "jest"
    }
}
```

Now:

```text
npm test
```

will run Jest.

---

# 4. How Does Jest Find Tests?

Jest automatically looks for files like:

```text
something.test.ts
something.test.tsx

something.spec.ts
something.spec.tsx
```

Example:

```text
src/
├── utils/
│   └── math.ts
│
└── utils/
    └── math.test.ts
```

Or:

```text
src/
├── math.ts
└── math.test.ts
```

Both naming approaches are common.

---

# 5. Our First Example

Create:

## `src/utils/math.ts`

```ts
export function add(a: number, b: number) {
    return a + b;
}
```

Now create:

## `src/utils/math.test.ts`

```ts
import { add } from "./math";

test("adds two numbers correctly", () => {
    expect(add(2, 3)).toBe(5);
});
```

This is your first Jest test.

---

# 6. Understanding the Test

Let's break it down.

```ts
test("adds two numbers correctly", () => {
```

This creates a test.

The first argument:

```text
"adds two numbers correctly"
```

is the test description.

Then:

```ts
add(2, 3)
```

produces:

```text
5
```

Finally:

```ts
expect(add(2, 3)).toBe(5);
```

means:

```text
I expect:

add(2, 3)

to be:

5
```

---

# 7. The Most Important Jest Pattern

```ts
expect(actual).toBe(expected);
```

Example:

```ts
expect(2 + 2).toBe(4);
```

Structure:

```text
expect()
   ↓
Actual value

toBe()
   ↓
Expected value
```

---

# 8. Run the Test

Run:

```bash
npm test
```

If everything works:

```text
PASS src/utils/math.test.ts

✓ adds two numbers correctly

Test Suites: 1 passed
Tests:       1 passed
```

---

# 9. What Happens If the Test Fails?

Change:

```ts
expect(add(2, 3)).toBe(10);
```

Now Jest says:

```text
Expected: 10
Received: 5
```

Result:

```text
FAIL
```

This is exactly why tests are useful.

They automatically detect incorrect behavior.

---

# 10. Multiple Tests

You can write multiple tests.

```ts
import { add } from "./math";

test("adds positive numbers", () => {
    expect(add(2, 3)).toBe(5);
});

test("adds negative numbers", () => {
    expect(add(-2, -3)).toBe(-5);
});

test("adds positive and negative numbers", () => {
    expect(add(10, -5)).toBe(5);
});
```

Result:

```text
✓ adds positive numbers
✓ adds negative numbers
✓ adds positive and negative numbers
```

---

# 11. `describe()` for Grouping Tests

When you have many related tests:

```ts
describe("add function", () => {
    test("adds positive numbers", () => {
        expect(add(2, 3)).toBe(5);
    });

    test("adds negative numbers", () => {
        expect(add(-2, -3)).toBe(-5);
    });
});
```

Output conceptually:

```text
add function
  ✓ adds positive numbers
  ✓ adds negative numbers
```

Mental model:

```text
describe()
↓
Groups tests


test()
↓
Individual test
```

---

# 12. Important Testing Structure

Usually:

```text
Arrange
↓
Act
↓
Assert
```

This is called AAA.

### Example:

```ts
test("adds two numbers", () => {

    // Arrange
    const a = 2;
    const b = 3;

    // Act
    const result = add(a, b);

    // Assert
    expect(result).toBe(5);

});
```

---

# Mental Model

```text
JEST TEST

test()
 │
 ├── Run function/code
 │
 ├── Get actual result
 │
 └── expect()
       ↓
    Compare result
       ↓
    Pass / Fail
```

---

# Important Note for Your React/Vite Projects

Your roadmap says **Jest**, so we're learning Jest fundamentals.

However, in modern Vite + React projects, you may also commonly see:

```text
Vitest
```

Vitest is very popular with Vite projects.

But the core testing concepts you're learning now are transferable:

```text
test()
expect()
describe()
Assertions
Mocking
Component testing
```

---

# What You Should Remember

```text
Jest
↓
Testing framework

test()
↓
Creates a test

expect()
↓
Checks actual value

toBe()
↓
Checks expected value

describe()
↓
Groups related tests
```

---

# Phase 11 Progress

```text
[x] Step 1 — Testing Fundamentals
[x] Step 2 — Jest Setup

[ ] Step 3 — First Unit Tests + Matchers
[ ] Step 4 — React Testing Library
[ ] Step 5 — Testing User Interactions
[ ] Step 6 — Mocking API Calls
[ ] Step 7 — E2E Testing with Playwright
```

# Next → Step 3: Jest Matchers and writing proper Unit Tests.

------------------------------------------------------------------------------------------------------------------------------------------

# Step 3 — Jest Matchers and Unit Tests

Now we know:

```text
test()
expect()
toBe()
describe()
```

Next, let's understand **Matchers**.

---

# 1. What Is a Matcher?

A matcher checks whether something matches your expectation.

Example:

```ts
expect(2 + 2).toBe(4);
```

Here:

```text
expect(2 + 2)
     ↓
Actual value

toBe(4)
     ↓
Matcher checking expected value
```

`toBe()` is a matcher.

Jest has many matchers.

---

# 2. `toBe()`

Used for primitive values:

```ts
expect(5).toBe(5);

expect("Hello").toBe("Hello");

expect(true).toBe(true);
```

Common primitive values:

```text
string
number
boolean
undefined
null
```

Example:

```ts
test("checks a number", () => {
    expect(10).toBe(10);
});
```

---

# 3. `toEqual()`

Used for objects and arrays.

Example:

```ts
const user = {
    name: "William",
    age: 25,
};
```

Test:

```ts
expect(user).toEqual({
    name: "William",
    age: 25,
});
```

Why not `toBe()`?

Because objects are compared by reference.

```ts
const user1 = { name: "William" };
const user2 = { name: "William" };

console.log(user1 === user2);
```

Result:

```text
false
```

Different objects in memory.

So:

```text
toBe()
↓
Best for primitive/reference equality

toEqual()
↓
Checks object/array contents
```

---

# 4. `toBeTruthy()` and `toBeFalsy()`

JavaScript has truthy and falsy values.

### Truthy:

```ts
expect(true).toBeTruthy();

expect("hello").toBeTruthy();

expect(1).toBeTruthy();
```

### Falsy:

```ts
expect(false).toBeFalsy();

expect(0).toBeFalsy();

expect("").toBeFalsy();

expect(null).toBeFalsy();
```

---

# 5. `toBeNull()`

Checks specifically for `null`.

```ts
const user = null;

expect(user).toBeNull();
```

---

# 6. `toBeDefined()`

Checks that something exists and is not `undefined`.

```ts
const name = "William";

expect(name).toBeDefined();
```

Example:

```ts
let age;

expect(age).not.toBeDefined();
```

---

# 7. `not`

You can reverse a matcher.

Example:

```ts
expect(5).not.toBe(10);
```

Meaning:

```text
I expect 5 NOT to be 10
```

More examples:

```ts
expect("Hello").not.toBe("World");

expect(user).not.toBeNull();

expect(value).not.toBeFalsy();
```

---

# 8. Number Matchers

Jest can compare numbers.

```ts
expect(10).toBeGreaterThan(5);

expect(10).toBeGreaterThanOrEqual(10);

expect(5).toBeLessThan(10);

expect(5).toBeLessThanOrEqual(5);
```

Example:

```ts
test("price is greater than zero", () => {
    const price = 100;

    expect(price).toBeGreaterThan(0);
});
```

---

# 9. String Matchers

You can check whether text contains something.

```ts
expect("Hello William").toContain("William");
```

Or use regular expressions:

```ts
expect("Hello William").toMatch(/William/);
```

---

# 10. Array Matchers

Example:

```ts
const fruits = [
    "Apple",
    "Banana",
    "Orange",
];
```

Check if something exists:

```ts
expect(fruits).toContain("Banana");
```

---

# 11. Important Matchers Summary

| Matcher         | Purpose                     |
| --------------- | --------------------------- |
| `toBe()`        | Primitive values            |
| `toEqual()`     | Objects and arrays          |
| `toBeTruthy()`  | Truthy value                |
| `toBeFalsy()`   | Falsy value                 |
| `toBeNull()`    | Checks null                 |
| `toBeDefined()` | Checks not undefined        |
| `toContain()`   | Array/string contains value |
| `toMatch()`     | Matches string pattern      |
| `not`           | Reverses expectation        |

---

# 12. Let's Build a Real Unit Test

Create a utility:

## `math.ts`

```ts
export function add(a: number, b: number) {
    return a + b;
}

export function multiply(a: number, b: number) {
    return a * b;
}

export function isEven(number: number) {
    return number % 2 === 0;
}
```

---

## `math.test.ts`

```ts
import {
    add,
    multiply,
    isEven,
} from "./math";

describe("Math utilities", () => {
    test("adds two numbers", () => {
        expect(add(2, 3)).toBe(5);
    });

    test("multiplies two numbers", () => {
        expect(multiply(4, 5)).toBe(20);
    });

    test("checks if number is even", () => {
        expect(isEven(4)).toBe(true);
    });

    test("checks if number is odd", () => {
        expect(isEven(5)).toBe(false);
    });
});
```

---

# 13. Better Testing Example

Imagine a function:

```ts
export function getDiscount(
    price: number,
    percentage: number
) {
    return price - (price * percentage) / 100;
}
```

Test:

```ts
import { getDiscount } from "./price";

describe("getDiscount", () => {
    test("calculates 10% discount", () => {
        expect(
            getDiscount(100, 10)
        ).toBe(90);
    });

    test("calculates 50% discount", () => {
        expect(
            getDiscount(200, 50)
        ).toBe(100);
    });
});
```

---

# 14. Good Unit Tests Test Different Cases

Don't test only the happy path.

Example:

```ts
add(2, 3)
```

is a normal case.

Also test:

```text
Positive numbers
Negative numbers
Zero
Different combinations
```

Example:

```ts
describe("add", () => {
    test("adds positive numbers", () => {
        expect(add(2, 3)).toBe(5);
    });

    test("adds negative numbers", () => {
        expect(add(-2, -3)).toBe(-5);
    });

    test("adds zero", () => {
        expect(add(5, 0)).toBe(5);
    });
});
```

This gives more confidence that the function works.

---

# 15. The Most Important Mental Model

Every test follows this idea:

```text
GIVEN
↓
Some starting condition

WHEN
↓
Something happens

THEN
↓
Check expected result
```

Example:

```text
GIVEN
Two numbers: 2 and 3

WHEN
add() is called

THEN
Result should be 5
```

Code:

```ts
test("adds two numbers", () => {
    expect(add(2, 3)).toBe(5);
});
```

---

# 16. AAA vs Given-When-Then

You may see both styles.

### AAA

```text
Arrange
Act
Assert
```

### Given-When-Then

```text
Given
When
Then
```

They mean almost the same idea.

---

# What You Should Remember

```text
JEST MATCHERS

toBe()
↓
Primitive values

toEqual()
↓
Objects and arrays

toContain()
↓
Contains value

toBeTruthy()
↓
Truthy value

not
↓
Reverse expectation
```

And:

```text
Good Unit Test
↓
Tests one small behavior
```

---

# Phase 11 Progress

```text
[x] Step 1 — Testing Fundamentals
[x] Step 2 — Jest Setup
[x] Step 3 — Jest Matchers + Unit Tests

[ ] Step 4 — React Testing Library
[ ] Step 5 — Testing User Interactions
[ ] Step 6 — Mocking API Calls
[ ] Step 7 — E2E Testing with Playwright
```

# Next → Step 4: React Testing Library

Now we move from testing simple functions to testing actual React components.

-----------------------------------------------------------------------------------------------------------------------------------------

# Step 4 — React Testing Library (RTL)

Now we move from testing simple functions to testing actual React components.

---

# 1. What Is React Testing Library?

React Testing Library is a library for testing React components.

The important idea is:

> Test your application the way a user uses it.

Instead of checking React internals:

```text
Did useState run?
Did this function get called internally?
```

We test what the user sees:

```text
Can the user see the button?
Can the user see the text?
Does clicking the button change the UI?
```

---

# 2. The Mental Model

```text
React Component
      ↓
Render component in test
      ↓
Find elements
      ↓
Interact like a user
      ↓
Check what appears
```

---

# 3. Example Component

## `Greeting.tsx`

```tsx
type GreetingProps = {
    name: string;
};

function Greeting({ name }: GreetingProps) {
    return <h1>Hello, {name}</h1>;
}

export default Greeting;
```

This component receives:

```text
name = "William"
```

And displays:

```text
Hello, William
```

---

# 4. Testing the Component

Create:

## `Greeting.test.tsx`

```tsx
import { render, screen } from "@testing-library/react";
import Greeting from "./Greeting";

test("displays the user's name", () => {
    render(<Greeting name="William" />);

    expect(
        screen.getByText("Hello, William")
    ).toBeInTheDocument();
});
```

---

# 5. Understanding the Code

### `render()`

```tsx
render(<Greeting name="William" />);
```

This renders the React component inside the test environment.

Conceptually:

```text
Test Environment

<Greeting name="William" />

↓

Hello, William
```

---

### `screen`

```tsx
screen.getByText("Hello, William");
```

This searches the rendered UI.

It asks:

> Can I find this text on the screen?

---

### `expect()`

```tsx
expect(
    screen.getByText("Hello, William")
).toBeInTheDocument();
```

Meaning:

```text
I expect this text
↓
to exist in the rendered document
```

---

# 6. Main RTL Pattern

Most React Testing Library tests follow:

```text
Render
↓
Find
↓
Interact
↓
Assert
```

Example:

```tsx
render(<Component />);

const button = screen.getByRole("button");

expect(button).toBeInTheDocument();
```

---

# 7. Why `getByRole()` Is Important

React Testing Library encourages finding elements the way users and accessibility tools find them.

Example component:

```tsx
function App() {
    return (
        <div>
            <h1>Welcome</h1>

            <button>Login</button>

            <input placeholder="Enter email" />
        </div>
    );
}
```

We can find elements in different ways.

---

# 8. `getByText()`

Find visible text.

```tsx
screen.getByText("Welcome");
```

Or:

```tsx
screen.getByText("Login");
```

---

# 9. `getByRole()`

Find elements by their HTML/accessibility role.

```tsx
screen.getByRole("button");
```

Find the heading:

```tsx
screen.getByRole("heading");
```

Find textbox:

```tsx
screen.getByRole("textbox");
```

A more specific example:

```tsx
screen.getByRole("button", {
    name: "Login",
});
```

This means:

```text
Find a button
with the accessible name "Login"
```

This is often preferred.

---

# 10. Common Roles

| HTML Element | Role    |
| ------------ | ------- |
| `<button>`   | button  |
| `<input>`    | textbox |
| `<h1>`       | heading |
| `<a>`        | link    |
| `<img>`      | img     |
| `<form>`     | form    |

Example:

```tsx
screen.getByRole("button", {
    name: "Submit",
});
```

---

# 11. `getByLabelText()`

Very useful for form inputs.

Component:

```tsx
function Login() {
    return (
        <div>
            <label htmlFor="email">
                Email
            </label>

            <input
                id="email"
                type="email"
            />
        </div>
    );
}
```

Test:

```tsx
const emailInput =
    screen.getByLabelText("Email");
```

This is another accessibility-friendly query.

---

# 12. `getByPlaceholderText()`

Example:

```tsx
<input placeholder="Enter email" />
```

Test:

```tsx
screen.getByPlaceholderText(
    "Enter email"
);
```

Useful sometimes, but generally prefer:

```text
getByRole()
getByLabelText()
```

when possible.

---

# 13. Query Priority

A good general order is:

```text
1. getByRole()
2. getByLabelText()
3. getByText()
4. getByPlaceholderText()
5. getByTestId()
```

Why?

Because the first options are closer to how users and accessibility tools interact with your application.

---

# 14. What Is `data-testid`?

You can add a special testing attribute.

```tsx
<div data-testid="user-card">
    William
</div>
```

Then:

```tsx
screen.getByTestId("user-card");
```

But don't use this everywhere.

Avoid:

```tsx
data-testid="button-1"
data-testid="text-1"
data-testid="div-1"
```

If users can identify the element naturally, use:

```tsx
getByRole()
getByText()
getByLabelText()
```

first.

---

# 15. Complete Example

## `UserCard.tsx`

```tsx
type UserCardProps = {
    name: string;
    email: string;
};

function UserCard({
    name,
    email,
}: UserCardProps) {
    return (
        <div>
            <h2>{name}</h2>

            <p>{email}</p>

            <button>
                View Profile
            </button>
        </div>
    );
}

export default UserCard;
```

---

## `UserCard.test.tsx`

```tsx
import {
    render,
    screen,
} from "@testing-library/react";

import UserCard from "./UserCard";

describe("UserCard", () => {
    test("displays user information", () => {
        render(
            <UserCard
                name="William"
                email="william@email.com"
            />
        );

        expect(
            screen.getByText("William")
        ).toBeInTheDocument();

        expect(
            screen.getByText("william@email.com")
        ).toBeInTheDocument();
    });

    test("displays profile button", () => {
        render(
            <UserCard
                name="William"
                email="william@email.com"
            />
        );

        expect(
            screen.getByRole("button", {
                name: "View Profile",
            })
        ).toBeInTheDocument();
    });
});
```

---

# 16. What Are We Actually Testing?

We're NOT testing:

```text
Does React create the component correctly internally?
```

We're testing:

```text
Given these props
      ↓
Does the user see the correct information?
```

Example:

```text
INPUT

name = William
email = william@email.com

        ↓

USER SEES

William
william@email.com
[View Profile]
```

That is the React Testing Library mindset.

---

# 17. `getBy`, `queryBy`, and `findBy`

These are important.

## `getBy`

Use when the element should exist immediately.

```tsx
screen.getByText("Hello");
```

If it doesn't exist:

```text
Test fails immediately
```

---

## `queryBy`

Use when you want to check that something does NOT exist.

```tsx
expect(
    screen.queryByText("Loading...")
).not.toBeInTheDocument();
```

Unlike `getBy`, it doesn't throw an error when the element is missing.

---

## `findBy`

Used for asynchronous elements.

Example:

```tsx
const user = await screen.findByText(
    "William"
);
```

React Testing Library waits for the element to appear.

Useful for:

```text
API data
Async UI updates
Delayed rendering
```

---

# 18. Important Comparison

| Query     | Use                             |
| --------- | ------------------------------- |
| `getBy`   | Element should exist now        |
| `queryBy` | Check if element does not exist |
| `findBy`  | Element appears asynchronously  |

Mental model:

```text
getBy
↓
"I expect it now"


queryBy
↓
"Check whether it exists"


findBy
↓
"Wait for it to appear"
```

---

# 19. Key Takeaway

React Testing Library gives us tools like:

```tsx
render()
screen.getByRole()
screen.getByText()
screen.getByLabelText()
```

And our testing mindset should be:

```text
Don't test React internals
        ↓
Test what the user sees and does
```

---

# Phase 11 Progress

```text
[x] Step 1 — Testing Fundamentals
[x] Step 2 — Jest Setup
[x] Step 3 — Jest Matchers + Unit Tests
[x] Step 4 — React Testing Library

[ ] Step 5 — Testing User Interactions
[ ] Step 6 — Mocking API Calls
[ ] Step 7 — E2E Testing with Playwright
```

# Next → Step 5: Testing User Interactions

This is where we'll test things like:

```text
User clicks button
↓
State changes
↓
UI updates
```

This is where React Testing Library becomes much more practical.

------------------------------------------------------------------------------------------------------------------------------------------


# Step 5 — Testing User Interactions

Now we test what happens when a user actually interacts with a React component.

So far:

```text
render component
↓
Check if text/elements exist
```

Now:

```text
User clicks/types
↓
State changes
↓
UI updates
↓
Test the result
```

---

# 1. Example: Counter Component

## `Counter.tsx`

```tsx
import { useState } from "react";

function Counter() {
    const [count, setCount] = useState(0);

    function handleIncrease() {
        setCount(count + 1);
    }

    return (
        <div>
            <h1>Count: {count}</h1>

            <button onClick={handleIncrease}>
                Increase
            </button>
        </div>
    );
}

export default Counter;
```

Normal user behavior:

```text
Initial:

Count: 0

User clicks Increase
        ↓
Count: 1
```

Let's test that.

---

# 2. `fireEvent`

React Testing Library provides ways to simulate user interactions.

One older/basic way is:

```tsx
fireEvent
```

Example:

```tsx
import {
    render,
    screen,
    fireEvent,
} from "@testing-library/react";
```

Test:

```tsx
import {
    render,
    screen,
    fireEvent,
} from "@testing-library/react";

import Counter from "./Counter";

test("increases count when button is clicked", () => {
    render(<Counter />);

    const button = screen.getByRole("button", {
        name: "Increase",
    });

    fireEvent.click(button);

    expect(
        screen.getByText("Count: 1")
    ).toBeInTheDocument();
});
```

Flow:

```text
Render Counter
      ↓
Find Increase button
      ↓
Click button
      ↓
State changes
      ↓
Check Count: 1
```

---

# 3. But Modern Testing Prefers `userEvent`

Instead of:

```tsx
fireEvent.click(button);
```

Modern React Testing Library projects often use:

```tsx
userEvent.click(button);
```

Why?

Because `userEvent` behaves more like a real user.

For example:

```text
Real User
↓
Mouse down
↓
Focus
↓
Mouse up
↓
Click
```

`userEvent` simulates interactions more realistically.

---

# 4. Install User Event

```bash
npm install -D @testing-library/user-event
```

Then:

```tsx
import userEvent from "@testing-library/user-event";
```

---

# 5. Counter Test Using `userEvent`

```tsx
import {
    render,
    screen,
} from "@testing-library/react";

import userEvent from "@testing-library/user-event";

import Counter from "./Counter";

test("increases count when button is clicked", async () => {
    const user = userEvent.setup();

    render(<Counter />);

    const button = screen.getByRole("button", {
        name: "Increase",
    });

    await user.click(button);

    expect(
        screen.getByText("Count: 1")
    ).toBeInTheDocument();
});
```

Notice:

```tsx
async () => {
```

and:

```tsx
await user.click(button);
```

Because user interactions can be asynchronous.

---

# 6. Full Mental Model

```text
TEST

Render
  ↓
Find element
  ↓
User interacts
  ↓
React state changes
  ↓
Component re-renders
  ↓
Check UI
```

This is the core pattern for interaction testing.

---

# 7. Testing Multiple Clicks

```tsx
test("increases count multiple times", async () => {
    const user = userEvent.setup();

    render(<Counter />);

    const button = screen.getByRole("button", {
        name: "Increase",
    });

    await user.click(button);
    await user.click(button);
    await user.click(button);

    expect(
        screen.getByText("Count: 3")
    ).toBeInTheDocument();
});
```

We are testing from the user's perspective.

```text
User clicks 3 times
       ↓
User should see Count: 3
```

---

# 8. Testing Input Fields

Let's create a simple component.

## `NameInput.tsx`

```tsx
import { useState } from "react";

function NameInput() {
    const [name, setName] = useState("");

    return (
        <div>
            <label htmlFor="name">
                Name
            </label>

            <input
                id="name"
                value={name}
                onChange={(event) =>
                    setName(event.target.value)
                }
            />

            <p>Hello, {name}</p>
        </div>
    );
}

export default NameInput;
```

User behavior:

```text
User types:

William

↓

Input value changes

↓

Hello, William
```

---

# 9. Testing Typing

```tsx
import {
    render,
    screen,
} from "@testing-library/react";

import userEvent from "@testing-library/user-event";

import NameInput from "./NameInput";

test("updates name when user types", async () => {
    const user = userEvent.setup();

    render(<NameInput />);

    const input = screen.getByLabelText("Name");

    await user.type(input, "William");

    expect(input).toHaveValue("William");

    expect(
        screen.getByText("Hello, William")
    ).toBeInTheDocument();
});
```

Flow:

```text
Render
 ↓
Find input
 ↓
User types "William"
 ↓
onChange runs
 ↓
setName("William")
 ↓
Component re-renders
 ↓
Check updated UI
```

---

# 10. Common `userEvent` Interactions

## Click

```tsx
await user.click(button);
```

## Type

```tsx
await user.type(input, "Hello");
```

## Clear an input

```tsx
await user.clear(input);
```

## Select an option

```tsx
await user.selectOptions(select, "admin");
```

## Keyboard interaction

```tsx
await user.keyboard("{Enter}");
```

These simulate real user behavior.

---

# 11. Testing Conditional Rendering

Example component:

## `Toggle.tsx`

```tsx
import { useState } from "react";

function Toggle() {
    const [isVisible, setIsVisible] = useState(false);

    return (
        <div>
            <button
                onClick={() =>
                    setIsVisible(!isVisible)
                }
            >
                Toggle Message
            </button>

            {isVisible && (
                <p>Hello World</p>
            )}
        </div>
    );
}

export default Toggle;
```

---

# 12. Test Before Clicking

Initially:

```text
Hello World
```

should NOT exist.

So we use:

```tsx
queryByText()
```

```tsx
test("shows message when button is clicked", async () => {
    const user = userEvent.setup();

    render(<Toggle />);

    expect(
        screen.queryByText("Hello World")
    ).not.toBeInTheDocument();
});
```

Why `queryBy`?

Because:

```text
getByText()
↓
Throws error if element doesn't exist


queryByText()
↓
Returns null if element doesn't exist
```

Perfect for testing absence.

---

# 13. Complete Toggle Test

```tsx
test("shows message when button is clicked", async () => {
    const user = userEvent.setup();

    render(<Toggle />);

    const button = screen.getByRole("button", {
        name: "Toggle Message",
    });

    // Initially hidden
    expect(
        screen.queryByText("Hello World")
    ).not.toBeInTheDocument();

    // User clicks
    await user.click(button);

    // Now visible
    expect(
        screen.getByText("Hello World")
    ).toBeInTheDocument();
});
```

---

# 14. Testing Forms

Example:

## `LoginForm.tsx`

```tsx
import { useState } from "react";

function LoginForm() {
    const [message, setMessage] = useState("");

    function handleSubmit(
        event: React.FormEvent<HTMLFormElement>
    ) {
        event.preventDefault();

        setMessage("Login successful");
    }

    return (
        <div>
            <form onSubmit={handleSubmit}>
                <label htmlFor="email">
                    Email
                </label>

                <input
                    id="email"
                    type="email"
                />

                <button type="submit">
                    Login
                </button>
            </form>

            {message && <p>{message}</p>}
        </div>
    );
}

export default LoginForm;
```

---

# 15. Testing Form Submission

```tsx
test("shows success message after login", async () => {
    const user = userEvent.setup();

    render(<LoginForm />);

    const emailInput =
        screen.getByLabelText("Email");

    const loginButton =
        screen.getByRole("button", {
            name: "Login",
        });

    await user.type(
        emailInput,
        "test@email.com"
    );

    await user.click(loginButton);

    expect(
        screen.getByText("Login successful")
    ).toBeInTheDocument();
});
```

Flow:

```text
Render form
    ↓
Find email input
    ↓
User types email
    ↓
User clicks Login
    ↓
Form submits
    ↓
State changes
    ↓
Success message appears
```

---

# 16. Testing From the User's Perspective

Bad testing mindset:

```text
Did setState run?
Did handleSubmit run?
Did the internal function execute?
```

Better mindset:

```text
User clicked Login

↓

What should the user see?
```

That is the key React Testing Library philosophy.

---

# 17. Complete Testing Pattern

Most interaction tests follow:

```tsx
test("description", async () => {
    const user = userEvent.setup();

    // 1. Render
    render(<Component />);

    // 2. Find
    const element = screen.getByRole(...);

    // 3. Interact
    await user.click(element);

    // 4. Assert
    expect(...).toBe(...);
});
```

---

# 18. `userEvent` vs `fireEvent`

| `userEvent`                     | `fireEvent`              |
| ------------------------------- | ------------------------ |
| Simulates real user behavior    | Directly triggers events |
| Preferred for most interactions | Lower-level              |
| More realistic                  | Simpler/basic            |

Recommended for your projects:

```text
userEvent
```

---

# What You Should Remember

```text
Testing User Interactions

render()
↓
Render component

screen.getBy...
↓
Find element

userEvent
↓
Simulate user interaction

expect()
↓
Check updated UI
```

The main flow:

```text
USER ACTION
↓
STATE CHANGE
↓
RE-RENDER
↓
ASSERT UI
```

---

# Phase 11 Progress

```text
[x] Step 1 — Testing Fundamentals
[x] Step 2 — Jest Setup
[x] Step 3 — Jest Matchers + Unit Tests
[x] Step 4 — React Testing Library
[x] Step 5 — Testing User Interactions

[ ] Step 6 — Mocking API Calls
[ ] Step 7 — E2E Testing with Playwright
```

# Next → Step 6: Mocking API Calls

This is important because when testing components that fetch API data, we usually don't want our tests to depend on a real external API.

-----------------------------------------------------------------------------------------------------------------------------------------


# Step 6 — Mocking API Calls

This is important because real API calls can make tests unreliable.

---

# 1. The Problem With Real APIs in Tests

Imagine this component:

```tsx
function Users() {
    const [users, setUsers] = useState([]);

    useEffect(() => {
        fetch("https://api.example.com/users")
            .then((response) => response.json())
            .then((data) => setUsers(data));
    }, []);

    return (
        <div>
            {users.map((user) => (
                <p key={user.id}>{user.name}</p>
            ))}
        </div>
    );
}
```

If we test this using the real API:

```text
Run test
   ↓
Make real internet request
   ↓
Wait for server
   ↓
Get response
```

Problems:

```text
❌ Internet might fail
❌ API server might be down
❌ API data might change
❌ Tests become slower
❌ External API controls your test
```

---

# 2. What Is Mocking?

Mocking means:

> Replacing a real dependency with a fake version during testing.

Instead of:

```text
Component
   ↓
Real API
```

We do:

```text
Component
   ↓
Fake API response
```

Example:

Real API might return:

```json
[
    {
        "id": 1,
        "name": "John"
    }
]
```

In our test, we create that response ourselves.

---

# 3. Mental Model

```text
REAL APPLICATION

Component
    ↓
Real API
    ↓
Real Server
```

During testing:

```text
TEST

Component
    ↓
Mock
    ↓
Fake Response
```

This makes tests predictable.

---

# 4. Simple Example Using Jest Mock

Suppose we have:

## `api.ts`

```ts
export async function getUsers() {
    const response = await fetch(
        "https://example.com/users"
    );

    return response.json();
}
```

And our component:

## `Users.tsx`

```tsx
import { useEffect, useState } from "react";
import { getUsers } from "./api";

type User = {
    id: number;
    name: string;
};

function Users() {
    const [users, setUsers] = useState<User[]>([]);

    useEffect(() => {
        getUsers().then((data) => {
            setUsers(data);
        });
    }, []);

    return (
        <div>
            <h1>Users</h1>

            {users.map((user) => (
                <p key={user.id}>
                    {user.name}
                </p>
            ))}
        </div>
    );
}

export default Users;
```

---

# 5. Mocking the API Function

In the test:

## `Users.test.tsx`

```tsx
import {
    render,
    screen,
} from "@testing-library/react";

import Users from "./Users";
import { getUsers } from "./api";

jest.mock("./api");
```

This tells Jest:

```text
Don't use the real api.ts implementation.
Use a mock instead.
```

---

# 6. Providing Fake Data

```tsx
const mockedGetUsers =
    getUsers as jest.MockedFunction<typeof getUsers>;

mockedGetUsers.mockResolvedValue([
    {
        id: 1,
        name: "William",
    },
    {
        id: 2,
        name: "John",
    },
]);
```

Now:

```text
Component calls getUsers()
        ↓
Mock intercepts it
        ↓
Returns fake users
```

No real API request happens.

---

# 7. Complete Test

```tsx
import {
    render,
    screen,
} from "@testing-library/react";

import Users from "./Users";
import { getUsers } from "./api";

jest.mock("./api");

const mockedGetUsers =
    getUsers as jest.MockedFunction<typeof getUsers>;

test("displays users from API", async () => {
    mockedGetUsers.mockResolvedValue([
        {
            id: 1,
            name: "William",
        },
        {
            id: 2,
            name: "John",
        },
    ]);

    render(<Users />);

    expect(
        await screen.findByText("William")
    ).toBeInTheDocument();

    expect(
        screen.getByText("John")
    ).toBeInTheDocument();
});
```

---

# 8. Why `findByText()`?

Remember:

```text
getBy
↓
Element exists immediately


findBy
↓
Wait for asynchronous element
```

API calls are asynchronous.

Initially:

```text
Users
```

Then:

```text
API resolves
    ↓
setUsers()
    ↓
Component re-renders
    ↓
William appears
```

So:

```tsx
await screen.findByText("William");
```

means:

> Wait until William appears.

---

# 9. Testing Loading State

Let's improve our component.

## `Users.tsx`

```tsx
import { useEffect, useState } from "react";
import { getUsers } from "./api";

type User = {
    id: number;
    name: string;
};

function Users() {
    const [users, setUsers] = useState<User[]>([]);
    const [loading, setLoading] = useState(true);

    useEffect(() => {
        getUsers()
            .then((data) => {
                setUsers(data);
            })
            .finally(() => {
                setLoading(false);
            });
    }, []);

    if (loading) {
        return <p>Loading...</p>;
    }

    return (
        <div>
            <h1>Users</h1>

            {users.map((user) => (
                <p key={user.id}>
                    {user.name}
                </p>
            ))}
        </div>
    );
}

export default Users;
```

Now the UI has states:

```text
1. Loading
2. Success
```

We should test both.

---

# 10. Testing Loading State

```tsx
test("shows loading initially", () => {
    mockedGetUsers.mockImplementation(
        () => new Promise(() => {})
    );

    render(<Users />);

    expect(
        screen.getByText("Loading...")
    ).toBeInTheDocument();
});
```

This Promise never resolves:

```text
getUsers()
   ↓
Still waiting forever
   ↓
Loading remains true
```

Therefore:

```text
Loading...
```

should appear.

---

# 11. Testing Success State

```tsx
test("shows users after successful API call", async () => {
    mockedGetUsers.mockResolvedValue([
        {
            id: 1,
            name: "William",
        },
    ]);

    render(<Users />);

    expect(
        await screen.findByText("William")
    ).toBeInTheDocument();
});
```

Flow:

```text
Component renders
      ↓
Loading...
      ↓
Mock API resolves
      ↓
setUsers()
      ↓
Loading = false
      ↓
William appears
```

---

# 12. Testing Error State

A good application handles errors.

Let's update the component:

```tsx
function Users() {
    const [users, setUsers] = useState<User[]>([]);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState("");

    useEffect(() => {
        getUsers()
            .then((data) => {
                setUsers(data);
            })
            .catch(() => {
                setError("Failed to load users");
            })
            .finally(() => {
                setLoading(false);
            });
    }, []);

    if (loading) {
        return <p>Loading...</p>;
    }

    if (error) {
        return <p>{error}</p>;
    }

    return (
        <div>
            <h1>Users</h1>

            {users.map((user) => (
                <p key={user.id}>
                    {user.name}
                </p>
            ))}
        </div>
    );
}
```

Now we have three states:

```text
Loading
   ↓
Success
OR
Error
```

---

# 13. Testing API Failure

```tsx
test("shows error when API request fails", async () => {
    mockedGetUsers.mockRejectedValue(
        new Error("API Error")
    );

    render(<Users />);

    expect(
        await screen.findByText(
            "Failed to load users"
        )
    ).toBeInTheDocument();
});
```

Flow:

```text
Component
   ↓
getUsers()
   ↓
Mock rejects
   ↓
catch()
   ↓
setError()
   ↓
Error message appears
```

---

# 14. The Three Important API States

Whenever you're dealing with API data, think:

```text
API REQUEST

        ↓

   ┌────┴────┐
   ↓         ↓

SUCCESS    ERROR
```

And before the result:

```text
LOADING
```

So your tests often check:

```text
✓ Loading
✓ Success
✓ Error
```

---

# 15. Very Important Testing Principle

Don't test:

```text
Did fetch() internally execute exactly like this?
```

Instead test:

```text
When API succeeds,
does the user see the data?


When API fails,
does the user see an error?


While waiting,
does the user see loading?
```

This is closer to how users experience the application.

---

# 16. Mocking vs Real API

| Real API                               | Mock API           |
| -------------------------------------- | ------------------ |
| Requires internet                      | No internet needed |
| Can be slow                            | Fast               |
| Data can change                        | Controlled data    |
| Server can fail randomly               | Predictable        |
| Less reliable for unit/component tests | Reliable           |

---

# 17. Important Note: Modern Applications

For larger applications, developers often use tools such as:

```text
MSW (Mock Service Worker)
```

MSW can intercept network requests and provide fake responses.

Conceptually:

```text
Component
   ↓
fetch("/api/users")
   ↓
MSW intercepts request
   ↓
Fake API response
```

This is more realistic for larger projects.

For now, understand the core idea first:

```text
Mocking
↓
Replace real dependency

Mock data
↓
Control the result

Test
↓
Check UI behavior
```

---

# Complete Mental Model

```text
REAL APP

Users Component
      ↓
   getUsers()
      ↓
   Real API


TEST

Users Component
      ↓
   getUsers()
      ↓
   Jest Mock
      ↓
 Fake Data / Fake Error
```

---

# What You Should Remember

```text
Mocking API Calls

jest.mock()
↓
Replace real implementation

mockResolvedValue()
↓
Fake successful response

mockRejectedValue()
↓
Fake failed response

findBy...
↓
Wait for async UI
```

And always think:

```text
LOADING
SUCCESS
ERROR
```

These are the three major API states you should test.

---

# Phase 11 Progress

```text
[x] Step 1 — Testing Fundamentals
[x] Step 2 — Jest Setup
[x] Step 3 — Jest Matchers + Unit Tests
[x] Step 4 — React Testing Library
[x] Step 5 — Testing User Interactions
[x] Step 6 — Mocking API Calls

[ ] Step 7 — E2E Testing with Playwright
```

# Next → Step 7: End-to-End (E2E) Testing with Playwright

This is the final testing topic. Here we'll test the application through an actual browser, just like a real user.

-----------------------------------------------------------------------------------------------------------------------------------------

# Step 7 — End-to-End (E2E) Testing with Playwright

This is the final step of **Phase 11: Testing**.

So far, our tests mostly ran in a simulated environment.

Now we test a real application flow through a browser.

---

# 1. What Is E2E Testing?

E2E means:

```text
End-to-End Testing
```

It tests the complete user journey.

Example:

```text
User opens website
      ↓
Clicks Login
      ↓
Enters email
      ↓
Enters password
      ↓
Clicks Submit
      ↓
Dashboard appears
```

We test the entire flow from beginning to end.

---

# 2. Why Is E2E Different?

Let's compare.

### Unit Test

```text
Test one function

add(2, 3)
↓
5
```

### Component Test

```text
Render Login component
↓
Type email
↓
Click Login
↓
Check message
```

### E2E Test

```text
Open real application in browser
↓
Navigate pages
↓
Interact with UI
↓
Test complete flow
```

---

# 3. What Is Playwright?

Playwright is a tool for browser automation and E2E testing.

It can control browsers automatically.

```text
Playwright
    ↓
Opens Browser
    ↓
Clicks buttons
    ↓
Types text
    ↓
Navigates pages
    ↓
Checks results
```

The test behaves similarly to a real user.

---

# 4. Install Playwright

In a React project:

```bash
npm init playwright@latest
```

During setup, it may ask questions such as:

```text
Use TypeScript?
Install browsers?
Add GitHub Actions workflow?
```

A typical project structure becomes:

```text
project/
│
├── src/
├── tests/
│   └── example.spec.ts
│
├── playwright.config.ts
└── package.json
```

---

# 5. Your First Playwright Test

Example:

```ts
import { test, expect } from "@playwright/test";

test("homepage has correct title", async ({ page }) => {
    await page.goto("http://localhost:5173");

    await expect(page).toHaveTitle(/React/);
});
```

Let's understand it.

---

# 6. `test()`

```ts
test("homepage has correct title", async ({ page }) => {
```

Creates an E2E test.

Similar to Jest:

```ts
test("description", () => {
});
```

---

# 7. `page`

```ts
async ({ page }) => {
```

`page` represents a browser page.

Think:

```text
page
↓
Browser tab
```

Playwright gives us control over that page.

---

# 8. `page.goto()`

```ts
await page.goto("http://localhost:5173");
```

This means:

```text
Browser opens
     ↓
Navigate to:
http://localhost:5173
```

Just like typing a URL into your browser.

---

# 9. `expect()`

```ts
await expect(page).toHaveTitle(/React/);
```

Checks whether the page has the expected title.

---

# 10. Testing a Button Click

Imagine our application:

```tsx
function App() {
    const [count, setCount] = useState(0);

    return (
        <div>
            <h1>Count: {count}</h1>

            <button
                onClick={() => setCount(count + 1)}
            >
                Increase
            </button>
        </div>
    );
}
```

Playwright test:

```ts
import { test, expect } from "@playwright/test";

test("user can increase counter", async ({ page }) => {
    await page.goto("http://localhost:5173");

    await page
        .getByRole("button", {
            name: "Increase",
        })
        .click();

    await expect(
        page.getByText("Count: 1")
    ).toBeVisible();
});
```

---

# 11. The Flow

```text
Playwright Test
      ↓
Open Browser
      ↓
Open Application
      ↓
Find Increase Button
      ↓
Click Button
      ↓
React State Changes
      ↓
UI Updates
      ↓
Check Count: 1
```

This is a real browser interaction.

---

# 12. Testing Form Input

Imagine a login page:

```tsx
function Login() {
    return (
        <form>
            <label>
                Email
                <input type="email" />
            </label>

            <label>
                Password
                <input type="password" />
            </label>

            <button>
                Login
            </button>
        </form>
    );
}
```

Playwright test:

```ts
test("user can fill login form", async ({ page }) => {
    await page.goto("http://localhost:5173");

    await page
        .getByLabel("Email")
        .fill("test@email.com");

    await page
        .getByLabel("Password")
        .fill("password123");

    await page
        .getByRole("button", {
            name: "Login",
        })
        .click();
});
```

---

# 13. Playwright Locators

Locators help us find elements.

Similar to React Testing Library:

```text
getByRole()
getByText()
getByLabel()
```

Example:

### Button

```ts
page.getByRole("button", {
    name: "Login",
});
```

### Text

```ts
page.getByText("Welcome");
```

### Input

```ts
page.getByLabel("Email");
```

### Link

```ts
page.getByRole("link", {
    name: "Dashboard",
});
```

Notice something interesting:

```text
React Testing Library
        ↓
getByRole()
getByText()
getByLabelText()


Playwright
        ↓
getByRole()
getByText()
getByLabel()
```

The philosophy is similar.

---

# 14. Testing Navigation

Imagine:

```text
Home
 │
 └── Click Products
          ↓
      Products Page
```

Test:

```ts
test("user can navigate to products page", async ({ page }) => {
    await page.goto("http://localhost:5173");

    await page
        .getByRole("link", {
            name: "Products",
        })
        .click();

    await expect(
        page.getByRole("heading", {
            name: "Products",
        })
    ).toBeVisible();
});
```

---

# 15. Complete E2E Example

Imagine a Todo App.

User flow:

```text
Open App
↓
Enter todo
↓
Click Add
↓
Todo appears
```

Test:

```ts
import { test, expect } from "@playwright/test";

test("user can add a todo", async ({ page }) => {
    await page.goto("http://localhost:5173");

    const input = page.getByLabel("Todo");

    await input.fill("Learn React Testing");

    await page
        .getByRole("button", {
            name: "Add",
        })
        .click();

    await expect(
        page.getByText("Learn React Testing")
    ).toBeVisible();
});
```

This tests the entire user experience.

---

# 16. Unit vs RTL vs E2E

| Type                  | What it Tests        |
| --------------------- | -------------------- |
| Unit Test             | Small functions      |
| React Testing Library | React components     |
| Playwright E2E        | Complete application |

Example Todo App:

### Unit

```text
Does calculateTotal() work?
```

### RTL

```text
Does clicking Add show the todo?
```

### E2E

```text
Open real app
Type todo
Click Add
See todo
```

---

# 17. Important E2E Testing Mindset

E2E tests should focus on important user flows.

Examples:

```text
✓ Login
✓ Signup
✓ Add to cart
✓ Checkout
✓ Create post
✓ Search products
```

Don't write E2E tests for every tiny thing.

Why?

Because E2E tests are generally:

```text
Slower
More complex
Heavier
```

A common strategy:

```text
Many Unit Tests
        ↓
Some Component Tests
        ↓
Few Important E2E Tests
```

This is often represented as the Testing Pyramid.

```text
          /\
         /E2E\
        /------\
       /Integration\
      /------------\
     / Unit Tests   \
    /________________\
```

---

# 18. How to Run Playwright Tests

Usually:

```bash
npx playwright test
```

Playwright will:

```text
Start tests
↓
Open browser automatically
↓
Run interactions
↓
Show results
```

You can also run tests with a visible browser during development:

```bash
npx playwright test --headed
```

---

# 19. Playwright Can Test Multiple Browsers

One major advantage is browser testing.

```text
Chromium
Firefox
WebKit
```

This helps check whether your application works across browsers.

---

# 20. Complete Mental Model

```text
PLAYWRIGHT E2E TEST

Test starts
    ↓
Browser opens
    ↓
page.goto()
    ↓
Application loads
    ↓
Find element
    ↓
User interaction
    ↓
Application responds
    ↓
expect()
    ↓
Test passes/fails
```

---

# What You Should Remember

```text
E2E
↓
End-to-End Testing

Playwright
↓
Controls a real browser

page
↓
Browser tab/page

page.goto()
↓
Navigate to website

getByRole()
getByText()
getByLabel()
↓
Find elements

click()
fill()
↓
User interactions

expect()
↓
Check result
```

---

# Phase 11 Complete

```text
TESTING

[x] Testing Fundamentals
[x] Jest
[x] Jest Matchers
[x] Unit Testing
[x] React Testing Library
[x] User Interaction Testing
[x] API Mocking
[x] E2E Testing with Playwright
```

# Complete Testing Mental Map

```text
TESTING
│
├── Unit Testing
│   ├── Jest
│   ├── test()
│   ├── expect()
│   └── Matchers
│
├── Component Testing
│   ├── React Testing Library
│   ├── render()
│   ├── screen
│   └── userEvent
│
├── API Testing
│   └── Mocking
│
└── E2E Testing
    └── Playwright
        ├── Browser
        ├── page.goto()
        ├── click()
        └── expect()
```

You have now completed **Phase 11 — Testing**.

-----------------------------------------------------------------------------------------------------------------------------------------