FROM amazonlinux:2.0.20220406.1 as build

COPY prebuildfs /

RUN yum install -y tar wget gzip make gcc acl ca-certificates curl procps
RUN mkdir redis-build

WORKDIR redis-build
RUN wget https://download.redis.io/releases/redis-6.2.7.tar.gz
RUN tar -xf redis-6.2.7.tar.gz
RUN cd redis-6.2.7 && make distclean && make PREFIX=/opt/bitnami/redis install
RUN mkdir -p /opt/bitnami/redis/etc
RUN cp /redis-build/redis-6.2.7/redis.conf /opt/bitnami/redis/etc/redis.conf
COPY wait-for-port /usr/local/bin/

RUN chmod g+rwX /opt/bitnami
RUN ln -s /opt/bitnami/scripts/redis/entrypoint.sh /entrypoint.sh
RUN ln -s /opt/bitnami/scripts/redis/run.sh /run.sh

COPY rootfs /

RUN /opt/bitnami/scripts/redis/postunpack.sh

ENV APP_VERSION="6.2.7" \
    BITNAMI_APP_NAME="redis" \
    PATH="/opt/bitnami/common/bin:/opt/bitnami/redis/bin:$PATH"

EXPOSE 6379

USER root
ENTRYPOINT [ "/opt/bitnami/scripts/redis/entrypoint.sh" ]
CMD [ "/opt/bitnami/scripts/redis/run.sh" ]
