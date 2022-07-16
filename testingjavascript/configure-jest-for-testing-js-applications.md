
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
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbNjE3NjEwMTcsMjAwOTY1MzQ4NF19
-->