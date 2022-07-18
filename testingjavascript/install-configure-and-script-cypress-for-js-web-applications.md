
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

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTYyNzczOTg5LC0xMzI0NDY2MDU3LDE0NT
U4OTQwNDMsMTkwODg4NjM3MSw2MzQ4NzU4MDZdfQ==
-->