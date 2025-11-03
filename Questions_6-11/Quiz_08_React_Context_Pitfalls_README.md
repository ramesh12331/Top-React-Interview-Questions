# 8. What are some pitfalls about using context in React?

# ⚠️ React Context Pitfalls — Interview Guide

## 💡 What is React Context?
React Context allows you to share data (like theme, user info, or language) across components without prop drilling.

### Example Setup
```jsx
const ThemeContext = React.createContext();

function App() {
  const [theme, setTheme] = React.useState("light");
  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      <Toolbar />
    </ThemeContext.Provider>
  );
}

function Toolbar() {
  const { theme, setTheme } = React.useContext(ThemeContext);
  return (
    <button onClick={() => setTheme(theme === "light" ? "dark" : "light")}>
      Current theme: {theme}
    </button>
  );
}
```

---

## ⚠️ Common Pitfalls

### ❌ 1. Too Much Data in One Context
```jsx
<AppContext.Provider value={{ user, theme, cart, notifications }}>
  {children}
</AppContext.Provider>
```
- When **any value** changes, **all components** using this context re-render.  
- This causes performance issues.

✅ **Fix: Split contexts**
```jsx
<UserContext.Provider value={user}>
  <ThemeContext.Provider value={theme}>
    <CartContext.Provider value={cart}>
      {children}
    </CartContext.Provider>
  </ThemeContext.Provider>
</UserContext.Provider>
```

---

### ❌ 2. Passing Non-Memoized Objects
If you pass new object literals every render:
```jsx
<ThemeContext.Provider value={{ theme, setTheme }}>
  {children}
</ThemeContext.Provider>
```
- Even if `theme` doesn’t change, the object reference is new → triggers re-renders.

✅ **Fix: Use `useMemo`**
```jsx
const value = React.useMemo(() => ({ theme, setTheme }), [theme]);
<ThemeContext.Provider value={value}>{children}</ThemeContext.Provider>;
```

---

### ❌ 3. Using Context for Rapidly Changing Data
Using context for fast-updating data (e.g., mouse position, typing) causes **frequent re-renders**.

✅ **Fix**
- Keep fast-changing data in **local state** or use libraries like **Redux, Zustand, or Recoil**.
- Use Context only for stable, global data.

---

## 🧠 Interview Scenarios

**Q:** Why is my app slow when I use Context for theme and user data?  
**A:** Context re-renders all consumers whenever its value changes — even those not using that data.

**Q:** How can you prevent unnecessary re-renders?  
**A:** Use `useMemo` and split large contexts into smaller ones.

---

## ✅ Summary Table
| Aspect | Bad Practice | Good Practice |
|--------|---------------|---------------|
| Large context value | Putting all app state in one context | Split into multiple contexts |
| Re-renders | Passing new object literals | Use `useMemo` |
| Rapid updates | Using context for fast-changing data | Use local state or Redux/Zustand |
| Maintainability | Overusing context | Use only when prop drilling is painful |

---

## 💬 Short Summary
React Context is great for sharing global data, but misuse can cause performance issues.  
👉 Split contexts, memoize values, and avoid using it for fast-changing data.
