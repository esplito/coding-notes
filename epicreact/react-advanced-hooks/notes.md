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

The sim
>

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2NzQwNTMxODAsLTc3ODMzMDI2NCwtOD
UxODYzODM5LDIxMzQxMzIwMywtNzI2NTYxMjQzLDY1MDQ1Mjc1
MSwyMjI3ODQ4NTMsMTQ3NTg1MjYxLC0zNTQwODQyNTIsMTgxNj
A0NTA1Miw1NjIzMDE3MTUsLTg1ODAyNzg1OV19
-->