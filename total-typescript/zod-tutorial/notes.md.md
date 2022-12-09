
# Zod Tutorial by Matt Pocock

Github repo: https://github.com/total-typescript/zod-tutorial

Tutorial: https://www.totaltypescript.com/tutorials/zod

In this markdown file you can find my notes from when I completed Matt Pocock's Total TypeScript - Zod Tutorial.

## 1. Runtime Type Checking with Zod

### Exercise 1
My solution:
```ts
const zodNumberParser = z.number();

export const toString = (num: unknown) => {
    zodNumberParser.parse(num)
    return String(num);
};
```

## 2. Verify Unknown APIs with an Object Schema

### Exercise 2
My solution:
```ts
```

## 3. Create an Array of Custom Types

### Exercise 3
My solution:
```ts
```

## 4. Extract a Type from a Parser Object

### Exercise 4
My solution:
```ts
```

## 5. Make Schemas Optional

### Exercise 5
My solution:
```ts
```

## 6. Set a Default Value with Zod

### Exercise 6
My solution:
```ts
```

## 7. Be Specific with Allowed Types

### Exercise 7
My solution:
```ts
```

## 8. Verify Unknown APIs with an Object Schema

### Exercise 8
My solution:
```ts
```

## 9. Verify Unknown APIs with an Object Schema

### Exercise 9
My solution:
```ts
```

## 10. Verify Unknown APIs with an Object Schema

### Exercise 10
My solution:
```ts
```
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyMDc2OTk4ODksNjkyMjI4OTg2XX0=
-->