
## Testing React Apps

### Intro
> Remember testing is 100 percent about how can you be confident that you can ship things. Another tip, when you're looking at a component that you want to test, step back for a second and think, "If I were a manual tester, how would I test this?" Then make your test do the thing that the manual tester would do. - Dodds

> So, what's a JavaScript test? It's simply some code which sets up some state, performs some action, and makes an assertion on the new state. - [But really, what is a Javascript test? - Dodds](https://kentcdodds.com/blog/but-really-what-is-a-javascript-test)

Other resource: https://kentcdodds.com/blog/but-really-what-is-a-javascript-mock

### Simple Test with ReactDOM

When we think about how an application is used, we have to consider who the users are.

Users:
1. The end users -> the ones who interact with our application (by clicking buttons etc.)
2. The developer users -> the ones who uses our code (rendering it, calling our functions etc.)

We want to avoid a third user creeping into our tests. More on this here: [Avoid the test user](https://kentcdodds.com/blog/avoid-the-test-user)

> When it comes to React components, our developer user will render our component with `ReactDOM` (similar concept for React Native) and in some cases they'll pass props and/or wrap it in a context provider. The end user will click buttons and assert on the output. 
> 
>So that's what our test will do. - Dodds


**Extra credit 1: use dispatchEvent**
In the extra credit we use `MouseEvent` and `dispatchEvent`. Resources on these can be found here:
* https://developer.mozilla.org/en-US/docs/Web/API/MouseEvent
* https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/dispatchEvent

It's important to clean up the DOM between each test. To do this we could write something like this:

```js
beforeEach(() => {
	document.body.innerHTML = '';
})
```

Note: If we use React Testing Library we would get lots of functionality without having to write them ourselves (like cleanup and click events etc.)

### Simple test with React Testing Library

React Testing Library removes the need for us to write quite a lot of code that is not so relevant to our tests. (like wrapping stuff in the `act`-function and d

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwNDU2MDg3OTAsMTQ1MTI2ODIwNyw2Mz
Q4OTU2ODQsLTMxNzI4MTk0OSwxNDU2MDgxNjE1LDI5NzgxODA0
MCwtMTk5NTQxNjA0NywxNTIxMTkyODYxXX0=
-->