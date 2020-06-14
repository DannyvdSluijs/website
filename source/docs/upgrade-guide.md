---
title: Upgrade Guide
description: Upgrade Guide
extends: _layouts.documentation
section: content
---

# Upgrade Guide

## Upgrading To 0.2 From 0.1

#### Estimated Upgrade Time: 1-4 Minutes

> We attempt to document every possible breaking change. Since some of these breaking changes are in obscure parts of the framework only a portion of these changes may actually affect your application.

### Updating Dependencies

Update your `pestphp/pest` dependency to `^0.2` in your `composer.json` file.

If you are using any official plugins, update them to `^0.2` in your `composer.json` file.

#### Namespaced Functions

Any functions provided by official plugins are no longer available globally. Therefore, you need to
import them. Here is an example:

```
use function Pest\Laravel\get;

get('/')->assertSee('Laravel');
```

Next section: [Community →](/docs/community)
