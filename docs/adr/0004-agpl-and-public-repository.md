# 0004: AGPL-3.0 and a public repository

## Status

Accepted

## Context

The project is intended to be self-hosted and forked. The license should keep
modified, network-hosted versions open to their users.

## Decision

- License: GNU AGPL-3.0-only (see `LICENSE`).
- The repository is public at <https://github.com/jmuise/library>.

## Consequences

- Anyone running a modified version as a network service must offer its source
  to that service's users.
- Some organizations avoid AGPL dependencies, which may limit adoption.
- Dependencies must be license-compatible with AGPL-3.0.
- Nothing secret may be committed; the repository is world-readable.
