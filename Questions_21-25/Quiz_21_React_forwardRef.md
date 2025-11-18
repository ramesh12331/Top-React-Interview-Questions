## 21. What is **forwardRef()** in React used for?

# 📘 React `forwardRef()` — Full Guide

## ⭐ Introduction

`forwardRef()` is a powerful React API that allows a parent component to **pass a ref through a child component** so it can directly access a DOM element or child instance.

Normally, refs do **not** automatically get passed through custom components. `forwardRef()` solves this by explicitly forwarding refs.

---

## 🔍 What is `forwardRef()`?

`forwardRef()` lets you wrap a component and give the parent the ability to attach a `ref` to an inner DOM node.

This is useful when:

* You wrap an element and still want the parent to access it
* You need to control focus, animations, or scroll actions
* You build reusable input components

---

## 🧠 Syntax

```jsx
const MyComponent = forwardRef((props, ref) => {
  return <input ref={ref} {...props} />;
});
```

Here, the incoming `ref` is attached to the DOM element inside the component.

---

## 🟢 Simple Example — Focus Input from Parent

```jsx
import React, { forwardRef, useRef } from "react";

const CustomInput = forwardRef((props, ref) => {
  return <input ref={ref} {...props} />;
});

function App() {
  const inputRef = useRef();

  const focusInput = () => {
    inputRef.current.focus();
  };

  return (
    <div>
      <CustomInput ref={inputRef} placeholder="Type here" />
      <button onClick={focusInput}>Focus Input</button>
    </div>
  );
}

export default App;
```

✔ Parent controls child input
✔ Useful for auto-focus, validations, animations

---

## 🔥 Medium Example — Forwarding Ref in Reusable Components

```jsx
const TextField = forwardRef((props, ref) => {
  return (
    <div>
      <label>{props.label}</label>
      <input ref={ref} type="text" />
    </div>
  );
});

function Form() {
  const nameRef = useRef();

  function highlight() {
    nameRef.current.style.border = "2px solid red";
  }

  return (
    <>
      <TextField label="Name" ref={nameRef} />
      <button onClick={highlight}>Highlight Input</button>
    </>
  );
}
```

✔ Great for custom form components

---

## 🧩 Advanced Example — Exposing Methods with `useImperativeHandle`

`forwardRef()` becomes even more powerful when combined with `useImperativeHandle()`.

```jsx
import { forwardRef, useRef, useImperativeHandle } from "react";

const FancyInput = forwardRef((props, ref) => {
  const inputRef = useRef();

  useImperativeHandle(ref, () => ({
    focus: () => inputRef.current.focus(),
    clear: () => (inputRef.current.value = "")
  }));

  return <input ref={inputRef} />;
});

function App() {
  const fancyRef = useRef();

  return (
    <div>
      <FancyInput ref={fancyRef} />
      <button onClick={() => fancyRef.current.focus()}>Focus</button>
      <button onClick={() => fancyRef.current.clear()}>Clear</button>
    </div>
  );
}
```

✔ Expose custom methods to parent components
✔ Common in component libraries like Material UI, Chakra UI

---

## 🎯 When Should You Use `forwardRef()`?

Use it when:

* Parent needs direct access to a DOM element inside a child
* Building reusable input or form components
* Integrating with animation or third‑party libraries
* Exposing methods like focus, scroll, or clear

Avoid it when:

* State/props can solve the problem
* The child does not need to expose internal elements

---

## ❗ Mistakes to Avoid

❌ Forgetting to pass ref into `forwardRef` function
❌ Using `forwardRef` when not needed
❌ Mutating ref without `useImperativeHandle`
❌ Not attaching ref to actual DOM element

---

## ⚡ Best Practices

✔ Use for wrapper components that must expose inner DOM
✔ Document ref behavior in component API
✔ Combine with `useImperativeHandle()` for custom methods
✔ Keep component structure simple when forwarding refs

---

## 🔧 Tricks

### 🔹 Append ref to multiple DOM nodes (via imperative handle)

```jsx
useImperativeHandle(ref, () => ({
  focusFirst: () => first.current.focus(),
  focusLast: () => last.current.focus()
}));
```

### 🔹 Use with motion libraries for animations

### 🔹 Perfect for reusable UI libraries

---

## 📝 Summary

* `forwardRef()` allows parent components to access child DOM nodes
* Helps manage focus, selections, animations, scroll control
* Used heavily in UI libraries and advanced React patterns
* Even more powerful with `useImperativeHandle()`

---

## 🎤 Interview Questions & Answers

### 🟢 Basic Level

**❓ What is `forwardRef`?**
💡 An API that forwards the parent ref to a child DOM element.

**❓ Why can’t you normally access a DOM node inside a custom component?**
💡 Because refs don't automatically pass through components.

---

### 🟡 Intermediate Level

**❓ What problem does `forwardRef()` solve?**
💡 It allows parent components to access internal DOM elements of wrapped components.

**❓ When should you use `forwardRef` instead of `useState`?**
💡 When direct DOM manipulation is needed, like focusing or selecting text.

---

### 🔥 Advanced Level

**❓ How do you expose custom functions using `forwardRef`?**
💡 By combining it with `useImperativeHandle()`.

**❓ Why is `forwardRef` commonly used in component libraries?**
💡 Because reusable components must expose DOM handles (e.g., input focus, animations).

---
