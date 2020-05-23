---
title: Writing Tests
description: Writing Tests
extends: _layouts.documentation
section: content
---

# Writing Tests

Pest makes it easy to write tests. This section illustrates how to write
a simple test suite with Pest, and what are the conventions you should use.

The setup is very simple, and usually looks like this:

```php
tests
    - Unit/ComponentTest.php <--
    - Feature/HomeTest.php <--
phpunit.xml
```

To write a test, create a file in the `Unit` or `Feature` directory,
and make sure its filename ends with the `..Test.php` suffix.
Then, all you need inside this file is a function which runs your test:

```
<?php
test('has home', function () {
    // ..
});

// or
it('has home', function () {
    // ..
});
```

> **Note**: Pest will only run a test file if its name ends with the suffix set in your `phpunit.xml`.


Now, on to the API reference. Pest offers you two functions to write your tests: `test()` & `it()`.
Use the one that best fits your test naming convention, or both. They share the same behavior & syntax:

### `test()`

The `test` function adds the given closure as test. The first argument is the test
description; the second argument is a closure that contains the test expectations:

```php
test('asserts true is true', function () {
    assertTrue(true);
});
```

> **Note**: The assertion above could also be written with `$this->assertTrue(true);`, because the test
closure is bound to the underlying TestCase class.

Here is what this example test will return:
```bash
✓ asserts true is true
```

### `it()`

The `it` function adds the given closure as test. The first argument is the test
description; the second argument is a closure that contains the test expectations:

```php
it('asserts true is true', function () {
    assertTrue(true);
});
```

Here is what this example test will return:
```bash
✓ it asserts true is true
```

> **Note**: Notice how, when using the `it` function, your test name gets prepended with 'it' in the 
returned description.

Next section: [Underlying Test Case →](/docs/underlying-test-case)
