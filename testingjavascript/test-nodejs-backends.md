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
- Expected to either call the `response` method to send a response to the caller _or_ call the `next` method which will continue the chain of middleware.

#### Example middleware function
```js
function someMiddlewareFunction(req, res, next) {
  console.log('Time:', Date.now())
  next()
}
```

#### Special case
```js
// The error middleware also accepts an error argument
function errorHandler(err, req, res, next) {
  console.error(err.stack)
  res.status(500).send('Something broke!')
}
```

In the exercises we will need to mock some functions and then it might be useful to check out the [jest mock-function api](https://jestjs.io/docs/mock-function-api).

## Write a Unit Test for Handling an UnauthorizedError

Kent recommends to test:
1. That we have responded with what we expected
	- Example: `expect(res.status).toHaveBeenCalledWith(401)`
2. That we have only responded the expected amount of times
	- Example: `expect(res.status).toHaveBeenCalledTimes(1)`

### First attempt
```js
// 🐨 Write a test for the UnauthorizedError case
test('responds with 401 for express-jwt UnauthorizedError', () => {
    const req = {}
    const next = jest.fn()
    const code = 'credentials_required'
    const message = 'No credentials were supplied'
    const error = new UnauthorizedError(code, {
        message,
    })
    const res = {
        json: jest.fn(() => res),
        status: jest.fn(() => res)
    }

    errorMiddleware(error, req, res, next)

    expect(next).not.toHaveBeenCalled()
    expect(res.status).toHaveBeenCalledWith(401)
    expect(res.status).toHaveBeenCalledTimes(1)
    expect(res.json).toHaveBeenCalledWith({
        code: error.code,
        message: error.message,
    })
    expect(res.json).toHaveBeenCalledTimes(1)
})

// 🐨 Write a test for the headersSent case

test('calls next if headersSent is true', () => {
    const req = {}
    const next = jest.fn()
    const error = new Error('some error')
    const res = {
        json: jest.fn(() => res),
        status: jest.fn(() => res),
        headersSent: true,
    }

    errorMiddleware(error, req, res, next)

    expect(next).toHaveBeenCalledWith(error)
    expect(next).toHaveBeenCalledTimes(1)
    expect(res.status).not.toHaveBeenCalled()
    expect(res.json).not.toHaveBeenCalled()
})

// 🐨 Write a test for the else case (responds with a 500)
test('responds with 500 and the error object', () => {
    const req = {}
    const next = jest.fn()
    const error = new Error('some error')
    const res = {
        json: jest.fn(() => res),
        status: jest.fn(() => res),
    }

    errorMiddleware(error, req, res, next)

    expect(next).not.toHaveBeenCalled()
    expect(res.status).toHaveBeenCalledWith(500)
    expect(res.status).toHaveBeenCalledTimes(1)
    expect(res.json).toBeCalledWith({
        message: error.message,
        stack: error.stack
    })
    expect(res.json).toHaveBeenCalledTimes(1)
})
```

### Extra credit 1: 💯 write a test object factory 
```js
function buildResponse(overrides) {
    const res = {
        json: jest.fn(() => res),
        status: jest.fn(() => res),
        ...overrides,
    }
    return res
}
```

If we create this test object factory, we can use `buildResponse`  in our tests and just pass the arguments that we would like to override:
```js
const res = buildResponse({
	headersSent: true,
})
```
This makes it more clear what we think is important in the test. (by hiding the details that stay the same throughout the tests) 

And if we just want to use it without any overrides:
```js
const res = buildResponse()
```

This basically the same thing as that Kent has shown on his blog and in EpicReact, where you got to create a `setup` function  that you could override.

### Extra credit 2:  💯  use  `utils/generate`

Kent has already prepared some test object factories: 
1. `buildRes`
2. `buildReq`
3. `buildNext`

In this extra credit we simply import them instead. You can find the [test object factories here](https://github.com/kentcdodds/testing-node-apps/blob/main/test/utils/generate.js).

#### Usage
```js
import {buildNext, buildReq, buildRes} from  'utils/generate'

const req = buildReq()
const next = buildNext()
const res = buildRes({
    headersSent: true,
})
```

The `buildReq` will most likely, in a bigger application, set up quite a bit of objects on the request, but in this one it only sets a `user`, `body` and `params`.

In the `buildRes` factory, Kent has also used the `.mockName` which will give better error messages:
```js
function buildRes(overrides = {}) {
    const res = {
        json: jest.fn(() => res).mockName('json'),
        status: jest.fn(() => res).mockName('status'),
        ...overrides,
    }
    return res
}
```

## Test Node Controllers Overview
In this part we'll test controllers and Kent has created a  [markdown file with instructions](https://github.com/kentcdodds/testing-node-apps/blob/tjs/src/routes/__tests__/list-items-controller.md).

- Testing controllers is a bit more difficult and requires more setup and cleanup.
- We also deal with more complex business logic.
- We'll mock our interactions with the database

Kent's explanation to why we mock the database interaction:
>1.  Test speed: Database/Service interactions will make our tests run slower.
>2.  Test simplicity: Database/Service interactions will make our tests require more complex setup/teardown logic.
>3.  Test stability: Database/Service interactions will make our tests more flaky by relying on services that may be outside our control.
>
> 🦉  While we get benefits by mocking databases and services, it's wise to not forget what we're giving up. Read  [Testing Implementation Details](https://kentcdodds.com/blog/testing-implementation-details)  and  [The Merits of Mocking](https://kentcdodds.com/blog/the-merits-of-mocking)  for more info.

This exercise is done in the `list-items-controller.exercise.js` and it uses the code from `list-items-controller.js`.

### Exercise notes

Kent mentions the following about the async/await syntax used in this project:
>🦉 Note that we're using `express-async-errors` in this project. That means that we can use `async/await` in our middleware and we don't have to call `next`. So you can call the middleware function with and `await` in your test.

#### First attempt

```js
test('getListItem returns the req.listItem', async () => {
    // 🐨 create a user
    const user = generate.buildUser()
    // 🐨 create a book
    const book = generate.buildBook()
    // 🐨 create a listItem that has the user as the owner and the book
    const listItem = generate.buildListItem({
        ownerId: user.id,
        bookId: book.id
    })
    // 🐨 mock booksDB.readById to resolve to the book
    // 💰 use mockResolvedValueOnce
    booksDB.readById.mockResolvedValueOnce(book)
    // 🐨 make a request object that has properties for the user and listItem
    // 💰 checkout the implementation of getListItem in ../list-items-controller
    // to see how the request object is used and what properties it needs.
    // 💰 and you can use buildReq from utils/generate
    const req = generate.buildReq({
        user,
        listItem,
    })
    // 🐨 make a response object
    // 💰 just use buildRes from utils/generate
    const res = generate.buildRes()
    // 🐨 make a call to getListItem with the req and res (`await` the result)
    await listItemsController.getListItem(req, res)
    // 🐨 assert that booksDB.readById was called correctly
    expect(booksDB.readById).toHaveBeenCalledWith(book.id)
    expect(booksDB.readById).toHaveBeenCalledTimes(1)
    //🐨 assert that res.json was called correctly
    expect(res.json).toHaveBeenCalledWith({
        listItem: {
            ...listItem,
            book
        }
    })
    expect(res.json).toBeCalledTimes(1)
})
```

#### Extra credit 1 💯 Use toMatchInlineSnapshot for errors

Kent mentions that `res.json.mock.calls[0]` is basically checking the same thing as `expect(res.json).toHaveBeenCalledWith`.

**Why use `toMatchInlineSnapshot`?**
Because if you were to change the error message that you are testing (when using `toHaveBeenCalledWith`), you would need to update the expected text in the test also.

_With `toMatchInlineSnapshot`_ you only need to press the `u` key in jest watch mode to update the snapshot.  The content of `toMatchInlineSnapshot` would then be updated. 
Example: `No bookId provided` changed to `no bookId provided`

**Without `toMatchInlineSnapshot`**
```js
test('createListItem returns a 400 error if no bookId is provided', async () => {
    const req = generate.buildReq({
	    body: {
	        bookId: undefined
	    }
	})
    const res = generate.buildRes()

    await listItemsController.createListItem(req, res)

    expect(res.status).toHaveBeenCalledWith(400)
    expect(res.status).toHaveBeenCalledTimes(1)
    expect(res.json).toHaveBeenCalledWith({
        message: `No bookId provided`
    })
    expect(res.json).toHaveBeenCalledTimes(1)
})
```

**With `toMatchInlineSnapshot`**
```js
// Extra credit 1: 💯 Use toMatchInlineSnapshot for errors
test('createListItem returns a 400 error if no bookId is provided', async () => {
    const req = generate.buildReq({
	    body: {
	        bookId: undefined
	    }
	})
    const res = generate.buildRes()

    await listItemsController.createListItem(req, res)

    expect(res.status).toHaveBeenCalledWith(400)
    expect(res.status).toHaveBeenCalledTimes(1)
    expect(res.json).toHaveBeenCalledTimes(1)
    expect(res.json.mock.calls[0]).toMatchInlineSnapshot(`
  Array [
    Object {
      "message": "No bookId provided",
    },
  ]
  `)
})
```
#### Extra credit 2 💯 Test everything else

**Error Case & happy creation flow for createListItem**
```js
test('createListItem returns 400 if the user already has a listitem for the provided bookid', async () => {
  const user = generate.buildUser({id: 'FAKE_USER_ID'})
  const book = generate.buildBook({id: 'FAKE_BOOK_ID'})
  const existingListItem = generate.buildListItem({
    ownerId: user.id,
    bookId: book.id,
  })
  listItemsDB.query.mockResolvedValueOnce([existingListItem])

  const req = generate.buildReq({
    user,
    body: {bookId: book.id},
  })
  const res = generate.buildRes()

  await listItemsController.createListItem(req, res)

  expect(listItemsDB.query).toHaveBeenCalledWith({
    ownerId: user.id,
    bookId: book.id,
  })
  expect(listItemsDB.query).toHaveBeenCalledTimes(1)

  expect(res.status).toHaveBeenCalledWith(400)
  expect(res.status).toHaveBeenCalledTimes(1)
  expect(res.json.mock.calls[0]).toMatchInlineSnapshot(`
    Array [
      Object {
        "message": "User FAKE_USER_ID already has a list item for the book with the ID FAKE_BOOK_ID",
      },
    ]
  `)
  expect(res.json).toHaveBeenCalledTimes(1)
})

// Happy Path of Creation Flow
test('createListItem creates and returns a listItem', async () => {
  const user = generate.buildUser()
  const book = generate.buildBook()
  const createdListItem = generate.buildListItem({
    ownerId: user.id,
    bookId: book.id,
  })

  listItemsDB.query.mockResolvedValueOnce([])
  listItemsDB.create.mockResolvedValueOnce(createdListItem)
  booksDB.readById.mockResolvedValueOnce(book)

  const req = generate.buildReq({
    user,
    body: {bookId: book.id},
  })
  const res = generate.buildRes()

  await listItemsController.createListItem(req, res)

  expect(listItemsDB.query).toHaveBeenCalledWith({
    ownerId: user.id,
    bookId: book.id,
  })
  expect(listItemsDB.query).toHaveBeenCalledTimes(1)
  expect(listItemsDB.create).toHaveBeenCalledWith({
    ownerId: user.id,
    bookId: book.id,
  })
  expect(listItemsDB.create).toHaveBeenCalledTimes(1)
  expect(booksDB.readById).toHaveBeenCalledWith(book.id)
  expect(res.json).toHaveBeenCalledWith({listItem: {...createdListItem, book}})
  expect(res.json).toHaveBeenCalledTimes(1)
})
```
> **Unit Test Business Logic of a Controller’s Middleware** - Not all controller middleware send responses. In this unit test, we’ll verify that a controller’s middleware interacts with the request, response, next function, and database correctly.
```js
test('setListItem sets listItem to req.listItem', async () => {
  const user = generate.buildUser()
  const listItem = generate.buildListItem({
    ownerId: user.id,
  })

  listItemsDB.readById.mockResolvedValueOnce(listItem)

  const req = generate.buildReq({
    user,
    params: {
      id: listItem.id,
    },
  })
  const res = generate.buildRes()
  const next = generate.buildNext()

  await listItemsController.setListItem(req, res, next)

  expect(listItemsDB.readById).toHaveBeenCalledWith(listItem.id)
  expect(listItemsDB.readById).toHaveBeenCalledTimes(1)

  expect(next).toHaveBeenCalledWith(/* NOTHING! */)
  expect(next).toHaveBeenCalledTimes(1)

  expect(req.listItem).toBe(listItem)
})
```
**404 edge case**
In the next one we test a 404 edge case and we use toMatchInlineSnapshot for asserting on the error message.
```js
test('setListItem returns 404 if no book is found', async () => {
  const fakeListItemId = 'FAKE_LIST_ITEM_ID'
  const req = generate.buildReq({
    params: {
      id: fakeListItemId,
    },
  })
  const res = generate.buildRes()
  const next = generate.buildNext()

  listItemsDB.readById.mockResolvedValueOnce(null)

  await listItemsController.setListItem(req, res, next)

  expect(listItemsDB.readById).toHaveBeenCalledWith(fakeListItemId)
  expect(listItemsDB.readById).toHaveBeenCalledTimes(1)

  expect(next).not.toHaveBeenCalled()

  expect(res.status).toHaveBeenCalledWith(404)
  expect(res.status).toHaveBeenCalledTimes(1)
  expect(res.json.mock.calls[0]).toMatchInlineSnapshot(`
    Array [
      Object {
        "message": "No list item was found with the id of FAKE_LIST_ITEM_ID",
      },
    ]
  `)
  expect(res.json).toHaveBeenCalledTimes(1)
})
```

**Unauthorized use case**
Next up is to check that we don't allow unauthorized users to setListItem on a list item that they don't own.

> In this one we handle dynamic data in a snapshot by making a consistent ID for the user and list item.
```js
test('setListItem returns 403 unauthorized if user is not owner of the listItem', async () => {
  const user = generate.buildUser({id: 'FAKE_USER_ID'})
  const listItem = generate.buildListItem({
    ownerId: 'ANOTHER_USER',
    id: 'FAKE_LIST_ITEM_ID',
  })
  listItemsDB.readById.mockResolvedValueOnce(listItem)

  const req = generate.buildReq({
    user,
    params: {
      id: listItem.id,
    },
  })
  const res = generate.buildRes()
  const next = generate.buildNext()

  await listItemsController.setListItem(req, res, next)

  expect(listItemsDB.readById).toHaveBeenCalledWith(listItem.id)
  expect(listItemsDB.readById).toHaveBeenCalledTimes(1)

  expect(next).not.toHaveBeenCalled()

  expect(res.status).toHaveBeenCalledWith(403)
  expect(res.status).toHaveBeenCalledTimes(1)
  expect(res.json.mock.calls[0]).toMatchInlineSnapshot(`
    Array [
      Object {
        "message": "User with id FAKE_USER_ID is not authorized to access the list item FAKE_LIST_ITEM_ID",
      },
    ]
  `)
  expect(res.json).toHaveBeenCalledTimes(1)
})
```

**Test getListItems for Getting Multiple Mock Objects**

```js
test(`getListItems returns a user's list items`, async () => {
  const user = generate.buildUser()
  const books = [generate.buildBook(), generate.buildBook()]
  const userListItems = [
    generate.buildListItem({
      ownerId: user.id,
      bookId: books[0].id,
    }),
    generate.buildListItem({
      ownerId: user.id,
      bookId: books[1].id,
    }),
  ]

  listItemsDB.query.mockResolvedValueOnce(userListItems)
  booksDB.readManyById.mockResolvedValueOnce(books)

  const req = generate.buildReq({user})
  const res = generate.buildRes()

  await listItemsController.getListItems(req, res)

  expect(listItemsDB.query).toHaveBeenCalledWith({ownerId: user.id})
  expect(listItemsDB.query).toHaveBeenCalledTimes(1)

  expect(booksDB.readManyById).toHaveBeenCalledWith([books[0].id, books[1].id])
  expect(booksDB.readManyById).toHaveBeenCalledTimes(1)

  expect(res.json).toHaveBeenCalledWith({
    listItems: [
      {...userListItems[0], book: books[0]},
      {...userListItems[1], book: books[1]},
    ],
  })
  expect(res.json).toHaveBeenCalledTimes(1)
})
```
**Happy path for update flow**
```js
test('updateListItem updates an existing item and returns it', async () => {
  const book = generate.buildBook({id: 'FAKE_BOOK_ID'})
  const listItem = generate.buildListItem({
    id: 'FAKE_LIST_ITEM_ID',
    bookId: book.id,
  })
  const updates = {
    notes: generate.notes(),
  }
  const mergedListItemAndUpdates = {...listItem, ...updates.notes}
  listItemsDB.update.mockResolvedValueOnce(mergedListItemAndUpdates)
  booksDB.readById.mockResolvedValueOnce(book)

  const req = generate.buildReq({listItem, body: updates})
  const res = generate.buildRes()

  await listItemsController.updateListItem(req, res)

  expect(listItemsDB.update).toHaveBeenCalledWith(listItem.id, updates)
  expect(listItemsDB.update).toHaveBeenCalledTimes(1)

  expect(booksDB.readById).toHaveBeenCalledWith(listItem.bookId)
  expect(booksDB.readById).toHaveBeenCalledTimes(1)

  expect(res.json).toHaveBeenCalledWith({
    listItem: {...mergedListItemAndUpdates, book},
  })
  expect(res.json).toHaveBeenCalledTimes(1)
})
```
**Testing deletion of resource**
> All we need to do here is verify that the remove method of our was called properly, but we don’t need to bother mocking out what it returns because our code doesn’t use that return value. We also want to verify that the res.json was called properly, so we’ll do that as well. - Kent C. Dodds
```js
test('deleteListItem deletes a list item', async () => {
  const listItem = generate.buildListItem({id: 'FAKE_LIST_ITEM_ID'})

  const req = generate.buildReq({listItem})
  const res = generate.buildRes()

  await listItemsController.deleteListItem(req, res)

  expect(listItemsDB.remove).toHaveBeenCalledWith(listItem.id)
  expect(listItemsDB.remove).toHaveBeenCalledTimes(1)

  expect(res.json).toHaveBeenCalledWith({success: true})
  expect(res.json).toHaveBeenCalledTimes(1)
})
```
## Test Authentication API  Routes - Overview

In this part we'll start doing some server integration tests and Kent has created a  [markdown file with instructions](https://github.com/kentcdodds/testing-node-apps/blob/tjs/src/__tests__/auth.md).

In this section we'll do as minimal mocking/faking as we possibly can. For simplification of the exercises we are using an in-memory database (which most likely would not do in a production api).

According to Kent, the biggest challenges with these types of tests are that:
1.  They _take more setup_
2.  They _have more points of failure_

He also mentions some good stuff about how to handle the server and database in these types of tests:
>The setup required includes getting the server started. The way I've structured it for this project is the way I recommend structuring it for all Node apps as it's a great way to run tests in isolation from one another but still in parallel. As mentioned, you'll also need to make sure that your database and any other services your app depends on are running or properly mocked.
>
>Another piece to the setup often includes making certain that the database is in a clean state. Sometimes you can get away with a database that has a lot of old test data in it, but it's better if you can have the database cleared out for every test.  **Tests like these are often more comprehensive**  in part for this reason as well as the fact that it makes writing the tests a little easier and more useful.

**What's good about these tests then?**
1. They give a lot of confidence
2. And give you fewer false negatives and false positives

Exercises for this can be found under `/src/__tests__/auth.exercise.js`.

### Exercises - notes

First off, we need to get our API server and database started and in a clean state.

>Because our tests and server both run in node, we can do this all using the utilities that Jest exposes for us like beforeAll and afterAll. 

```js
// Testing Authentication API Routes

// 🐨 import the things you'll need
// 💰 here, I'll just give them to you. You're welcome
import axios from 'axios'
import {resetDb} from 'utils/db-utils'
import * as generate from 'utils/generate'
import startServer from '../start'

// 🐨 you'll need to start/stop the server using beforeAll and afterAll
// 💰 This might be helpful: server = await startServer({port: 8000})
let server
beforeAll(async () => {
  server = await startServer({port: 8000})
})
afterAll(async () => {
  server.close()
})

// 🐨 beforeEach test in this file we want to reset the database
beforeEach(() => {
  resetDb()
})

test('auth flow', async () => {
  const userData = generate.loginForm()
  
  const resultRegister = await axios.post(
    'http://localhost:8000/api/auth/register',
    userData,
  )

  expect(resultRegister.data.user).toEqual({
    id: expect.any(String),
    token: expect.any(String),
    username: userData.username,
  })

  const resultLogin = await axios.post(
    'http://localhost:8000/api/auth/login',
    userData,
  )
  
  expect(resultRegister.data.user).toEqual(resultLogin.data.user)
  
  const userToken = resultLogin.data.user.token
  const resultMe = await axios.get('http://localhost:8000/api/auth/me', {
    headers: {Authorization: `Bearer ${userToken}`},
  })
  
  expect(resultMe.data).toEqual(resultLogin.data)
})
```

>These `expect.any` are called asymmetric matchers. There are a bunch of those that are really, really helpful for situations where you're not exactly sure what the actual value is, but you still want to make some sort of assertion of what kind of value it is.

Kent comments on the fact that we are asserting the `user`-object from the register-response with the `user`-object from the logjn-response:

> With that, we're verifying that the  `token`  and the  `id`  are consistent, at least between these two API calls. We aren't verifying that the  `token`  works when making authenticator request, but that's what we're going to do next.

And he also highlights that there could be issues with this, depending on how your auth solution works:

>You could actually run into a problem if your authentication system generates a brand-new `token` every time somebody logs in, which is very possible. In that situation, you would want to change this assertion so that it doesn't assert on the `token` specifically. We'll cover that the `token` works with the authenticated request.

He also highlights why this test is important:

>This covers a lot of our code with not a whole lot of test. It does make it a lot easier to break this test because this test is touching so much of our code base. It also makes it a little harder to debug what it is that broke this test.
>
> Because this test is so free of implementation details, we know we cannot ship to production if this test is failing. I'd much rather have a test that breaks when it's supposed to, than have no test here at all.

#### 💯  Create a pre-configured axios client

**Error message before using `axios.interceptors` with `handleRequestFailure`**
```terminal
auth flow - with pre-configured axios client and interceptors

    Request failed with status code 400

      at createError (node_modules/axios/lib/core/createError.js:16:15)
      at settle (node_modules/axios/lib/core/settle.js:17:12)
      at IncomingMessage.handleStreamEnd (node_modules/axios/lib/adapters/http.js:244:11)
```

**Error message after using `handleRequestFailure`**
```terminal
auth flow - with pre-configured axios client and interceptors

    400: {"message":"username can't be blank"}
```

After using `getData` on `axios.interceptors.response.use` we can also skip adding `.data` when asserting the data. So `resultRegister.data` can instead be `resultRegister`.

These helpers can be found in [`/test/utils/async.js`](https://github.com/kentcdodds/testing-node-apps/blob/tjs/test/utils/async.js).

```js
// + rest of the imports from the previous example
import {getData, handleRequestFailure} from 'utils/async'

const api = axios.create({baseURL: 'http://localhost:8000/api'})
api.interceptors.response.use(getData, handleRequestFailure)

test('auth flow - with pre-configured axios client and interceptors', async () => {
  const userData = generate.loginForm()
  // register
  const resultRegister = await api.post('/auth/register', userData)
  expect(resultRegister.user).toEqual({
    id: expect.any(String),
    token: expect.any(String),
    username: userData.username,
  })

  // login
  const resultLogin = await api.post('/auth/login', userData)
  expect(resultRegister.user).toEqual(resultLogin.user)

  // authenticated request
  const userToken = resultLogin.user.token
  const resultMe = await api.get('/auth/me', {
    headers: {Authorization: `Bearer ${userToken}`},
  })
  expect(resultMe).toEqual(resultLogin)
})
```
#### 💯  Ensure a unique server port
> If you want to run your tests in parallel and each of your tests starts up the server on its own, then you could run into a problem where two servers try to bind to the same port. To avoid this, we can use the JEST_WORKER_ID environment variable so we can ensure that the port our server starts on is unique.

We get the following error if create a new integration test that runs at the same time as the other integration test:
```terminal
TypeError: Cannot read property 'close' of undefined

      16 | })
      17 | afterAll(async () => {
    > 18 |   server.close()
         |          ^
      19 | })
```

We can solve it by writing the following:
```js
const port = 8000 + Number(process.env.JEST_WORKER_ID);
```
and then replace the port in `startServer` and the `baseUrl`:
```js
server = await startServer({port})

const api = axios.create({baseURL: `http://localhost:${port}/api`})
```
_But_ it is a bit annoying, having to do this in every single test file. So instead, we can use our `setup-env.js`-file and set the port there with `process.env.PORT`:
```js
const port = 8000 + Number(process.env.JEST_WORKER_ID)
process.env.PORT = process.env.PORT || port
```

And then we can instead use:
```js
const api = axios.create({baseURL: `http://localhost:${process.env.PORT}/api`})
```

**Final solution for unique port**
However, we can take this a step further. We move our axios set up to the `beforeAll`:

```js
let api, server
beforeAll(async () => {
  server = await startServer()
  api = axios.create({baseURL: `http://localhost:${server.address().port}/api`})
  api.interceptors.response.use(getData, handleRequestFailure)
})
```
>The `server` has a special method called `address().port`. This server is an express server, and you access the port from this address method. Now, I don't care what the port is, I don't care how the server get its port. I just care, whatever that port is, that's what my `baseURL` should be.

#### 💯  Interact directly with the database

We import a util function called `resolve` to be able to resolve the "register"-request even though it is throwing an error. The test will fail otherwise and then we can't assert the content of the error message.

```js
// 💯 Interact directly with the database
test('should return error message if trying to register a user with an already existing username', async () => {
  // circumvene the api and insert a user in the db
  const username = generate.username()
  await usersDB.insert(generate.buildUser({username}))
  // try to register a user through the api with the same username
  const password = generate.password()

  const error = await api
    .post('/auth/register', {username, password})
    .catch(resolve)
  // assert error message with toMatchInlineSnapshot
  expect(error.message).toMatchInlineSnapshot(
    `"400: {\\"message\\":\\"username taken\\"}"`,
  )
})
```
#### 💯 Test all the edge cases
> If we take a look at the `auth-controller` and `res.status` calls, we're going to get a whole bunch of places where there are different edge cases that we might want to cover, like if the `username` can't be blank or the `password` can't be blank or the `password` is not strong enough. We've got a bunch of cases that we can cover here. - Kent C. Dodds

```js
// 💯 Test all the edge cases
test('GET me unauthenticated returns error', async () => {
  const error = await api.get('/auth/me').catch(resolve)
  expect(error).toMatchInlineSnapshot(
    `[Error: 401: {"code":"credentials_required","message":"No authorization token was found"}]`,
  )
})

test('username required to register', async () => {
  const error = await api
    .post('/auth/register', {password: generate.password()})
    .catch(resolve)
  expect(error).toMatchInlineSnapshot(
    `[Error: 400: {"message":"username can't be blank"}]`,
  )
})

test('password required to register', async () => {
  const error = await api
    .post('/auth/register', {username: generate.username()})
    .catch(resolve)
  expect(error).toMatchInlineSnapshot(
    `[Error: 400: {"message":"password can't be blank"}]`,
  )
})

test('username required to login', async () => {
  const error = await api
    .post('/auth/login', {password: generate.password()})
    .catch(resolve)
  expect(error).toMatchInlineSnapshot(
    `[Error: 400: {"message":"username can't be blank"}]`,
  )
})

test('password required to login', async () => {
  const error = await api
    .post('/auth/login', {username: generate.username()})
    .catch(resolve)
  expect(error).toMatchInlineSnapshot(
    `[Error: 400: {"message":"password can't be blank"}]`,
  )
})

test('user must exist to login', async () => {
  const error = await api
    .post('/auth/login', generate.loginForm({username: '__will_never_exist__'}))
    .catch(resolve)
  expect(error).toMatchInlineSnapshot(
    `[Error: 400: {"message":"username or password is invalid"}]`,
  )
})
```

>In review, for all of these edge cases, again, this is typically not the place you're going to be doing your edge case testing, but sometimes it can be useful. As you can see here, it's actually easy to do these kinds of edge case tests because, while making correct requests may be a bit of a challenge, it's pretty easy to make incorrect requests. - Kent C. Dodds

## Test CRUD API Routes Overview
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTM3MDA0MzIwNywtMTI2OTkyNTMxNiwtNz
I4MTYyNjQzLC0xNjYzOTY1MzQwLC0yODU1ODU5NzEsLTY0NDkx
ODAyMywxOTQ3MDIzNDI5LC0xOTcxOTQwNjMzLDEwNjgxMTcyMz
YsLTEwMDcyNTQ1MTQsLTE3ODkxMDAzODEsNTExNzU4NzYxLDE1
ODI2MDUwMzAsMTk0MDY2ODA0NywtMTg4MzUzMzc1NywtMTU4Mj
AyNzcsLTE1MTI0MjE2MzYsLTEyMzY0OTc3MTMsNzk3NDMwNzE3
LC0xMDcwNTkzMDMwXX0=
-->