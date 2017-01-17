FROM alpine:edge

ENV DOCKERIZE_VERSION 0.2.0
RUN apk add --no-cache ca-certificates curl && \
    update-ca-certificates && \
    mkdir -p /usr/local/bin/ && \
    curl -SL https://github.com/jwilder/dockerize/releases/download/v${DOCKERIZE_VERSION}/dockerize-linux-amd64-v${DOCKERIZE_VERSION}.tar.gz \
    | tar xzC /usr/local/bin

ENV INFLUXDB_VERSION 1.1.1
RUN apk add --no-cache --virtual .build-deps wget tar && \
    wget -q https://s3.amazonaws.com/influx-enterprise/releases/influx-enterprise-${INFLUXDB_VERSION}-static_linux_amd64.tar.gz && \
    mkdir -p /usr/src && \
    tar -C /usr/src -xzf influx-enterprise-${INFLUXDB_VERSION}-static_linux_amd64.tar.gz && \
    rm -f /usr/src/influx-enterprise-*/influxdb.conf && \
    chmod +x /usr/src/influx-enterprise-*/* && \
    cp -a /usr/src/influx-enterprise-*/* /usr/bin/ && \
    rm -rf *.tar.gz* /usr/src /root/.gnupg && \
    apk del .build-deps

COPY influx-enterprise.tmpl /etc/influx-enterprise/influx-enterprise.tmpl

EXPOSE 3000

ENV HOSTNAME influxdb-web
ENV INFLUXDB_WEB_URL http://influxdb-web:3000
ENV INFLUXDB_WEB_DB postgres://postgres:password@influxdb-web-postgres:5432/enterprise
ENV INFLUXDB_LICENSE_KEY INVALID
ENV INFLUXDB_SHARED_SECRET SECRET

CMD dockerize \
    -template /etc/influx-enterprise/influx-enterprise.tmpl:/etc/influx-enterprise/influx-enterprise.conf \
    /usr/bin/influx-enterprise run -c /etc/influx-enterprise/influx-enterprise.conf
