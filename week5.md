# 🗓️ Week 5: React Testing (Jest, React Testing Library & Vitest)

### 🎯 Goal:

Learn how to test React applications effectively using Jest, React Testing Library (RTL), and Vitest. Understand unit, integration, and snapshot testing to ensure code reliability.

---

## 📚 Topics:

1. **Why Testing Matters**
2. **Testing Types (Unit, Integration, E2E)**
3. **Jest Fundamentals**
4. **React Testing Library (RTL)**
5. **Mocking Functions and API Calls**
6. **Vitest Overview (Alternative to Jest)**
7. **Snapshot Testing**
8. **Best Practices & Folder Structure**

---

## 🧠 Deep Dive & Examples

### 1. Why Testing Matters

Testing ensures components work as expected, prevents regressions, and builds confidence during refactors.

---

### 2. Jest Fundamentals

Jest is a powerful testing framework for JavaScript with built-in assertions and mocking.

💡 **Example:**

```jsx
// sum.js
export function sum(a, b) {
  return a + b;
}

// sum.test.js
import { sum } from './sum';

test('adds two numbers', () => {
  expect(sum(2, 3)).toBe(5);
});
```

✅ Run: `npm test`

---

### 3. React Testing Library (RTL)

RTL focuses on testing components as users interact with them — not implementation details.

💡 **Example:**

```jsx
// Counter.jsx
import { useState } from 'react';

export default function Counter() {
  const [count, setCount] = useState(0);
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

```jsx
// Counter.test.jsx
import { render, screen, fireEvent } from '@testing-library/react';
import Counter from './Counter';

test('increments counter on click', () => {
  render(<Counter />);
  const button = screen.getByText('Increment');
  fireEvent.click(button);
  expect(screen.getByText('Count: 1')).toBeInTheDocument();
});
```

✅ Tests actual user behavior — clicking, typing, focusing, etc.

---

### 4. Mocking API Calls

Use Jest to mock API responses.

💡 **Example:**

```jsx
// api.js
export async function fetchData() {
  const res = await fetch('/api/data');
  return res.json();
}

// api.test.js
import { fetchData } from './api';

global.fetch = jest.fn(() => Promise.resolve({
  json: () => Promise.resolve({ id: 1, name: 'Harsha' })
}));

test('fetches data successfully', async () => {
  const data = await fetchData();
  expect(data.name).toBe('Harsha');
});
```

✅ Mocking avoids hitting real APIs during tests.

---

### 5. Snapshot Testing

Snapshots capture the rendered UI structure for future comparison.

```jsx
import { render } from '@testing-library/react';
import Counter from './Counter';

test('matches snapshot', () => {
  const { asFragment } = render(<Counter />);
  expect(asFragment()).toMatchSnapshot();
});
```

✅ Detects unintended UI changes.

---

### 6. Vitest Overview

Vitest is a fast test runner compatible with Jest’s syntax, built for Vite-based React projects.

```bash
npm install -D vitest @testing-library/react @testing-library/jest-dom
```

💡 **vitest.config.js**

```js
import { defineConfig } from 'vitest/config';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
  test: {
    globals: true,
    environment: 'jsdom'
  }
});
```

✅ Run: `npx vitest`

---

### 7. Best Practices

* Keep tests close to components (`/src/components/ComponentName/__tests__/ComponentName.test.jsx`)
* Use `data-testid` only when necessary.
* Avoid testing implementation details.
* Prefer user-facing queries (`getByText`, `getByRole`).

---

## 💻 Assignments

### 🎯 Task 1:

Write unit tests for simple functions (sum, formatDate, etc.) using Jest.

### 🎯 Task 2:

Test a `Todo` component using React Testing Library:

* Add new todos.
* Mark as completed.
* Remove items.

### 🎯 Task 3:

Add snapshot testing for a UI component.

### 🎯 Task 4:

Set up **Vitest** in a Vite project and migrate 2 Jest tests.

---

## 🧩 Bonus Challenge

* Mock a real API using `msw` (Mock Service Worker) and test an async React component.
* Add coverage reporting using `--coverage` flag.

---

## ✅ Summary

* Jest → Unit + Mocking + Snapshot tests
* RTL → User interaction testing
* Vitest → Lightweight, Vite-native Jest alternative
