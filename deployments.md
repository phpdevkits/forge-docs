---
title: Deployments
---

# Deployments

Deployments live under a site:
`$forge->server($s)->site($id)->deployments()` / `->deployment($d)`, plus the
`deploy()` sugar and the deployment script.

## List & get

```php
use PhpDevKits\ForgeSdk\Data\ListDeploymentsOptions;

$site = $forge->server($serverId)->site($siteId);

$page = $site->deployments()->all(new ListDeploymentsOptions(commitAuthor: 'jane'));
$deployment = $site->deployment($deploymentId)->get();   // Deployment DTO (nested commit)
```

`ListDeploymentsOptions` parameters:

| Parameter | Query | Notes |
|-----------|-------|-------|
| `size` | `page[size]` | results per page |
| `cursor` | `page[cursor]` | pagination cursor |
| `sort` | `sort` | e.g. `-created_at` |
| `commitHash` | `filter[commit_hash]` | |
| `commitMessage` | `filter[commit_message]` | |
| `commitAuthor` | `filter[commit_author]` | |

## Trigger a deploy

```php
$deployment = $forge->server($serverId)->site($siteId)->deploy();   // sugar for deployments()->trigger()
```

A `Deployment`'s `startedAt` / `endedAt` are nullable while it's queued.

## Deployment script

```php
use PhpDevKits\ForgeSdk\Data\UpdateDeploymentScriptData;

$script = $site->deploymentScript()->get();   // DeploymentScript DTO

$site->deploymentScript()->update(new UpdateDeploymentScriptData(
    content: "cd /home/forge/app\ngit pull origin main\ncomposer install --no-dev",
));
```

> A `php`-type site pointed at a Laravel repo needs `composer install` *before* `artisan` in its deploy script, or the first deploy fails on a missing `vendor/autoload.php`.
