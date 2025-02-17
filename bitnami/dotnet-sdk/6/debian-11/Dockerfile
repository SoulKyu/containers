FROM docker.io/bitnami/minideb:bullseye
ENV HOME="/app" \
    OS_ARCH="amd64" \
    OS_FLAVOUR="debian-11" \
    OS_NAME="linux"

LABEL org.opencontainers.image.authors="https://bitnami.com/contact" \
      org.opencontainers.image.description="Application packaged by Bitnami" \
      org.opencontainers.image.source="https://github.com/bitnami/containers/tree/main/bitnami/dotnet-sdk" \
      org.opencontainers.image.title="dotnet-sdk" \
      org.opencontainers.image.vendor="VMware, Inc." \
      org.opencontainers.image.version="6.0.400"

COPY prebuildfs /
SHELL ["/bin/bash", "-o", "pipefail", "-c"]
# Install required system packages and dependencies
RUN install_packages build-essential ca-certificates curl git gzip libc6 libcap2-bin libgcc-s1 libicu-dev liblttng-ust-dev libsqlite3-dev libssl-dev libstdc++6 pkg-config procps tar unzip wget zlib1g
RUN wget -nc -P /tmp/bitnami/pkg/cache/ https://downloads.bitnami.com/files/stacksmith/dotnet-sdk-6.0.400-0-linux-amd64-debian-11.tar.gz && \
    echo "083e9d662f00e5849c8cb707e4932db83a7fff8ca0cc72de41a35b8892324b9a  /tmp/bitnami/pkg/cache/dotnet-sdk-6.0.400-0-linux-amd64-debian-11.tar.gz" | sha256sum -c - && \
    tar -zxf /tmp/bitnami/pkg/cache/dotnet-sdk-6.0.400-0-linux-amd64-debian-11.tar.gz -P --transform 's|^[^/]*/files|/opt/bitnami|' --wildcards '*/files' && \
    rm -rf /tmp/bitnami/pkg/cache/dotnet-sdk-6.0.400-0-linux-amd64-debian-11.tar.gz
RUN apt-get update && apt-get upgrade -y && \
    rm -r /var/lib/apt/lists /var/cache/apt/archives
RUN sed -i 's/^PASS_MAX_DAYS.*/PASS_MAX_DAYS    90/' /etc/login.defs && \
    sed -i 's/^PASS_MIN_DAYS.*/PASS_MIN_DAYS    0/' /etc/login.defs && \
    sed -i 's/sha512/sha512 minlen=8/' /etc/pam.d/common-password

COPY rootfs /
RUN /opt/bitnami/scripts/dotnet-sdk/postunpack.sh
ENV APP_VERSION="6.0.400" \
    BITNAMI_APP_NAME="dotnet-sdk" \
    DOTNET_CLI_HOME="/app" \
    DOTNET_ROOT="/opt/bitnami/dotnet-sdk/bin" \
    PATH="/opt/bitnami/dotnet-sdk/bin:$PATH"

WORKDIR /app
ENTRYPOINT [ "/opt/bitnami/scripts/dotnet-sdk/entrypoint.sh" ]
CMD [ "/bin/bash" ]
