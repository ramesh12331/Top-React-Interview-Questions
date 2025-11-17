# 📘 React `useId` Hook — పూర్తి గైడ్ (తెలుగులో)

## ⭐ Introduction

React లో **useId** Hook ప్రత్యేకమైన, స్థిరమైన, unique IDs ని సృష్టించడానికి ఉపయోగిస్తారు. ఇవి:

* Multiple renders
* Client + Server-side rendering (SSR)
* Hydration

ఎన్నిసార్లు component render అయినా ID మారదు.

👉 ముఖ్యంగా **labels** మరియు **inputs** ను accessibility కోసం కలపడానికి వాడుతారు.

---

## 🔍 What is `useId`?

`useId` ఒక React Hook, ఇది:

✔ Unique ID string ఇచ్చుతుంది
✔ Renders మధ్య ID మారదు
✔ SSR + hydration లో perfect గా పనిచేస్తుంది
✔ ID collisions కి అవకాశం ఉండదు

🛑 ఇది random ID కాదు.
🛑 ఇది global counter తో పనిచేయదు.

---

## 🧠 Syntax

```js
const id = useId();
```

ఇది ఈ విధంగా ID ఇస్తుంది:

```
:react-12345
```

దీనికి suffixes జోడించి multiple IDs create చేసుకోవచ్చు.

---

## 🟢 Simple Example

```jsx
import React, { useId } from "react";

function LoginForm() {
  const id = useId();

  return (
    <form>
      <label htmlFor={id}>Username:</label>
      <input id={id} type="text" />
    </form>
  );
}
```

✔ Label మరియు input మధ్య perfect connection
✔ SSR + hydration లో mismatch రాదు

---

## 🔥 Medium Example – Multiple Input Fields

```jsx
function ContactForm() {
  const nameId = useId();
  const emailId = useId();

  return (
    <>
      <label htmlFor={nameId}>Name:</label>
      <input id={nameId} type="text" />

      <label htmlFor={emailId}>Email:</label>
      <input id={emailId} type="email" />
    </>
  );
}
```

✔ ప్రతి field కి unique ID
✔ పెద్ద forms లో perfect

---

## 🧩 Advanced Example — Repeated Components (Dynamic Forms)

Repeated components లో ID clashes కాకుండా useId protect చేస్తుంది.

```jsx
function Question({ label }) {
  const id = useId();

  return (
    <div>
      <label htmlFor={id}>{label}</label>
      <input id={id} />
    </div>
  );
}

function Survey() {
  return (
    <>
      <Question label="Your age" />
      <Question label="Your country" />
      <Question label="Your favorite color" />
    </>
  );
}
```

✔ ప్రతి repeated component కి unique ID వస్తుంది

---

## 🏗 Advanced Example — Accessibility & ARIA Attributes

```jsx
function PasswordField() {
  const inputId = useId();
  const descriptionId = `${inputId}-description`;

  return (
    <div>
      <label htmlFor={inputId}>Password</label>
      <input id={inputId} aria-describedby={descriptionId} type="password" />

      <p id={descriptionId}>
        Your password must include a number and a special character.
      </p>
    </div>
  );
}
```

✔ ARIA attributes కు perfect
✔ Screen readers కు best experience

---

## 🎯 When Should You Use `useId`?

వీటిలో తప్పనిసరిగా useId వాడాలి:

✔ Labels ↔ Inputs linking
✔ ARIA attributes
✔ Repeated components IDs
✔ SSR hydration-safe IDs
✔ Accessibility requirements

❌ వాడకూడదు:

* List keys కోసం
* Random IDs create చేయడానికి
* Database IDs కోసం
* Component బయట

---

## ❗ Mistakes to Avoid

❌ `Math.random()` వాడడం → SSR mismatch

❌ Global counters వాడడం → Server/Client IDs match కావు

❌ పెద్ద forms లో manually IDs ఇవ్వడం

❌ IDs ని parent renders మీద ఆధారపరచడం

---

## ⚡ Best Practices

✔ Labels & inputs కోసం useId తప్పనిసరి

✔ Suffixes వాడి multiple related IDs create చేయండి

```js
const baseId = useId();
const titleId = `${baseId}-title`;
const descId = `${baseId}-desc`;
```

✔ Reusable components లో useId perfect

✔ Accessibility-first development కి best tool

---

## 📝 Summary

* useId → unique, stable, hydration-safe IDs
* SSR + CSR రెండింటిలో పనిచేస్తుంది
* Accessibility కి చాలా ముఖ్యమైన Hook
* Form fields, aria attributes, repeated components కు best

---

## 🎤 Interview Questions & Answers

### 🟢 Basic Level

**❓ What is useId?**
💡 Unique, stable ID strings ఇవ్వడానికి React Hook.

**❓ Why not use Math.random()?**
💡 Hydration mismatch అవుతుంది.

---

### 🟡 Intermediate Level

**❓ How does useId improve accessibility?**
💡 Labels మరియు inputs ని unique IDs తో link చేస్తుంది.

**❓ Can useId be used for list keys?**
💡 No. Keys data identity ఆధారంగా ఉండాలి.

---

### 🔥 Advanced Level

**❓ How does useId prevent hydration mismatches?**
💡 Server + Client రెండింటిలో ఒకే ID generate అవుతుంది.

**❓ Can useId be used outside components?**
💡 లేదు. Hooks ఎప్పుడూ components లోనే వాడాలి.

**❓ Multiple related IDs ఎలా create చేయాలి?**

```js
const id = useId();
const labelId = `${id}-label`;
const descId = `${id}-desc`;
```

---

If you'd like, I can also generate a downloadable **README.md file**.
