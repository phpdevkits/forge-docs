---
title: Servers
---

# Servers

`$forge->servers()` (collection) and `$forge->server($id)` (item) manage servers within the bound organization.

## List

```php
use PhpDevKits\ForgeSdk\Data\ListServersOptions;

$page = $forge->servers()->all(new ListServersOptions(size: 25, provider: 'hetzner'));

foreach ($forge->servers()->iterate() as $server) {
    echo $server->name.PHP_EOL;
}
```

`ListServersOptions` parameters:

| Parameter | Query | Notes |
|-----------|-------|-------|
| `size` | `page[size]` | results per page |
| `cursor` | `page[cursor]` | pagination cursor |
| `sort` | `sort` | e.g. `-created_at` |
| `ipAddress` | `filter[ip_address]` | |
| `name` | `filter[name]` | |
| `region` | `filter[region]` | |
| `sizeFilter` | `filter[size]` | named `sizeFilter` to avoid clashing with `size` (pagination) |
| `provider` | `filter[provider]` | |
| `ubuntuVersion` | `filter[ubuntu_version]` | |
| `phpVersion` | `filter[php_version]` | |
| `databaseType` | `filter[database_type]` | |

## Get

```php
$server = $forge->server($id)->get();   // Server DTO
```

## Create

The create payload is provider-polymorphic — pass the matching provider config:

```php
use PhpDevKits\ForgeSdk\Data\{CreateServerData, HetznerServerConfig};
use PhpDevKits\ForgeSdk\Enums\{PhpVersion, ServerType, UbuntuVersion};

$server = $forge->servers()->create(new CreateServerData(
    name: 'web-1',
    provider: 'hetzner',
    type: ServerType::App,
    ubuntuVersion: UbuntuVersion::Ubuntu2404,
    phpVersion: PhpVersion::Php84,
    hetzner: new HetznerServerConfig(regionId: 'fsn1', sizeId: 'cax11', networkId: 12345),
));
```

> **Hetzner note:** `sizeId` must be the size *code* (e.g. `cax11`), not the numeric id; `networkId` is required despite being optional in the spec. `regionId` accepts the Forge id, code, or alt.

## Update / delete / actions

```php
use PhpDevKits\ForgeSdk\Data\UpdateServerData;

$forge->server($id)->update(new UpdateServerData(name: 'web-1-renamed'));
$forge->server($id)->reboot();
$forge->server($id)->powerCycle();
$forge->server($id)->delete();
```
