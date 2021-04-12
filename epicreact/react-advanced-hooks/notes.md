## Advanced React Hooks

### useReducer: simple Counter

Code example from Kent C. Dodds:
>```javascript
function nameReducer(previousName, newName) {
  return newName
}

const initialNameValue = 'Joe'

function NameInput() {
  const [name, setName] = React.useReducer(nameReducer, initialNameValue)
  const handleChange = event => setName(event.target.value)
  return (
    <>
      <label>
        Name: <input defaultValue={name} onChange={handleChange} />
      </label>
      <div>You typed: {name}</div>
    </>
  )
}
```

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTU3NDA1MTcxMV19
-->