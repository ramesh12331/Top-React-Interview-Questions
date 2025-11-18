# 📘 React `useState` Hook — Full Guide (Telugu)

## ⭐ పరిచయం

React లో **`useState`** Hook functional components లో state (మారుతున్న డేటా)ని నిల్వ చేయడానికి మరియు అప్‌డేట్ చేయడానికి ఉపయోగిస్తారు. State మారితే component **re-render** అవుతుంది.

---

## 📌 నిర్వచనం

`useState` ద్వారా మీరు:

* ఒక component లో data సేవ్ చేయవచ్చు
* ఆ data ను అప్‌డేట్ చేయవచ్చు
* మారినప్పుడు UI ఆటోమేటిక్‌గా refresh అవుతుంది

---

## 🧠 Syntax

```jsx
const [state, setState] = useState(initialValue);
```

**state** → ప్రస్తుత విలువు
**setState** → state అప్‌డేట్ చేసే function

---

## 🟢 Example — Basic Usage

```jsx
const [color, setColor] = useState("red");
```

---

## 🟢 Example — State Read చేయడం

```jsx
function FavoriteColor() {
  const [color, setColor] = useState("red");
  return <h1>My favorite color is {color}!</h1>;
}
```

---

## 🔥 State Update చేయడం

```jsx
setColor("blue");
```

❗ Direct mutation చేయకూడదు → `color = "blue"` ❌

---

# 🧩 State ఏం స్టోర్ చేయగలదు?

✔ strings
✔ numbers
✔ booleans
✔ arrays
✔ objects
✔ functions మరియు మరిన్ని

---

## 🧩 Multiple States Example

```jsx
const [brand, setBrand] = useState("Ford");
const [model, setModel] = useState("Mustang");
```

---

## 🧩 Single Object State Example

```jsx
const [car, setCar] = useState({ brand: "Ford", model: "Mustang" });
```

---

# 🔧 Object State Update — సరైన విధానం

### ❌ Wrong

```jsx
setCar({ model: "Fiesta" });
```

### ✅ Correct

```jsx
setCar(prev => ({ ...prev, model: "Fiesta" }));
```

---

# 🔧 Array State Update

```jsx
setItems(prev => [...prev, newItem]);
```

---

# 🏗 Real‑Time Examples

## 1️⃣ Login Form

```jsx
const [email, setEmail] = useState("");
const [password, setPassword] = useState("");
```

---

## 2️⃣ Theme Toggle

```jsx
setTheme(prev => (prev === "light" ? "dark" : "light"));
```

---

## 3️⃣ Todo List

```jsx
setTasks(prev => [...prev, task]);
```

---

## 4️⃣ Modal Toggle

```jsx
setIsOpen(true);
```

---

## 5️⃣ Counter with Step

```jsx
setCount(prev => prev + step);
```

---

# 🎯 ఎప్పుడు `useState` ఉపయోగించాలి?

* Value మారినప్పుడు UI update కావాలి
* Inputs, toggles, counters వంటి interactive values
* Local component data

---

# ❗ తప్పులు నివారించాలి

❌ Direct mutation
❌ Previous state depend అయినప్పుడు normal setState
❌ Unnecessary deep objects

---

# ⚡ Best Practices

✔ State చిన్నగా ఉంచాలి
✔ Multiple states ఉపయోగించండి, nested objects కాకుండా
✔ Previous value కోసం functional updates ఉపయోగించండి

---

# 🔧 Useful Tricks

### Toggle Boolean

```jsx
setShow(prev => !prev);
```

### Reset State

```jsx
setForm(initialState);
```

### Add to Array

```jsx
setItems(prev => [...prev, item]);
```

### Update Object Field

```jsx
setUser(prev => ({ ...prev, age: prev.age + 1 }));
```

---

# 📝 Summary

* `useState` React లో state manage చేయడానికి ప్రధాన Hook
* State మారితే component re-render అవుతుంది
* Arrays & objects ను update చేయడానికి spread operator ఉపయోగించాలి

---

# 🎤 Interview Q&A

### 🟢 1. useState అంటే ఏమిటి?

React లో state నిల్వ & అప్‌డేట్ చేసే Hook.

### 🟢 2. State update అయితే ఏమవుతుంది?

Component **re-render** అవుతుంది.

### 🟡 3. useState ఏ values స్టోర్ చేయగలదు?

Strings, numbers, arrays, objects, booleans—అన్నీ.

### 🟡 4. Previous state మీద ఆధారపడితే?

Functional update ఉపయోగించాలి:

```jsx
setCount(prev => prev + 1);
```

### 🔥 5. Nested objects update ఎలా?

Spread operator వాడాలి:

```jsx
setState(prev => ({ ...prev, user: { ...prev.user, name: "Ramesh" }}));
```

---

ఇక మీరు చెప్పండి — దీనికి **TOC జోడించాలా?**, లేదా **useState vs useReducer vs useRef** comparison కూడా కావాలా?
