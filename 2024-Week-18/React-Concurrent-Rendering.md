*Status: In Progress*

Note: **React 18**

A typical state update block the main task. Width [Concurrent Rendering](https://react.dev/blog/2022/03/29/react-v18#what-is-concurrent-react) we can explicitly mark some state updates and the re-rendering caused by them as "non-critical". As a result, React will calculate these updates in the "background" instead of blocking the main task. If something "critical" happens (i.e., a normal state update), React will pause its "background" rendering, execute the critical update, and then either return to the previous task or abandon it completely and start a new one.

"Background" is just a useful mental model here, of course: JavaScript is single-threaded. React will just periodically check the main queue while it's busy with the "background" task. If something new appears in the queue, it will take priority over the "background" work.

We can do this with useTransition and useDeferredValue hook.

# useTransition

        const [isPending, startTransition] = useTransition();

If I wrap the state update in a transition, React doesn't just trigger the state update on the "background". It's actually a two-step process. First, an immediate "critical" re-render with the old state is triggered, and the boolean `isPending` that we extract from `useTransition` hook moves from `false` to `true`. Only after that critical "traditional" re-render has finished, will React start with the non-critical state update.

In short, useTransition [causes two re-renders](https://github.com/facebook/react/issues/24269) instead of one. If I'm on the heavy component and trigger something that render lightweight component, the initial re-render is triggered with state still being heavy component. The very heavy component blocks the main task for 1 second while it re-renders. Only after this is done will the non-critical state update from heavy to lightweight be scheduled and executed.

## How to use useTransition properly
### Memoization to everything
In order to fix the performance degradation from the above, we need to make sure that the additional first re-render is as lightweight as possible. Typically, it would mean that we need to memoize everything that can slow it down:
- All heavy components should be wrapped in React.memo, with their props memoized with useMemo and useCallback
- All heavy operations are memoized with useMemo
- `isPending` is not passed as a prop or dependency to anything from the above

Or Transition from "nothing" to "heavy". For example, first re-render does not have any data when component trying to fetch data after component mounted.

But this just shows that useTransition is definitely not a tool for everyday use: one small mistake in memoization, and you're making your app visibly worse than it was before useTransition.

# useDeferredValue
Is a React Hook that lets you defer updating a part of the UI.
It's usually recommended for use when you don't have access to the state update function. When the value is coming from props, for example.
However, the problem of double rendering is present here as well.
So the solution here will be exactly the same as for useTransition. Mark updates as non-critical only if:
- Everything that is affected is memoized;
- Or if we're transitioning from "nothing" to "heavy", and never in the opposite direction;

# Conclusion:
- Concurrent rendering hooks cause double re-renders. So, never use them for all state updates. Their use case is very specific and requires a very deep understanding of the React lifecycle, re-renders, and memoization.

This article is not yet done. It will be updated in the future.

Source:
- https://www.developerway.com/posts/use-transition
- https://react.dev/reference/react/useTransition
- https://react.dev/reference/react/useDeferredValue
