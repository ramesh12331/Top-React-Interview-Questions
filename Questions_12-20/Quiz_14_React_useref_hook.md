# ⚛️ 14. What is the `useRef` Hook in React and When Should It Be Used?

# 📘 React `useRef` Hook — Full Guide

> A complete, interview-ready guide with definition, syntax, simple → advanced examples, real-time use cases, purpose, mistakes, tricks, summary, and Q&A.

---

## ⭐ Introduction

The **`useRef`** hook in React creates a **persistent mutable reference** that:

* ❌ does **NOT** trigger re-renders when updated
* ✔ remains the same between renders
* ✔ is commonly used for accessing DOM elements or storing values across renders

It returns an object:

```
{ current: ... }
```

that survives re-renders.

---

## 📌 Definition

**`useRef`** returns a mutable reference object whose `.current` property persists for the component’s entire lifecycle.

Use it when you need to store:

* DOM references (focus, scroll, measurements)
* Previous state values
* Timers/intervals
* WebSocket connections
* Mutable values that shouldn’t trigger re-renders

---

## 🧠 Syntax

```jsx
const ref = useRef(initialValue);

// Accessing value
ref.current;
```

---

## 🟢 Simple Example — Focusing an Input

```jsx
import React, { useRef, useEffect } from 'react';

function FocusInput() {
  const inputRef = useRef(null);

  useEffect(() => {
    inputRef.current.focus();
  }, []);

  return <input ref={inputRef} type="text" />;
}
```

✔ The input auto-focuses on mount using the DOM reference.

---

## 🔥 Medium Example — Tracking Render Count (Does NOT Re-render)

```jsx
function App() {
  const [inputValue, setInputValue] = useState("");
  const count = useRef(0);

  useEffect(() => {
    count.current = count.current + 1;
  });

  return (
    <>
      <p>Type in the input field:</p>
      <input
        value={inputValue}
        onChange={(e) => setInputValue(e.target.value)}
      />
      <h1>Render Count: {count.current}</h1>
    </>
  );
}
```

✔ `count.current` updates without causing re-renders.

---

## 🧩 Advanced Example — Accessing DOM Element on Button Click

```jsx
function App() {
  const inputRef = useRef();

  const focusInput = () => {
    inputRef.current.focus();
  };

  return (
    <>
      <input type="text" ref={inputRef} />
      <button onClick={focusInput}>Focus Input</button>
    </>
  );
}
```

✔ The button focuses the input using `ref.current`.

---

## 🏗 Real-Time Example — Storing Previous State

```jsx
function App() {
  const [inputValue, setInputValue] = useState("");
  const previousInputValue = useRef("");

  useEffect(() => {
    previousInputValue.current = inputValue;
  }, [inputValue]);

  return (
    <>
      <input
        value={inputValue}
        onChange={(e) => setInputValue(e.target.value)}
      />
      <h2>Current Value: {inputValue}</h2>
      <h2>Previous Value: {previousInputValue.current}</h2>
    </>
  );
}
```

✔ Tracks previous value without triggering re-renders.

---

## 🎯 Main Purpose of `useRef`

### ✔ Accessing DOM nodes

* Input focus
* Scroll position
* Measuring element size

### ✔ Storing mutable values that do NOT cause re-renders

* Counters
* Previous values
* Temporary variables

### ✔ Holding references to external resources

* Timers
* Intervals
* WebSocket connections

---

## ❗ Mistakes to Avoid

* ❌ Using `useRef` for state that **should** trigger UI updates → use `useState`
* ❌ Assuming updating `.current` will re-render the UI → it won’t
* ❌ Overusing refs instead of proper React state management
* ❌ Putting large data structures inside refs without reason

---

## ⚡ Best Practices

* ✔ Use `useRef` for DOM access or storing non-UI values
* ✔ Keep referenced data outside rendering logic
* ✔ Combine with `useEffect` for DOM interactions
* ✔ Do not replace real state with refs

---

## 🔧 Tricks

### 🔹 Store Expensive Calculations Without Re-rendering

```jsx
const expensiveValue = useRef(computeHeavyTask());
```

### 🔹 Keep Timeout or Interval IDs

```jsx
const timerRef = useRef();

useEffect(() => {
  timerRef.current = setInterval(() => console.log("tick"), 1000);
  return () => clearInterval(timerRef.current);
}, []);
```

### 🔹 Smooth Scroll to an Element

```jsx
sectionRef.current.scrollIntoView({ behavior: "smooth" });
```

---

## 📝 Summary

* `useRef` stores **persistent values** across renders
* Updating refs does **not** cause re-renders
* Great for DOM manipulation and storing mutable values
* Essential for timers, focus, scroll, and external resources

---

# 🎤 Interview Questions & Answers

### 🟢 1. What is `useRef`?

A hook that returns a persistent reference object that does not trigger re-renders when updated.

---

### 🟢 2. When should you use `useRef`?

* Accessing DOM elements
* Storing previous state
* Holding mutable values across renders
* Managing timers or external connections

---

### 🟡 3. Does updating a ref trigger a re-render?

No — refs update silently without causing UI changes.

---

### 🔥 4. How do refs help avoid infinite loops?

They store changing values **without** causing re-renders like state does.

---

### 🔥 5. Can you use `useRef` to store previous props or state?

Yes — by updating `.current` inside `useEffect`.

---

If you'd like, I can:

📑 Add a table of contents
🎨 Improve spacing + alignment
📘 Add DOM diagrams
📄 Export as PDF or DOCX
Just tell me!
