# 📘 React `useRef` Hook — పూర్తి తెలుగు గైడ్

> Definition, syntax, simple → advanced examples, real‑time use cases, mistakes, best practices, tricks, summary, మరియు Interview Q&A కలిగిన పూర్తి guide.

---

## ⭐ Introduction

React లో **`useRef`** hook ద్వారా component re-render అయ్యినా కూడా **persist అయ్యే mutable reference** ని create చేయవచ్చు.

✔ ఇది re-renders మధ్య value ని నిలుపుకుంటుంది
✔ `.current` property ద్వారా value access చేయవచ్చు
❌ `.current` update చేసినా **re-render జరగదు**

ఇది ఎక్కువగా DOM elements కి direct access కోసం వాడతారు.

---

## 📌 Definition

`useRef` ఒక object ని return చేస్తుంది:

```
{ current: ... }
```

ఇది component మొత్తం lifecycle లో ఒకేలా ఉంటుంది.

### `useRef` ని ఎప్పుడు వాడాలి?

* DOM elements access చేయాలి (focus, scroll)
* Previous values store చేయాలి
* Timers, intervals, API connections store చేయాలి
* Re-render trigger కాకూడని values save చేయాలి

---

## 🧠 Syntax

```jsx
const ref = useRef(initialValue);

// Access
ref.current;
```

---

## 🟢 Simple Example — Input Auto Focus

```jsx
function FocusInput() {
  const inputRef = useRef(null);

  useEffect(() => {
    inputRef.current.focus();
  }, []);

  return <input ref={inputRef} type="text" />;
}
```

✔ DOM element reference తో ఆటో focus.

---

## 🔥 Medium Example — Render Count Track (Without Re-render)

```jsx
function App() {
  const [value, setValue] = useState("");
  const count = useRef(0);

  useEffect(() => {
    count.current = count.current + 1;
  });

  return (
    <>
      <input value={value} onChange={(e) => setValue(e.target.value)} />
      <h1>Render Count: {count.current}</h1>
    </>
  );
}
```

✔ count మారినా UI re-render కాదు.

---

## 🧩 Advanced Example — Button Click లో DOM Access

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

✔ Button click → inputకు focus.

---

## 🏗 Real-Time Example — Previous State Store

```jsx
function App() {
  const [value, setValue] = useState("");
  const prevValue = useRef("");

  useEffect(() => {
    prevValue.current = value;
  }, [value]);

  return (
    <>
      <input value={value} onChange={(e) => setValue(e.target.value)} />
      <h2>Current: {value}</h2>
      <h2>Previous: {prevValue.current}</h2>
    </>
  );
}
```

✔ Previous value ను track చేయడానికి perfect.

---

## 🎯 Main Purposes of `useRef`

### ✔ DOM access

* input focus
* scroll position
* element size measure

### ✔ Store values without re-renders

* counters
* flags
* temporary variables

### ✔ External resource references

* setTimeout / setInterval IDs
* WebSocket లేదా API connections

---

## ❗ Mistakes to Avoid

❌ UI లో చూపించాలి అనుకునే value ను `useRef` లో పెట్టడం (re-render అవదు)
❌ State స్థానంలో ref వాడడం
❌ `.current` మార్చితే UI update అవుతుందని అనుకోవడం
❌ Large data structures refs లో ఉంచడం

---

## ⚡ Best Practices

✔ DOM reference కోసం మాత్రమే వాడండి
✔ Non-UI values కోసం వాడండి
✔ `useEffect` తో combine చేసి DOM control చేయండి
✔ UI state కోసం ఎప్పుడూ `useState` వాడాలి

---

## 🔧 Tricks

### 🔹 Heavy Computation Cache చేయడం

```jsx
const expensiveValue = useRef(computeHeavy());
```

### 🔹 Timers Store చేయడం

```jsx
const timerRef = useRef();

useEffect(() => {
  timerRef.current = setInterval(() => console.log("tick"), 1000);
  return () => clearInterval(timerRef.current);
}, []);
```

### 🔹 Smooth Scroll

```jsx
ref.current.scrollIntoView({ behavior: "smooth" });
```

---

## 📝 Summary

* `useRef` persistent, mutable reference object return చేస్తుంది
* `.current` update చేసినా re-render కాదు
* DOM access, timers, previous values కోసం ideal
* UI state కోసం కాదు ⇒ `useState` వాడాలి

---

# 🎤 Interview Questions & Answers

### 🟢 1. `useRef` అంటే ఏమిటి?

Persistent reference object; re-render అవకుండా data నిలుపుతుంది.

---

### 🟢 2. `useRef` ఎప్పుడు వాడాలి?

* DOM access
* timers / intervals store చేయడానికి
* previous values save చేయడానికి
* mutable values store చేయడానికి

---

### 🟡 3. `.current` update చేస్తే UI update అవుతుందా?

**లేదు** — ఇది re-render cause చేయదు.

---

### 🔥 4. Infinite loops ఎలా నివారిస్తుంది?

State లాగా re-render చేయదు కాబట్టి.

---

### 🔥 5. Previous state store చేయడానికి వాడవచ్చా?

అవును — `useEffect` లో `.current` update చేస్తే సరిపోతుంది.

---

మీకు కావాలంటే:
📌 Table of contents
🎨 Formatting improvements
📄 PDF/DOCX గా export చేసి ఇస్తాను!
