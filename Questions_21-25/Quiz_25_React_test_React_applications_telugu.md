# 📘 Testing React Applications — Full Guide

## ⭐ Introduction

Testing ensures React components behave correctly and reliably. ఇది bugs ను ముందుగానే గుర్తించడంలో, కోడ్ maintainability పెంచడంలో, మరియు కొత్త features చేరుస్తున్నప్పుడు confidence ఇవ్వడంలో కీలకమైనది.

React applications ఎక్కువగా ఈ tools తో test చేస్తారు:

* **Jest** → Test runner + assertion library
* **React Testing Library (RTL)** → User perspective నుండి UI behavior ని test చేస్తుంది
* **Cypress / Playwright** → End-to-End (E2E) browser testing tools

---

## 🔍 What Does Testing Mean in React?

Testing verifies that components:

* సరైనగా render అవుతున్నాయా?
* User interactions సరిగ్గా handle చేస్తున్నాయా?
* State updates సరిగ్గా జరుగుతున్నాయా?
* APIs తో communication expected విధంగా ఉందా?

React Testing Library **implementation details కాకుండా real behavior** ని test చేయమని సూచిస్తుంది.

---

## 🧠 Types of Tests

### 1️⃣ Unit Tests

ఒక component లేదా function ను isolated గా test చేయడం.

### 2️⃣ Integration Tests

Multiple components కలిసి ఎలా పనిచేస్తున్నాయో test చేయడం.

### 3️⃣ End-to-End (E2E) Tests

Browser లో మొత్తం user flow ని test చేయడం (Cypress / Playwright తో).

---

## 🟢 Simple Example — Unit Test with RTL

```jsx
import { render, screen, fireEvent } from '@testing-library/react';
import Counter from './Counter';

test('increments count on button click', () => {
  render(<Counter />);

  const button = screen.getByText('Increment');

  fireEvent.click(button);

  expect(screen.getByText('Count: 1')).toBeInTheDocument();
});
```

✔ User clicks simulate చేస్తుంది
✔ Output ఆధారంగా test చేస్తుంది, internals కాదు

---

## 🔥 Medium Example — Testing Form Submission

```jsx
render(<LoginForm />);

fireEvent.change(screen.getByPlaceholderText('Username'), {
  target: { value: 'Ramesh' }
});

fireEvent.click(screen.getByText('Submit'));

expect(screen.getByText('Welcome Ramesh')).toBeInTheDocument();
```

✔ Controlled inputs test చేయడానికి perfect

---

## 🧩 Advanced Example — Mocking API Calls

```jsx
jest.mock('axios');

axios.get.mockResolvedValue({ data: { name: 'Ramesh' } });

render(<UserProfile />);

await waitFor(() => expect(screen.getByText('Ramesh')).toBeInTheDocument());
```

✔ Async API behavior ని టెస్ట్ చేయడానికి ఉత్తమ మార్గం

---

## 🧪 E2E Testing (Cypress Example)

```js
describe('Login Flow', () => {
  it('logs in successfully', () => {
    cy.visit('/login');
    cy.get('input[name=username]').type('admin');
    cy.get('input[name=password]').type('12345');
    cy.get('button').click();
    cy.contains('Dashboard').should('be.visible');
  });
});
```

✔ Browser లో full workflow ని test చేస్తుంది

---

## 🎯 When Should You Test?

* Feature release ముందు
* Bug fix చేసిన తర్వాత
* Refactoring లో
* కొత్త components చేరుస్తున్నప్పుడు

---

## ❗ Mistakes to Avoid

❌ Implementation details ని test చేయడం
❌ Over-mocking
❌ Mock cleanup మర్చిపోవడం
❌ చిన్న UI changes వల్ల tests break అయ్యే brittle tests

---

## ⚡ Best Practices

✔ User ఎలా interact చేస్తాడో దానికి అనుగుణంగా test చేయాలి
✔ Tests independent గా ఉంచాలి
✔ `beforeEach`/`afterEach` తో mocks reset చేయాలి
✔ `getByRole` లాంటి accessible queries వాడాలి
✔ CSS selectors avoid చేయాలి

---

## 🔧 Tricks

* `jest.spyOn()` తో function calls track చేయవచ్చు
* `jest.mock()` తో API calls mock చేయవచ్చు
* Accessibility-based queries వాడండి (`getByRole()`)
* Factories తో reusable test data తయారు చేయండి

---

## 📝 Summary

* React apps ని test చేయడానికి Jest + RTL ప్రధాన combo
* Tests మూడు రకాలుగా ఉంటాయి: Unit, Integration, E2E
* User experience మీద tests focus అవ్వాలి
* Tests stability, maintainability, confidence పెంచుతాయి

---

## 🎤 Interview Questions

### 🟢 Basic

**❓ React apps ను test చేయడానికి ఏ tools వాడతారు?**
→ Jest + React Testing Library.

**❓ Testing purpose ఏమిటి?**
→ Components సరిగ్గా work అవుతున్నాయని verify చేయడం.

---

### 🟡 Intermediate

**❓ Unit test మరియు Integration test లో తేడా?**
→ Unit → ఒక్క component; Integration → కలిసి పనిచేసే components.

**❓ Implementation details ఎందుకు test చేయకూడదు?**
→ UI చిన్న changes చేసినా tests break అవుతాయి.

---

### 🔥 Advanced

**❓ Async behavior ని ఎలా test చేస్తారు?**
→ Mocks + `waitFor` వాడి.

**❓ Login వంటి complex flows ఎలా test చేయాలి?**
→ Cypress / Playwright తో E2E testing.
