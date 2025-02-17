FROM docker.io/bitnami/minideb:bullseye
ENV HOME="/" \
    OS_ARCH="amd64" \
    OS_FLAVOUR="debian-11" \
    OS_NAME="linux"

COPY prebuildfs /
SHELL ["/bin/bash", "-o", "pipefail", "-c"]
# Install required system packages and dependencies
RUN install_packages ca-certificates curl fonts-freefont-ttf fonts-ipafont-gothic fonts-kacst fonts-thai-tlwg fonts-wqy-zenhei gnupg gzip jq libavahi-client3 libavahi-common3 libblkid1 libbsd0 libbz2-1.0 libc6 libcom-err2 libcrypt1 libcups2 libdbus-1-3 libexpat1 libffi7 libgcc-s1 libgcrypt20 libglib2.0-0 libgmp10 libgnutls30 libgpg-error0 libgssapi-krb5-2 libhogweed6 libidn2-0 libk5crypto3 libkeyutils1 libkrb5-3 libkrb5support0 liblz4-1 liblzma5 libmd0 libmount1 libncursesw6 libnettle8 libnsl2 libp11-kit0 libpcre2-8-0 libpcre3 libreadline8 libselinux1 libsqlite3-0 libssl1.1 libstdc++6 libsystemd0 libtasn1-6 libtinfo6 libtirpc3 libunistring2 libx11-6 libx11-xcb1 libxau6 libxcb-dri3-0 libxcb1 libxdmcp6 libxext6 libxshmfence1 libxss1 libxtst6 libzstd1 procps tar wget zlib1g
RUN wget -nc -P /tmp/bitnami/pkg/cache/ https://downloads.bitnami.com/files/stacksmith/python-3.8.13-164-linux-amd64-debian-11.tar.gz && \
    echo "3908ffa59bfa43c84081a4803d79ac26ce1647f020ad97ffb264bdbcaa134163  /tmp/bitnami/pkg/cache/python-3.8.13-164-linux-amd64-debian-11.tar.gz" | sha256sum -c - && \
    tar -zxf /tmp/bitnami/pkg/cache/python-3.8.13-164-linux-amd64-debian-11.tar.gz -P --transform 's|^[^/]*/files|/opt/bitnami|' --wildcards '*/files' && \
    rm -rf /tmp/bitnami/pkg/cache/python-3.8.13-164-linux-amd64-debian-11.tar.gz
RUN wget -nc -P /tmp/bitnami/pkg/cache/ https://downloads.bitnami.com/files/stacksmith/node-14.20.0-2-linux-amd64-debian-11.tar.gz && \
    echo "127f7b97bd46468ce62bc5cd9868371ca2c4c1e7201f6271a309b34a17b97d90  /tmp/bitnami/pkg/cache/node-14.20.0-2-linux-amd64-debian-11.tar.gz" | sha256sum -c - && \
    tar -zxf /tmp/bitnami/pkg/cache/node-14.20.0-2-linux-amd64-debian-11.tar.gz -P --transform 's|^[^/]*/files|/opt/bitnami|' --wildcards '*/files' && \
    rm -rf /tmp/bitnami/pkg/cache/node-14.20.0-2-linux-amd64-debian-11.tar.gz
RUN wget -nc -P /tmp/bitnami/pkg/cache/ https://downloads.bitnami.com/files/stacksmith/grafana-image-renderer-3.5.0-1-linux-amd64-debian-11.tar.gz && \
    echo "64b6351d51404246e4abc3f5e23f22a3decba3172fb7122e89a9a072f9985df3  /tmp/bitnami/pkg/cache/grafana-image-renderer-3.5.0-1-linux-amd64-debian-11.tar.gz" | sha256sum -c - && \
    tar -zxf /tmp/bitnami/pkg/cache/grafana-image-renderer-3.5.0-1-linux-amd64-debian-11.tar.gz -P --transform 's|^[^/]*/files|/opt/bitnami|' --wildcards '*/files' && \
    rm -rf /tmp/bitnami/pkg/cache/grafana-image-renderer-3.5.0-1-linux-amd64-debian-11.tar.gz
RUN apt-get update && apt-get upgrade -y && \
    rm -r /var/lib/apt/lists /var/cache/apt/archives
RUN chmod g+rwX /opt/bitnami
RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - && \
    echo 'deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main' >> /etc/apt/sources.list.d/google.list && \
    install_packages google-chrome-unstable

ENV APP_VERSION="3.5.0" \
    BITNAMI_APP_NAME="grafana-image-renderer" \
    HTTP_HOST="0.0.0.0" \
    HTTP_PORT="8080" \
    PATH="/opt/bitnami/python/bin:/opt/bitnami/node/bin:$PATH"

EXPOSE 3000 8080

WORKDIR /opt/bitnami/grafana-image-renderer
USER 1001
ENTRYPOINT [ "node" ]
CMD [ "build/app.js", "server", "--config=conf/config.json" ]
