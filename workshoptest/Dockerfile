# https://developers.home-assistant.io/docs/add-ons/configuration#add-on-dockerfile
ARG BUILD_FROM
FROM $BUILD_FROM

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
        util-linux \
        nano \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# -----------------------------------------------------------
# Copy root filesystem
# -----------------------------------------------------------
COPY rootfs /
# Dar permisos de ejecución a todos los scripts de s6
RUN chmod a+x \
    /etc/services.d/example/run \
    /etc/services.d/example/finish \
    /usr/bin/my_program