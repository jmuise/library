# library

A self-hostable "lending library" web app: a place to keep track of items you
own and lend out to other people.

## Status

Early stage. There is no application yet. The repository currently ships only
a static "coming soon" page served by [Caddy](https://caddyserver.com/) in a
container image. The application stack has not been chosen. The design
direction is recorded in [docs/adr/](docs/adr/README.md), and those records
describe intent, not features that exist today.

## License

[AGPL-3.0-only](LICENSE).

## Develop

Open the repository in the devcontainer (`.devcontainer/`). It provides the
lint tools used for this repo (actionlint, zizmor, hadolint, shellcheck) and
Docker. There are no backing services to start.

The lint commands are available as VS Code tasks (Run Task): `actionlint`,
`zizmor .`, `hadolint Dockerfile` and `shellcheck`. The same commands can be
run from a terminal.

## More

- Architecture decisions: [docs/adr/](docs/adr/README.md)
- Reporting security issues: [SECURITY.md](SECURITY.md)
