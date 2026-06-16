# React - Comprehensive Notes

## Overview

React is a component-based JavaScript library for building fast, interactive user interfaces. It focuses on rendering UI from state, composing reusable components, and keeping application behavior predictable as applications grow.

**Key Point:** React helps developers build modern single-page applications with reusable components, a strong ecosystem, and flexible integration with backend services.

---

## Learning Objectives

By the end of this module, you will:
- [ ] Understand React architecture and component-based design
- [ ] Build UIs with JSX, props, and state
- [ ] Manage side effects with hooks
- [ ] Handle forms and user input
- [ ] Implement client-side routing
- [ ] Fetch and manage API data
- [ ] Apply state management patterns
- [ ] Build and deploy React applications

---

## Core Concepts

### 1. Components and JSX

```tsx
type UserCardProps = {
  user: {
    name: string;
    email: string;
  };
  onSelect: (email: string) => void;
};

export function UserCard({ user, onSelect }: UserCardProps) {
  return (
    <div className="user-card">
      <h3>{user.name}</h3>
      <p>{user.email}</p>
      <button onClick={() => onSelect(user.email)}>Select</button>
    </div>
  );
}
```

Key ideas:
- Components are reusable UI building blocks.
- JSX combines markup with JavaScript logic.
- Props pass data from parent components to children.

### 2. State and Events

```tsx
import { useState } from "react";

export function Counter() {
  const [count, setCount] = useState(0);

  function increment() {
    setCount((current) => current + 1);
  }

  return (
    <section>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
    </section>
  );
}
```

Key ideas:
- State stores changing component data.
- Updating state triggers a re-render.
- Event handlers respond to clicks, input, and form actions.

### 3. Conditional Rendering and Lists

```tsx
type Task = {
  id: number;
  title: string;
  completed: boolean;
};

export function TaskList({ tasks }: { tasks: Task[] }) {
  if (tasks.length === 0) {
    return <p>No tasks available.</p>;
  }

  return (
    <ul>
      {tasks.map((task) => (
        <li key={task.id}>
          {task.title} - {task.completed ? "Done" : "Pending"}
        </li>
      ))}
    </ul>
  );
}
```

Key ideas:
- Use `map()` for lists.
- Every rendered list item needs a stable `key`.
- Use conditions to render loading, empty, or success states.

### 4. Effects and API Calls

```tsx
import { useEffect, useState } from "react";

type User = {
  id: number;
  name: string;
};

export function UserList() {
  const [users, setUsers] = useState<User[]>([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    async function loadUsers() {
      try {
        const response = await fetch("https://api.example.com/users");

        if (!response.ok) {
          throw new Error("Failed to load users");
        }

        const data: User[] = await response.json();
        setUsers(data);
      } catch (err) {
        setError(err instanceof Error ? err.message : "Unknown error");
      } finally {
        setLoading(false);
      }
    }

    loadUsers();
  }, []);

  if (loading) {
    return <p>Loading users...</p>;
  }

  if (error) {
    return <p>{error}</p>;
  }

  return (
    <ul>
      {users.map((user) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

Use `useEffect` when you need to:
- Fetch data
- Subscribe to external events
- Synchronize with systems outside React

### 5. Forms

```tsx
import { FormEvent, useState } from "react";

type UserFormState = {
  name: string;
  email: string;
};

export function UserForm() {
  const [formState, setFormState] = useState<UserFormState>({
    name: "",
    email: "",
  });

  function handleSubmit(event: FormEvent<HTMLFormElement>) {
    event.preventDefault();
    console.log(formState);
  }

  return (
    <form onSubmit={handleSubmit}>
      <input
        value={formState.name}
        onChange={(event) =>
          setFormState((current) => ({ ...current, name: event.target.value }))
        }
        placeholder="Name"
      />
      <input
        value={formState.email}
        onChange={(event) =>
          setFormState((current) => ({ ...current, email: event.target.value }))
        }
        placeholder="Email"
      />
      <button type="submit">Save</button>
    </form>
  );
}
```

Key ideas:
- Controlled inputs keep form values in React state.
- Validation can run on change, blur, or submit.
- Libraries like React Hook Form are useful for larger forms.

### 6. Routing

```tsx
import { BrowserRouter, Link, Route, Routes } from "react-router-dom";

function Home() {
  return <h2>Home</h2>;
}

function Users() {
  return <h2>Users</h2>;
}

export function AppRouter() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/users">Users</Link>
      </nav>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/users" element={<Users />} />
      </Routes>
    </BrowserRouter>
  );
}
```

### 7. Shared State and Context

```tsx
import { createContext, useContext, useState } from "react";

type Theme = "light" | "dark";

type ThemeContextValue = {
  theme: Theme;
  toggleTheme: () => void;
};

const ThemeContext = createContext<ThemeContextValue | null>(null);

export function ThemeProvider({ children }: { children: React.ReactNode }) {
  const [theme, setTheme] = useState<Theme>("light");

  return (
    <ThemeContext.Provider
      value={{
        theme,
        toggleTheme: () =>
          setTheme((current) => (current === "light" ? "dark" : "light")),
      }}
    >
      {children}
    </ThemeContext.Provider>
  );
}

export function useTheme() {
  const context = useContext(ThemeContext);

  if (!context) {
    throw new Error("useTheme must be used within ThemeProvider");
  }

  return context;
}
```

State management options:
- Local component state with `useState`
- Shared state with Context
- Server state with TanStack Query
- Complex client state with Redux Toolkit or Zustand

---

## Suggested Project Structure

```text
src/
|-- api/
|-- components/
|-- features/
|-- hooks/
|-- pages/
|-- routes/
|-- services/
|-- styles/
|-- types/
|-- App.tsx
`-- main.tsx
```

---

## Best Practices

- Keep components small and focused.
- Prefer derived UI over duplicated state.
- Handle loading, error, and empty states clearly.
- Use TypeScript for safer props and data models.
- Extract reusable logic into custom hooks.
- Write tests for components and user flows.

---

## React Ecosystem

Core tools:
- React
- React DOM
- Vite or Next.js
- React Router

Helpful libraries:
- TanStack Query
- Redux Toolkit
- Zustand
- React Hook Form
- Axios
- Testing Library

---

## Revision Topics

- JSX and rendering
- Props vs state
- Hooks and side effects
- Controlled forms
- Routing in SPAs
- Context API
- Performance basics

---

## References

### Official Resources
- [React Official Docs](https://react.dev/)
- [React Learn](https://react.dev/learn)
- [React Router Docs](https://reactrouter.com/)

### Practice Resources
- [Frontend Mentor](https://www.frontendmentor.io/)
- [freeCodeCamp React](https://www.freecodecamp.org/news/tag/react/)
- [JavaScript Info](https://javascript.info/)

### Recommended Tools
- VS Code with React and TypeScript extensions
- React Developer Tools browser extension
- Node.js and npm
- Vite for project setup

---

## Summary

React connects state to UI through reusable components, which makes frontend applications easier to reason about and scale. Once you are comfortable with components, hooks, forms, routing, and API integration, you will be ready to build strong frontend applications that work well with ASP.NET Core backends and microservices.

**Last Updated:** June 2026
