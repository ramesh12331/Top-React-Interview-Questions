# ⚛️ 15. React లో `useCallback` Hook అంటే ఏమిటి? ఎప్పుడు వాడాలి?

## 🧠 సులభమైన నిర్వచనం (Simple Definition)

React లో **`useCallback` hook** ను **functions ను memoize చేయడానికి** వాడతారు — అంటే React ఆ function ను **re-render మధ్య reuse చేస్తుంది**, dependencies మారే వరకు.

దీని వల్ల **performance మెరుగవుతుంది** మరియు **అవసరం లేని child component re-renders నివారించవచ్చు**.

---

## ⚙️ Syntax

```jsx
const memoizedFunction = useCallback(() => {
  // function logic
}, [dependencies]);
```

### Parameters

* **Callback Function** → మీరు memoize చేయాలనుకునే function.  
* **Dependency Array** → function కొత్తగా సృష్టించాల్సిన సమయాన్ని నిర్ణయిస్తుంది.

✅ Dependencies మారకపోతే → React అదే function reference ను వాడుతుంది.

---

## 🧩 Example — Child Re-render నివారించడం

```jsx
import React, { useState, useCallback } from 'react';

function Parent() {
  const [count, setCount] = useState(0);

  const increment = useCallback(() => {
    setCount(c => c + 1);
  }, []);

  return <Child onIncrement={increment} />;
}

function Child({ onIncrement }) {
  console.log('👶 Child rendered');
  return <button onClick={onIncrement}>Increment</button>;
}

export default Parent;
```

### 🧠 వివరణ

* React ప్రతి render లో కొత్త function object ను సృష్టిస్తుంది.  
* అందువల్ల `increment` కొత్తగా సృష్టించబడితే, Child component re-render అవుతుంది.  
* `useCallback` వాడటం వల్ల React అదే function reference ను వాడుతుంది (dependencies మారకపోతే).  

✅ Child re-render అవ్వదు, performance మెరుగవుతుంది.

---

## 🧩 Example — Dependencies ఉన్నప్పుడు

```jsx
const fetchData = useCallback(() => {
  console.log('Fetching data for user:', userId);
}, [userId]);
```

✅ `userId` మారినప్పుడు మాత్రమే function కొత్తగా సృష్టించబడుతుంది.

---

## ⚡ ఎప్పుడు `useCallback` వాడాలి?

✔ Child components కి **functions props** గా పంపినప్పుడు  
✔ **`React.memo()`** వాడుతున్నప్పుడు  
✔ **Event handlers** లేదా **complex calculations** ఉన్నప్పుడు  
✔ Function re-creation వల్ల **performance drop** అవుతున్నప్పుడు

---

## 🚫 Common Mistakes

| ❌ తప్పు | ⚠️ కారణం |
| -------- | -------- |
| ప్రతి చోటా `useCallback` వాడటం | అవసరం లేని memory usage పెరుగుతుంది |
| Dependencies ఇవ్వకపోవడం | పాత data (stale values) వస్తాయి |
| Over-optimization | Rendering slow అవుతుంది |

---

## ✅ Best Practices

✔ `React.memo` వాడుతున్నప్పుడు మాత్రమే `useCallback` వాడండి  
✔ అన్ని dependencies ను array లో చేర్చండి  
✔ అవసరం లేని చోట వాడకండి  
✔ `useMemo` తో కలిపి వాడితే performance ఇంకా మెరుగవుతుంది

---

## 🎨 Visual Diagram — `useCallback` ఎలా పనిచేస్తుంది

```
Render 1 → Function F1 సృష్టించబడింది  
Render 2 (deps మారలేదు) → అదే F1 వాడబడింది ✅  
Render 3 (deps మారాయి) → Function F2 సృష్టించబడింది ⚡
```

✅ Dependencies మారకపోతే → Function re-use అవుతుంది  
⚡ మారితే → కొత్త function సృష్టించబడుతుంది

---

## 💡 గుర్తుంచుకోవాల్సిన Trick

> 🧩 "`useCallback` → Function memory save చేస్తుంది, dependencies మారినప్పుడు మాత్రమే update అవుతుంది."

| Hook | Memoize చేసే విషయం | ఉపయోగం |
| ----- | ---------------- | -------- |
| `useCallback` | Function reference | Child re-render నివారించడానికి |
| `useMemo` | Computed value | Expensive calculations cache చేయడానికి |

---

## 💬 Interview Questions

**Q1:** Child component ఎక్కువ సార్లు re-render అవుతోంది, ఎలా తగ్గించాలి?  
👉 `useCallback` + `React.memo` వాడాలి.

**Q2:** Functions re-render లో కొత్తగా ఎందుకు సృష్టించబడతాయి?  
👉 JavaScript లో ప్రతి render కి కొత్త function object సృష్టించబడుతుంది.

**Q3:** `useCallback` vs `useMemo` తేడా ఏమిటి?  
👉 `useCallback` → function ను memoize చేస్తుంది.  
👉 `useMemo` → function యొక్క result ను memoize చేస్తుంది.

---

## 🧾 Short Interview Summary

> “`useCallback` ఒక function ను memoize చేస్తుంది, అంటే అది ప్రతి render లో కొత్తగా సృష్టించబడదు. Dependencies మారినప్పుడు మాత్రమే కొత్త function వస్తుంది. ఇది unnecessary child re-renders నివారించడంలో సహాయపడుతుంది.”

---

## ⚡ One-Line Trick

> 🧠 “`useCallback` = Memoized Function — Dependencies మారితే మాత్రమే కొత్తదిగా అవుతుంది.”

---

## ⚖️ `useCallback` vs `useMemo` vs `React.memo`

| Feature | `useCallback` | `useMemo` | `React.memo` |
| -------- | -------------- | ---------- | ------------- |
| Memoizes | Function | Computed Value | Component Output |
| Re-run Condition | Dependencies మారినప్పుడు | Dependencies మారినప్పుడు | Props మారినప్పుడు |
| Purpose | Function re-use | Value cache | Component re-render నివారణ |
| Returns | Function | Value | Component |
| Used For | Event Handlers | Filtered / Derived Data | Functional Components |

---

## 🎨 Visual Flow Diagram

```
Parent Re-render
│
├── Without useCallback → New function → Child re-render ⚠️
│
├── With useCallback → Same function → Child skip ✅
│
├── With useMemo → Cached value ✅
│
└── With React.memo → Props మారినప్పుడే re-render ✅
```

---

## 🧩 Example — `useCallback` + `useMemo` కలిపి వాడటం

```jsx
import React, { useState, useCallback, useMemo } from 'react';

function ProductList({ products }) {
  const [filter, setFilter] = useState('');

  const filterProducts = useCallback(() => {
    return products.filter((p) => p.name.toLowerCase().includes(filter.toLowerCase()));
  }, [products, filter]);

  const filteredList = useMemo(() => filterProducts(), [filterProducts]);

  return (
    <div>
      <input
        type="text"
        placeholder="Search product"
        value={filter}
        onChange={(e) => setFilter(e.target.value)}
      />
      <ul>
        {filteredList.map((product) => (
          <li key={product.id}>{product.name}</li>
        ))}
      </ul>
    </div>
  );
}

export default ProductList;
```

### 🧠 వివరణ

* `useCallback()` function reference ను స్థిరంగా ఉంచుతుంది.  
* `useMemo()` filtered result ను cache చేస్తుంది.  

✅ **ఫలితం:** Efficient rendering మరియు unnecessary re-renders నివారణ.

---

## 💡 Final Tip

> “`useCallback` → Function Memoize 🔁 | `useMemo` → Value Memoize 💾 | `React.memo` → Component Memoize 🧩”

---

**Author:** *Mamidi Ramesh*  
**Topic:** React Hooks — `useCallback` Hook  
**Category:** Frontend / React.js  
