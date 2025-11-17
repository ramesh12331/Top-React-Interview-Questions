
# 📘 React `useReducer` Hook — Full Guide

## ⭐ Introduction

The React **useReducer** Hook is an alternative to `useState` for managing **complex state logic**. It is best used when:

* State contains **multiple sub-values**
* The next state depends on the **previous state**
* The state logic is **complex or multi-step**

---

## 🔍 What is `useReducer`?

`useReducer` is a hook that manages state through a **reducer function**.

A reducer is a pure function:

```
(state, action) => newState
```

It returns a **new updated state** based on the given action.

---

## 🧠 Syntax

```js
const [state, dispatch] = useReducer(reducer, initialState);
```

### Reducer function:

```js
function reducer(state, action) {
  switch(action.type) {
    case "ACTION":
      return { ...state };
    default:
      throw new Error("Unknown action type");
  }
}
```

---

## 🟢 Simple Example (with Full Explanation)

### ✅ Code

```jsx
import React, { useReducer } from 'react';

const initialState = { count: 0 };

function reducer(state, action) {
  switch(action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      throw new Error('Unknown action');
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <h1>Count: {state.count}</h1>
      <button onClick={() => dispatch({ type: 'increment' })}> + </button>
      <button onClick={() => dispatch({ type: 'decrement' })}> - </button>
    </div>
  );
}

export default Counter;
```

### 📝 Simple Example — Step-by-Step Explanation

#### 🔹 1. Initial State

```js
const initialState = { count: 0 };
```

This is the starting value of the counter.

---

#### 🔹 2. Reducer Function

```js
function reducer(state, action) {
  switch(action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      throw new Error('Unknown action');
  }
}
```

The reducer receives:

* `state` → current state
* `action` → something like `{ type: "increment" }`

It returns a **new state** depending on `action.type`.

---

#### 🔹 3. Setting up `useReducer`

```js
const [state, dispatch] = useReducer(reducer, initialState);
```

This gives:

* `state` → the current state
* `dispatch` → a function to send actions

---

#### 🔹 4. Dispatching Actions

```jsx
<button onClick={() => dispatch({ type: 'increment' })}> + </button>
<button onClick={() => dispatch({ type: 'decrement' })}> - </button>
```

`dispatch` sends an action object to the reducer.

---

#### 🔹 5. Rendering State

```jsx
<h1>Count: {state.count}</h1>
```

React re-renders the UI whenever the reducer returns a new state.

---

## 🔥 Real-Time Example — Expensive State Management

### 🟠 Medium Example – Form State Handling

`useReducer` is excellent for forms with multiple fields.

```jsx
const initialState = { name: "", email: "" };

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
      <input name="name" value={state.name} onChange={handleChange} />
      <input name="email" value={state.email} onChange={handleChange} />
    </>
  );
}
```

➡ Perfect for complex forms.

---

### 🧩 Real-Time Advanced Example — Game Scoreboard

```jsx
const initialScore = [
  { id: 1, name: "John", score: 0 },
  { id: 2, name: "Sally", score: 0 },
];

function reducer(state, action) {
  switch (action.type) {
    case "INCREASE":
      return state.map(player =>
        player.id === action.id
          ? { ...player, score: player.score + 1 }
          : player
      );
    default:
      return state;
  }
}

function Score() {
  const [score, dispatch] = useReducer(reducer, initialScore);

  return (
    <>
      {score.map(player => (
        <div key={player.id}>
          <button onClick={() => dispatch({ type: "INCREASE", id: player.id })}>
            {player.name}
          </button>
          {player.score}
        </div>
      ))}
    </>
  );
}
```

➡ Used in multiplayer games, scoreboards, team trackers.

---

### 🏗 Real-Time Advanced Example — Undo/Redo (State History)

```jsx
const initialState = {
  past: [],
  present: 0,
  future: []
};

function reducer(state, action) {
  const { past, present, future } = state;

  switch(action.type) {
    case "INCREMENT":
      return {
        past: [...past, present],
        present: present + 1,
        future: []
      };

    case "UNDO":
      if (past.length === 0) return state;
      return {
        past: past.slice(0, -1),
        present: past[past.length - 1],
        future: [present, ...future]
      };

    case "REDO":
      if (future.length === 0) return state;
      return {
        past: [...past, present],
        present: future[0],
        future: future.slice(1)
      };

    default:
      return state;
  }
}
```

➡ Commonly used for text editors, drawing apps, design tools, and undoable workflows.

---

*### 🧨 Advanced Example — Complex Form with Nested State

```jsx
const initialState = {
  user: {
    name: "",
    email: "",
  },
  preferences: {
    theme: "light",
    notifications: true,
  }
};

function reducer(state, action) {
  switch (action.type) {
    case "UPDATE_USER":
      return {
        ...state,
        user: {
          ...state.user,
          [action.field]: action.value,
        },
      };

    case "TOGGLE_NOTIFICATIONS":
      return {
        ...state,
        preferences: {
          ...state.preferences,
          notifications: !state.preferences.notifications,
        },
      };

    default:
      throw new Error("Unknown action type");
  }
}

function ProfileForm() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <input
        name="name"
        placeholder="Name"
        value={state.user.name}
        onChange={(e) => dispatch({ type: "UPDATE_USER", field: "name", value: e.target.value })}
      />

      <input
        name="email"
        placeholder="Email"
        value={state.user.email}
        onChange={(e) => dispatch({ type: "UPDATE_USER", field: "email", value: e.target.value })}
      />

      <label>
        <input
          type="checkbox"
          checked={state.preferences.notifications}
          onChange={() => dispatch({ type: "TOGGLE_NOTIFICATIONS" })}
        />
        Notifications
      </label>
    </div>
  );
}
```

### 🛒 Advanced Example — Shopping Cart Manager

```jsx
const initialCart = [];

function reducer(cart, action) {
  switch (action.type) {
    case "ADD_ITEM":
      return [...cart, action.item];

    case "REMOVE_ITEM":
      return cart.filter((i) => i.id !== action.id);

    case "CLEAR_CART":
      return [];

    default:
      throw new Error("Unknown action type");
  }
}
```

This is commonly asked in interviews and is used in real e‑commerce apps.

---

## 🆚 Difference Between useState, useReducer, and Redux

| Feature               | useState         | useReducer          | Redux                                |
| --------------------- | ---------------- | ------------------- | ------------------------------------ |
| **Best For**          | Simple state     | Complex/local state | Large-scale global state             |
| **State Location**    | Local            | Local               | Global (store)                       |
| **Logic Handling**    | Inside component | Reducer function    | Reducers + actions + middleware      |
| **Boilerplate**       | Very low         | Medium              | High                                 |
| **Async Support**     | Manual           | Manual              | Built‑in via middleware (Thunk/Saga) |
| **Debugging Tools**   | Minimal          | Good structure      | Excellent DevTools                   |
| **Re-render Control** | Component level  | Component level     | App-level control                    |
| **Learning Curve**    | Easiest          | Medium              | Hardest                              |

### 👉 When to choose what?

* **useState** → Simple state like toggles, inputs, counters.
* **useReducer** → Complex state logic, multiple related values, predictable state transitions.
* **Redux** → App-wide global state, complex async flows, debugging, large teams.*

---

## 🧩 When to Use `useReducer`

* Complex state logic
* Multiple related state values
* Updates depend on previous state
* Managing forms, nested objects, or UI flows

---

## ❗ Mistakes to Avoid

* Using `useReducer` for simple state (overkill)
* Not handling unknown actions
* Mutating state inside reducer

---

## ⚡ Tricks & Best Practices

* Keep reducer pure
* Use constants for action types
* Combine with Context API for global state

---

## 🎤 Interview Questions & Answers

Below are **interactive-style Q&A icons** for better readability:

* ❓ = Question
* 💡 = Answer

*### 🟢 Basic Level

**❓ 1. What is useReducer in React?**
💡 useReducer is a hook that manages state using a reducer function.

**❓ 2. When should you prefer useReducer over useState?**
💡 When state has multiple sub-values or logic is complex.

**❓ 3. What does dispatch do?**
💡 It sends an action to the reducer to update the state.

---

### 🟡 Intermediate Level

**❓ 4. What is a pure reducer function?**
💡 A function that returns the same output for the same inputs and produces no side effects.

**❓ 5. Can useReducer improve performance?**
💡 Yes, by centralizing and structuring update logic.

**❓ 6. How do you structure action objects?**
💡 Use a `type` and an optional `payload` to send meaningful data.

---

### 🔥 Advanced Level

**❓ 7. How do you integrate useReducer with Context API?**
💡 Wrap your component tree with a Context Provider and pass `state` + `dispatch`.

**❓ 8. How to implement undo/redo using useReducer?**
💡 Maintain `past`, `present`, and `future` arrays to track state history.

**❓ 9. How is useReducer similar to Redux?**
💡 Both use reducers, dispatching, and action types.

**❓ 10. Can you use multiple reducers?**
💡 Yes. You can combine reducers manually or use multiple hooks.
