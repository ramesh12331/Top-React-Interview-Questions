## 16. React లో **useMemo** హుక్ అంటే ఏమిటి? ఎప్పుడు ఉపయోగించాలి?

# 📘 React useMemo Hook — పూర్తి గైడ్ (తెలుగు)

## ⭐ Introduction
React లో **useMemo** హుక్‌ను ఒక computation (భారీ లెక్కింపు) ఫలితాన్ని **memoize (cache)** చేయడానికి ఉపయోగిస్తారు.  
దీంతో ప్రతి render లో computation మళ్లీ జరగదు — dependencies మారినప్పుడు మాత్రమే లెక్కింపును చేస్తుంది.

ఇది UI పనితీరును (performance) గణనీయంగా మెరుగుపరుస్తుంది.

---

## 🔍 What is useMemo?
`useMemo` ఒక memoized value ను return చేసే React Hook.

🧠 సింపుల్‌గా అర్థం చేసుకుంటే:  
“React unnecessary గా మళ్లీ లెక్కించకుండా, ముందున్న ఫలితాన్ని గుర్తుంచుకోవడం.”

---

## 🧠 Syntax
```js
const memoizedValue = useMemo(() => {
  // expensive computation
  return result;
}, [dependencies]);
```

👉 dependencies మారితే మాత్రమే React computation మళ్లీ చేస్తుంది.

---

## 🎯 Why use useMemo?
✔ భారీ computations నివారించడానికి  
✔ UI performance పెంచడానికి  
✔ child components unnecessary గా re-render కాకుండా నిలిపేందుకు  
✔ derived values ని memoize చేయడానికి  

---

## 🟢 సింపుల్ Example
```jsx
import { useState, useMemo } from "react";

function App() {
  const [count, setCount] = useState(0);

  const double = useMemo(() => {
    console.log("Calculating...");
    return count * 2;
  }, [count]);

  return (
    <div>
      <h1>Double: {double}</h1>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

📌 `count` మారినప్పుడు మాత్రమే `double` మళ్లీ లెక్కించబడుతుంది.

---

## 🔥 Real-Time Example — Expensive Function

### ❌ useMemo లేకుండా
```js
const calculation = expensiveCalculation(count);
```
➡ ప్రతి render లో run అవుతుంది.

### ✅ useMemo తో
```js
const calculation = useMemo(() => expensiveCalculation(count), [count]);
```
➡ `count` మారినప్పుడు మాత్రమే run అవుతుంది.

---

## 🧩 Heavy Example (Filtering Large Dataset)
```jsx
const filteredUsers = useMemo(() => {
  console.log("Filtering...");
  return users.filter(user => user.age > 25);
}, [users]);
```

➡ ప్రతిసారి filter అవ్వకుండా, అవసరం ఉన్నప్పుడు మాత్రమే అవుతుంది.

---

## 🏗 Child Re-renders ను నివారించడం
```jsx
const sortedList = useMemo(() => {
  return items.sort((a, b) => a - b);
}, [items]);

return <Child list={sortedList} />;
```

➡ Child component కు stable reference దొరుకుతుంది → re-render తగ్గుతుంది.

---

## 🆚 useMemo vs useCallback

| Hook | Returns | Used For |
|------|---------|----------|
| **useMemo** | memoized **value** | లెక్కించిన ఫలితాలు cache చేయడానికి |
| **useCallback** | memoized **function** | functions re-create కాకుండా నిలిపేందుకు |

---

## ❗ Common Mistakes

❌ useMemo ను ఎక్కడపడితే అక్కడ ఉపయోగించడం  
❌ dependency array తప్పుగా ఇవ్వడం  
❌ performance ఖచ్చితంగా మెరుగుపడుతుందని అనుకోవడం  

---

## 📌 useMemo ఎప్పుడు వాడాలి?

✔ Computation చాలా heavy గా ఉన్నప్పుడు  
✔ repeated calculations ఉన్నప్పుడు  
✔ పెద్ద lists / tables ఉన్నప్పుడు  
✔ objects/arrays ని children కు pass చేస్తున్నప్పుడు  
✔ unnecessary re-renders తగ్గించాల్సినప్పుడు  

---

## 🎯 Best Practices

- Actual performance issue ఉన్నప్పుడు మాత్రమే useMemo వాడాలి  
- dependency array సరిగ్గా ఇవ్వాలి  
- React.memo + useCallback తో వాడితే ఇంకా మంచి ఫలితం  
- ముందు profiling చేసి bottleneck ఏదో చూసి optimize చేయాలి  

---

## ⚡ Tricks

### 🔹 Objects/Arrays Memoize చేయడం
```js
const options = useMemo(() => ({ theme: "dark" }), []);
```

### 🔹 Filtered Data Memoize చేయడం
```js
const activeUsers = useMemo(() => users.filter(u => u.active), [users]);
```

### 🔹 Dashboards / Big Data Tables లో useMemo తప్పనిసరి  

---

## 📝 Summary

- useMemo unnecessary calculations ను నివారిస్తుంది  
- పెద్ద components & పెద్ద datasets లో performance improve చేస్తుంది  
- కానీ అదుపు లేకుండా వాడితే overhead పెరుగుతుంది  

---

# 🎤 Interview Questions & Answers (useMemo) — Telugu Version

---

## 📌 Basic Level

### **1. useMemo అంటే ఏమిటి?**  
ఇది computation ఫలితాన్ని memoize చేసేది. dependency మారేవరకు మళ్లీ run అవదు.

---

### **2. Memoization అంటే ఏమిటి?**  
భారీ లెక్కింపు ఫలితాన్ని cache చేసి, అదే inputs వస్తే cached result ఉపయోగించడం.

---

### **3. Dependency array role ఏమిటి?**  
ఏప్పుడు మళ్లీ లెక్కించాలో React కు చెబుతుంది.

---

## 📌 Intermediate Level

### **4. useMemo ఎప్పుడు వాడాలి?**  
Expensive calculations, filtering/sorting వంటి పనులలో.

---

### **5. Empty dependency array ఇచ్చినప్పుడు ఏమవుతుంది?**  
కేవలం ఒకసారి run అవుతుంది — మళ్లీ run కాదు.

---

### **6. useMemo ప్రతి render లో run అవుతుందా?**  
కాదు. Dependencies మారినప్పుడు మాత్రమే.

---

### **7. useMemo vs useCallback?**  
- useMemo → value ను memoize చేస్తుంది  
- useCallback → function ను memoize చేస్తుంది  

---

## 📌 Advanced Level

### **8. useMemo ఎక్కువగా వాడితే ఎందుకు తప్పు?**  
Memoization కు కూడా cost ఉంటుంది — CPU + memory → overhead పెరుగుతుంది.

---

### **9. useMemo performance హామీ ఇస్తుందా?**  
కాదు. Bottleneck ఎక్కడుందో ముందు profile చేయాలి.

---

### **10. useMemo ఎలా child re-renders నివారిస్తుంది?**  
Stable references ఇస్తుంది → React.memo unnecessary renders నిలిపేస్తుంది.

---

### **11. Filtering/Sorting optimize అవుతుందా?**  
అవును — ఇది useMemo యొక్క ప్రధాన use-case.

---

### **12. API caching కోసం useMemo వాడాలా?**  
కాదు. దానికి React Query / SWR వాడాలి.

---

### **13. Component unmount తర్వాత memoized values ఉంటాయా?**  
కాదు. Component lifecycle తో ముగుస్తాయి.

---

### **14. React useMemo ను internally ఎలా optimize చేస్తుంది?**  
Old dependencies & results ను fiber tree లో store చేస్తుంది → మార్పు లేనప్పుడు cached value ఇస్తుంది.

---

## 🎉 End of README (Telugu Version)
