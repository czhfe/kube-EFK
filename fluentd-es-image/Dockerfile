FROM ubuntu:18.04

COPY Gemfile /Gemfile

RUN set -ex \
    && sed -i 's#http://.*.ubuntu.com#http://mirrors.aliyun.com#' /etc/apt/sources.list \
    && apt update \
    && BUILD_DEPS="make gcc g++ libc6-dev ruby-dev libffi-dev" \
    && apt-get install -y --no-install-recommends $BUILD_DEPS \
        ruby \
        libjemalloc1 \
    && echo 'gem: --no-document' >> /etc/gemrc \
    && gem sources --remove https://rubygems.org/ \
    && gem sources -a https://mirrors.aliyun.com/rubygems/ \
    && gem install --file Gemfile \
    && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false $BUILD_DEPS \
    && ulimit -n 65536 \
    && apt-get clean -y \
    && rm -rf \
       /var/cache/debconf/* \
       /var/lib/apt/lists/* \
       /var/log/* \
       /tmp/* \
       /var/tmp/* \
       /Gemfile

COPY fluent.conf /etc/fluent/fluent.conf

# Expose prometheus metrics.
EXPOSE 80

ENTRYPOINT ["fluentd"]