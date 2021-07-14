FROM php:7.0-fpm-alpine

# =================================================================================================
# --build-arg timezone=Asia/Shanghai
ARG timezone
# pdt pre test dev
ARG app_env=pdt
ARG add_user=www

ENV APP_ENV=${app_env:-"pdt"} \
    TIMEZONE=${timezone:-"Asia/Shanghai"} \
    #  install and remove building packages
    PHPIZE_DEPS="autoconf dpkg-dev dpkg file g++ gcc libc-dev make php7-dev php7-pear pkgconf re2c pcre-dev zlib-dev"
# Install packages
RUN set -ex \
        &&  ls -lh /usr/local/bin \
        &&  ls -lh /usr/local/sbin \
        &&  ls -lh /usr/local/etc \
        &&  ls -lh /usr/local/etc/php \
        &&  ls -lh /usr/local/etc/php/conf.d \
        &&  ls -lh /usr/local/etc/php-fpm.d \
#        &&  ls -lh /usr/local/etc/php-fpm.conf \
#        &&  cat /usr/local/etc/php-fpm.conf \
        &&  mkdir -p /etc/php7 \
        && cp -rf /usr/local/etc/php/php.ini-production  /usr/local/etc/php/php.ini \
        && ln -s /usr/local/etc/php/php.ini /etc/php7/php.ini \
        && ln -s /usr/local/etc/php-fpm.conf /etc/php7/php-fpm.conf \
        && ln -s /usr/local/etc/php-fpm.d /etc/php7/php-fpm.d \
        && ln -s /usr/local/etc/php/conf.d /etc/php7/conf.d \
        && ls -lh /etc/php7 \
        && ln -s /usr/local/bin/php /usr/local/bin/php7 \
        && ln -s /usr/local/sbin/php-fpm /usr/local/bin/php7-fpm \
        #
        && cd /usr/local/etc/php \
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
                        && mkdir -p /wwwroot \
                        && chown -R ${add_user}:${add_user} /data \
                        && chown -R ${add_user}:${add_user} /wwwroot \
                && cd /usr/local/etc \
                && { \
                    echo "[global]"; \
                    echo "pid = /var/run/php-fpm.pid"; \
                    echo "[www]"; \
                    echo "user = www"; \
                    echo "group = www"; \
                } | tee php-fpm.d/custom.conf \
                # - config site
                && chown -R www:www /var/www \
                && sed -i 's/127.0.0.1:9000/0.0.0.0:9000/g' /usr/local/etc/php-fpm.d/www.conf \
                && { \
                    echo "#!/bin/sh"; \
                    # echo "php /var/www/uem.phar taskServer:start -d"; \
                    echo "php-fpm7 -F"; \
                } | tee /run.sh \
                && chmod 755 /run.sh \
                && echo "alias ls='ls -lhG --color=auto'" >> /root/.bashrc \
                && echo "alias ll='ls -lhG --color=auto'" >> /root/.bashrc \
                && chmod 755 /root/.bashrc \
                && . /root/.bashrc \
        && curl -sS https://getcomposer.org/installer | php \
        && mv composer.phar /usr/local/bin/ \
        && chmod 755 /usr/local/bin/composer.phar \
        && ln -s /usr/local/bin/composer.phar /usr/local/bin/composer \
            # change apk source repo
            && sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/' /etc/apk/repositories \
            && apk update
RUN apk add --no-cache git supervisor curl curl-dev autoconf libjpeg-turbo-dev freetype-dev libpng-dev libmcrypt-dev libcurl libbz2 bzip2-dev geoip openssl-dev icu icu-dev icu-libs zlib-dev memcached libmemcached-dev alpine-sdk build-base gcc wget cyrus-sasl-dev geoip-dev libxslt-dev libxml2-dev

RUN docker-php-ext-configure intl && docker-php-ext-install intl
RUN docker-php-ext-install mcrypt iconv curl mbstring bcmath bz2 mysqli pdo pdo_mysql zip xml json xsl
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd

RUN pecl channel-update pecl.php.net
RUN pecl install redis \
#    && pecl install xdebug \
    && pecl install memcached \
    && pecl install geoip-1.1.1 \
    && docker-php-ext-enable redis memcached geoip

#RUN curl -sS https://getcomposer.org/installer | php && mv ./composer.phar /usr/local/bin/composer

RUN set -ex \
        # change apk source repo
        # - config PHP-FPM
        && rm -rf /var/cache/apk/* /tmp/* /usr/share/man /usr/share/php7 \
        && echo -e "\033[42;37m Build Completed :).\033[0m\n"

EXPOSE 9000
VOLUME ["/var/www", "/data", "/wwwroot"]
WORKDIR "/var/www"

CMD /run.sh
