# 🗓️ Week 2: Component Composition, Props Drilling & Context API

## 🎯 Goal

Master **React component communication and state sharing** across the app using **composition patterns**, and eliminate prop drilling with the **Context API**.

---

## 📚 Topics

### 1. Component Composition

Composition means **building complex UIs by combining smaller, reusable components**.
Instead of inheritance, React encourages using **composition** to pass components and behavior.

#### Example:

```jsx
function Card({ title, children }) {
  return (
    <div className="card">
      <h2>{title}</h2>
      <div>{children}</div>
    </div>
  );
}

export default function App() {
  return (
    <Card title="Profile">
      <p>This is inside the card!</p>
      <button>Click Me</button>
    </Card>
  );
}
```

🧠 **Explanation:**

* The `Card` component uses **composition** via `props.children` to render whatever is passed between the opening and closing tags.
* This allows flexible component design without tightly coupling behavior.

---

### 2. Props & Props Drilling

Props are how **data flows from parent → child** components.
But if you pass props through many intermediate components, it’s called **props drilling** — which makes code harder to maintain.

#### Example (Props Drilling):

```jsx
function GrandParent() {
  const user = { name: "Harsha" };
  return <Parent user={user} />;
}

function Parent({ user }) {
  return <Child user={user} />;
}

function Child({ user }) {
  return <p>Hello {user.name}</p>;
}
```

🧠 **Problem:**
Even though only `Child` needs `user`, it must be passed through `Parent`. This is where **Context API** helps.

---

### 3. Context API

The **Context API** provides a way to share data across the component tree without passing props manually.

#### Steps:

1. Create context using `createContext()`.
2. Wrap components with a **Provider**.
3. Consume context using `useContext()`.

#### Example:

```jsx
import { createContext, useContext, useState } from "react";

// 1️⃣ Create Context
const UserContext = createContext();

// 2️⃣ Provider Component
function UserProvider({ children }) {
  const [user, setUser] = useState({ name: "Harsha" });
  return (
    <UserContext.Provider value={{ user, setUser }}>
      {children}
    </UserContext.Provider>
  );
}

// 3️⃣ Consumer Component
function Profile() {
  const { user } = useContext(UserContext);
  return <h2>Hello {user.name}</h2>;
}

// 4️⃣ App Composition
export default function App() {
  return (
    <UserProvider>
      <Profile />
    </UserProvider>
  );
}
```

🧠 **Explanation:**

* `UserProvider` wraps the app and provides context.
* Any child (like `Profile`) can directly access context without prop drilling.

---

### 4. useContext Hook Deep Dive

`useContext(MyContext)` allows direct access to the nearest `Provider`’s value.

* ✅ React automatically re-renders components when context value changes.
* 🚫 Avoid overusing Context — it causes all consumers to re-render on value change.

#### Example (Theme Toggle):

```jsx
const ThemeContext = createContext();

function ThemeProvider({ children }) {
  const [theme, setTheme] = useState("light");
  const toggleTheme = () => setTheme(theme === "light" ? "dark" : "light");

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

function Button() {
  const { theme, toggleTheme } = useContext(ThemeContext);
  return (
    <button onClick={toggleTheme}>
      Current Theme: {theme}
    </button>
  );
}

export default function App() {
  return (
    <ThemeProvider>
      <Button />
    </ThemeProvider>
  );
}
```

---

### 5. Advanced Composition Patterns

#### a. **Render Props Pattern**

A technique to share logic between components using a function prop.

```jsx
function DataFetcher({ render }) {
  const [data, setData] = useState([]);
  useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/users")
      .then(res => res.json())
      .then(setData);
  }, []);

  return render(data);
}

export default function App() {
  return (
    <DataFetcher render={(data) => (
      <ul>{data.map(user => <li key={user.id}>{user.name}</li>)}</ul>
    )} />
  );
}
```

#### b. **Higher Order Components (HOCs)**

A function that takes a component and returns an enhanced component.

```jsx
function withLogger(WrappedComponent) {
  return function Enhanced(props) {
    console.log('Props received:', props);
    return <WrappedComponent {...props} />;
  };
}

function Hello({ name }) {
  return <h3>Hello, {name}</h3>;
}

const HelloWithLogger = withLogger(Hello);

export default function App() {
  return <HelloWithLogger name="Harsha" />;
}
```

🧠 **Tip:** Prefer hooks and composition over HOCs in modern React.

---

## 💻 Assignment

### 1. **Theme Toggle App**

* Use Context API to manage theme (light/dark).
* Store theme preference in `localStorage`.
* Apply CSS changes dynamically based on context.

### 2. **User Auth Context**

* Build a simple Auth Context with `login` and `logout` functions.
* Share user data (like name, email) globally.
* Display user info in any component using `useContext()`.

---

## 🧩 Bonus Challenge

* Create a reusable `Modal` component using **children** and **composition**.
* Make a custom hook `useTheme()` that internally uses the Context.

---

## ✅ Summary

| Concept            | Description                                  |
| ------------------ | -------------------------------------------- |
| Composition        | Building UIs from smaller components         |
| Props Drilling     | Passing props down multiple levels           |
| Context API        | Avoids prop drilling by sharing global state |
| useContext         | Hook to consume context values               |
| Render Props / HOC | Patterns to share logic                      |



Learn to manage global state using Redux Toolkit and Zustand for scalable apps.
