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

Kent C. Dodds also talks about the issue with using useCallback and the fact that we have to put dispatch in our dependencies in the `run`-function and in the useSafeDispatch

> "This is the reason why I don't suggest that you use useCallback for every function you define in your application because as you can see, it spiders all over your code base when you start memoizing things. While it is valuable to do in some instances, only do it when you're deriving the value." - Dodds

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjA2Nzc0NTgzNSwyMjI3ODQ4NTMsMTQ3NT
g1MjYxLC0zNTQwODQyNTIsMTgxNjA0NTA1Miw1NjIzMDE3MTUs
LTg1ODAyNzg1OV19
-->