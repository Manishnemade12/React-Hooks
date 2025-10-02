# useReducer — Complete Deep Research

A detailed guide on React's `useReducer` hook: how it works, its effect on rendering, real-world applications, and best practices.


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
# Real-World useReducer Logic Examples

```javascript
// Common initialState for different cases
const initialState = {}; 
const [state, dispatch] = useReducer(reducer, initialState);

```

1. Form Handling

```javascript
dispatch({ type: 'update', field: 'name', value: 'John' });
dispatch({ type: 'setError', errors: { email: 'Invalid' } });

```

2. Todo Apps
```javascript
dispatch({ type: 'add', todo: newTodo });
dispatch({ type: 'remove', id: 1 });
dispatch({ type: 'update', id: 2, todo: { text: 'Updated' } });

```

 3. Complex UI States
```javascript
dispatch({ type: 'toggleModal' });
dispatch({ type: 'nextStep' });
dispatch({ type: 'notify', message: 'Saved!' });

```

4. Game State
```javascript
dispatch({ type: 'score', points: 10 });
dispatch({ type: 'levelUp' });
dispatch({ type: 'loseLife' });

```
---


## 8. Best Practices

- Keep the reducer pure (no side effects inside it).
- Use action objects with type and optional payload.
- Centralize related state updates into one reducer.
- Combine with `useContext` for global state management.
- Avoid large monolithic reducers; split if necessary.
- Comment and document action types for clarity.

```jsx
// 1. Keep reducer pure & centralized
function reducer(state, action) {
  switch(action.type) {
    case 'increment': return { count: state.count + 1 };
    case 'decrement': return { count: state.count - 1 };
    default: return state; // always return new state
  }
}

// 2. Setup global state with useReducer + useContext
const [state, dispatch] = useReducer(reducer, { count: 0 });
const { state: ctxState, dispatch: ctxDispatch } = useContext(CounterContext);

// 3. Dispatch actions with type (and optional payload)
ctxDispatch({ type: 'increment' });
ctxDispatch({ type: 'decrement' });

```


---

## 🔗 Additional Resources

- [React Docs: useReducer](https://react.dev/reference/react/useReducer)
- [React Docs: useContext](https://react.dev/reference/react/useContext)
- [Kent C. Dodds: useReducer vs useState](https://kentcdodds.com/blog/useReducer-vs-useState)
