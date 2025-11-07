# ⚛️ 15. What is the `useCallback` Hook in React and When Should It Be Used?

## 🧠 Definition (Simple and Clear)
The **`useCallback` hook** in React is used to **memoize functions** — meaning React will **reuse the same function instance** between re-renders **unless its dependencies change**.

This helps to **optimize performance** and **prevent unnecessary re-renders**, especially when passing functions as props to child components.

---

## ⚙️ Syntax
```jsx
const memoizedFunction = useCallback(() => {
  // function logic
}, [dependencies]);
```

### Parameters:
- **Callback function** → The function you want to memoize.
- **Dependency array** → The values that determine when the function should be re-created.

✅ If dependencies don’t change → React reuses the same function reference.

---

## 🧩 Example — Preventing Unnecessary Child Re-renders
```jsx
import React, { useState, useCallback } from 'react';

function Parent() {
  const [count, setCount] = useState(0);

  // ✅ Memoized function
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

### 🧠 Explanation
- In React, **functions are recreated** on every re-render.
- If `increment` was recreated every time, the **Child** would re-render even if `count` didn’t change.
- `useCallback` keeps the same function reference across renders (until dependencies change).  
✅ The child component re-renders **only when necessary**.

---

## 🧩 Example — With Dependencies
```jsx
const fetchData = useCallback(() => {
  console.log('Fetching data for user:', userId);
}, [userId]);
```
✅ This function will only be recreated if `userId` changes.

---

## ⚡ When Should You Use `useCallback`?
Use `useCallback` when:
- You pass **functions as props** to child components.
- You use **`React.memo()`** to memoize components.
- You want to **avoid recreating event handlers** unnecessarily.
- The function is **expensive** (e.g., complex calculations or API calls).

---

## 🚫 Common Mistakes
| Mistake | Why It’s Wrong |
|----------|----------------|
| ❌ Using `useCallback` everywhere | Adds unnecessary complexity and memory overhead |
| ❌ Forgetting dependencies | Leads to stale data or outdated references |
| ❌ Over-optimizing | Can slow down rendering if misused |

---

## ✅ Best Practices
✔ Use `useCallback` when passing **callbacks to memoized children** (`React.memo`).  
✔ Include **all dependencies** used inside the callback.  
✔ Don’t overuse — measure performance first.  
✔ Combine with `useMemo` and `React.memo` for maximum optimization.

---

## 🎨 Visual Diagram — How `useCallback` Works
```
Render 1 → Creates function F1
Render 2 (no dependency change) → Reuses F1 ✅
Render 3 (dependency changes) → Creates new function F2 ⚡

✅ Same reference → No re-render in child
⚡ New reference → Child re-renders
```

---

## 💡 Trick to Remember
> 🧩 “`useCallback` keeps your function’s memory fresh — until its dependencies change.”

| Hook | Memoizes What | Common Use |
|------|----------------|-------------|
| `useCallback` | Function reference | Prevent child re-renders |
| `useMemo` | Computed value | Cache expensive calculations |

---

## 💬 Interview Scenarios
**🗣 Scenario 1:** How to avoid unnecessary re-renders in child components?  
👉 Use `useCallback` with `React.memo` to keep the same function reference.

**🗣 Scenario 2:** Why do functions cause re-renders?  
👉 Because in JavaScript, new function objects are created on every re-render.

**🗣 Scenario 3:** What’s the difference between `useCallback` and `useMemo`?  
👉 `useCallback` memoizes a function, `useMemo` memoizes the result of a function.

---

## 🧾 Short Interview Summary
> “`useCallback` memoizes a function so it doesn’t get recreated on every render. It’s mainly used to optimize performance and prevent unnecessary child re-renders when passing callbacks as props.”

---

## ⚡ One-Line Trick
> 🧠 “`useCallback` = memoized function. Only changes when dependencies change.”

---

**Author:** _Mamidi Ramesh_  
**Topic:** React Hooks — `useCallback` Hook  
**Category:** Frontend / React.js


---

## ⚖️ Visual Comparison — `useCallback` vs `useMemo` vs `React.memo`

| Feature | `useCallback` | `useMemo` | `React.memo` |
|----------|----------------|------------|---------------|
| **What it memoizes** | Function reference | Computed value (result) | Whole component output |
| **When it re-runs** | When dependencies change | When dependencies change | When props change |
| **Purpose** | Prevents re-creation of functions | Caches expensive calculations | Prevents unnecessary re-renders of child components |
| **Returns** | The same function reference | The computed value | The same component instance (if props unchanged) |
| **Used With** | Functions passed as props | Expensive computations | Functional components |
| **Typical Use Case** | Event handlers, API callbacks | Derived values, filtering, sorting | Wrapping child components |

---

## 🎨 Visual Flow Diagram
```
Parent Component Re-renders
│
├── Without useCallback → New function created → Child re-renders ⚠️
│
├── With useCallback → Function reused → Child does NOT re-render ✅
│
├── With useMemo → Computed value cached ✅
│
└── With React.memo → Child re-renders only if props change ✅
```

---

## 💡 Trick to Remember
> 🧠 “useCallback = Memoize the Function 🔁  |  useMemo = Memoize the Value 💾  |  React.memo = Memoize the Component 🧩”

---

**Author:** _Mamidi Ramesh_  
**Topic:** React Hooks — `useCallback`, `useMemo`, and `React.memo` Comparison  
**Category:** Frontend / React.js

---

## 🧩 Example — Combining `useCallback` and `useMemo`

```jsx
import React, { useState, useCallback, useMemo } from 'react';

function ProductList({ products }) {
  const [filter, setFilter] = useState('');

  // ✅ Memoize the filter function to avoid recreation on each render
  const filterProducts = useCallback(() => {
    return products.filter((p) => p.name.toLowerCase().includes(filter.toLowerCase()));
  }, [products, filter]);

  // ✅ useMemo caches the filtered results until dependencies change
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

### 🧠 Explanation
- `useCallback()` ensures the **filtering function** keeps the same reference unless `products` or `filter` changes.
- `useMemo()` caches the **filtered result**, recomputing only when the function (or its dependencies) changes.

✅ **Result:** Efficient filtering — no unnecessary recalculations or re-renders.

---

**Tip:**
> Combine `useCallback` for stable function references and `useMemo` for caching computed results in performance-sensitive components.
