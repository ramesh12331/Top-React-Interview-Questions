# 📘 React — Component State Resetting (పూర్తి తెలుగు గైడ్)

## ⭐ పరిచయం

React లో state reset చేయడం అంటే component ని **initial values** కి తిరిగి తీసుకురావడం. ఇది ముఖ్యంగా ఈ సందర్భాల్లో ఉపయోగపడుతుంది:

* ఫారమ్ క్లియర్ చేయడం
* UI ని రీసెట్ చేయడం
* Filters / Timers / Pagination reset చేయడం
* Multi-step forms లో back లేదా cancel చేయడం

React లో state ని reset చేయడానికి, దాన్ని **initial state object** కి తిరిగి సెట్ చేస్తాం.

---

## 🔍 State Reset చేయడం అంటే ఏమిటి?

React లో state ను update చేయడానికి:

* Functional components → `useState`
* Class components → `setState`

State reset చేయాలంటే, setter function ని ఉపయోగించి initial state ని తిరిగి assign చేస్తాం.

✔ Direct mutation చేయకూడదు
✔ ఎప్పుడూ **కొత్త object** return కావాలి

---

## 🧠 Syntax

```jsx
const [state, setState] = useState(initialState);

setState(initialState); // state reset
```

---

## 🟢 Simple Example — Form Reset

```jsx
const initialState = { name: '', email: '' };

function Form() {
  const [formData, setFormData] = useState(initialState);

  const handleReset = () => setFormData(initialState);

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
```

✔ Reset బటన్ క్లిక్ చేస్తే form మొత్తం initial values కి వెళ్లిపోతుంది

---

## 🔥 Medium Example — Multiple Fields Reset

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

✔ Complex forms లో చాలా ఉపయోగపడుతుంది

---

## 🧩 Advanced — useReducer తో Reset

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

✔ Nested state, complex logic ఉన్నప్పుడు best approach

---

## 🎯 ఎప్పుడు State Reset చేయాలి?

* Form submit / cancel
* Filters clear చేయడం
* Timers / Counters reset
* View మారినప్పుడు
* Pagination reset

---

## ❗ తప్పులు చేయకూడదు

❌ State ని direct గా mutate చేయడం
❌ Nested objects clone చేయకుండా reset చేయడం
❌ Stale initialState ఉపయోగించడం
❌ initialState ని component లో define చేయడం (ప్రతి render కొత్త object అవుతుంది)

---

## ⚡ Best Practices

✔ initialState ని component బయట define చేయాలి
✔ ఎప్పుడూ immutable updates వాడాలి
✔ Complex state కోసం `useReducer` వాడాలి
✔ Partial reset కంటే full object reset safe

---

## 🔧 Useful Tricks

### 🔹 Component ని పూర్తిగా reset చేయడానికి `key` prop

```jsx
function MyForm() {
  const [key, setKey] = useState(0);
  return <Form key={key} onReset={() => setKey(prev => prev + 1)} />;
}
```

→ Component remount అవుతుంది → పూర్తిగా fresh state వస్తుంది

### 🔹 Custom Hook తో Reusable Reset Logic

```jsx
function useResettableState(initial) {
  const [state, setState] = useState(initial);
  const reset = () => setState(initial);
  return [state, setState, reset];
}
```

---

## 📝 Summary

* State reset అంటే initial values కి తిరిగి తీసుకురావడం
* `useState` లేదా `useReducer` ద్వారా reset చేయొచ్చు
* Forms, filters, timers లో చాలా అవసరం
* Immutable updates తప్పనిసరి

---

## 🎤 Interview Q&A

### 🟢 Basic

**❓ State reset అంటే ఏమిటి?**
→ Initial value కి తిరిగి సెట్ చేయడం

**❓ useState తో ఎలా reset చేయాలి?**
→ `setState(initialState)`

---

### 🟡 Intermediate

**❓ initialState ని component బయట ఎందుకు define చేయాలి?**
→ ప్రతి render లో కొత్త object create కాకుండా ఉండడానికి

**❓ Nested state reset లో తప్పు ఏంటి?**
→ Clone చేయకుండా mutate చేయడం

---

### 🔥 Advanced

**❓ Complex state ను ఎలా reset చేయాలి?**
→ `useReducer` + `reset` action

**❓ Component ని పూర్తిగా reset చేయడం ఎలా?**
→ దాని `key` prop మార్చాలి (React remount చేస్తుంది)
