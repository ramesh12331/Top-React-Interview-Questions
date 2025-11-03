# 🧭 React Hooks అంటే ఏమిటి?

React **Hooks** అనేవి **React 16.8** లో పరిచయం చేయబడ్డాయి.  
ఇవి **functional components** లో state, lifecycle methods, context వంటి ఫీచర్లు వాడటానికి అనుమతిస్తాయి — ఇవి ముందుగా కేవలం **class components** లో మాత్రమే అందుబాటులో ఉండేవి.

---

## ⚙️ Hooks ఎందుకు వచ్చాయి?

Hooks రాకముందు React developers కొన్ని సమస్యలు ఎదుర్కొన్నారు:

🚧 **Complex Class Components**
- Lifecycle methods వేర్వేరు ప్రదేశాల్లో ఉండేవి (`componentDidMount`, `componentDidUpdate`, `componentWillUnmount`).
- ఒకే logic ని నిర్వహించడం కష్టం (fetch + cleanup వేర్వేరు methods లో ఉండేవి).

🔁 **Code Duplication**
- Form handling, data fetching లాంటి logic ని share చేయాలంటే **HOCs** లేదా **Render Props** వాడాల్సి వచ్చేది — దీని వల్ల code nested గా, క్లిష్టంగా మారేది.

🤔 **“this” keyword గందరగోళం**
- Beginners కి `this.state`, `this.setState` వంటి concepts క్లిష్టంగా ఉండేవి.

🔒 **Logic Reuse కష్టం**
- Stateful logic ని components మధ్య share చేయడం సులభం కాదు.

👉 **Hooks ఈ సమస్యలన్నీ పరిష్కరించాయి**, ఎందుకంటే వీటితో మీరు logic ని **functions లోనే compose చేయవచ్చు.**

---

## ⚛️ React Hooks యొక్క ప్రధాన ప్రయోజనాలు

### 1️⃣ కోడ్ నిర్మాణం సులభం

Hooks తో మీరు components ని **సాధారణ functions** లా రాయవచ్చు — classes, constructors, lifecycle methods అవసరం లేదు.

#### 🧩 ఉదాహరణ: Class vs Hook

**Before (Class Component):**
```jsx
class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  }

  componentDidMount() {
    document.title = `Count: ${this.state.count}`;
  }

  componentDidUpdate() {
    document.title = `Count: ${this.state.count}`;
  }

  render() {
    return (
      <button onClick={() => this.setState({ count: this.state.count + 1 })}>
        Count: {this.state.count}
      </button>
    );
  }
}
```

**After (Using Hooks):**
```jsx
import { useState, useEffect } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `Count: ${count}`;
  }, [count]);

  return (
    <button onClick={() => setCount(count + 1)}>
      Count: {count}
    </button>
  );
}
```

✅ Constructors అవసరం లేదు  
✅ `this` keyword లేదు  
✅ కోడ్ సులభంగా చదవవచ్చు, maintain చేయవచ్చు

---

### 2️⃣ Custom Hooks ద్వారా Logic Reuse

Custom Hooks తో మీరు **stateful logic ని extract చేసి share** చేయవచ్చు.

#### ఉదాహరణ:
```jsx
function useWindowWidth() {
  const [width, setWidth] = useState(window.innerWidth);

  useEffect(() => {
    const handleResize = () => setWidth(window.innerWidth);
    window.addEventListener("resize", handleResize);
    return () => window.removeEventListener("resize", handleResize);
  }, []);

  return width;
}

function DisplayWidth() {
  const width = useWindowWidth();
  return <h2>Window width: {width}px</h2>;
}
```

💡 **ప్రయోజనం:** ఈ logic ను ఎక్కడైనా తిరిగి వాడవచ్చు — ఇది కేవలం function మాత్రమే!

---

### 3️⃣ కోడ్ చదవడం సులభం

Hooks (`useState`, `useEffect`) వాడడం వల్ల మీ component logic **modular** గా ఉంటుంది.

✅ సంబంధిత logic ఒకచోటే ఉంటుంది  
✅ maintain చేయడం సులభం

---

### 4️⃣ Functional Programming దృక్పథం

Hooks React ను **functional** గా మార్చాయి — predictable data flow, clear side effects.

```jsx
function Greeting({ name }) {
  useEffect(() => {
    document.title = `Hello, ${name}`;
  }, [name]);

  return <h1>Hello, {name}</h1>;
}
```

---

### 5️⃣ Testing సులభం

Hooks తో మీరు logic ని UI లేకుండా **unit testing** చేయవచ్చు.

---

### 6️⃣ Gradual Migration

మీరు మీ app లో Hooks ని **gradually adopt** చేయవచ్చు — మొత్తం app rewrite చేయాల్సిన అవసరం లేదు.

---

## 🚫 సాధారణ తప్పులు

❌ Hooks ని loops లేదా conditions లో వాడడం — order తప్పుతుంది  
❌ `useEffect` లో dependency array మర్చిపోవడం — infinite re-renders వస్తాయి  
❌ కొత్త object references పంపడం — unnecessary re-renders

✅ **Fix:** Hooks ఎప్పుడూ top-level లో వాడండి, dependency arrays సరైన విధంగా వాడండి, values ని memoize చేయండి.

---

## 🧩 Custom Hook Example — API Call

```jsx
function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch(url)
      .then(res => res.json())
      .then(json => setData(json))
      .finally(() => setLoading(false));
  }, [url]);

  return { data, loading };
}

function UserList() {
  const { data, loading } = useFetch("https://jsonplaceholder.typicode.com/users");

  if (loading) return <p>Loading...</p>;

  return (
    <ul>
      {data.map(user => <li key={user.id}>{user.name}</li>)}
    </ul>
  );
}
```

✅ Reusable  
✅ Logic మరియు UI వేరు

---

## 🧠 ఇంటర్వ్యూ ప్రశ్నలు

**Q1. React Hooks ఎందుకు పరిచయం అయ్యాయి?**  
👉 కోడ్ సింప్లిఫికేషన్, logic reuse, class component సమస్యల పరిష్కారం కోసం.

**Q2. Hooks తో logic reuse ఎలా సాధ్యం?**  
👉 Custom Hooks ద్వారా logic ను functions లోకి మార్చి share చేయవచ్చు.

**Q3. Hooks తో Class Components అవసరం లేకుండా పోతాయా?**  
👉 అవును, React లో కొత్త ఫీచర్లు అన్ని Hooks ఆధారితంగా వస్తాయి.

**Q4. Hooks లో సాధారణ తప్పు ఏది?**  
👉 Rules of Hooks ని ఉల్లంఘించడం లేదా dependency arrays మర్చిపోవడం.

---

## 📝 Summary Table

| అంశం | Class Components | Hooks Components |
|:-------|:----------------|:----------------|
| **State** | this.state, setState() | useState() |
| **Lifecycle** | componentDidMount, componentDidUpdate | useEffect() |
| **Logic Reuse** | HOCs, Render Props | Custom Hooks |
| **Syntax** | Verbose, this వాడాలి | Simple, functional |
| **Testing** | కష్టం | సులభం |
| **Migration** | Full rewrite | Gradual adoption |

---

## 🎯 Short Summary

Hooks React ను modernize చేశాయి — **functions యొక్క simplicity** మరియు **state + side effects యొక్క power** కలిపాయి.  
ఇవి కోడ్ తగ్గిస్తాయి, reusability పెంచుతాయి, complex UI ను సులభంగా handle చేయడానికి సహాయపడతాయి.

🧠 **Think Functionally. Code Efficiently. Reuse Smartly. 🚀**
