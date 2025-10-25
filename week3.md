# 🗓️ Week 3: State Management with Redux & Zustand

## 🎯 Goal

Master **global state management** using **Redux Toolkit** and **Zustand**, understand when to use each, and learn how to structure scalable state architecture in React.

---

## 📚 Topics

### 1. Why State Management?

* React’s `useState` and `useReducer` work well for local state.
* As apps grow, managing shared or global state (like user data, theme, cart items) becomes hard.
* Libraries like **Redux** and **Zustand** help centralize and manage state efficiently.

---

### 2. Redux Toolkit (RTK)

Redux Toolkit is the **official, modern, opinionated way** to use Redux. It simplifies boilerplate with easy-to-use APIs.

#### Key Concepts:

| Concept      | Description                                  |
| ------------ | -------------------------------------------- |
| **Store**    | Holds the entire app state                   |
| **Slice**    | A modular piece of state + actions + reducer |
| **Dispatch** | Sends actions to update state                |
| **Selector** | Extracts specific state data                 |
| **Thunk**    | Handles async logic like API calls           |

#### Example: Counter using Redux Toolkit

```bash
npm install @reduxjs/toolkit react-redux
```

```jsx
// store.js
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from './counterSlice';

export const store = configureStore({
  reducer: {
    counter: counterReducer,
  },
});
```

```jsx
// counterSlice.js
import { createSlice } from '@reduxjs/toolkit';

const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: (state) => { state.value += 1; },
    decrement: (state) => { state.value -= 1; },
    reset: (state) => { state.value = 0; },
  },
});

export const { increment, decrement, reset } = counterSlice.actions;
export default counterSlice.reducer;
```

```jsx
// App.jsx
import { Provider, useDispatch, useSelector } from 'react-redux';
import { store } from './store';
import { increment, decrement, reset } from './counterSlice';

function Counter() {
  const count = useSelector((state) => state.counter.value);
  const dispatch = useDispatch();

  return (
    <div>
      <h2>Count: {count}</h2>
      <button onClick={() => dispatch(increment())}>Increment</button>
      <button onClick={() => dispatch(decrement())}>Decrement</button>
      <button onClick={() => dispatch(reset())}>Reset</button>
    </div>
  );
}

export default function App() {
  return (
    <Provider store={store}>
      <Counter />
    </Provider>
  );
}
```

🧠 **Explanation:**

* Redux Toolkit manages global state using `createSlice`.
* `useSelector()` reads state, `useDispatch()` sends actions.
* Redux state updates are immutable but simplified using Immer internally.

---

### 3. Async Thunks (Redux Async Logic)

Redux Toolkit allows async operations via `createAsyncThunk()`.

```jsx
import { createSlice, createAsyncThunk } from '@reduxjs/toolkit';

export const fetchUsers = createAsyncThunk('users/fetch', async () => {
  const res = await fetch('https://jsonplaceholder.typicode.com/users');
  return res.json();
});

const userSlice = createSlice({
  name: 'users',
  initialState: { data: [], loading: false },
  extraReducers: (builder) => {
    builder
      .addCase(fetchUsers.pending, (state) => { state.loading = true; })
      .addCase(fetchUsers.fulfilled, (state, action) => {
        state.data = action.payload;
        state.loading = false;
      })
      .addCase(fetchUsers.rejected, (state) => { state.loading = false; });
  },
});

export default userSlice.reducer;
```

🧠 **Tip:** Async thunks make it simple to fetch or update data while tracking loading states.

---

### 4. Zustand — Lightweight State Management

Zustand is a **minimalistic, scalable** state management library with hooks-based APIs.
It avoids Redux’s boilerplate while maintaining simplicity.

#### Installation

```bash
npm install zustand
```

#### Example: Counter with Zustand

```jsx
import { create } from 'zustand';

const useCounterStore = create((set) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
  decrement: () => set((state) => ({ count: state.count - 1 })),
  reset: () => set({ count: 0 }),
}));

export default function Counter() {
  const { count, increment, decrement, reset } = useCounterStore();

  return (
    <div>
      <h2>Count: {count}</h2>
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
      <button onClick={reset}>Reset</button>
    </div>
  );
}
```

🧠 **Why Zustand?**

* No boilerplate (no reducers, actions, or providers).
* Uses native hooks for simplicity.
* Supports middlewares like persistence and devtools.

---

### 5. Zustand Middleware Examples

#### a. Persist State to LocalStorage

```jsx
import { create } from 'zustand';
import { persist } from 'zustand/middleware';

const useThemeStore = create(persist(
  (set) => ({
    theme: 'light',
    toggleTheme: () => set((state) => ({ theme: state.theme === 'light' ? 'dark' : 'light' })),
  }),
  { name: 'theme-storage' }
));
```

#### b. Zustand with Async Fetch

```jsx
const useUserStore = create((set) => ({
  users: [],
  fetchUsers: async () => {
    const res = await fetch('https://jsonplaceholder.typicode.com/users');
    const data = await res.json();
    set({ users: data });
  },
}));
```

---

### 6. Redux vs Zustand Comparison

| Feature          | Redux Toolkit      | Zustand               |
| ---------------- | ------------------ | --------------------- |
| Boilerplate      | Moderate           | Very Low              |
| Learning Curve   | Medium             | Easy                  |
| Async Logic      | `createAsyncThunk` | Direct async in store |
| Middleware       | Built-in           | Optional plugins      |
| DevTools Support | Excellent          | Excellent             |
| Best For         | Large-scale apps   | Small/medium apps     |

🧠 **Guideline:** Use Redux Toolkit for enterprise-level state management. Use Zustand for smaller or UI-driven apps.

---

## 💻 Assignments

### 1. **Redux Todo App**

* Add, remove, and toggle todos.
* Use `createSlice` for todos.
* Add async thunk to simulate saving todos to a server.

### 2. **Zustand Theme Manager**

* Create a Zustand store for theme switching.
* Persist theme to localStorage.
* Toggle theme from multiple components without prop drilling.

---

## 🧩 Bonus Challenge

* Combine **Redux Toolkit** for API data and **Zustand** for UI preferences in a single app.
* Add Redux DevTools and Zustand middleware for persistence.

---

## ✅ Summary

| Concept        | Redux Toolkit      | Zustand           |
| -------------- | ------------------ | ----------------- |
| State Handling | Slices & Reducers  | Direct set/update |
| Async Calls    | `createAsyncThunk` | Native async      |
| DevTools       | Built-in           | Optional          |
| Boilerplate    | Higher             | Minimal           |
