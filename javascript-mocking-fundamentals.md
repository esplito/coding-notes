# Javascript Mocking Fundamentals

## Intro

In this module we will try to mock both with jest and without any framework to get an understanding of what jest is doing in the background.

Github repo here: https://github.com/kentcdodds/js-mocking-fundamentals

## Override Object Properties to Mock with Monkey-patching in JavaScript

> Mocking allows our tests to be deterministic and ensure that we will get the expected result every time. - Dodds

> In this lesson, we’ll monkey patch our `getWinner` function to always return the same winner every time the function is called. After the test is run, we’ll clean up the mock and assign the original function implementation back to `getWinner`. - Dodds

The above is referred to as the "most naive" approach to mocking in Javascript. The approach means that we will override an object's properties in a test.

Pros:
✓ Pretty simple & straightforward

Cons:
╳  Fairly limited

Code example: https://github.com/kentcdodds/js-mocking-fundamentals/blob/main/src/no-framework/monkey-patching.js

## Ensure Functions are Called Correctly with JavaScript Mocks

> Often when writing JavaScript tests and mocking dependencies, you’ll want to verify that the function was called correctly. That requires keeping track of how often the function was called and what arguments it was called with. That way we can make assertions on how many times it was called and ensure it was called with the right arguments. - Dodds

Code with no framework: https://github.com/kentcdodds/js-mocking-fundamentals/blob/main/src/no-framework/mock-fn.js

Code with jest: https://github.com/kentcdodds/js-mocking-fundamentals/blob/main/src/__tests__/mock-fn.js

## Restore the Original Implementation of a Mocked JavaScript Function with jest.spyOn 

`jest.spyOn` can help us avoid bookkeeping and simplify our test code. 

Code with `jest.spyOn` example: https://github.com/kentcdodds/js-mocking-fundamentals/blob/main/src/__tests__/spy.js

Code with "no framework" `spyOn`: https://github.com/kentcdodds/js-mocking-fundamentals/blob/main/src/no-framework/spy.js

## Mock a JavaScript module in a test

If we want to mock a ESModule export it doesn't allow this kind of monkey-patching. We have to instead mock the entire module so when the test requires the module they get the mocked one instead.

Code in jest: https://github.com/kentcdodds/js-mocking-fundamentals/blob/main/src/__tests__/inline-module-mock.js

Code with "no framework": https://github.com/kentcdodds/js-mocking-fundamentals/blob/main/src/no-framework/inline-module-mock.js

>In Jest, we can put this `jest.mock` call anywhere, and Jest will ensure that our mock is used when the `thumb-war` requires the `utils` module. An interesting fact with the way that it works is, before Jest runs our code, it transforms that to move the `jest.mock` call up to the top of the file to ensure that the mock is in place before any of our modules are loaded. - Dodds

## Make a shared JavaScript mock module

If we want to mock our file throughout the test in the codebase, we can create a `__mocks__` directory where we put our mock file.

Code that uses it with jest: https://github.com/kentcdodds/js-mocking-fundamentals/blob/main/src/__tests__/external-mock-module.js

Code that uses it with "no framework": https://github.com/kentcdodds/js-mocking-fundamentals/blob/main/src/no-framework/external-mock-module.js



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjE0NTA1MTQ3NV19
-->