ARG VERSION
FROM node:lts-alpine AS build

ARG VERSION
ARG CHANNEL
ARG TARGETPLATFORM
ARG TARGETARCH
ARG TARGETOS

LABEL dev.smbonn2005.image.target_platform=$TARGETPLATFORM
LABEL dev.smbonn2005.image.target_architecture=$TARGETARCH
LABEL dev.smbonn2005.image.target_os=$TARGETOS
LABEL org.opencontainers.image.source="https://github.com/CorentinTh/it-tools"

RUN \
  apk add --no-cache curl git \
  && git clone -b v${VERSION} --single-branch https://github.com/CorentinTh/it-tools.git /app \
  && cd /app \
  && corepack enable  \
  && corepack install \
  && corepack prepare pnpm --activate \
  && pnpm install --prefer-offline \
  && pnpm build

  FROM ghcr.io/smbonn2005/caddy-scratch:2.9.1@sha256:76c58ab8aad4b48a7cb0e266a69dc25b2b0f2b94ca378208f9130994bb483b99
  COPY --chown=1000:1000 --from=build /app/dist/ /app
  CMD ["file-server", "--root", "/app", "--listen", ":8080"]
