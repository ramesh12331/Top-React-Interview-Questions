## 19. What does **re-rendering** mean in React?

# 📘 React Re-rendering — Full Guide

> A complete, interview‑ready guide with definition, syntax, examples, advanced real‑time use cases, purpose, tricks, summary, and Q&A.

---

## ⭐ Introduction

React **re-rendering** refers to the process where React updates a component’s UI when its **state**, **props**, or **context** change. React regenerates the virtual DOM, compares it with the previous version, and updates only what changed in the real DOM.

This helps React maintain high performance while keeping UI consistent with logic.

---

## 📌 Definition

**Re-rendering** in React is the process where a component’s output is recalculated and updated whenever its **state, props, or context changes**.

React:

1. Recreates the **virtual DOM tree** for that component.
2. Runs **reconciliation** (diffing algorithm).
3. Updates **only the changed parts** of the real DOM.

---

## 🧠 Syntax (Triggering Re-renders)

Re-renders occur when:

```jsx
setState(newValue);
```

or

```jsx
<Component newProps={value} />
```

or

```jsx
<MyContext.Provider value={...}>
```

---

## 🟢 Simple Example — State Change Causes Re-render

```jsx
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  console.log("Component rendered");

  return (
    <div>
      <h1>Count: {count}</h1>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

Each click updates state → triggers re-render.

---

## 🔥 Medium Example — Props Trigger Re-render

```jsx
function Child({ value }) {
  console.log("Child rendered");
  return <h2>Value: {value}</h2>;
}

function Parent() {
  const [num, setNum] = useState(1);

  return (
    <>
      <Child value={num} />
      <button onClick={() => setNum(num + 1)}>Update Value</button>
    </>
  );
}
```

Changing `num` causes both Parent and Child to re-render.

---

## 🧩 Advanced Example — Context Triggering Re-renders

```jsx
const ThemeContext = React.createContext();

function ThemeProvider() {
  const [theme, setTheme] = useState("light");

  return (
    <ThemeContext.Provider value={theme}>
      <Toolbar />
      <button onClick={() => setTheme(theme === "light" ? "dark" : "light")}>
        Toggle Theme
      </button>
    </ThemeContext.Provider>
  );
}

function Toolbar() {
  const theme = React.useContext(ThemeContext);
  console.log("Toolbar rendered");

  return <div>Theme: {theme}</div>;
}
```

Updating context → all consumers re-render.

---

## 🏗 Real-Time Example — Search Input with Live Filtering

```jsx
function SearchList({ items }) {
  const [query, setQuery] = useState('');

  const filtered = items.filter(item => item.includes(query));

  return (
    <div>
      <input
        placeholder="Search..."
        value={query}
        onChange={(e) => setQuery(e.target.value)}
      />

      {filtered.map((item, i) => (
        <p key={i}>{item}</p>
      ))}
    </div>
  );
}
```

Every keystroke → state changes → re-render → filtered list updates.

---

## 🎯 When Does Re-rendering Happen?

React re-renders when:

### ✔ State changes

`setState` or `useState` updates trigger re-renders.

### ✔ Props change

Any new value passed from parent triggers re-render.

### ✔ Context value changes

All consumers re-render.

### ✔ Force updates

Rarely used, but possible in class components.

---

## ❗ Mistakes to Avoid

* ❌ Causing unnecessary re-renders by creating inline functions/objects.
* ❌ Deeply nested state that triggers heavy UI updates.
* ❌ Forgetting that *every* state change causes a re-render.
* ❌ Passing new object references without memoization.

---

## ⚡ Best Practices

* ✔ Keep state **minimal and flat**.
* ✔ Use `React.memo` for pure components.
* ✔ Use `useCallback` for stable functions.
* ✔ Use `useMemo` to memoize expensive calculations.
* ✔ Lift state only when necessary.
* ✔ Avoid frequent context updates.

---

## 🔧 Tricks & Optimization Patterns

### 🔹 Memoizing Components

```jsx
const Child = React.memo(function Child({ value }) {
  return <p>{value}</p>;
});
```

### 🔹 Memoizing Functions

```jsx
const handleClick = useCallback(() => {
  console.log("Clicked");
}, []);
```

### 🔹 Preventing Re-renders in Lists

```jsx
const itemList = useMemo(() => items.map(i => i * 2), [items]);
```

### 🔹 Splitting Large Components

Break complex UI into smaller memoized units.

---

## 📝 Summary

* React re-renders when **state, props, or context** change.
* Re-rendering updates the **virtual DOM** and applies minimal changes to the real DOM.
* Optimize re-renders using **memoization** and **clean component structure**.
* Understanding re-renders is essential for performance tuning.

---

# 🎤 Interview Questions & Answers

### 🟢 **1. What is re-rendering in React?**

Re-rendering is the process where React updates a component’s output when its **state**, **props**, or **context** changes.

---

### 🟢 **2. What triggers a re-render?**

* State update
* Props change
* Context value update
* Force update

---

### 🟢 **3. How does React update the DOM efficiently?**

React uses the **virtual DOM** and **diffing algorithm** to update only the changed parts.

---

### 🟡 **4. How to prevent unnecessary re-renders?**

* Use `React.memo`
* Use `useCallback`, `useMemo`
* Avoid creating new objects/functions inside JSX

---

### 🟡 **5. Why does updating state cause a re-render?**

State is part of the component’s data model. When it changes, React must update the UI to stay consistent.

---

### 🔥 **6. How would you debug performance issues related to re-renders?**

* Use React DevTools "Highlight Updates"
* Check props passed to child components
* Memoize expensive operations

---

### 🔥 **7. Does updating parent state always re-render children?**

Yes, unless children are wrapped in **React.memo**.

---

If you'd like, I can add:

✅ Visual diagrams

✅ Flowcharts (render lifecycle)

✅ Performance optimization section

Just tell me!
