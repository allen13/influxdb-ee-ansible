FROM alpine:3.5

ENV DOCKERIZE_VERSION 0.2.0
RUN apk add --no-cache ca-certificates curl && \
    mkdir -p /usr/local/bin/ && \
    curl -SL https://github.com/jwilder/dockerize/releases/download/v${DOCKERIZE_VERSION}/dockerize-linux-amd64-v${DOCKERIZE_VERSION}.tar.gz \
    | tar xzC /usr/local/bin

ENV INFLUXDB_VERSION 1.1.1-c1.1.1
RUN apk add --no-cache --virtual .build-deps wget tar && \
    wget -q https://dl.influxdata.com/enterprise/releases/influxdb-data-${INFLUXDB_VERSION}-static_linux_amd64.tar.gz && \
    mkdir -p /usr/src && \
    tar -C /usr/src -xzf influxdb-data-${INFLUXDB_VERSION}-static_linux_amd64.tar.gz && \
    rm -f /usr/src/influxdb-data-*/influxdb.conf && \
    chmod +x /usr/src/influxdb-data-*/* && \
    cp -a /usr/src/influxdb-data-*/* /usr/bin/ && \
    rm -rf *.tar.gz* /usr/src /root/.gnupg && \
    apk del .build-deps

COPY influxdb.tmpl /etc/influxdb/influxdb.tmpl

EXPOSE 8086 8088

VOLUME /var/lib/influxdb

ENV INFLUXDB_SHARED_SECRET SECRET
ENV INFLUXDB_LICENSE_FILE /etc/influx-enterprise/license/influx-enterprise.key.json

CMD dockerize \
    -template /etc/influxdb/influxdb.tmpl:/etc/influxdb/influxdb.conf \
    /usr/bin/influxd -config /etc/influxdb/influxdb.conf
