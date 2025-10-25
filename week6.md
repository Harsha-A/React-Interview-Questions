# 🗓️ Week 6: Advanced Patterns & Performance Profiling (HOC, Render Props, Compound Components)

### 🎯 Goal:

Master advanced React component design patterns to write reusable, scalable, and maintainable UI components. Learn performance profiling techniques for production-grade React apps.

---

## 📚 Topics:

1. **Higher-Order Components (HOC)**
2. **Render Props Pattern**
3. **Compound Components**
4. **Custom Hooks for Abstraction**
5. **Performance Profiling in Production**
6. **Optimizing React Context & Re-renders**
7. **Real-world Architecture Patterns**

---

## 🧠 Deep Dive & Examples

### 1. Higher-Order Components (HOC)

A **Higher-Order Component** is a function that takes a component and returns an enhanced version of it.

💡 **Example:**

```jsx
function withLogger(WrappedComponent) {
  return function EnhancedComponent(props) {
    useEffect(() => {
      console.log('Component Mounted:', WrappedComponent.name);
    }, []);
    return <WrappedComponent {...props} />;
  };
}

function Button({ label }) {
  return <button>{label}</button>;
}

const LoggedButton = withLogger(Button);
```

✅ Use HOCs for **cross-cutting concerns** like logging, analytics, or permissions.

⚠️ Avoid deep nesting — prefer hooks for modern React.

---

### 2. Render Props Pattern

The **Render Props** pattern allows components to share logic using a function as a prop.

💡 **Example:**

```jsx
function MouseTracker({ render }) {
  const [pos, setPos] = useState({ x: 0, y: 0 });
  return (
    <div onMouseMove={(e) => setPos({ x: e.clientX, y: e.clientY })}>
      {render(pos)}
    </div>
  );
}

export default function App() {
  return (
    <MouseTracker render={({ x, y }) => <p>Mouse at ({x}, {y})</p>} />
  );
}
```

✅ Useful for reusing logic across unrelated components.

---

### 3. Compound Components

A **Compound Component** is a group of components that share implicit state through context, behaving as a single unit.

💡 **Example:**

```jsx
const TabsContext = createContext();

function Tabs({ children }) {
  const [active, setActive] = useState(0);
  return (
    <TabsContext.Provider value={{ active, setActive }}>
      <div>{children}</div>
    </TabsContext.Provider>
  );
}

Tabs.TabList = function TabList({ children }) {
  return <div className="tab-list">{children}</div>;
};

Tabs.Tab = function Tab({ index, children }) {
  const { active, setActive } = useContext(TabsContext);
  return (
    <button onClick={() => setActive(index)}
      className={active === index ? 'active' : ''}>{children}</button>
  );
};

Tabs.Panel = function Panel({ index, children }) {
  const { active } = useContext(TabsContext);
  return active === index ? <div>{children}</div> : null;
};

export default function App() {
  return (
    <Tabs>
      <Tabs.TabList>
        <Tabs.Tab index={0}>Home</Tabs.Tab>
        <Tabs.Tab index={1}>Profile</Tabs.Tab>
      </Tabs.TabList>
      <Tabs.Panel index={0}>🏠 Welcome Home</Tabs.Panel>
      <Tabs.Panel index={1}>👤 User Profile</Tabs.Panel>
    </Tabs>
  );
}
```

✅ Provides flexibility + encapsulation — common in UI libraries like Radix UI.

---

### 4. Custom Hooks for Abstraction

Encapsulate logic shared by multiple components.

💡 **Example:**

```jsx
function useFetch(url) {
  const [data, setData] = useState(null);
  useEffect(() => {
    fetch(url).then(res => res.json()).then(setData);
  }, [url]);
  return data;
}

function User() {
  const user = useFetch('/api/user');
  if (!user) return <p>Loading...</p>;
  return <p>Hello, {user.name}</p>;
}
```

✅ Encourages clean separation of logic and UI.

---

### 5. Performance Profiling in Production

Use **React Profiler API** or **Chrome DevTools Performance tab** to identify bottlenecks.

* Record CPU flame chart.
* Identify wasted renders.
* Optimize via memoization, code splitting, and lazy loading.

💡 **React Profiler Example:**

```jsx
import { Profiler } from 'react';

function onRender(id, phase, actualDuration) {
  console.log(`${id} rendered in ${actualDuration}ms`);
}

<Profiler id="App" onRender={onRender}>
  <App />
</Profiler>
```

✅ Helps detect performance issues dynamically.

---

### 6. Optimizing React Context & Re-renders

* Split large contexts into smaller ones.
* Memoize provider values.
* Avoid unnecessary re-renders.

💡 **Example:**

```jsx
const ThemeContext = createContext();

export function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');
  const value = useMemo(() => ({ theme, setTheme }), [theme]);
  return <ThemeContext.Provider value={value}>{children}</ThemeContext.Provider>;
}
```

✅ Prevents all consumers from re-rendering unnecessarily.

---

### 7. Real-World Architecture Patterns

* Use **Provider Pattern** for shared state (Auth, Theme, Config).
* Apply **Render Props or Custom Hooks** for reusable logic.
* Keep components small, composable, and focused.

---

## 💻 Assignments

### 🎯 Task 1:

Create a `withLogger` HOC that logs prop changes in a wrapped component.

### 🎯 Task 2:

Build a `MouseTracker` app using the Render Props pattern.

### 🎯 Task 3:

Implement a **Tabs Component** using the Compound Components pattern.

### 🎯 Task 4:

Profile an app using React Profiler and optimize re-renders.

---

## 🧩 Bonus Challenge

* Convert an HOC-based component into a Custom Hook version.
* Measure improvement in readability and performance.

---

## ✅ Summary

* HOCs and Render Props → code reuse patterns.
* Compound Components → powerful, flexible UI APIs.
* Custom Hooks → modern abstraction.
* Profiling and memoization → ensure scalability.
