## 24. What are error boundaries in React for?

# 📘 React Error Boundaries — Full Guide

## ⭐ Introduction

Error Boundaries are **special React components** that catch JavaScript errors occurring in their child components **during rendering, lifecycle methods, or constructors**.
They prevent the entire React app from crashing and instead allow you to show a **fallback UI**, improving user experience and app resilience.

---

## 🔍 What Are Error Boundaries?

Error boundaries work like “try/catch” for React's UI rendering tree.

They catch errors that occur in:

* Rendering
* Lifecycle methods
* Constructors of child components

They **do NOT** catch errors from:
❌ Event handlers
❌ Async code (setTimeout, promises)
❌ Server-side rendering
❌ Error thrown inside Error Boundary itself

---

## 🧠 Syntax — How to Create an Error Boundary

Error boundaries must be **class components** because they rely on two special lifecycle methods:

### ✔ `static getDerivedStateFromError(error)`

Used to update the fallback UI.

### ✔ `componentDidCatch(error, info)`

Used to log the error.

---

## 🟢 Simple Example — Basic Error Boundary

```jsx
import React from 'react';

class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true };
  }

  componentDidCatch(error, info) {
    console.error('Error caught:', error, info);
  }

  render() {
    if (this.state.hasError) {
      return <h2>Something went wrong.</h2>;
    }

    return this.props.children;
  }
}

export default ErrorBoundary;
```

Use it like this:

```jsx
function App() {
  return (
    <ErrorBoundary>
      <BuggyComponent />
    </ErrorBoundary>
  );
}
```

---

## 🔥 Medium Example — Catching Errors from Child Components

```jsx
function BuggyComponent() {
  throw new Error('Oops! Component crashed');
  return <div>Normal UI</div>;
}
```

When rendered inside `<ErrorBoundary>`, the fallback UI appears instead of crashing the page.

---

## 🧩 Advanced Example — Custom Fallback UI Component

```jsx
function FallbackUI({ reset }) {
  return (
    <div style={{ padding: 20, background: '#ffeeee' }}>
      <h2>⚠️ Something broke!</h2>
      <button onClick={reset}>Try Again</button>
    </div>
  );
}

class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError() {
    return { hasError: true };
  }

  resetError = () => {
    this.setState({ hasError: false });
  };

  render() {
    if (this.state.hasError) {
      return <FallbackUI reset={this.resetError} />;
    }

    return this.props.children;
  }
}
```

✔ Allows retrying the failed component
✔ Useful for dashboards, forms, or lazy-loaded components

---

## 🏗 Real-World Use Case — Protecting Isolated Parts of UI

```jsx
function App() {
  return (
    <div>
      <Navbar />

      <ErrorBoundary>
        <UserProfile />
      </ErrorBoundary>

      <ErrorBoundary>
        <NewsFeed />
      </ErrorBoundary>

      <Footer />
    </div>
  );
}
```

✔ Each major component is isolated
✔ Only failing component hides, not entire app
✔ Improves stability in production

---

## 🎯 When Should You Use Error Boundaries?

Use them when you want to:

* ✔ Prevent UI crashes
* ✔ Display friendly fallback UI
* ✔ Log errors in production
* ✔ Isolate failing components (feeds, charts, forms)

---

## ❗ Mistakes to Avoid

❌ Expecting event handler errors to be caught
❌ Wrapping entire application (makes debugging harder)
❌ Not logging caught errors
❌ Writing fallback UI that itself contains errors

---

## ⚡ Best Practices

✔ Wrap only critical UI sections
✔ Use multiple error boundaries
✔ Provide meaningful fallback UI
✔ Log errors to monitoring tools (Sentry, Datadog)
✔ Keep error boundaries small and focused

---

## 🔧 Tricks

### 🔹 Trick 1: Combine Error Boundary with Lazy Loading

```jsx
<Suspense fallback={<Loader />}>
  <ErrorBoundary>
    <LazyLoadedChart />
  </ErrorBoundary>
</Suspense>
```

### 🔹 Trick 2: Trigger reset by changing key

```jsx
<ErrorBoundary key={userId}>
  <UserProfile userId={userId} />
</ErrorBoundary>
```

---

## 📝 Summary

* Error Boundaries catch render-time errors in child components
* Prevent entire app from crashing
* Provide fallback UI to users
* Do not catch async or event handler errors
* Essential for stable and scalable UI architectures

---

## 🎤 Interview Questions & Answers

### 🟢 Basic Level

**❓ What is an Error Boundary?**
💡 A special component that catches errors in its children and renders fallback UI.

**❓ Can functional components be error boundaries?**
💡 No, only class components can (as of now).

---

### 🟡 Intermediate Level

**❓ What lifecycle methods do error boundaries use?**
💡 `getDerivedStateFromError` and `componentDidCatch`.

**❓ Do error boundaries catch event handler errors?**
💡 No. Event handler errors must be handled with try/catch.

---

### 🔥 Advanced Level

**❓ How do error boundaries improve app stability?**
💡 They prevent a single broken component from crashing the whole UI.

**❓ How do you isolate errors for different parts of an app?**
💡 Use multiple error boundaries around independent components.

---
