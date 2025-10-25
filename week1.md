# 🗓️ Week 1: React Fundamentals & Hooks (Core)

## 🎯 Goal

Understand how **React works under the hood** and master **Hooks** like `useState`, `useEffect`, `useRef`, `useMemo`, and `useCallback`.

---

## ⚙️ 1. Virtual DOM & Reconciliation

### 🔹 What is the Virtual DOM?

* The **Virtual DOM (VDOM)** is a lightweight copy of the **real DOM**.
* When React components render, they create a **virtual tree of UI elements**.
* On each state or prop change, React:

  1. Creates a **new Virtual DOM**.
  2. **Diffs** it with the previous one (Reconciliation Algorithm).
  3. Updates **only changed parts** in the real DOM.

### 🔹 Why it matters:

* Real DOM updates are slow.
* Virtual DOM minimizes updates for better performance.

### 💡 Example

```jsx
import { useState } from "react";

export default function VDomExample() {
  const [text, setText] = useState("Hello");

  return (
    <div>
      <h1>{text}</h1>
      <button onClick={() => setText("Hello React!")}> 
        Update Text
      </button>
    </div>
  );
}
```

🧠 React re-renders only the `<h1>` when `text` changes — not the entire DOM.

---

## ⚙️ 2. JSX and Babel

### 🔹 JSX (JavaScript XML)

JSX lets you write HTML-like syntax in JavaScript.

```jsx
const element = <h1>Hello, JSX!</h1>;
```

Internally transformed to:

```jsx
const element = React.createElement("h1", null, "Hello, JSX!");
```

### 🔹 Babel

* **Babel** compiles modern JavaScript (and JSX) into browser-compatible code.
* Tools like **Vite** or **Create React App** use Babel under the hood.

---

## ⚙️ 3. Component Lifecycle (Before Hooks)

React previously used **class components** and lifecycle methods:

| Phase      | Method                   | Description                      |
| ---------- | ------------------------ | -------------------------------- |
| Mounting   | `componentDidMount()`    | Runs after component mounts      |
| Updating   | `componentDidUpdate()`   | Runs after state/prop update     |
| Unmounting | `componentWillUnmount()` | Runs before component is removed |

### 💡 Example (Class-based)

```jsx
class Timer extends React.Component {
  state = { seconds: 0 };

  componentDidMount() {
    this.timer = setInterval(() => {
      this.setState({ seconds: this.state.seconds + 1 });
    }, 1000);
  }

  componentWillUnmount() {
    clearInterval(this.timer);
  }

  render() {
    return <h2>Time: {this.state.seconds}</h2>;
  }
}
```

➡️ Modern React replaces this with **Hooks**.

---

## ⚙️ 4. React Hooks Deep Dive

### 🪝 `useState`

Adds **state** to a functional component.

```jsx
import { useState } from "react";

export default function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>+</button>
      <h2>{count}</h2>
      <button onClick={() => setCount(count - 1)}>-</button>
    </div>
  );
}
```

🧠 `useState()` returns a state variable and a function to update it.

---

### 🪝 `useEffect`

Handles **side effects** like API calls, subscriptions, DOM updates, etc.

```jsx
import { useState, useEffect } from "react";

export default function TitleUpdater() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `Count: ${count}`;
  }, [count]);

  return <button onClick={() => setCount(count + 1)}>Count is {count}</button>;
}
```

🧩 `useEffect` runs **after render**.
The `[count]` dependency ensures it only runs when `count` changes.
Return a cleanup function to remove listeners or timers.

---

### 🪝 `useRef`

Access **DOM elements** or store **mutable values** without triggering re-renders.

```jsx
import { useRef } from "react";

export default function InputFocus() {
  const inputRef = useRef();

  const handleFocus = () => inputRef.current.focus();

  return (
    <div>
      <input ref={inputRef} type="text" placeholder="Click button to focus" />
      <button onClick={handleFocus}>Focus Input</button>
    </div>
  );
}
```

---

### 🪝 `useMemo`

Memoizes **expensive computations** so they don’t run on every render.

```jsx
import { useMemo, useState } from "react";

export default function ExpensiveComputation() {
  const [num, setNum] = useState(0);

  const double = useMemo(() => {
    console.log("Computing...");
    return num * 2;
  }, [num]);

  return (
    <>
      <h3>Double: {double}</h3>
      <button onClick={() => setNum(num + 1)}>Increase</button>
    </>
  );
}
```

🧠 `useMemo` runs only when dependencies change.

---

### 🪝 `useCallback`

Memoizes functions so they don’t get recreated on every render.

```jsx
import { useState, useCallback } from "react";

export default function CallbackExample() {
  const [count, setCount] = useState(0);

  const increment = useCallback(() => setCount((c) => c + 1), []);

  return <button onClick={increment}>Count: {count}</button>;
}
```

✅ Useful when passing callbacks to child components.

---

## ⚙️ 5. Controlled vs Uncontrolled Components

### 🔹 Controlled Components

React **controls the input value** through state.

```jsx
import { useState } from "react";

export default function ControlledInput() {
  const [value, setValue] = useState("");

  return (
    <input
      value={value}
      onChange={(e) => setValue(e.target.value)}
      placeholder="Controlled input"
    />
  );
}
```

---

### 🔹 Uncontrolled Components

The **DOM handles** the input’s value instead of React.

```jsx
import { useRef } from "react";

export default function UncontrolledInput() {
  const inputRef = useRef();

  const handleSubmit = () => {
    alert(`Input Value: ${inputRef.current.value}`);
  };

  return (
    <>
      <input ref={inputRef} placeholder="Uncontrolled input" />
      <button onClick={handleSubmit}>Submit</button>
    </>
  );
}
```

---

## 💻 Assignment: Counter App

### 🎯 Requirements

✅ Increment / Decrement
✅ Persist count using `localStorage`
✅ Show last updated timestamp

### 🧩 Final Example

```jsx
import { useState, useEffect } from "react";

export default function CounterApp() {
  const [count, setCount] = useState(() => {
    return Number(localStorage.getItem("count")) || 0;
  });

  const [lastUpdated, setLastUpdated] = useState("");

  useEffect(() => {
    localStorage.setItem("count", count);
    setLastUpdated(new Date().toLocaleTimeString());
  }, [count]);

  return (
    <div style={{ textAlign: "center", marginTop: "50px" }}>
      <h1>Counter App</h1>
      <h2>Count: {count}</h2>
      <p>Last Updated: {lastUpdated}</p>

      <button onClick={() => setCount(count + 1)}>➕ Increment</button>
      <button onClick={() => setCount(count - 1)}>➖ Decrement</button>
      <button onClick={() => setCount(0)}>🔄 Reset</button>
    </div>
  );
}
```

---

## 🧠 Bonus Challenge

* [ ] Add **dark/light mode** toggle (save preference in `localStorage`).
* [ ] Show a **toast notification** when the count is saved (use `react-hot-toast` or similar).
* [ ] Add a **Reset Confirmation Modal** using `useState` and conditional rendering.

---

## 🧾 Summary

| Concept      | Key Idea                                     |
| ------------ | -------------------------------------------- |
| Virtual DOM  | Lightweight copy of real DOM for performance |
| JSX & Babel  | JSX → compiled to `React.createElement()`    |
| useState     | State management hook                        |
| useEffect    | Handles side effects and lifecycle           |
| useRef       | Access DOM or store mutable values           |
| useMemo      | Cache expensive computations                 |
| useCallback  | Cache function references                    |
| Controlled   | React manages input value                    |
| Uncontrolled | DOM manages input value                      |

---

📚 **Next Week → Week 2: Component Composition, Props Drilling & Context API**
