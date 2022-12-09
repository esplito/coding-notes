
# Zod Tutorial by Matt Pocock

Github repo: https://github.com/total-typescript/zod-tutorial

Tutorial: https://www.totaltypescript.com/tutorials/zod

In this markdown file you can find my notes from when I completed Matt Pocock's Total TypeScript - Zod Tutorial.

## 1. Runtime Type Checking with Zod

### Exercise 1
Exercise: https://github.com/total-typescript/zod-tutorial/blob/main/src/01-number.problem.ts

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
Exercise: https://github.com/total-typescript/zod-tutorial/blob/main/src/02-object.problem.ts

My solution:
```ts
const PersonResult = z.object({
    name: z.string()
});
```

**Data before parsing:**
```js
data: {
    name: 'C-3PO',
    height: '167',
    mass: '75',
    hair_color: 'n/a',
    skin_color: 'gold',
    eye_color: 'yellow',
    birth_year: '112BBY',
    gender: 'n/a',
    homeworld: 'https://swapi.dev/api/planets/1/',
    films: [
        'https://swapi.dev/api/films/1/',
        'https://swapi.dev/api/films/2/',
        'https://swapi.dev/api/films/3/',
        'https://swapi.dev/api/films/4/',
        'https://swapi.dev/api/films/5/',
        'https://swapi.dev/api/films/6/'
    ],
    species: ['https://swapi.dev/api/species/2/'],
    vehicles: [],
    starships: [],
    created: '2014-12-10T15:10:51.357000Z',
    edited: '2014-12-20T21:17:50.309000Z',
    url: 'https://swapi.dev/api/people/2/'
}
```

When you then run `PersonResult.parse(data)` it will strip away the fields that it does not recognize.

**Data after parsing:**
```js
{ name: 'C-3PO' }
```


## 3. Create an Array of Custom Types

### Exercise 3
Exercise: https://github.com/total-typescript/zod-tutorial/blob/main/src/03-array.problem.ts

My solution:
```ts
const StarWarsPerson = z.object({
    name: z.string(),
});

const StarWarsPeopleResults = z.object({
    results: StarWarsPerson.array()
});
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

## 8. Complex Schema Validation

### Exercise 8
My solution:
```ts
```

## 9. Reduce Duplicated Code by Composing Schemas

### Exercise 9
My solution:
```ts
```

## 10. Transform Data from Within a Schema

### Exercise 10
My solution:
```ts
```
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjEyMzEzMTA2NywtOTQ0OTg4NTgwLDE3OD
Q2ODg4MjQsMTYxNTA2NTIwNCwzNzk3NDg5MDNdfQ==
-->