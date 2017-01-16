FROM alpine:3.5

ENV DOCKERIZE_VERSION 0.2.0
RUN apk add --no-cache ca-certificates curl && \
    mkdir -p /usr/local/bin/ && \
    curl -SL https://github.com/jwilder/dockerize/releases/download/v${DOCKERIZE_VERSION}/dockerize-linux-amd64-v${DOCKERIZE_VERSION}.tar.gz \
    | tar xzC /usr/local/bin

ENV INFLUXDB_VERSION 1.1.1-c1.1.1
RUN apk add --no-cache --virtual .build-deps wget tar && \
    wget -q https://dl.influxdata.com/enterprise/releases/influxdb-meta-${INFLUXDB_VERSION}-static_linux_amd64.tar.gz && \
    mkdir -p /usr/src && \
    tar -C /usr/src -xzf influxdb-meta-${INFLUXDB_VERSION}-static_linux_amd64.tar.gz && \
    rm -f /usr/src/influxdb-meta-*/influxdb.conf && \
    chmod +x /usr/src/influxdb-meta-*/* && \
    cp -a /usr/src/influxdb-meta-*/* /usr/bin/ && \
    rm -rf *.tar.gz* /usr/src /root/.gnupg && \
    apk del .build-deps

COPY influxdb-meta.tmpl /etc/influxdb/influxdb-meta.tmpl

EXPOSE 8088 8089 8091

VOLUME /var/lib/influxdb

ENV INFLUXDB_REGISTRATION_URL http://influxdb-web:3000
ENV INFLUXDB_LICENSE_KEY INVALID
ENV INFLUXDB_SHARED_SECRET SECRET

CMD dockerize \
    -template /etc/influxdb/influxdb-meta.tmpl:/etc/influxdb/influxdb-meta.conf \
    /usr/bin/influxd-meta -config /etc/influxdb/influxdb-meta.conf
