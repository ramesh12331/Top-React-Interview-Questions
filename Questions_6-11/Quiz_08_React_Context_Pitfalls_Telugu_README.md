# ⚠️ React Context Pitfalls — ఇంటర్వ్యూ గైడ్ (Telugu)

## 💡 React Context అంటే ఏమిటి?
React Context అనేది **data (theme, user info, language మొదలైనవి)** ని component tree లో props ద్వారా పంపకుండా share చేయడానికి ఉపయోగిస్తారు.

### ఉదాహరణ:
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

## ⚠️ సాధారణ తప్పులు (Common Pitfalls)

### ❌ 1️⃣ ఒకే Context లో చాలా Data పెట్టడం
```jsx
<AppContext.Provider value={{ user, theme, cart, notifications }}>
  {children}
</AppContext.Provider>
```
- **ఒక value** అయినా మారితే, **అన్ని components** re-render అవుతాయి.  
- దీని వల్ల performance తగ్గుతుంది.

✅ **సరైన పద్ధతి: Contexts ని విడగొట్టండి**
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

### ❌ 2️⃣ Non-Memoized Object Values పంపడం
ప్రతి render లో కొత్త object literal పంపితే:
```jsx
<ThemeContext.Provider value={{ theme, setTheme }}>
  {children}
</ThemeContext.Provider>
```
- `theme` మారకపోయినా, కొత్త object reference సృష్టించబడుతుంది → re-render అవుతుంది.

✅ **Fix: `useMemo` వాడండి**
```jsx
const value = React.useMemo(() => ({ theme, setTheme }), [theme]);
<ThemeContext.Provider value={value}>{children}</ThemeContext.Provider>;
```

---

### ❌ 3️⃣ వేగంగా మారే data కోసం Context వాడటం
ఉదాహరణకు: mouse position, typing వంటి fast-updating data కోసం Context వాడితే — **frequent re-renders** వస్తాయి.

✅ **Fix:**
- ఆ data ని **local state** లో ఉంచండి లేదా **Redux, Zustand, Recoil** లాంటి libraries వాడండి.
- Context ని **stable/global data** కోసం మాత్రమే వాడండి.

---

## 🧠 ఇంటర్వ్యూ ప్రశ్నలు

**Q:** నా app slow గా ఉంది — నేను Context లో theme & user data పెట్టాను. ఎందుకు?  
**A:** Context లో value మారినప్పుడు, దానిని ఉపయోగిస్తున్న అన్ని components re-render అవుతాయి — ఆ data వాడకపోయినా కూడా.

**Q:** unnecessary re-renders ఎలా నివారించాలి?  
**A:** `useMemo` వాడండి మరియు పెద్ద Context ని చిన్న Contexts గా విభజించండి.

---

## ✅ Summary Table
| అంశం | తప్పు పద్ధతి | సరైన పద్ధతి |
|--------|---------------|---------------|
| పెద్ద Context | అన్ని state ఒకే Context లో పెట్టడం | వేర్వేరు Contexts గా విడగొట్టడం |
| Re-renders | కొత్త object literals పంపడం | `useMemo` వాడటం |
| వేగంగా మారే data | Context వాడటం | Local state / Redux వాడటం |
| Maintainability | Context ఎక్కువ వాడటం | అవసరమైతే మాత్రమే వాడటం |

---

## 💬 Short Summary
React Context తో global data share చేయడం సులభం, కానీ దాన్ని తప్పుడు పద్ధతిలో వాడితే performance సమస్యలు వస్తాయి.  
👉 Contexts ని విడగొట్టండి, memoize చేయండి, మరియు వేగంగా మారే data కోసం Context ని వాడకండి.
