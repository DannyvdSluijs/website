---
title: Creating Plugins
description: The Plugin Creation Guide
extends: _layouts.documentation
section: content
---

# Plugins

Plugins can extend Pest's functionality and are easy to write.

## Setting up the plugin template

To create your own plugin, it's easiest to start with the [pest-plugin-template](https://github.com/pestphp/pest-plugin-template).
To create a new repository from it click on the "Use this template" button. Call it `pest-plugin-<yourpluginname>`.

Now clone the new repository to work on your plugin.

Edit _composer.json_ and adjust the fields `name` and `description` to fit your plugin.

## Adding functions

A plugin may integrate additional functions. These may be even external
libraries. A good example is the [Faker plugin](https://github.com/pestphp/pest-plugin-faker),
which we are going to reproduce here.

If your plugin has any dependencies, then add them first with `composer`:

```bash
composer require fzaninotto/faker
```

All plugin files reside inside the `src/` folder. Let's assume we want to provide a `faker()` function
for our tests. We create a file `Autoload.php` with the following code:

```php
<?php

namespace YourGithubUsername\PestPluginName;

use Faker\Factory;
use Faker\Generator;

function faker(string $locale = null): Generator
{
    return Factory::create($locale ?? Factory::DEFAULT_LOCALE);
}
```

This provides a `faker()` function inside our tests, by creating an object and returning it.
Now we can use `Faker` directly and easily in our tests:

```php
use function YourGithubUsername\PestPluginName\faker;

it('doing something with faker', function() ) {
    assertIsString(faker()->name);
}
```

Before our `faker()` function is available though, we must ensure it is always loaded.

To load our extension, we must add or adjust the `autoload` section in the `composer.json` to point to the files we want to load:

```json
...
    "autoload": {
        "files": ["src/Autoload.php"]
    },
...
```

Any amount of further files can be added this way. They will be loaded automatically.

## Adding methods to `$this` / Higher Order methods

Methods can also be added for the `$this` variable inside closures, or to higher order tests.

We are going to add a trait and tell Pest to use it. We are going to call our imaginery plugin `MyPlugin`.
For writing your own plugin, choice your own name and make sure you adjust all occurences in the following code.

First, let's create our trait directly in `src/MyPluginTrait.php`:

```php
<?php

namespace YourGithubUsername\PestPluginName;

trait MyPluginTrait
{
    public function visitHomePage()
    {
        return $this->get('/')
                    ->assertSee('Welcome');
    }

    public function actOnProfilePage()
    {
        $user = factory(User::class)->create();

        return $this->actingAs($user)->get('/profile')->assertSee($user->name);
    }
}
```

Make sure, you always return the object, so high order functionality isn't broken.

This will allow us to write tests like this:

```php
it('has a homepage')
    ->visitHomePage()
    ->actOnProfilePage()
    ->assertTrue(true);
```

To make the functionality available, we need to let Pest know that it should use it. We will do this in `Autoload.php`:

```php
use YourGithubUsername\PestPluginName\MyPluginTrait;

Pest\Plugin::uses(MyPluginTrait::class);
```

And finally, we have to load the files. We adjust our `composer.json` as following, to reference our namespace
for PSR-4 autoloading and tell it to load and run our `Plugin.php`:

```json
    ...
    "autoload": {
        "psr-4": {
            "YourGithubUsername\\PestPluginName\\": "src/"
        },
        "files": ["src/Autoload.php"]
    },
    ...
```

## Tips & Tricks

Inside functions, to access the current test case in your plugin, you can use the global function `test()`.

In traits, `$this` is also available to access the current test case.

Next section: [Laravel Plugin →](/docs/plugins/laravel)
