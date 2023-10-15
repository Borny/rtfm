# Test

- [Guidelines](#guidelines)

TAP => Test Anything Protocol

## Guidelines

Every new functions should be tested in a related file.  
A test file usually has the same name as the file with the functions it should test but ending with **.test.js** (e.g: some-file-name.test.js).

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
