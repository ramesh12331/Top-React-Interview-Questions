# ⚛️ 15. What is the `useCallback` Hook in React and When Should It Be Used?

# 📘 React `useCallback` Hook — Full Guide

> A complete, interview‑ready guide with definition, syntax, simple → advanced examples, real-time use cases, purpose, mistakes, tricks, summary, and Q&A.

---

## ⭐ Introduction

The **`useCallback`** Hook in React is used to **memoize functions**, meaning it keeps the same function instance **until its dependencies change**.

This prevents unnecessary re-renders, especially when functions are passed as props to child components.

---

## 📌 Definition

`useCallback` returns a **memoized version** of a callback function.

It ensures that the function is **not recreated** on every render unless its dependencies change.

---

## 🧠 Syntax

```jsx
useCallback(callbackFunction, [dependencies]);
```

### Arguments:

* **callbackFunction** → The function to memoize.
* **dependencies** → Array of values that determine when the function should be recreated.

---

## 🟢 Simple Example — Without `useCallback`

```jsx
function Parent() {
  const [count, setCount] = useState(0);

  const increment = () => {
    setCount(count + 1);
  };

  return <Child onIncrement={increment} />;
}
```

Each render creates a **new function**, causing `Child` to re-render.

---

## 🟢 Simple Example — With `useCallback`

```jsx
function Parent() {
  const [count, setCount] = useState(0);

  const increment = useCallback(() => {
    setCount(c => c + 1);
  }, []);

  return <Child onIncrement={increment} />;
}
```

✔ Function reference remains stable.
✔ Child does NOT re-render unnecessarily.

---

## 🔥 Medium Example — Memoized Child Component

```jsx
const Child = React.memo(({ onIncrement }) => {
  console.log("Child rendered");
  return <button onClick={onIncrement}>Increment</button>;
});
```

Using `useCallback` + `React.memo` avoids unwanted re-renders.

---

## 🧩 Advanced Example — Two Independent Buttons

### ❌ Without `useCallback`

```jsx
const Button = React.memo(({ onClick, text }) => {
  alert(`Child ${text} button rendered`);
  return <button onClick={onClick}>{text}</button>;
});

function WithoutCallbackExample() {
  const [count1, setCount1] = useState(0);
  const [count2, setCount2] = useState(0);

  const handleClick1 = () => setCount1(count1 + 1);
  const handleClick2 = () => setCount2(count2 + 1);

  alert("Parent rendered");

  return (
    <div>
      <h2>Without useCallback:</h2>
      <p>Count 1: {count1}</p>
      <p>Count 2: {count2}</p>
      <Button onClick={handleClick1} text="Button 1" />
      <Button onClick={handleClick2} text="Button 2" />
    </div>
  );
}
```

✔ All components re-render → inefficient.

---

### ✅ With `useCallback`

```jsx
const Button = React.memo(({ onClick, text }) => {
  console.log(`${text} button rendered`);
  return <button onClick={onClick}>{text}</button>;
});

function WithCallbackExample() {
  const [count1, setCount1] = useState(0);
  const [count2, setCount2] = useState(0);

  const handleClick1 = useCallback(() => setCount1(count1 + 1), [count1]);
  const handleClick2 = useCallback(() => setCount2(count2 + 1), [count2]);

  console.log("Parent rendered");

  return (
    <div>
      <h2>With useCallback:</h2>
      <p>Count 1: {count1}</p>
      <p>Count 2: {count2}</p>
      <Button onClick={handleClick1} text="Button 1" />
      <Button onClick={handleClick2} text="Button 2" />
    </div>
  );
}
```

✔ Only changed button + parent re-render.
✔ Great performance improvement.

---

## 🏗 Real-Time Example — Expensive Event Handler

```jsx
function Search({ onSearch }) {
  return <input onChange={(e) => onSearch(e.target.value)} placeholder="Search..." />;
}

function App() {
  const [query, setQuery] = useState('');

  const handleSearch = useCallback(() => {
    console.log("Filtering heavy data...");
  }, []);

  return (
    <div>
      <Search onSearch={handleSearch} />
    </div>
  );
}
```

✔ `handleSearch` won't be recreated → improves performance.

---

## 🎯 When to Use `useCallback`

Use `useCallback` when:

* ✔ Passing callback props to memoized child components (`React.memo`)
* ✔ Functions are expensive to recreate
* ✔ Preventing unnecessary re-renders
* ✔ Managing heavy event handlers

---

## ❗ Mistakes to Avoid

* ❌ Overusing `useCallback` — adds unnecessary complexity
* ❌ Forgetting dependencies → stale values
* ❌ Memoizing everything → worse performance
* ❌ Using it without profiling your app

---

## ⚡ Best Practices

* ✔ Use only when passing functions to children
* ✔ Add all dependencies to the array
* ✔ Combine with `React.memo` for best results
* ✔ Memoize heavy operations using `useMemo`
* ✔ Keep dependencies simple and minimal

---

## 🔧 Tricks

### 🔹 Stable Callback for Event Listeners

```jsx
const onScroll = useCallback(() => console.log("scrolling..."), []);
```

### 🔹 Prevent Function Recreation in Lists

```jsx
const onItemClick = useCallback((id) => {
  console.log("Clicked", id);
}, []);
```

### 🔹 Works Great with `useMemo`

```jsx
const filteredData = useMemo(() => expensiveFilter(data), [data]);
const handleFilter = useCallback(() => console.log(filteredData), [filteredData]);
```

---

## 📝 Summary

* `useCallback` memoizes functions
* Prevents unnecessary child re-renders
* Useful in performance‑critical components
* Requires correct dependency arrays
* Works best with `React.memo` and `useMemo`

---

## 🧾 Difference Between `useMemo` and `React.memo`

| Feature             | `useMemo`                                        | `React.memo`                                     |
| ------------------- | ------------------------------------------------ | ------------------------------------------------ |
| **Type**            | Hook                                             | Higher-Order Component (HOC)                     |
| **Purpose**         | Memoizes a **value** (result of a function)      | Memoizes a **component** to prevent re-rendering |
| **What it returns** | Cached value                                     | Memoized component                               |
| **When it runs**    | Runs when dependencies change                    | Re-renders only when props change                |
| **Use case**        | Avoid expensive calculations on every render     | Prevent child component re-renders               |
| **Example usage**   | `const value = useMemo(() => compute(), [data])` | `export default React.memo(ChildComponent)`      |
| **Helps with**      | Performance of heavy computations                | Avoiding re-renders of child components          |
| **Works best with** | `useCallback` (for stable function refs)         | `useCallback` (to keep props stable)             |

---

# 🎤 Interview Questions & Answers

### 🟢 1. What is `useCallback`?

A hook that memoizes a function and returns the same function reference unless dependencies change.

---

### 🟢 2. When should you use `useCallback`?

* When passing functions to memoized child components
* When preventing unnecessary re-renders
* When function recreation is expensive

---

### 🟡 3. What is the difference between `useCallback` and `useMemo`?

* `useMemo` → memoizes a **value**
* `useCallback` → memoizes a **function**

---

### 🔥 4. What happens if dependencies are missing?

You get **stale closures** — the callback uses outdated values.

---

### 🔥 5. Why shouldn't we overuse `useCallback`?

Memoization itself costs memory and computation. Overuse can **hurt performance** instead of helping.

---

If you want, I can also add:

📑 Table of contents
🎨 Better alignment/styling
📘 Comparison table (useCallback vs useMemo vs memo)
📄 PDF or DOCX export
Just tell me!
