# LLM Container Images

Container images for the OpenClaw homelab project. Built and pushed to `ghcr.io` via GitHub Actions.

## Images

| Image | Source | Purpose |
|-------|--------|---------|
| `ghcr.io/andrewthetechie/openclaw` | [openclaw/openclaw](https://github.com/openclaw/openclaw) | OpenClaw Gateway |

## Building Locally

```bash
# OpenClaw
cd images/openclaw
docker build -t ghcr.io/andrewthetechie/openclaw:latest .
docker push ghcr.io/andrewthetechie/openclaw:latest
```

## CI/CD

Images are automatically built and pushed on:
- Push to `main` (tags `latest` + git SHA)
- Git tags matching `v*` (tags the version)
- Manual dispatch (select which image to build)
