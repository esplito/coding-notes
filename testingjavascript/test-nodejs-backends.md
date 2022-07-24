# Test Node.js Backends

## Intro to Test Node.js Backends
Github repo: https://github.com/kentcdodds/testing-node-apps

This course module has a similar format to EpicReact. This means that I get some exercises to do first and then Kent walks through the solution.

### Course module topics
-   Testing Pure Functions
-   Testing Middleware
-   Testing Controllers
-   Testing API routes
-   Mocking third party dependencies
-   Testing authenticated code

## Test Pure Functions Overview

Background info and exercise: https://github.com/kentcdodds/testing-node-apps/blob/tjs/src/utils/__tests__/auth.md

## Exercise 1: Write Unit Tests for a Simple Pure Function

### First  try without `jest-in-case`

```js
import {isPasswordAllowed} from  '../auth'

test('should allow valid password', () => {
   const validPassword = '!aBc123'
   expect(isPasswordAllowed(validPassword)).toBeTruthy()
})

test('should not allow invalid passwords', () => {
    const tooShortPw = 'a2c!'
    const noAlphabetCharsPw = '123456!'
    const noNumbersPw = 'ABCdef!'
    const noUpperCasePw = 'abc123!'
    const noLowerCasePw = 'ABC123!'
    const noNonAlphaNumericsPw = 'ABCdef123'

    expect(isPasswordAllowed(tooShortPw)).toBeFalsy()
    expect(isPasswordAllowed(noAlphabetCharsPw)).toBeFalsy()
    expect(isPasswordAllowed(noNumbersPw)).toBeFalsy()
    expect(isPasswordAllowed(noUpperCasePw)).toBeFalsy()
    expect(isPasswordAllowed(noLowerCasePw)).toBeFalsy()
    expect(isPasswordAllowed(noNonAlphaNumericsPw)).toBeFalsy()
})
```

### Extra-credit (💯  reduce duplication) without `jest-in-case`
```js
// My second try with less duplication
describe('valid passwords', () => {
    const validPasswords = ['!aBc123']
    validPasswords.forEach((password) => {
        test(`${password} is valid`, () => {
            expect(isPasswordAllowed(password)).toBe(true)
        })
    })
})

describe('invalid password', () => {
    const invalidPasswords = [
        'a2c!',
        '123456!',
        'ABCdef!',
        'abc123!',
        'ABC123!',
        'ABCdef123',
    ]

    invalidPasswords.forEach((password) => {
        test(`${password} is invalid`, () => {
            expect(isPasswordAllowed(password)).toBe(false)
        })
    })
})
```

### Extra credit (💯  jest-in-case)

```js
// Third try with jest-in-case instead
const validPasswords = {
    '!aBc123': {
        result: true
    }
}
cases(
    'allowed passwords',
    (options) => {
        const result = isPasswordAllowed(options.name)
        expect(result).toBe(options.result)
    },
    validPasswords,
)

const invalidPasswords = {
    'a2c!': {
        result: false
    },
    '123456!': {
        result: false
    },
    'ABCdef!': {
        result: false
    },
    'abc123!': {
        result: false
    },
    'ABC123!': {
        result: false
    },
    'ABCdef123': {
        result: false
    },
}
cases(
    'disallowed passwords',
    (options) => {
        const result = isPasswordAllowed(options.name)
        expect(result).toBe(options.result)
    },
    invalidPasswords,
)
```

```js
// Re-visited version for disallowed passwords
const invalidPasswords = {
  'a2c!': {},
  '123456!': {},
  'ABCdef!': {},
  'abc123!': {},
  'ABC123!': {},
  'ABCdef123': {},
}
cases(
  'disallowed passwords',
  (options) => {
    const result = isPasswordAllowed(options.name)
    expect(result).toBe(false)
  },
  invalidPasswords,
)
```

### Extra credit (💯  improved titles for jest-in-case)
```js
// Fourth try with failure reason
function reasonify(obj) {
  return Object.entries(obj).map(([pwRule, name]) => ({
    name: `${pwRule} - ${name}`,
    password: name,
  }))
}

cases(
  'disallowed passwords',
  ({password}) => {
    const result = isPasswordAllowed(password)
    expect(result).toBe(false)
  },
  reasonify({
    'too short': 'a2c!',
    'no letters': '123456!',
    'no numbers': 'ABCdef!',
    'no uppercase letters': 'abc123!',
    'no lowercase letters': 'ABC123!',
    'no non-alphanumeric characters': 'ABCdef123',
  }),
)
```

💡 **Idea:** Create a typed function that makes it more obvious that it gives a "reason"/"rule" to the case, that is displayed when running the test: `FAIL: <value> - <reason_or_rule>`

## Test Node Middleware Overview

Node middleware in [this course module](https://github.com/kentcdodds/testing-node-apps/blob/tjs/src/utils/__tests__/error-middleware.md) refers to different [Express middlewares](https://expressjs.com/en/guide/using-middleware.html).

The following types of middleware are mentioned: 
>-   Application-level middleware (our app isn't really using this kind)
>-   Router-level middleware (all our routes use this strategy of middleware)
>-   Error-handling middleware (this is what  `error-middleware.js`  is)
>-   Built-in middleware (we're not using any of these)
>-   Third-party middleware (we're using a few of these, like  `cors`,  `body-parser`,  `express-jwt`, and  `passport`).

### Middleware & `req`, `res`, `next`
- Each kind of middleware accepts accept these as arguments. 
- Expected to either call the `response`-method to send a response to the caller _or_ call the 


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjExNzIyNzAxOSwtMjQ3MDk1MTYzLC01Mj
Y2NjA3NzIsLTE1NDQ5NDAwODMsLTU2OTc5OTA5NCwyMDIwNDIz
OTU0LC0xNTI3NTI1NjgxLDg2MTQzNDEwNSwxOTczNTA3NjAwLC
0xMzEwMjg0OV19
-->