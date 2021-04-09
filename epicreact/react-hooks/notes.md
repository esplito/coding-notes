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

Reading from localStorage every time a component function is run, can be a performance bottleneck (because reading from localStorage can be slow). We only need to read the value from localStorage the first time the component is rendered! 

> To avoid this problem, React’s useState hook allows you to pass a function instead of the actual value, and then it will only call that function to get the state value when the component is rendered the first time. So you can go from this:  `React.useState(someExpensiveComputation())`  To this:  `React.useState(() => someExpensiveComputation())`
> 
>And the  `someExpensiveComputation`  function will only be called when it’s needed!

If you put an object in the dependency array it will always be shallow compared because React sees it as a new object, even though it looks exactly the same.

### Hooks Flow

LayoutEffects are very similar to useEffect.

React doesn't call the `function Child()` until it knows that it needs to render it. That's why the parent component (`function App()`) finishes rendering before it renders the child component.

`useEffect` cleanups needs to re-run before it re-runs any of the `useEffect`s.

### Lifting state

Useful resources: [Lifting State up - React](https://reactjs.org/docs/lifting-state-up.html)

> There should be a single “source of truth” for any data that changes in a React application. 
> Source: [Lessons Learned - Lifting State up - React](https://reactjs.org/docs/lifting-state-up.html#lessons-learned)

> Whenever I'm looking at some state that's being managed, I like to determine what components are using the state, or what elements in this component are using the state. - Kent C. Dodds

Always think about where the state should be managed. **Colocation of state** can lead to performance benefits and makes maintenance easier.

### useState: tic tac toe

This example is lifted from React's official tutorial, but that one still uses Class Components and this one uses Hooks!

-   **Managed State:**  State that you need to explicitly manage
-   **Derived State:**  State that you can calculate based on other state

Useful resources: [Don't sync state. Derive it! - Dodds](https://kentcdodds.com/blog/dont-sync-state-derive-it)

When you're maintaining the state of your app and trying to solve a synchronization bug, think about how you could change it to make it derived on the fly instead.

One thing to note about state is that Class Component do lazy initialization by default, but if you refactor it to a Functional Component with `useState` then you have to make sure that you are lazy initializing.

Class Component example:
```js
state = {
  squares: JSON.parse(window.localStorage.getItem('squares') || Array(9).f
```

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTczNDAwMTEzNiwtMTA5OTU5MzgyOCwxNT
Q0MzUzNTM2LDYzMzc5MTE0MiwxMTYwNDc2ODYzLC0xMTMyODQw
MzE0LDE1MjAzODExLC02MjExOTYyMTgsNDY4ODYwNDAsLTc4Mj
ExNjM5MiwtMzExNzM2NDkyLDk4NDUyNjY3OCwyMTA5ODcwMzcx
LDExOTQ0NTk3MzEsLTQ2OTY3NDY1NiwtMzc0MjM5ODM4LC0xMz
Y4MjkyODIsLTYxMDU1NTg2MywtMzQyMTM5MTgzXX0=
-->