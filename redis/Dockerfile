# syntax=docker/dockerfile:1.4

FROM --platform=amd64 bitnami/redis:7.0.9-debian-11-r2 as origin

FROM isac322/wait-for-port:1.0.6 as wait-for-port

FROM redis:7.0.9

COPY --link --from=wait-for-port /usr/local/bin/wait-for-port /usr/local/bin/wait-for-port
RUN --mount=type=bind,from=origin,target=/tmp/opt,source=/opt \
    cp --dereference --recursive -p /tmp/opt / \
    && ln --force -s "$(which redis-server)" /opt/bitnami/redis/bin/redis-check-aof \
    && ln --force -s "$(which redis-server)" /opt/bitnami/redis/bin/redis-check-rdb \
    && ln --force -s "$(which redis-server)" /opt/bitnami/redis/bin/redis-sentinel \
    && ln --force -s "$(which redis-server)" /opt/bitnami/redis/bin/redis-server \
    && ln --force -s "$(which redis-cli)" /opt/bitnami/redis/bin/redis-cli \
    && ln --force -s "$(which redis-benchmark)" /opt/bitnami/redis/bin/redis-benchmark \
    && ln --force -s /usr/local/bin/wait-for-port /opt/bitnami/common/bin/wait-for-port \
    && ln --force -s "$(which gosu)" /opt/bitnami/common/bin/gosu \
    && ln -s /opt/bitnami/scripts/redis/entrypoint.sh /entrypoint.sh \
    && ln -s /opt/bitnami/scripts/redis/run.sh /run.sh

RUN /opt/bitnami/scripts/redis/postunpack.sh
ENV APP_VERSION="7.0.7" \
    BITNAMI_APP_NAME="redis" \
    PATH="/opt/bitnami/common/bin:/opt/bitnami/redis/bin:$PATH"

USER 1001
ENTRYPOINT [ "/opt/bitnami/scripts/redis/entrypoint.sh" ]
CMD [ "/opt/bitnami/scripts/redis/run.sh" ]
WORKDIR /
VOLUME ["/bitnami/redis/data"]
