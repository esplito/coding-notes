## Static Analysis Testing Javascript Applications

  

Repo:

[https://github.com/kentcdodds/static-testing-tools/tree/tjs/step-14](https://github.com/kentcdodds/static-testing-tools/tree/tjs/step-14)

  

### Eslint

  

“valid-typeof”: error -> CI will fail if there is faulty typeof in the code.

  

“valid-typeof”: warn -> CI will not fail but you’ll get a warning

  

“browser”: true eliminates some errors. For example if you get “console” is not

defined

  

Use `.eslintignore` to not run eslint on “dist” and “node_modules” or add the

following in your “scripts” in package.json:

  

`"lint": "eslint --ignore-path .gitignore .`

  

This will tell eslint to ignore the paths specified in the gitignore.

  

### Prettier

  

Run `npx prettier <file here>` to see the prettier version of the code in your

terminal. Add `--write` if you want to overwrite the file.

  

To enable prettier, go to your settings.json in VS code and use the following:

  

```json

{

"editor.defaultFormatter": "esbenp.pretter-vscode",

"editor.formatOnSave": true

}

```

  

To validate your files you can add “check-format” and a “validate” script.

  

```json

"scripts": {

"build": "babel src --out-dir dist",

"lint": "eslint --ignore-path .gitignore --ext .js,.ts,.tsx .",

"check-types": "tsc",

"prettier": "prettier --ignore-path .gitignore \"**/*.+(js|json)\"",

"format": "npm run prettier -- --write",

"check-format": "npm run prettier -- --list-different",

"validate": "npm-run-all --parallel check-types check-format lint build"

}

```

  

### Typescript

  

To make babel capable of parsing typescript, install its typescript-preset:

  

```bash

npm install --save-dev @babel/preset-typescript

```

  

and add the following to your presets in the `.babelrc`:

  

`"@babel/preset-typescript"`

  

Our new build script that also checks typescript files:

  

`"build": "babel src --extensions .js,.ts,.tsx --out-dir dist",`

  

### Husky

  

> “We have a few checks we’ll run in continuous integration when someone opens a

> pull request, but it’d be even better if we could run some of those checks

> before they even commit their code so they can fix it right away rather than

> waiting for CI to run. Let’s use husky’s precommit script to run our

> validation. “ - Kent C. Doods

  

.huskyrc example

  

```json

{

"hooks": {

"pre-commit": "npm run validate"

}

}

```

  

> “What this is going to do is that hooks directory is built into Git Anytime

> you do a commit, Git is going to run that pre-commit script that's in that

> hooks directory. That pre-commit script that Husky created for us is actually

> going to look up this configuration and run this script that we have here.”

> Kent C. Dodds

  

Another example of husky where we check types and use lint-staged:

  

```json

{

"hooks": {

"pre-commit": "npm run check-types && lint-staged"

}

}

```

  

Our `.lintstagedrc`:

  

```json

{

"*.js": ["eslint"],

"*.+(js|json|ts)": ["prettier --write", "git add"]

}

```

  

We can run this on pre-commit also:

  

```json

{
"hooks": {
"pre-commit": "npm run check-types && lint-staged && npm run build"
}
}

```

### npm-run-all

> “I use npm scripts a lot, and that validate script is a great way to bring everything together. But with the way these tools work, we can run them all at the same time and things will work just as well. This will speed up our script runtime a lot, so let’s use [npm-run-all](https://npm.im/npm-run-all) to make that happen.” Kent C. Dodds

🎉 Adding the following in our “validate” script will make the scripts run in parallel! 🎉

`"validate": "npm-run-all --parallel check-types check-format lint build"`

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTU3NzcxMzMzMl19
-->