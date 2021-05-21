
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

[React Testing Library](https://testing-library.com/react) removes the need for us to write boilerplate code that is not so relevant to us as developers (like wrapping stuff in the `act`-function and  writing cleanup-functions).

> As we're rendering, this React Testing Library will keep track of all the divs. As we're rendering, this React Testing Library will actually keep track of all the divs that it's creating. It will remove those divs from the document.body, as well as unmount the components that were rendered. - Dodds

To get more assertions that you can use in your tests you can import Testing Library´s own suite of assertions for jest: [`@testing-library/jest-dom`](http://testing-library.com/jest-dom).


### Avoid Implementation Details

> One of the most important things to remember about testing our software the way it is used is to avoid testing implementation details. "Implementation details" is a term referring to how an abstraction accomplishes a certain outcome. Thanks to the expressiveness of code, you can typically accomplish the same outcome using completely different implementation details. - Dodds

> The implementation of your abstractions does not matter to the users of your abstraction and if you want to have confidence that it continues to work through refactors then **neither should your tests.** - Dodds

More about implementation details here: [Testing Implementation Details](https://kentcdodds.com/blog/testing-implementation-details)

> You can tell whether tests rely on implementation details if they're written in a way that would fail if the implementation changes. For example, what if we wrapped our counter component in another `div` or swapped our message from a `div` to a `span` or `p`? Or what if we added another button for `reset`? Or what if instead of a `button` we switched to a clickable (and accessible) `div`? (That's not an easy thing to do, so I recommend just using a button, but the point is hopefully clear). - Dodds

If we want to be truly implementation detail free we should fire all the events that normally happens when a user clicks something. With `fireEvent` we only fire one event, `click`, but normally there would be some MouseEvents also, like `mouseover`, `mousedown`, `mouseup` etc. We can do this with `userEvent` which can be imported from `@testing-library/user-event`.

If you want learn more about how the click-method in `userEvent` works, check out the code here: https://github.com/testing-library/user-event/blob/1af67066f57377c5ab758a1215711dddabad2d83/src/index.js#L109-L131

It is preferable to query elements by their role or by text, but how do you find the role of the element that you want to test? Open up the DevTools in Google Chrome and look for the tab "Accessibility". Here you can find the "Name" which is read by screen readers and you can also find the "role" of an element.

![Screenshot showing how you can find accessibility properties on a HTML element](https://github.com/esplito/coding-notes/blob/master/accessibility_in_chrome.png?raw=true)

Test example where we get a button by it's role and name:
```js
const decrementBtn = screen.getByRole('button', { name: /decrement/i})
```
Here's how the button looks in React:
```jsx
<button  onClick={decrement}>Decrement</button>
```

Great site for trying out testing library: https://testing-playground.com/

### Form Testing

**Extra credit 1: use a jest mock function**
Instead of us having to re-create function that are used in the component that we are testing, we can use jest's built-in mock functions API, `jest.fn()`: https://jestjs.io/docs/mock-function-api.

**Extra credit 2: generate test data**
Here's what Kent says about how you should generate the test data in your tests.

>An important thing to keep in mind when testing is simplifying the maintenance of the tests by reducing the amount of unrelated cruft in the test. You want to make it so the code for the test communicates what's important and what is not important.
> ```javascript
> const  username  =  'chucknorris'
>const  password  =  'i need no password'
>```

>Does my code behave differently when the username is `chucknorris`? Do I have special logic around that? Without looking at the implementation I cannot be completely sure. What would be better is if the code communicated that the actual value is irrelevant. But how do you communicate that? A code comment? Nah, let's generate the value!
>```javascript
>const  username  =  getRandomUsername()
>const  password  =  getRandomPassword()
>```

>That communicates the intent really well. As a reader of the test I can think: "Oh, ok, great, so it doesn't matter what the username _is_, just that it's a typical username."

It might be even better to do like this and create a function that returns both username and password:
```javascript
const {username, password} =  buildLoginForm()
```

**Extra credit 3: allow for overrides**
Sometimes you have data you want to be specific in a test, like for example that a password has to be of a certain strength. Then it is good if our `buildLoginForm`-function accepts overrides also.

Like this:
```javascript
const {username, password} =  buildLoginForm({password: 'abc'})
// password === 'abc'
``` 
> That communicates the reader of the test: "We just need a normal login form, except the password needs to be something specific for this test."

**Extra credit 4: use Test Data Bot**
> Sometimes, your object factories can be a little bit more complicated. To help with that a little bit, there's a module that I like to use called jackfranklin/test-data-bot.

### Mocking HTTP requests
> For our Integration and Unit component tests, we're going to trade-off some confidence for convenience and we'll make up for that with E2E tests. So for all of our Jest tests, we'll start up a mock server to handle all of the `window.fetch` requests we make during our tests.
> 
>To handle these fetch requests, we're going to start up a "server" which is not actually a server, but simply a request interceptor. This makes it really easy to get things setup (because we don't have to worry about finding an available port for the server to listen to and making sure we're making requests to the right port) and it also allows us to mock requests made to other domains.
> We'll be using a tool called [MSW](https://mswjs.io/) for this.
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbOTkxNTg4NzczLDE5NTU1NjM0OTIsLTExND
YxMTQ0OTcsLTIyNjc1OTY3MywtNTQzMzU5ODg0LC00NTM2MTc2
NzcsLTQzMzg0NjUxMSwtMjExNDc2ODg4MCwyNjg1MTMyODUsMj
EzNzQxNjQzNSwtMTcyNjQyMDg1NSw3MjI4MjUyODcsMjExMDg4
MzQzNCw1MjQ5MjY5MTYsLTE0MzgxMjU4NzksMzg2MDUxMDk1LD
E4NzcwNTY0MzYsMTQ1MTI2ODIwNyw2MzQ4OTU2ODQsLTMxNzI4
MTk0OV19
-->