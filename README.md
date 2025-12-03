# DevRunner

Base container images for MonkeyCode-AI developer workflows.

## Base image (bookworm)
- Dockerfile: `docker/base/bookworm/Dockerfile` (Debian bookworm-slim, git/curl/build-essential/python3, en_US.UTF-8 locale, default user root).
- Build locally: `STACK=base VERSION=bookworm ./scripts/build.sh`
- Push to GHCR: `PUSH=true REGISTRY=ghcr.io/monkeycode-ai/devrunner STACK=base VERSION=bookworm ./scripts/build.sh` (needs `docker login ghcr.io`).
- Run: `docker run --rm -it ghcr.io/monkeycode-ai/devrunner/base:bookworm bash`

## Layout
- `docker/base/bookworm/Dockerfile` – base image definition.
- `scripts/build.sh` – helper to build/push images (env-driven: STACK, VERSION, REGISTRY, PUSH).
- `docs/` – docs placeholder for future stacks and CI notes.

## CI/CD
- Workflow: `.github/workflows/ci.yaml`
  - PR: build only (no push).
  - Push to `main` or any git tag: login to GHCR with `GITHUB_TOKEN` and push `ghcr.io/monkeycode-ai/devrunner/base:bookworm` (+ `latest` on main, branch-suffixed tags on non-main, git tag as image tag on releases, and `bookworm-<git-tag>` for releases).
- No personal access token needed; workflow requests `packages: write` and `contents: read` via `GITHUB_TOKEN` (ensure Actions permissions allow this if repository is restricted).
