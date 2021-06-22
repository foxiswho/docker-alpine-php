# @description php 8.0 image base on the alpine 3.10 镜像更小
#                       some information
# ------------------------------------------------------------------------------------
# @link https://hub.docker.com/_/alpine/      alpine image
# @link https://hub.docker.com/_/php/         php image
# @link https://github.com/docker-library/php php dockerfiles
# ------------------------------------------------------------------------------------
# @build-example docker build . -f alphp-base.Dockerfile -t alphp/alphp:base
#

FROM alpine:edge
LABEL maintainer="foxwho <foxiswho@gmail.com>" version="1.0"

##
# ---------- env settings ----------
##

# --build-arg timezone=Asia/Shanghai
ARG timezone
# pdt pre test dev
ARG app_env=pdt
ARG add_user=www

ENV APP_ENV=${app_env:-"pdt"} \
    TIMEZONE=${timezone:-"Asia/Shanghai"} \
    #  install and remove building packages
    PHPIZE_DEPS="autoconf dpkg-dev dpkg file g++ gcc libc-dev make  pkgconf re2c pcre-dev zlib-dev "

ENV HIREDIS_VERSION=1.0.0 \
    PHALCON_VERSION=4.1.2 \
    SWOOLE_VERSION=4.6.7 \
    MONGO_VERSION=1.9.1
##
# ---------- building ----------
##

RUN set -ex \
        # change apk source repo
        #&& sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/' /etc/apk/repositories \
#        echo "https://mirror.tuna.tsinghua.edu.cn/alpine/edge/main" > /etc/apk/repositories \
#        && echo "https://mirror.tuna.tsinghua.edu.cn/alpine/edge/community" >> /etc/apk/repositories \
        && echo "https://mirror.tuna.tsinghua.edu.cn/alpine/edge/testing" >> /etc/apk/repositories \
        && apk update \
        && apk add --no-cache \
        # Install base packages
        vim \
        wget \
        net-tools \
        git \
        zip \
        unzip \
        ca-certificates \
        curl \
        tar \
        xz \
        #libressl \
        # openssh  \
        openssl  \
        tzdata \
        pcre \
        supervisor \
        #
        # install php8 and some extensions
        php8 \
        php8-common \
        php8-dev \
        php8-pear \
        php8-fpm \
        #php8-amqp \
        php8-bz2 \
        php8-bcmath \
        php8-calendar \
        php8-curl \
        php8-cgi \
        php8-ctype \
        php8-dom \
        php8-dba \
        php8-doc \
        php8-embed \
        php8-enchant \
        php8-exif \
        php8-ftp \
        php8-fileinfo \
        # php8-filter \
        php8-gd \
        php8-gettext \
        php8-gmp \
        php8-iconv \
        php8-imap \
        php8-intl \
        #php8-json \
        php8-ldap \
        #php8-litespeed \
#        php8-memcached \
#        php8-mcrypt \
        php8-mbstring \
        php8-mongodb \
        php8-mysqlnd \
        php8-mysqli \
        php8-odbc \
        php8-openssl \
        php8-opcache \
        php8-pcntl \
        php8-pdo \
        php8-pdo_dblib \
        php8-pdo_mysql \
        php8-pdo_odbc \
        php8-pdo_pgsql \
        php8-pdo_sqlite \
        php8-pgsql \
        php8-phar \
        php8-phpdbg \
        php8-pspell \
        php8-posix \
        #php8-recode \
        php8-simplexml \
        #php8-sqlite \
        php8-session \
        php8-shmop \
        php8-snmp \
        php8-sockets \
        php8-soap \
        php8-sodium \
        php8-sqlite3 \
        #php8-ssh2 \
        php8-sysvshm \
        php8-sysvmsg \
        php8-sysvsem \
#        php8-timezonedb \
        php8-tidy \
        php8-tokenizer \
        #php8-wddx \
        php8-xml \
        php8-xmlreader \
       #php8-xmlrpc \
        php8-xmlwriter \
        php8-xsl \
        php8-zip \
        #php8-zlib \
#        php8-xlswriter \
        php8-pecl-redis  \
        php8-pecl-memcached  \
        php8-pecl-mongodb  \
        php8-pecl-swoole  \
        php8-pecl-imagick  \
        php8-pecl-event  \
        php8-pecl-protobuf  \
        php8-pecl-xhprof  \
        php8-pecl-ssh2  \
        php8-pecl-csv  \
        php8-pecl-ssh2  \
        php8-pecl-decimal  \
        php8-pecl-timezonedb  \
        php8-pecl-xmlrpc  \
        php8-pecl-seaslog  \
        php8-pecl-xlswriter  \
        php8-pecl-yaml  \
        php8-pecl-uuid  \
        php8-pecl-mcrypt  \
        php8-pecl-grpc  \
        && apk del --purge *-dev
##        && rm -rf /var/cache/apk/* /tmp/* /usr/share/man /usr/share/php8

##
# ---------- some config,clear work ----------
##
RUN set -ex \
        && ls -alh /etc \
        #&& ls -alh /etc/php \
        && cd /etc/php8 \
        # - config PHP
        && { \
            echo "upload_max_filesize=100M"; \
            echo "post_max_size=108M"; \
            echo "memory_limit=1024M"; \
            echo "date.timezone=${TIMEZONE}"; \
        } | tee conf.d/99-overrides.ini \
        #
        # - config timezone
        && ln -sf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime \
        && echo "${TIMEZONE}" > /etc/timezone \
        #
        # ---------- some config work ----------
        # - ensure 'www' user exists
        && addgroup -S ${add_user} \
        && adduser -D -S -G ${add_user} ${add_user} \
        #
        # - create user dir
        && mkdir -p /data \
        && chown -R ${add_user}:${add_user} /data \
        && mkdir -p /usr/local/etc/ \
        && ln -s /etc/php8/php-fpm.conf /usr/local/etc/php-fpm.conf \
        && ln -s /etc/php8/php-fpm.d /usr/local/etc/php-fpm.d \
        && ls -alh /etc/php8/conf.d/ \
        #
        && echo -e "\033[42;37m Build Completed :).\033[0m\n"

##
# install php extensions
##

# 下载太慢，所以可以先下载好
# COPY deps/hiredis-${HIREDIS_VERSION}.tar.gz hiredis.tar.gz
# COPY deps/swoole-${SWOOLE_VERSION}.tar.gz swoole.tar.gz
# COPY deps/cphalcon-${PHALCON_VERSION}.tar.gz cphalcon.tar.gz
# COPY deps/mongodb-${MONGO_VERSION}.tgz mongodb.tgz
RUN set -ex \
        && cd /tmp \
        #&& curl -SL "https://github.com/redis/hiredis/archive/v${HIREDIS_VERSION}.tar.gz" -o hiredis.tar.gz \
        #&& curl -SL "https://github.com/swoole/swoole-src/archive/v${SWOOLE_VERSION}.tar.gz" -o swoole.tar.gz \
        # && curl -SL "https://github.com/phalcon/cphalcon/archive/v${PHALCON_VERSION}.tar.gz" -o cphalcon.tar.gz \
        # && curl -SL "https://github.com/mongodb/mongo-php-driver/archive/v${MONGO_VERSION}.tgz" -o mongodb.tgz \
        # && curl -SL "http://pecl.php.net/get/mongodb-${MONGO_VERSION}.tgz" -o mongodb.tgz \
        && ln -s /usr/bin/php8 /usr/bin/php \
        && ln -s /usr/bin/pecl8 /usr/bin/pecl \
        && ls -alh /usr/local/bin \
        && ls -alh /usr/local/bin \
        && ls -alh /usr/bin \
        #&& wget http://pear.php.net/go-pear.phar -O go-pear.php \
        #&& /usr/bin/php8 go-pear.php \
        #&& ls -alh /usr/local/bin \
        #        && ls -alh /usr/bin \
        && ls -alh \
        && apk update \
        && apk add --no-cache --virtual .phpize-deps \
        $PHPIZE_DEPS \
        # for mongodb ext
        openssl-dev \
        # for swoole ext
        libaio linux-headers libaio-dev \
        #
        # php extension: mongodb
        # && pecl install mongodb.tgz \
        #&& pecl8 install mongodb \
        #&& echo "extension=mongodb.so" > /etc/php8/conf.d/20_mongodb.ini  \
        #&& pecl8 install seaslog \
        #&& echo "extension=seaslog.so" > /etc/php8/conf.d/20_seaslog.ini  \
        ##&& pecl8 install swoole \
        #&& echo "extension=swoole.so" > /etc/php8/conf.d/20_swoole.ini  \
        #
        # php extension: phalcon framework
        # && tar -xf cphalcon.tar.gz \
        # && apk add --no-cache --virtual .phpize-deps $PHPIZE_DEPS \
        # && cd cphalcon-${PHALCON_VERSION}/build \
        # # in alpine no bash shell, so change to 'sh install'
        # # && ./install \
        # && sh install \
        # && cp ../tests/_ci/phalcon.ini $(php-config --configure-options | grep -o "with-config-file-scan-dir=\([^ ]*\)" | awk -F'=' '{print $2}') \
        # && cd ../../ \
        # && rm -r cphalcon-${PHALCON_VERSION} \
        #
        # hiredis - redis C client, provide async operate support for Swoole
        # && wget -O hiredis.tar.gz -c https://github.com/redis/hiredis/archive/v${HIREDIS_VERSION}.tar.gz \
        #&& cd /tmp \
        #&& tar -zxvf hiredis.tar.gz \
        #&& cd hiredis-${HIREDIS_VERSION} \
        #&& make -j && make install \
        #
        # php extension: swoole
        #&& cd /tmp \
        #&& mkdir -p swoole \
        #&& tar -xf swoole.tar.gz -C swoole --strip-components=1 \
        #&& rm swoole.tar.gz \
        #&& ( \
        #    cd swoole \
        #    && phpize \
        #    && ./configure --enable-async-redis --enable-mysqlnd --enable-coroutine \
        #    && make -j$(nproc) && make install \
        #) \
        #&& rm -r swoole \
        && ls -alh /etc/php8/conf.d/ \
#        && echo "extension=swoole.so" > /etc/php8/conf.d/20_swoole.ini \
        && curl -sS https://getcomposer.org/installer | php \
        && mv composer.phar /usr/local/bin/ \
        && chmod 755 /usr/local/bin/composer.phar \
        && ln -s /usr/local/bin/composer.phar /usr/local/bin/composer \
        && curl -sSLo phpunit.phar https://phar.phpunit.de/phpunit.phar \
        && chmod 755 phpunit.phar \
        && mv phpunit.phar /usr/local/bin/ \
        && ln -s /usr/local/bin/phpunit.phar /usr/local/bin/phpunit \
        && apk del .phpize-deps \
        && rm -rf /var/cache/apk/* /tmp/* /usr/share/man /usr/share/php8 \
        && mkdir -p /var/log/www/default \
        #
        && echo -e "\033[42;37m Build Completed :).\033[0m\n"

EXPOSE 9000
#EXPOSE 9501

VOLUME ["/var/www", "/data"]
WORKDIR "/var/www"
