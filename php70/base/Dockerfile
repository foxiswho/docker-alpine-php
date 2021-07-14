FROM php:7.0-fpm-alpine

# =================================================================================================

# Install packages
RUN set -ex \
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
    && pecl install xdebug \
    && pecl install memcached \
    && pecl install geoip-1.1.1 \
    && docker-php-ext-enable redis memcached xdebug geoip

RUN curl -sS https://getcomposer.org/installer | php && mv ./composer.phar /usr/local/bin/composer

RUN set -ex \
        # change apk source repo
        # - config PHP-FPM
        && cd /etc/php7 \
        && { \
            echo "[global]"; \
            echo "pid = /var/run/php-fpm.pid"; \
            echo "[www]"; \
            echo "user = www"; \
            echo "group = www"; \
        } | tee php-fpm.d/custom.conf \
        # - config site
        && chown -R www:www /var/www \
        && sed -i 's/127.0.0.1:9000/0.0.0.0:9000/g' /etc/php7/php-fpm.d/www.conf \
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
        && mkdir -p /data \
        && mkdir -p /wwwroot \
        && echo -e "\033[42;37m Build Completed :).\033[0m\n"

EXPOSE 9000
VOLUME ["/var/www", "/data", "/wwwroot"]
WORKDIR "/var/www"

CMD /run.sh
