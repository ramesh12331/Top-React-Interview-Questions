# 📘 React `useEffect` Hook — Full Guide

> A complete, interview‑ready guide with definition, syntax, simple → advanced examples, real‑time use cases, cleanup, mistakes, best practices, summary, and interview Q&A.

---

## ⭐ Introduction

The **`useEffect`** Hook lets you run **side effects** in React functional components.

Side effects include:

* Fetching data
* Working with the DOM
* Timers and intervals
* Subscriptions
* Event listeners

It replaces lifecycle methods like:

* `componentDidMount`
* `componentDidUpdate`
* `componentWillUnmount`

---

## 📌 Definition

`useEffect` runs a given function **after the component renders**.

```jsx
useEffect(() => {
  // side effect code
}, [dependencies]);
```

The optional **dependency array** controls when the effect runs.

---

## 🧠 Syntax

```jsx
useEffect(callbackFunction, dependencyArray);
```

* **callbackFunction** → runs after render
* **dependencyArray** → controls when the effect re‑runs

---

## 🟢 Example 1 — Runs on *Every Render*

```jsx
useEffect(() => {
  // Runs every time component renders
});
```

---

## 🟢 Example 2 — Runs Only on First Render

```jsx
useEffect(() => {
  // Runs only on mount
}, []);
```

---

## 🟢 Example 3 — Runs When Dependency Changes

```jsx
useEffect(() => {
  // Runs when "count" changes
}, [count]);
```

---

# 🔥 Timer Example — Without Dependency Array (Buggy)

```jsx
function Timer() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    setTimeout(() => {
      setCount(count + 1);
    }, 1000);
  });

  return <h1>Rendered {count} times!</h1>;
}
```

❌ Effect runs on *every render* → infinite increments.

---

# ✅ Timer Example — Correct (Runs Once)

```jsx
useEffect(() => {
  setTimeout(() => {
    setCount(count + 1);
  }, 1000);
}, []);
```

✔ Runs only after first render.

---

# 🧩 Dependency Example — Recalculating Values

```jsx
function Counter() {
  const [count, setCount] = useState(0);
  const [calculation, setCalculation] = useState(0);

  useEffect(() => {
    setCalculation(count * 2);
  }, [count]);

  return (
    <>
      <p>Count: {count}</p>
      <button onClick={() => setCount(c => c + 1)}>+</button>
      <p>Calculation: {calculation}</p>
    </>
  );
}
```

✔ Runs only when `count` changes.

---

# 🧼 Effect Cleanup — Prevent Memory Leaks

Cleanup is used for:

* Timers
* Event listeners
* Subscriptions
* Intervals

### Example — Clear Timer on Unmount

```jsx
function Timer() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const timer = setTimeout(() => {
      setCount(c => c + 1);
    }, 1000);

    return () => clearTimeout(timer);
  }, []);

  return <h1>Rendered {count} times!</h1>;
}
```

✔ Prevents memory leaks.

---

# 🏗 Real‑World Examples

## 1️⃣ Fetching API Data

```jsx
useEffect(() => {
  async function fetchData() {
    const res = await fetch('/api/users');
    const data = await res.json();
    setUsers(data);
  }
  fetchData();
}, []);
```

## 2️⃣ Listening to Window Resize

```jsx
useEffect(() => {
  const handleResize = () => console.log(window.innerWidth);
  window.addEventListener('resize', handleResize);

  return () => window.removeEventListener('resize', handleResize);
}, []);
```

## 3️⃣ Watching Authentication State

```jsx
useEffect(() => {
  const unsub = auth.onAuthStateChanged(user => setUser(user));
  return () => unsub();
}, []);
```

---

# 🎯 When to Use `useEffect`

* ✔ Fetching data on mount
* ✔ Running logic when props/state change
* ✔ Working with timers
* ✔ Attaching listeners
* ✔ DOM manipulation

---

# ❗ Mistakes to Avoid

* ❌ Missing dependencies → stale values
* ❌ Adding every variable blindly → infinite loops
* ❌ Updating state inside an effect without conditions
* ❌ Forgetting cleanup → memory leaks

---

# ⚡ Best Practices

* ✔ Use empty array `[]` for one‑time effects
* ✔ Always include required dependencies
* ✔ Cleanup subscriptions/timers
* ✔ Avoid heavy logic inside effects
* ✔ Move reusable logic into custom hooks

---

# 🔧 Tricks

### 🔹 Use Multiple Effects Instead of One Large Effect

```jsx
useEffect(() => doA(), []);
useEffect(() => doB(), [value]);
```

### 🔹 Use Custom Hook for Reusable Logic

```jsx
function useDocumentTitle(title) {
  useEffect(() => {
    document.title = title;
  }, [title]);
}
```

### 🔹 Trigger Effect When Multiple States Change

```jsx
useEffect(() => {
  console.log("values changed!");
}, [state1, state2]);
```

---

## 🏗 React Class Lifecycle Equivalents

Below are the definitions of the classic React lifecycle methods and their purposes:

### 📘 **Definitions**

#### 🔹 **componentDidMount**

Runs **once** after the component is inserted into the DOM.
Used for:

* Fetching data
* Starting timers
* Subscribing to events
* DOM manipulation

#### 🔹 **componentDidUpdate**

Runs **after every update** (state or props change).
Used for:

* Responding to state/prop changes
* Updating the DOM based on new data
* Calling APIs when values change
* Comparing previous & current values

#### 🔹 **componentWillUnmount**

Runs **right before** a component is removed from the DOM.
Used for:

* Cleanup operations
* Clearing timers
* Removing event listeners
* Unsubscribing from services

---

## 🏗 React Class Lifecycle Equivalents

`useEffect` can replicate classic React lifecycle methods:

---

### 🔹 `componentDidMount` (Run Once on Mount)

Equivalent:

```jsx
useEffect(() => {
  console.log("Mounted!");
}, []);
```

Class Component Example:

```jsx
class Example extends React.Component {
  componentDidMount() {
    console.log("Mounted!");
  }

  render() {
    return <h1>Hello</h1>;
  }
}
```

---

### 🔹 `componentDidUpdate` (Run When State/Props Change)

Equivalent:

```jsx
useEffect(() => {
  console.log("Updated! Count changed.");
}, [count]);
```

Class Component Example:

```jsx
class Counter extends React.Component {
  state = { count: 0 };

  componentDidUpdate(prevProps, prevState) {
    if (prevState.count !== this.state.count) {
      console.log("Count updated");
    }
  }

  render() {
    return (
      <button onClick={() => this.setState({ count: this.state.count + 1 })}>
        {this.state.count}
      </button>
    );
  }
}
```

---

### 🔹 `componentWillUnmount` (Cleanup on Unmount)

Equivalent:

```jsx
useEffect(() => {
  const timer = setInterval(() => console.log("Running..."), 1000);

  return () => {
    clearInterval(timer);
    console.log("Unmounted! Cleanup done.");
  };
}, []);
```

Class Component Example:

```jsx
class Timer extends React.Component {
  componentDidMount() {
    this.interval = setInterval(() => console.log("Running..."), 1000);
  }

  componentWillUnmount() {
    clearInterval(this.interval);
    console.log("Cleanup done.");
  }

  render() {
    return <h1>Timer Running</h1>;
  }
}
```

---

# 📝 Summary

* `useEffect` runs side effects after rendering
* Dependency array controls when it re‑runs
* Cleanup prevents memory leaks
* Useful for API calls, timers, DOM work, listeners, subscriptions
* Critical for real‑world React applications

---

# 🎤 Interview Questions & Answers

### 🟢 1. What is `useEffect`?

A hook that lets you perform side effects in functional components.

---

### 🟢 2. When does `useEffect` run?

* After every render (no dependency array)
* Only on mount (empty array)
* When dependencies change (dependency array)

---

### 🟡 3. What is cleanup in `useEffect`?

A return function that clears timers, listeners, or subscriptions.

---

### 🔥 4. Why does `useEffect` run twice in React Strict Mode?

React deliberately double‑invokes effects in development to detect issues.

---

### 🔥 5. What is a common cause of infinite loops in `useEffect`?

Updating state inside an effect **without proper dependencies**.

---

If you want, I can add:

* 📑 Table of contents
* 🎨 Better alignment/styling
* 📘 Advanced patterns (debouncing, throttling, observers)
* 📄 Export to DOCX or PDF
  Just tell me!
