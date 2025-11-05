# ⚛️ React Interview Notes — Quick Summary  
**Author:** 🧑‍💻 *Ramesh Mamidi*  
**Category:** Frontend / React.js  
**Goal:** Master React Interview Questions — Short, Clear & Practical

---

## 🧩 6. Using Array Indices as Keys

### 💡 Definition  
React uses **keys** to identify list items during rendering.  
If you use **array indices (`0,1,2...`)** as keys, React can confuse elements when list order changes, causing UI bugs.

---

### ⚙️ Syntax
```jsx
// ❌ Bad: Using index as key
{items.map((item, index) => (
  <li key={index}>{item}</li>
))}

// ✅ Good: Use unique ID
{items.map(item => (
  <li key={item.id}>{item.name}</li>
))}
```

---

### 🧠 Example
```jsx
const fruits = [
  { id: 1, name: "Apple" },
  { id: 2, name: "Banana" },
  { id: 3, name: "Cherry" },
];

function FruitList() {
  return (
    <ul>
      {fruits.map(fruit => (
        <li key={fruit.id}>{fruit.name}</li>
      ))}
    </ul>
  );
}
```

---

### ⚠️ Consequence  
- React may reuse wrong DOM nodes  
- Causes UI mismatches or wrong animations  
- Inputs can retain wrong values  

✅ **Fix:** Always use stable, unique IDs.

🎯 **Trick:** *“Index key = Invisible bug”*

---

## 🧩 7. Controlled vs Uncontrolled Components

### 💡 Definition  
React form inputs can be managed in two ways:  
- **Controlled:** React state controls the value.  
- **Uncontrolled:** DOM manages its own value using `ref`.

---

### ⚙️ Syntax & Example

#### ✅ Controlled Component
```jsx
function ControlledInput() {
  const [value, setValue] = React.useState("");

  return (
    <div>
      <input
        value={value}
        onChange={(e) => setValue(e.target.value)}
      />
      <p>Current Value: {value}</p>
    </div>
  );
}
```

#### ✅ Uncontrolled Component
```jsx
function UncontrolledInput() {
  const inputRef = React.useRef();

  const handleClick = () => alert(inputRef.current.value);

  return (
    <>
      <input ref={inputRef} />
      <button onClick={handleClick}>Show Value</button>
    </>
  );
}
```

---

### ⚖️ Comparison Table

| Feature | Controlled | Uncontrolled |
|----------|-------------|--------------|
| Data Source | React state | DOM |
| Access | via state | via ref |
| Validation | Easy | Hard |
| Use Case | Dynamic forms | Simple forms |

🎯 **Trick:** *“Controlled = React boss, Uncontrolled = DOM boss”*

---

## ⚠️ 8. React Context Pitfalls

### 💡 Definition  
React Context shares global data (like user, theme) between components without prop drilling.

---

### ⚙️ Syntax
```jsx
const ThemeContext = React.createContext();

function App() {
  const [theme, setTheme] = React.useState("light");
  const value = React.useMemo(() => ({ theme, setTheme }), [theme]);

  return (
    <ThemeContext.Provider value={value}>
      <Toolbar />
    </ThemeContext.Provider>
  );
}
```

---

### ⚠️ Common Pitfalls
1️⃣ Putting **too much data** in one context → many re-renders  
2️⃣ Passing **new object literals** each render  
3️⃣ Using context for **fast-changing data**  

✅ **Fixes:**
- Split contexts by feature  
- Memoize values using `useMemo()`  
- Use local state or Redux/Zustand for dynamic data  

🎯 **Trick:** *“Context = Global, but don’t overload it!”*

---

## 🪝 9. Benefits of Using Hooks

### 💡 Definition  
Hooks let functional components use **state, lifecycle, and context** — no class components needed.

---

### ⚙️ Syntax
```jsx
function Counter() {
  const [count, setCount] = React.useState(0);

  React.useEffect(() => {
    console.log(`Count is ${count}`);
  }, [count]);

  return <button onClick={() => setCount(count + 1)}>Increment</button>;
}
```

---

### ✅ Benefits
1️⃣ Simpler, cleaner code  
2️⃣ Reusable logic via **custom hooks**  
3️⃣ Better readability  
4️⃣ Functional, testable style  
5️⃣ No `this` confusion  

🎯 **Trick:** *“Hooks = Power to functional components”*

---

## 🧠 10. Rules of Hooks

### 💡 Definition  
Hooks must follow strict rules for React to maintain state order correctly.

---

### ⚙️ Rules & Examples

#### 1️⃣ Call Hooks at Top Level  
```jsx
// ✅ Correct
const [count, setCount] = useState(0);

// ❌ Wrong
if (isActive) {
  const [count, setCount] = useState(0);
}
```

#### 2️⃣ Only Inside React Functions  
```jsx
function MyComponent() {
  const [data, setData] = useState([]);
}
```

#### 3️⃣ Custom Hooks Must Start with “use”  
```jsx
function useFetchData() {
  const [data, setData] = useState([]);
  return data;
}
```

🎯 **Trick:** *“Top → React → use” = 3 Golden Rules!*

---

## ⚙️ 11. useEffect vs useLayoutEffect

### 💡 Definition  
Both run side effects — main difference is **when** they execute.

---

### ⚙️ Syntax
```jsx
useEffect(() => {
  console.log("🎨 useEffect: runs AFTER paint");
}, []);

useLayoutEffect(() => {
  console.log("🧱 useLayoutEffect: runs BEFORE paint");
}, []);
```

---

### ⚖️ Comparison Table

| Hook | Timing | Use Case | Blocking |
|------|---------|-----------|-----------|
| **useEffect** | After paint | API calls, logging, async ops | ❌ No |
| **useLayoutEffect** | Before paint | DOM measurement, animations | ✅ Yes |

🎯 **Trick:** *“Effect after paint, LayoutEffect before paint”*

---

## 🧾 Overall Summary

| # | Topic | Key Idea | Trick |
|---|--------|-----------|--------|
| 6 | Keys | Avoid index as key | “Index key = Invisible bug” |
| 7 | Controlled vs Uncontrolled | React vs DOM control | “React boss vs DOM boss” |
| 8 | Context Pitfalls | Avoid overuse | “Global ≠ Everything” |
| 9 | Hooks Benefits | Reusable logic | “Hooks = Power” |
| 10 | Rules of Hooks | 3 Golden Rules | “Top → React → use” |
| 11 | useEffect vs useLayoutEffect | Timing difference | “Effect after, Layout before” |

---

## 💬 Final Interview Tip  
> 🧠 *“React is all about predictable rendering and stable state.”*  
> ✅ Use unique keys  
> ✅ Keep forms controlled  
> ✅ Use context carefully  
> ✅ Follow hook rules strictly  
> ✅ Choose correct effect timing

---

⭐ **Made with 💻 by Ramesh Mamidi**  
📘 *Frontend Developer | React.js Enthusiast | Interview Notes Collection*
