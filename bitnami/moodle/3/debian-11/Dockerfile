FROM docker.io/bitnami/minideb:bullseye
ENV HOME="/" \
    OS_ARCH="amd64" \
    OS_FLAVOUR="debian-11" \
    OS_NAME="linux"

ARG EXTRA_LOCALES=""
ARG WITH_ALL_LOCALES="no"

COPY prebuildfs /
SHELL ["/bin/bash", "-o", "pipefail", "-c"]
# Install required system packages and dependencies
RUN install_packages acl ca-certificates cron curl gzip libaudit1 libbrotli1 libbsd0 libbz2-1.0 libc6 libcap-ng0 libcom-err2 libcrypt1 libcurl4 libedit2 libexpat1 libffi7 libfftw3-double3 libfontconfig1 libfreetype6 libgcc-s1 libgcrypt20 libglib2.0-0 libgmp10 libgnutls30 libgomp1 libgpg-error0 libgssapi-krb5-2 libhogweed6 libicu67 libidn2-0 libjpeg62-turbo libk5crypto3 libkeyutils1 libkrb5-3 libkrb5support0 liblcms2-2 libldap-2.4-2 liblqr-1-0 libltdl7 liblzma5 libmagickcore-6.q16-6 libmagickwand-6.q16-6 libmcrypt4 libmd0 libmemcached11 libncurses6 libnettle8 libnghttp2-14 libonig5 libp11-kit0 libpam0g libpcre2-8-0 libpcre3 libpng16-16 libpq5 libpsl5 libreadline8 librtmp1 libsasl2-2 libsodium23 libsqlite3-0 libssh2-1 libssl1.1 libstdc++6 libsybdb5 libtasn1-6 libtidy5deb1 libtinfo6 libunistring2 libuuid1 libwebp6 libx11-6 libxau6 libxcb1 libxdmcp6 libxext6 libxml2 libxslt1.1 libzip4 locales procps tar zlib1g
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "php" "7.4.30-156" --checksum 9a2008341bef6242b06933fac4651d8dfbd29d4139b7ea732964d2ba26842ebb
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "apache" "2.4.54-154" --checksum 5a352475f0d887b8489e41d0c7f2e40c814899208ac53e4cf2b8b9ae96dc41b9
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "postgresql-client" "13.7.0-152" --checksum c36c823d21ed1c114369c7335ffe72df1c53e57214861702a10fc290f7350e48
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "mysql-client" "10.6.8-152" --checksum 3c4757f1064b7a1648886658f05706957cf165c57fa922f4a6ebb757a19034cb
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "libphp" "7.4.30-152" --checksum 0a783cb719a50d8b30ab5f9af77c9f22258f060dd2cdf709b8852132c6148ad3
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "render-template" "1.0.3-152" --checksum 7909504f3232654c427681ecc8ed4b8599a2df0f17f26ff769a83ed167d05b69
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "moodle" "3.11.8-2" --checksum 0818175e22592e088a558d9376011e8ecbf1fc31d3d70637e9a44171f966cac8
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "gosu" "1.14.0-153" --checksum d3ae0109be24e8bc56f97359b8290b91243454de946dd60bd8520a0896a88449
RUN apt-get update && apt-get upgrade -y && \
    rm -r /var/lib/apt/lists /var/cache/apt/archives
RUN chmod g+rwX /opt/bitnami
RUN localedef -c -f UTF-8 -i en_US en_US.UTF-8
RUN sed -i -e '/pam_loginuid.so/ s/^#*/#/' /etc/pam.d/cron
RUN update-locale LANG=C.UTF-8 LC_MESSAGES=POSIX && \
    DEBIAN_FRONTEND=noninteractive dpkg-reconfigure locales
RUN echo 'en_AU.UTF-8 UTF-8' >> /etc/locale.gen && locale-gen
RUN echo 'en_US.UTF-8 UTF-8' >> /etc/locale.gen && locale-gen

COPY rootfs /
RUN /opt/bitnami/scripts/locales/add-extra-locales.sh
RUN /opt/bitnami/scripts/apache/postunpack.sh
RUN /opt/bitnami/scripts/php/postunpack.sh
RUN /opt/bitnami/scripts/apache-modphp/postunpack.sh
RUN /opt/bitnami/scripts/moodle/postunpack.sh
RUN /opt/bitnami/scripts/mysql-client/postunpack.sh
ENV APACHE_HTTPS_PORT_NUMBER="" \
    APACHE_HTTP_PORT_NUMBER="" \
    APP_VERSION="3.11.8" \
    BITNAMI_APP_NAME="moodle" \
    LANG="en_US.UTF-8" \
    LANGUAGE="en_US:en" \
    PATH="/opt/bitnami/php/bin:/opt/bitnami/php/sbin:/opt/bitnami/apache/bin:/opt/bitnami/postgresql/bin:/opt/bitnami/mysql/bin:/opt/bitnami/common/bin:$PATH"

EXPOSE 8080 8443

USER root
ENTRYPOINT [ "/opt/bitnami/scripts/moodle/entrypoint.sh" ]
CMD [ "/opt/bitnami/scripts/moodle/run.sh" ]
