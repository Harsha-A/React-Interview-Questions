# 🗓️ Week 7: React + TypeScript for Scalable Architecture

### 🎯 Goal:

Master building large-scale React applications using **TypeScript** for type safety, scalability, and maintainability. Learn best practices for folder structure, interfaces, and typing complex React patterns.

---

## 📚 Topics:

1. **Why TypeScript for React?**
2. **TypeScript Setup in Vite / CRA**
3. **Typing Props, State & Events**
4. **Using Interfaces & Types**
5. **Typing Hooks and Context**
6. **Typing HOCs, Render Props, and Compound Components**
7. **Folder Structure & Architecture Patterns**
8. **Best Practices for Large Apps**

---

## 🧠 Deep Dive & Examples

### 1. Why TypeScript for React?

TypeScript provides **static typing**, **auto-completion**, and **refactor safety**.

✅ Benefits:

* Fewer runtime errors
* Better IDE support
* Self-documenting code
* Scalable team collaboration

---

### 2. Setting Up TypeScript

For **Vite**:

```bash
npm create vite@latest my-app -- --template react-ts
```

For **CRA**:

```bash
npx create-react-app my-app --template typescript
```

✅ You’ll get `.tsx` files and `tsconfig.json` automatically.

---

### 3. Typing Props & State

💡 **Example:**

```tsx
type UserProps = {
  name: string;
  age?: number; // optional prop
};

export default function User({ name, age }: UserProps) {
  const [count, setCount] = useState<number>(0);
  return (
    <div>
      <p>{name} ({age ?? 'N/A'})</p>
      <button onClick={() => setCount(count + 1)}>Clicked {count} times</button>
    </div>
  );
}
```

✅ Explicit types reduce bugs in props and event handling.

---

### 4. Typing Events

💡 **Example:**

```tsx
function Form() {
  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    console.log(e.target.value);
  };
  const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
  };
  return (
    <form onSubmit={handleSubmit}>
      <input type="text" onChange={handleChange} />
    </form>
  );
}
```

✅ Use React’s built-in event types instead of `any`.

---

### 5. Typing Custom Hooks

💡 **Example:**

```tsx
function useFetch<T>(url: string): [T | null, boolean] {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch(url)
      .then(res => res.json())
      .then(json => setData(json))
      .finally(() => setLoading(false));
  }, [url]);

  return [data, loading];
}

function User() {
  const [data, loading] = useFetch<{ id: number; name: string }>('https://api.example.com/user');
  if (loading) return <p>Loading...</p>;
  return <p>{data?.name}</p>;
}
```

✅ Generics (`<T>`) make hooks reusable across different data types.

---

### 6. Typing Context

💡 **Example:**

```tsx
type ThemeContextType = {
  theme: 'light' | 'dark';
  toggleTheme: () => void;
};

const ThemeContext = createContext<ThemeContextType | undefined>(undefined);

export const ThemeProvider: React.FC<{ children: React.ReactNode }> = ({ children }) => {
  const [theme, setTheme] = useState<'light' | 'dark'>('light');
  const toggleTheme = () => setTheme(prev => (prev === 'light' ? 'dark' : 'light'));

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};

export const useTheme = () => {
  const ctx = useContext(ThemeContext);
  if (!ctx) throw new Error('useTheme must be used within ThemeProvider');
  return ctx;
};
```

✅ Always type Context value and create a safe custom hook wrapper.

---

### 7. Typing HOCs & Render Props

💡 **Example:**

```tsx
function withLogger<P>(Component: React.ComponentType<P>) {
  return (props: P) => {
    console.log('Props:', props);
    return <Component {...props} />;
  };
}

interface ButtonProps {
  label: string;
}
const Button: React.FC<ButtonProps> = ({ label }) => <button>{label}</button>;

const LoggedButton = withLogger(Button);
```

✅ `<P>` represents the generic prop type of the wrapped component.

---

### 8. Folder Structure for Scalable Apps

```
src/
 ┣ components/
 ┃ ┣ common/
 ┃ ┣ layout/
 ┣ hooks/
 ┣ context/
 ┣ types/
 ┣ utils/
 ┣ pages/
 ┣ services/
 ┗ App.tsx
```

✅ Keep UI, logic, and data concerns separated.

---

## 💻 Assignments

### 🎯 Task 1:

Refactor an existing JavaScript React app to TypeScript.

* Type props and state.
* Type custom hooks and contexts.

### 🎯 Task 2:

Build a **User Dashboard App**:

* Fetch user data using typed `useFetch`.
* Show loading/error states.
* Use Context for theme or authentication.

### 🎯 Task 3:

Add strict typing to an HOC or compound component.

---

## 🧩 Bonus Challenge

* Add **Zod or Yup** for runtime validation alongside TypeScript.
* Create reusable `types.ts` files for backend contracts (API responses).

---

## ✅ Summary

* TypeScript makes React code predictable, maintainable, and self-documented.
* Always type props, state, and contexts.
* Use generics for flexible reusable hooks.
* Follow clean folder architecture for scalability.

