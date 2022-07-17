
# Configure Jest for Testing Javascript Applications

Repo: [https://github.com/kentcdodds/jest-cypress-react-babel-webpack/tree/tjs/jest-23](https://github.com/kentcdodds/jest-cypress-react-babel-webpack/tree/tjs/jest-23) (Different branches for each lesson, jest-00, jest-01 etc.)

## Install and run Jest

1. Install jest:
	 ```bash
	npm install --save dev jest
	```

Kent prefers to run both linting and tests in the CI, so he has a script called `setup` which include the following:
```json
"setup": "npm run setup && npm run validate"
```

`validate` runs the following:
```json
"validate": "npm run lint && npm run test && npm run build",
```

## Compile Modules with Babel in Jest Tests

If you get a `Unexpected token error` it might be because node doesn't know how to handle an `import`-statement. (See  `__tests__/utils.js`)

The reason why it doesn't know how to handle it is because we have set the `@babel/preset-env` to `modules: false` in our `.babelrc`. We have this set, because we want webpack to handle our modules so that we can reap the benefits of tree-shaking.

However, if we remove it, our jest tests can be run without the error (but we no longer have tree-shaking 😢). 

**The solution 🎉**
```js
const isTest = String(process.env.NODE_ENV) === 'test';
...
...
// inside module.exports
'@babel/preset-env', {modules: isTest ? 'commonjs' : false}
```
> "One thing that I want to call out here is the fact that jest picks up the .babelrc automatically." Kent C. Dodds

## Configure Jest’s Test Environment for Testing Node or Browser Code

When running Jest, we can either run it in `node` or `jsdom` (installed by default when installing jest).  There's a little performance hit with using `jsdom`, so if your test code doesn't need access to the `window`-object, you could either run your test with the following command:
```bash
npm t -- --env=node
```

or by setting up a `jest.config.js` that looks like this:
```js
module.exports = {
  testEnvironment: 'jest-environment-node' 
  // jest-environment-jsdom if you want access to browser apis
}
```

## Support Importing CSS files with Jest’s moduleNameMapper

You might run into problems when trying to test a react component that imports a CSS module file (`something.module.css`).

Then you most likely need to add some configuration for `moduleNameMapper` in your jest config.

1. Create a file called `style-mock.js` that only contains `module.exports = {}`
2. Change your `jest.config.js` to the following:
```js
module.exports = {
  testEnvironment: 'jest-environment-jsdom',
  moduleNameMapper: {
    '\\.css$': require.resolve('./test/style-mock.js'),
  },
}
```
> "...the reason that this works in our application is because this is being bundled with webpack. We have webpack configured to handle CSS files with the CSS-loader and the style-loader. Webpack is managing this for our application and we simply needed to make Jest manage the same thing for our test." Kent C. Dodds

## Support using Webpack CSS Modules with Jest

If you want the `className` to be populated during tests, you can install `identity-obj-proxy`:
```bash
npm install --save dev identity-obj-proxy
```
We'll then get the name of the CSS Module className by adding the following to `moduleNameMapper` inside the `jest.config.js`:
```js
'\\.module\\.css$': 'identity-obj-proxy',
```

## Generate a Serializable Value with Jest Snapshots

Instead of having to manually update the `.toEqual` in the following test:
```js
import {getFlyingSuperHeros} from '../super-heros'

test('returns returns super heros that can fly', () => {
  const flyingHeros = getFlyingSuperHeros()
  console.log(flyingHeroes)
  expect(flyingHeros).toEqual([
    {name: 'Dynaguy', powers: ['disintegration ray', 'fly']},
    {name: 'Apogee', powers: ['gravity control', 'fly']},
    {name: 'Jack-Jack', powers: ['shapeshift', 'fly']},
  ])
})
```
we could use `toMatchSnapshot`.
```js
import {getFlyingSuperHeros} from '../super-heros'

test('returns returns super heros that can fly', () => {
  const flyingHeros = getFlyingSuperHeros()
  console.log(flyingHeroes)
  expect(flyingHeros).toMatchSnapshot() // <-- This is the difference
})
```
The output of `.toMatchSnapshot` ends up in a `.snap`-file.

> "What Jest is doing is it takes the object that we pass to the assertion, and it serializes it into a string and saves that string into this file." - Kent C. Dodds
>
> "Part of the serialization process is giving a label to these. It's not like a JSON stringify. It's giving a label to each one of these objects so that it's more clear what these things are. We have basically the same output that we had when we copy/pasted, except _we didn't have to do this manually_." - Kent C. Dodds

If we make a code change that affects the output of `getFlyingSuperHeros()`, the jest runner will then tell us that the snapshot failed:
```bash
1 snapshot failed from 1 test suite. 
Inspect your code changes or run npm test -- -u to update them
```

If this shouldn't have happened, we'll have to check our code and make sure to fix the bug, but if it was correct, we can just run the specified command to update the snapshot.

>"A snapshot is an assertion that lives in two places, the assertion living in the `test()`, and then the actual snapshot value living in the .snap file. You're going to want to commit this file to source control. 
>
> One of the problems with snapshots though is that they can get very long and, having them live in a separate file, makes it harder to review." - Kent C. Dodds

Kent instead recommends the use of `toMatchInlineSnapshot` which will automatically add the snapshot string inside the test instead.

👇⚠️ Don't forget to have Prettier installed in your project though! ⚠️ 👇
> "I should add here that when you're using `toMatchInlineSnapshot()`, you are required to have Prettier installed in your project because jest is updating the code in your test file and it wants to make sure that it doesn't change more than it has to with regard to your formatting." - Kent C. Dodds

He mentions this when using `toMatchInlineSnapshot` with DOM nodes. Example: `expect(container).toMatchInlineSnapshot()`.

## Test an Emotion Styled UI with Custom Jest Snapshot Serializers

**UPDATE: `jest-emotion` has been renamed to `@emotion/jest` and you add the serializer with `'@emotion/jest/serializer'` instead of simply `'jest-emotion'`.**

By adding the emotion jest serializer  `snapShotSerializer` to the `jest.config.js`  we'll get more readable classnames (example: `emotion-0`) generated for the css in the tests. You also get the CSS in the snapshot with the changes you made to the CSS. 💅

> ℹ️ The serializer makes it easier to see the changes made to the CSS!

## Support Custom Module Resolution with Jest moduleDirectories

> "Webpack’s resolve.modules configuration is a great way to make common application utilities easily accessible throughout your application. We can emulate this same behavior in Jest using the moduleDirectories configuration option." - Kent C. Dodds

`resolve.modules` makes it possible to import shared modules as if they were a module inside `node_modules`. However this import is not supported by default in node. We'll need to add the following to the `jest.config.js`:
```js
moduleDirectories: ['node_modules', path.join(__dirname, 'src'), 'shared'],
```

## Configure Jest to Run Setup for All Tests with Jest setupFilesAfterEnv

When using the `.toBe`-assertion we don't get so great explanations to why something is making a test fail. Instead we can install som more assertion by running:
```bash
npm install --save-dev @testing-library/jest-dom
```

Then if we just add the following to our `jest.config.js`, we'll have the new assertions available in all the test files:
```js
setupFilesAfterEnv: ['@testing-library/jest-dom/extend-expect'],
``` 

You can find the list of custom matchers here: https://github.com/testing-library/jest-dom#custom-matchers

One that I often use is `.toHaveTextContent`, which means that:
1. I don't have to write: 
	```js
	expect(myRandomDOMNode.textContent).toBe("something")
	```
	and get an error message that says something like "Expected _thisText_ but received _someOtherText_".
	
2. I can instead write: 
	```js
	expect(myRandomDOMNode).toHaveTextContent("something")
	```
	and get an error message that says "Expected element to have text content _thisText_ but received _someOtherText_".

## Support a Test Utilities File with Jest moduleDirectories

It's not the first time that I see this utils-file for wrapping React Testing Library's render function, but I might as well paste in here anyway:

```jsx
// test-utils.js
import React from 'react'
import PropTypes from 'prop-types'
import {render as rtlRender} from '@testing-library/react'
import {render} from '@testing-library/react'
import {ThemeProvider} from 'emotion-theming'
import {dark} from '../src/themes'

function render(ui,options) {
  return rtlRender(ui, {wrapper: Wrapper, ...options}}
}

function Wrapper({children}) {
  return <ThemeProvider theme={dark}>{children}</ThemeProvider>
}

Wrapper.propTypes = {
  children: PropTypes.node,
}

export * from '@testing-librasry/react'
export {render}
```

With this we can use `render` in our tests just as before, but with the benefit of it wrapping our rendered component with a ThemeProvider (from `@emotion`). I normally use this for wrapping with providers from `redux`, `react-query` and `react-router`. 

If we want to be able to import the `test-utils.js` without having loads of `../../..`, we can use `moduleDirectories` inside `jest-config.js` and add:
```js
// this is the folder where test-utils.js lives
path.join(__dirname, 'test'),
```

This might however give you an ESLint error and then you need to the following 2 steps:
1. Install `eslint-import-resolver-jest`
	```bash
	npm install --save-dev eslint-import-resolver-jest
	```
2. In our  `.eslintrc.js` we need to add an override similar to what we have for webpack:
```js
overrides: [
    {
      files: ['**/src/**'],
      settings: {'import/resolver': 'webpack'},
    },
    {
      files: ['**/__test__/**'],
      settings: {
        'import/resolver': {
          jest: {
            jestConfigFile: path.join(__dirname, './jest.config)
        },
    }},
  ],
```


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQ0NDI0MjgyLDIwNjE5NzM1MiwtMTM2OD
c4Mzk1NCw3NDk2MjY3MzMsLTcwMjYxODE0LDYyNjIyMSwxNDE3
Mzk5NTk0LC02NTczOTM4NTUsMTAwOTY0NTI4Nyw2MTc2MTAxNy
wyMDA5NjUzNDg0XX0=
-->