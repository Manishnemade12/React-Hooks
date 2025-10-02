# useMemo — Complete Deep Research

A detailed guide on React's `useMemo` hook: how it works, its effect on rendering, real-world applications, and best practices.

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

```jsx
import { useMemo } from "react";

// Common state
const [count, setCount] = useState(0);
const [items, setItems] = useState([1,2,3]);
```

1. Derived State
   ```jsx
const doubleCount = useMemo(() => count * 2, [count]);
const total = useMemo(() => items.reduce((sum, i) => sum + i, 0), [items]);
```

 2. Prevent Re-Renders in Child Components
    ```jsx
const memoizedValue = useMemo(() => ({ count }), [count]);
// <ChildComponent data={memoizedValue} /> wrapped in React.memo prevents unnecessary renders
```

// 3. Complex Object Creation
```jsx
const config = useMemo(() => ({ theme: 'dark', layout: 'grid' }), []);
const expensiveArray = useMemo(() => new Array(1000).fill(0).map((_, i) => i*i), []);
```

---

## 5. Best Practices

- Use `useMemo` for performance optimization, not for controlling render logic.
- Keep dependency arrays precise to prevent bugs.
- Avoid using it for simple calculations, which are cheaper than memoization overhead.
- Combine with `useCallback` when memoizing functions.
- Comment when intentionally omitting dependencies

## useMemo Best Practices 

```jsx
import { useMemo, useCallback } from "react";

// 1. Performance optimization only
const expensiveValue = useMemo(() => heavyCalculation(data), [data]);

// 2. Keep dependencies precise
const filteredItems = useMemo(() => items.filter(i => i.active), [items]);

// 3. Avoid for simple calculations
const sum = a + b; // no useMemo needed

// 4. Combine with useCallback for functions
const memoizedHandler = useCallback(() => {
  console.log('Handled!');
}, [dependency]);

// 5. Comment when intentionally omitting dependencies
const value = useMemo(() => compute(), []); // intentional: only once

```
---

## 8. Additional Resources

- [React Docs: useMemo](https://react.dev/reference/react/useMemo)
- [React Docs: Optimizing Performance](https://react.dev/learn/optimizing-performance)
- [Kent C. Dodds: When to useMemo and useCallback](https://kentcdodds.com/blog/usememo-and-usecallback)

---
