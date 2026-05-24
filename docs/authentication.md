---
title: Authentication
---

# Authentication

Grab a personal access token from your [Forge API settings](https://forge.laravel.com/user-profile/api), then build a `Forge` client one of three ways.

### 1. Explicit

```php
use PhpDevKits\ForgeSdk\Forge;

$forge = new Forge(token: 'your-forge-api-token', organization: 'acme');
```

### 2. From environment variables

Reads `FORGE_TOKEN` and the optional `FORGE_ORGANIZATION`:

```php
$forge = Forge::fromEnvironment();
```

### 3. From a JSON config file

Reads `./forge.json`, or `$FORGE_CONFIG_PATH`, or an explicit path:

```php
$forge = Forge::fromConfig();
$forge = Forge::fromConfig('/path/to/forge.json');
```

```json
{
    "token": "your-forge-api-token",
    "organization": "acme"
}
```

## Organization context

Org-scoped resources read the organization from the constructor, env, or config. Switch context per call with an immutable clone — the original client is untouched:

```php
$servers = $forge->org('another-org')->servers()->all();
```

Calling an org-scoped resource with no organization bound throws `OrganizationNotSetException`.

## Errors

Every non-2xx response throws a typed exception. All extend `ForgeException`, so you can catch the whole family or a specific case:

| Status | Exception | Notes |
|--------|-----------|-------|
| 400 | `BadRequestException` | |
| 401 | `UnauthorizedException` | bad or missing token |
| 403 | `ForbiddenException` | token lacks the required scope |
| 404 | `NotFoundException` | |
| 422 | `ValidationException` | exposes `->errors()` (field → messages) |
| 429 | `RateLimitException` | |
| 5xx | `ServerException` | |
| — | `ConnectionException` | network-layer failure (no HTTP response) |
| — | `OrganizationNotSetException` | client-side guard — thrown before any request when an org-scoped call has no organization bound |

```php
use PhpDevKits\ForgeSdk\Exceptions\{ForgeException, ValidationException};

try {
    $forge->servers()->create($data);
} catch (ValidationException $e) {
    $messages = $e->errors();        // ['name' => ['The name field is required.'], ...]
} catch (ForgeException $e) {
    report($e);                      // any other Forge error
}
```
