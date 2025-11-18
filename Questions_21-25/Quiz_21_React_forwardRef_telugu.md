# 📘 React `forwardRef()` — పూర్తి తెలుగు గైడ్

## ⭐ పరిచయం

React లో `forwardRef()` అనే API వల్ల Parent component → Child component లో ఉండే DOM element ను నేరుగా access చేయగలదు. సాధారణంగా custom component కు ref ఇస్తే అది లోపల DOM దాకా వెళ్లదు. ఆ సమస్యను `forwardRef()` పరిష్కరిస్తుంది.

---

## 🔍 `forwardRef()` అంటే ఏమిటి?

Child component లోపల ఉన్న DOM node ను Parent నేరుగా access చేయాలంటే **ref ని forward చేయాలి**.

**అవసరం అయ్యే సందర్భాలు:**

* Parent నుండి child input కు focus పెట్టాలి
* Scroll, animation, validation
* Custom reusable inputs
* UI libraries నిర్మించేటప్పుడు

---

## 🧠 Syntax

```jsx
const MyComponent = forwardRef((props, ref) => {
  return <input ref={ref} {...props} />;
});
```

Parent ఇచ్చిన ref → Child లోని DOM కు వెళ్లిపోతుంది.

---

## 🟢 Simple Example — Parent నుండి Child Inputకి Focus

```jsx
import React, { forwardRef, useRef } from "react";

const CustomInput = forwardRef((props, ref) => {
  return <input ref={ref} {...props} />;
});

function App() {
  const inputRef = useRef();

  const focusInput = () => {
    inputRef.current.focus();
  };

  return (
    <div>
      <CustomInput ref={inputRef} placeholder="Type here" />
      <button onClick={focusInput}>Focus Input</button>
    </div>
  );
}
```

✔ Parent → child input కి focus పెట్టగలదు

---

## 🔥 Medium Example — Reusable Form Components

```jsx
const TextField = forwardRef((props, ref) => {
  return (
    <div>
      <label>{props.label}</label>
      <input ref={ref} type="text" />
    </div>
  );
});

function Form() {
  const nameRef = useRef();

  function highlight() {
    nameRef.current.style.border = "2px solid red";
  }

  return (
    <>
      <TextField label="Name" ref={nameRef} />
      <button onClick={highlight}>Highlight Input</button>
    </>
  );
}
```

✔ Custom form fields కి perfect

---

## 🧩 Advanced — useImperativeHandle తో Custom Methods

```jsx
const FancyInput = forwardRef((props, ref) => {
  const inputRef = useRef();

  useImperativeHandle(ref, () => ({
    focus: () => inputRef.current.focus(),
    clear: () => (inputRef.current.value = "")
  }));

  return <input ref={inputRef} />;
});
```

✔ `.focus()`, `.clear()` వంటి methods parent కు ఇవ్వొచ్చు

---

## 🎯 ఎప్పుడు వాడాలి?

### ✔ వాడాల్సిన సందర్భాలు:

* DOM direct access అవసరం ఉన్నప్పుడు
* Custom UI components లో
* Animations, scroll, focus
* Methods expose చేయాలంటే

---

## ❗ తప్పులు

❌ ref ని forward చేయడం మర్చిపోవడం
❌ అవసరం లేకుండా forwardRef వాడటం
❌ useImperativeHandle ఉపయోగించకపోవడం
❌ Ref‌ని DOMకు attach చేయకపోవడం

---

## 🎤 Interview Questions (తెలుగులో)

### **Basic**

**❓ forwardRef అంటే?** → Parent ref ను child DOMకు పంపే API.

### **Intermediate**

**❓ ఇది ఏ సమస్యను solve చేస్తుంది?** → Custom component లోని DOM కు direct access ఇస్తుంది.

### **Advanced**

**❓ custom methods ఎలా expose చేయాలి?** → `useImperativeHandle` ఉపయోగించి.

---

## 📝 Summary

* `forwardRef()` Parent → Child DOM access ఇస్తుంది
* Focus, scroll, animation కోసం చాలా ఉపయోగపడుతుంది
* Reusable componentల్లో ముఖ్యమైనది
* `useImperativeHandle` తో మరింత powerful అవుతుంది
