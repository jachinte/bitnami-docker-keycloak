FROM docker.io/bitnami/minideb:buster
LABEL maintainer "Bitnami <containers@bitnami.com>"

ENV HOME="/" \
    OS_ARCH="amd64" \
    OS_FLAVOUR="debian-10" \
    OS_NAME="linux"

COPY prebuildfs /
# Install required system packages and dependencies
RUN install_packages acl ca-certificates curl gzip libaio1 libc6 procps rsync tar zlib1g
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "wait-for-port" "1.0.0-3" --checksum 7521d9a4f9e4e182bf32977e234026caa7b03759799868335bccb1edd8f8fd12
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "java" "11.0.12-0" --checksum 14c1274e93b1135d4d1b82ad7985fc2fa7f5d0689b6da18e0c94da37407cd4ea
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "keycloak" "15.0.2-2" --checksum a9c60374e6f34130f8621fe871bba7eb163583538ece932652d03450d6426d55
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "gosu" "1.14.0-0" --checksum 3e6fc37ca073b10a73a804d39c2f0c028947a1a596382a4f8ebe43dfbaa3a25e
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "mysql-client" "10.3.31-2" --checksum 15b394a022ca1d0b4b89bfee0487ee13f9e8cc697d0ca09ce62a0a37325
RUN chmod g+rwX /opt/bitnami

COPY rootfs /

# Create the mysql module (com.mysql)
RUN mkdir -p /opt/bitnami/keycloak/modules/system/layers/base/com/mysql/main
RUN curl -L -o mysql-connector.jar https://repo1.maven.org/maven2/mysql/mysql-connector-java/8.0.26/mysql-connector-java-8.0.26.jar
RUN echo 'e5ec6610020a3084b7d32ee725d1650176f6b3de  mysql-connector.jar' | sha1sum -c -
RUN mv mysql-connector.jar /opt/bitnami/keycloak/modules/system/layers/base/com/mysql/main
RUN mv /opt/bitnami/scripts/mysql-module.xml /opt/bitnami/keycloak/modules/system/layers/base/com/mysql/main/module.xml

RUN /opt/bitnami/scripts/keycloak/postunpack.sh
ENV BITNAMI_APP_NAME="keycloak" \
    BITNAMI_IMAGE_VERSION="15.0.2-debian-10-r42" \
    PATH="/opt/bitnami/common/bin:/opt/bitnami/java/bin:/opt/bitnami/keycloak/bin:$PATH"

USER 1001
ENTRYPOINT [ "/opt/bitnami/scripts/keycloak/entrypoint.sh" ]
CMD [ "/opt/bitnami/scripts/keycloak/run.sh" ]
