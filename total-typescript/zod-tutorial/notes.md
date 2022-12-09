
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
**Data before parsing:**
```js
{
    count: 82,
    next: 'https://swapi.dev/api/people/?page=2',
    previous: null,
    results: [{
            name: 'Luke Skywalker',
            height: '172',
            mass: '77',
            hair_color: 'blond',
            skin_color: 'fair',
            eye_color: 'blue',
            birth_year: '19BBY',
            gender: 'male',
            homeworld: 'https://swapi.dev/api/planets/1/',
            films: [Array],
            species: [],
            vehicles: [Array],
            starships: [Array],
            created: '2014-12-09T13:50:51.644000Z',
            edited: '2014-12-20T21:17:56.891000Z',
            url: 'https://swapi.dev/api/people/1/'
        },
        {
            name: 'C-3PO',
            height: '167',
            mass: '75',
            hair_color: 'n/a',
            skin_color: 'gold',
            eye_color: 'yellow',
            birth_year: '112BBY',
            gender: 'n/a',
            homeworld: 'https://swapi.dev/api/planets/1/',
            films: [Array],
            species: [Array],
            vehicles: [],
            starships: [],
            created: '2014-12-10T15:10:51.357000Z',
            edited: '2014-12-20T21:17:50.309000Z',
            url: 'https://swapi.dev/api/people/2/'
        }
		// I removed the rest of the objects for readability
    ]
}
```
**Data after parsing:**
```js
{
  results: [
    { name: 'Luke Skywalker' },
    { name: 'C-3PO' },
    { name: 'R2-D2' },
    { name: 'Darth Vader' },
    { name: 'Leia Organa' },
    { name: 'Owen Lars' },
    { name: 'Beru Whitesun lars' },
    { name: 'R5-D4' },
    { name: 'Biggs Darklighter' },
    { name: 'Obi-Wan Kenobi' }
  ]
}
```

## 4. Extract a Type from a Parser Object

### Exercise 4
Exercise: https://github.com/total-typescript/zod-tutorial/blob/main/src/04-infer.problem.ts

My solution:
```ts
import { z } from "zod";

const StarWarsPerson = z.object({
  name: z.string(),
});

const StarWarsPeopleResults = z.object({
  results: z.array(StarWarsPerson),
});

// All I needed to do to get the correct type
type StarWarsPeopleResults = z.infer<typeof StarWarsPeopleResults>;

const logStarWarsPeopleResults = (data: StarWarsPeopleResults) => {
  //                                    ^ 🕵️‍♂️
  data.results.map((person) => {
    console.log(person.name);
  });
};
```

## 5. Make Schemas Optional

### Exercise 5
Exercise: https://github.com/total-typescript/zod-tutorial/blob/main/src/05-optional.problem.ts

Goal: Make `phoneNumber` optional.

My solution:
```ts
const Form = z.object({
  name: z.string(),
  phoneNumber: z.string().optional(),
  //                     ^ 🕵️‍♂️
});
```

**Side note:**
This is the type that `Form` gets:
```ts
type FormType = {
  phoneNumber?: string | undefined;
  name: string;
};
```
> 💡 Try `type FormType = z.infer<typeof Form>` to see this.


## 6. Set a Default Value with Zod

### Exercise 6
Exercise: https://github.com/total-typescript/zod-tutorial/blob/main/src/06-default.problem.ts 

Goal: Set a default value for `keywords` using Zod.

My solution:
```ts
const Form = z.object({
  repoName: z.string(),
  keywords: z.array(z.string()).optional().default([]),
});
```

Matt's solution:
```ts
const Form = z.object({
  repoName: z.string(),
  keywords: z.array(z.string()).default([]),
  //                           ^ 🕵️‍♂️
});
```

> 💡 Optional is not really needed since we set a default value.

**Side note:**
Matt mentioned that we can use `z.input` and `z.infer` to get types for our input and output in the form.

```ts 
type FormInput = z.input<typeof Form>;
``` 
`FormInput` would then get the following type:
```ts
type FormInput = {
  keywords?: string[] | undefined;
  repoName: string;
};
```

```ts 
type FormInput = z.input<typeof Form>;
``` 
And `FormOutput` would get:
```ts
type FormOutput = {
  repoName: string;
  keywords: string[];
};
```
## 7. Be Specific with Allowed Types

### Exercise 7
Exercise: https://github.com/total-typescript/zod-tutorial/blob/main/src/07-union.problem.ts

Goal: Make `privacyLevel` more specific than just allowing any `string`.

My solution:
```ts
const Form = z.object({
  repoName: z.string(),
  privacyLevel: z.enum(["public", "private"]),
  //              ^ 🕵️‍♂️
});
```

**Side note:**
The inferred type that I get for `Form` when using `z.enum()` is the following:
```ts
type Form = {
  repoName: string;
  privacyLevel: "public" | "private";
};
```

Matt's alternative 1 solution (using `z.union()` and `z.literal()`:
```ts
const Form = z.object({
  repoName: z.string(),
  privacyLevel: z.union(
    [
      z.literal("private"),
      z.literal("public")
    ]
  ),
});
```

> 💡 Matt said that `z.enum()` is just syntactic sugar for creating the same thing as we get with `z.union()` + `z.literal()`.

## 8. Complex Schema Validation

### Exercise 8
Exercise: https://github.com/total-typescript/zod-tutorial/blob/main/src/08-validations.problem.ts

Goals:
- If `phoneNumber` is passed it must be at least 5 digits and at most 20 digits
- `email` must have a valid email format
- If `website` is passed it must be a valid website url

My solution:
```ts
const Form = z.object({
  name: z.string(),
  phoneNumber: z.string().min(5).max(20).optional(),
  email: z.string().email(),
  website: z.string().url().optional(),
});
```

Matt also suggested to add `.min(1)` to `name` because we should not be able to pass in an empty string.

So the final solution would be:
```ts
const Form = z.object({
  name: z.string().min(1),
  phoneNumber: z.string().min(5).max(20).optional(),
  email: z.string().email(),
  website: z.string().url().optional(),
});
```

> 💡 "Note that we can't use `.optional().min()` because `min` doesn't exist on the optional types. This means we have to put `.optional()` at the end after other validations." - Matt


## 9. Reduce Duplicated Code by Composing Schemas

### Exercise 9
Exercise: https://github.com/total-typescript/zod-tutorial/blob/main/src/09-composing-objects.problem.ts

Goal: Use the Zod apis to make the code a bit cleaner and DRY. (`type cases` should not get any type errors after refactoring)

My first solution:
```ts
const Id = { id: z.string().uuid()}

const User = z.object({
  name: z.string(),
}).extend(Id);

const Post = z.object({
  title: z.string(),
  body: z.string(),
}).extend(Id);

const Comment = z.object({
  text: z.string(),
}).extend(Id);
```

Another solution with `.extend()`:
```ts
const ObjectWithId = z.object({ id: z.string().uuid() });

const User = ObjectWithId.extend({
  name: z.string(),
});

const Post = ObjectWithId.extend({
  title: z.string(),
  body: z.string(),
});

const Comment = ObjectWithId.extend({
  text: z.string(),
});
```

Another solution with `.merge()`:
```ts
const ObjectWithId = z.object({ id: z.string().uuid() });

const User = ObjectWithId.merge(
  z.object({
    name: z.string(),
  })
);

const Post = ObjectWithId.merge(
  z.object({
    title: z.string(),
    body: z.string(),
  })
);

const Comment = ObjectWithId.merge(
  z.object({
    text: z.string(),
  })
);
```
**Side note:**
> 💡Using  `.merge()`  is slightly more verbose than  `.extend()`. We have to pass in a  `z.object()`  that contains the name  `z.string()`.

Merging is generally used when two different types are being combined, rather than just extending a single type.

Solution 1 that Matt shows:
```ts
const Id = z.string().uuid();

const User = z.object({
  id: Id,
  name: z.string(),
});

const Post = z.object({
  id: Id,
  title: z.string(),
  body: z.string(),
});

const Comment = z.object({
  id: Id,
  text: z.string(),
});
```

## 10. Transform Data from Within a Schema

### Exercise 10
My solution:
```ts
```
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbNjg1Nzg5MTIwLDE5NjU0OTY4MjcsLTI5MD
c0MjM1Niw1OTE4OTMyMiwtMTI5MTg2NDAxMiwxODk4NzE0ODIz
LDE4ODE2MzU4NDgsLTE3MDcwMzY4MjYsLTg1ODEwNDg5MSw2OD
QxOTQwOTQsMzEwNDY0NTMwLDE1MTkwOTc3MTAsLTEwNjU1Njcz
ODksMTMyMDUxNDAwMiwtMTM1NDU3OTc0NiwyNjgzOTgyODQsLT
k0NDk4ODU4MCwxNzg0Njg4ODI0LDE2MTUwNjUyMDQsMzc5NzQ4
OTAzXX0=
-->