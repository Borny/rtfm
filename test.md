# Test

- [Guidelines](#guidelines)
- [AAA Pattern](#aaa-pattern)
- [Test suite](#test-suite)
- [Unit tests](#unit-tests)
- [Integration tests](#integration-tests)
- [end-to-end tests](#end-to-end-tests)
- [Asynchronous Code](#asynchronous-code)
- [Testing Hooks](#testing-hooks)
- [Spies](#spies)
- [Mocks](#mocks)
- [Frameworks](#frameworks)
  - [Tape](#tape)
  - [Vitest](#vitest)

TAP => Test Anything Protocol

## Guidelines

Every new functions should be tested in a related file.  
A test file usually has the same name as the file with the functions it should test but ending with **.test.js** (e.g: some-file-name.test.js).

Only test your code, not third-party code/packages.

Keep the number of assertions ('expects') low.

## AAA Pattern

Try to follow the `AAA Pattern`: Arrange - Act - Assert

```js
test('Using the AAA Pattern', () => {
  // Arrange: define some values
  const validString = 'Some valid string'

  // Act: run some code/function
  const result = validString.trim().length !== 0

  // Assert: check if the test passes
  expect(result).toBeTruthy()
})
```

## Test suite

A test suite is a collection of test cases that are intended to be executed together.  
They make it easier to read by grouping the related tests (for the same function for example) together.

```js
function add(a, b) {
  return a + b;
}

function subtract(a, b) {
  return a - b;
}

describe('Math Functions', () => {
  // Test cases
  test('adds 1 + 2 to equal 3', () => {
    expect(add(1, 2)).toBe(3);
  });

  test('subtracts 5 - 2 to equal 3', () => {
    expect(subtract(5, 2)).toBe(3);
  });
});
```

## Unit tests

Unit tests will make sure that individual units such as functions, methods or components work as intended.

## Integration tests

Integration tests will test the interactions between multiple units or components.  
They test the combination of unit tests.

## end-to-end tests

## Asynchronous Code

```js
import jwt from 'jsonwebtoken';

export function generateTokenCallback(userEmail, doneFn) {
  jwt.sign({ email: userEmail }, 'secret123', doneFn);
}

export function generateTokenPromise(userEmail) {
  const promise = new Promise((resolve, reject) => {
    jwt.sign({ email: userEmail }, 'secret123', (error, token) => {
      if (error) {
        reject(error);
      } else {
        resolve(token);
      }
    });
  });

  return promise;
}
```

```js
test('generateTokenCallback should return a valid token', (done) => {
  // Arrange
  const userEmail = 'some@email.com';

  // Act
  generateTokenCallback(userEmail, (error, token) => {
    // Assert
    expect(token).toBeDefined();
    done();
  });
});

test('generateTokenPromise should return a valid token', () => {
  // Arrange
  const userEmail = 'some@email.com';

  // Act and Assert
  return expect(generateTokenPromise(userEmail)).resolves.toBeDefined();
  return expect(generateTokenPromise(userEmail)).resolves.toBeTypeOf('string');
});

it('should generate a token value with async', async () => {
  const testUserEmail = 'some@email.com'

  const token = await generateTokenPromise(testUserEmail)

  expect(token).toBeDefined()
  expect(token).toBeTypeOf('string')
})
```

## Testing hooks

Use hooks to make the code leaner:

- beforeAll()
- afterAll()
- beforeEach()
- afterEach()

```js
beforeAll(() => {
  // run some code here to setup the test
  // e.g: set up a database connection
})

afterAll(() => {
  // run some code here to clean up after the test
  // e.g: close the database connection
})

beforeEach(() => {
  // run some code before each test
  // e.g: set up some data
  const validString = 'Some valid string'
})

afterEach(() => {
  // run some code after each test
})

test('Using the AAA Pattern', () => {
  // Arrange: define some values (done in the beforeAll function)

  // Act: run some code/function
  const result = validString.trim().length !== 0

  // Assert: check if the test passes
  expect(result).toBeTruthy()
})
```

## Spies

`Spies` are functions that record how they were called.  

```js
import { vi, test, expect } from 'vitest';
import { validateNumber } from './validateNumber';

test('should call validateNumber with a number', () => {
  // Arrange
  const number = 1;
  const spy = vi.spyOn(global, 'validateNumber');

  // Act
  validateNumber(number);

  // Assert
  expect(spy).toHaveBeenCalled();
});
```

## Mocks

`Mocks` are objects that simulate the behavior of real objects.

```js
import { vi, test, expect } from 'vitest';
import { validateNumber } from './validateNumber';

test('should call validateNumber with a number', () => {
  // Arrange
  const number = 1;
  const mock = vi.mock(global, 'validateNumber');

  // Act
  validateNumber(number);

  // Assert
  expect(mock).toHaveBeenCalled();
});
```

## Frameworks

### Tape

#### Methods

Some of the **Tape** methods used:

| Name                | Methods                          |
|---------------------|----------------------------------|
| test()              | Main method that will run a test |
| test.skip()         | Will not run the test            |
| test.only()         | Will only run this test, cannot be applied to multiple tests          |
| assert.test()       | Will run a test inside a test    |
| assert.plan()       | Declares how many assertions should be run |
| assert.end()        | Will end the test explicitly            |
| assert.pass()       | Generates a passing assertion with a message  |
| assert.fail()       | Generates a failing assertion with a message |
| assert.ok()         | Assert that value is truthy with an optional description of the assertion message |
| assert.notOk()      | Assert that value is falsy with an optional description of the assertion message |
| assert.equal()      | Assert that Object.is(actual, expected) with an optional description of the assertion message |
| assert.deepEqual()  | Assert that actual and expected have the same structure and nested values |
| assert.looseEqual() | Assert that actual == expected with an optional description of the assertion message |

#### Syntax

```js
test('Add numbers', (assert) => {
  assert.plan(1) // will expect one test to pass
  // Arrange 
  const numbers = [1, 2, 3]

  // Act 
  const result = numbers.reduce((acc, curr) => {
      return acc + curr
  }, 0)
  
  // Assert
  assert.equal(result, 6, 'Should return the expected sum')
})
```

### Vitest

#### Vitest Methods

|Name| Methods|
|-----|-------|
|it()| Main method that will run a test|
|test()| Main method that will run a test|
|expect()| Asserts the test |
|toBe()| Checks for strict equality |
|toEqual()| Checks for deep equality |

#### Vitest Syntax

```js
export function validateNumber(number) {
  if (isNaN(number) || typeof number !== 'number') {
    throw new Error('Invalid number input.');
  }
}

it('should not throw an error, if a number is provided', () => {
  // Arrange
  const input = 1;

  // Act
  const validationFn = () => validateNumber(input);
  
  // Assert
  expect(validationFn).not.toThrow();
});
```
