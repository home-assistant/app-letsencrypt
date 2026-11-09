# Base image updated by Renovate, update versionCompatibility on Debian base bump
ARG BUILD_FROM=ghcr.io/home-assistant/base-debian:trixie-2026.10.3@sha256:a9fb29d0ffab1a81d6379e6252db21e0edbd1263c94d9de2cfc54a396aad8611
FROM $BUILD_FROM

# Allow pip to install packages system-wide on Debian
ENV PIP_BREAK_SYSTEM_PACKAGES=1

# setup base
# certbot-dns-multi replaces most individual DNS plugins using lego
# Only legacy plugins are kept for providers not supported by lego: gehirn, eurodns, noris
# Azure and HE is also kept as legacy for backward compatibility (will be removed in future version)
ARG \
    ACME_VERSION=3.3.0 \
    CERTBOT_DNS_AZURE_VERSION=2.6.1 \
    CERTBOT_DNS_EURODNS_VERSION=1.8.2 \
    CERTBOT_DNS_HURRICANE_ELECTRIC_VERSION=0.1.0 \
    CERTBOT_DNS_MULTI_VERSION=5.3.1 \
    CERTBOT_DNS_NORISNETWORK_VERSION=0.3.0 \
    CERTBOT_VERSION=3.3.0 \
    CRYPTOGRAPHY_VERSION=44.0.2

RUN \
    set -x \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
        build-essential \
        ca-certificates \
        cargo \
        golang \
        libffi8 \
        libffi-dev \
        libssl-dev \
        openssl \
        python3 \
        python3-dev \
        python3-pip \
        rustc \
    && pip3 install --no-cache-dir \
        acme==${ACME_VERSION} \
        certbot==${CERTBOT_VERSION} \
        certbot-dns-azure==${CERTBOT_DNS_AZURE_VERSION} \
        certbot-dns-eurodns==${CERTBOT_DNS_EURODNS_VERSION} \
        certbot-dns-gehirn==${CERTBOT_VERSION} \
        certbot-dns-hurricane-electric==${CERTBOT_DNS_HURRICANE_ELECTRIC_VERSION} \
        certbot-dns-multi==${CERTBOT_DNS_MULTI_VERSION} \
        certbot-dns-norisnetwork==${CERTBOT_DNS_NORISNETWORK_VERSION} \
        cryptography==${CRYPTOGRAPHY_VERSION} \
    && apt-get purge -y --auto-remove \
        build-essential \
        cargo \
        golang \
        libffi-dev \
        libssl-dev \
        python3-dev \
        python3-pip \
        rustc \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /root/.cache

# Copy data
COPY rootfs /

LABEL \
    org.opencontainers.image.title="Home Assistant App: Let's Encrypt" \
    org.opencontainers.image.description="Manage certificate from Let's Encrypt" \
    org.opencontainers.image.source="https://github.com/home-assistant/app-letsencrypt" \
    org.opencontainers.image.licenses="Apache License 2.0"
