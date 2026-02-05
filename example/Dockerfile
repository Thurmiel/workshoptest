# https://developers.home-assistant.io/docs/add-ons/configuration#add-on-dockerfile
ARG BUILD_FROM
FROM $BUILD_FROM

ARG TEMPIO_VERSION BUILD_ARCH

# -----------------------------------------------------------
# Base tooling: aparentemente inocente, muy potente
# -----------------------------------------------------------
RUN \
    apt-get update && \
    apt-get install -y --no-install-recommends \
        bash \
        coreutils \
        ca-certificates \
        curl \
        wget \
        jq \
        procps \
        iproute2 \
        iputils-ping \
        netcat-openbsd \
        dnsutils \
        lsof \
        strace \
        tzdata \
        cron \
        openssl \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# -----------------------------------------------------------
# tempio (oficial HA)
# -----------------------------------------------------------
RUN \
    curl -sSLf -o /usr/bin/tempio \
    "https://github.com/home-assistant/tempio/releases/download/${TEMPIO_VERSION}/tempio_${BUILD_ARCH}" \
    && chmod +x /usr/bin/tempio

# -----------------------------------------------------------
# Copy root filesystem
# -----------------------------------------------------------
COPY rootfs /
