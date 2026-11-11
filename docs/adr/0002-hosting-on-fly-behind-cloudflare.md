# 0002: Hosting on Fly.io behind Cloudflare

## Status

Accepted

## Context

The upstream instance needs low-effort hosting with TLS and basic edge
protection, without locking the application to one provider.

## Decision

- Run the image on Fly.io, behind Cloudflare.
- Cloudflare TLS mode is Full (strict), so traffic is encrypted and the origin
  certificate is validated.
- The application takes no Fly-specific dependencies, so it stays portable to
  any host that can run an OCI image.
- Deploys use an app-scoped, expiring Fly deploy token stored as a secret on
  the `production` GitHub environment.

## Consequences

- Moving to another host means changing deployment config, not application code.
- The deploy token must be rotated before it expires.
- Using a scoped token limits the damage if it leaks.

## Deferred / Open

- Hosting is configured outside this repository and the deploy workflow does
  not exist yet.
