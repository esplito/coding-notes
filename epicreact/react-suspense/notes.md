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


### useTransition for improved loading states

>This is an intentional feature of React and Concurrent Mode that it will just wait for a little bit before it decides to render out your fallback here. - Dodds
>
(He refers to the example where React waits a bit before displaying the fallback. https://github.com/kentcdodds/react-suspense/blob/main/src/exercise/03.js)

Example: https://react-suspense.netlify.app/3

>📣 **BREAKING CHANGE ALERT:** The version of React in this project works like it does in the recorded videos. However in the future experimental builds of React, the `SUSPENSE_CONFIG` option to `useTransition` has been completely removed. Read more about this here: [https://github.com/facebook/react/pull/19703](https://github.com/facebook/react/pull/19703)

**startTransition and isPending**

>The important thing to remember around of this is that a React Suspense Boundary will immediately show the fallback, regardless of any useTransition, startTransition function when it's initially mounted. The first time it's mounted, it will show you that fallback, regardless of any pending state. - Dodds

If you don't want the behaviour that Dodds explains above, you'll have to move Suspense outside of the ternary, but still inside the div that has `isPending`.  (Code referenced here: https://github.com/kentcdodds/react-suspense/blob/main/src/final/03.js)

> The idea is that in our Suspense config, we will be able to say, "Hey, React. I don't want you to show this loading state for 4,000 ms, so I'm going to handle that loading state myself with this isPending thing." - Dodds

### Cache Resources

> Creating a new promise in the render method is dangerous because you cannot rely on your render method only being called once, so you have to do things carefully by using a promise cache. - Dodds

More info about the above can be found here: https://react-suspense.netlify.app/4

⚠ ⚠ ⚠ 
>In real apps, you’ll likely be using a robust abstraction for caching. A good one to try out is [react-query](https://github.com/tannerlinsley/react-query). - Dodds

// Extra credit 2: create a context provider

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTc2OTg5NzM2OCw5ODMxMzA0MTIsLTI1MD
Y0MzAzLC0xNDM1Mjc5OTEzLC0xNDIxOTg3MTI5LC02NzQ4ODQ2
ODgsLTE3MDQ2NjA1MDQsNjk1MzQxNzQ1LC0yMzM2MTU2OCwtMT
g5MTAzODY3MCwyMDAyNzAzNjk2LDU3NTczNjc5MiwyMjYwMDgw
NDUsLTE2MTEwODA5ODksLTgyNTUxMTU4M119
-->