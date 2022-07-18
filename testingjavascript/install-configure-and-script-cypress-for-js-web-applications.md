
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


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbNTc5MDIyOTU3LDEwODU5ODYyNjQsLTEwMT
U0NTE5NTUsLTEzMjQ0NjYwNTcsMTQ1NTg5NDA0MywxOTA4ODg2
MzcxLDYzNDg3NTgwNl19
-->