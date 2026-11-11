# Stack-agnostic toolbox for building, linting and shipping the container image.
# No language runtime is installed on purpose: the app stack is undecided.

# Pinned by tag and digest (index digest, multi-arch; resolved 2026-11-10). Refresh with:
#   docker buildx imagetools inspect mcr.microsoft.com/devcontainers/base:bookworm
FROM mcr.microsoft.com/devcontainers/base:bookworm@sha256:99f5250dd2d5f8b11046d5662dd94c1e3c1dae56b560c7caad5ceb7edf349046

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Proving apt works in our own layer makes an expired third-party signing key fail
# here rather than opaquely inside a devcontainer feature install.
RUN apt-get update \
    && export DEBIAN_FRONTEND=noninteractive \
    && apt-get install -y --no-install-recommends \
        ca-certificates \
        curl \
        git \
        jq \
        less \
        make \
        xz-utils \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Pinned, checksum-verified release binaries (not curl|sh). Hashes come from each
# release's checksums file / GitHub asset digests; bump version and both hashes together.
#   actionlint 1.7.12   zizmor 1.32.0   hadolint 2.15.1   shellcheck 0.11.0   flyctl 0.5.7
RUN set -eux; \
    arch="$(dpkg --print-architecture)"; \
    fetch() { curl --fail --silent --show-error --location --output "$1" "$2"; echo "$3  $1" | sha256sum --check --strict; }; \
    cd /tmp; \
    case "$arch" in \
        amd64) al=8aca8db96f1b94770f1b0d72b6dddcb1ebb8123cb3712530b08cc387b349a3d8; alarch=amd64; \
               zz=bb110cc765eca9cd7efb2e284a956be5974c60027b0970f16397d86208a9957b; zarch=x86_64; \
               hl=c7187db94eeeeca956519a6af171adc31453941a1e777961f6e680f697c8c507; harch=x86_64; \
               sc=b7af85e41cc99489dcc21d66c6d5f3685138f06d34651e6d34b42ec6d54fe6f6; scarch=x86_64; \
               fl=1b340c3f326abc34659b7bc1f2eed69eac31115c9796e325596e27687ea421a5; flarch=x86_64 ;; \
        arm64) al=325e971b6ba9bfa504672e29be93c24981eeb1c07576d730e9f7c8805afff0c6; alarch=arm64; \
               zz=cb152b207f815116d7fa1c1ee4614f61578803351395052908095e79adfd9fd0; zarch=aarch64; \
               hl=f6198ef8090f404dbb771abfee086eb8c48ac177f30da7fd3510aca35b344b5d; harch=arm64; \
               sc=68a8133197a50beb8803f8d42f9908d1af1c5540d4bb05fdfca8c1fa47decefc; scarch=aarch64; \
               fl=01cb77e62cc00b61f20c31a070c462b61733dd7d6a2c5aee7e5287d41b4ec1c8; flarch=arm64 ;; \
        *) echo "unsupported architecture: $arch" >&2; exit 1 ;; \
    esac; \
    fetch al.tgz "https://github.com/rhysd/actionlint/releases/download/v1.7.12/actionlint_1.7.12_linux_${alarch}.tar.gz" "$al"; \
    tar -xzf al.tgz -C /usr/local/bin actionlint; \
    fetch zz.tgz "https://github.com/zizmorcore/zizmor/releases/download/v1.32.0/zizmor-${zarch}-unknown-linux-gnu.tar.gz" "$zz"; \
    tar -xzf zz.tgz -C /usr/local/bin zizmor; \
    fetch /usr/local/bin/hadolint "https://github.com/hadolint/hadolint/releases/download/v2.15.1/hadolint-linux-${harch}" "$hl"; \
    chmod 0755 /usr/local/bin/hadolint; \
    fetch sc.tgz "https://github.com/koalaman/shellcheck/releases/download/v0.11.0/shellcheck-v0.11.0.linux.${scarch}.tar.gz" "$sc"; \
    tar -xzf sc.tgz --strip-components=1 -C /usr/local/bin shellcheck-v0.11.0/shellcheck; \
    fetch fl.tgz "https://github.com/superfly/flyctl/releases/download/v0.5.7/flyctl_0.5.7_Linux_${flarch}.tar.gz" "$fl"; \
    tar -xzf fl.tgz -C /usr/local/bin flyctl; \
    ln -s flyctl /usr/local/bin/fly; \
    rm -f /tmp/*.tgz; \
    actionlint -version; zizmor --version; hadolint --version; shellcheck --version; flyctl version

# Nothing user-specific is baked in: no git identity, no tokens, no shell config.
USER vscode

# Claude Code CLI 2.1.322, pinned and checksum-verified per architecture (not curl|sh).
# This mirrors what install.sh does, minus its "latest" lookup: fetch the versioned binary from
# the release bucket, verify its sha256 (the values below were computed from the downloaded
# artifacts and match the release manifest.json), then let the binary set up ~/.local/bin/claude.
# Bump version and both hashes together. DISABLE_AUTOUPDATER stops the CLI replacing itself
# so the pin holds. The dotfiles install no longer provides packages in a devcontainer
# (dotfiles #41), so the image does.
ENV DISABLE_AUTOUPDATER=1
RUN set -eux; \
    case "$(dpkg --print-architecture)" in \
        amd64) cc=bfb3703ad5380242328459bc62ada4bde8ae7ba57c6e1eeadb9c00e08874c478; ccarch=linux-x64 ;; \
        arm64) cc=f220ca50ea4f3222043ff83cc224a1262e0196e9897acc0871efa94235b0c1fd; ccarch=linux-arm64 ;; \
        *) echo "unsupported architecture" >&2; exit 1 ;; \
    esac; \
    curl --fail --silent --show-error --location --output /tmp/claude "https://downloads.claude.ai/claude-code-releases/2.1.322/${ccarch}/claude"; \
    echo "${cc}  /tmp/claude" | sha256sum --check --strict; \
    chmod 0755 /tmp/claude; \
    /tmp/claude install 2.1.322; \
    rm -f /tmp/claude; \
    /home/vscode/.local/bin/claude --version
ENV PATH="/home/vscode/.local/bin:${PATH}"
