FROM ubuntu:18.04

ARG KIBANA_VERSION=7.9.2

RUN set -ex \
    && sed -i 's#http://.*.ubuntu.com#http://mirrors.aliyun.com#' /etc/apt/sources.list \
    && apt-get update \
    && apt-get install -y --no-install-recommends ca-certificates curl \
    && groupadd --gid 1000 kibana \
    && useradd --uid 1000 --gid 1000 --home-dir /usr/local/kibana --no-create-home kibana \
    && cd /tmp && curl -o kibana-${KIBANA_VERSION}-linux-$(uname -m).tar.gz https://artifacts.elastic.co/downloads/kibana/kibana-${KIBANA_VERSION}-linux-$(uname -m).tar.gz \
    && curl -o kibana.tar.gz.sha512 https://artifacts.elastic.co/downloads/kibana/kibana-${KIBANA_VERSION}-linux-$(uname -m).tar.gz.sha512 \
    && sha512sum -c kibana.tar.gz.sha512  \
    && mkdir /usr/local/kibana \
    && tar --no-same-owner --strip-components=1 -xf kibana-${KIBANA_VERSION}-linux-$(uname -m).tar.gz  -C /usr/local/kibana \
    && chown -R kibana:kibana /usr/local/kibana \
    && apt-get clean -y \
    && rm -rf \
       /var/cache/debconf/* \
       /var/lib/apt/lists/* \
       /var/log/* \
       /tmp/* \
       /var/tmp/*

COPY kibana-docker.sh /usr/local/bin/kibana-docker

USER kibana:kibana

ENTRYPOINT ["/usr/local/bin/kibana-docker"]
