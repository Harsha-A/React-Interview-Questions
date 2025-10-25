# 🗓️ Week 4: React Performance Optimization

### 🎯 Goal:

Learn how to analyze and improve React app performance using built-in tools, React.memo, hooks optimization, and lazy loading.

---

## 📚 Topics:

1. **React Re-rendering Basics**
2. **React.memo and Pure Components**
3. **useMemo and useCallback Hooks**
4. **Code Splitting & Lazy Loading (React.lazy & Suspense)**
5. **React Profiler and DevTools Optimization**
6. **Avoiding Expensive Computations and Inline Functions**
7. **Virtualization (React-Window / React-Virtualized)**

---

## 🧠 Deep Dive & Examples

### 1. React Re-rendering Basics

Every time a state or prop changes, React re-renders the component tree. Over-rendering leads to performance degradation.

💡 **Example:**

```jsx
function Child({ value }) {
  console.log('Child rendered');
  return <p>Value: {value}</p>;
}

export default function Parent() {
  const [count, setCount] = useState(0);
  const [text, setText] = useState('Hello');

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <button onClick={() => setText(text + '!')}>Change Text</button>
      <Child value={text} />
    </div>
  );
}
```

Even though `Child` only depends on `text`, it re-renders whenever `count` changes.

---

### 2. React.memo

`React.memo` prevents re-rendering unless props change.

```jsx
const Child = React.memo(function Child({ value }) {
  console.log('Child rendered');
  return <p>Value: {value}</p>;
});
```

Now `Child` won’t re-render when `count` updates.

✅ Use for **pure functional components** with stable props.

---

### 3. useCallback & useMemo

`useCallback` memoizes functions. `useMemo` memoizes computed values.

```jsx
const increment = useCallback(() => setCount(c => c + 1), []);
const expensiveValue = useMemo(() => computeExpensive(data), [data]);
```

These prevent new function or object instances that trigger re-renders.

---

### 4. Code Splitting & Lazy Loading

React supports dynamic imports via `React.lazy()` and `Suspense`.

```jsx
const HeavyComponent = React.lazy(() => import('./HeavyComponent'));

export default function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <HeavyComponent />
    </Suspense>
  );
}
```

✅ Loads components **only when needed** — improves initial load time.

---

### 5. React Profiler & DevTools

Use **React DevTools → Profiler tab** to:

* Identify slow components
* Measure render times
* Visualize re-renders

📍 Run your app in development mode and use Chrome DevTools to record a profile session.

---

### 6. Avoiding Inline Functions & Objects

Inline functions and object literals recreate new references on every render.

❌ Bad:

```jsx
<Component onClick={() => handleClick(id)} />
```

✅ Good:

```jsx
const handleClickWithId = useCallback(() => handleClick(id), [id]);
<Component onClick={handleClickWithId} />;
```

---

### 7. Virtualization

For large lists, use **react-window** or **react-virtualized**.

```jsx
import { FixedSizeList as List } from 'react-window';

const Row = ({ index, style }) => <div style={style}>Row {index}</div>;

export default function App() {
  return <List height={200} itemCount={1000} itemSize={35} width={300}>{Row}</List>;
}
```

✅ Renders only visible items → massive performance gains.

---

## 💻 Assignment

### 🎯 Task 1:

Optimize an app with nested components and frequent renders.

* Use `React.memo`, `useCallback`, and `useMemo`.
* Profile before/after changes using React Profiler.

### 🎯 Task 2:

Lazy load components using `React.lazy()` and `Suspense`.

### 🎯 Task 3:

Render a list of 1000+ items using **react-window** to avoid performance issues.

---

## 🧩 Bonus Challenge

* Implement a **Theme Switcher App** that:

  * Uses Context for theme management.
  * Memoizes theme provider to avoid re-renders.
  * Lazy-loads the theme settings page.

---

## ✅ Summary

* Use `React.memo` for pure components.
* Use `useCallback`/`useMemo` to prevent unnecessary recalculations.
* Use `React.lazy` for code splitting.
* Use virtualization for rendering large lists.
* Profile performance regularly.

