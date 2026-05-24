---
title: Sites
---

# Sites

Sites live under a server: `$forge->server($id)->sites()` (collection) and
`$forge->server($id)->site($siteId)` (item).

## List & get

```php
use PhpDevKits\ForgeSdk\Data\ListSitesOptions;

$page = $forge->server($serverId)->sites()->all(new ListSitesOptions(name: 'example.com'));

$site = $forge->server($serverId)->site($siteId)->get();   // Site DTO
```

A `Site` carries nested `repository` and `maintenanceMode` value objects.

`ListSitesOptions` parameters:

| Parameter | Query | Notes |
|-----------|-------|-------|
| `size` | `page[size]` | results per page |
| `cursor` | `page[cursor]` | pagination cursor |
| `sort` | `sort` | e.g. `-created_at` |
| `name` | `filter[name]` | |

## Create

```php
use PhpDevKits\ForgeSdk\Data\CreateSiteData;
use PhpDevKits\ForgeSdk\Enums\SiteType;

$site = $forge->server($serverId)->sites()->create(new CreateSiteData(
    type: SiteType::Laravel,
    name: 'app',
    domainMode: 'on-forge',
));
```

> `domain_mode` is required despite being optional in the spec; on-forge sites take a single dotless subdomain `name`.

## Update / delete

```php
use PhpDevKits\ForgeSdk\Data\UpdateSiteData;

// PUT returns 202 with an empty body, so update() is void — call get() to read new state.
$forge->server($serverId)->site($siteId)->update(new UpdateSiteData(directory: '/public'));
$forge->server($serverId)->site($siteId)->delete();
```

Continue to [Deployments](deployments) to ship code to a site.
