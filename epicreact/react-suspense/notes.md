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

1. **Fetch-on-render**: We kind of get a waterfall effect when doing this. 

	**Example**:
	We load the code for the PokemonInfo-module -> Then we make the request for PokemonInfo from the GraphQL Pokemon API -> When that's all loaded, we load the pikachu image
	Code: https://github.com/kentcdodds/react-suspense/blob/main/src/examples/fetch-approaches/fetch-on-render.js
	
2. **Fetch-then-render**

	**Example:** 
With this we are still getting a waterfall. The only thing that has changed is what is loading first.
Make the GraphQL-request -> Load the code -> Load the image
	Code: https://github.com/kentcdodds/react-suspense/blob/main/src/examples/fetch-approaches/fetch-then-render.js

3. **Render-as-you-fetch**
This is quite different to the other approaches. With this we are loading all at once.
	> The whole idea behind Render-as-You-Fetch is you start fetching the data as soon as you have all the information that you need to start fetching the data. - Dodds

	Code: https://github.com/kentcdodds/react-suspense/blob/main/src/examples/fetch-approaches/render-as-you-fetch.js





> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjAwMjcwMzY5Niw1NzU3MzY3OTIsMjI2MD
A4MDQ1LC0xNjExMDgwOTg5LC04MjU1MTE1ODNdfQ==
-->