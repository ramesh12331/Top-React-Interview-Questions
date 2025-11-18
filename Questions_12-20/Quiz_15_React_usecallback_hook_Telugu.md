# ⚛️ 15. React `useCallback` Hook అంటే ఏమిటి? ఎప్పుడు వాడాలి?

# 📘 React `useCallback` Hook — పూర్తి తెలుగు గైడ్

> Definition, syntax, simple → advanced examples, real-time use cases, purpose, mistakes, tricks, summary, మరియు Interview Q&A తో పూర్తి గైడ్.

---

## ⭐ Introduction

React లో **`useCallback`** Hook ని **functions ని memoize చేయడానికి** వాడుతారు. అంటే, ఒక function ప్రతి render కి కొత్తగా create కాకుండా, దాని **dependencies change అయ్యే వరకు అలా నే reuse** అవుతుంది.

ఇది ముఖ్యంగా ఈ సందర్భాల్లో ఉపయోగపడుతుంది:

* Functions ని **child components కి props గా పంపేటప్పుడు**
* `React.memo` వాడినప్పుడు child unnecessary గా re-render కాకుండా ఆపాలనుకున్నప్పుడు

---

## 📌 Definition

`useCallback` ఒక **memoized version of function** ని return చేస్తుంది.

👉 Dependencies change అయ్యే వరకు **అదే function reference** ఉంటుంది.
👉 Dependencies change అయితే కొత్త function create అవుతుంది.

---

## 🧠 Syntax

```jsx
const memoizedFn = useCallback(callbackFunction, [dependencies]);
```

### Arguments:

* **callbackFunction** → memoize చేయాలనుకునే function
* **dependencies** → change అయితే మాత్రమే function మళ్లీ create కావాల్సిన values array

---

## 🟢 Simple Example — Without `useCallback`

```jsx
function Parent() {
  const [count, setCount] = useState(0);

  const increment = () => {
    setCount(count + 1);
  };

  return <Child onIncrement={increment} />;
}
```

ఇక్కడ `increment` function **ప్రతి render కి కొత్తగా create అవుతుంది**.
`Child` component `React.memo` అయినా కూడా, function reference మారుతున్నందుకు re-render అవుతుంది.

---

## 🟢 Simple Example — With `useCallback`

```jsx
function Parent() {
  const [count, setCount] = useState(0);

  const increment = useCallback(() => {
    setCount(c => c + 1);
  }, []);

  return <Child onIncrement={increment} />;
}
```

✔ `increment` function reference stable గా ఉంటుంది (dependencies లేకపోవడంతో ఒకసారి మాత్రమే create అవుతుంది)
✔ `Child` component unnecessary గా re-render కాదు (assuming `React.memo` వాడితే)

---

## 🔥 Medium Example — Memoized Child Component

```jsx
const Child = React.memo(({ onIncrement }) => {
  console.log("Child rendered");
  return <button onClick={onIncrement}>Increment</button>;
});
```

`React.memo` + `useCallback` కలిపి వాడితే:

* Parent re-render అయినా
* `onIncrement` reference మారకపోతే
* `Child` re-render కాదు ✅

---

## 🧩 Advanced Example — Two Independent Buttons

### ❌ Without `useCallback`

```jsx
const Button = React.memo(({ onClick, text }) => {
  console.log(`Child ${text} button rendered`);
  return <button onClick={onClick}>{text}</button>;
});

function WithoutCallbackExample() {
  const [count1, setCount1] = useState(0);
  const [count2, setCount2] = useState(0);

  const handleClick1 = () => setCount1(count1 + 1);
  const handleClick2 = () => setCount2(count2 + 1);

  console.log("Parent rendered");

  return (
    <div>
      <h2>Without useCallback:</h2>
      <p>Count 1: {count1}</p>
      <p>Count 2: {count2}</p>
      <Button onClick={handleClick1} text="Button 1" />
      <Button onClick={handleClick2} text="Button 2" />
    </div>
  );
}
```

ఈ setup లో ప్రతి render కి `handleClick1`, `handleClick2` కొత్త references అవ్వడంతో రెండు buttons కూడా re-render అవుతాయి.

---

### ✅ With `useCallback`

```jsx
const Button = React.memo(({ onClick, text }) => {
  console.log(`${text} button rendered`);
  return <button onClick={onClick}>{text}</button>;
});

function WithCallbackExample() {
  const [count1, setCount1] = useState(0);
  const [count2, setCount2] = useState(0);

  const handleClick1 = useCallback(() => setCount1(c => c + 1), []);
  const handleClick2 = useCallback(() => setCount2(c => c + 1), []);

  console.log("Parent rendered");

  return (
    <div>
      <h2>With useCallback:</h2>
      <p>Count 1: {count1}</p>
      <p>Count 2: {count2}</p>
      <Button onClick={handleClick1} text="Button 1" />
      <Button onClick={handleClick2} text="Button 2" />
    </div>
  );
}
```

✔ Button1 click చేసినప్పుడు Button2 re-render కాదు (మరియు vice-versa), ఎందుకంటే functions references stable గా ఉంటాయి.

> గమనిక: dependencies array ని సరైన విధంగా ఇవ్వడం చాలా ముఖ్యం. లేకపోతే stale state సమస్యలు వస్తాయి.

---

## 🏗 Real-Time Example — Expensive Event Handler

```jsx
function Search({ onSearch }) {
  return <input onChange={(e) => onSearch(e.target.value)} placeholder="Search..." />;
}

function App() {
  const [query, setQuery] = useState('');

  const handleSearch = useCallback((value) => {
    setQuery(value);
    console.log("Heavy filtering with:", value);
  }, []);

  return (
    <div>
      <Search onSearch={handleSearch} />
      <p>Query: {query}</p>
    </div>
  );
}
```

✔ `handleSearch` ప్రతి render కి recreate కాదు → Large lists filter చేస్తున్నప్పుడు performance మెరుగుపడుతుంది.

---

## 🎯 When to Use `useCallback`

`useCallback` వాడాల్సిన సరైన సందర్భాలు:

* Child component `React.memo` తో memoized అయి ఉండి, **function props** వల్ల re-render అవుతున్నప్పుడు
* Function లో **expensive logic** ఉన్నప్పుడు
* Event handlers ని చాలా components కి pass చేస్తున్నప్పుడు
* Lists / tables లో per-row handlers pass చేస్తున్నప్పుడు

---

## ❗ Mistakes to Avoid

❌ **Overuse**: ప్రతి function మీద `useCallback` పెట్టడం → clutter + overhead
❌ Dependencies array లో values మర్చిపోవడం → stale values / bugs
❌ `useCallback` వాడి కూడా `React.memo` వాడకపోవడం (child ఇంకా re-render అవుతుంది)
❌ Profile చేయకుండా ముందే "performance కోసం" everywhere వాడడం

---

## ⚡ Best Practices

✔ Function ను child కి prop గా పంపుతుంటే, child memoized అయితే `useCallback` గురించి ఆలోచించండి
✔ `dependencies` array లో ఉపయోగిస్తున్న state/props అన్నీ add చేయాలి
✔ Expensive computations కోసం `useMemo` + handler కోసం `useCallback` కలిపి వాడండి
✔ Dependencies simple గా ఉంచండి; nested objects avoid చేయండి

---

## 🔧 Tricks

### 🔹 Stable Callback for Event Listeners

```jsx
const onScroll = useCallback(() => {
  console.log("scrolling...");
}, []);
```

### 🔹 Lists లో Item Click Handler

```jsx
const onItemClick = useCallback((id) => {
  console.log("Clicked", id);
}, []);
```

### 🔹 useMemo + useCallback Combo

```jsx
const filteredData = useMemo(() => expensiveFilter(data), [data]);
const handleFilter = useCallback(() => {
  console.log(filteredData);
}, [filteredData]);
```

---

## 📝 Summary

* `useCallback` → **functions** ని memoize చేస్తుంది
* Dependencies మారే వరకు function reference మారదు
* `React.memo` + `useCallback` కలిపి వాడితే child re-renders తగ్గుతాయి
* తప్పుగా వాడితే performance కి minus అవుతుంది, plus కాదు

---

## 🧾 `useCallback` vs `useMemo` vs `React.memo`

| Feature         | `useCallback`               | `useMemo`                      | `React.memo`                                 |
| --------------- | --------------------------- | ------------------------------ | -------------------------------------------- |
| Type            | Hook                        | Hook                           | Higher-Order Component (HOC)                 |
| Memoizes        | **Function**                | **Value / Computation result** | **Component rendering output**               |
| Return value    | Stable function reference   | Cached value                   | Memoized component                           |
| Use case        | Functions props to children | Expensive calculations         | Child re-render optimization                 |
| Works best with | `React.memo`, `useMemo`     | `useCallback`                  | `useCallback` (for function props stability) |

---

# 🎤 Interview Questions & Answers

### 🟢 1. `useCallback` అంటే ఏమిటి?

**Answer:**
`useCallback` అనేది ఒక hook; ఇది ఇచ్చిన callback function కి **memoized version** return చేస్తుంది. Dependencies change అయ్యేప్పుడు మాత్రమే కొత్త function create అవుతుంది.

---

### 🟢 2. ఎప్పుడు `useCallback` వాడాలి?

* Function ని memoized child component కి prop గా పంపితే
* Unnecessary re-renders తగ్గించాలి అనుకుంటే
* Function recreate కావడం వల్ల performance issues వస్తే

---

### 🟡 3. `useCallback` మరియు `useMemo` మధ్య తేడా?

* `useCallback` → **function** memoize చేస్తుంది
* `useMemo` → **value / result** memoize చేస్తుంది

---

### 🔥 4. Dependencies తప్పుగా ఇస్తే ఏమవుతుంది?

Dependencies మిస్ అయితే → **stale closures** వస్తాయి; function పాత values తో పనిచేస్తుంది.

---

### 🔥 5. `useCallback` ని ఎందుకు overuse చేయకూడదు?

Memoization కూడా ఒక cost — memory + comparison.
చాలా చోట్ల వాడితే complexity పెరిగి, performance కూడా తగ్గొచ్చు.

---

మీకు కావాలంటే:

* `useCallback` behavior ని explain చేసే diagram
* Small practice questions
* Real interview coding task కూడా జతచేసి ఇస్తాను.
