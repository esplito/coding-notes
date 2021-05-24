
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
>
> We'll be using a tool called [MSW](https://mswjs.io/) for this.

> Something that's really important with the server handlers is that you simulate the exact behavior of your backend. You try to do that with as much robustness as possible. One thing that our backend does is it will send a 400 response if a required username or password is not provided.

**Extra credit 3: use inline snapshots for error messages**
>Copy and pasting output into your test assertion (like the error message in our last extra credit) is no fun. Especially if that error message were to change in the future.
>Instead, we can use a special assertion to take a "snapshot" of the error message and Jest will update our code for us. Use `toMatchInlineSnapshot` rather than an explicit assertion on that error element. ([Snapshot Testing](https://jestjs.io/docs/en/snapshot-testing))

Dodds prefers to use `toMatchInlineSnapshot()` for error messages, because then jest will check if the thing that we are expecting is the same as in previous test suite. If we would change what we have in our `expect` then jest will ask us wether we want to update the snapshot. So if there's nothing wrong with our change we can just tell jest to update our test.

`expect` before changes:
```js
expect(screen.getByRole('alert')).toMatchInlineSnapshot(`
<div
	role="alert"
	style="color: red;"
>
password required
</div>
`)
```

Message displayed when run `npm t` in watch-mode:
![Screenshot of how jest asks us what to do when snapshot fails](https://github.com/esplito/coding-notes/blob/master/snapshot_suggestion_jest.png?raw=true)

`expect` after changes and pressing 'u':
```js
expect(screen.getByRole('alert').textContent).toMatchInlineSnapshot(
`"password required"`,
)
```

**Extra credit 4: use one-off server handlers**
If we want to test one-off cases, for example when the server fails for an unknown reason, we can use `server.use` in MSW.

Example from Dodds:
```javascript
server.use(
	rest.post(
		// note that it's the same URL as our app-wide handler
		// so this will override the other.
		'https://auth-provider.example.com/api/login',
		async (req, res, ctx) => {
		// your one-off handler here
		},
	),
)
```

>One thing to keep in mind here is we've added a server handler for this request, effectively overriding the existing request. Things are working OK right here because this is the last test in our file, but if I move this up to the very top and make it the first test in our file, then sure this test will pass just fine, but my other tests are totally going to fail.
>The reason that they fail is because every time we make this POST request, we're going to respond with a 500. We need to clear this out somehow. One way we can do that is say server reset handlers. Now my tests are passing with flying colors. - Dodds

Solution to not break other tests. Use `afterEach(() => server.resetHandlers())`.

### Mocking Browser APIs and Modules
> We're not running our test in a real browser. They're a simulated browser in Node using a module called jsdom. Sometimes there are things that jsdom isn't able to do that you have to just fake out.
> 
> For example, matchMedia, which is supported on Windows is not supported in jsdom. You have to include a polyfill for that and then also, we monkey-patch-resize too on Windows so that we can test out things that rely on matchMedia. That's just one of several examples of APIs that jsdom which you might need to test for your particular application. - Dodds

> In my [Advanced React Hooks workshop](https://kentcdodds.com/workshops/advanced-react-hooks), I teach something using a custom `useMedia` hook and to test it, I have to mock out the browser `window.resizeTo` method and polyfill `window.matchMedia`.
> Here's how I go about doing that:
>```javascript
>
>import  matchMediaPolyfill  from  'mq-polyfill'
>  
>beforeAll(()  =>  {
>  matchMediaPolyfill(window)
>  window.resizeTo  =  function  resizeTo(width, height)  {
>  Object.assign(this, {
>    innerWidth: width,
>    innerHeight: height,
>    outerWidth: width,
>    outerHeight: height,
>  }).dispatchEvent(new  this.Event('resize'))
>  }
>})
>```
>This allows me to continue to test with Jest (in node) while not actually running in a browser. - Dodds

You can mock parts of a module by providing your own mock module getter-function:
```javascript
jest.mock('../math', () => {
  const  actualMath  = jest.requireActual('../math')
  return {
    ...actualMath,
    subtract: jest.fn(),
  }
})

// now the `add` export is the normal function,
// but the `subtract` export is a mock function.
```

More about jest mocking here: https://github.com/kentcdodds/how-jest-mocking-works

If you get an act-warning, read how to fix it here: https://kentcdodds.com/blog/fix-the-not-wrapped-in-act-warning



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjQzNDU0NjExLDEyMzkzNjEwMDgsOTkwNj
E0MzAxLDQwNDA2OTg2LC02MzIyOTE1OTQsLTE1ODAyMjg5MzUs
LTM3MjI0NTM0OSw2ODQ3OTkzNjUsMTk1NTU2MzQ5MiwtMTE0Nj
ExNDQ5NywtMjI2NzU5NjczLC01NDMzNTk4ODQsLTQ1MzYxNzY3
NywtNDMzODQ2NTExLC0yMTE0NzY4ODgwLDI2ODUxMzI4NSwyMT
M3NDE2NDM1LC0xNzI2NDIwODU1LDcyMjgyNTI4NywyMTEwODgz
NDM0XX0=
-->