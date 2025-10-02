# useContext — Complete Deep Research

A detailed guide on React's `useContext` hook: how it works, its effect on rendering, real-world applications, and best practices.


## 1. What is useContext?

`useContext` is a React Hook that lets you **consume context values** in function components.

- Context provides a way to **share data globally** in a component tree.
- `useContext` is an alternative to the **Context Consumer pattern**.
- It avoids **prop drilling** (passing props through many layers).

> In short: `useContext` lets a component access values from a context directly without passing props.

---

## 2. How useContext Works & Rendering Impact

1. **Context Creation:** Use `React.createContext()` to create a context with a default value.
2. **Context Provider:** Wrap components with `Context.Provider` and provide a value.
3. **Consuming Context:** Any nested component can call `useContext(Context)` to read the value.
4. **Rendering:** 
   - When context value changes, all consuming components **re-render automatically**.
   - Only the components consuming the updated value re-render, not the entire tree.

> `useContext` itself does **not create state**; it just accesses values from context.

---

## 3. Syntax and Common Patterns

### Creating a Context

```jsx
import React, { createContext, useContext } from 'react';

const ThemeContext = createContext('light'); // default value
```

### Providing a Context

```jsx
<ThemeContext.Provider value={theme}>
  <App />
</ThemeContext.Provider>
```

### Consuming Context

```jsx
const theme = useContext(ThemeContext);
console.log(theme); // 'light' or current value
```

### Full Example

```jsx
import React, { createContext, useContext, useState } from 'react';

const ThemeContext = createContext();

function App() {
  const [theme, setTheme] = useState('light');
  
  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      <Toolbar />
    </ThemeContext.Provider>
  );
}

function Toolbar() {
  return (
    <div>
      <ThemeButton />
    </div>
  );
}

function ThemeButton() {
  const { theme, setTheme } = useContext(ThemeContext);

  return (
    <button onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}>
      Current Theme: {theme}
    </button>
  );
}

export default App;
```

---

## 4. Real-World Applications

- **Theme Switching:** Light/dark mode management globally.
- **Authentication:** Store user info (token, role) in a global context.
- **Localization:** Language selection accessible across components.
- **Settings / Configs:** App-wide configuration settings.
- **Shopping Cart:** Share cart items state across multiple components.

---



## 8. Best Practices

- Use context only for global/shared state.
- Avoid frequent changes in context value to prevent unnecessary re-renders.
- Combine `useReducer` with `useContext` for complex state.
- Memoize provider values with `useMemo`.
- Create custom hooks for easier consumption of context.
- Do not overuse context; not a replacement for all state management.

---
