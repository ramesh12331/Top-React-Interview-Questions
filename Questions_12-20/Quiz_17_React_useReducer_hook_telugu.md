# 📘 React `useReducer` Hook — పూర్తి తెలుగు గైడ్

## ⭐ పరిచయం

React **useReducer** Hook అనేది `useState` కు ప్రత్యామ్నాయం, ప్రత్యేకంగా **complex state logic** ఉన్నప్పుడు ఉపయోగపడుతుంది. ఇది బాగా ఉపయోగపడే సందర్భాలు:

* State లో **multiple sub-values** ఉన్నప్పుడు
* Next state, previous state మీద ఆధారపడినప్పుడు
* State updates **complex లేదా multi‑step** ఉన్నప్పుడు

---

## 🔍 `useReducer` అంటే ఏమిటి?

`useReducer` state ను ఒక **reducer function** ద్వారా నిర్వహిస్తుంది.

Reducer అనేది ఒక pure function:

```
(state, action) => newState
```

ఇది ఇచ్చిన action ఆధారంగా ఒక **కొత్త state** ను return చేస్తుంది.

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

## 🟢 Simple Example — Counter (Full Explanation)

```jsx
const initialState = { count: 0 };

function reducer(state, action) {
  switch(action.type) {
    case 'increment': return { count: state.count + 1 };
    case 'decrement': return { count: state.count - 1 };
    default: throw new Error('Unknown action');
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <h1>Count: {state.count}</h1>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
    </div>
  );
}
```

### Step-by-Step వివరణ

#### 🔹 1. Initial State

```js
const initialState = { count: 0 };
```

#### 🔹 2. Reducer Function

Action ఆధారంగా కొత్త state తయారవుతుంది.

#### 🔹 3. useReducer setup

```js
const [state, dispatch] = useReducer(reducer, initialState);
```

#### 🔹 4. Actions పంపడం

```jsx
dispatch({ type: 'increment' })
```

#### 🔹 5. UI re-render

కొత్త state వచ్చినప్పుడు component మళ్లీ render అవుతుంది.

---

## 🔥 Medium Example — Form State Handling

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

➡ Complex forms కి best.

---

## 🧩 Advanced Example — Game Scoreboard

```jsx
const initialScore = [
  { id: 1, name: "John", score: 0 },
  { id: 2, name: "Sally", score: 0 },
];

function reducer(state, action) {
  switch (action.type) {
    case "INCREASE":
      return state.map(p => p.id === action.id ? { ...p, score: p.score + 1 } : p);
    default:
      return state;
  }
}
```

➡ Games, scoreboards లో ఉపయోగపడుతుంది.

---

## 🏗 Advanced — Undo/Redo Logic

Undo/redo వంటి workflows లో చాలా ఉపయోగపడుతుంది.

```jsx
const initialState = { past: [], present: 0, future: [] };
```

➡ Text editors, drawing tools లో ఎక్కువగా వాడతారు.

---

## 🧨 Complex Nested State Example

User data + Preferences వంటి nested structures handle చేయడానికి perfect.

---

## 🛒 Shopping Cart Example

```jsx
function reducer(cart, action) {
  switch (action.type) {
    case "ADD_ITEM": return [...cart, action.item];
    case "REMOVE_ITEM": return cart.filter(i => i.id !== action.id);
    case "CLEAR_CART": return [];
    default: throw new Error("Unknown action type");
  }
}
```

➡ Real e‑commerce apps లో చాలా ఉపయోగపడుతుంది.

---

## 🆚 useState vs useReducer vs Redux

| Feature     | useState     | useReducer          | Redux                 |
| ----------- | ------------ | ------------------- | --------------------- |
| Best For    | Simple state | Complex local state | Global state          |
| Logic       | In component | Reducer             | Reducers + middleware |
| Boilerplate | Low          | Medium              | High                  |
| Debugging   | Low          | Good                | Excellent             |

### ఎప్పుడు ఏది వాడాలి?

* **useState** → Simple toggles, inputs
* **useReducer** → Complex logic
* **Redux** → Global shared complex flows

---

## 🧩 When to Use `useReducer`

* Complex state logic
* Related multiple state values
* Previous state పై ఆధారపడిన updates
* Forms, games, nested objects

---

## ❗ Mistakes to Avoid

❌ Simple state కి useReducer వాడటం (overkill)
❌ Unknown actions handle చేయకపోవడం
❌ Reducer లో state mutate చేయడం

---

## ⚡ Best Practices

✔ Reducer pure గా ఉంచండి
✔ Action types constants గా ఉంచండి
✔ Context + useReducer = Local global state management

---

## 🎤 Interview Questions & Answers

### 🟢 Basic

**❓ useReducer అంటే ఏమిటి?**
💡 Reducer ద్వారా state manage చేసే hook.

**❓ useState కంటే useReducer ఎప్పుడు ఉపయోగిస్తారు?**
💡 State logic complex ఉన్నప్పుడే.

### 🟡 Intermediate

**❓ Pure reducer అంటే ఏమిటి?**
💡 Same inputs → same output, no side effects.

### 🔥 Advanced

**❓ useReducer ను Context తో ఎలా integrate చేస్తారు?**
💡 State + dispatch ను Context Provider ద్వారా పంపుతారు.

**❓ undo/redo ఎలా implement చేస్తారు?**
💡 past, present, future arrays తో.
