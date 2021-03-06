
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

Now the ESLint error, will be gone. We still get warnings if make a typo in the import though. If we want to be able to hit F12 and get directly to the `test-utils.js`-file we can add `test/*` to our `jsconfig.json` (or `tsconfig.json` if we use TS):
```json
{
  "compilerOptions": {
    "baseUrl": "./",
    "paths": {
      "*": ["src/*", "src/shared/*", "test/*"]
    }
  },
  "include": ["src", "test/*"]
}
```

One improvement that Kent mentions is to add the possible to pass in custom options in our `render`.  Something like:
```jsx
render(<MyComponent />, { theme: 'light'})
```

This can be done by changing our `render` that we have inside our `test-utils.js`-file:
```jsx
import React from 'react'
import PropTypes from 'prop-types'
import {render as rtlRender} from '@testing-library/react'
import {render} from '@testing-library/react'
import {ThemeProvider} from 'emotion-theming'
import {dark} from '../src/themes'

function render(ui, {theme = themes.dark, ...options}) {
  function Wrapper({children}) {
    return <ThemeProvider theme={theme}>{children}</ThemeProvider>
}
Wrapper.propTypes = {
  children: PropTypes.node,
}
  return rtlRender(ui, {wrapper: Wrapper, ...options}}
}

export * from '@testing-librasry/react'
export {render}
```
I have previously used this approach to give developers the possibility to set some initial values of the redux store in their tests. 

## Use Jest Watch Mode to Speed Up Development

Run `jest --watch` or create your own script so you can run `npm run test:watch`
```json
scripts: {
	"test:watch": "jest --watch"
}
```
This is amazing! 🥳 I use it all the time. 🤩
Jest watch mode can be used in various different modes:

- **a** = run _all_ tests 
- **o** = only run _tests related to changed files_
- **f** = run only _failed_ tests
- **p** = filter by _filename_ regex pattern
- **t** = filter by _testname_ regex pattern
- **u** = update snapshots
- **i** = update snapshots interactively = it runs each test 1-by-1 and let you decide if you want to update the snapshot or skip it.
- **q** = quit
- **Enter** = trigger a new test run

You can combine **p** and **t** to filter which tests to run.

## Step through Code in Jest using the Node.js Debugger and Chrome DevTools

> "What I really want to be able to do is just add a debugger statement in here and have my browser dev tools stop right there. I can do that in the browser, but I can't do it when I'm running my test. Or can I?" - Kent C. Dodds

Answer: Yes! ✅

We can add a new script to our `package.json`:
```json
	"test:debug": "node --inspect-brk",	
```
`--inspect-brk` means "inspect break" and this in turn mean that Node will spin up a process and before it runs any code, it will add a breakpoint. You can then hook up that Node process to the Chrome Debugger!

However, this doesn't quite cut it yet! We need to tell it where the jest binary is:
```json
"test:debug": "node --inspect-brk ./node_modules/jest/bin/jest.js --runInBand --watch"
```
We add `--runInBand` because we need the tests to run inside the same Node process. We also add `--watch` because it's nice to have.

> If you open up Chrome and go to **chrome://inspect**, that will take you to this special view where I'll show you the different debugger sessions available.

If we have added a `debugger` statement in our code, the Chrome Devtools will now wait at the `debugger`-statement and we can step through the code.

## Configure Jest to Report Code Coverage on Project Files

By adding `--coverage` to the `jest`-command (when running `npm test` we get a coverage report for our project. It create a coverage directory with all the information.

We can open up the coverage report by writing the following command in the terminal:
```bash
open coverage/lcov-report/index.html
```
A problem can be that it includes the test utilities in the coverage report and this skews the overall coverage numbers, since the test utils have 100% coverage. We are also not seeing all files from `src` included, for example `index.js` or `app.js`. 

This can be solved by adding the following line in `jest.config.js`:
```js
collectCoverageFrom: ['**/src/**/*.js'],
```
It will now also automatically exclude the folder with the test utils. 

We also don't want to include the coverage map in source control so we need to add `coverage` to our `.gitignore`.

## Analyze Jest Code Coverage Reports
- How does Jest know that some part of my code is not being tested? Because it runs a tool called `babel-plugin-istanbul`

> "It's not actually changing the behavior of your code, it's just adding some instrumentation so that it can keep track of the side of the default value expression run." - Kent C. Dodds

If we want it to ignore some part of the code, we can add:
`/* istanbul ignore next */`

> I typically recommend against using these kinds of comments because you're basically lying to yourself about how much coverage you're getting, but sometimes it can be useful. - Kent C. Dodds

ℹ️ Kent explains it very well in his video how the plugin checks the code. If you ever need to analyze why the code coverage looks a certain way -> Revisit [this lesson](https://testingjavascript.com/lessons/jest-analyze-jest-code-coverage-reports)!

## Set a Code Coverage Threshold in Jest to Maintain Code Coverage Levels
### The four elements of Code Coverage
1. Statements
2. Branches
3. Functions
4. Lines

You can set a threshold by adding the following to your `jest.config.js`:
```js
coverageThreshold: {
    global: {
      statements: 34,
      branches: 24,
      functions: 29,
      lines: 29,
    },
  },
```
> "I typically like to do a couple percentage points lower than what we have currently so that as changers are made and things, we have a little bit of flexibility here." - Kent C. Dodds

He set the thresholds to around  2-3 percentage units lower than current coverage.

🚨 **Important to note**🚨
> "Now, one really important thing about coverage to remember is that **it's not a perfect metric for confidence.** 
> 
>The problem is that not all lines in your code base are equal. For example, maybe this utilities file is super, super important because it's used just all over the place. The autoscaling text has this line here that's really not that important, it doesn't happen all that often. Even if that were to happen, it's not a huge deal that that gets broken as an example." - Kent C. Dodds 

💡 What you can do instead is to add specific thresholds for import files such as utils-files:
```js
coverageThreshold: {
	global: { ... },
	'./src/shared/utils.js': {
      statements: 100,
      branches: 80,
      functions: 100,
      lines: 100,
    },
}
```
This will take the utils-file out of the global coverage though (so global coverage becomes lower), so you'll have to adapt the global threshold.

## Report Jest Test Coverage to Codecov through TravisCI
> "The coverage report generated by Jest is fantastic, but it’d be great to track that coverage over time and be able to review that coverage at a glance, maybe even put it up on a display in the office! Codecov.io is a fantastic service that can consume our code coverage report and it integrates great with GitHub." - Kent C. Dodds

By adding some configuration to the travis conf we can make sure that it uploads the code coverage files to codecov. (Example is only with Travis CI)

```yaml
after_script: npx codecov@3
```

## Run Jest Watch Mode by Default Locally with is-ci-cli

It's a bit tedious to be force to write `npm run test:watch` all the time when running tests locally. It would be nice to just be able to write `npm t`. We can do this by installing a new devDependency! 🥳
```bash
npm install --save-dev is-ci-cli
```

For this to work, we also need to modify our scripts in `package.json`:
```js
	"test": "is-ci \"test:coverage\" \"test:watch\"",
    "test:coverage": "jest --coverage",
```
We now have a separate script for running with the `--coverage` flag and we also let `is-ci-cli` determine whether we should run `test:coverage` or `test:watch`.

If the environment variable `CI` is set to `1`, it will run `test:coverage`, otherwise `test:watch`.

If you want to force CI test locally you can run `CI=1 npm t`.

## Run Tests with a Different Configuration using Jest’s --config Flag and testMatch Option

Sometimes you might want to run a different configuration for some tests. This lesson covers that.

Kent sets up three different files inside the `test` directory:
1. `jest-common.js` (previously `jest.config.js`
2. `jest.client.js`
3. `jest.server.js`

Here's the content of the new files:
**1. jest-common.js**
```js
const path = require('path')

module.exports = {
  // Needed so that it finds the tests (otherwise it will look inside the same folder as this file is located)
  rootDir: path.join(__dirname, '..'), 
  moduleDirectories: [
    'node_modules',
    path.join(__dirname, '../src'),
    'shared',
    path.join(__dirname),
  ],
  moduleNameMapper: {
    '\\.module\\.css$': 'identity-obj-proxy',
    '\\.css$': require.resolve('./style-mock.js'),
  },
  collectCoverageFrom: ['**/src/**/*.js'],
}
```
**2. jest.client.js**
```js
module.exports = {
  ...require('./jest-common'),
  testEnvironment: 'jest-environment-jsdom',
  setupFilesAfterEnv: ['@testing-library/jest-dom/extend-expect'],
  snapshotSerializers: ['jest-emotion'],
  coverageThreshold: {
    global: {
      statements: 15,
      branches: 10,
      functions: 15,
      lines: 15,
    },
    './src/shared/utils.js': {
      statements: 100,
      branches: 80,
      functions: 100,
      lines: 100,
    },
  },
}
```
**3. jest.server.js**
```js
const path = require('path')

module.exports = {
  ...require('./jest-common'),
  coverageDirectory: path.join(__dirname, '../coverage/server'),
  testEnvironment: 'jest-environment-node',
  testMatch: ['**/__server_tests__/**/*.js'],
}
```

We also need to update our `eslintrc` to point to our `jest-common.js`-file instead of `jest.config.js`:
```js
jestConfigFile: path.join(__dirname, './test/jest-common.js'),
``` 

After all of these changes, we need to update our scripts in `package.json` so that we can run both client- and server-side tests:
```json
	"test": "is-ci \"test:coverage\" \"test:watch:client\"",
    "test:coverage": "npm run test:coverage:client && npm run test:coverage:server",
    "test:coverage:client": "jest --config test/jest.client.js --coverage",
    "test:coverage:server": "jest --config test/jest.server.js --coverage",
    "test:watch:client": "jest --config test/jest.client.js --watch",
    "test:watch:server": "jest --config test/jest.server.js --watch",
    "test:debug:client": "node --inspect-brk ./node_modules/jest/bin/jest.js --config test/jest.client.js --runInBand --watch",
    "test:debug:server": "node --inspect-brk ./node_modules/jest/bin/jest.js --config test/jest.server.js --runInBand --watch",
```
We can only run one watch mode at a time, so we run the client-side tests by default.

## Support Running Multiple Configurations with Jest’s Projects Feature
Okay, so what we did in the previous lesson is a bit much and Kent provides a solution for making it look as clean as it did before, without having to double all of our test scripts. The solution is to use the `projects` feature of Jest.

So to do this we: 
1. Create our `jest.config.js` again and update the eslintrc:
	```js
	jestConfigFile: path.join(__dirname, './jest.config.js'),
	``` 
2. Move some configuration from `jest.client.js` to `jest.config.js`:
```js
module.exports = {
	...require('./test/jest-common'),
	collectCoverageFrom: ['**/src/**/*.js'],
	coverageThreshold: {
	  global: {
	    statements: 15,
	    branches: 10,
	    functions: 15,
	    lines: 15,
	  },
	  './src/shared/utils.js': {
	    statements: 100,
	    branches: 80,
	    functions: 100,
	    lines: 100,
	  },
	 },
	 projects: ['./test/jest.clients.js', './test/jest.server.js'],
}
```

`npm t` will now run both client- and server-environment tests when running watch mode. 🎉

We can inspect our global jest configuration by running:
```bash
npx jest --showConfig
```
ℹ️ Look out for the `globalConfig`-object. 
> "Anything inside of the global config is what needs to be configured inside of the jest.config.js configuration which is your main entry for your configuration." - Kent C. Dodds

Currently it is a bit difficult, in watch mode, to see from which configuration the tests are run. We can solve this by adding labels (`displayName`) to them:

```js
// jest.server.js
module.exports = {
  ...require('./jest-common'),
  displayName: 'server',
  testEnvironment: 'jest-environment-node',
  testMatch: ['**/__server_tests__/**/*.js'],
}

// jest.client.js
module.exports = {
  ...require('./jest-common'),
  displayName: 'client',
  testEnvironment: 'jest-environment-jsdom',
  setupFilesAfterEnv: ['@testing-library/jest-dom/extend-expect'],
  snapshotSerializers: ['jest-emotion'],
}
```

## Run ESLint with Jest using jest-runner-eslint
The great thing about the jest-runner is that we can set up custom runners, for example to run ESLint in watch mode! 🤯

### Steps for setting it up
1. Install `jest-runner-eslint`
	```bash
	npm install --save-dev jest-runner-eslint
	```
2. Create new file in the test directory: `jest.lint.js`:
	```js
	const path = require('path')

	module.exports = {
	  rootDir: path.join(__dirname, '..'),
	  displayName: 'lint',
	  runner: 'jest-runner-eslint',
	  testMatch: ['<rootDir>/**/*.js'],
	}
	```
3. Add some options to `package.json`:
	```json
	"jest-runner-eslint": {
	  "clipOptions": {
	    "ignorePath": "./.gitignore
	  }
	}
	```
4. Add the new jest lint file to the projects inside `jest.config.js`:
	```js
	projects: [
	    './test/jest.lint.js',
	    './test/jest.client.js',
	    './test/jest.server.js',
	  ],
	```
5. Update the scripts in `package.json` and remove unnecessary use of the lint command in validation:
```json
"scripts": {
    "test": "is-ci \"test:coverage\" \"test:watch\"",
    "test:coverage": "jest --coverage",
    "test:watch": "jest --watch",
    "test:debug": "node --inspect-brk ./node_modules/jest/bin/jest.js --runInBand --watch",
    "dev": "webpack-dev-server --mode=development",
    "build": "webpack --mode=production",
    "postbuild": "cp ./public/index.html ./dist/index.html",
    "start": "serve --no-clipboard --single --listen 8080 dist",
    "format": "prettier --ignore-path .gitignore --write \"**/*.+(js|json|css|html|md)\"",
    // Here's the interesting changes 🤩
    "lint": "jest --config test/jest.lint.js",
    "validate": "npm run test && npm run build",
    "setup": "npm install && npm run validate"
 }
```
6. Voilá! 🙌 We can now run linting in watch mode and as a part of our test command in the CI 🥳

👑 **Great explanation to when this is useful** 👑
>"This might seem like just an extra dependency that's not totally necessary. Depending on your project, it might not be totally necessary. It **can be nice for bigger projects where you have tons of files that you're linting and you want to scope down the files that you're linting to just the ones that you're working with** at the time you're committing your code." - Kent C. Dodds

## Test Specific Projects in Jest Watch Mode with jest-watch-select-projects
> "It’s great that we can run multiple projects in our watch mode and that we can scope the tests down to specific tests, but sometimes it’s nice to be able to quickly switch between projects in watch mode." - Kent C. Dodds

We can do it with this command, but it's a bit tedious:
```bash
npx jest --config test/jest.client.js
```

Instead, we can install a new devDependency:
```bash
npm install --save-dev jest-watch-select-projects
```

And then we just modify our `jest-common.js` and add this line:
```js
watchPlugins: ['jest-watch-select-projects'],
```

🥳 Now we can hit `P` in watch mode and select/de-select the different projects, with `space` and then press `Enter`

## Filter which Tests are Run with Typeahead Support in Jest Watch Mode
Typeahead is a super nice plugin that makes it a lot easier to select the correct files or tests that you want to run. 

1. You only need to install it:
	```bash 
	npm install --save-dev jest-watch-typeahead
	```

2. and add it to the `watchPlugins` in `jest-common.js`:
	```js
	watchPlugins: [
	  'jest-watch-select-projects',
	  'jest-watch-typeahead/filename',
	  'jest-watch-typeahead/testname',
	]
	```
Voilá 🥳 Now you get suggestions when you start typing in "pattern matching mode" for both filename & testname. 

## Run Only Relevant Jest Tests on Git Commit to Avoid Breakages

Running the tests on commit is a way to avoid breaking the application and not having to wait for them to be run in the CI. However this can be very slow if we need to run all of them each time. It would nice to just run it for the files that have been affected!

We can do this with `husky` and `lint-staged`.

1. Install them:
	```bash
	npm install --save-dev husky lint-staged
	```
2. Add conf to package.json:
	```json
	"husky": {
	    "hooks": {
	      "pre-commit": "lint-staged && npm run build"
	    }
	  },
	  "lint-staged": {
	    "**/*.+(js|json|css|html|md)": [
	      "prettier",
	      "jest --findRelatedTests",
	      "git add"
	    ]
	  },
	```
The `--findRelatedTests` flag makes sure that we only run tests related to the file changes.

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTc5ODgxNTk0MiwtMTA3ODEwODg0LDE1Mj
UxNTg1MjQsLTg5NDg5NjQxLC01MTA1NzA4MDYsMTM1NDY2ODkx
NSwtMTE0MTI3MzcwNSwtODYxNjk1MDU0LC0xNzAwNTUxODk3LD
E5OTcwMDE2OTAsNDMwMDkyOTU0LDkyNzgwODQ4MiwxNjIxMzg5
NzMzLDIwNjE5NzM1MiwtMTM2ODc4Mzk1NCw3NDk2MjY3MzMsLT
cwMjYxODE0LDYyNjIyMSwxNDE3Mzk5NTk0LC02NTczOTM4NTVd
fQ==
-->