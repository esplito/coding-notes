
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

The reason why it doesn't know how to handle it is because we have set the `@babel/preset-env` to `modules: false`. We have this set, because we want webpack to handle our modules so that we can reap the benefits of tree-shaking.

However, if we remove it, our jest tests can be run with the error.

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTU3MzQwNzY2LDYxNzYxMDE3LDIwMDk2NT
M0ODRdfQ==
-->