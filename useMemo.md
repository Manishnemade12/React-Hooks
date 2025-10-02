# useMemo — Complete Deep Research

A detailed guide on React's `useMemo` hook: how it works, its effect on rendering, real-world applications, and best practices.

---

## 📚 Table of Contents

1. [What is useMemo?](#what-is-usememo)
2. [How useMemo Works & Rendering Impact](#how-usememo-works--rendering-impact)
3. [Syntax and Common Patterns](#syntax-and-common-patterns)
4. [Real-World Applications](#real-world-applications)
5. [Common Pitfalls & Fixes](#common-pitfalls--fixes)
6. [Performance Considerations](#performance-considerations)
7. [Best Practices](#best-practices)
8. [Additional Resources](#additional-resources)

---

## 1. What is useMemo?

`useMemo` is a React Hook that **memoizes expensive computations**.  
It allows you to **cache the result** of a function so that it is **recomputed only when dependencies change**, avoiding unnecessary recalculations.

> In short: `useMemo` optimizes performance by preventing expensive calculations on every render.

---

## 2. How useMemo Works & Rendering Impact

1. **Initial Render:** React runs the function and stores the result.
2. **Subsequent Renders:** The cached value is returned unless one of the dependencies has changed.
3. **Effect on Rendering:**
   - Prevents recalculation of expensive values.
   - Reduces unnecessary re-rendering of components that depend on these values.
   - Does **not prevent component re-rendering** itself; it only memoizes the computation.

---

## 3. Syntax and Common Patterns

### Basic Syntax

```jsx
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

### Example: Expensive Calculation

```jsx
import React, { useMemo } from 'react';

function ExpensiveComponent({ number }) {
  const factorial = useMemo(() => {
    console.log('Calculating factorial...');
    const computeFactorial = (n) => {
      if (n <= 1) return 1;
      return n * computeFactorial(n - 1);
    };
    return computeFactorial(number);
  }, [number]);

  return <div>Factorial of {number} is {factorial}</div>;
}

export default ExpensiveComponent;
```

### Example: Filtering List

```jsx
const filteredItems = useMemo(() => {
  return items.filter(item => item.includes(searchTerm));
}, [items, searchTerm]);
```

---

## 4. Real-World Applications

- **Expensive Computations:** Calculations, data processing, sorting, filtering large arrays.
- **Derived State:** Compute derived values from state efficiently.
- **Prevent Re-Renders in Child Components:** Use memoized values as props to `React.memo` components.
- **Complex Object Creation:** Avoid creating new object/array references unnecessarily.

---

## 5. Common Pitfalls & Fixes

- **Overusing useMemo:** Not all calculations need memoization; small computations may add overhead.
- **Incorrect Dependencies:** Missing dependencies can cause stale or incorrect values.
- **Misunderstanding Memoization:** `useMemo` does not prevent re-renders—it only memoizes the value.
- **Reference Equality:** Always memoize objects/arrays to prevent unnecessary child re-renders.

---

## 6. Performance Considerations

- Only memoize expensive computations.
- Ensure dependencies are minimal and accurate.
- Avoid recomputing on every render by caching derived data.
- Combine with `React.memo` for child component optimization.

---

## 7. Best Practices

- Use `useMemo` for performance optimization, not for controlling render logic.
- Keep dependency arrays precise to prevent bugs.
- Avoid using it for simple calculations, which are cheaper than memoization overhead.
- Combine with `useCallback` when memoizing functions.
- Comment when intentionally omitting dependencies in advanced scenarios.

---

## 8. Additional Resources

- [React Docs: useMemo](https://react.dev/reference/react/useMemo)
- [React Docs: Optimizing Performance](https://react.dev/learn/optimizing-performance)
- [Kent C. Dodds: When to useMemo and useCallback](https://kentcdodds.com/blog/usememo-and-usecallback)

---