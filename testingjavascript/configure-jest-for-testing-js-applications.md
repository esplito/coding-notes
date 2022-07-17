
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

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzMjU5ODI5MzgsNjI2MjIxLDE0MTczOT
k1OTQsLTY1NzM5Mzg1NSwxMDA5NjQ1Mjg3LDYxNzYxMDE3LDIw
MDk2NTM0ODRdfQ==
-->