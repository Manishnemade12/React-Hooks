# useDebugValue — Complete Deep Research

A detailed guide on React's `useDebugValue` hook: how it works, its effect on rendering, real-world applications, and best practices.

---

## 1. What is useDebugValue?

`useDebugValue` is a React Hook that **can be used to display a custom label for custom hooks in React DevTools**.

- It helps in **inspecting values** within custom hooks during development.
- Only active when React DevTools are open.
- Primarily for **debugging purposes**, it does not affect the component's behavior or rendering in production.

> In short: `useDebugValue` provides a way to add extra information to the React DevTools for custom hooks, making debugging easier and more insightful.

---

## 2. How useDebugValue Works & Rendering Impact

1. **Custom Hook Context:** `useDebugValue` must be called inside a custom hook (a function whose name starts with `use`).
2. **React DevTools Integration:** When React DevTools are active, they detect `useDebugValue` calls within custom hooks.
3. **Custom Label Display:** The value passed to `useDebugValue` is then displayed alongside the custom hook in the DevTools component tree.
4. **No Rendering Impact:** This hook has no impact on your component's rendering logic, performance, or behavior in a production build.

- **Formatter Function (Optional):** You can pass a second argument, a formatter function, which will be called only when the hook is inspected. This is useful for expensive formatting operations.

> `useDebugValue` is a development-only tool that enhances the observability of your custom hooks within React DevTools.

---

## 3. Syntax and Common Patterns

### Basic Syntax

```jsx
import { useDebugValue, useState } from 'react';

function useMyCustomHook(initialValue) {
  const [value, setValue] = useState(initialValue);
  
  // Display the current value in React DevTools
  useDebugValue(value);
  
  return [value, setValue];
}

// Usage in a component
function MyComponent() {
  const [count, setCount] = useMyCustomHook(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}

export default MyComponent;
```

### With Formatter Function

```jsx
import { useDebugValue, useState } from 'react';

function useToggle(initialValue = false) {
  const [on, setOn] = useState(initialValue);

  const toggle = () => setOn(prev => !prev);

  // Formatter function helps display a more readable value
  useDebugValue(on, (value) => (value ? 'On' : 'Off'));

  return [on, toggle];
}

// Usage in a component
function App() {
  const [isOpen, toggleOpen] = useToggle(true);

  return (
    <div>
      <p>State: {isOpen ? 'Open' : 'Closed'}</p>
      <button onClick={toggleOpen}>Toggle</button>
    </div>
  );
}

export default App;
```

---

## 4. Real-World Applications

- **Debugging custom hooks:** Providing clear, human-readable labels for the internal state or derived values of custom hooks.
- **Conditional debugging:** Using the formatter function to compute expensive debug values only when inspected in DevTools.

```jsx
//1. Debugging a custom form validation hook
function useFormValidation(initialValues) {
  const [values, setValues] = useState(initialValues);
  const [errors, setErrors] = useState({});

  useDebugValue(errors, (err) => `Errors: ${Object.keys(err).length}`);

  return { values, errors, setValues, setErrors };
}

//2. Displaying user status in an authentication hook
function useAuth() {
  const [user, setUser] = useState(null);
  const [isAuthenticated, setIsAuthenticated] = useState(false);

  useDebugValue(isAuthenticated ? `Logged in as ${user?.name}` : 'Logged out');

  return { user, isAuthenticated, setUser, setIsAuthenticated };
}

//3. Tracking active state in a timer/interval hook
function useInterval(callback, delay) {
  const [isActive, setIsActive] = useState(false);
  useDebugValue(isActive ? 'Active' : 'Inactive');
  // ... interval logic ...
  return [isActive, setIsActive];
}
```

---

## 5. Best Practices

- **Use only for custom hooks:** It's designed to work within custom hooks, not directly in components.
- **Keep it simple:** The debug value should be concise and easily understandable.
- **Leverage formatter function for expensive computations:** If generating the debug value is costly, use the optional formatter function to defer computation.
- **Don't use in production:** It's a development tool and should not affect production bundles or performance.

```jsx
// ✅ Simple debug value for a counter hook
function useCounter(initial) {
  const [count, setCount] = useState(initial);
  useDebugValue(count);
  return count;
}

// ✅ Using formatter for a complex object
function useComplexData() {
  const [data, setData] = useState({ /* ... */ });
  useDebugValue(data, (d) => `Items: ${d.items.length}, Status: ${d.status}`);
  return data;
}

// ❌ Don't use outside custom hooks
// function MyComponent() {
//   useDebugValue('Not in a custom hook'); // This will cause an error
//   return <div>...</div>;
// }
```

---

## 6. Additional Resources

- [React Docs: useDebugValue](https://react.dev/reference/react/useDebugValue)
- [React DevTools Documentation](https://react.dev/learn/react-developer-tools)

