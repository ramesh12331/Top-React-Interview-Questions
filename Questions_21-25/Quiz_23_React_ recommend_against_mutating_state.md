## 23. Why does React recommend **against mutating state**?

# 📘 Why React Recommends *Not* Mutating State — Full Guide

## ⭐ Introduction

React recommends **never mutating state directly** because its rendering system relies on **detecting reference changes**.
If you mutate state directly, React may fail to update the UI, causing **stale renders, unpredictable behavior, and performance bugs**.

---

## 🔍 Why Does React Warn Against Mutating State?

React detects state changes using **shallow comparison**:

* It checks if the **reference** to the object/array has changed
* If the reference is the same → React assumes nothing changed

When you mutate state directly:

* The reference **does not change** ⚠️
* React **skips re-rendering**
* UI does **not reflect the updated values**

---

## 🧠 Correct vs Incorrect Syntax

### ❌ Incorrect (Mutation)

```jsx
const [items, setItems] = useState([1, 2, 3]);

function addItem() {
  items.push(4);     // ❌ Mutating array!
  setItems(items);   // ❌ React may not rerender
}
```

### ✅ Correct (Immutable Update)

```jsx
function addItem() {
  setItems(prev => [...prev, 4]);
}
```

✔ Creates a **new array reference**
✔ React re-renders correctly

---

## 🔥 Simple Example — Updating Objects Correctly

### ❌ Wrong

```jsx
user.name = "Ramesh";     // mutation
setUser(user);             // same reference → no rerender
```

### ✅ Right

```jsx
setUser(prev => ({ ...prev, name: "Ramesh" }));
```

✔ Creates new object
✔ React detects the change

---

## 🎯 Why Mutating State Causes Issues

* React compares **old vs new references**
* Mutation keeps the **same reference**, so React thinks nothing changed
* Leads to UI not updating
* Breaks `React.memo`, `useEffect`, and optimization logic
* Causes inconsistent and unpredictable behavior

---

## 🧩 When Is This a Big Problem?

* Working with **arrays** (push, pop, splice, sort)
* Working with **objects** (adding/removing properties)
* Updating **nested** or deeply structured state
* Using **React.memo** or memoized components
* Using **useEffect** dependencies that rely on reference changes

---

## 🔥 Medium Example — Nested State (Common Bug)

### ❌ Mutation

```jsx
const [state, setState] = useState({ user: { name: "A" } });
state.user.name = "B";   // ❌ mutation
setState(state);          // ❌ no rerender
```

### ✅ Immutable Update

```jsx
setState(prev => ({
  ...prev,
  user: {
    ...prev.user,
    name: "B"
  }
}));
```

✔ New reference for both `state` and `user`

---

## 🏗 Advanced Example — Immutable Array Updates

```jsx
// Add item
setList(prev => [...prev, newItem]);

// Remove item
setList(prev => prev.filter(item => item.id !== id));

// Update item
setList(prev => prev.map(item => item.id === id ? { ...item, value: 100 } : item));
```

✔ Always returns **new arrays**

---

## ⚡ Real-world Example — Mutation Breaking React.memo

```jsx
const MemoizedList = React.memo(({ items }) => {
  console.log("Rendered");
  return items.map(i => <p>{i}</p>);
});

const [items, setItems] = useState([1,2,3]);

function add() {
  items.push(4);    // ❌ mutation → reference unchanged
  setItems(items);
}
```

`React.memo` skips rendering because the array reference is the same → UI never updates ❌

---

## ❗ Mistakes to Avoid

❌ Using array mutators: push, pop, shift, unshift, splice, sort, reverse
❌ Mutating object properties directly
❌ Updating nested objects without cloning
❌ Mixing mutable and immutable patterns
❌ Assuming React will detect deep changes

---

## ⚡ Best Practices

✔ Always create **new objects/arrays** when updating state
✔ Use: `map`, `filter`, `reduce`, spread operator `...`
✔ Store `initialState` separately
✔ Use `useReducer` for complex state
✔ Use libraries like **Immer.js** for deep immutable updates

---

## 🔧 Tricks

### 🔹 Trick 1: Freeze state during development

```jsx
Object.freeze(state);
```

Helps detect accidental mutations.

### 🔹 Trick 2: Use Immer for nested updates

```jsx
import produce from "immer";

setState(prev => produce(prev, draft => {
  draft.user.name = "Ramesh";
}));
```

### 🔹 Trick 3: Return new references in reducers

```jsx
return { ...state, value: newValue };
```

---

## 📝 Summary

* React depends on **reference changes**, not value changes
* Mutating state breaks rendering logic
* Always update state **immutably**
* Use functional updates, spread syntax, and pure functions
* Prevents UI bugs, stale renders, and broken optimizations

---

## 🎤 Interview Questions & Answers

### 🟢 Basic Level

**❓ Why shouldn't we mutate state in React?**
💡 Because React won’t detect the change if the reference stays the same.

**❓ How does React detect state changes?**
💡 By shallow comparison of references (not deep comparison).

---

### 🟡 Intermediate Level

**❓ What happens if you mutate an array or object stored in state?**
💡 UI may not rerender, leading to stale or incorrect display.

**❓ How do you update arrays immutably?**
💡 By using `[...prev]`, `map`, `filter`, etc.

---

### 🔥 Advanced Level

**❓ Why does mutation break React.memo?**
💡 memoized components re-render only when props change by reference.

**❓ When should you use useReducer instead of useState?**
💡 When managing nested or complex state that must remain immutable.

---

---

## 🏗 Component-Level Example — Mutation vs Immutability

This example clearly shows how mutation breaks React updates and how immutability fixes it.

### ❌ Incorrect Component (Mutating State)

```jsx
function TodoList() {
  const [todos, setTodos] = useState(["Buy milk", "Go to gym"]);

  function addTodo() {
    todos.push("New Task"); // ❌ Direct mutation
    setTodos(todos);       // ❌ Same reference → React may not rerender
  }

  return (
    <div>
      {todos.map((t, i) => <p key={i}>{t}</p>)}
      <button onClick={addTodo}>Add Todo</button>
    </div>
  );
}
```

⚠️ Problem: UI might not update because `todos` reference remains the same.

---

### ✅ Correct Component (Immutable Update)

```jsx
function TodoList() {
  const [todos, setTodos] = useState(["Buy milk", "Go to gym"]);

  function addTodo() {
    setTodos(prev => [...prev, "New Task"]); // ✔ New array
  }

  return (
    <div>
      {todos.map((t, i) => <p key={i}>{t}</p>)}
      <button onClick={addTodo}>Add Todo</button>
    </div>
  );
}
```

✔ UI updates correctly because a **new array instance** is created.

---

## 🔍 Mutable vs Immutable — Simple Explanation

### 🔄 Mutable (Can be changed directly)

Mutable data structures **change their original value**.

#### Example (Mutable Object)

```jsx
const user = { name: "Ramesh" };
user.name = "Arjun"; // changed same object
console.log(user.name); // "Arjun"
```

Same object → mutated.

#### Example (Mutable Array)

```jsx
const arr = [1, 2, 3];
arr.push(4); // modifies original array
console.log(arr); // [1,2,3,4]
```

Changes original reference → mutation.

---

## 🧊 Immutable (Cannot be changed directly)

Immutable updates create a **new copy** without modifying the original.

#### Example (Immutable Object)

```jsx
const user = { name: "Ramesh" };
const updatedUser = { ...user, name: "Arjun" };
```

`updatedUser` is new. `user` stays unchanged.

#### Example (Immutable Array)

```jsx
const arr = [1, 2, 3];
const newArr = [...arr, 4];
```

`newArr` is a separate array.

---

## 🧠 Why Immutability Matters in React

React re-renders only when **references change**:

* Mutable update → ❌ reference same → no rerender
* Immutable update → ✔ reference new → rerender triggered

Immutability ensures:

* Predictable UI
* Correct reactivity
* Reliable `React.memo` and `useEffect`
* Simplified debugging

---
