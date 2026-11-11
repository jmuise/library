# 0003: Fork-your-own / self-hosting requirements

## Status

Accepted as requirements. The application stack and language are Deferred.

## Context

The point of the project is that anyone can run their own instance. That
shapes the application before any code is written.

## Decision

The application must meet these requirements:

- SQLite by default, with Postgres as an option.
- A first-run setup flow rather than hand-edited seed data.
- Configuration through environment variables.
- All telemetry is opt-in and off by default.
- Releases are managed by release-please and follow semver.
- Database migrations run automatically on startup.
- No multi-tenancy in the application. A hosting business, if there ever is
  one, runs one instance per tenant.

## Consequences

- The data layer must support both SQLite and Postgres.
- Startup migrations need to be safe to run on every boot.
- Operators get a simple default (one container, one file) with an upgrade
  path.

## Deferred / Open

- Application stack and language: not decided.
- None of the requirements above is implemented yet.
