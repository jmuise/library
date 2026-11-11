# library

Self-hostable "lending library" web app (AGPL-3.0, github.com/jmuise/library). Currently only a static "coming soon" page served by Caddy in a container image. The image is the distribution artifact: multi-arch (amd64/arm64), published to GHCR, signed with cosign, deployed to Fly.io behind Cloudflare at library.muise.org. App stack is undecided; no language runtime is installed yet.

## Develop

- Open in the devcontainer (Debian bookworm, `.devcontainer/Dockerfile`). No backing services.
- Pinned tools: actionlint 1.7.12, zizmor 1.32.0, hadolint 2.15.1, shellcheck 0.11.0, flyctl 0.5.7 (also `fly`), jq, gh, Claude Code CLI. Binaries are checksum-verified per architecture; bump version and both hashes together.
- Docker is docker-in-docker (devcontainer feature), not the host socket: images built or run inside stay inside, and published ports are reachable on the container's localhost. Multi-arch builds need QEMU: `docker run --privileged --rm tonistiigi/binfmt --install arm64` once per container, then `docker buildx build --platform linux/amd64,linux/arm64 .`.
- VS Code tasks in `.vscode/tasks.json` mirror the lint commands: `actionlint`, `zizmor .`, `hadolint Dockerfile`, `shellcheck`.
- Port 8080 (the Caddy container port) is forwarded.

## Manual steps

GitHub repo creation, Fly.io, Cloudflare and DNS configuration are the Captain's manual steps. Agents never perform them.
