---
title: Introduction
---

# Forge SDK

**Forge SDK** is an ultra-strict, type-safe PHP client for the [Laravel Forge API](https://forge.laravel.com/api-documentation), built on [Saloon v3](https://docs.saloon.dev/). It is engineered for developers who want to automate their Forge infrastructure with the same rigor they apply to their application code — fully typed, immutable, and fail-fast.

## Why this SDK?

The Forge API is JSON:API with cursor pagination, async endpoints, and a few places where the documented schema and the live behavior disagree. This SDK absorbs all of that so you don't have to:

- **100% type coverage** — every method, property, and parameter is explicitly typed. No `mixed`, no array soup.
- **Immutable, hydrated DTOs** — responses become `final readonly` objects (`Server`, `Site`, `Deployment`, …) with typed fields and `DateTimeImmutable` dates, not loose arrays.
- **Fail-fast, typed exceptions** — every non-2xx response throws a specific exception (`ValidationException`, `NotFoundException`, `RateLimitException`, …) so errors surface at the call site.
- **Pagination that gets out of your way** — cursor pagination is wrapped in `Page<T>` with a lazy `iterate()` that walks every page.
- **Framework-agnostic** — no Laravel required. It's plain Saloon; drop it into any PHP 8.4+ project.

## Requirements

- PHP 8.4+

## Next steps

- [Installation](installation) — add the package via Composer.
- [Authentication](authentication) — build a `Forge` client.
- Then jump into [Servers](servers), [Sites](sites), [Deployments](deployments), [SSH Keys](ssh-keys), or [Daemons](daemons).
