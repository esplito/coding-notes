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
  squares: JSON.parse(window.localStorage.getItem('squares') || Array(9).fill(null)
}
```
Functional Component with `useState`:
```js
// without lazy initialization
const [squares, setSquares] = React.useState(
  JSON.parse(window.localStorage.getItem('squares') || Array(9).fill(null)
);

// with lazy initialization
const [squares, setSquares] = React.useState(
  () => JSON.parse(window.localStorage.getItem('squares') || Array(9).fill(null)
);
```

### useRef and useEffect: DOM interaction

>After the component has been rendered, it’s considered “mounted.” That’s when the React.useEffect callback is called and so by that point, the ref should have its `current` property set to the DOM node. So often you’ll do direct DOM interactions/manipulations in the `useEffect` callback. - Kent C. Dodds

**Don't forget to add a cleanup function in case that the component gets unmounted, so that you don't have event handlers etc. hanging around. It could cause memory leaks in the browser.**

### useEffect: HTTP Requests

HTTP requests are a common side-effect that we need to do in our applications and this is no different from the side-effects we need to apply to a rendered DOM or when interacting with browser APIs lik localStorage. We do all of these within `useEffect`. It ensures that whenever certain changes take place, we apply the side-effects based on those changes.

>One important thing to note about the `useEffect` hook is that you cannot return anything other than the cleanup function. This has interesting implications with regard to async/await syntax:
>```js
>// this does not work, don't do this:
>React.useEffect(async () => {
>  const result = await doSomeAsyncThing()
>  // do something with the result
>})
>```

Why doesn't this work? Because when you make the function async it will automatically return a promise, regardless of if you return anything. This happens because of the semantics of async/await syntax. If you want to use async/await do like this:

>```javascript
>React.useEffect(() => {
>  async function effect() {
>    const result = await doSomeAsyncThing()
>    // do something with the result
>  }
>  effect()
>})
>```
>This ensures that you don’t return anything but a cleanup function.

Kent C. Dodds preference:
>🦉 I find that it’s typically just easier to extract all the async code into a utility function which I call and then use the promise-based `.then` method instead of using async/await syntax:
> ```javascript
>React.useEffect(() => {
>  doSomeAsyncThing().then(result => {
>    // do something with the result
>  })
>})
>```

**Error handling**
>```javascript
>// option 1: using .catch
>fetchPokemon(pokemonName)
>  .then(pokemon => setPokemon(pokemon))
>  .catch(error => setError(error))
>
>// option 2: using the second argument to .then
>fetchPokemon(pokemonName).then(
>  pokemon => setPokemon(pokemon),
>  error => setError(error),
>)
>```
>These are functionally equivalent for our purposes, but they are semantically different in general.
>
>Using  `.catch`  means that you’ll handle an error in the  `fetchPokemon`  promise, but you’ll  _also_  handle an error in the  `setPokemon(pokemon)`  call as well. This is due to the semantics of how promises work.
>
>Using the second argument to  `.then`  means that you will catch an error that happens in  `fetchPokemon`  only.

Instead of using a `isLoading`-state it's better to use a status that has one of the following values depending on how the request pan out:
-   `idle`: no request made yet
-   `pending`: request started
-   `resolved`: request successful
-   `rejected`: request failed

You could put them together like this if you want:
 ```js
const isLoading = status === 'idle' || status === 'pending
const isResolved = status === 'resolved'
const isRejected = status === 'rejected'
```

Resource: [Stop using isLoading booleans - Dodds](https://kentcdodds.com/blog/stop-using-isloading-booleans)

**Potential Problem (extra credit 3)**
>You’ll notice that we’re calling a bunch of state updaters in a row. This is normally not a problem, but each call to our state updater can result in a re-render of our component. React normally batches these calls so you only get a single re-render, but it’s unable to do this in an asynchronous callback (like our promise success and error handlers).
>
>So you might notice that if you do this:
>```javascript
>setStatus('resolved')
>setPokemon(pokemon)
>```
>You’ll get an error indicating that you cannot read  `image`  of  `null`. This is because the  `setStatus`  call results in a re-render that happens before the  `setPokemon`  happens.
>
>In the future, you’ll learn about how  `useReducer`  can solve this problem really elegantly, but we can still accomplish this by storing our state as an object that has all the properties of state we’re managing. 
>

**Error Boundaries**
Only available as a Class Component. Can be used to handle errors when a component crashes. If you haven't installed React with `create-react-app` you can install [# @babel/plugin-transform-react-jsx-source](https://www.npmjs.com/package/@babel/plugin-transform-react-jsx-source)

This enables you see exactly where in the component tree that the fail happened. You can also see the filenames and line numbers in the component stack trace.



<!--stackedit_data:
eyJoaXN0b3J5IjpbMTczMTQ4NzkwMCwtNzE3MDkwMzg3LDE5ND
MwOTk5NDcsLTIxMjk4MzA4NDcsNDA4MTE3MzI3LDI5MTUyNzAw
NSwtMTAwNzI5ODcyMiw2NTg3MzMzNjIsMjY0NDU2NDY1LDE4OD
E3NTA5MSwtMTA5OTU5MzgyOCwxNTQ0MzUzNTM2LDYzMzc5MTE0
MiwxMTYwNDc2ODYzLC0xMTMyODQwMzE0LDE1MjAzODExLC02Mj
ExOTYyMTgsNDY4ODYwNDAsLTc4MjExNjM5MiwtMzExNzM2NDky
XX0=
-->