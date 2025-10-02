# useEffect — Complete Deep Research

> A detailed guide on React's `useEffect` hook: how it works, its effect on rendering, real-world applications, and best practices.


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


## Best Practices

* Include all external values in dependency arrays.
```jsx
useEffect(() => {
  console.log(userId);
}, [userId]); // include external values

```

* Use cleanup functions for subscriptions and timers.
```jsx
  useEffect(() => {
  const id = setInterval(() => {}, 1000);
  return () => clearInterval(id); // cleanup
}, []);

```

* Use `useLayoutEffect` only when you need synchronous DOM measurements.
```jsx
  useLayoutEffect(() => {
  const w = ref.current.offsetWidth;
}, []);

```

* Convert repeated/complex effects into custom hooks.
```jsx
function useFetch(url) { /* logic */ }
const data = useFetch("/api");

```


---
