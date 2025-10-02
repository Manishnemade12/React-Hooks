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
```jsx
- **Authentication:** Store user info (token, role) in a global context.

  const AuthContext = createContext();

export const AuthProvider = ({ children }) => {
  const [user, setUser] = useState({ token: null, role: null });
  const login = (token, role) => setUser({ token, role });
  const logout = () => setUser({ token: null, role: null });

  return (
    <AuthContext.Provider value={{ user, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
};
```

- **Localization:** Language selection accessible across components.
```jsx
  const LocaleContext = createContext();

export const LocaleProvider = ({ children }) => {
  const [language, setLanguage] = useState("en"); // default: English
  const switchLanguage = (lang) => setLanguage(lang);

  return (
    <LocaleContext.Provider value={{ language, switchLanguage }}>
      {children}
    </LocaleContext.Provider>
  );
};
```

- **Shopping Cart:** Share cart items state across multiple components.
```jsx
  // manage cart items
const [cartItems, setCartItems] = useState([]);
const addItem = (item) => setCartItems([...cartItems, item]);
const removeItem = (id) => setCartItems(cartItems.filter(i => i.id !== id));
```

---



## 5. Best Practices

- Use context only for global/shared state.
- Avoid frequent changes in context value to prevent unnecessary re-renders.
- Combine `useReducer` with `useContext` for complex state.
- Memoize provider values with `useMemo`.
- Create custom hooks for easier consumption of context.
- Do not overuse context; not a replacement for all state management.

---
