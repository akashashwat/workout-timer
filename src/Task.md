so this exercise is to find the performance issue in the workout app,

first using profiler target whats the issue,

using profiler I found that there is a App component, inside which there are three components a timer component, a toggleSound component and a calculator component.

the timer using useEffect, so its rendering every second, and since its inside App component therefore App component is also re-rendering every second causing performance issue.

since App component is re-rendering every second, all the compoments inside it is re-rendering every seconds causing performance issue.

so to fix this performance issue, we can memoized components.

as props inside the ToggleSound component isn't changing, we can memoize this component using memo. so that it will not render every second.

now we memoize Calculator component, but doing so still re-renders calculator component ever second because it contains a prop named workouts, which is an array of objects, and as we know:

# in React, everything is re-created on every re-render (inc. objects and functions)

# in Javascript, two objects or functions that looks same are actually different. therefore,

# if objects or functions are passed as props, the child component(here calculator) will always sees them as new props on each re-render.

# if props are different between re-renders, memo will not work.so the solution is

# we need to memoize objects and functions to make them stable(preserve) between re-renders and then pass them as props.

now we will memoize the workouts object using useMemo and as a callback function we will return the object (prop), so that prop will be stable between re-render.

but doing so still does not fix problem because as we update any of 4 states, effect in calculator runs again causing two renders.

we memoize playSound function using useCallback, but doing so creates another problem that whenever we set timer manually and we click on sound button, timer gets reset, so basically effect is unable to synchronice between duration and playSound.

so to fix this we use another effect to synchronise playSound function with duration state.

// Memo, useMemo and useCallback

# we use memo to memoize a component.

# we use useMemo to memoize a function or object with a callback and return that object.

# we use useCallback to memoize a function so that it will be stable between re-renders.
