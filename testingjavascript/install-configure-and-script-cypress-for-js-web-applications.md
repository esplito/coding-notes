
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

Cypress uses the "Chai assertion library". In the following line the `have.text`
```js
cy.findByTestId('total').should('have.text', '3')
```

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTgwNDUxMDMxOSwxNDU1ODk0MDQzLDE5MD
g4ODYzNzEsNjM0ODc1ODA2XX0=
-->