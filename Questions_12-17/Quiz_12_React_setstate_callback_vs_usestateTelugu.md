# ⚛️ 12. Purpose of the Callback Function Format of `setState()` in React

## 🧠 సులభమైన నిర్వచనం (Simple Definition)

React **class components** లో `setState()` updates **asynchronous** మరియు **batched** గా జరుగుతాయి — అంటే React performance కోసం చాలా updates ను ఒకేసారి process చేస్తుంది.

👉 కాబట్టి, **కొత్త state పాత state మీద ఆధారపడి ఉంటే**, `this.state` ని నేరుగా ఉపయోగించడం వల్ల **పాత లేదా తప్పు values** రావచ్చు.

✅ దీన్ని నివారించడానికి React ఒక **callback function format** ను అందిస్తుంది.  
ఇది తాజా (`latest`) `state` మరియు `props` ను access చేసేలా సహాయపడుతుంది — తద్వారా updates ఎప్పుడూ సరైనవిగా ఉంటాయి.

---

## ⚙️ Syntax

```jsx
this.setState((prevState, props) => {
  return {
    key: updatedValueBasedOn(prevState.key)
  };
});
```

### Parameters:

- **`prevState`** → పాత (తాజా) state update అవ్వడానికి ముందు ఉన్న విలువ  
- **`props`** → ప్రస్తుతం ఉన్న props values  

---

## 🧩 Example

### ❌ తప్పు విధానం — `this.state` నేరుగా ఉపయోగించడం

```jsx
this.setState({
  count: this.state.count + 1
});
```

ఇది వేగంగా చాలాసార్లు కాల్ చేసినప్పుడు React batching వల్ల ఒక్క increment మాత్రమే పని చేయవచ్చు.

---

### ✅ సరైన విధానం — Callback Function ఉపయోగించడం

```jsx
this.setState((prevState) => ({
  count: prevState.count + 1
}));
```

ఇప్పుడు React ఎప్పుడూ **తాజా state** ని ఉపయోగిస్తుంది — కాబట్టి values సరైనవి అవుతాయి.

---

### ✅ Complete Example

```jsx
class Counter extends React.Component {
  state = { count: 0 };

  increment = () => {
    this.setState((prevState) => ({
      count: prevState.count + 1
    }));
  };

  render() {
    return (
      <div>
        <p>Count: {this.state.count}</p>
        <button onClick={this.increment}>Increment</button>
      </div>
    );
  }
}
```

✅ ఈ కోడ్‌లో మీరు వేగంగా క్లిక్ చేసినా ప్రతి సారి **సరైన కౌంట్** పెరుగుతుంది.

---

## ⚡ ఎప్పుడు Callback Format ఉపయోగించాలి?

కొత్త state పాత state లేదా props మీద ఆధారపడినప్పుడు తప్పనిసరిగా ఉపయోగించాలి.

ఉదాహరణలు:

- Counters (`count + 1`)
- Toggles (`!isOpen`)
- Accumulative calculations
- Async లేదా వేగంగా జరిగే event-based updates

---

## 🚫 Common Mistakes

| ❌ Mistake | ⚠️ ఎందుకు తప్పు |
| ----------- | ---------------- |
| `this.state` ని నేరుగా ఉపయోగించడం | పాత state ని ఉపయోగించే అవకాశం ఉంది |
| `setState` వెంటనే update అవుతుందని అనుకోవడం | ఇది asynchronous |
| callback లో side effects చేయడం | function pure గా ఉండదు |

---

## ✅ Best Practices

- పాత state మీద ఆధారపడే update లకు ఎప్పుడూ callback form ఉపయోగించాలి.  
- callback function ను **pure** గా ఉంచాలి.  
- React batching వల్ల updates తక్షణం జరగవు — ఇది సాధారణం.

---

## ⚖️ Comparison — `setState()` vs `useState()` (Functional Update)

| Feature | `setState()` (Class Component) | `useState()` (Functional Component) |
| -------- | ------------------------------ | ----------------------------------- |
| **Syntax** | `this.setState((prevState, props) => newState)` | `setState(prev => newValue)` |
| **Receives** | `prevState`, `props` | `previous state only` |
| **Used In** | Class Components | Functional Components |
| **Async Behavior** | Yes (batched updates) | Yes (batched updates) |
| **When to Use** | కొత్త state పాత state లేదా props మీద ఆధారపడితే | పాత state మీద ఆధారపడితే |
| **Example** | `this.setState(prev => ({ count: prev.count + 1 }))` | `setCount(prev => prev + 1)` |

---

## 🧩 Example with `useState()`

```jsx
import React, { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  const increment = () => {
    // ✅ Functional update ensures latest value
    setCount(prev => prev + 1);
  };

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
    </div>
  );
}

export default Counter;
```

✅ ఇది కూడా class component లోని `setState((prevState) => …)` లాగానే పనిచేస్తుంది.

---

## 💬 Interview Scenarios

**🗣 Q1:** Callback version ఎందుకు ఉపయోగిస్తారు?  
👉 React asynchronous updates వల్ల పాత state రావొచ్చు. Callback format వాడితే ఎల్లప్పుడూ తాజా state వాడుతుంది.

**🗣 Q2:** Functional component లో equivalent ఏమిటి?  
👉 `useState()` లో functional update form — `setCount(prev => prev + 1)`.

**🗣 Q3:** ఎప్పుడు వాడాలి?  
👉 కొత్త state పాత state మీద ఆధారపడినప్పుడు (ఉదా: counters, toggles).

---

## 🧾 Short Interview Summary

> “`setState()` యొక్క callback format వల్ల React ఎప్పుడూ తాజా state మరియు props ను ఉపయోగిస్తుంది. ఇది batching వల్ల వచ్చే stale values ను నివారిస్తుంది.  
> Functional components లో అదే concept `useState()` యొక్క functional update రూపంలో ఉంటుంది.”

---

## ⚡ One-Line Trick to Remember

> 🧩 **కొత్త state పాత state మీద ఆధారపడితే — ఎప్పుడూ callback form వాడాలి!**  
> `setState(prev => newValue)` లేదా `setCount(prev => prev + 1)`

---
