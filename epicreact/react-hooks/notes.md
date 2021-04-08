## React Hooks

### Prerequisites

Watch [Why React Hooks](https://www.youtube.com/watch?v=zWsZcBiwgVE&list=PLV5CVI1eNcJgNqzNwcs4UKrlJdhfDjshf). (35 min)

**Notes - Why React Hooks**
* Intro to Hooks
* What makes React so hard?
	* JavaScript (Have to learn intricacies of javascript)
	* Lifecycles
	* Logic Reuse (patterns)
* RenderProps are powerful, but leads to a "pyramid of doom".
* Demo of a Geo-chat app in both Class Components and Hooks

### React Hooks Welcome
The backend is mocked with MSW, but you can uncomment code in backend.js if you want to use the live endpoint.

### useState: greeting

Common built-in hooks:
-   `React.useState`
-   `React.useEffect`
-   `React.useContext`
-   `React.useRef`
-   `React.useReducer`

Each hook has a unique api:
- Some return a value
	- `React.useRef` and `React.useContext`
- Others return a pair of values
	- `React.useState` and `React.useReducer`
- And some return nothing
	- `React.useEffect`

`React.useState` accepts a **single argument**, that is the initial state for the instance of the component.

`React.useState` returns a pair of values by returning an array with two elements. The first of the pair is the state value and the second is a function we can call to update the state.

**State** can be defined as:
>data that changes over time.

The entire component function is re-run (component re-render) when we update the state. This continues until the component is unmounted (removed from the app) or the user closes the app.

### useEffect: persistent state
`React.useEffect` is a built-in hook that makes it possible to run some custom code after React renders (and re-renders) the component to the DOM. It accepts a callback which React will call after the DOM has been updated.

```js
React.useEffect(() => {
  // your side-effect code here.
  // this is where you can make HTTP requests or interact with browser APIs.
})
```

[Hook flow](https://github.com/donavon/hook-flow) can be checked by inspecting the console here: `http://localhost:3000/isolated/examples/hook-flow.js`

Reading from localStorage every time a component function is run, can be a performance bottleneck (because reading from localStorage can be slow). We only need to read the value from 


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3ODcxMjM4MzQsOTg0NTI2Njc4LDIxMD
k4NzAzNzEsMTE5NDQ1OTczMSwtNDY5Njc0NjU2LC0zNzQyMzk4
MzgsLTEzNjgyOTI4MiwtNjEwNTU1ODYzLC0zNDIxMzkxODNdfQ
==
-->