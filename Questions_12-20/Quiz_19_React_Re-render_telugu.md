# 📘 React Re-rendering — Full Guide

> A complete, interview‑ready guide with definition, syntax, examples, real‑time use cases, purpose, tricks, summary, and Q&A.

---

## ⭐ Introduction

React **re-rendering** అంటే component యొక్క **state**, **props**, లేదా **context** మారినప్పుడు, React UI ను మళ్లీ update చేయడం. React virtual DOM ను regenerate చేసి, పాత virtual DOM తో compare చేసి, real DOM లో అవసరం ఉన్న changes మాత్రమే update చేస్తుంది.

---

## 📌 Definition

**Re-rendering** అంటే:

* Component యొక్క output ని మళ్లీ calculate చేయడం
* Virtual DOM ని recreate చేయడం
* Diffing చేసి, real DOM లో మార్పులు apply చేయడం

ఇది UI ను logic కి అనుగుణంగా ఉంచేది.

---

## 🧠 Syntax (Triggers for Re-render)

Re-render జరిగే సందర్భాలు:

```jsx
setState(newValue);
```

```jsx
<Component newProps={value} />
```

```jsx
<MyContext.Provider value={...}>
```

---

## 🟢 Simple Example — State Change Causes Re-render

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  console.log("Component rendered");

  return (
    <div>
      <h1>Count: {count}</h1>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

State మారినప్పుడల్లా component re-render అవుతుంది.

---

## 🔥 Medium Example — Props వల్ల Re-render

```jsx
function Child({ value }) {
  console.log("Child rendered");
  return <h2>Value: {value}</h2>;
}

function Parent() {
  const [num, setNum] = useState(1);

  return (
    <>
      <Child value={num} />
      <button onClick={() => setNum(num + 1)}>Update Value</button>
    </>
  );
}
```

Parent state మారితే Child కూడా re-render అవుతుంది.

---

## 🧩 Advanced Example — Context Triggering Re-renders

```jsx
const ThemeContext = React.createContext();

function ThemeProvider() {
  const [theme, setTheme] = useState("light");

  return (
    <ThemeContext.Provider value={theme}>
      <Toolbar />
      <button onClick={() => setTheme(theme === "light" ? "dark" : "light")}>
        Toggle Theme
      </button>
    </ThemeContext.Provider>
  );
}

function Toolbar() {
  const theme = React.useContext(ThemeContext);
  console.log("Toolbar rendered");

  return <div>Theme: {theme}</div>;
}
```

Context value మారితే అన్ని consumers re-render అవుతాయి.

---

## 🏗 Real-Time Example — Search Input Live Filtering

```jsx
function SearchList({ items }) {
  const [query, setQuery] = useState('');

  const filtered = items.filter(item => item.includes(query));

  return (
    <div>
      <input
        placeholder="Search..."
        value={query}
        onChange={(e) => setQuery(e.target.value)}
      />

      {filtered.map((item, i) => (
        <p key={i}>{item}</p>
      ))}
    </div>
  );
}
```

ప్రతి keystroke → state change → re-render → filtered list update.

---

## 🎯 When Does Re-rendering Happen?

* ✔ State update
* ✔ Props change
* ✔ Context value change
* ✔ Force update (rare)

---

## ❗ Mistakes to Avoid

❌ Inline functions object references మార్చడం ద్వారా re-render పెరుగుతుంది

❌ Deep state structures వల్ల unnecessary rendering

❌ State మారితే ఎప్పుడూ re-render అవుతుందనే విషయం మర్చిపోవడం

❌ Memoization లేకుండా కొత్త objects/arrays పంపడం

---

## ⚡ Best Practices

✔ State ని minimal & flat గా ఉంచండి

✔ `React.memo` వాడండి

✔ Functions కోసం `useCallback` వాడండి

✔ Expensive calculations కోసం `useMemo` వాడండి

✔ Context ను ఎక్కువగా update చేయొద్దు

✔ State ని అవసరమైతేనే lift చేయండి

---

## 🔧 Tricks & Optimization Patterns

### 🔹 Memoizing Components

```jsx
const Child = React.memo(function Child({ value }) {
  return <p>{value}</p>;
});
```

### 🔹 Memoizing Functions

```jsx
const handleClick = useCallback(() => {
  console.log("Clicked");
}, []);
```

### 🔹 Memoizing Lists

```jsx
const itemList = useMemo(() => items.map(i => i * 2), [items]);
```

### 🔹 Split Large Components

Big components → smaller memoized subcomponents.

---

## 📝 Summary

* React re-renders when **state, props, or context** change.
* Re-rendering అంటే virtual DOM regenerate చేసి real DOM లో minimal updates చేయడం.
* Memoization + good component structure performance పెంచుతుంది.

---

# 🎤 Interview Questions & Answers

### 🟢 1. What is re-rendering?

State, props, లేదా context మారినప్పుడు component మళ్లీ update అవడం.

---

### 🟢 2. What triggers re-renders?

* State change
* Props change
* Context change
* Force update

---

### 🟢 3. How does React update the DOM efficiently?

Virtual DOM + diffing algorithm ద్వారా.

---

### 🟡 4. How to prevent unnecessary re-renders?

`useMemo`, `useCallback`, `React.memo` వాడండి.

---

### 🟡 5. Why does state change cause re-render?

State UI కి సంబంధిత data కాబట్టి.

---

### 🔥 6. Debug performance issues?

* React DevTools → Highlight Updates
* Props unnecessary changes check చేయండి
* Memoization వాడండి

---

### 🔥 7. Does parent update → child always re-render?

అవును, కానీ child ను `React.memo` తో optimize చేయవచ్చు.
