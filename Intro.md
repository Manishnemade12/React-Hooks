# React Hooks 

React Hooks were introduced in **React 16.8** to let developers use **state** and other React features inside **functional components**. Before hooks, only **class components** could use state and lifecycle methods. Hooks make React code **simpler, reusable, and more powerful**.

---

## 🔹 What are React Hooks?

* React Hooks are **functions** that let you “hook into” React features like state and lifecycle.
* They allow us to write **functional components with state and side effects**.
* With hooks, we no longer need class components for most cases.

---

## 🔹 Pros of Hooks

- Easier to understand than classes.  
- Reusable logic with **custom hooks**.  
- Cleaner and shorter code.  
- Better separation of concerns (logic grouped together).  
- Functional components + hooks are now the React standard.  

---

## 🔹 Cons of Hooks

- Beginners may find dependency arrays (`useEffect`) confusing.  
- Overuse of `useMemo`/`useCallback` can make code complex.  
- Debugging hook order issues can be tricky.  
- Some developers prefer class lifecycle methods for clarity.  


## 🔹 Basic Hooks

1. **[useState](./useState.md)** – Manages state inside functional components.
2. **[useEffect](./UseEffect.md)** – Handles side effects like data fetching, subscriptions, or timers.
3. **[useContext](./useContext.md)** – Consumes values from React Context without prop drilling.

---

## 🔹 Additional Hooks

4. **[useReducer](./useReducer.md)** – Manages complex state logic using a reducer function.
5. **[useMemo](./useMemo.md)** – Memoizes computed values to optimize performance.
6. **[useRef](./useCallback.md)** – Memoizes computed function.
---
