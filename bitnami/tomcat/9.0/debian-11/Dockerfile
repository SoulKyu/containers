FROM docker.io/bitnami/minideb:bullseye
ENV HOME="/" \
    OS_ARCH="amd64" \
    OS_FLAVOUR="debian-11" \
    OS_NAME="linux"

ARG JAVA_EXTRA_SECURITY_DIR="/bitnami/java/extra-security"
LABEL org.opencontainers.image.authors="https://bitnami.com/contact" \
      org.opencontainers.image.description="Application packaged by Bitnami" \
      org.opencontainers.image.source="https://github.com/bitnami/containers/tree/main/bitnami/tomcat" \
      org.opencontainers.image.title="tomcat" \
      org.opencontainers.image.vendor="VMware, Inc." \
      org.opencontainers.image.version="9.0.65"

COPY prebuildfs /
SHELL ["/bin/bash", "-o", "pipefail", "-c"]
# Install required system packages and dependencies
RUN install_packages acl ca-certificates curl gzip libc6 libssl1.1 procps tar xmlstarlet zlib1g
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "java" "11.0.15-1-2" --checksum 1a5468c7b552dfc8a33c5c9b214dbf7dd143a5f008cc3aa2e0418fab5eb16c5c
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "tomcat" "9.0.65-1" --checksum 0af1fe226f74ae4fc5136d5059ac622367856c0374fcf5b7fc4f48538b7d9795
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "render-template" "1.0.3-152" --checksum 7909504f3232654c427681ecc8ed4b8599a2df0f17f26ff769a83ed167d05b69
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "gosu" "1.14.0-153" --checksum d3ae0109be24e8bc56f97359b8290b91243454de946dd60bd8520a0896a88449
RUN apt-get update && apt-get upgrade -y && \
    rm -r /var/lib/apt/lists /var/cache/apt/archives
RUN chmod g+rwX /opt/bitnami

COPY rootfs /
RUN /opt/bitnami/scripts/java/postunpack.sh
RUN /opt/bitnami/scripts/tomcat/postunpack.sh
ENV APP_VERSION="9.0.65" \
    BITNAMI_APP_NAME="tomcat" \
    JAVA_HOME="/opt/bitnami/java" \
    PATH="/opt/bitnami/java/bin:/opt/bitnami/tomcat/bin:/opt/bitnami/common/bin:$PATH"

EXPOSE 8009 8080

USER 1001
ENTRYPOINT [ "/opt/bitnami/scripts/tomcat/entrypoint.sh" ]
CMD [ "/opt/bitnami/scripts/tomcat/run.sh" ]
