# useReducer — Complete Deep Research

A detailed guide on React's `useReducer` hook: how it works, its effect on rendering, real-world applications, and best practices.

---

## 📚 Table of Contents

1. [What is useReducer?](#what-is-usereducer)
2. [How useReducer Works & Rendering Impact](#how-usereducer-works--rendering-impact)
3. [Syntax and Common Patterns](#syntax-and-common-patterns)
4. [Real-World Applications](#real-world-applications)
5. [Combining useReducer with useContext](#combining-usereducer-with-usecontext)
6. [Common Pitfalls & Fixes](#common-pitfalls--fixes)
7. [Performance Considerations](#performance-considerations)
8. [Best Practices](#best-practices)

---

## 1. What is useReducer?

`useReducer` is a React Hook used for **state management** in functional components. It is ideal when:

- State logic is **complex**
- State depends on **previous state**
- Multiple state variables **interact together**
- You want to **centralize state updates**

> In short: `useReducer` lets you manage state using **actions** and a **reducer function**, instead of multiple `useState` calls.

---

## 2. How useReducer Works & Rendering Impact

1. **Reducer Function:** A pure function that receives `state` and `action` and returns a **new state**.
2. **Dispatch Function:** Call `dispatch(action)` to send actions to the reducer.
3. **State Updates:** React triggers a re-render with the **new state**.
4. **Rendering Impact:**  
   - Only the component using the state re-renders.  
   - `useReducer` **prevents unnecessary multiple `useState` updates** for related state logic.

---

## 3. Syntax and Common Patterns

### Basic Syntax

```jsx
const [state, dispatch] = useReducer(reducer, initialState);
```

### Reducer Function Example

```jsx
function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    case 'reset':
      return { count: 0 };
    default:
      return state;
  }
}
```

### Using useReducer in a Component

```jsx
const initialState = { count: 0 };

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <>
      <h1>Count: {state.count}</h1>
      <button onClick={() => dispatch({ type: 'increment' })}>Increment</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>Decrement</button>
      <button onClick={() => dispatch({ type: 'reset' })}>Reset</button>
    </>
  );
}
```

---

## 4. Real-World Applications

- **Form Handling:** Manage multiple inputs and validation states.
- **Todo Apps:** Centralize add, remove, and update actions.
- **Complex UI States:** Modal visibility, notifications, multi-step forms.
- **Game State:** Track multiple variables like score, level, lives.
- **Shopping Cart:** Add/remove items and calculate totals centrally.

---

## 5. Combining useReducer with useContext

`useReducer` + `useContext` is a common pattern for global state management.

```jsx
import React, { createContext, useReducer, useContext } from 'react';

const CounterContext = createContext();

const initialState = { count: 0 };
function reducer(state, action) {
  switch (action.type) {
    case 'increment': return { count: state.count + 1 };
    case 'decrement': return { count: state.count - 1 };
    default: return state;
  }
}

export function CounterProvider({ children }) {
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <CounterContext.Provider value={{ state, dispatch }}>
      {children}
    </CounterContext.Provider>
  );
}

export function useCounter() {
  return useContext(CounterContext);
}

// Usage in a component
function Counter() {
  const { state, dispatch } = useCounter();
  return (
    <>
      <h1>Count: {state.count}</h1>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
    </>
  );
}
```

---

## 6. Common Pitfalls & Fixes

- **Mutating State Directly:** Always return a new state object, do not mutate the old state.
- **Complex Reducers:** Split into smaller reducers if it becomes too large.
- **Overusing useReducer:** For simple states, `useState` is simpler and cleaner.
- **Unnecessary Dispatches:** Avoid dispatching actions unnecessarily; can cause extra re-renders.

---

## 7. Performance Considerations

- Keep reducer logic pure and fast.
- Memoize dispatch payloads or objects if they are complex.
- Use separate reducers for unrelated states to reduce re-rendering.
- Combine with `useContext` carefully to avoid re-rendering all consumers on small changes.

---

## 8. Best Practices

- Keep the reducer pure (no side effects inside it).
- Use action objects with type and optional payload.
- Centralize related state updates into one reducer.
- Combine with `useContext` for global state management.
- Avoid large monolithic reducers; split if necessary.
- Comment and document action types for clarity.

---

## 🔗 Additional Resources

- [React Docs: useReducer](https://react.dev/reference/react/useReducer)
- [React Docs: useContext](https://react.dev/reference/react/useContext)
- [Kent C. Dodds: useReducer vs useState](https://kentcdodds.com/blog/useReducer-vs-useState)