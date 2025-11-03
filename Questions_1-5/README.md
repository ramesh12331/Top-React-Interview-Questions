# ⚛️ React Interview Quick Guide

## 1️⃣ What is React?

React is a **JavaScript library** for building **dynamic UIs** using reusable **components**.

### ⭐ Benefits

* 🧩 **Component-Based** — Reusable pieces of UI.
* ⚡ **Virtual DOM** — Fast rendering.
* 💬 **Declarative** — Easier to read and debug.
* 🔁 **One-Way Data Flow** — Predictable updates.
* 💻 **JSX** — HTML + JavaScript syntax.

**Interview Q:** What is React?
**A:** A library for building fast, reusable UI components using Virtual DOM and one-way data flow.

---

## 2️⃣ React Node vs Element vs Component

| Concept      | Meaning                          | Example                           |
| ------------ | -------------------------------- | --------------------------------- |
| 🧱 Node      | Anything React can render        | `"Hello"`, `<div />`              |
| 🧩 Element   | Blueprint (object) of UI         | `<h1>Hello</h1>`                  |
| ⚙️ Component | Function/Class returning element | `function App() { return <h1/> }` |

**Interview Q:** Difference between Node, Element, and Component?
**A:** Node is renderable, Element is UI description, Component defines UI logic.

---

## 3️⃣ What is JSX?

JSX = **JavaScript XML** → lets you write HTML-like syntax in JS.

```jsx
const el = <h1>Hello World</h1>;
```

➡️ Compiles to:

```js
React.createElement('h1', null, 'Hello World');
```

**Interview Q:** Why use JSX?
**A:** It makes React code cleaner, more readable, and easier to visualize.

---

## 4️⃣ Props vs State

| Feature       | Props             | State              |
| ------------- | ----------------- | ------------------ |
| Controlled By | Parent            | Component          |
| Mutable?      | ❌ No              | ✅ Yes              |
| Purpose       | Configuration     | Dynamic data       |
| Re-render?    | If parent changes | When state changes |

**Interview Q:** Difference between props and state?
**A:** Props are read-only inputs; state is internal, mutable data.

---

## 5️⃣ Purpose of the Key Prop

**Key** → Unique ID for React list elements.
Helps React identify items that changed, added, or removed.

✅ Good:

```jsx
<li key={item.id}>{item.value}</li>
```

❌ Bad:

```jsx
<li key={index}>{item.value}</li>
```

**Interview Q:** Why use keys in lists?
**A:** To let React efficiently track and update list elements.

---

---

## 🧩 **Quick Interview Q&A**

| ❓ Question | 💡 Short Answer |
|-------------|----------------|
| What is React? | A JS library for building UI components. |
| What is JSX? | HTML-like syntax compiled to React elements. |
| What are Props? | Read-only inputs from parent components. |
| What is State? | Mutable data managed by the component itself. |
| Why use Keys in lists? | To help React efficiently update items. |
| Difference between Node, Element, Component? | Node: renderable output, Element: blueprint, Component: logic + UI. |

---

### ✨ Created with ❤️ by Ramesh

> *Quick, clean, and ready for your next React interview!*

---

**Tags:**
`#React` `#Interview` `#Props` `#State` `#JSX` `#Keys`
