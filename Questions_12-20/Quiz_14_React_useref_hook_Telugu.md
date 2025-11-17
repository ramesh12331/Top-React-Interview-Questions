# ⚛️ 14. React లో `useRef` Hook అంటే ఏమిటి? ఎప్పుడు ఉపయోగించాలి?

## 🧠 సులభమైన నిర్వచనం (Simple Definition)

React లో **`useRef` hook** అనేది ఒక **persistent reference** ను సృష్టించడానికి ఉపయోగిస్తారు.  
ఇది update చేసినప్పుడు **component re-render కాకుండా** ఉంటుంది.  
`useRef()` ఒక **mutable object** ను return చేస్తుంది — `{ current: value }`, ఇది **renders మధ్య కూడా values ని నిలుపుతుంది**.

✅ ఇది ముఖ్యంగా ఈ సందర్భాల్లో ఉపయోగిస్తారు:

* **DOM elements** ను access చేయడానికి (ఉదా: focus, scroll)
* **mutable values** (ఉదా: timers, previous values, counters) నిల్వ చేయడానికి
* **focus**, **scroll position**, లేదా **custom references** కోసం

---

## ⚙️ Syntax

```jsx
const ref = useRef(initialValue);
```

* `useRef()` ఒక object `{ current: initialValue }` ను return చేస్తుంది  
* `current` value ను read / modify చేయవచ్చు — కానీ **re-render చేయదు**

---

## 🧩 Example 1 — Input Field పై ఆటోమేటిక్ Focus

```jsx
import React, { useRef, useEffect } from 'react';

function FocusInput() {
  const inputRef = useRef(null);

  useEffect(() => {
    inputRef.current.focus(); // ✅ ఆటోమేటిక్‌గా focus అవుతుంది
  }, []);

  return <input ref={inputRef} type="text" placeholder="Focus on mount" />;
}

export default FocusInput;
```

### 🧠 ఎలా పనిచేస్తుంది:

* `useRef(null)` → `{ current: null }` సృష్టిస్తుంది  
* React input element ను `inputRef.current` కి assign చేస్తుంది  
* Mount సమయంలో `inputRef.current.focus()` ద్వారా input కు focus ఇస్తుంది

✅ `inputRef.current` మార్చినా re-render జరగదు.

---

## 🧩 Example 2 — Previous State నిల్వ చేయడం

```jsx
import React, { useRef, useEffect, useState } from 'react';

function PreviousValueExample() {
  const [count, setCount] = useState(0);
  const prevCount = useRef();

  useEffect(() => {
    prevCount.current = count; // ప్రతి render తరువాత పాత విలువను నిల్వ చేస్తుంది
  });

  return (
    <div>
      <p>Current Count: {count}</p>
      <p>Previous Count: {prevCount.current}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

🧠 **వివరణ:**  
`prevCount.current` గత render లోని `count` విలువను గుర్తుంచుకుంటుంది.  
దీని update వల్ల re-render జరగదు.

---

## 🧩 Example 3 — Timer Reference నిల్వ చేయడం

```jsx
import React, { useRef, useEffect, useState } from 'react';

function TimerExample() {
  const [seconds, setSeconds] = useState(0);
  const timerRef = useRef();

  useEffect(() => {
    timerRef.current = setInterval(() => setSeconds(s => s + 1), 1000);
    return () => clearInterval(timerRef.current);
  }, []);

  return <p>Timer: {seconds}s</p>;
}
```

✅ `timerRef` interval ID ని నిల్వ చేస్తుంది, తద్వారా unmount సమయంలో దాన్ని clear చేయవచ్చు.

---

## ⚡ ఎప్పుడు `useRef` వాడాలి?

**Use Cases:**

✔ **DOM elements** access చేయడానికి (focus, scroll, measure)  
✔ **Previous values** లేదా పాత state నిల్వ చేయడానికి  
✔ **Timers / WebSockets / Subscriptions** కోసం references ఉంచడానికి  
✔ **Mutable data** (state అవసరం లేనివి) నిల్వ చేయడానికి

---

## 🚫 Common Mistakes

| ❌ తప్పు | ⚠️ ఎందుకు తప్పు |
| -------- | ---------------- |
| `useRef` ను reactive state కోసం వాడటం | Re-render జరగదు — `useState` వాడాలి |
| Ref change అయినప్పుడు UI update అవుతుందని అనుకోవడం | React render లో ref changes పరిగణనలోకి తీసుకోదు |
| ఎక్కువ refs వాడి logic చేయడం | Code చదవటానికి కష్టం అవుతుంది — state/context వాడాలి |

---

## ✅ Best Practices

✔ **Non-UI data** కోసం మాత్రమే useRef వాడండి  
✔ DOM manipulation refs ద్వారా **తక్కువగా** చేయండి  
✔ UI updates అవసరమైతే `useState` వాడండి  
✔ Cleanup references (intervals, events) ను refs లో ఉంచండి

---

## 🎨 Visual Diagram — `useRef` ఎలా పనిచేస్తుంది

```
Render 1 → useRef() returns { current: initialValue }
Render 2 → useRef() returns SAME object

✅ ref.current మారినా re-render జరగదు
✅ React అదే object ను ప్రతి render లో వాడుతుంది
```

---

## 💡 గుర్తుంచుకోవాల్సిన Trick

> 🧩 “`useState` మారితే re-render అవుతుంది, కానీ `useRef` మారితే re-render కాదు.”

| Hook | Re-render Trigger అవుతుందా? | Values Persist అవుతాయా? | ఉపయోగం |
| ----- | ---------------------------- | ------------------------- | -------- |
| `useState` | ✅ అవును | ✅ అవును | Reactive UI updates |
| `useRef` | ❌ కాదు | ✅ అవును | DOM access, Mutable data |

---

## 💬 Interview Questions

**Q1:** Input ఆటోమేటిక్‌గా focus కావాలంటే ఎలా?  
👉 `useRef` వాడి, `inputRef.current.focus()` ను `useEffect` లో కాల్ చేయాలి.

**Q2:** గత render లోని విలువలను ఎలా గుర్తుంచుకోవాలి?  
👉 `useRef` ద్వారా previous state నిల్వ చేయాలి.

**Q3:** Timer లేదా interval handle ఎలా నిల్వ చేయాలి?  
👉 `useRef` లో interval ID ఉంచాలి, తద్వారా cleanup చేయవచ్చు.

---

## 🧾 Short Interview Summary

> “`useRef` ఒక persistent object ను సృష్టిస్తుంది, ఇది renders మధ్య `.current` value ను నిలుపుతుంది.  
> ఇది re-render చేయకుండా DOM elements, timers లేదా పాత state values ను నిల్వ చేయడానికి వాడతారు.”

---

## ⚡ One-Line Trick

> 🧠 “`useState` re-render చేస్తుంది, కానీ `useRef` గుర్తుంచుకుంటుంది.”

---

**Author:** *Mamidi Ramesh*  
**Topic:** React Hooks — `useRef` Hook  
**Category:** Frontend / React.js  
