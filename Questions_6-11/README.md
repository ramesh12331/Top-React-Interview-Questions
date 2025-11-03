# ⚛️ React Interview Notes — Quick Summary

A concise guide covering key React concepts and interview-ready explanations.

---

## 🧩 6. Using Array Indices as Keys

- **Problem:** Using array indices (`0,1,2...`) as keys causes UI mismatches when list order changes.  
- **Why:** React uses keys to track elements — changing order breaks the mapping.  
- **Effect:** Wrong DOM reuse, messed-up inputs or animations.  
- **Best Practice:** Use unique stable keys (like IDs).  
- ✅ OK only for static lists that never change.

---

## 🧠 7. Controlled vs Uncontrolled Components

| Type | Controlled | Uncontrolled |
|------|-------------|---------------|
| Data Source | React state | DOM (via ref) |
| Access Value | state & `onChange` | `ref.current.value` |
| Validation | Easy | Harder |
| Use Case | Dynamic forms | Simple inputs |

🟢 **Use controlled** for validations & logic.  
🟡 **Use uncontrolled** for simple or external forms.

---

## ⚠️ 8. Pitfalls of Using Context

Common mistakes:
1. **Too much data in one context** → unnecessary re-renders  
   → ✅ Split contexts  
2. **Non-memoized values** → use `useMemo`  
3. **Fast-changing data** (like typing) → avoid context; use local state or stores  

**Tip:** Context is great for stable global data, not for frequent updates.

---

## 🧭 9. Benefits of Using Hooks

- Simplifies code (no classes or `this`)
- Reusable logic via **custom hooks**
- Better readability & functional approach
- Easier testing & gradual migration
- Encourages cleaner, modular design

| Feature | Before (Class) | After (Hooks) |
|----------|----------------|----------------|
| State | `this.state` | `useState()` |
| Lifecycle | `componentDidMount` | `useEffect()` |
| Logic Reuse | HOCs / Render Props | Custom Hooks |

---

## ⚙️10. Rules of React Hooks

1. **Call hooks at the top level** – not inside loops, conditions, or nested functions.  
2. **Only call hooks in React components or custom hooks.**  
3. **Custom hooks must start with `use`.**

💡 React tracks hooks by **order** — breaking order breaks state mapping.

---

## 🧩11. useEffect vs useLayoutEffect

| Aspect | `useEffect` | `useLayoutEffect` |
|--------|--------------|-------------------|
| Runs | After paint | Before paint |
| Blocking | ❌ No | ✅ Yes |
| Use for | Data fetching, logging | DOM measurement, layout sync |
| Performance | Better | Heavier if misused |

🟢 Default to `useEffect`  
🔧 Use `useLayoutEffect` for DOM-dependent logic or animations.

---

## 🧾 Summary

| Concept | Key Takeaway |
|----------|--------------|
| Keys | Use stable unique IDs |
| Forms | Controlled = React-managed; Uncontrolled = DOM-managed |
| Context | Split & memoize to avoid re-renders |
| Hooks | Simplify logic & improve reusability |
| Hook Rules | Always top-level, inside React, start with `use` |
| useEffect vs useLayoutEffect | Async vs sync side effects |

---

**Author:** _Mamidi Ramesh_  
**Category:** React.js Interview Preparation  
**Version:** 1.0  
