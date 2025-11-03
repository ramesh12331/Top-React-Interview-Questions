# ⚛️ React Q7 — Controlled vs Uncontrolled Components (Telugu)

## 🔍 మూల భావన (Core Idea)
React లో form inputs ని రెండు విధాలుగా నిర్వహించవచ్చు:
- **Controlled Components** — React state ద్వారా form dataని నియంత్రిస్తుంది  
- **Uncontrolled Components** — DOM (HTML element) dataని స్వయంగా నిర్వహిస్తుంది (Refs ద్వారా access)

---

## 🧠 1️⃣ Controlled Components

### 💡 నిర్వచనం (Definition)
Controlled Component లో input value React state ద్వారా నియంత్రించబడుతుంది.  
అంటే, input లో ఏమి టైప్ చేసినా, అది `useState()` లో ఉన్న విలువ ద్వారా ప్రతిబింబిస్తుంది.

### 🧩 ఉదాహరణ (Example)
```jsx
function ControlledInput() {
  const [value, setValue] = React.useState('');

  return (
    <div>
      <input
        value={value} // ✅ React state ద్వారా నియంత్రించబడుతుంది
        onChange={(e) => setValue(e.target.value)}
      />
      <p>ప్రస్తుత విలువ: {value}</p>
    </div>
  );
}
```

### ⚙️ ఇది ఎలా పని చేస్తుంది?
1. యూజర్ టైప్ చేస్తే → `onChange` event ట్రిగ్గర్ అవుతుంది  
2. React state (`setValue`) అప్‌డేట్ అవుతుంది  
3. Input value state నుండి తిరిగి రీ-రెండర్ అవుతుంది  

🧠 **State అనేది ఒకే “source of truth” (నిజమైన మూలం)**

---

## 🧠 2️⃣ Uncontrolled Components

### 💡 నిర్వచనం (Definition)
Uncontrolled Component లో input value DOM ద్వారా స్వయంగా నిర్వహించబడుతుంది.  
React దానిని state ద్వారా track చేయదు. Valueని **ref** ద్వారా access చేస్తాము.

### 🧩 ఉదాహరణ (Example)
```jsx
function UncontrolledInput() {
  const inputRef = React.useRef();

  const handleClick = () => {
    alert(`Input value: ${inputRef.current.value}`);
  };

  return (
    <>
      <input ref={inputRef} />  {/* DOM ఈ input ని నియంత్రిస్తుంది */}
      <button onClick={handleClick}>Show Value</button>
    </>
  );
}
```

### ⚙️ ఇది ఎలా పని చేస్తుంది?
- Input తన valueని DOM లో స్వయంగా ఉంచుకుంటుంది  
- React దానిని `ref.current.value` ద్వారా మాత్రమే చదవగలదు  

🧠 **ఇక్కడ “DOM” source of truth అవుతుంది**

---

## ⚖️ పోలిక పట్టిక (Comparison Table)

| లక్షణం | Controlled Component | Uncontrolled Component |
|--------|----------------------|------------------------|
| డేటా మూలం | React State | DOM |
| విలువ పొందడం | state ద్వారా | ref ద్వారా |
| అప్డేట్స్ | ప్రతి టైప్ మీద | అవసరమైనప్పుడు మాత్రమే |
| Validation | సులభం | కష్టంగా ఉంటుంది |
| Use Case | Dynamic forms, Validations | Simple forms |
| Performance | కొంచెం నెమ్మది | వేగంగా ఉంటుంది |

---

## 🧩 ఉదాహరణలు (Example Scenarios)

### ✅ Controlled Form (Validation కోసం)
```jsx
function Form() {
  const [email, setEmail] = React.useState('');

  const handleSubmit = (e) => {
    e.preventDefault();
    if (!email.includes('@')) alert("Invalid email!");
  };

  return (
    <form onSubmit={handleSubmit}>
      <input value={email} onChange={(e) => setEmail(e.target.value)} />
      <button>Submit</button>
    </form>
  );
}
```

### ✅ Uncontrolled Form (సాధారణ ఉపయోగం కోసం)
```jsx
function SimpleForm() {
  const emailRef = React.useRef();

  const handleSubmit = (e) => {
    e.preventDefault();
    alert(`Submitted: ${emailRef.current.value}`);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input ref={emailRef} />
      <button>Submit</button>
    </form>
  );
}
```

---

## ✅ ఉత్తమ పద్ధతులు (Best Practices)

✔️ **Controlled Components వాడాలి**, ఎప్పుడు:
- Live validation చేయాలి
- React state లో form data అవసరం ఉంటుంది

✔️ **Uncontrolled Components వాడాలి**, ఎప్పుడు:
- Simple forms లేదా external plugins వాడుతున్నప్పుడు
- React state అవసరం లేనప్పుడు

---

## 🧾 సారాంశం (Summary)

| అంశం | Controlled | Uncontrolled |
|-------|-------------|--------------|
| నిర్వచనం | React state ద్వారా నియంత్రణ | DOM ద్వారా నియంత్రణ |
| Value Access | state మరియు onChange | ref.current.value |
| Best For | Validations, Dynamic forms | Simple forms |
| Performance | కొంచెం ఎక్కువ rendering | తక్కువ rendering |

---

## 🎯 Interview Tip
> “Controlled components లో React inputs ని పూర్తిగా నియంత్రిస్తుంది, validations సులభం అవుతుంది.  
> Uncontrolled components లో DOM నియంత్రిస్తుంది, సులభమైన కానీ తక్కువ flexibility ఉన్న విధానం.” ✅
