# ⚛️ 6. What is the consequence of using array indices as the value for key
in React?


# ⚛️ React Interview Q6 — Using Array Indices as Keys

## ✅ Question
**What happens if you use array indices as keys in React?**

---

## 🔍 Concept Recap
In React, **keys** help the Virtual DOM identify elements efficiently.  
If you use **array indices** (`0, 1, 2...`) as keys, React may mismatch elements when the list order changes — leading to unexpected UI behavior.

---

## ⚠️ Problem Example — Using Index as Key
```jsx
const items = ['Apple', 'Banana', 'Cherry'];

function FruitList() {
  return (
    <ul>
      {items.map((item, index) => (
        <li key={index}>{item}</li> // ❌ Bad: using index as key
      ))}
    </ul>
  );
}
```

### When the array changes order:
```js
['Banana', 'Apple', 'Cherry']
```
React still assumes:
- Index 0 → was "Apple", now "Banana"  
- Index 1 → was "Banana", now "Apple"

⚡ **Result:** React reuses wrong DOM nodes, causing:
- UI mismatches  
- Wrong input values  
- Incorrect animations

---

## 🧠 Example of a Bug
```jsx
const [fruits, setFruits] = useState(['Apple', 'Banana', 'Cherry']);

return (
  <ul>
    {fruits.map((fruit, index) => (
      <input
        key={index}
        value={fruit}
        onChange={e => {
          const newFruits = [...fruits];
          newFruits[index] = e.target.value;
          setFruits(newFruits);
        }}
      />
    ))}
  </ul>
);
```
If you insert a new fruit at the top, input values will appear in **wrong fields** 😱

---

## ✅ Correct Example — Use Unique IDs
```jsx
const fruits = [
  { id: 101, name: 'Apple' },
  { id: 102, name: 'Banana' },
  { id: 103, name: 'Cherry' }
];

function FruitList() {
  return (
    <ul>
      {fruits.map(fruit => (
        <li key={fruit.id}>{fruit.name}</li> // ✅ Stable and unique keys
      ))}
    </ul>
  );
}
```

Even if the order changes, React correctly matches elements to DOM nodes.

---

## 💡 Short Summary

| Concept | Explanation |
|----------|-------------|
| **What happens** | React may reuse or reorder DOM nodes incorrectly |
| **Why** | Array indices change when list order changes |
| **Effect** | UI bugs, wrong state, misplaced animations |
| **Best Practice** | Use unique stable keys (like IDs) |
| **When OK** | Only if the list is static and never changes |

---

## 🧠 Interview Tip
> "React uses keys to identify list items. Using array indices can cause incorrect UI updates if the list changes. Always use unique, stable IDs instead."
