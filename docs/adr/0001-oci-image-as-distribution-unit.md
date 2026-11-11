# 0001: OCI container image as the distribution unit

## Status

Accepted

## Context

The project is meant to be self-hosted by people with different machines and
hosting setups. The application stack is not chosen yet, so the distribution
format must not depend on it.

## Decision

The distributable artifact is an OCI container image.

- Multi-arch: linux/amd64 and linux/arm64.
- Published to GitHub Container Registry (GHCR).
- Signed with cosign using keyless signing.
- Published with SBOM and provenance attestations.
- Base images are pinned by digest.

## Consequences

- Anyone can run the same artifact that is deployed upstream.
- Consumers can verify signature and provenance before running an image.
- Digest pins need regular refreshing; see ADR 0005 for the update tooling.
- Multi-arch builds need emulation or native runners in CI.

## Deferred / Open

- The build and publish pipeline is not implemented yet; this ADR records the
  target. Today the repository contains only the devcontainer definition.
