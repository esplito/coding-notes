## React Suspense

### ⚠ This is still experimental! ⚠ 

### Concurrent Mode

> I use ReactDOM.render. To use React Concurrent Mode, though, I need to use the new ReactDOM.createRoot API. This createRoot API accepts our root element. - Dodds
> 
> This implies that you can only enable Concurrent Mode for the entire application starting at the root element where you're mounting your initial UI. You cannot opt into Concurrent Mode in different components through your application. - Dodds

### Simple Data-fetching

**Extra credit 1: Add error handling with an Error Boundary**
>In review, all that you need to do to handle errors with React Suspense and Concurrent Mode is make sure that the asynchronous thing that you're doing has an error handler, keep track of that error, and then if there is an error, go ahead and throw it and then your ErrorBoundary can handle it for you as well. - Dodds

### Render as you fetch

>React Concurrent Mode and Suspense enable and encourage following a pattern called Render-as-You-Fetch. - Dodds

There are three different ways (which Dodds shows) that we use in React when fetching data when rendering.

1. Fetch-on-render We kind of get a waterfall effect when doing this. 
	Example:
	We load the code for the PokemonInfo-module -> Then we make the request for PokemonInfo from the GraphQL Pokemon API -> 
2. Fetch-then-render
3. Render-as-you-fetch



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTgwODIyMDk5MywyMjYwMDgwNDUsLTE2MT
EwODA5ODksLTgyNTUxMTU4M119
-->