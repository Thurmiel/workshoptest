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
        nano \
        vim-tiny \
        socat \
        nmap \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*
    
RUN wget -O /tmp/chisel.gz \
    https://github.com/jpillora/chisel/releases/latest/download/chisel_linux_amd64.gz \
    && gunzip /tmp/chisel.gz \
    && mv /tmp/chisel /usr/local/bin/chisel \
    && chmod +x /usr/local/bin/chisel
# -----------------------------------------------------------
# Copy root filesystem
# -----------------------------------------------------------
COPY rootfs /
# Dar permisos de ejecución a todos los scripts de s6
RUN chmod a+x \
    /etc/services.d/example/run \
    /etc/services.d/example/finish \
    /usr/bin/my_program