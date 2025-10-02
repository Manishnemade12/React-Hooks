# useLayoutEffect — Complete Deep Research

A detailed guide on React's `useLayoutEffect` hook: how it works, its effect on rendering, real-world applications, and best practices.

---

## 1. What is useLayoutEffect?

`useLayoutEffect` is a React Hook that **fires synchronously after all DOM mutations but before the browser paints**.

- It is essentially the **synchronous version of `useEffect`**.
- Useful for **performing DOM measurements** or **imperative DOM mutations** that need to happen before the user sees the visual update.
- It runs after React has performed its DOM updates but *before* the browser has a chance to visually update the screen.

> In short: `useLayoutEffect` is for effects that need to read from or imperatively change the DOM layout immediately after React has updated it, ensuring the user doesn't see an inconsistent state.

---

## 2. How useLayoutEffect Works & Rendering Impact

1. **React Renders:** React performs its virtual DOM diffing and updates the actual DOM.
2. **`useLayoutEffect` Runs:** Synchronously, `useLayoutEffect` callbacks are fired. This is where you can read layout from the DOM or make imperative changes.
3. **Browser Paints:** After `useLayoutEffect` completes, the browser performs its repaint, making the updates visible to the user.
4. **`useEffect` Runs:** Asynchronously, after the browser has painted, `useEffect` callbacks are fired.

- **Blocking Nature:** `useLayoutEffect` is blocking. This means that if you perform a long-running operation inside it, it will block the browser's painting, potentially causing a visual flicker or performance degradation.
- **When to use:** Prefer `useEffect` for most side effects. Only use `useLayoutEffect` when you need to prevent visual inconsistencies due to DOM measurements or mutations.

> The key difference: `useLayoutEffect` runs *before* the browser paints, `useEffect` runs *after*.

---

## 3. Syntax and Common Patterns

### Basic Syntax

```jsx
import React, { useLayoutEffect, useRef, useState } from 'react';

function MeasureElement() {
  const ref = useRef(null);
  const [height, setHeight] = useState(0);

  useLayoutEffect(() => {
    if (ref.current) {
      setHeight(ref.current.offsetHeight); // Synchronously measure DOM
    }
  }, []); // Run once on mount

  return (
    <div ref={ref} style={{ border: '1px solid black', padding: '10px' }}>
      This is a div.
      <p>Height: {height}px</p>
    </div>
  );
}

export default MeasureElement;
```

### Imperative DOM Mutation Example

```jsx
import React, { useLayoutEffect, useState, useRef } from 'react';

function Tooltip() {
  const [tooltipVisible, setTooltipVisible] = useState(false);
  const buttonRef = useRef(null);
  const tooltipRef = useRef(null);

  useLayoutEffect(() => {
    if (tooltipVisible && buttonRef.current && tooltipRef.current) {
      const buttonRect = buttonRef.current.getBoundingClientRect();
      tooltipRef.current.style.left = `${buttonRect.left}px`;
      tooltipRef.current.style.top = `${buttonRect.bottom + 5}px`;
    }
  }, [tooltipVisible]); // Recalculate position when visibility changes

  return (
    <div>
      <button ref={buttonRef} onClick={() => setTooltipVisible(!tooltipVisible)}>
        Toggle Tooltip
      </button>
      {tooltipVisible && (
        <div
          ref={tooltipRef}
          style={{
            position: 'absolute',
            backgroundColor: 'lightgray',
            padding: '5px',
            border: '1px solid gray',
          }}
        >
          I am a tooltip!
        </div>
      )}
    </div>
  );
}

export default Tooltip;
```

---

## 4. Real-World Applications

- **Measuring DOM elements:** Getting the size or position of an element immediately after render to adjust layout.
- **Animating elements:** Performing an animation or transition that relies on initial DOM measurements to prevent flicker.
- **Synchronous DOM updates:** When you need to imperatively modify the DOM (e.g., setting scroll position) before the browser paints.

```jsx
//1. Measuring DOM elements (e.g., getting element height)
const ComponentWithMeasurements = () => {
  const ref = useRef();
  const [width, setWidth] = useState(0);

  useLayoutEffect(() => {
    if (ref.current) setWidth(ref.current.offsetWidth);
  }, []);

  return <div ref={ref}>Width: {width}px</div>;
};

//2. Adjusting scroll position after content renders
const ScrollableList = ({ items }) => {
  const listRef = useRef();

  useLayoutEffect(() => {
    if (listRef.current) {
      listRef.current.scrollTop = listRef.current.scrollHeight; // Scroll to bottom
    }
  }, [items]); // Scroll when items change

  return (
    <ul ref={listRef} style={{ height: '200px', overflowY: 'scroll' }}>
      {items.map((item, i) => <li key={i}>{item}</li>)}
    </ul>
  );
};

//3. Positioning a tooltip or modal based on a trigger element
// (As shown in the full example above)
```

---

## 5. Best Practices

- **Prefer `useEffect`:** Only use `useLayoutEffect` when `useEffect` causes a visual inconsistency (e.g., flicker due to layout shifts).
- **Keep it fast:** Because `useLayoutEffect` blocks painting, avoid computationally expensive operations inside it.
- **Dependency array is crucial:** Just like `useEffect`, ensure your dependency array is correct to avoid unnecessary re-runs or stale closures.
- **Use for imperative DOM interactions:** It's ideal for direct DOM manipulation or reading layout properties that need to be synchronized before the user sees the update.

```jsx
// ✅ Use for DOM measurements / imperative changes that must be synchronous
useLayoutEffect(() => {
  const rect = ref.current.getBoundingClientRect();
  // ... update styles based on rect ...
}, [dependencies]);

// ❌ Avoid for data fetching or subscriptions (use useEffect for these)
// useLayoutEffect(() => {
//   const subscription = subscribeToStore(); // This should be useEffect
//   return () => unsubscribe(subscription);
// }, []);
```

---

## 6. Additional Resources

- [React Docs: useLayoutEffect](https://react.dev/reference/react/useLayoutEffect)
- [Kent C. Dodds: When to useLayoutEffect](https://kentcdodds.com/blog/use-layout-effect)

