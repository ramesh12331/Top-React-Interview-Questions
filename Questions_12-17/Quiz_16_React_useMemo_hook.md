## 16. What is the **useMemo** hook in React and when should it be used?

# 📘 React useMemo Hook — Full Guide

## ⭐ Introduction
The React **useMemo** Hook is used to memoize (cache) the result of a computation so that it does **NOT** run again on every render unless its dependencies change.  
It helps avoid unnecessary recalculations and boosts performance for heavy computations.

---

## 🔍 What is useMemo?
`useMemo` is a React Hook that returns a memoized value.

🧠 Think of it as:  
“Remembering the output of an expensive computation so React doesn't redo the work.”

---

## 🧠 Syntax
```js
const memoizedValue = useMemo(() => {
  // expensive computation
  return result;
}, [dependencies]);
```

React recalculates only when a dependency changes.  
If dependencies do **not** change → React returns the **cached value**.

---

## 🎯 Why use useMemo?
✔ Prevent expensive calculations  
✔ Improve UI performance  
✔ Avoid unnecessary child component re-renders  
✔ Memoize derived state (computed values)  

---

## 🟢 Simple Example
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

📌 Only recalculates `double` when **count** changes.

---

## 🔥 Real-Time Example — Expensive Function

### ❌ Without useMemo
```js
const calculation = expensiveCalculation(count);
```

➡ Runs on **every render**, even if not needed.

### ✅ With useMemo
```js
const calculation = useMemo(() => expensiveCalculation(count), [count]);
```

➡ Runs **only when count changes**.

---

## 🧩 Real-Time Heavy Example (Filtering Large Dataset)
```jsx
const filteredUsers = useMemo(() => {
  console.log("Filtering...");
  return users.filter(user => user.age > 25);
}, [users]);
```

➡ Prevents unnecessary filtering on every keystroke.

---

## 🏗 Advanced Example — Preventing Child Re-renders
```jsx
const sortedList = useMemo(() => {
  return items.sort((a, b) => a - b);
}, [items]);

return <Child list={sortedList} />;
```

➡ Prevents unnecessary re-render of `<Child />`.

---

## 🆚 useMemo vs useCallback

| Hook | Returns | Used For |
|------|---------|----------|
| **useMemo** | memoized value | caching computed results |
| **useCallback** | memoized function | preventing function recreation |

---

## ❗ Common Mistakes

❌ Overusing useMemo → adds overhead  
❌ Incorrect dependency arrays → stale values  
❌ Assuming it always improves performance  

---

## 📌 When to Use useMemo

Use it when your computation is:

✔ Heavy CPU work  
✔ Runs frequently  
✔ Derived from stable inputs  
✔ Used in large lists/tables  
✔ Passed to children to avoid re-renders  

---

## 🎯 Best Practices

- Use useMemo **only** for expensive logic  
- Always define correct dependencies  
- Profile performance first  
- Use with `React.memo` + `useCallback` for optimization  

---

## ⚡ Tricks

### 🔹 Memoize objects/arrays
```js
const options = useMemo(() => ({ theme: "dark" }), []);
```

### 🔹 Memoize filtered data
```js
const activeUsers = useMemo(() => users.filter(u => u.active), [users]);
```

### 🔹 Improve dashboards/tables
Perfect for sorting + filtering huge data.

---

## 📝 Summary

- `useMemo` prevents unnecessary recalculation  
- Best for expensive computations  
- Helps optimize large components  
- Avoid overuse  

---

## 🧑‍💻 Interview‑Ready Explanation
> "useMemo memoizes the result of a function so it only recomputes when its dependencies change. It improves performance by preventing expensive recalculations in large or complex components."

---

# 🎤 React useMemo — Interview Questions & Answers

---

## 📌 Basic Level

### **1. What is useMemo in React?**  
**Answer:**  
useMemo memoizes a value so that expensive computations don't re-run on every render.

---

### **2. What does memoization mean?**  
**Answer:**  
Memoization caches the results of expensive operations so future calls with the same input return cached output.

---

### **3. What does the dependency array do?**  
**Answer:**  
It tells React when to recompute the memoized value.

---

## 📌 Intermediate Level

### **4. When should you use useMemo?**  
**Answer:**  
When the computation is expensive, such as sorting/filtering large lists or preventing unnecessary child re-renders.

---

### **5. What happens if you give an empty dependency array?**  
**Answer:**  
It runs **once** and never recomputes again.

---

### **6. Does useMemo run on every render?**  
**Answer:**  
No. Only when dependencies change.

---

### **7. Difference between useMemo and useCallback?**  
**Answer:**  
- useMemo → memoized **value**  
- useCallback → memoized **function**

---

## 📌 Advanced Level

### **8. Why can overusing useMemo be harmful?**  
**Answer:**  
Because memoization consumes CPU + memory. Overusing it creates unnecessary overhead.

---

### **9. Does useMemo guarantee performance improvement?**  
**Answer:**  
No. You must profile first.

---

### **10. How does useMemo help avoid child re-renders?**  
**Answer:**  
It provides stable object/array references so `React.memo` prevents unnecessary renders.

---

### **11. Can useMemo optimize filtering/sorting?**  
**Answer:**  
Yes — one of the most common real-world uses.

---

### **12. Can useMemo be used for API caching?**  
**Answer:**  
Not directly. Use React Query or SWR.

---

### **13. Does useMemo persist after component unmount?**  
**Answer:**  
No. Memoized values are per component lifecycle.

---

### **14. How does React internally optimize useMemo?**  
**Answer:**  
It stores previous dependencies & results inside the fiber tree and reuses cached values when dependencies haven't changed.

---

# 🎉 End of README
