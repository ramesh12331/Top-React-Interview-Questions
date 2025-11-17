# 📘 React `useId` Hook — Full Guide

> A complete, interview‑ready guide to React’s `useId` hook with examples, best practices, and accessibility patterns.

---

## 📑 Table of Contents

1. [Introduction](#-introduction)
2. [What is `useId`?](#-what-is-useid)
3. [Syntax](#-syntax)
4. [Basic Example – Single Input](#-basic-example--single-input)
5. [Medium Example – Multiple Inputs](#-medium-example--multiple-inputs)
6. [Advanced Example – Repeated Components](#-advanced-example--repeated-components)
7. [Accessibility & ARIA Example](#-accessibility--aria-example)
8. [When to Use `useId`](#-when-to-use-useid)
9. [Mistakes to Avoid](#-mistakes-to-avoid)
10. [Best Practices](#-best-practices)
11. [Tricks & Patterns](#-tricks--patterns)
12. [Summary](#-summary)
13. [Interview Questions & Answers](#-interview-questions--answers)

---

## ⭐ Introduction

The React **`useId`** Hook is used to generate **unique, stable IDs** that remain consistent across:

* Multiple renders
* Client and server rendering (SSR)
* Hydration

These IDs are most commonly used to connect **labels** and **inputs** for accessibility.

---

## 🔍 What is `useId`?

`useId` generates a unique ID string that:

* ✔ Is stable across renders
* ✔ Prevents ID collisions
* ✔ Works correctly in SSR + hydration scenarios
* ✔ Uses **no randomness**
* ✔ Uses **no global counters**

It is especially useful when you need IDs that must remain **predictable** and **unique**.

---

## 🧠 Syntax

```jsx
const id = useId();
```

This returns a unique ID string like:

```txt
:react-12345
```

You can append suffixes to create multiple related IDs.

---

## 🟢 Basic Example – Single Input

```jsx
import React, { useId } from "react";

function LoginForm() {
  const id = useId();

  return (
    <form>
      <label htmlFor={id}>Username:</label>
      <input id={id} type="text" />
    </form>
  );
}
```

**Why this is good:**

* ✔ Ensures the `<label>` is linked with the `<input>`
* ✔ ID stays stable across server + client rendering

---

## 🔥 Medium Example – Multiple Input Fields

```jsx
function ContactForm() {
  const nameId = useId();
  const emailId = useId();

  return (
    <>
      <div>
        <label htmlFor={nameId}>Name:</label>
        <input id={nameId} type="text" />
      </div>

      <div>
        <label htmlFor={emailId}>Email:</label>
        <input id={emailId} type="email" />
      </div>
    </>
  );
}
```

* ✔ Each ID is unique
* ✔ Safe for forms rendered many times

---

## 🧩 Advanced Example – Repeated Components

`useId` prevents ID clashes when components repeat.

```jsx
function Question({ label }) {
  const id = useId();

  return (
    <div>
      <label htmlFor={id}>{label}</label>
      <input id={id} />
    </div>
  );
}

function Survey() {
  return (
    <>
      <Question label="Your age" />
      <Question label="Your country" />
      <Question label="Your favorite color" />
    </>
  );
}
```

* ✔ Each repeated `<Question />` receives its own stable unique ID

---

## 🏗 Accessibility & ARIA Example

```jsx
function PasswordField() {
  const inputId = useId();
  const descriptionId = `${inputId}-description`;

  return (
    <div>
      <label htmlFor={inputId}>Password</label>
      <input
        id={inputId}
        type="password"
        aria-describedby={descriptionId}
      />

      <p id={descriptionId}>
        Your password must include a number and a special character.
      </p>
    </div>
  );
}
```

* ✔ Perfect for ARIA
* ✔ Enhances accessibility

---

## 🎯 When to Use `useId`

Use `useId` when:

* ✔ Linking labels with inputs
* ✔ Adding ARIA attributes
* ✔ Generating unique accessibility IDs
* ✔ Avoiding hydration mismatch in SSR
* ✔ Rendering multiple identical components requiring IDs

Do **NOT** use `useId` for:

* ❌ Keys in lists
* ❌ Generating random IDs
* ❌ Database identifiers
* ❌ Anything outside the DOM

---

## ❗ Mistakes to Avoid

* ❌ Using random values like `Math.random()` → breaks SSR
* ❌ Using global counters → inconsistent IDs on server/client
* ❌ Forgetting IDs for accessibility labels
* ❌ Manually managing IDs in large forms

---

## ⚡ Best Practices

* ✔ Always use `useId` for form accessibility
* ✔ Append suffixes for related IDs
* ✔ Use inside components (not outside)
* ✔ Do not use for dynamic list keys
* ✔ Keep IDs consistent by not wrapping components with unnecessary providers

---

## 🔧 Tricks & Patterns

### 🔹 Generate Multiple Related IDs

```jsx
const baseId = useId();

const titleId = `${baseId}-title`;
const descriptionId = `${baseId}-desc`;
```

### 🔹 Use Inside Reusable Components

* Avoid ID collisions in maps or repeated components
* Combine with ARIA attributes

---

## 📝 Summary

* `useId` generates **unique, stable IDs**
* Works on both **client and server**
* Essential for **accessibility**
* Prevents **hydration mismatches**
* Works great with **repeated components**

---

## 🎤 Interview Questions & Answers

### 🟢 Basic Level

**❓ What is `useId`?**
💡 A React hook that generates unique and stable IDs for accessibility and SSR.

**❓ Why not use `Math.random()` or counters for IDs?**
💡 They may mismatch between server and client during hydration.

---

### 🟡 Intermediate Level

**❓ How does `useId` help accessibility?**
💡 It ensures elements like `<label>` and `<input>` are properly linked using unique IDs.

**❓ Can you use `useId` for list keys?**
💡 No. List keys should reflect data identity, not random or generated values.

**❓ What does a `useId` value look like?**
💡 Something like `:r123-5`, generated internally by React.

---

### 🔥 Advanced Level

**❓ How does `useId` prevent hydration mismatches?**
💡 It generates the same ID on the server and client, ensuring consistent markup.

**❓ Can `useId` be used outside React components?**
💡 No. Hooks must be called only inside functional components.

**❓ How do you generate multiple related IDs using `useId`?**

```jsx
const id = useId();
const labelId = `${id}-label`;
const descId = `${id}-desc`;
```

---
