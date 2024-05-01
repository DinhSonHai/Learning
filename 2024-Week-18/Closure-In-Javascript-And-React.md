
**Important point**

The function inside "closes" over all the data from the outside. It's essentially a snapshot of all the "outside" data frozen in time stored separately in memory.

- Closures are formed every time a function is created inside another function.
- Since React components are just functions, every function created inside forms a closure, including such hooks as useCallback and useRef.
- When a function that forms a closure is called, all the data around it is "frozen", like a snapshot.
- To update that data, we need to re-create the "closed" function. This is what dependencies of hooks like useCallback allow us to do.
- If we miss a dependency, or don't refresh the closed function assigned to ref.current, the closure becomes "stale".
- We can escape the "stale closure" trap in React by taking advantage of the fact that Ref is a mutable object. We can mutate ref.current outside of the stale closure, and then access it inside. Will be the latest data.

- [React.memo also accept a comparison function as second argument](https://react.dev/reference/react/memo#specifying-a-custom-comparison-function)

Source: https://www.developerway.com/posts/fantastic-closures
