# useCallback — Complete Deep Research

A detailed guide on React's `useCallback` hook: how it works, its effect on rendering, real-world applications, and best practices.

---

## 📚 Table of Contents

1. [What is useCallback?](#what-is-usecallback)
2. [How useCallback Works & Rendering Impact](#how-usecallback-works--rendering-impact)
3. [Syntax and Common Patterns](#syntax-and-common-patterns)
4. [Real-World Applications](#real-world-applications)
5. [Common Pitfalls & Fixes](#common-pitfalls--fixes)
6. [Performance Considerations](#performance-considerations)
7. [Best Practices](#best-practices)
8. [Additional Resources](#additional-resources)

---

## 1. What is useCallback?

`useCallback` is a React Hook that **memoizes a function**.  
It returns a **cached version of the callback function** that only changes if one of the dependencies has changed.

> In short: `useCallback` helps prevent unnecessary re-creation of functions on every render, which can optimize performance, especially for child components.

---

## 2. How useCallback Works & Rendering Impact

1. **Initial Render:** React creates the function and caches it.
2. **Subsequent Renders:** Returns the same function reference if dependencies haven't changed.
3. **Effect on Rendering:**
   - Prevents passing new function references to child components.
   - Reduces unnecessary re-renders in `React.memo` child components.
   - Does **not prevent component re-rendering** by itself; it only memoizes the function reference.

---

## 3. Syntax and Common Patterns

### Basic Syntax

```jsx
const memoizedCallback = useCallback(
  () => {
    doSomething(a, b);
  },
  [a, b],
);
```

### Example: Prevent Child Re-Renders

```jsx
import React, { useState, useCallback } from 'react';

const Child = React.memo(({ onClick }) => {
  console.log('Child rendered');
  return <button onClick={onClick}>Click me</button>;
});

function Parent() {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    console.log('Button clicked');
  }, []); // function reference is memoized

  return (
    <>
      <h1>Count: {count}</h1>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <Child onClick={handleClick} />
    </>
  );
}

export default Parent;
```

---

## 4. Real-World Applications

- **Passing callbacks to memoized children:** Prevents unnecessary re-renders.
- **Event handlers in complex components:** Avoids recreating functions on every render.
- **Integration with custom hooks:** Ensures stable function references.
- **Performance optimization in lists:** Prevents creating new functions for each item in mapping.

---

## 5. Common Pitfalls & Fixes

- **Overusing useCallback:** Memoizing trivial functions may add overhead.
- **Incorrect Dependencies:** Missing dependencies can lead to stale values.
- **Misunderstanding Memoization:** `useCallback` only memoizes the function reference, not the result.
- **Not Needed Without React.memo:** If the function is not passed to memoized children, `useCallback` often isn’t needed.

---

## 6. Performance Considerations

- Only memoize functions that are passed as props or are expensive to recreate.
- Combine with `React.memo` to maximize benefit.
- Ensure dependency array is accurate to avoid stale closures.
- Avoid using `useCallback` for very simple components or handlers; React is already fast.

---

## 7. Best Practices

- Use `useCallback` for functions passed to memoized children.
- Keep dependency arrays minimal and correct.
- Combine with `useMemo` if you also need to memoize the result of the function.
- Comment when intentionally omitting dependencies.
- Avoid over-optimization; only use when measurable performance benefit is expected.

---

## 8. Additional Resources

- [React Docs: useCallback](https://react.dev/reference/react/useCallback)
- [React Docs: Optimizing Performance](https://react.dev/learn/optimizing-performance)
- [Kent C. Dodds: When to useMemo and useCallback](https://kentcdodds.com/blog/usememo-and-usecallback)

---