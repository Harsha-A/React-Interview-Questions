Perfect! Here’s **Week 3 README** ready to copy into your repo:

---

````markdown
# 🧪 Week 3 — React Testing, TypeScript & Build Optimization

> 🎯 Goal: Ensure code reliability, type safety, and optimal builds for large-scale React applications.

---

## 🧠 1. Testing in React

Testing is crucial for maintainable, bug-free apps. Common approaches:

### A. Unit Testing with Jest
```jsx
// sum.js
export const sum = (a, b) => a + b;

// sum.test.js
import { sum } from './sum';

test('adds 1 + 2 to equal 3', () => {
  expect(sum(1, 2)).toBe(3);
});
````

### B. Component Testing with React Testing Library (RTL)

```jsx
import { render, screen, fireEvent } from '@testing-library/react';
import Counter from './Counter';

test('increments counter on button click', () => {
  render(<Counter />);
  const button = screen.getByText(/increment/i);
  fireEvent.click(button);
  expect(screen.getByText(/count: 1/i)).toBeInTheDocument();
});
```

**💬 Interview Insight:**

> RTL emphasizes testing *behavior over implementation*, aligning with how users interact with UI.

---

### C. Mocking APIs

```jsx
import axios from 'axios';
import { fetchUser } from './api';
jest.mock('axios');

test('fetches user data', async () => {
  axios.get.mockResolvedValue({ data: { name: 'John' } });
  const data = await fetchUser();
  expect(data.name).toBe('John');
});
```

---

## ⚙️ 2. TypeScript for Large Apps

### A. Types vs Interfaces

```ts
interface User {
  id: number;
  name: string;
}

type Product = {
  id: number;
  title: string;
};
```

* **Interfaces:** extendable, ideal for objects/classes
* **Types:** more flexible (unions, intersections)

### B. Typing Props & Hooks

```tsx
type ButtonProps = {
  label: string;
  onClick: () => void;
};

const Button: React.FC<ButtonProps> = ({ label, onClick }) => (
  <button onClick={onClick}>{label}</button>
);
```

```tsx
const [count, setCount] = React.useState<number>(0);
```

**💬 Interview Insight:**

> Strong typing prevents runtime errors and improves IDE auto-completion.

---

### C. Generics for Reusable Hooks

```tsx
function useFetch<T>(url: string): { data: T | null; loading: boolean } {
  const [data, setData] = React.useState<T | null>(null);
  const [loading, setLoading] = React.useState(true);

  React.useEffect(() => {
    fetch(url)
      .then(res => res.json())
      .then(json => setData(json))
      .finally(() => setLoading(false));
  }, [url]);

  return { data, loading };
}
```

---

## 🧩 3. Build & Bundle Optimization

### A. Code Splitting

* Split bundles by route or feature to reduce initial load.

```tsx
const Dashboard = React.lazy(() => import('./Dashboard'));
```

### B. Tree Shaking

* Remove unused code via ES modules (supported by Webpack/Rollup).

### C. Bundle Analysis

```bash
npm run build
npx source-map-explorer build/static/js/*.js
```

* Identifies heavy modules or dependencies.

### D. Environment Variables & `.env`

```env
REACT_APP_API_URL=https://api.example.com
```

```ts
const apiUrl = process.env.REACT_APP_API_URL;
```

* Keeps sensitive config separate.

---

## 🧠 4. Interview Questions (Week 3)

| Question                                                 | What They’re Testing                       |
| -------------------------------------------------------- | ------------------------------------------ |
| How do you test a component with side-effects?           | Jest + RTL, mocking APIs                   |
| Difference between `interface` and `type` in TypeScript? | Type safety and OOP knowledge              |
| How do you optimize bundle size in React?                | Code splitting, tree shaking, lazy loading |
| How do you test asynchronous API calls?                  | Mocking, Promises, RTL `waitFor`           |
| How do you type a generic reusable hook?                 | Advanced TypeScript knowledge              |

---

## 🧰 5. Homework / Repo Tasks

**Folder:** `/week3-testing-typescript/`

1. Write unit tests for a **Counter** and **Form** component
2. Create a **mock API** call with Jest and test data fetching
3. Convert an existing feature to **TypeScript**, adding proper types for props, state, and hooks
4. Implement **lazy loading** on a route-based feature
5. Analyze bundle size using **source-map-explorer** and note heavy dependencies

---

✅ **End of Week 3:**
By the end of this week, you should be able to:

* Write robust unit & integration tests
* Leverage TypeScript for type safety and maintainable code
* Optimize React bundle size for faster loading
* Use environment variables and proper build tooling

