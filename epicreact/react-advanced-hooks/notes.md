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


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTg1ODAyNzg1OV19
-->