# 📘 React — State ను Direct గా మార్చొద్దని ఎందుకు చెబుతుంది? (పూర్తి తెలుగు గైడ్)

## ⭐ పరిచయం

React లో state ను **direct గా mutate చేయొద్దు** అని strongly recommend చేస్తుంది. ఎందుకంటే React rendering system **reference changes** మీద ఆధారపడుతుంది.

State ను direct గా మార్చేస్తే:

* React కి మార్పు కనిపించదు
* UI update అవదు
* stale data కనిపిస్తుంది
* unpredictable bugs వస్తాయి
* performance కూడా తగ్గుతుంది

---

## 🔍 React ఎందుకు State Mutate చేయొద్దని Warning ఇస్తుంది?

React state changes ను **shallow comparison** ద్వారా detect చేస్తుంది:

* object లేదా array యొక్క **reference మారితే** → React rerender చేస్తుంది
* reference అదే ఉన్నా → React change లేదని అనుకుంటుంది

Direct mutation చేస్తే:

* reference మారదు ❌
* React rerender skip చేస్తుంది ❌
* UI పాత values నే చూపిస్తుంది ⚠️

---

## 🧠 Correct vs Incorrect Syntax

### ❌ Incorrect (Mutation)

```jsx
items.push(4); // ❌ array mutate చేస్తున్నారు
setItems(items); // ❌ same reference → React rerender కాదు
```

### ✅ Correct (Immutable Update)

```jsx
setItems(prev => [...prev, 4]);
```

✔ కొత్త array వస్తుంది
✔ React rerender అవుతుంది

---

## 🔥 Object Update — సరైన vs తప్పు

### ❌ Wrong

```jsx
user.name = "Ramesh"; // mutation
setUser(user); // ❌ same reference
```

### ✅ Right

```jsx
setUser(prev => ({ ...prev, name: "Ramesh" }));
```

✔ కొత్త object
✔ React కి change కనిపిస్తుంది

---

## 🎯 Mutating State వల్ల వచ్చే సమస్యలు

* UI update అవదు
* React.memo పనిచేయదు
* useEffect dependencies trigger కావు
* unpredictable behavior
* debugging కష్టమవుతుంది

---

## 🧩 Mutations ఎక్కువగా సమస్య ఇచ్చే సందర్భాలు

* Arrays: push, pop, splice, sort, reverse
* Objects: direct property update
* Nested లేదా deep state updates
* Memoized components
* useEffect dependency arrays

---

## 🔥 Nested State Example

### ❌ Mutation

```jsx
state.user.name = "B";
setState(state); // ❌ same reference
```

### ✅ Immutable Update

```jsx
setState(prev => ({
  ...prev,
  user: {
    ...prev.user,
    name: "B"
  }
}));
```

✔ nested object కి కూడా కొత్త reference

---

## 🏗 Array Immutable Updates

```jsx
// Add
setList(prev => [...prev, newItem]);

// Remove
setList(prev => prev.filter(i => i.id !== id));

// Update
setList(prev => prev.map(i => i.id === id ? { ...i, value: 100 } : i));
```

✔ ప్రతిసారి కొత్త array వస్తుంది

---

## ⚡ Real-world Example — React.memo Break అవుతుంది

```jsx
items.push(4); // ❌ mutation
setItems(items); // ❌ same reference
```

React.memo props reference మారితేనే rerender చేస్తుంది.

---

## ❗ తప్పులు చేయకూడదు

❌ push, pop, sort, splice వంటి mutating functions వాడడం
❌ Object properties direct గా మార్చడం
❌ Nested state update లో clone చేయకపోవడం
❌ Mutable & immutable patterns mix చేయడం
❌ React deep comparison చేస్తుందని అనుకోవడం

---

## ⚡ Best Practices

✔ ఎప్పుడూ కొత్త objects/arrays create చేయాలి
✔ spread operator (...), map, filter, reduce వాడాలి
✔ initialState separate గా పెట్టాలి
✔ Complex state కోసం useReducer వాడాలి
✔ Deep updates కోసం Immer.js బాగుంటుంది

---

## 🔧 Tricks

### 🔹 Trick 1: Development లో State Freeze

```jsx
Object.freeze(state);
```

Accidental mutations వెంటనే error ఇస్తుంది.

### 🔹 Trick 2: Immer.js

```jsx
setState(prev => produce(prev, draft => {
  draft.user.name = "Ramesh";
}));
```

### 🔹 Trick 3: Reducer లో ఎప్పుడూ కొత్త reference

```jsx
return { ...state, value: newValue };
```

---

## 📝 Summary

* React reference changes మీద rerender అవుతుంది
* Mutable updates → React detect చేయదు
* Always immutable updates వాడాలి
* Spread operator, map, filter, reducers వాడాలి
* Debugging, performance, memoization అన్నీ సరిగ్గా పనిచేస్తాయి

---

## 🎤 Interview Questions

### 🟢 Basic

**❓ State mutate చేయొద్దని React ఎందుకు అంటుంది?**
→ Reference change లేకపోతే React rerender చేయదు.

**❓ React state changes ఎలా detect చేస్తుంది?**
→ Shallow reference comparison.

---

### 🟡 Intermediate

**❓ Arrays/Object mutate చేస్తే ఏమవుతుంది?**
→ UI update కాదు.

**❓ Arrays immutably update చేసే విధాలు?**
→ [...prev], map, filter.

---

### 🔥 Advanced

**❓ Mutation వల్ల React.memo ఎందుకు break అవుతుంది?**
→ Props reference మారకపోతే rerender కాదు.

**❓ useReducer ఎప్పుడు వాడాలి?**
→ Nested లేదా complex state ఉన్నప్పుడు.

---

## 🏗 Component Level Example

### ❌ Incorrect

```jsx
todos.push("New Task");
setTodos(todos); // ❌ same reference
```

### ✅ Correct

```jsx
setTodos(prev => [...prev, "New Task"]);
```

---

## 🔍 Mutable vs Immutable

### 🔄 Mutable — Example

```jsx
// Mutable Object Example
const user = { name: "Ramesh", age: 25 };
user.age = 30; // ❌ same object మార్చబడింది
console.log(user); // { name: "Ramesh", age: 30 }

// Mutable Array Example
const nums = [1, 2, 3];
nums.push(4); // ❌ original array modify అయ్యింది
console.log(nums); // [1, 2, 3, 4]
```

### 🧊 Immutable — Example

```jsx
// Immutable Object Example
const user = { name: "Ramesh", age: 25 };
const updatedUser = { ...user, age: 30 }; // ✔ కొత్త object
console.log(updatedUser); // { name: "Ramesh", age: 30 }
console.log(user); // { name: "Ramesh", age: 25 }

// Immutable Array Example
const nums = [1, 2, 3];
const newNums = [...nums, 4]; // ✔ కొత్త array
console.log(newNums); // [1, 2, 3, 4]
console.log(nums); // [1, 2, 3]
```

### 🔄 Mutable

Original data directly మార్చబడుతుంది.

### 🧊 Immutable

Original data untouched → కొత్త copy generate అవుతుంది.

---

## 🧠 Immutability ఎందుకు ముఖ్యము?

* React new references మీద rerender అవుతుంది
* Mutable updates → UI update కాదు
* Immutable updates → Predictable, stable UI
* Memo, useEffect అన్నీ సరిగ్గా పనిచేస్తాయి
