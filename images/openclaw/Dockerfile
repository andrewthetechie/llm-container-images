FROM node:22-slim AS base

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    jq \
    && rm -rf /var/lib/apt/lists/*

# Clone OpenClaw at build time for reproducibility
ARG OPENCLAW_VERSION=main
RUN git clone --depth 1 --branch ${OPENCLAW_VERSION} \
    https://github.com/openclaw/openclaw.git /build

WORKDIR /build

# Install pnpm
RUN corepack enable && corepack prepare pnpm@latest --activate

# Install dependencies and build
RUN pnpm install --frozen-lockfile || pnpm install
RUN pnpm ui:build || true
RUN pnpm build

# --- Runtime stage ---
FROM node:22-slim

RUN apt-get update && apt-get install -y \
    git \
    curl \
    jq \
    && rm -rf /var/lib/apt/lists/*

RUN corepack enable && corepack prepare pnpm@latest --activate

COPY --from=base /build /app
WORKDIR /app

# Set ownership
RUN chown -R 1000:1000 /app

USER 1000

# Default config and workspace directories
ENV HOME=/home/openclaw
RUN mkdir -p /home/openclaw/.openclaw/workspace

EXPOSE 18789

CMD ["pnpm", "openclaw", "gateway", "--port", "18789", "--host", "0.0.0.0"]
