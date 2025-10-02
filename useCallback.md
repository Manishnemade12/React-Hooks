# useCallback — Complete Deep Research

A detailed guide on React's `useCallback` hook: how it works, its effect on rendering, real-world applications, and best practices.

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


```jsx
//1. Passing callbacks to memoized children
const Child = React.memo(({ onClick }) => (
  <button onClick={onClick}>Click</button>
));

const handleClick = useCallback(() => {
  console.log("Clicked");
}, []); // stable reference prevents unnecessary re-renders


//2. Event handlers in complex components

const handleInputChange = useCallback((e) => {
  setValue(e.target.value);
}, []); // avoids recreating function on every render


//3. Integration with custom hooks

function useCustomHook(callback) {
  useEffect(() => {
    callback();
  }, [callback]); // ensures stable function reference
}

useCustomHook(handleClick);


//4. Performance optimization in lists

const items = [1,2,3];
const handleItemClick = useCallback((id) => {
  console.log("Item", id);
}, []); // prevents creating new function for each item

items.map(item => <button key={item} onClick={() => handleItemClick(item)}>{item}</button>);
```
---

## 5. Best Practices

- Use `useCallback` for functions passed to memoized children.
- Keep dependency arrays minimal and correct.
- Combine with `useMemo` if you also need to memoize the result of the function.


# useCallback & useMemo Best Practices Example

```jsx
const increment = useCallback(() => {
  setCount(prev => prev + 1);
}, []); // dependency array minimal and correct


// Only include variables that the callback actually uses
const multiply = useCallback(() => count * multiplier, [count, multiplier]);

//Combine with useMemo if you also need to memoize the result
const computedValue = useMemo(() => {
  return count * multiplier;
}, [count, multiplier]); // recompute only when count or multiplier changes

```

---

## 6. Additional Resources

- [React Docs: useCallback](https://react.dev/reference/react/useCallback)
- [React Docs: Optimizing Performance](https://react.dev/learn/optimizing-performance)
- [Kent C. Dodds: When to useMemo and useCallback](https://kentcdodds.com/blog/usememo-and-usecallback)

---
