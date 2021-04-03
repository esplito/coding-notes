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
let result, expected;

result = sum(3, 7);
expected = 10;

expect(result).toBe(expected);

result = subtract(7, 3);
expected = 4;

expect(result).toBe(expected);

function test(title, callback) {
	try {
		callback();
		console.log(`‹ ${title}`);
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
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTI3ODA2Nzk3OSwxNTg3MzY4NzIyLDM2Nz
kxNzU5Nyw4Mzc2ODU2OTddfQ==
-->