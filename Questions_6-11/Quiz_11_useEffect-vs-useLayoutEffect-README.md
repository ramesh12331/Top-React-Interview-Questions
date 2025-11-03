# 11. What is the difference between useEffect and useLayoutEffect in React?

# 🧩 useEffect vs useLayoutEffect in React

Both **`useEffect`** and **`useLayoutEffect`** are React Hooks used for performing **side effects** — actions that happen outside the normal rendering flow, such as:

- Fetching data  
- DOM manipulation  
- Subscribing/unsubscribing to events  

The **main difference** between them is **when** they run in the component lifecycle.

---

## ⚙️ Execution Timing Difference

| Aspect | useEffect | useLayoutEffect |
|:-------|:-----------|:----------------|
| **When it runs** | After the component renders and the browser paints the screen | After React updates the DOM but **before** the browser paints |
| **Render blocking** | ❌ Non-blocking (asynchronous) | ✅ Blocking (synchronous) — delays paint until it finishes |
| **Use cases** | Data fetching, event listeners, async tasks | DOM measurement, layout synchronization, animations |
| **Performance** | Better for most cases | Heavier, can cause jank if misused |

---

## 🧠 Conceptual Understanding

Imagine React as a **painter 🧑‍🎨**:

- **`useEffect`** → runs *after* the painting is complete (screen is visible).  
- **`useLayoutEffect`** → runs *right before* the painting — giving you a chance to adjust the layout before the user sees it.

---

## 🧩 Example: Understanding the Timing

```jsx
import React, { useEffect, useLayoutEffect, useRef, useState } from "react";

function Example() {
  const [width, setWidth] = useState(0);
  const boxRef = useRef();

  // Runs BEFORE paint
  useLayoutEffect(() => {
    console.log("🧱 useLayoutEffect: Measuring layout...");
    setWidth(boxRef.current.offsetWidth);
  }, []);

  // Runs AFTER paint
  useEffect(() => {
    console.log("🎨 useEffect: UI already painted, width:", width);
  }, [width]);

  return (
    <div
      ref={boxRef}
      style={{
        width: "50%",
        height: "100px",
        background: "lightblue",
        margin: "20px",
      }}
    >
      Width: {width}px
    </div>
  );
}

export default Example;
```

---

## 🔍 Step-by-step what happens

1️⃣ React renders the `<div>` → DOM is updated.  
2️⃣ `useLayoutEffect` runs → measures `offsetWidth` (before paint).  
3️⃣ It updates `width` state → triggers a re-render.  
4️⃣ Browser paints the final UI (with width shown).  
5️⃣ `useEffect` runs → logs info **after** the paint.

---

## 🚫 Common Mistakes

| Mistake | Why It’s Wrong |
|:--------|:----------------|
| Using `useLayoutEffect` for API calls | Blocks rendering unnecessarily — poor UX |
| Forgetting cleanup in effects | Can cause memory leaks |
| Assuming both behave the same | `useLayoutEffect` delays painting; use carefully |

---

## 💡 Best Practices

✅ Default to **`useEffect`** — it's asynchronous and improves performance.  
✅ Use **`useLayoutEffect`** only when:
- You need to measure or adjust the DOM before the screen is visible.
- You’re working with animations, scroll positions, or layout calculations.  

✅ Always clean up side effects:

```jsx
useEffect(() => {
  const handleResize = () => console.log("Resized");
  window.addEventListener("resize", handleResize);
  return () => window.removeEventListener("resize", handleResize);
}, []);
```

---

## 🧩 Interview Scenarios

**🗣 Scenario 1:**  
> “Why does my component flicker when I measure DOM using `useEffect`?”  
👉 Because `useEffect` runs **after paint** — layout changes appear after the user sees the UI.  
Use **`useLayoutEffect`** instead.

---

**🗣 Scenario 2:**  
> “When should I *not* use `useLayoutEffect`?”  
👉 When the side effect doesn’t affect layout (e.g., API calls, logging, subscriptions).

---

**🗣 Scenario 3:**  
> “Why is `useLayoutEffect` blocking?”  
👉 It waits for all DOM updates and layout reads to complete before painting — ensuring the user sees a consistent layout.

---

## 🧾 Summary

| Hook | Runs | Use for | Blocking? |
|:------|:------|:--------|:----------|
| **useEffect** | After paint | Data fetching, API calls, logging, subscriptions | ❌ No |
| **useLayoutEffect** | Before paint | DOM measurement, sync layout, animations | ✅ Yes |

---

## 💬 In Short

> 🧩 **Use `useEffect`** for side effects **after rendering**.  
> 🧱 **Use `useLayoutEffect`** for DOM-dependent logic **before painting** to avoid flicker.

---

**Author:** _Mamidi Ramesh_  
**Topic:** React Hooks — `useEffect` vs `useLayoutEffect`  
**Category:** Frontend / React.js
