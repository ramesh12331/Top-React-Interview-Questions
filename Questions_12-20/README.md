# 📘 React Hooks — Short README (Definitions, Syntax, Examples, Tricks & Summary)

## ⭐ Introduction

React Hooks allow functional components to use **state**, **lifecycle**, **refs**, and other React features without writing class components.

---

## 🔹 1. What Are Hooks?

Hooks are special React functions that let you "hook into" React features like **state**, **effects**, **refs**, **memoization**, and more.

### ✔ Rules of Hooks

* Call Hooks **only inside function components**.
* Call Hooks **at the top level**.
* **Never** call Hooks conditionally.

---

## 🔹 2. useState — Manage Component State

### **Definition**

Stores and updates state in functional components.

### **Syntax**

```js
const [value, setValue] = useState(initialValue);
```

### **Example**

```jsx
const [count, setCount] = useState(0);
<button onClick={() => setCount(count + 1)}>+</button>
```

---

## 🔹 3. useEffect — Handle Side Effects

### **Definition**

Runs after render. Used for API calls, timers, subscriptions.

### **Syntax**

```js
useEffect(() => {
  // side effect
}, [dependencies]);
```

### **Example**

```jsx
useEffect(() => {
  document.title = `Count: ${count}`;
}, [count]);
```

---

## 🔹 4. useRef — Mutable Reference (No Re-render)

### **Definition**

Holds mutable values or DOM references.

### **Syntax**

```js
const inputRef = useRef(null);
```

### **Example**

```jsx
<input ref={inputRef} />
<button onClick={() => inputRef.current.focus()}>Focus</button>
```

---

## 🔹 5. useMemo — Memoize Expensive Calculations

### **Definition**

Returns cached computed value.

### **Syntax**

```js
const result = useMemo(() => compute(value), [value]);
```

### **Example**

```jsx
const doubled = useMemo(() => count * 2, [count]);
```

---

## 🔹 6. useCallback — Memoize Functions

### **Definition**

Prevents unnecessary function recreation.

### **Syntax**

```js
const handleClick = useCallback(() => doSomething(), []);
```

### **Example**

```jsx
<Child onClick={handleClick} />
```

---

## 🔹 7. React.memo — Prevent Child Re-renders

### **Definition**

Rerenders child only when props change.

### **Syntax**

```js
export default React.memo(ChildComponent);
```

---

## 🔹 8. Fragments — Group Elements Without Extra DOM

### **Syntax**

```jsx
<>
  <p>Hello</p>
  <p>World</p>
</>
```

---

## 🔹 9. useId — Generate Stable IDs

### **Example**

```jsx
const id = useId();
<label htmlFor={id}>Name</label>
<input id={id} />
```

---

# 🎯 Useful Tricks

* Toggle boolean:

```js
setShow(prev => !prev);
```

* Update array:

```js
setItems(prev => [...prev, newItem]);
```

* Update object:

```js
setUser(prev => ({ ...prev, age: 20 }));
```

* Memoize heavy logic:

```js
useMemo(() => expensiveWork(data), [data]);
```

* Prevent child re-renders:

```js
React.memo(Child)
```

---

# 🧾 Summary (Short Conclusion)

React Hooks make functional components powerful by providing state management, side‑effects control, performance optimization, and DOM manipulation without using classes. They help keep code clean, reusable, and easy to maintain.

Hooks you should master:
✔ useState
✔ useEffect
✔ useRef
✔ useMemo
✔ useCallback
✔ React.memo
✔ useId
✔ Fragments

With these, you can confidently build modern, optimized React applications.

---

If you want, I can add:

* More advanced examples
* Real-time projects
* Comparison tables
* Interview questions
* Styling improvements
  Just tell me!

## 📘 useReducer

## 🔗 Relationship Between Corresponding Hooks

(Added Section)
Here’s how React hooks relate to each other:

### ⭐ `useState` ↔ `useReducer`

* Both manage state.
* `useState` → simple state.
* `useReducer` → complex logic.

### ⭐ `useCallback` ↔ `useMemo` ↔ `React.memo`

* `useCallback` memoizes functions.
* `useMemo` memoizes values.
* `React.memo` memoizes components.

### ⭐ `useRef` ↔ `useEffect`

* `useRef` stores persistent data.
* `useEffect` reacts to state/prop changes.

### ⭐ Lifecycle Mapping

* `componentDidMount` → `useEffect(() => {}, [])`
* `componentDidUpdate` → `useEffect(() => {}, [deps])`
* `componentWillUnmount` → cleanup return in `useEffect`.

### ⭐ Hook Categories

| Category     | Hooks                                  | Purpose               |
| ------------ | -------------------------------------- | --------------------- |
| State        | `useState`, `useReducer`               | Store reactive values |
| Performance  | `useMemo`, `useCallback`, `React.memo` | Avoid rerenders       |
| References   | `useRef`                               | Persist values        |
| Side Effects | `useEffect`                            | Run code after render |

### ⭐ Summary

Hooks complement each other to manage state, performance, lifecycle, and DOM interactions.
Hook

### ⭐ Definition

`useReducer` is a React Hook used for managing **complex state logic**. It is an alternative to `useState` and is ideal when:

* State has **multiple sub‑values**
* Updates depend on previous state
* Logic involves **multiple actions**
* You need predictable state transitions like Redux

### 🧠 Syntax

```
const [state, dispatch] = useReducer(reducer, initialState);

function reducer(state, action) {
  switch (action.type) {
    case "ACTION":
      return { ...state, ... };
    default:
      return state;
  }
}
```

---

### 🟢 Simple Example

```
const initialState = { count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      return state;
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <>
      <h1>Count: {state.count}</h1>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
    </>
  );
}
```

---

### 🔥 Real-Time Example — Form Handling

```
const initialState = { name: '', email: '' };

function reducer(state, action) {
  return { ...state, [action.field]: action.value };
}

function Form() {
  const [state, dispatch] = useReducer(reducer, initialState);

  function handleChange(e) {
    dispatch({ field: e.target.name, value: e.target.value });
  }

  return (
    <>
      <input name="name" onChange={handleChange} value={state.name} />
      <input name="email" onChange={handleChange} value={state.email} />
    </>
  );
}
```

---

### 🎯 When to Use useReducer

* Complex state objects
* Multiple state transitions
* Logic that depends on previous state
* Scenarios similar to Redux patterns

---

### 💡 Tricks

* Use constants for action types
* Keep reducer **pure**
* Never mutate state inside reducer
* Combine with Context for global state

---

### 📝 Short Summary

`useReducer` provides predictable and scalable state management for complex logic by using dispatch-based updates through a reducer function.

## 🪄 Additional Useful Tricks for Hooks

### 🔹 Global Hook Tricks

* **Use functional updates** (`setCount(c => c + 1)`) to avoid stale state.
* **Memoize expensive logic** using `useMemo` and memoize functions with `useCallback`.
* **Combine memoization + React.memo** to stop unwanted re-renders.
* **Use refs for mutable variables** that should NOT cause re-renders.
* **Break large components** into smaller memoized ones for performance.

### 🔹 `useState` Tricks

* Toggle boolean easily:

  ```js
  setIsOpen(prev => !prev);
  ```
* Update nested objects safely using the spread operator.
* Reset state to initial value:

  ```js
  setForm(initialForm);
  ```

### 🔹 `useEffect` Tricks

* Run effect only once using `[]`.
* Use cleanup functions to avoid memory leaks.
* Use multiple `useEffect` hooks instead of one big one.
* Use custom hooks to reuse side-effect logic.

### 🔹 `useRef` Tricks

* Store previous state values:

  ```js
  prev.current = state;
  ```
* Hold timers/intervals in refs.
* Smooth scroll:

  ```js
  ref.current.scrollIntoView({ behavior: "smooth" });
  ```

### 🔹 `useCallback` Tricks

* Use with `React.memo` to stop child re-renders.
* Stable event handlers for expensive operations.
* Pass stable functions to child components.

### 🔹 `useMemo` Tricks

* Memoize arrays/objects to maintain stable references.
* Avoid expensive calculations on every render.
* Improve performance in lists, tables, and dashboards.

### 🔹 `useReducer` Tricks

* Centralize complex logic in reducer functions.
* Use action objects like Redux for clarity.
* Perfect for forms, nested objects, and multi-step flows.
* Combine with Context API for global state.

---
