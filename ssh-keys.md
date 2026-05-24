---
title: SSH Keys
---

# SSH Keys

SSH keys live under a server:
`$forge->server($id)->sshKeys()` (collection) and `$forge->server($id)->sshKey($keyId)` (item).

## List & get

```php
use PhpDevKits\ForgeSdk\Data\ListSshKeysOptions;

$page = $forge->server($serverId)->sshKeys()->all(new ListSshKeysOptions(user: 'forge'));
$key  = $forge->server($serverId)->sshKey($keyId)->get();   // SshKey DTO
```

`ListSshKeysOptions` parameters:

| Parameter | Query | Notes |
|-----------|-------|-------|
| `size` | `page[size]` | results per page |
| `cursor` | `page[cursor]` | pagination cursor |
| `name` | `filter[name]` | |
| `user` | `filter[user]` | |

## Create

```php
use PhpDevKits\ForgeSdk\Data\CreateSshKeyData;

$forge->server($serverId)->sshKeys()->create(new CreateSshKeyData(
    name: 'laptop',
    key: 'ssh-ed25519 AAAA... user@example.com',
    // user: 'forge' (default)
));
```

> Create returns `202` with an empty body (Forge installs the key asynchronously), so `create()` returns `void`. List afterwards to retrieve the new key.

## Delete

```php
$forge->server($serverId)->sshKey($keyId)->delete();
```

SSH keys have no update operation.
