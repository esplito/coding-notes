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
`React.useEffect` is a built-in hook that makes it possible to run some custom code after Re


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTg3MjY1NTYxOSwtNDY5Njc0NjU2LC0zNz
QyMzk4MzgsLTEzNjgyOTI4MiwtNjEwNTU1ODYzLC0zNDIxMzkx
ODNdfQ==
-->