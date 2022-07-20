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

### Extra-credit without `jest-in-case`
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
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDc2MDkyNDA1LC0xNTI3NTI1NjgxLDg2MT
QzNDEwNSwxOTczNTA3NjAwLC0xMzEwMjg0OV19
-->