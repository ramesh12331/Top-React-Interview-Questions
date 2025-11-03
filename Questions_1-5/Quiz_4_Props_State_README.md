# ⚛️4. What is the difference between state and props in React?

# ⚛️ Props vs State — React Explained

## ✅ What are Props?
Props are **inputs to a component** — data passed from a **parent** to a **child**.

| Concept | Meaning |
|--------:|---------|
| **Props** | Data passed from parent to child |
| **Nature** | Read-only inside the child |
| **Ownership** | Controlled by parent |
| **Purpose** | To configure or customize a component |

> **Props = “inputs to a component”**

---

## ✅ What is State?
State is the **component’s personal memory** — data owned and managed by the component itself.

| Concept | Meaning |
|--------:|---------|
| **State** | Data owned by the component itself |
| **Nature** | Mutable (can change using setState or hooks) |
| **Ownership** | Controlled by the same component |
| **Purpose** | To store dynamic values that change over time |

> **State = “component’s personal memory”**

---

## 🔁 Visual Flow
```
Parent (props) --> Child
Child (state) --> internal re-render triggers
```

---

## 🧩 Detailed Example
```jsx
// Child Component (uses both props and state)
function Counter({ initialValue }) {          // <-- props
  const [count, setCount] = React.useState(initialValue);  // <-- state

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}

// Parent Component
function App() {
  return <Counter initialValue={5} />;  // passing props
}
```

### 📝 Explanation
| Feature | Controlled By | Mutable? | Example |
|--------:|---------------|---------:|--------:|
| **props** | Parent | ❌ | `initialValue={5}` |
| **state** | Child | ✅ | `count` changed by button click |

---

## 🍳 Real-World Analogy
| Concept | Analogy |
|--------:|--------|
| **Props** | Data given to you like instructions (e.g., a recipe someone gives you) |
| **State** | Data you maintain yourself (e.g., actual cooking progress: chopping, boiling time) |

---

## 🔄 How State Causes Re-render
When `setCount(count + 1)` is called:
1. React updates the **state**.
2. React **re-renders** the component.
3. Virtual DOM **diffs** old vs new UI.
4. React updates only the changed part in the real DOM — **efficient update**!

---

## 🧾 Comparison Table (For Interview)
| Feature | Props | State |
|--------:|:-----:|:-----:|
| Data source | Parent | Component itself |
| Mutable? | No (read-only) | Yes (can update) |
| Who controls? | Parent component | Same component |
| Used for | Configuration | Dynamic data |
| Triggers re-render? | If parent changes props | Yes, when state changes |

---

## 🧠 Final Summary (Interview-Style Answer)
- **Props** are external inputs passed from parent to child and are **read-only** inside the child.  
- **State** is internal data owned by the component and can change over time using `setState` or hooks.  
- When **state changes**, React re-renders the component and updates the UI efficiently via the Virtual DOM.

---

### ✨ Created with ❤️ by Ramesh
> _Empowering developers to build, learn, and grow._

---

### 📚 Tags
`#React` `#Props` `#State` `#Frontend` `#JavaScript`
