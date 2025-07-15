# React-Interview-Questions
React Interview Questions

Here's a list of **25 advanced React interview questions and answers** tailored for an **SDE-3 role** at **product-based companies**. These questions go beyond basics and test your **depth, design skills, performance optimizations**, and **React ecosystem expertise**.

---

### ✅ **1. What are React Fiber internals and how do they improve rendering?**

**Answer**: React Fiber is a complete rewrite of React's core algorithm. It allows incremental rendering, improves responsiveness, and enables features like error boundaries, suspense, and concurrent rendering. It splits the render work into units of work that can be paused, resumed, and aborted.

---

### ✅ **2. How does React handle reconciliation?**

**Answer**: Reconciliation is the process of comparing the new virtual DOM with the previous one to compute minimal DOM operations. React uses a heuristic algorithm: elements of different types lead to full replacement, and keyed elements help in tracking across renders.

---

### ✅ **3. What is the difference between controlled and uncontrolled components?**

**Answer**:

* **Controlled**: Form data is handled by React via `useState`.
* **Uncontrolled**: Form data is handled by the DOM (`useRef`).
* Controlled components are preferred for complex forms and validation.

---

### ✅ **4. What are React Hooks and how do they avoid class components?**

**Answer**: Hooks like `useState`, `useEffect`, `useRef`, etc., let you use state and lifecycle in functional components, avoiding classes. They encourage cleaner, reusable logic through custom hooks.

---

### ✅ **5. What are closures in React hooks and how can they lead to bugs?**

**Answer**: Closures can cause stale state issues in hooks like `useEffect` or `setInterval`. Use functional updates or refs to avoid capturing old values.

```js
setCount(prev => prev + 1); // preferred
```

---

### ✅ **6. Explain useCallback vs useMemo.**

**Answer**:

* `useCallback(fn, deps)` returns **memoized function**.
* `useMemo(factory, deps)` returns **memoized value**.
  Used to avoid unnecessary recalculations or re-renders.

---

### ✅ **7. Why is lifting state up sometimes a bad idea?**

**Answer**: Overusing lifting state up can make components tightly coupled and hard to manage. Prefer **context** or **state machines** (like XState or Zustand) for better separation of concerns.

---

### ✅ **8. What are render props and how do they compare with hooks?**

**Answer**: Render props are a pattern to share logic via a prop that is a function. Hooks are simpler and cleaner for logic sharing, avoiding deeply nested components and prop drilling.

---

### ✅ **9. What are Higher Order Components (HOCs)? Why are they less used today?**

**Answer**: HOCs are functions that take a component and return a new component. They're used for cross-cutting concerns (e.g., auth, theming). Hooks are more composable and avoid the wrapper hell of HOCs.

---

### ✅ **10. Explain the concept of React Context and when to use it.**

**Answer**: React Context provides a way to share values (like theme, user, locale) across the component tree without prop drilling. Avoid using for high-frequency state (causes unnecessary re-renders).

---

### ✅ **11. How does React batching work and how does it change in concurrent mode?**

**Answer**: React batches state updates in event handlers to optimize rendering. In concurrent mode, React batches async updates (e.g., `setTimeout`, fetch) too. `flushSync()` can be used to force synchronous updates.

---

### ✅ **12. What is the useLayoutEffect hook?**

**Answer**: `useLayoutEffect` is like `useEffect` but fires synchronously after all DOM mutations, useful for reading layout or measuring DOM nodes.

---

### ✅ **13. How does React key prop affect rendering?**

**Answer**: `key` helps React identify which items changed, are added, or removed. Wrong or missing keys can cause inefficient or incorrect DOM updates.

---

### ✅ **14. How do you optimize React performance in large apps?**

**Answer**:

* Use memoization: `React.memo`, `useMemo`, `useCallback`.
* Split code: `React.lazy`, `Suspense`, dynamic imports.
* Virtualize lists: `react-window`, `react-virtualized`.
* Avoid unnecessary re-renders with `shouldComponentUpdate` or `React.memo`.

---

### ✅ **15. What is Concurrent React? How is it different from legacy mode?**

**Answer**: Concurrent React allows React to pause work and come back later, prioritizing urgent tasks like user input. It improves responsiveness by making rendering interruptible.

---

### ✅ **16. What is Suspense and how is it used for data fetching?**

**Answer**: `Suspense` allows React to wait ("suspend") rendering until data is ready. It works well with concurrent mode. With libraries like **React Query**, **Relay**, or **SWR**, you can fetch data while showing fallback UIs.

---

### ✅ **17. How does error boundary work in React?**

**Answer**: Error boundaries are React components that catch JavaScript errors in their subtree. Use `componentDidCatch` or `static getDerivedStateFromError` in class components.

---

### ✅ **18. What is the difference between SSR, CSR, and SSG in React?**

**Answer**:

* **CSR (Client-Side Rendering)**: Everything runs in the browser.
* **SSR (Server-Side Rendering)**: HTML is rendered on the server (e.g., Next.js).
* **SSG (Static Site Generation)**: HTML is built at compile-time.
  Next.js supports all three.

---

### ✅ **19. How do you handle authentication in a React SPA?**

**Answer**:

* Store JWT tokens in HttpOnly cookies or localStorage (with caution).
* Use context or state management for user session.
* Protect routes using HOC or `PrivateRoute` component.

---

### ✅ **20. How to handle memory leaks in React?**

**Answer**: Clean up side effects in `useEffect` return function. Cancel subscriptions, abort fetches, and avoid updating state after unmount.

```js
useEffect(() => {
  const controller = new AbortController();
  fetch(url, { signal: controller.signal });
  return () => controller.abort();
}, []);
```

---

### ✅ **21. What are common anti-patterns in React development?**

**Answer**:

* Overuse of state in global context.
* Large monolithic components.
* Uncontrolled side effects inside render.
* Not memoizing callbacks or values.
* Frequent unnecessary re-renders.

---

### ✅ **22. What is hydration in React?**

**Answer**: Hydration is when React attaches event handlers to the existing server-rendered HTML during SSR. Important in frameworks like Next.js.

---

### ✅ **23. What is the purpose of React DevTools Profiler?**

**Answer**: It helps detect performance bottlenecks, expensive re-renders, and wasted renders. Can track components that took the most time to render.

---

### ✅ **24. How do you design a component library in React?**

**Answer**:

* Use TypeScript and Storybook for documentation.
* Build atomic components (Button, Input, Modal).
* Handle theme and style via CSS-in-JS or Tailwind.
* Ensure accessibility (a11y).
* Use Rollup or Vite to publish as NPM package.

---

### ✅ **25. How does React 18 change the game for high-performance UI?**

**Answer**:

* Introduces automatic batching, concurrent rendering, `startTransition`.
* Better `Suspense` support.
* Enhances performance via selective rendering.
* Enables React Server Components (RSC).

---

# React Hooks 

Absolutely! Here's a **detailed breakdown of all commonly used React hooks** from an **SDE-3 (senior engineer) point of view**, including advanced usage, optimization tips, and **code examples** that show **real-world relevance** (not just beginner counters).

---

## ✅ **1. `useState` – Local Component State**

### 🔍 Purpose:

Manages local state in functional components.

### 🧠 SDE-3 Insight:

* Avoid deep object mutation; always return new object/array.
* Use functional updates to avoid stale closure bugs.

### 🧪 Example:

```tsx
const UserForm = () => {
  const [user, setUser] = useState({ name: '', age: '' });

  const handleChange = (e) => {
    const { name, value } = e.target;
    setUser(prev => ({ ...prev, [name]: value })); // prevent mutation
  };

  return (
    <form>
      <input name="name" value={user.name} onChange={handleChange} />
      <input name="age" value={user.age} onChange={handleChange} />
    </form>
  );
};
```

---

## ✅ **2. `useEffect` – Side Effects**

### 🔍 Purpose:

Handles side effects like data fetching, subscriptions, etc.

### 🧠 SDE-3 Insight:

* Dependencies are crucial – misuse causes bugs or re-renders.
* Always clean up subscriptions or timers.

### 🧪 Example:

```tsx
useEffect(() => {
  const controller = new AbortController();

  fetch('/api/data', { signal: controller.signal })
    .then(res => res.json())
    .then(setData)
    .catch(err => {
      if (err.name !== 'AbortError') console.error(err);
    });

  return () => controller.abort(); // cleanup
}, []); // only run once on mount
```

---

## ✅ **3. `useContext` – Global Data Access**

### 🔍 Purpose:

Access shared global data like theme, auth, config, etc.

### 🧠 SDE-3 Insight:

* Avoid putting frequently changing data in context – causes re-renders.
* Split contexts (e.g., UserContext vs ThemeContext).

### 🧪 Example:

```tsx
const AuthContext = createContext();

export const useAuth = () => useContext(AuthContext);

const AuthProvider = ({ children }) => {
  const [user, setUser] = useState(null);
  return <AuthContext.Provider value={{ user, setUser }}>{children}</AuthContext.Provider>;
};
```

---

## ✅ **4. `useRef` – Mutable Container / DOM Access**

### 🔍 Purpose:

* Access DOM nodes.
* Store mutable values without causing re-renders.

### 🧠 SDE-3 Insight:

* Use for instance variables (e.g., timer IDs, websocket refs).
* Avoid overusing for state; prefer `useState` if you need reactivity.

### 🧪 Example:

```tsx
const Timer = () => {
  const intervalRef = useRef(null);

  useEffect(() => {
    intervalRef.current = setInterval(() => console.log('tick'), 1000);
    return () => clearInterval(intervalRef.current);
  }, []);

  return <div>Timer running...</div>;
};
```

---

## ✅ **5. `useCallback` – Memoize Functions**

### 🔍 Purpose:

Returns a memoized version of a callback to avoid unnecessary re-creations.

### 🧠 SDE-3 Insight:

* Use with memoized components that rely on function identity.
* Avoid premature optimization – profile first.

### 🧪 Example:

```tsx
const handleClick = useCallback(() => {
  console.log('Button clicked');
}, []); // Only created once
```

---

## ✅ **6. `useMemo` – Memoize Values**

### 🔍 Purpose:

Memoizes expensive computations.

### 🧠 SDE-3 Insight:

* Only use when computation is expensive or stable referential equality is required.

### 🧪 Example:

```tsx
const sortedData = useMemo(() => {
  return data.sort((a, b) => a.name.localeCompare(b.name));
}, [data]);
```

---

## ✅ **7. `useLayoutEffect` – Sync Side Effects Before Paint**

### 🔍 Purpose:

Same as `useEffect`, but runs **before** painting the screen.

### 🧠 SDE-3 Insight:

* Use when you need DOM measurements before UI is visible (e.g., modals, animations).

### 🧪 Example:

```tsx
useLayoutEffect(() => {
  const { height } = ref.current.getBoundingClientRect();
  setModalHeight(height);
}, []);
```

---

## ✅ **8. `useImperativeHandle` – Expose Instance Methods**

### 🔍 Purpose:

Used with `forwardRef` to expose specific methods/properties.

### 🧠 SDE-3 Insight:

* Useful in component libraries or public APIs (e.g., `.focus()` method on custom input).

### 🧪 Example:

```tsx
const Input = forwardRef((props, ref) => {
  const inputRef = useRef();
  useImperativeHandle(ref, () => ({
    focus: () => inputRef.current.focus(),
  }));
  return <input ref={inputRef} {...props} />;
});
```

---

## ✅ **9. `useReducer` – Complex State Logic**

### 🔍 Purpose:

Alternative to `useState` for complex or nested state logic.

### 🧠 SDE-3 Insight:

* Ideal for forms, undo/redo logic, state machines.
* Keeps state transitions predictable.

### 🧪 Example:

```tsx
const reducer = (state, action) => {
  switch (action.type) {
    case 'increment': return { count: state.count + 1 };
    case 'reset': return { count: 0 };
    default: return state;
  }
};

const [state, dispatch] = useReducer(reducer, { count: 0 });
```

---

## ✅ **10. `useId` – Unique ID for Accessibility**

### 🔍 Purpose:

Generates stable, unique IDs across renders, useful for forms and ARIA.

### 🧠 SDE-3 Insight:

* Avoid manually generating UUIDs in SSR/CSR mismatch scenarios.

### 🧪 Example:

```tsx
const id = useId();
return (
  <label htmlFor={`${id}-name`}>
    Name
    <input id={`${id}-name`} />
  </label>
);
```

---

## ✅ **11. `useTransition` – Non-blocking State Updates**

### 🔍 Purpose:

Prioritize user-interactive vs non-urgent updates.

### 🧠 SDE-3 Insight:

* Use with large lists, typeahead, expensive filtering.

### 🧪 Example:

```tsx
const [isPending, startTransition] = useTransition();

const handleChange = (e) => {
  const query = e.target.value;
  startTransition(() => {
    setFilteredItems(filterItems(query));
  });
};
```

---

## ✅ **12. `useDeferredValue` – Debounced Rendering**

### 🔍 Purpose:

Used to defer rendering of non-critical updates.

### 🧠 SDE-3 Insight:

* Great for responsive UIs with search, charts, etc.

### 🧪 Example:

```tsx
const deferredValue = useDeferredValue(searchTerm);
const filteredList = useMemo(() => expensiveFilter(deferredValue), [deferredValue]);
```

---

## ✅ **13. `useSyncExternalStore` – External State Subscription**

### 🔍 Purpose:

Subscribe to external stores (Redux, Zustand, etc.) with consistency in concurrent rendering.

### 🧠 SDE-3 Insight:

* Required for strict mode/concurrent mode safe external state.

### 🧪 Example:

```tsx
const getSnapshot = () => store.getState();
const subscribe = (cb) => store.subscribe(cb);
const state = useSyncExternalStore(subscribe, getSnapshot);
```

---

## ✅ **14. `useInsertionEffect` – Inject styles before layout**

### 🔍 Purpose:

Inject styles *before* layout to avoid FOUC (Flash of Unstyled Content).

### 🧠 SDE-3 Insight:

* Rarely needed, mostly for style-in-JS libraries like Emotion or styled-components.

---

## 🧠 Bonus: Custom Hook Pattern (Common SDE-3 Pattern)

### ✅ **`useDebounce` Custom Hook**

```tsx
function useDebounce(value, delay) {
  const [debounced, setDebounced] = useState(value);

  useEffect(() => {
    const timer = setTimeout(() => setDebounced(value), delay);
    return () => clearTimeout(timer);
  }, [value, delay]);

  return debounced;
}
```

---

### 📌 Summary Table

| Hook                   | Use Case                             | Advanced Note                   |
| ---------------------- | ------------------------------------ | ------------------------------- |
| `useState`             | Local component state                | Functional updates prevent bugs |
| `useEffect`            | Side effects, data fetching          | Clean up on unmount             |
| `useContext`           | Global data like auth or theme       | Avoid overuse                   |
| `useRef`               | DOM access, persistent values        | Doesn't cause re-renders        |
| `useCallback`          | Memoize functions                    | Useful with `React.memo`        |
| `useMemo`              | Memoize expensive calculations       | Avoid unnecessary re-renders    |
| `useLayoutEffect`      | Sync DOM read/write before paint     | Can block paint                 |
| `useImperativeHandle`  | Expose ref methods from components   | Works with `forwardRef`         |
| `useReducer`           | Complex state logic                  | Predictable state transitions   |
| `useId`                | Stable unique IDs for SSR-safe apps  | Good for forms                  |
| `useTransition`        | Defer non-urgent updates             | Boost UX                        |
| `useDeferredValue`     | Avoid blocking UI with slow updates  | For input-heavy UI              |
| `useSyncExternalStore` | Safe subscription to external stores | Used in Redux 5+                |
| `useInsertionEffect`   | Inject styles before layout          | Niche use                       |

---

Would you like this formatted as a PDF or blog article? I can also generate **interview-ready flashcards or mini-projects** using these hooks.

