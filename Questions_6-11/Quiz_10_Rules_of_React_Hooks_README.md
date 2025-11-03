# 10. What are the rules of React hooks?
# 🧠 Rules of React Hooks

Hooks are special functions in React that let functional components use **state** and **lifecycle** features like `useState`, `useEffect`, and `useContext`.

However — since React tracks hook calls **by their order**, there are strict rules to ensure predictable behavior.

If you break these rules, React will lose track of which state belongs to which hook, causing bugs or errors.

---

## ⚙️ The 3 Golden Rules of React Hooks

### 1️⃣ Call Hooks at the Top Level

**✅ Meaning:**  
Hooks should always be called at the **top level** of a component or custom hook — never inside loops, conditions, or nested functions.

**💡 Why:**  
React relies on the order of hook calls to associate state and effects with components.  
If the order changes between renders, React can’t match the right state to the right hook.

**❌ Incorrect Example**
```jsx
function MyComponent({ isLoggedIn }) {
  if (isLoggedIn) {
    // ❌ useState called conditionally
    const [user, setUser] = useState("Guest");
  }
  return <div>Welcome!</div>;
}
```

**✅ Correct Example**
```jsx
function MyComponent({ isLoggedIn }) {
  const [user, setUser] = useState("Guest"); // ✅ always called

  if (isLoggedIn) {
    console.log("User logged in!");
  }

  return <div>Hello {user}</div>;
}
```

---

### 2️⃣ Call Hooks Only Inside React Functions

**✅ Hooks should only be called from:**
- React functional components
- Custom hooks (functions that start with `use`)

**🚫 You cannot call hooks from:**
- Regular JavaScript functions
- Class components
- Event handlers, loops, or nested callbacks

**❌ Incorrect Example**
```jsx
function fetchData() {
  const [data, setData] = useState([]); // Error!
}
```

**✅ Correct Example**
```jsx
function MyComponent() {
  const [data, setData] = useState([]);
  return <p>Data length: {data.length}</p>;
}
```

Or inside a custom hook:
```jsx
function useFetchData() {
  const [data, setData] = useState([]);
  return { data, setData };
}
```

---

### 3️⃣ Custom Hooks Must Start With “use”

**✅ Example:**
```jsx
function useUserData() {
  const [user, setUser] = useState(null);
  return [user, setUser];
}
```

**🚫 Incorrect:**
```jsx
function getUserData() { // ❌ doesn’t start with “use”
  const [user, setUser] = useState(null);
}
```

This naming convention helps React and ESLint detect hooks correctly.

---

## 🧩 Why These Rules Matter (Internally)

React uses a **linked list** internally to track hooks in the order they appear.

Example:
```
Hook 1 → Hook 2 → Hook 3 → Hook 4 ...
```

If the order changes, React’s internal mapping breaks.

**❌ Wrong Example**
```jsx
function Example({ show }) {
  if (show) {
    const [count, setCount] = useState(0); // ❌ wrong
  }
  const [name, setName] = useState("Ramesh");
  return <p>{name}</p>;
}
```

**✅ Correct Example**
```jsx
function Example({ show }) {
  const [count, setCount] = useState(0);
  const [name, setName] = useState("Ramesh");

  if (!show) return <p>Hidden</p>;

  return <p>{name} — Count: {count}</p>;
}
```

---

## 🧩 Bonus: Enforcing Hook Rules Automatically

Install the ESLint plugin:
```bash
npm install eslint-plugin-react-hooks --save-dev
```

**In `.eslintrc`:**
```json
{
  "plugins": ["react-hooks"],
  "rules": {
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "warn"
  }
}
```

---

## 💡 Interview Insights

**Q1:** Why must hooks be called at the top level?  
➡ Because React relies on the order of hook calls to associate state with components.

**Q2:** Can we call hooks inside callbacks or event handlers?  
➡ No, hooks must run during rendering, not inside nested functions.

**Q3:** Why must custom hooks start with “use”?  
➡ So React and ESLint can detect them and ensure rules are followed.

---

## 🧾 Summary Table

| Rule | Description | Example |
|------|--------------|----------|
| 1️⃣ Top-level only | Never call inside loops, conditions, or nested functions | ✅ `const [x, setX] = useState()` at top |
| 2️⃣ Only inside React | Must be used in components or custom hooks | ❌ Not inside regular JS functions |
| 3️⃣ Must start with “use” | React recognizes custom hooks by naming | ✅ `useFetchData()` |

---

## 🧭 Summary (Easy to Remember)

Hooks are like seats on a roller coaster 🎢 — React assigns them in order.  
If you skip a seat (call conditionally), React gives the wrong person the wrong seat!

✅ Always call them in the same order.  
✅ Only inside React functions.  
✅ Always start custom hooks with `use`.
