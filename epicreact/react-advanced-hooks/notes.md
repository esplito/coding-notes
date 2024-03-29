## Advanced React Hooks

### useReducer: simple Counter

Code example from Kent C. Dodds:
>```javascript
>function nameReducer(previousName, newName) {
>  return newName
>}
>
>const initialNameValue = 'Joe'
>
>function NameInput() {
>  const [name, setName] = React.useReducer(nameReducer, initialNameValue)
>  const handleChange = event => setName(event.target.value)
>  return (
>    <>
>      <label>
>        Name: <input defaultValue={name} onChange=>{handleChange} />
>      </label>
>      <div>You typed: {name}</div>
>    </>
>  )
>}
>```

Important to note in the code above is that we call the reducer with two arguments:
1. Current state
2. Whatever that you call the dispatch function (`setName` above) with. Often called an "action"

###  `useState` or `useReducer`?
Resources:
* https://kentcdodds.com/blog/should-i-usestate-or-usereducer
* https://kentcdodds.com/blog/how-to-implement-usestate-with-usereducer

When you have single piece of state to manage, you're almost certainly better off using `useState` rather than `useReducer`.

**General rule**: When one element of your state relies on the value of another  state to update itself, they should be in a reducer.

### useCallback: custom hooks

Resources:
* [Memoization and React](https://epicreact.dev/memoization-and-react/)
* [When to useMemo and useCallback](https://kentcdodds.com/blog/usememo-and-usecallback)

**Note regarding the use of useLayoutEffect in exercise 2, extra credit 3**
> `useLayoutEffect`: This will ensure that this function is going to be called as soon as we're mounted without waiting for the browser to paint the screen and it will also ensure that this cleanup is called as soon as we're unmounted without waiting for anything either.
> 
> Code:
> ```js
> React.useLayoutEffect(() => {
>  mountedRef.current  =  true
>  // cleanup function that's called when un-mounted
>  return () => {
>    mountedRef.current  =  false
>  }
>}, [])
> ```

Kent C. Dodds also talks about the issue with using useCallback and the fact that we have to put dispatch in our dependencies in the `run`-function and in the `useSafeDispatch`:

> "This is the reason why I don't suggest that you use useCallback for every function you define in your application because as you can see, it spiders all over your code base when you start memoizing things. While it is valuable to do in some instances, only do it when you're deriving the value." - Dodds

### useContext: simple Counter
Sometimes, to avoid prop drilling, you can use useContext. It enables us to insert som state into a part of our react tree and then extract that anywhere within that tree, without having to explicitly pass it everywhere.

Kent's recommendations:
>Most of the time, I don’t recommend using a default value because it’s probably a mistake to try and use context outside a provider, so in our exercise I’ll show you how to avoid that from happening.
>
>🦉 Keep in mind that while context makes sharing state easy, it’s not the only solution to Prop Drilling pains and it’s not necessarily the best solution either. React’s composition model is powerful and can be used to avoid issues with prop drilling as well. Learn more about this from  [Michael Jackson on Twitter](https://twitter.com/mjackson/status/1195495535483817984) 
>
>We’re putting everything in one file to keep things simple, but I’ve labeled things a bit so you know that typically your context provider will be placed in a different file and expose the provider component itself as well as the custom hook to access the context value.
>
>A common mistake of context (and generally any “application” state) is to make it globally available anywhere in your application when it’s actually only needed to be available in a part of the app (like a single page). Keeping a context value scoped to the area that needs it most has improved performance and maintainability characteristics.

Creating a custom hook for consumers of a context is recommended. With the custom hook you can specify a more meaningful error that for example tells you `useCount must be used within a CountProvider`, when you have missed to wrap the relevant components with a `<CountProvider></CountProvider>`.

Other resources:
* [Using Composition in React to avoid "Prop Drilling"](https://www.youtube.com/watch?v=3XaXKiXtNjw)

### useLayoutEffect: auto-scrolling textarea

The simple rule for when you should use `useLayoutEffect` according to Kent:
> If you are making observable changes to the DOM, then it should happen in `useLayoutEffect`, otherwise `useEffect`.

More here: https://kentcdodds.com/blog/useeffect-vs-uselayouteffect

TLDR of that article:
> -   **useLayoutEffect:**  If you need to mutate the DOM and/or  **do need**  to perform measurements
>-   **useEffect:**  If you don't need to interact with the DOM at all or your DOM changes are unobservable (seriously, most of the time you should use this).

In the example exercise. If you switch `useEffect` to `useLayoutEffect` in the `SloooowSibling`-component the interactivity will become janky again. My guess is that this is because the browser will have to wait for this slow component before it paints the content. 

### useImperativeHandle: scroll to top/bottom

> You are not going to be using this hook very often, and anytime you do, you should probably document exactly why it's necessary because normally it's not.
>  
>  **NOTE**: most of the time you should not need  `useImperativeHandle`. Before you reach for it, really ask yourself whether there’s ANY other way to accomplish what you’re trying to do. Imperative code can sometimes be really hard to follow and it’s much better to make your APIs declarative if possible. For more on this, read  [Imperative vs Declarative Programming](https://tylermcginnis.com/imperative-vs-declarative-programming/)

Other resources:
* https://reactjs.org/docs/hooks-reference.html#useimperativehandle

### useDebugValue: useMedia

> "`useDebugValue`, this is useful only for your custom hooks, you can't really use this inside of components directly. What this is for is for the React DevTools browser extension."

When writing custom hooks and when you want to differentiate different usages of a custom hook in a given component, then useDebugValue is handy.

You can use it like this:
>```javascript
>function useCount({initialCount = 0, step = 1} = {}) {
>  React.useDebugValue({initialCount, step})
>  const [count, setCount] = React.useState(0)
>  const increment = () => setCount(c => c + 1)
>  return [count, increment]
>}
>```

When they use `useCount` they'll see `initialCount` and `step` values in the devTools.

Screenshots of the difference when using `useDebugValue` in the exercise with the `useMedia`-hook.

**Before**

![Before useDebugValue](https://github.com/esplito/coding-notes/blob/master/before_useDebugValue.png?raw=true)

**After**

![After useDebugValue](https://github.com/esplito/coding-notes/blob/master/after_useDebugValue.png?raw=true)

It's just for labelling within custom hooks. It **doesn't work** in components!

Dodds comments regarding the format function that you can pass as a second argument to `useDebugValue`:
>"The only time you would ever actually use this is if calculating that DebugValue is somehow expensive, and you want to optimize that away."

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTU3MDg5ODM2MSwtMTYxNTc3NTYxLDgzMz
E3OTQ0NCwtMTIxMjU3MTMzMiwtMTExODUwNTg2NiwtMTE0MTEx
ODU4MiwtODM4MTQzNTIsLTE4MTM2Njc4NDUsNDMxNjczNTIxLC
01NDY1ODc2MTMsLTE3NDQ1NzY4MDYsLTc3ODMzMDI2NCwtODUx
ODYzODM5LDIxMzQxMzIwMywtNzI2NTYxMjQzLDY1MDQ1Mjc1MS
wyMjI3ODQ4NTMsMTQ3NTg1MjYxLC0zNTQwODQyNTIsMTgxNjA0
NTA1Ml19
-->