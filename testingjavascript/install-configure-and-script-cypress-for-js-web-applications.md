
# Install, Configure, and Script Cypress for JavaScript Web Applications

## What's changed in Install, Configure, and Script Cypress for JavaScript Web Applications

- Kent updated some packages
- Most significant changes are in `@testing-library/cypress`

1. You no longer need to do chaining of `.findByText().click().findByText().click()...`
2. You no long need to use `queryBy` when asserting if an element is not in the DOM. You can use `findBy`! 🥳

## Intro to Install, Configure, and Script Cypress for JavaScript Web Applications

If you want to follow along, you can checkout the branches `tjs/cypress-<number-here>` (00-17). 

Example of the last lesson's branch: https://github.com/kentcdodds/jest-cypress-react-babel-webpack/tree/tjs/cypress-17

ℹ️ Most of the time is spent in the `e2e`-folder, but also in the `support`-folder.

## Install and Run Cypress

You can install it as a devDependency. 
```bash
npm install --save-dev cypress
```
To open cypress, run:
```bash
npx cypress open
```

### Get ESLint to work with Cypress

Install the ESLint plugin:
```bash
npm install --save-dev eslint-plugin-cypress
```
Create `.eslintrc.js` in your cypress-folder:
```js
// This includes some rules from Kent C. Dodds
module.exports = {
  root: true,
  plugins: ['eslint-plugin-cypress'],
  extends: ['kentcdodds', 'kentcdodds/import', 'plugin:cypress/recommended'],
  env: {'cypress/globals': true},
}
```
`root: true` is added so that we don't get a conflict between the ESLint conf for jest and this one.

>"That way, ESLint will stop here as it's looking at the file system for the configuration to use for a particular file." - Kent C. Dodds

We also add the `cypress/videos` & `cypress/screenshots` to the `.gitignore` so that videos & screenshots from Cypress are not added to source control.

## Write the First Cypress Test
We follow the instructions from cypress and adds `localhost:8080` to `cy.visit()`. Then we use the Cypress Selector to get our selectors. They look a bit funny due to fact that we have CSS Modules generating hashed classnames. We'll change the selectors later so that it is easier to follow.

Cypress uses the "Chai assertion library". In the following line the `should('have.text', '3')` is from the [chai BDD assertions](https://docs.cypress.io/guides/references/assertions#BDD-Assertions).
```js
cy.findByTestId('total').should('have.text', '3');
```

>"... I like to call `cy` -> `user` and then we say, "User, I want you to visit that thing and then user, I want to get that and then user, I want to click on that." The way that I see these commands is more of a list of instructions that I'm going to give to a user, rather than to a to the Cypress robot." - Kent C. Dodds

```js
// Example of above quote
   const user = cy
   user.visit('http://localhost:8080')
   user.get('something')
// etc.
```

## Configure Cypress in cypress.json

We use the `cypress.json`-file to set `baseUrl` and move some test files (to an e2e folder) to make it more clear what we want to test.

### Example `cypress.json`
```json
{
  "baseUrl": "http://localhost:8080",
  "integrationFolder": "cypress/e2e",
  "viewportHeight": 900,
  "viewportWidth": 400
}
```

When you have Cypress running, you can go to `Settings -> Configuration` to find all the settings that you can change inside your `cypress.json`.

## Installing Cypress Testing Library

To get better commands for finding elements, we install `@testing-library/cypress` 🥳
```bash
npm install --save-dev @testing-library/cypress
```

Inside `support/index.js` we'll also need to import the commands:
```js
import '@testing-library/cypress/add-commands'
```

After that we'll have access to for example, `findByText`. However, we don't get access to `getByText` which we might know of from `@testing-library/react` and that is because we shouldn't write synchronous queries in Cypress. 

Using these commands, makes the tests way more readable! 📖

## Script Cypress for Local Development and Continuous Integration

>"It'd be really great if I could just run one script and it would start up my server and start up Cypress automatically." - Kent C. Dodds

Ok, so what do we do accomplish what Kent mentioned?

1. Install `start-server-and-test`
	```bash
	npm install --save-dev start-server-and-test
	```
2. Update our scripts in `package.json`
```json
"cy:run": "cypress run",
"cy:open": "cypress open",
"test:e2e": "is-ci \"test:e2e:run\"  \"test:e2e:dev\"",
"pretest:e2e:run": "npm run build",
"test:e2e:run": "start-server-and-test start http://localhost:8080 cy:run",
"test:e2e:dev": "start-server-and-test dev http://localhost:8080 cy:open",
```
The `cypress run` command runs Cypress in headless mode (which we want to run in CI). We also want to run the freshest build when doing this and therefore we have the `pretest:e2e:run` script which builds our application.

We also update the `validate` script to run e2e-tests:
```json
"validate": "npm run test:coverage && npm run test:e2e:run"
```

Kent also adds `npm run test:e2e:run` to the pre-commit hook of husky, but that could be a bit cumbersome to have when you are working on a big project. (I would probably never have it on pre-commit. I'd rather rely on it being run in CI)

Kent also does some CI configuration for Travis CI (check the lesson again if interested).

## Debug a Test with Cypress
A great thing that you can do in Cypress when debugging is to add the follow to one of your queries:
```js
cy.findByText('something')
	.then((subject) => {
		debugger;
		return subject;
	});
```
If you have Chrome DevTools opened in the tab where you are running your Cypress tests, the Chrome debugger will stop at the `debugger` statement.

💡 You can also just add `.debug()` to get some debug info in the console and a `debugger` statement automatically.

Another thing you can do is adding `.pause()` to pause your test and let you inspect & manipulate the application in that current state. Then you can just hit the `Resume` button to continue the test.

When writing your tests you can also play around with using `window.Cypress` to set some stuff on the window-object. 

🚨 However it's **not recommended to use in your tests**🚨

> "You could do something like this with your redux store, your context value, or whatever it is that helps you develop these tests. I would recommend against using these in your test because this isn't something that the user typically will have access to." - Kent C. Dodds

### `window.Cypress` example
```js
if (window.Cypress) {
   debugger
   window.theme = theme
   window.setTheme = setTheme
 }
```
## Test User Registration with Cypress

In this test, we test the functionality for registering a new user. We can't register the same username and password each time we run the test. The best would be to be able to clear what we added to the database, but Kent shows another way of doing it, if you can't do that.

> "Now, clearing out your database between tests is a good thing to do, but sometimes you can't do that." - Kent C. Dodds

He creates a `buildUser`-function that generates a fake user with the help of the package `test-data-bot`.

In the test he verifies that registration was successful by checking if we get redirected to the `baseUrl` and by checking if `localStorage.token` is a `string`.

```js
  .url()
  .should('eq', `${Cypress.config().baseUrl}/`)
  .window()
  .its('localStorage.token')
  .should('be.a', 'string')
```

> "One thing that I want to add here is that if your registration process sends out an email and then user can't do anything until they've confirmed their email address, it's a good idea to just mark that service out entirely for your test. Writing an automated test for something like that would be very non-trivial and would only give you a little bit extra confidence.
> 
> I'd recommend marking that out so that your end-to-end test can continue on without having to worry about that email." - Kent C. Dodds

## Cypress Driven Development

Kent talks about the idea of using Cypress kind of as a Test Driven Development (TDD) tool, hence the title "Cypress Driven Development".

>"If you've ever developed an experience where a user has to enter in seven pages of forms and then you get to the success page, and that's what you're developing is that success page, and it's a total nightmare to develop that because you have to type in all those form inputs, now you can just use Cypress to have that type in all type in those form inputs automatically for you. You can get to that last page quickly, and use Cypress as your tool for development." - Kent C. Dodds

## Simulate HTTP Errors in Cypress Tests
We can use Cypress to simulate errors and test how the application responds.

Example of how to use Cypress interceptor:
```js
cy.server()
  cy.route({
    method: 'POST',
    url: 'http://localhost:3000/register',
    status: 500,
    response: {},
  })
```

## Create a User with `cy.request` from Cypress
>"Wouldn't it be nice if we could just say, "Hey, Cypress, do this thing that our app is doing so that we can have a registered user"? That's exactly what we're going to do." - Kent C. Dodds

Instead of having to click through the registration form again in the "login existing user"-test, we'll use `cy.request` to make the same `POST`-request that the application does when hitting "Register" so that we get an existing user that we can try to log in with:
```js
// create user
   const user = buildUser()
   cy.request({
     url: 'http://localhost:3000/register',
     method: 'POST',
     body: user,
   })
 // then we run through the login process
```

## Keep Tests Isolated and Focused with Custom Cypress Commands

To get some more isolation we can create Cypress Commands. For example, instead of doing the `cy.request` etc. when creating a user in a test, we can write a command that does this for us.

```js
import {buildUser} from '../support/generate'

Cypress.Commands.add('createUser', overrides => {
  const user = buildUser(overrides)
  cy.request({
    url: 'http://localhost:3000/register',
    method: 'POST',
    body: user,
  }).then(response => ({...response.body.user, ...user}))
})
```

This command also makes the subject `user` available to us in the test so we'll just run our test code inside the `.then()`:
```js
it('should login an existing user', () => {
   cy.createUser().then(user => {
     // do all the test stuff here. 
     // We have access to user.username etc.
   })
 })
```

## Use Custom Cypress Command for Reusable Assertions
We can also use custom Cypress Commands to create custom assertions. This is great if we need to re-use the same assertion and can make our tests leaner and more descriptive. One good example that Kent shows is this one for asserting that we are logged in as as specific user:
```js
Cypress.Commands.add('assertLoggedInAs', user => {
  cy.window()
    .its('localStorage.token')
    .should('be.a', 'string')
    .findByTestId('username-display')
    .should('have.text', user.username)
})
```

Then we can just add `cy.assertLoggedInAs(user)` to our test.

## Use cy.request from Cypress to Authenticate as a New User
In this lesson we get to see how we can use `cy.request` to replace some unnecessary steps for logging in when testing that a user's display name is shown when logged in and is not shown when logged out.

You can add stuff to `localStorage`  after the

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyNTU2NzY3NTQsLTExMzY2MzYwMzIsLT
YyMTU3MDQ0OCwtMjA0MjYxMTczMSwxMzc0NDc1MjA3LDU3OTAy
Mjk1NywxMDg1OTg2MjY0LC0xMDE1NDUxOTU1LC0xMzI0NDY2MD
U3LDE0NTU4OTQwNDMsMTkwODg4NjM3MSw2MzQ4NzU4MDZdfQ==

-->