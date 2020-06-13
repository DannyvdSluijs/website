---
title: Livewire Plugin
description: The Livewire Plugin
extends: _layouts.documentation
section: content
---

# Livewire Plugin

The Livewire Plugin for Pest provides additional functions for testing your [Livewire](https://laravel-livewire.com/) components.

**Source code**: [github.com/pestphp/pest-plugin-livewire](https://github.com/pestphp/pest-plugin-livewire)

Install the plugin using Composer:
```bash
composer require pestphp/pest-plugin-livewire --dev
```

### `livewire()`

You may use the `livewire` function to test your Livewire components:
```php
use function Pest\Livewire\livewire;

it('can be incremented', function () {
    livewire(Counter::class)
        ->call('increment')
        ->assertSee(1);
});

it('can be decremented', function () {
    livewire(Counter::class)
        ->call('decrement')
        ->assertSee(-1);
});
```

Of course, the method `livewire()` can be equaly used in your higher order tests:
```php
it('can be incremented')
    ->livewire(Counter::class)
    ->call('increment')
    ->assertSee(1);

it('can be decremented')
    ->livewire(Counter::class)
    ->call('decrement')
    ->assertSee(-1);
```

Finally, for the full list of available Livewire methods, please refer to the [Livewire documentation](https://laravel-livewire.com/docs/testing).

Next section: [Faker →](/docs/plugins/faker)
