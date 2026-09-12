# Step 13 — Build: Cart System

Now we reach the **project required by Phase 7**:

> **Build: cart system**

This is where we'll stop doing isolated Redux examples and build something closer to a real application.

We will build it step by step.

---

# 1. What Are We Building?

Our application will have products:

```text
Products

Laptop       ₹50,000    [Add to Cart]
Phone        ₹20,000    [Add to Cart]
Headphones    ₹5,000    [Add to Cart]
```

When the user clicks **Add to Cart**:

```text
Products
   ↓
Add to Cart
   ↓
Redux Store
   ↓
Cart
```

Then the cart will show:

```text
Cart

Laptop       ₹50,000    Qty: 1
Phone        ₹20,000    Qty: 2

Total: ₹90,000
```

We'll eventually support:

```text
[x] Add product
[x] Remove product
[x] Increase quantity
[x] Decrease quantity
[x] Calculate total
```

---

# 2. Why Use Redux Here?

This is a good example of shared state.

Imagine:

```text
Navbar
   ↓
Cart count
```

and:

```text
Products
   ↓
Add to cart
```

and:

```text
Cart page
   ↓
Cart items
```

All three need the same cart information.

```text
                  Redux Store
                      │
                    cart
                 /    |    \
                ↓     ↓     ↓
            Navbar Products Cart
```

This is exactly the kind of situation where centralized state can be useful.

---

# 3. First, Create the Cart Slice

Our project becomes:

```text
src
│
├── App.tsx
├── main.tsx
├── Counter.tsx
│
└── store
    ├── store.ts
    ├── hooks.ts
    ├── counterSlice.ts
    └── cartSlice.ts
```

Create:

```text
src/store/cartSlice.ts
```

---

# 4. Define the Product Type

Because we're using TypeScript, first define what a product looks like.

```tsx
type Product = {
    id: number;
    name: string;
    price: number;
};
```

So every product must have:

```text
id
name
price
```

Example:

```text
{
    id: 1,
    name: "Laptop",
    price: 50000
}
```

---

# 5. Define the Cart Item

A cart item needs the product plus its quantity.

```tsx
type CartItem = Product & {
    quantity: number;
};
```

So a cart item looks like:

```text
{
    id: 1,
    name: "Laptop",
    price: 50000,
    quantity: 2
}
```

---

# 6. Create the Initial State

Our cart starts empty:

```tsx
initialState: {
    items: []
}
```

So:

```text
Redux Store
   ↓
cart
   ↓
items: []
```

---

# 7. Create the Slice

Our first version:

```tsx
import { createSlice, PayloadAction } from "@reduxjs/toolkit";

type Product = {
    id: number;
    name: string;
    price: number;
};

type CartItem = Product & {
    quantity: number;
};

type CartState = {
    items: CartItem[];
};

const initialState: CartState = {
    items: []
};

const cartSlice = createSlice({
    name: "cart",

    initialState,

    reducers: {
        addToCart(state, action: PayloadAction<Product>) {
            const product = action.payload;

            const existingItem = state.items.find(
                item => item.id === product.id
            );

            if (existingItem) {
                existingItem.quantity += 1;
            } else {
                state.items.push({
                    ...product,
                    quantity: 1
                });
            }
        }
    }
});

export const { addToCart } = cartSlice.actions;

export default cartSlice.reducer;
```

Don't worry if this looks bigger than our counter.

That's because we're now working with **real application data**.

---

# 8. Understand `action.payload`

This is a new concept.

We have:

```tsx
addToCart(state, action)
```

The `action` contains the data we send when dispatching.

For example:

```tsx
dispatch(
    addToCart({
        id: 1,
        name: "Laptop",
        price: 50000
    })
);
```

The product becomes:

```text
action.payload
```

So:

```tsx
const product = action.payload;
```

means:

> Get the product that the component sent to Redux.

---

# 9. Why `PayloadAction<Product>`?

We wrote:

```tsx
PayloadAction<Product>
```

This tells TypeScript:

> The payload of this action must be a `Product`.

So this is valid:

```text
id
name
price
```

But something like:

```text
name
age
```

would not be a valid Product.

TypeScript helps us catch mistakes.

---

# 10. What Happens When We Add a Product?

Initially:

```text
items = []
```

We click:

```text
Add Laptop
```

and dispatch:

```text
addToCart(Laptop)
```

Redux receives:

```text
action.payload
    ↓
Laptop
```

The reducer checks:

```text
Does Laptop already exist?
```

No.

So:

```text
items.push(Laptop)
```

with:

```text
quantity: 1
```

The state becomes:

```text
items = [
    {
        id: 1,
        name: "Laptop",
        price: 50000,
        quantity: 1
    }
]
```

---

# 11. What Happens If We Add Laptop Again?

The reducer finds:

```text
Laptop already exists
```

So instead of adding another Laptop:

```text
Laptop
Laptop
```

we increase:

```text
quantity
```

From:

```text
quantity: 1
```

to:

```text
quantity: 2
```

So the cart remains:

```text
Laptop
quantity: 2
```

This is how a real cart normally behaves.

---

# 12. Add Cart Reducer to the Store

Open:

```text
src/store/store.ts
```

Currently we have:

```tsx
import { configureStore } from "@reduxjs/toolkit";

import counterReducer from "./counterSlice";

export const store = configureStore({
    reducer: {
        counter: counterReducer
    }
});

export type RootState = ReturnType<typeof store.getState>;

export type AppDispatch = typeof store.dispatch;
```

Add the cart reducer:

```tsx
import { configureStore } from "@reduxjs/toolkit";

import counterReducer from "./counterSlice";
import cartReducer from "./cartSlice";

export const store = configureStore({
    reducer: {
        counter: counterReducer,
        cart: cartReducer
    }
});

export type RootState = ReturnType<typeof store.getState>;

export type AppDispatch = typeof store.dispatch;
```

Now our Redux Store is:

```text
Redux Store
│
├── counter
│    └── value
│
└── cart
     └── items
```

---

# 13. Create Products

Now we need something to add to the cart.

Create:

```text
src/Products.tsx
```

We'll start with some simple products:

```tsx
import { useAppDispatch } from "./store/hooks";
import { addToCart } from "./store/cartSlice";

const products = [
    {
        id: 1,
        name: "Laptop",
        price: 50000
    },
    {
        id: 2,
        name: "Phone",
        price: 20000
    },
    {
        id: 3,
        name: "Headphones",
        price: 5000
    }
];

function Products() {
    const dispatch = useAppDispatch();

    return (
        <div>
            <h1>Products</h1>

            {products.map(product => (
                <div key={product.id}>
                    <h2>{product.name}</h2>

                    <p>₹{product.price}</p>

                    <button
                        onClick={() =>
                            dispatch(addToCart(product))
                        }
                    >
                        Add to Cart
                    </button>
                </div>
            ))}
        </div>
    );
}

export default Products;
```

---

# 14. Notice the Redux Flow

When we click:

```text
[ Add to Cart ]
```

the code executes:

```text
dispatch(addToCart(product))
```

So:

```text
Products
   ↓
dispatch()
   ↓
addToCart(product)
   ↓
cartReducer
   ↓
Redux Store
```

---

# 15. Create the Cart Component

Now create:

```text
src/Cart.tsx
```

```tsx
import {
    useAppDispatch,
    useAppSelector
} from "./store/hooks";

import {
    removeFromCart,
    increaseQuantity,
    decreaseQuantity
} from "./store/cartSlice";

function Cart() {
    const items = useAppSelector(
        state => state.cart.items
    );

    const dispatch = useAppDispatch();

    return (
        <div>
            <h1>Cart</h1>

            {items.map(item => (
                <div key={item.id}>
                    <h2>{item.name}</h2>

                    <p>Price: ₹{item.price}</p>

                    <p>Quantity: {item.quantity}</p>
                </div>
            ))}
        </div>
    );
}

export default Cart;
```

But there's a problem.

We haven't created:

```text
removeFromCart
increaseQuantity
decreaseQuantity
```

yet.

That's intentional.

We'll add those reducers next.

---

# 16. What We Have Learned So Far

Our cart state:

```text
Redux Store
│
└── cart
     │
     └── items
          │
          ├── Laptop
          │    └── quantity
          │
          └── Phone
               └── quantity
```

Products can:

```text
dispatch(addToCart(product))
```

Cart can:

```text
useAppSelector(
    state => state.cart.items
)
```

So we have both sides:

```text
WRITE
  ↓
dispatch()

READ
  ↓
useAppSelector()
```

---

# 17. Important Redux Concept

Notice how `Products` does **not** have to send the cart to `Cart`.

We don't have:

```text
Products
   ↓ cart props
App
   ↓ cart props
Cart
```

Instead:

```text
             Redux Store
             /         \
            ↓           ↓
       Products        Cart
          │               │
       dispatch        selector
```

That's the benefit of centralized state.

---

# 18. Current Project

```text
src
│
├── App.tsx
├── main.tsx
├── Products.tsx
├── Cart.tsx
│
└── store
    ├── store.ts
    ├── hooks.ts
    ├── counterSlice.ts
    └── cartSlice.ts
```

Our Redux Store:

```text
Redux Store
│
├── counter
│    └── value
│
└── cart
     └── items
```

---

# Phase 7 Progress

```text
## 7. State Management

### Context API

[x] Context API
[x] Auth state app

### Redux Toolkit

[x] Why Redux?
[x] Context vs Redux
[x] Install Redux Toolkit
[x] Redux Store
[x] Provider
[x] createSlice()
[x] initialState
[x] reducers
[x] actions
[x] useSelector()
[x] useDispatch()
[x] TypeScript + Redux Toolkit
[x] Start Cart System
[x] Add product to cart

[ ] Remove product
[ ] Increase quantity
[ ] Decrease quantity
[ ] Calculate cart total
[ ] Cart count in Navbar
[ ] Complete Cart System
```

**Next → Step 14: Cart reducers — `removeFromCart`, `increaseQuantity`, and `decreaseQuantity`.**


-------------------------------------------------------------------------------------------------------------------------------------------

# Step 14 — Cart Reducers

Now we'll complete the **cart operations**.

We already have:

```text
addToCart()
```

Now we need:

```text
removeFromCart()
increaseQuantity()
decreaseQuantity()
```

So our cart will behave like a real shopping cart.

---

# 1. What We Have Now

Our cart state looks like:

```text
cart
└── items
     ├── Laptop
     │    ├── price: 50000
     │    └── quantity: 2
     │
     └── Phone
          ├── price: 20000
          └── quantity: 1
```

We want to be able to:

```text
Laptop
Quantity: 2

[ - ] [ + ] [ Remove ]
```

---

# 2. Update `cartSlice.ts`

Open:

```text
src/store/cartSlice.ts
```

Replace the reducers section with:

```tsx id="knxg9h"
reducers: {
    addToCart(state, action: PayloadAction<Product>) {
        const product = action.payload;

        const existingItem = state.items.find(
            item => item.id === product.id
        );

        if (existingItem) {
            existingItem.quantity += 1;
        } else {
            state.items.push({
                ...product,
                quantity: 1
            });
        }
    },

    removeFromCart(state, action: PayloadAction<number>) {
        state.items = state.items.filter(
            item => item.id !== action.payload
        );
    },

    increaseQuantity(state, action: PayloadAction<number>) {
        const item = state.items.find(
            item => item.id === action.payload
        );

        if (item) {
            item.quantity += 1;
        }
    },

    decreaseQuantity(state, action: PayloadAction<number>) {
        const item = state.items.find(
            item => item.id === action.payload
        );

        if (item && item.quantity > 1) {
            item.quantity -= 1;
        }
    }
}
```

And update the export:

```tsx id="o5g4bp"
export const {
    addToCart,
    removeFromCart,
    increaseQuantity,
    decreaseQuantity
} = cartSlice.actions;
```

---

# 3. Understand `removeFromCart()`

We have:

```tsx id="w0d4hi"
removeFromCart(state, action: PayloadAction<number>)
```

The payload is the product ID.

For example:

```text id="xx0qz3"
dispatch(removeFromCart(2))
```

means:

> Remove the product whose ID is `2`.

The reducer does:

```text id="q96l9w"
state.items
    ↓
filter()
    ↓
remove matching ID
```

---

# 4. Example

Before:

```text id="6f9wdu"
items:

Laptop
Phone
Headphones
```

We dispatch:

```text id="a3b9vq"
removeFromCart(2)
```

Result:

```text id="l3q7bl"
Laptop
Headphones
```

Phone is removed.

---

# 5. Understand `increaseQuantity()`

Suppose:

```text id="1g54pg"
Laptop
quantity: 1
```

We click:

```text id="2fypvr"
+
```

We dispatch:

```text id="90q6cb"
increaseQuantity(1)
```

The `1` is the product ID.

The reducer finds the product:

```text id="nd4rj8"
id === 1
```

and does:

```text id="5j9z3k"
quantity += 1
```

Result:

```text id="hkgd2e"
Laptop
quantity: 2
```

---

# 6. Understand `decreaseQuantity()`

Suppose:

```text id="85h9d1"
Laptop
quantity: 3
```

Click:

```text id="e70zfj"
-
```

We dispatch:

```text id="d67rru"
decreaseQuantity(1)
```

Result:

```text id="1x8p1t"
quantity: 2
```

And:

```text id="1x8p1t"
2 → 1
```

---

# 7. Why Do We Check `quantity > 1`?

Our reducer has:

```tsx id="n20z7q"
if (item && item.quantity > 1) {
    item.quantity -= 1;
}
```

This means:

```text id="j7l2t8"
quantity = 1
       ↓
click -
       ↓
stays 1
```

We don't allow:

```text id="j7l2t8"
quantity = 0
```

At least for this version of the cart.

Later, we could choose to remove the item when quantity reaches zero.

---

# 8. Now Update `Cart.tsx`

Our Cart component can now use all three actions.

```tsx id="trj6be"
import {
    useAppDispatch,
    useAppSelector
} from "./store/hooks";

import {
    removeFromCart,
    increaseQuantity,
    decreaseQuantity
} from "./store/cartSlice";

function Cart() {
    const items = useAppSelector(
        state => state.cart.items
    );

    const dispatch = useAppDispatch();

    return (
        <div>
            <h1>Cart</h1>

            {items.length === 0 ? (
                <p>Your cart is empty.</p>
            ) : (
                items.map(item => (
                    <div key={item.id}>
                        <h2>{item.name}</h2>

                        <p>
                            Price: ₹{item.price}
                        </p>

                        <p>
                            Quantity: {item.quantity}
                        </p>

                        <button
                            onClick={() =>
                                dispatch(
                                    decreaseQuantity(item.id)
                                )
                            }
                        >
                            -
                        </button>

                        <button
                            onClick={() =>
                                dispatch(
                                    increaseQuantity(item.id)
                                )
                            }
                        >
                            +
                        </button>

                        <button
                            onClick={() =>
                                dispatch(
                                    removeFromCart(item.id)
                                )
                            }
                        >
                            Remove
                        </button>
                    </div>
                ))
            )}
        </div>
    );
}

export default Cart;
```

---

# 9. Understand the `item.id`

Look at:

```tsx id="15xgiy"
dispatch(increaseQuantity(item.id))
```

Suppose:

```text id="w5txjn"
item = Laptop
item.id = 1
```

Then this becomes:

```text id="3mj6sc"
dispatch(increaseQuantity(1))
```

Redux receives:

```text id="4wd1y0"
action.payload = 1
```

The reducer searches for:

```text id="c5s1ri"
item.id === 1
```

and changes that item's quantity.

---

# 10. Our Cart Now Works Like This

### Add

```text id="j2w9gn"
Products
   ↓
Add to Cart
   ↓
dispatch(addToCart(product))
   ↓
Redux
   ↓
cart.items
```

### Increase

```text id="2f8zq6"
+
 ↓
dispatch(increaseQuantity(id))
 ↓
Redux
 ↓
quantity + 1
```

### Decrease

```text id="40k56d"
-
 ↓
dispatch(decreaseQuantity(id))
 ↓
Redux
 ↓
quantity - 1
```

### Remove

```text id="1jcey0"
Remove
 ↓
dispatch(removeFromCart(id))
 ↓
Redux
 ↓
item removed
```

---

# 11. The Complete Cart Flow

This is worth understanding carefully:

```text id="k7xk5w"
                  Products
                     │
                     │ dispatch(addToCart)
                     ↓
                Redux Store
                     │
                     ↓
                    cart
                     │
                     ↓
                    items
                     │
                     ↓
                   Cart
                     │
          ┌──────────┼──────────┐
          ↓          ↓          ↓
      increase    decrease    remove
          │          │          │
          └──────────┼──────────┘
                     ↓
                   Redux
                     ↓
               state changes
                     ↓
                    Cart
```

---

# 12. Why This Is Better Than Keeping Cart in `Products`

Imagine we put:

```text id="t2q6by"
cart state
```

inside `Products`.

Then `Cart` would need access to it.

We would potentially end up passing:

```text id="yq1m4k"
Products
   ↓
App
   ↓
Cart
```

Redux gives us:

```text id="42r9x4"
Products ────────┐
                 ↓
             Redux Store
                 ↑
                 │
Cart ────────────┘
```

Both components communicate through the shared store.

---

# 13. One Important Thing About Redux

Redux is **not storing the UI**.

It's storing **application state**.

For example:

```text id="3twjco"
Redux
│
└── cart
     └── items
```

But things like:

```text id="g5r1jt"
Is this dropdown open?
Is this tooltip visible?
```

usually don't need to go into Redux.

Those can remain local:

```text id="1r4xjw"
useState()
```

Don't put everything into Redux.

---

# 14. Current Project

Our project now looks like:

```text id="a0z2tr"
src
│
├── App.tsx
├── main.tsx
├── Products.tsx
├── Cart.tsx
│
└── store
    ├── store.ts
    ├── hooks.ts
    ├── counterSlice.ts
    └── cartSlice.ts
```

Redux:

```text id="y7k3py"
Redux Store
│
├── counter
│    └── value
│
└── cart
     └── items
          ├── product
          └── quantity
```

---

# 15. Phase 7 Progress

```text id="z4s5v8"
## 7. State Management

### Context API

[x] Context API
[x] Auth state app

### Redux Toolkit

[x] Why Redux?
[x] Context vs Redux
[x] Install Redux Toolkit
[x] Redux Store
[x] Provider
[x] createSlice()
[x] initialState
[x] reducers
[x] actions
[x] useSelector()
[x] useDispatch()
[x] TypeScript + Redux Toolkit
[x] Counter with Redux
[x] Add product to cart
[x] Remove product
[x] Increase quantity
[x] Decrease quantity

[ ] Calculate cart total
[ ] Cart count in Navbar
[ ] Complete Cart System
```

**Next → Step 15: Cart total + cart count — we'll calculate the total price and show the number of items in the Navbar.**


-------------------------------------------------------------------------------------------------------------------------------------------

# Step 15 — Cart Total + Cart Count

Now we'll finish the two important pieces:

1. **Cart total price**
2. **Cart item count**

This will teach you another important Redux concept: **derived data**.

---

# 1. Our Current Cart

Suppose the cart contains:

```text
Laptop
₹50,000 × 1

Phone
₹20,000 × 2

Headphones
₹5,000 × 1
```

We want:

```text
Cart Items: 4

Total: ₹95,000
```

Notice something important:

We don't actually need to store:

```text
total: 95000
```

in Redux.

We already have:

```text
price
quantity
```

So we can **calculate** the total.

---

# 2. Calculate the Total

The calculation is:

```text
price × quantity
```

for each item.

Then add everything together.

For example:

```text
Laptop
50,000 × 1 = 50,000

Phone
20,000 × 2 = 40,000

Headphones
5,000 × 1 = 5,000
```

Total:

```text
50,000 + 40,000 + 5,000
= 95,000
```

---

# 3. Use `reduce()`

We already learned JavaScript array methods in the earlier roadmap.

Now we'll use:

```text
reduce()
```

Our cart items:

```text
[
    Laptop,
    Phone,
    Headphones
]
```

We can calculate:

```text
items.reduce(...)
```

The idea is:

```text
items
 ↓
item 1 → price × quantity
 ↓
item 2 → price × quantity
 ↓
item 3 → price × quantity
 ↓
add everything
 ↓
total
```

---

# 4. Update `Cart.tsx`

We'll calculate the total from the Redux state.

```tsx id="3m6v8c"
import {
    useAppDispatch,
    useAppSelector
} from "./store/hooks";

import {
    removeFromCart,
    increaseQuantity,
    decreaseQuantity
} from "./store/cartSlice";

function Cart() {
    const items = useAppSelector(
        state => state.cart.items
    );

    const dispatch = useAppDispatch();

    const total = items.reduce(
        (sum, item) =>
            sum + item.price * item.quantity,
        0
    );

    return (
        <div>
            <h1>Cart</h1>

            {items.length === 0 ? (
                <p>Your cart is empty.</p>
            ) : (
                <>
                    {items.map(item => (
                        <div key={item.id}>
                            <h2>{item.name}</h2>

                            <p>
                                Price: ₹{item.price}
                            </p>

                            <p>
                                Quantity: {item.quantity}
                            </p>

                            <button
                                onClick={() =>
                                    dispatch(
                                        decreaseQuantity(item.id)
                                    )
                                }
                            >
                                -
                            </button>

                            <button
                                onClick={() =>
                                    dispatch(
                                        increaseQuantity(item.id)
                                    )
                                }
                            >
                                +
                            </button>

                            <button
                                onClick={() =>
                                    dispatch(
                                        removeFromCart(item.id)
                                    )
                                }
                            >
                                Remove
                            </button>
                        </div>
                    ))}

                    <h2>Total: ₹{total}</h2>
                </>
            )}
        </div>
    );
}

export default Cart;
```

---

# 5. Understand `total`

This is the important part:

```tsx id="y8m2v4"
const total = items.reduce(
    (sum, item) =>
        sum + item.price * item.quantity,
    0
);
```

Let's break it down.

Initially:

```text
sum = 0
```

First item:

```text
Laptop
50,000 × 1
```

So:

```text
sum = 50,000
```

Next:

```text
Phone
20,000 × 2
```

So:

```text
sum = 90,000
```

Next:

```text
Headphones
5,000 × 1
```

So:

```text
sum = 95,000
```

Final:

```text
total = 95,000
```

---

# 6. Why Not Store `total` in Redux?

This is an important concept.

We already have:

```text
items
 ├── price
 └── quantity
```

Therefore:

```text
total = price × quantity
```

We can calculate it whenever we need it.

If we stored:

```text
items
total
```

we would now have two pieces of information that must stay synchronized.

For example:

```text
quantity changes
       ↓
Did we remember to update total?
```

That creates unnecessary complexity.

So we calculate **derived data**.

---

# 7. What Is Derived Data?

Derived data means:

> Data that can be calculated from existing state.

For example:

```text
Existing state:

items
```

Derived:

```text
total
```

Another example:

```text
items
```

Derived:

```text
number of items
```

So:

```text
Redux State
     ↓
Existing data
     ↓
Calculate
     ↓
Derived data
```

---

# 8. Cart Count

Now let's calculate the number of products in the cart.

Suppose:

```text
Laptop × 1
Phone × 2
Headphones × 1
```

The cart count should be:

```text
4
```

Not:

```text
3
```

because there are three different products but four total units.

We can calculate:

```tsx id="i0j7yp"
const cartCount = items.reduce(
    (count, item) =>
        count + item.quantity,
    0
);
```

---

# 9. Create a Navbar

Create:

```text id="o8t5zi"
src/Navbar.tsx
```

```tsx id="s1m5ye"
import { useAppSelector } from "./store/hooks";

function Navbar() {
    const items = useAppSelector(
        state => state.cart.items
    );

    const cartCount = items.reduce(
        (count, item) =>
            count + item.quantity,
        0
    );

    return (
        <nav>
            <h2>My Shop</h2>

            <p>
                Cart: {cartCount}
            </p>
        </nav>
    );
}

export default Navbar;
```

---

# 10. Understand the Flow

Navbar reads:

```text id="9ezcwj"
state.cart.items
```

Then calculates:

```text id="czg0tw"
quantity + quantity + quantity
```

So:

```text id="v2k4a1"
Laptop × 1
Phone × 2
Headphones × 1
```

becomes:

```text id="x4c4tu"
Cart: 4
```

---

# 11. Why Does Navbar Update Automatically?

Suppose the cart initially has:

```text id="0myg4t"
Cart: 0
```

User adds Laptop:

```text id="sjk7ph"
dispatch(addToCart(product))
```

Redux:

```text id="3i8cyn"
cart.items changes
```

Navbar is using:

```text id="1d9c80"
useAppSelector(
    state => state.cart.items
)
```

Therefore Navbar gets the updated state.

Result:

```text id="v8m4hy"
Cart: 1
```

Add Laptop again:

```text id="i2s5gb"
Cart: 2
```

Add Phone:

```text id="z5g5w7"
Cart: 3
```

---

# 12. The Important Redux Idea

Notice that **Products doesn't communicate directly with Navbar**.

It's:

```text
Products
   │
   ↓
Redux Store
   │
   ↓
Navbar
```

And:

```text
Products
   │
   ↓
Redux Store
   │
   ↓
Cart
```

The Redux Store is the shared state.

---

# 13. Update `App.tsx`

Now let's put everything together.

```tsx id="6b1x5j"
import Navbar from "./Navbar";
import Products from "./Products";
import Cart from "./Cart";

function App() {
    return (
        <div>
            <Navbar />

            <Products />

            <Cart />
        </div>
    );
}

export default App;
```

Now the application flow is:

```text
                 App
                  │
        ┌─────────┼─────────┐
        ↓         ↓         ↓
     Navbar    Products    Cart
        │         │         │
        └─────────┼─────────┘
                  ↓
             Redux Store
```

---

# 14. Full Application Example

Now our application behaves like this:

```text
┌─────────────────────────────┐
│ My Shop                     │
│ Cart: 0                     │
└─────────────────────────────┘

Products

Laptop
₹50,000

[ Add to Cart ]


Phone
₹20,000

[ Add to Cart ]


Headphones
₹5,000

[ Add to Cart ]
```

Click Laptop:

```text
Cart: 1
```

Cart:

```text
Laptop
₹50,000
Quantity: 1

[-] [+] [Remove]

Total: ₹50,000
```

Click `+`:

```text
Cart: 2

Laptop
₹50,000
Quantity: 2

Total: ₹100,000
```

Click Remove:

```text
Cart: 0

Your cart is empty.
```

---

# 15. What You Just Learned

This small project has actually taught us several important Redux concepts.

### Reading state

```text
useAppSelector()
```

### Changing state

```text
useAppDispatch()
```

### Sending data

```text
action.payload
```

### Updating state

```text
reducers
```

### Derived data

```text
reduce()
```

### Shared state

```text
Redux Store
```

---

# 16. Our Redux Architecture

At this point:

```text id="9jglr2"
                     Redux Store
                          │
             ┌────────────┴────────────┐
             ↓                         ↓
         counter                      cart
             │                         │
           value                      items
                                      │
                          ┌───────────┼───────────┐
                          ↓           ↓           ↓
                       Products      Cart       Navbar
                          │           │           │
                       dispatch    selector    selector
```

---

# 17. Phase 7 Progress

```text
## 7. State Management

### Context API

[x] Context API
[x] Auth state app

### Redux Toolkit

[x] Why Redux?
[x] Context vs Redux
[x] Install Redux Toolkit
[x] Redux Store
[x] Provider
[x] createSlice()
[x] initialState
[x] reducers
[x] actions
[x] useSelector()
[x] useDispatch()
[x] TypeScript + Redux Toolkit
[x] Counter with Redux
[x] Add product to cart
[x] Remove product
[x] Increase quantity
[x] Decrease quantity
[x] Calculate cart total
[x] Cart count in Navbar
[x] Complete Cart System
```

## Phase 7 is now complete

According to **your roadmap**, we've completed:

```text
7. State Management
        ↓
Context API
        ↓
Redux Toolkit
        ↓
Form state / validation
```

We still need to cover the **Form state/validation** part:

```text
React Hook Form + Zod
```

That's the next section before we move to Phase 8.

**Next → Step 16: React Hook Form — why form state becomes difficult with normal `useState()` and how React Hook Form solves it.**

-------------------------------------------------------------------------------------------------------------------------------------------