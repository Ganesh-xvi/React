# Build: Fetch API Data App + Search/Filter UI

We'll build it **step by step**, not dump the entire project at once.

The flow will be:

```text
1. Create the React project
        ↓
2. Create basic component
        ↓
3. Fetch API data
        ↓
4. Store data using useState
        ↓
5. Handle loading state
        ↓
6. Handle error state
        ↓
7. Display the data
        ↓
8. Add search
        ↓
9. Add filtering
        ↓
10. Use useMemo where appropriate
```

------------------------------------------------------------------------------------------------------------------------------------------

# Fetch API Data App

We'll build this **step by step**.

The final app will be something like:

```text
User List

Search: [ john             ]

-------------------------
John
john@example.com
-------------------------
Jane
jane@example.com
-------------------------
```

We will use an API to get the users, then add search/filter.

---

# Step 1 — Create the React Project

Since your roadmap says **Vite is preferred**, we'll use Vite.

If you already have a fresh React + TypeScript project, you can use that instead.

Create the project:

```text
npm create vite@latest
```

When Vite asks:

```text
Framework → React
Variant   → TypeScript
```

Then go into the project and install the dependencies.

After that, start the development server.

You should see the default Vite + React page in the browser.

---

# Step 2 — Clean the Default App

For this project, we don't need the default Vite demo.

We want a simple structure:

```text
src
│
├── App.tsx
├── main.tsx
└── ...
```

For now, **don't create lots of folders or files**.

We'll keep the project simple while learning.

---

# Step 3 — Our First Goal

Before adding search or filtering, let's do **only one thing**:

> Fetch data from an API and display it.

We'll use a public API:

```text
https://jsonplaceholder.typicode.com/users
```

The API gives us user information.

Conceptually:

```text
React App
   ↓
useEffect
   ↓
fetch API
   ↓
Users data
   ↓
useState
   ↓
Display users
```

Notice how we're using the Hooks we just learned.

---

# Step 4 — Which Hooks Are We Using?

For this first part:

### `useState`

We'll store the users:

```text
users
 ↓
array of users
```

### `useEffect`

We'll fetch the users when the component starts:

```text
App starts
   ↓
useEffect
   ↓
fetch()
```

So already we're applying what we learned instead of just learning Hooks theoretically.

---

# Step 5 — Basic Code

In `App.tsx`:

```tsx
import { useEffect, useState } from "react";

type User = {
    id: number;
    name: string;
    email: string;
};

function App() {
    const [users, setUsers] = useState<User[]>([]);

    useEffect(() => {
        fetch("https://jsonplaceholder.typicode.com/users")
            .then((response) => response.json())
            .then((data) => {
                setUsers(data);
            });
    }, []);

    return (
        <div>
            <h1>User List</h1>

            {users.map((user) => (
                <div key={user.id}>
                    <h2>{user.name}</h2>
                    <p>{user.email}</p>
                </div>
            ))}
        </div>
    );
}

export default App;
```

---

# Step 6 — Understand the Flow

Don't worry about memorizing the whole code yet.

Look at the important part:

```text
App starts
   ↓
useEffect()
   ↓
fetch()
   ↓
API returns users
   ↓
setUsers(data)
   ↓
users state changes
   ↓
React re-renders
   ↓
users.map()
   ↓
Users appear on screen
```

This is the main React pattern we're practicing.

---

# Step 7 — Why `[]`?

We have:

```tsx
useEffect(() => {
    ...
}, []);
```

Remember what we learned:

```text
[] 
 ↓
run when component initially mounts
```

That's exactly what we want.

We want to fetch the users when the app starts.

We don't want to continuously call the API after every render.

---

# Step 8 — Why `User[]`?

This part:

```tsx
useState<User[]>([]);
```

is TypeScript.

We are saying:

> `users` will be an array containing `User` objects.

And our `User` type says:

```tsx
type User = {
    id: number;
    name: string;
    email: string;
};
```

So TypeScript knows:

```text
user.id     → number
user.name   → string
user.email  → string
```

---

# Step 9 — Why `key={user.id}`?

We have:

```tsx
{users.map((user) => (
    <div key={user.id}>
```

React needs a **key** when rendering a list.

The key helps React identify each item.

For example:

```text
User 1 → key 1
User 2 → key 2
User 3 → key 3
```

We'll talk more about keys when needed, but for now remember:

> **When rendering a list with `.map()`, give each item a unique `key`.**

---

# Your First Target

At this stage, your browser should show something like:

```text
User List

Leanne Graham
Sincere@april.biz

Ervin Howell
Shanna@melissa.tv

Clementine Bauch
Nathan@yesenia.net

...
```

Once this works, **don't add search yet**.

Next step will be:

**Step 10 — Add loading and error states**, so the app doesn't just sit blank while the API is loading or when the request fails.


------------------------------------------------------------------------------------------------------------------------------------------

# Step 1 — Loading State

Our API app is working, but there is one problem.

When the API request is happening:

```text
App starts
   ↓
fetch API
   ↓
⏳ waiting...
   ↓
data arrives
   ↓
users displayed
```

During `⏳ waiting...`, our `users` array is empty, so the screen may look blank.

We can fix this with **loading state**.

---

# 1. Create Loading State

We already have:

```tsx
const [users, setUsers] = useState<User[]>([]);
```

Add:

```tsx
const [loading, setLoading] = useState(true);
```

This means:

```text
loading = true
```

when the app starts.

---

# 2. Turn Loading Off

After the API data arrives:

```tsx
setUsers(data);
setLoading(false);
```

So the flow becomes:

```text
App starts
   ↓
loading = true
   ↓
Fetch API
   ↓
Data arrives
   ↓
setUsers(data)
   ↓
setLoading(false)
   ↓
Show users
```

---

# 3. Show Loading Message

Now we can conditionally render:

```tsx
{loading ? (
    <p>Loading...</p>
) : (
    users.map((user) => (
        <div key={user.id}>
            <h2>{user.name}</h2>
            <p>{user.email}</p>
        </div>
    ))
)}
```

Remember your **conditional rendering** lesson?

We're using it here:

```text
loading = true
     ↓
"Loading..."

loading = false
     ↓
Users
```

---

# 4. Full Code So Far

```tsx
import { useEffect, useState } from "react";

type User = {
    id: number;
    name: string;
    email: string;
};

function App() {
    const [users, setUsers] = useState<User[]>([]);
    const [loading, setLoading] = useState(true);

    useEffect(() => {
        fetch("https://jsonplaceholder.typicode.com/users")
            .then((response) => response.json())
            .then((data) => {
                setUsers(data);
                setLoading(false);
            });
    }, []);

    return (
        <div>
            <h1>User List</h1>

            {loading ? (
                <p>Loading...</p>
            ) : (
                users.map((user) => (
                    <div key={user.id}>
                        <h2>{user.name}</h2>
                        <p>{user.email}</p>
                    </div>
                ))
            )}
        </div>
    );
}

export default App;
```

---

# 5. The Important Part

We now have **two pieces of state**:

```text
users
 ↓
Stores API data

loading
 ↓
Stores whether API is still loading
```

So:

```text
useState
│
├── users
│
└── loading
```

And the UI depends on the state:

```text
loading
   ↓
true  → Loading...
false → Users
```

This is a real example of the **state → UI** pattern you learned earlier.

---

# 6. One Problem Still Remains

What happens if the API fails?

Currently:

```text
App starts
   ↓
fetch API
   ↓
❌ API fails
   ↓
Nothing useful is shown
```

We should handle that too.

### Next → Step 11: **Error State**

We'll add:

```text
loading
error
users
```

and understand how all three states work together.


-------------------------------------------------------------------------------------------------------------------------------------------

# Step 2 — Error State

Now our app handles **loading**, but what if the API request fails?

We need one more state:

```text id="0g2k6q"
users
loading
error
```

---

# 1. Create Error State

Add:

```tsx id="u8r4k1"
const [error, setError] = useState("");
```

Initially:

```text id="6j5m2p"
error = ""
```

That means:

> There is currently no error.

---

# 2. Handle API Errors

Our current fetch is:

```tsx id="d7q3v9"
fetch("https://jsonplaceholder.typicode.com/users")
    .then((response) => response.json())
    .then((data) => {
        setUsers(data);
        setLoading(false);
    });
```

We need to catch errors.

Add:

```tsx id="n2f8w5"
.catch(() => {
    setError("Failed to load users");
    setLoading(false);
});
```

Now the flow is:

```text id="q6m3x8"
fetch API
   ↓
   ├── Success
   │     ↓
   │   setUsers(data)
   │
   └── Failure
         ↓
      setError(...)
```

---

# 3. Display the Error

Now our UI can check the error:

```tsx id="r9k4v2"
{error ? (
    <p>{error}</p>
) : loading ? (
    <p>Loading...</p>
) : (
    users.map((user) => (
        <div key={user.id}>
            <h2>{user.name}</h2>
            <p>{user.email}</p>
        </div>
    ))
)}
```

But there's something important here.

The order matters.

We can think about the UI states like this:

```text id="f4m8n1"
             App
              ↓
        What is happening?
              ↓
     ┌────────┼────────┐
     ↓        ↓        ↓
  Loading   Error    Success
     ↓        ↓        ↓
 Loading...  Error    Users
```

---

# 4. Full Code So Far

```tsx id="e8p3q6"
import { useEffect, useState } from "react";

type User = {
    id: number;
    name: string;
    email: string;
};

function App() {
    const [users, setUsers] = useState<User[]>([]);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState("");

    useEffect(() => {
        fetch("https://jsonplaceholder.typicode.com/users")
            .then((response) => response.json())
            .then((data) => {
                setUsers(data);
                setLoading(false);
            })
            .catch(() => {
                setError("Failed to load users");
                setLoading(false);
            });
    }, []);

    return (
        <div>
            <h1>User List</h1>

            {error ? (
                <p>{error}</p>
            ) : loading ? (
                <p>Loading...</p>
            ) : (
                users.map((user) => (
                    <div key={user.id}>
                        <h2>{user.name}</h2>
                        <p>{user.email}</p>
                    </div>
                ))
            )}
        </div>
    );
}

export default App;
```

---

# 5. Understand the Three States

This is more important than the code.

### When the app starts:

```text id="a3m7q9"
loading = true
error = ""
users = []
```

UI:

```text
Loading...
```

---

### If API succeeds:

```text id="v8k2p4"
loading = false
error = ""
users = [users...]
```

UI:

```text
User List

John
john@example.com

Jane
jane@example.com
```

---

### If API fails:

```text id="m5x9r2"
loading = false
error = "Failed to load users"
users = []
```

UI:

```text
User List

Failed to load users
```

---

# 6. This Is a Very Common React Pattern

Whenever you're fetching data, you'll often have:

```text id="q4n8s1"
Loading
Error
Success
```

Think:

```text id="z6p2m7"
API Request
    ↓
┌───────────────┐
│   Loading     │
└───────┬───────┘
        ↓
   Request result
      ↙     ↘
 Success    Error
    ↓         ↓
  Data      Message
```

You'll see this pattern again and again in React applications.

---

# 7. One Small Improvement

There's one thing we should fix in our fetch logic.

`fetch()` does **not automatically reject the Promise for HTTP errors like `404` or `500`**.

So this:

```tsx
fetch(...)
    .then(...)
    .catch(...)
```

doesn't catch every HTTP failure automatically.

We should check:

```tsx
if (!response.ok) {
    throw new Error("Failed to fetch users");
}
```

Then `.catch()` can handle it.

The important idea is:

```text id="g3v9k1"
HTTP error
   ↓
throw Error
   ↓
catch
   ↓
setError()
```

We'll use the improved version when we clean up the app.

---

## Current Build Progress

Our Phase 5 build is:

```text
Fetch API Data App + Search/Filter UI

[x] Create React + TypeScript app
[x] Fetch API data
[x] Store data with useState
[x] useEffect for API request
[x] Loading state
[x] Error state
[ ] Display/organize users
[ ] Search
[ ] Filter
[ ] Apply useMemo where useful
```

### Next → Step 12: **Search**

We'll add a search box and connect it to `useState`, so typing `"john"` will show only matching users.


-------------------------------------------------------------------------------------------------------------------------------------------

##### The key thing to remember is:

**`()` = what goes into the function**

**`{}` = what the function does**

**No `{}` after `=>` = automatic return of the expression**.


-------------------------------------------------------------------------------------------------------------------------------------------

# Step 3 — Add Search

Now we'll add **search functionality** to our User List.

The goal is:

```text
User List

Search: [ john ]

       ↓

Only users matching "john"
```

We'll do this using `useState`.

---

# 1. Create Search State

Add this:

```tsx
const [search, setSearch] = useState("");
```

Now we have:

```text
users
 ↓
API users

loading
 ↓
API loading status

error
 ↓
API error

search
 ↓
What the user typed
```

---

# 2. Add the Search Input

Above the user list:

```tsx
<input
    type="text"
    placeholder="Search users..."
    value={search}
    onChange={(event) => setSearch(event.target.value)}
/>
```

This is the **controlled input** concept we discussed earlier.

The flow is:

```text
User types "john"
       ↓
onChange
       ↓
setSearch("john")
       ↓
search = "john"
```

---

# 3. Filter the Users

Now we have:

```text
users
```

and:

```text
search
```

We can filter the users:

```tsx
const filteredUsers = users.filter((user) =>
    user.name.toLowerCase().includes(search.toLowerCase())
);
```

Let's understand this carefully.

---

# 4. `filter()`

Suppose the API gives:

```text
John
Jane
Bob
William
```

And:

```text
search = "john"
```

Then:

```text
users.filter(...)
```

checks every user.

```text
John      → matches → keep
Jane      → doesn't match → remove
Bob       → doesn't match → remove
William   → doesn't match → remove
```

Result:

```text
John
```

---

# 5. Why `toLowerCase()`?

Suppose the user searches:

```text
john
```

but the user's name is:

```text
John
```

Without converting both to lowercase:

```text
"John" ≠ "john"
```

With:

```tsx
user.name.toLowerCase()
```

we get:

```text
"john"
```

And:

```tsx
search.toLowerCase()
```

also gives:

```text
"john"
```

Now they match.

So our search becomes **case-insensitive**.

---

# 6. Why `includes()`?

This:

```tsx
"john".includes("oh")
```

returns:

```text
true
```

So the user doesn't have to type the entire name.

For example:

```text
Search: "oh"

John → match
```

---

# 7. Display `filteredUsers`

Previously we had:

```tsx
users.map(...)
```

Now we use:

```tsx
filteredUsers.map(...)
```

So:

```text
API users
    ↓
filter()
    ↓
filteredUsers
    ↓
map()
    ↓
UI
```

---

# 8. Full Code

Here's our app with search added:

```tsx
import { useEffect, useState } from "react";

type User = {
    id: number;
    name: string;
    email: string;
};

function App() {
    const [users, setUsers] = useState<User[]>([]);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState("");
    const [search, setSearch] = useState("");

    useEffect(() => {
        fetch("https://jsonplaceholder.typicode.com/users")
            .then((response) => {
                if (!response.ok) {
                    throw new Error("Failed to fetch users");
                }

                return response.json();
            })
            .then((data) => {
                setUsers(data);
                setLoading(false);
            })
            .catch(() => {
                setError("Failed to load users");
                setLoading(false);
            });
    }, []);

    const filteredUsers = users.filter((user) =>
        user.name.toLowerCase().includes(search.toLowerCase())
    );

    return (
        <div>
            <h1>User List</h1>

            <input
                type="text"
                placeholder="Search users..."
                value={search}
                onChange={(event) => setSearch(event.target.value)}
            />

            {error ? (
                <p>{error}</p>
            ) : loading ? (
                <p>Loading...</p>
            ) : (
                filteredUsers.map((user) => (
                    <div key={user.id}>
                        <h2>{user.name}</h2>
                        <p>{user.email}</p>
                    </div>
                ))
            )}
        </div>
    );
}

export default App;
```

---

# 9. Understand the Complete Flow

Now our app is getting more interesting:

```text
             API
              ↓
            users
              ↓
          search state
              ↓
           filter()
              ↓
       filteredUsers
              ↓
           map()
              ↓
             UI
```

When the user types:

```text
"john"
```

the flow is:

```text
User types john
       ↓
onChange
       ↓
setSearch("john")
       ↓
search state changes
       ↓
Component re-renders
       ↓
filter users
       ↓
Show matching users
```

---

# 10. What Happens When Search Is Empty?

Initially:

```text
search = ""
```

Then:

```tsx
"john".includes("")
```

is `true`.

So every user matches.

Therefore:

```text
Search empty
   ↓
All users displayed
```

That's exactly what we want.

---

# 11. Where Is `useMemo`?

We learned `useMemo` earlier.

Right now we have:

```tsx
const filteredUsers = users.filter(...)
```

Every time the component renders, the filtering runs again.

For our tiny list of 10 users, that's completely fine.

We **don't need `useMemo` yet**.

Later, we can demonstrate when `useMemo` actually makes sense.

That's important because we shouldn't use Hooks just because we learned them.

---

## Current Progress

```text
Fetch API Data App + Search/Filter UI

[x] Create React + TypeScript app
[x] Fetch API data
[x] useState
[x] useEffect
[x] Loading state
[x] Error state
[x] Display users
[x] Search
[ ] Filter
[ ] useMemo where appropriate
```

### Next → Step 13: **Filter**

We'll add a filter such as:

```text
All users
Completed
Not completed
```

But because our current API is a **User API**, we'll use a more useful filter for this app rather than forcing a Todo-style completed filter.


-------------------------------------------------------------------------------------------------------------------------------------------

# Step 4 — Add Filter

Now we'll add the **filter** part of our Phase 5 build.

We already have:

```text
API
 ↓
users
 ↓
search
 ↓
filtered users
```

Now we'll add another condition:

```text
Filter by
   ↓
All
New York
London
```

Our API users have an `address.city`, so we can filter users by city.

---

# 1. First Understand the Goal

Our UI will look like:

```text
User List

Search: [ john ]

City: [ All ▼ ]

----------------
John
New York
----------------
```

The user can use:

**Search** → find by name

**Filter** → find by city

And both can work together.

---

# 2. Add Filter State

We need to remember which city the user selected.

Add:

```tsx
const [city, setCity] = useState("All");
```

Now our state is:

```text
users
loading
error
search
city
```

`city` represents the current filter.

Initially:

```text
city = "All"
```

That means:

> Don't filter by city.

---

# 3. Add City to the User Type

Our API user contains an address.

So update the type:

```tsx
type User = {
    id: number;
    name: string;
    email: string;
    address: {
        city: string;
    };
};
```

Now TypeScript understands:

```text
user.address.city
```

---

# 4. Add the Select Box

Below the search input:

```tsx
<select
    value={city}
    onChange={(event) => setCity(event.target.value)}
>
    <option value="All">All</option>
    <option value="Gwenborough">Gwenborough</option>
    <option value="Wisokyburgh">Wisokyburgh</option>
    <option value="McKenziehaven">McKenziehaven</option>
</select>
```

This is another **controlled input**.

The flow is:

```text
User selects city
       ↓
onChange
       ↓
setCity()
       ↓
city state changes
       ↓
React re-renders
```

---

# 5. Apply the Filter

We already have:

```tsx
const filteredUsers = users.filter((user) =>
    user.name.toLowerCase().includes(search.toLowerCase())
);
```

Now we need **two conditions**:

```text
Name matches search
AND
City matches selected city
```

So:

```tsx
const filteredUsers = users.filter((user) => {
    const matchesSearch = user.name
        .toLowerCase()
        .includes(search.toLowerCase());

    const matchesCity =
        city === "All" || user.address.city === city;

    return matchesSearch && matchesCity;
});
```

---

# 6. Understand `matchesSearch`

This:

```tsx
const matchesSearch = user.name
    .toLowerCase()
    .includes(search.toLowerCase());
```

answers:

> Does this user's name match the search?

For example:

```text
search = "john"

John
 ↓
matchesSearch = true
```

---

# 7. Understand `matchesCity`

This is the important part:

```tsx
const matchesCity =
    city === "All" || user.address.city === city;
```

If:

```text
city = "All"
```

then:

```text
city === "All"
 ↓
true
```

So everyone passes the city filter.

If:

```text
city = "Gwenborough"
```

then React checks:

```text
user.address.city === "Gwenborough"
```

Only users from that city pass.

---

# 8. Why `&&`?

At the end:

```tsx
return matchesSearch && matchesCity;
```

`&&` means **both conditions must be true**.

For example:

```text
Search matches → true
City matches   → true

true && true
      ↓
    true
```

But:

```text
Search matches → true
City matches   → false

true && false
      ↓
    false
```

So that user won't be displayed.

---

# 9. Search + Filter Together

Now imagine:

```text
Search = "john"
City = "Gwenborough"
```

The app checks every user:

```text
        User
          ↓
    Name matches?
       ↙    ↘
     Yes     No
      ↓       ↓
   City?    Remove
    ↙ ↘
  Yes  No
   ↓    ↓
 Keep  Remove
```

This gives us:

> Users whose **name matches the search AND whose city matches the selected city**.

---

# 10. Full Code

```tsx
import { useEffect, useState } from "react";

type User = {
    id: number;
    name: string;
    email: string;
    address: {
        city: string;
    };
};

function App() {
    const [users, setUsers] = useState<User[]>([]);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState("");
    const [search, setSearch] = useState("");
    const [city, setCity] = useState("All");

    useEffect(() => {
        fetch("https://jsonplaceholder.typicode.com/users")
            .then((response) => {
                if (!response.ok) {
                    throw new Error("Failed to fetch users");
                }

                return response.json();
            })
            .then((data) => {
                setUsers(data);
                setLoading(false);
            })
            .catch(() => {
                setError("Failed to load users");
                setLoading(false);
            });
    }, []);

    const filteredUsers = users.filter((user) => {
        const matchesSearch = user.name
            .toLowerCase()
            .includes(search.toLowerCase());

        const matchesCity =
            city === "All" || user.address.city === city;

        return matchesSearch && matchesCity;
    });

    return (
        <div>
            <h1>User List</h1>

            <input
                type="text"
                placeholder="Search users..."
                value={search}
                onChange={(event) => setSearch(event.target.value)}
            />

            <select
                value={city}
                onChange={(event) => setCity(event.target.value)}
            >
                <option value="All">All</option>
                <option value="Gwenborough">Gwenborough</option>
                <option value="Wisokyburgh">Wisokyburgh</option>
                <option value="McKenziehaven">McKenziehaven</option>
            </select>

            {error ? (
                <p>{error}</p>
            ) : loading ? (
                <p>Loading...</p>
            ) : (
                filteredUsers.map((user) => (
                    <div key={user.id}>
                        <h2>{user.name}</h2>
                        <p>{user.email}</p>
                        <p>{user.address.city}</p>
                    </div>
                ))
            )}
        </div>
    );
}

export default App;
```

---

# 11. Our Complete Flow Now

```text
                 API
                  ↓
                users
                  ↓
        ┌─────────┴─────────┐
        ↓                   ↓
      Search              Filter
        ↓                   ↓
   search state          city state
        └─────────┬─────────┘
                  ↓
               filter()
                  ↓
           filteredUsers
                  ↓
                map()
                  ↓
                  UI
```

We've now completed the **search/filter functionality** of the build.

### Phase 5 Build Progress

```text
[x] Fetch API data
[x] Store data with useState
[x] useEffect for API request
[x] Loading state
[x] Error state
[x] Display data
[x] Search
[x] Filter
[ ] Review where useMemo fits
[ ] Final cleanup / recap
```

**Next → Step 14: `useMemo` in this project.**

We'll specifically look at our `filteredUsers` calculation and understand **whether we actually need `useMemo` here or not**, instead of blindly adding it.


-------------------------------------------------------------------------------------------------------------------------------------------

# Step 5 — `useMemo` in Our Project

Now let's connect **`useMemo`** to the app we just built.

We currently have:

```tsx
const filteredUsers = users.filter((user) => {
    const matchesSearch = user.name
        .toLowerCase()
        .includes(search.toLowerCase());

    const matchesCity =
        city === "All" || user.address.city === city;

    return matchesSearch && matchesCity;
});
```

This calculates the filtered users every time the component renders.

---

# 1. Do We Actually Need `useMemo`?

For our app:

```text
10 users
```

**No.**

The calculation is very small.

Using `useMemo` here isn't necessary.

This is actually an important lesson:

> **Don't use `useMemo` just because you know `useMemo`.**

For a small list, the normal `filter()` is perfectly fine.

---

# 2. But Let's Understand How `useMemo` Would Work

If we wanted to memoize the calculation:

```tsx
const filteredUsers = useMemo(() => {
    return users.filter((user) => {
        const matchesSearch = user.name
            .toLowerCase()
            .includes(search.toLowerCase());

        const matchesCity =
            city === "All" || user.address.city === city;

        return matchesSearch && matchesCity;
    });
}, [users, search, city]);
```

Now React watches:

```text
users
search
city
```

---

# 3. Why These Three Dependencies?

Because our calculation uses all three.

```text
filteredUsers
      ↑
      │
 ┌────┼─────┐
 │    │     │
users search city
```

If `users` changes:

```text
users changed
   ↓
calculate again
```

If `search` changes:

```text
search changed
   ↓
calculate again
```

If `city` changes:

```text
city changed
   ↓
calculate again
```

If none of them changes:

```text
other state changes
       ↓
component renders
       ↓
users/search/city unchanged
       ↓
reuse previous filteredUsers
```

---

# 4. Important Difference

Without `useMemo`:

```text
Component renders
      ↓
filter()
      ↓
filteredUsers
```

With `useMemo`:

```text
Component renders
      ↓
Did users/search/city change?
      ↓
   ┌───┴───┐
   No      Yes
   ↓        ↓
Reuse    filter()
result      ↓
         new result
```

---

# 5. Why Would This Matter?

Imagine instead of 10 users, we have:

```text
1,000,000 users
```

And the filtering calculation is expensive.

Now we don't necessarily want to repeat it when some unrelated state changes.

For example:

```text
Component has another state
       ↓
that state changes
       ↓
component re-renders
       ↓
users/search/city didn't change
       ↓
useMemo reuses filtered result
```

That can improve performance.

---

# 6. But There Is a Trade-off

`useMemo` itself isn't free.

React has to:

```text
Remember the value
+
Remember dependencies
+
Compare dependencies
```

So for something simple like:

```tsx
const doubled = count * 2;
```

using `useMemo` can actually make the code unnecessarily complicated.

That's why:

```text
Simple calculation
      ↓
Normal variable

Expensive calculation
      ↓
Consider useMemo
```

---

# 7. Our App: Normal vs `useMemo`

### Current version

```tsx
const filteredUsers = users.filter(...);
```

**Good choice for our small app.**

### `useMemo` version

```tsx
const filteredUsers = useMemo(() => {
    return users.filter(...);
}, [users, search, city]);
```

Also valid, but unnecessary for our current data size.

---

# 8. Why Did We Learn `useMemo`, Then?

Because you need to understand **when it is appropriate**.

The goal isn't:

```text
"I know useMemo, therefore I must use it."
```

The goal is:

```text
"I understand useMemo, so I know when I actually need it."
```

That's the more important React skill.

---

# 9. Phase 5 Is Now Complete

Let's check our roadmap:

```text
## 5. React Hooks (Core)

[x] useState
[x] useEffect
[x] useRef
[x] useMemo
[x] useCallback

Build:
[x] fetch API data app
[x] search/filter UI
```

**Phase 5 is complete.**

-------------------------------------------------------------------------------------------------------------------------------------------