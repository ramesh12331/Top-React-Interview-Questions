# 📘 React `useEffect` Hook — Full Telugu Guide

> Definition, syntax, simple → advanced examples, real‑time use cases, cleanup logic, mistakes, best practices, summary, and interview Q&A తో పూర్తి ఇంటర్వ్యూ రెడీ గైడ్.

---

## ⭐ Introduction

React లో **`useEffect`** Hook ని functional components లో **side effects** execute చేయడానికి ఉపయోగిస్తారు.

Side Effects అంటే:

* API calls (data fetching)
* DOM manipulation
* Timers / intervals
* Event listeners
* Subscriptions

ఈ Hook, class components లోని lifecycle methods ని replace చేస్తుంది:

* `componentDidMount`
* `componentDidUpdate`
* `componentWillUnmount`

---

## 📌 Definition

`useEffect` అంటే component render అయిన తర్వాత execute అయ్యే function.

```jsx
useEffect(() => {
  // side effect code
}, [dependencies]);
```

**Dependency array** effect ఎప్పుడు run కావాలో నిర్ణయిస్తుంది.

---

## 🧠 Syntax

```jsx
useEffect(callbackFunction, dependencyArray);
```

**callbackFunction** → ప్రతి render తర్వాత run అవుతుంది.
**dependencyArray** → మార్చినప్పుడు మాత్రమే effect తిరిగి run అవుతుంది.

---

## 🟢 Example 1 — Runs on *Every Render*

```jsx
useEffect(() => {
  console.log("Runs every render");
});
```

---

## 🟢 Example 2 — Runs Only on First Render

```jsx
useEffect(() => {
  console.log("Runs only on mount");
}, []);
```

---

## 🟢 Example 3 — Runs When Dependency Changes

```jsx
useEffect(() => {
  console.log("count changed");
}, [count]);
```

---

# 🔥 Incorrect Timer Example — Causes Bug

```jsx
useEffect(() => {
  setTimeout(() => setCount(count + 1), 1000);
});
```

❌ ప్రతి render → కొత్త timeout → infinite increments.

---

# ✅ Correct Timer Example

```jsx
useEffect(() => {
  const timer = setTimeout(() => setCount(c => c + 1), 1000);
  return () => clearTimeout(timer);
}, []);
```

✔ mount అయినప్పుడు ఒక్కసారి మాత్రమే run అవుతుంది.

---

# 🧩 Dependency Example

```jsx
useEffect(() => {
  setCalculation(count * 2);
}, [count]);
```

✔ `count` మారితే మాత్రమే effect run అవుతుంది.

---

# 🧼 Cleanup Function — Memory Leaks నివారించడానికి

Cleanup mostly ఉపయోగించేది:

* Timers
* Subscriptions
* Event listeners
* Intervals

```jsx
useEffect(() => {
  const timer = setInterval(() => console.log("tick"), 1000);
  return () => clearInterval(timer);
}, []);
```

✔ Component unmount సమయంలో timer clear అవుతుంది.

---

# 🏗 Real‑World Examples

## 1️⃣ Fetch API Example

```jsx
useEffect(() => {
  async function getData() {
    const res = await fetch('/api/users');
    const data = await res.json();
    setUsers(data);
  }
  getData();
}, []);
```

---

## 2️⃣ Window Resize Listener

```jsx
useEffect(() => {
  const handleResize = () => console.log(window.innerWidth);
  window.addEventListener('resize', handleResize);

  return () => window.removeEventListener('resize', handleResize);
}, []);
```

---

## 3️⃣ Authentication Listener

```jsx
useEffect(() => {
  const unsub = auth.onAuthStateChanged(user => setUser(user));
  return () => unsub();
}, []);
```

---

# 🎯 When to Use `useEffect`

* ✔ Data fetch చేయాలి
* ✔ props/state change కి react కావాలి
* ✔ Timers/intervals setup చేయాలి
* ✔ Event listeners attach చేయాలి
* ✔ DOM operations చేయాలి

---

# ❗ Mistakes to Avoid

❌ Dependencies మిస్ కావడం → stale values
❌ Every variable ని dependencies లో పెట్టడం → infinite loops
❌ State ని effect లో unnecessary గా update చేయడం
❌ Cleanup మర్చిపోవడం → memory leaks

---

# ⚡ Best Practices

✔ One‑time operations కి `[]` వాడండి
✔ Required dependencies మాత్రమే ఇవ్వండి
✔ Cleanup ని ఎప్పుడూ handle చేయండి
✔ Heavy logic ని effects లో avoid చేయండి
✔ Reusable side effects కోసం custom hooks వాడండి

---

# 🔧 Tricks

### 🔹 Multiple Effects (clean code)

```jsx
useEffect(() => loadUser(), []);
useEffect(() => saveForm(), [form]);
```

### 🔹 Custom Hook Example

```jsx
function useDocumentTitle(title) {
  useEffect(() => {
    document.title = title;
  }, [title]);
}
```

### 🔹 Multiple Dependencies

```jsx
useEffect(() => {
  console.log("state1 or state2 changed");
}, [state1, state2]);
```

---

# 🏗 React Class Lifecycle Equivalents

## 🔹 `componentDidMount` → Runs once after mount

```jsx
useEffect(() => {
  console.log("Mounted");
}, []);
```

---

## 🔹 `componentDidUpdate` → Runs after update

```jsx
useEffect(() => {
  console.log("Updated");
}, [count]);
```

---

## 🔹 `componentWillUnmount` → Cleanup

```jsx
useEffect(() => {
  const timer = setInterval(() => console.log('tick'), 1000);
  return () => clearInterval(timer);
}, []);
```

---

# 📝 Summary

* `useEffect` side effects execute చేయడానికి వాడతారు
* dependency array effect ఎప్పుడు run అవాలో control చేస్తుంది
* cleanup memory leaks నివారిస్తుంది
* API calls, timers, DOM work, listeners లలో చాలా ఉపయోగపడుతుంది

---

# 🎤 Interview Questions & Answers

### 🟢 1. `useEffect` అంటే ఏమిటి?

React లో side effects run చేసే Hook.

### 🟢 2. useEffect ఎప్పుడు run అవుతుంది?

* every render (dependencies లేకుండా)
* once on mount (`[]`)
* dependencies change అయితే

### 🟡 3. Cleanup అంటే ఏమిటి?

Return function ద్వారా timers/listeners/subscriptions remove చేయడం.

### 🔥 4. React Strict Mode లో useEffect రెండుసార్లు ఎందుకు run అవుతుంది?

Development లో bugs detect చేయడానికి React double invoke చేస్తుంది.

### 🔥 5. Infinite loops ఎప్పుడు వస్తాయి?

Effect లో state update చేసి, అదే variable ని dependency లో పెట్టినప్పుడు.

---

ఇంకా కావాలంటే: diagrams, debouncing/throttling examples, or export as PDF కూడా తయారు చేస్తాను!
