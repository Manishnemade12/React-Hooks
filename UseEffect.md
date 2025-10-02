# useEffect — Complete Deep Research

> A detailed guide on React's `useEffect` hook: how it works, its effect on rendering, real-world applications, and best practices.

---

## Table of Contents

1. [What is useEffect](#what-is-useeffect)
2. [How useEffect Works & Rendering Impact](#how-useeffect-works--rendering-impact)
3. [Syntax and Common Patterns](#syntax-and-common-patterns)
4. [Real-World Applications](#real-world-applications)
5. [Cleanup Functions](#cleanup-functions)
6. [Common Pitfalls & Fixes](#common-pitfalls--fixes)
7. [Performance Considerations](#performance-considerations)
8. [Best Practices](#best-practices)

---

## What is useEffect?

`useEffect` is a React Hook that lets you perform **side effects** in function components. Side effects include:

* Fetching data from APIs
* Subscribing to events or WebSockets
* Timers or intervals
* Logging or analytics
* Direct DOM manipulations

> In short: `useEffect` allows function components to handle side effects after rendering.

---

## How useEffect Works & Rendering Impact

1. **Initial Render:** React renders the component and updates the DOM.
2. **After Paint:** `useEffect` runs after the DOM is painted.
3. **State Updates inside Effects:** Calling `setState` inside `useEffect` triggers a re-render.
4. **Cleanup:** If the effect returns a function, React calls it before the next effect run or on unmount.

**Dependency Behavior:**

* `[]` → runs once (componentDidMount)
* No array → runs after every render
* `[dep1, dep2]` → runs after mount and when any dependency changes

> Effects do not block rendering, improving UI responsiveness.

---

## Syntax and Common Patterns

```jsx
import { useEffect } from 'react';

// Runs every render
useEffect(() => {
  console.log('Effect runs after every render');
});

// Runs once on mount
useEffect(() => {
  console.log('Effect runs once on mount');
}, []);

// Runs when dependencies change
useEffect(() => {
  console.log('Effect runs when dep changes:', dep);
}, [dep]);

// Cleanup example
useEffect(() => {
  const id = setInterval(() => console.log('Tick'), 1000);
  return () => clearInterval(id);
}, []);
```

---

## Real-World Applications

### Event Listeners

```jsx
useEffect(() => {
  const handleResize = () => setSize({ w: window.innerWidth, h: window.innerHeight });
  window.addEventListener('resize', handleResize);
  handleResize();
  return () => window.removeEventListener('resize', handleResize);
}, []);
```

### Subscriptions / WebSockets

```jsx
function MyComponent({ url }) {
  const [messages, setMessages] = useState([]); // State to store all incoming messages

  // Function that runs every time a message is received from the server
  const handleMessage = (event) => {
    setMessages((prev) => [...prev, event.data]); // Add new message to state
  };

  useEffect(() => {
    const socket = new WebSocket(url);                 // Create a new WebSocket connection
    socket.addEventListener("message", handleMessage); // Attach message listener

    return () => socket.close(); // Cleanup: close the WebSocket when component unmounts or URL changes
  }, [url]);

  // Normally the UI would go here, but only logic is needed
  return null;
}

```

### Timers / Debouncing

```jsx
useEffect(() => {
  const id = setTimeout(() => saveDraft(draft), 800);
  return () => clearTimeout(id);
}, [draft]);
```

### Third-Party Library Integration

```jsx
useEffect(() => {
  const widget = Library.init(ref.current, options);
  return () => widget.destroy();
}, [ref, options]);
```

---

## Cleanup Functions

Always return a cleanup function for:

* Subscriptions
* Timers / intervals
* Event listeners
* Abort controllers

This prevents memory leaks and ensures effects don’t duplicate work.

---

## Common Pitfalls & Fixes

* **Infinite Loops:** Avoid updating state unconditionally in an effect without proper dependencies.
* **Stale Closures:** Always include dependencies to avoid referencing outdated values.
* **Missing Cleanup:** Always clean up subscriptions or timers to prevent leaks.
* **Object/Array Dependencies:** Use `useMemo` to prevent unnecessary effect runs.

---

## Performance Considerations

* Minimize dependencies to essential values.
* Avoid heavy synchronous work inside effects.
* Memoize expensive values/functions using `useMemo` or `useCallback`.
* Use conditional logic inside effects to skip unnecessary work.

---

## Best Practices

* Include all external values in dependency arrays.
* Use cleanup functions for subscriptions and timers.
* Use `useLayoutEffect` only when you need synchronous DOM measurements.
* Convert repeated/complex effects into custom hooks.
* Comment when intentionally omitting ESLint `exhaustive-deps` rules.
* Use `AbortController` to cancel fetch requests if needed.

---
