# Fundamentals of Testing in Javascript

## Intro
- We are going to create our own "mini"-jest. 

## Throw Error with a Simple Test

```js
/*
We have a function 'sum' which isn't
working properly so we write a simple test.
*/
const sum = (a, b) => a - b;
const subtract (a, b) => a - b; 

let result = sum(3, 7);
let expected = 10;

// Test sum-function
if(result !== expected) {
  throw new Error(`${result} is not equal to ${expected}`)
}

// Test subtract-function
// This is the most fundamental form of a test in JavaScript.
result = subtract(7, 3);
expected = 4;

if(result !== expected) {
  throw new Error(`${result} is not equal to ${expected}`)
}

```

## Abstract Test Assertions -> JavaScript Assertion Library

```js
let result, expected;

result = sum(3, 7);
expected = 10;

expect(result).toBe(expected);

result = subtract(7, 3);
expected = 4;

expect(result).toBe(expected);

// Expect is like an assertion library
function expect(actual) {
  return {
    toBe(expected) {
        if (actual !== expected) {
            throw new Error(`${actual} is not equal to ${expected}`)
        }
    },
    // We can add more assertions
    toEqual(expected) {},
    toBeGreaterThan(expected) {}
  }
}
```

## Encapsulate & Isolate Tests -> Build a Javascript Testing Framework

`testing-framework.js`
```js
test('sum adds numbers', () => {
  const result = sum(3, 7);
  const expected = 10;
  expect(result).toBe(expected);
});

test('subtract subtracts number', () => {
  const result = subtract(7, 3);
  const expected = 4;
  expect(result).toBe(expected);
});

function test(title, callback) {
  try {
    callback();
    console.log(`✓ ${title}`);
  } catch (error) {
    console.error(`X ${title}`);
    console.error(error);
  }
}

// Expect is like an assertion library
function expect(actual) {
  return {
    toBe(expected) {
      if (actual !== expected) {
        throw new Error(`${actual} is not equal to ${expected}`)
      }
    },
  }
}
```

## Support Async Tests - Promises through async await

```js
test('sum adds numbers asynchronously', async () => {
  const result = await sumAsync(3, 7);
  const expected = 10;
  expect(result).toBe(expected);
});

test('subtract subtracts number asynchronously', async () => {
  const result = await subtractAsync(7, 3);
  const expected = 4;
  expect(result).toBe(expected);
});

// this will work for both synchronous and asynchronous tests
async function test(title, callback) {
  try {
    await callback(); // this will work for both synchronous and asynchronous tests
    console.log(`✓ ${title}`);
  } catch (error) {
    console.error(`X ${title}`);
    console.error(error);
  }
}

// Expect is like an assertion library
function expect(actual) {
  return {
    toBe(expected) {
      if (actual !== expected) {
        throw new Error(`${actual} is not equal to ${expected}`)
      }
    },
  }
}
```

## Provide Testing Helper Functions as Globals

We could take the following code and put into a `setup-globals.js`-file:

```js
// this will work for both synchronous and asynchronous tests
async function test(title, callback) {
  try {
    await callback(); // this will work for both synchronous and asynchronous tests
    console.log(`✓ ${title}`);
  } catch (error) {
    console.error(`X ${title}`);
    console.error(error);
  }
}

// Expect is like an assertion library
function expect(actual) {
  return {
    toBe(expected) {
      if (actual !== expected) {
        throw new Error(`${actual} is not equal to ${expected}`)
      }
    },
  }
}

global.test = test;
global.expect = expect;
```
Then we could require this file when we run our code and the tests will run just like before:
```terminal
node --require ./setup-globals lessons/globals.js
```

## Verify Custom JavaScript Tests with Jest
The utils that we have created mirror the utilities provided by Jest!

Run:
```terminal
npx jest
```
and we will see the same result but with a bit clearer error messages.

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTM1MjQ2NDc3NiwxMzg3MzAzMzQ3LC0yNz
gwNjc5NzksMTU4NzM2ODcyMiwzNjc5MTc1OTcsODM3Njg1Njk3
XX0=
-->