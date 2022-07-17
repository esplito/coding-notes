
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



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbNjI2MjIxLDE0MTczOTk1OTQsLTY1NzM5Mz
g1NSwxMDA5NjQ1Mjg3LDYxNzYxMDE3LDIwMDk2NTM0ODRdfQ==

-->