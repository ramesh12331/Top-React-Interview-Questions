## 22. How do you **reset a component’s state** in React?

# 📘 React — Resetting Component State (Full Guide)

## ⭐ Introduction

Resetting a component's state in React means restoring it to its **initial values**.
This is commonly needed when:

* Clearing forms
* Resetting UI after a successful action
* Navigating between views
* Resetting filters, selections, timers, and more

React allows you to reset state by simply setting it back to the **initial state object**.

---

## 🔍 What Does It Mean to Reset State?

React state is updated using:

* `setState` (class components)
* `useState` (functional components)

To reset state, you reassign the original state value using the setter function.

✔ No direct mutation
✔ Always return a **new object**

---

## 🧠 Syntax

```jsx
const [state, setState] = useState(initialState);

setState(initialState); // reset back to original
```

---

## 🟢 Simple Example — Resetting a Form

```jsx
import React, { useState } from 'react';

const initialState = { name: '', email: '' };

function Form() {
  const [formData, setFormData] = useState(initialState);

  const handleReset = () => {
    setFormData(initialState);
  };

  return (
    <div>
      <input
        value={formData.name}
        onChange={(e) => setFormData({ ...formData, name: e.target.value })}
        placeholder="Name"
      />

      <input
        value={formData.email}
        onChange={(e) => setFormData({ ...formData, email: e.target.value })}
        placeholder="Email"
      />

      <button onClick={handleReset}>Reset</button>
    </div>
  );
}

export default Form;
```

✔ Clicking reset restores the form to default empty values

---

## 🔥 Medium Example — Resetting State with Multiple Fields

```jsx
const initialState = {
  user: { name: '', age: '' },
  settings: { theme: 'light', notifications: true }
};

function App() {
  const [state, setState] = useState(initialState);

  const resetAll = () => setState(initialState);

  return (
    <>
      <input
        value={state.user.name}
        onChange={(e) => setState({
          ...state,
          user: { ...state.user, name: e.target.value }
        })}
      />

      <button onClick={resetAll}>Reset Everything</button>
    </>
  );
}
```

✔ Useful for complex forms

---

## 🧩 Advanced Example — Resetting Using a Reducer (Best for Complex State)

```jsx
const initialState = { count: 0, step: 1 };

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { ...state, count: state.count + state.step };
    case 'reset':
      return initialState;
    default:
      return state;
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <h1>{state.count}</h1>
      <button onClick={() => dispatch({ type: 'increment' })}>Increment</button>
      <button onClick={() => dispatch({ type: 'reset' })}>Reset</button>
    </div>
  );
}
```

✔ Best for complex or nested state requiring clear reset logic

---

## 🎯 When Should You Reset State?

Use reset when:

* ✔ Submitting or cancelling a form
* ✔ Switching between different data views
* ✔ Clearing controlled input fields
* ✔ Resetting timers, counters, filters
* ✔ Resetting pagination or query data

---

## ❗ Mistakes to Avoid

❌ Mutating state directly
❌ Using stale references when resetting
❌ Forgetting to clone nested objects
❌ Creating initialState *inside* component (causes recreation)

---

## ⚡ Best Practices

✔ Store `initialState` **outside** the component
✔ Use state immutability — always return new objects
✔ For complex state, use `useReducer`
✔ Reset entire objects, not individual keys

---

## 🔧 Tricks

### 🔹 Reset with a unique `key` prop

```jsx
function MyForm() {
  const [key, setKey] = useState(0);
  return <Form key={key} onReset={() => setKey(prev => prev + 1)} />;
}
```

This fully remounts the component — perfect for multi-step forms.

### 🔹 Use a custom hook for reusable reset logic

```jsx
function useResettableState(initial) {
  const [state, setState] = useState(initial);
  const reset = () => setState(initial);
  return [state, setState, reset];
}
```

---

## 📝 Summary

* Resetting state means restoring initial values
* Works with `useState` and `useReducer`
* Essential for forms, filters, timers, UI resets
* Always use immutability when resetting state

---

## 🎤 Interview Questions & Answers

### 🟢 Basic Level

**❓ What does resetting state mean?**
💡 Setting state back to its initial value.

**❓ How do you reset state with useState?**
💡 `setState(initialState)`

---

### 🟡 Intermediate Level

**❓ Why should initialState be defined outside the component?**
💡 To avoid recreating the initial object on every render.

**❓ What is a common mistake when resetting nested state?**
💡 Forgetting to clone nested objects.

---

### 🔥 Advanced Level

**❓ How do you reset complex state?**
💡 Use `useReducer` with a `reset` action.

**❓ How do you force a full component reset?**
💡 Change its `key` prop so React remounts it.

---
