
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

When running Jest, we can either run it in `node` or `jsdom`.  There's a little performance hit with using `jsdom`, so if your test code doesn't need access to the `window`-object, you could either run your test with the following command:
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




> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyNTU2NDAxOTcsNjE3NjEwMTcsMjAwOT
Y1MzQ4NF19
-->