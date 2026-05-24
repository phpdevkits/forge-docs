---
title: Daemons
---

# Daemons

Daemons (Forge's "background processes") live under a server:
`$forge->server($id)->daemons()` (collection) and `$forge->server($id)->daemon($daemonId)` (item).

## List & get

```php
use PhpDevKits\ForgeSdk\Data\ListDaemonsOptions;

$page   = $forge->server($serverId)->daemons()->all(new ListDaemonsOptions(user: 'forge'));
$daemon = $forge->server($serverId)->daemon($daemonId)->get();   // Daemon DTO
```

`ListDaemonsOptions` parameters:

| Parameter | Query | Notes |
|-----------|-------|-------|
| `size` | `page[size]` | results per page |
| `cursor` | `page[cursor]` | pagination cursor |
| `sort` | `sort` | e.g. `-created_at` |
| `user` | `filter[user]` | |
| `siteId` | `filter[site_id]` | |
| `directory` | `filter[directory]` | |

## Create

```php
use PhpDevKits\ForgeSdk\Data\CreateDaemonData;
use PhpDevKits\ForgeSdk\Enums\DaemonUser;

$daemon = $forge->server($serverId)->daemons()->create(new CreateDaemonData(
    name: 'queue-worker',
    command: 'php artisan queue:work',
    user: DaemonUser::Forge,
    processes: 1,
));
```

## Update, actions & log

```php
use PhpDevKits\ForgeSdk\Data\UpdateDaemonData;

$daemon = $forge->server($serverId)->daemon($daemonId);

$daemon->restart();
$daemon->stop();
$daemon->start();
$daemon->emptyLog();

echo $daemon->log();   // raw supervisor log content

// Update requires `config` (the raw supervisor config) — name-only 500s.
$daemon->update(new UpdateDaemonData(name: 'queue-worker', config: $supervisorConfig));
```

> A freshly-created daemon is `installing` then `installed` (never `running`); actions 500 until it's `installed`.
