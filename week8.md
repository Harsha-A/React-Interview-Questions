# 🗓️ Week 8: React Architecture, Code Splitting & Project Structuring

## 🎯 Goal

Design a **production-ready, modular, and scalable React architecture** that supports growth, lazy-loading, maintainability, and team collaboration.

---

## 📚 Topics

### 1. Feature-Based Folder Structure

Instead of grouping files by type (components, styles, etc.), group them by **features**.
This improves scalability and reduces coupling.

**Example Structure:**

```bash
src/
├── app/
│   ├── App.tsx
│   ├── routes.tsx
│   ├── index.css
│   └── providers/
│       ├── ThemeProvider.tsx
│       └── AuthProvider.tsx
├── features/
│   ├── auth/
│   │   ├── components/
│   │   │   └── LoginForm.tsx
│   │   ├── hooks/
│   │   │   └── useAuth.ts
│   │   ├── services/
│   │   │   └── authApi.ts
│   │   └── index.ts
│   ├── dashboard/
│   │   ├── components/
│   │   │   └── DashboardPage.tsx
│   │   ├── hooks/
│   │   │   └── useDashboard.ts
│   │   └── index.ts
├── components/
│   └── common/
│       ├── Button.tsx
│       ├── Loader.tsx
│       └── ErrorBoundary.tsx
├── hooks/
│   ├── useFetch.ts
│   └── useDebounce.ts
├── services/
│   ├── apiClient.ts
│   ├── storageService.ts
│   └── loggerService.ts
├── utils/
│   ├── formatDate.ts
│   ├── constants.ts
│   └── helpers.ts
├── config/
│   ├── env.ts
│   └── routes.ts
└── main.tsx
```

---

### 2. Code Splitting & Lazy Loading

Use **React.lazy** and **Suspense** to load components or routes **only when needed**.

**Example:**

```tsx
import React, { Suspense, lazy } from 'react';

const Dashboard = lazy(() => import('./features/dashboard/components/DashboardPage'));
const Login = lazy(() => import('./features/auth/components/LoginForm'));

function AppRoutes() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <Routes>
        <Route path="/login" element={<Login />} />
        <Route path="/dashboard" element={<Dashboard />} />
      </Routes>
    </Suspense>
  );
}

export default AppRoutes;
```

✅ Benefits:

* Smaller bundle sizes
* Faster initial load
* Better user experience

---

### 3. Route-Based Chunking

You can use **dynamic imports** to automatically split code by route.

**Example (Vite + React Router):**

```tsx
import { createBrowserRouter } from 'react-router-dom';

const router = createBrowserRouter([
  {
    path: '/',
    lazy: () => import('../features/home/HomePage'),
  },
  {
    path: '/profile',
    lazy: () => import('../features/profile/ProfilePage'),
  },
]);

export default router;
```

---

### 4. Environment Configuration

Manage environments via `.env` files and a central config.

**Example:**

```ts
// config/env.ts
export const ENV = {
  API_URL: import.meta.env.VITE_API_URL,
  NODE_ENV: import.meta.env.MODE,
  IS_DEV: import.meta.env.MODE === 'development',
};
```

Usage:

```ts
import { ENV } from '@/config/env';
console.log(ENV.API_URL);
```

---

### 5. Microfrontend Basics (Optional Advanced Topic)

Split your app into **independent deployable modules** using tools like **Module Federation**.
Each team can own a feature and deploy independently.

**Example Use Cases:**

* Separate Admin and User dashboards
* Split static vs dynamic modules

---

## 🧭 Architecture Flow Diagram

```text
App (Root)
│
├── Providers (ThemeProvider, AuthProvider)
│
├── Router (Lazy-loaded routes)
│
├── Features
│   ├── Auth → LoginForm → useAuth → authApi
│   ├── Dashboard → DashboardPage → useDashboard → dashboardApi
│
├── Shared Components (Button, Loader, ErrorBoundary)
│
├── Hooks (useFetch, useDebounce)
│
├── Services (apiClient, logger, storage)
│
├── Utils (helpers, constants)
│
└── Config (env, routes)
```

---

## 💡 Example: Dynamic Imports with Fallback

```tsx
import { Suspense, lazy } from 'react';

const Reports = lazy(() => import('./features/reports/ReportsPage'));

function App() {
  return (
    <Suspense fallback={<div>Loading Reports...</div>}>
      <Reports />
    </Suspense>
  );
}
```

---

## 🧠 Deep Dive: Why Feature-Based Architecture?

* Encourages **domain-driven design**
* Avoids **cross-dependency clutter**
* Makes testing and refactoring easier
* Enables **parallel development** by teams

---

## 💻 Assignment

**Goal:** Build a modular app with lazy-loaded routes.

🧩 Tasks:

1. Create `features/` folders for at least 3 modules (auth, dashboard, profile).
2. Configure lazy loading for each route.
3. Add an `AuthProvider` using Context.
4. Persist login using `localStorage`.
5. Implement environment-based API configuration.

**Bonus:**
Add code-splitting for heavy components like charts or analytics.

---

## 🧩 Bonus Challenge

Refactor your Week 7 project (TypeScript) into this **feature-based folder structure**, implement code-splitting for major routes, and use dynamic imports with fallback UI.

---

## ✅ Summary

By the end of this week, you’ll:

* Master React architecture design
* Implement lazy loading & dynamic imports
* Organize features modularly
* Prepare projects for enterprise-scale development

