# 📘 React Error Boundaries — Full Guide

## ⭐ Introduction

Error Boundaries అనేవి React లోని ప్రత్యేకమైన components. ఇవి child components లో వచ్చే JavaScript errors ను **rendering సమయంలో**, **lifecycle methods లో**, లేదా **constructor లో** పట్టుకుని మొత్తం app crash అవకుండా fallback UI చూపిస్తాయి.

---

## 🔍 What Are Error Boundaries?

Error boundaries అనేవి React UI rendering tree కోసం “try/catch” లాంటివి.

ఇవి ఈ చోట్ల వచ్చే errors ను catch చేస్తాయి:

* Rendering సమయంలో
* Lifecycle methods లో
* Child components constructors లో

❌ ఇవి catch చేయవు:

* Event handlers
* Async code (setTimeout, promises)
* Server-side rendering
* Error boundary లోపలే throw అయ్యే errors

---

## 🧠 Syntax — Error Boundary తయారు చేసే విధానం

Error boundaries **class components** తో మాత్రమే పనిచేస్తాయి. ఎందుకంటే ఇవి రెండు ప్రత్యేక lifecycle methods మీద ఆధారపడుతాయి.

### ✔ `static getDerivedStateFromError(error)`

Fallback UI update చేయడానికి వాడతారు.

### ✔ `componentDidCatch(error, info)`

Error ని log చేయడానికి వాడతారు.

---

## 🟢 Simple Example — Basic Error Boundary

```jsx
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
```

ఉపయోగించడం:

```jsx
<ErrorBoundary>
  <BuggyComponent />
</ErrorBoundary>
```

---

## 🔥 Medium Example — Child Component Error Catching

```jsx
function BuggyComponent() {
  throw new Error('Oops! Component crashed');
  return <div>Normal UI</div>;
}
```

ErrorBoundary లో వేసినప్పుడు fallback UI చూపించి app crash ఆపుతుంది.

---

## 🧩 Advanced Example — Custom Fallback UI

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

✔ Retry button ఇవ్వవచ్చు
✔ Dashboards, forms, lazy-loaded components కోసం useful

---

## 🏗 Real-World Use Case — UI Sections ని Protect చేయడం

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

✔ ఒక్క component fail అయినా మొత్తం app crash కాదు

---

## 🎯 Error Boundaries ఎప్పుడు వాడాలి?

* UI crashes నివారించాలి
* Friendly fallback UI చూపించాలి
* Production errors log చేయాలి
* Sections ను isolate చేయాలి

---

## ❗ తప్పులు చేయకూడదు

❌ Event handler errors catch చేస్తాయని అనుకోవడం
❌ మొత్తం app ను ఒక పెద్ద boundary లో wrap చేయడం
❌ Logged errors ignore చేయడం
❌ Fallback UI లో errors ఉండడం

---

## ⚡ Best Practices

✔ Multiple error boundaries వాడండి
✔ Important UI sections మాత్రమే wrap చేయండి
✔ మంచి fallback UI ఇవ్వండి
✔ Errors ను monitoring tools కు పంపండి
✔ Boundaries simple గా ఉంచండి

---

## 🔧 Tricks

### 🔹 Trick 1: Lazy Loading తో కలిపి వాడడం

```jsx
<Suspense fallback={<Loader />}>
  <ErrorBoundary>
    <LazyLoadedChart />
  </ErrorBoundary>
</Suspense>
```

### 🔹 Trick 2: key మార్చితే boundary reset అవుతుంది

```jsx
<ErrorBoundary key={userId}>
  <UserProfile userId={userId} />
</ErrorBoundary>
```

---

## 📝 Summary

* Error Boundaries child components లోని render-time errors ని catch చేస్తాయి
* మొత్తం app crash కాకుండా fallback UI చూపిస్తాయి
* Async/event handler errors catch చేయవు
* Stable & scalable React apps కోసం చాలా అవసరం

---

## 🎤 Interview Q&A

### 🟢 Basic

**❓ Error Boundary అంటే ఏమిటి?**
→ Child components లో errors catch చేసి fallback UI చూపించే special component.

**❓ Functional components Error Boundaries అవుతాయా?**
→ కాదు, ఇప్పటికి class components మాత్రమే.

---

### 🟡 Intermediate

**❓ Error Boundaries ఏ lifecycle methods వాడతాయి?**
→ `getDerivedStateFromError` మరియు `componentDidCatch`.

**❓ Event handler errors catch అవుతాయా?**
→ కాదు.

---

### 🔥 Advanced

**❓ Error Boundaries app stability ఎలా improve చేస్తాయి?**
→ ఒక్క component error వల్ల మొత్తం UI crash కాకుండా isolate చేస్తాయి.

**❓ Different sections ను isolate చేయడానికి ఎలా వాడాలి?**
→ ప్రతి major section చుట్టూ separate Error Boundary పెట్టాలి.
