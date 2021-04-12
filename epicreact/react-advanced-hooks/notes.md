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

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQ3NTg1MjYxLC0zNTQwODQyNTIsMTgxNj
A0NTA1Miw1NjIzMDE3MTUsLTg1ODAyNzg1OV19
-->