# Fundamentals of Testing in Javascript

## 1 Intro
- We are going to create our own "mini"-jest. 

## 2 Throw Error with a Simple Test

```js
/*
We have a function 'sum' which isn't
working properly so we write a simple test.
*/
const sum = (a, b) => a - b;
const subtract (a, b) => a - b; 

const result = sum(3, 7);
const expected = 10;

if(result !== expected) {
	throw new Error(`${result} is not equal to ${expected}`)
}
```

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbOTA4NDAyNDU3LDE1ODczNjg3MjIsMzY3OT
E3NTk3LDgzNzY4NTY5N119
-->