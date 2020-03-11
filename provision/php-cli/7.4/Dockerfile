FROM php:7.4-cli

RUN apt-get update && apt-get install -y \
    zlib1g \
    zlib1g-dbg \
    zlib1g-dev \
    zlibc \
    libmcrypt-dev \
    libicu-dev \
    g++ \
    git \
    libxml2-dev \
    libgmp-dev \
    libzip-dev \
    iproute2 \
    mariadb-client \
    libfreetype6-dev \
    libjpeg-dev \
    libwebp-dev \
    libpng-dev \
    libjpeg62-turbo-dev \
    # mbstring fix
    libonig-dev

RUN docker-php-ext-configure intl
RUN docker-php-ext-configure gd --with-freetype --with-webp --with-jpeg

RUN docker-php-ext-install -j$(nproc) intl soap mysqli pdo_mysql zip bcmath pcntl gmp gd && \
    curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN pecl install mongodb \
    && docker-php-ext-enable mongodb

ENV COMPOSER_HOME /.composer

ARG WITH_XDEBUG=false
RUN if [ $WITH_XDEBUG = "true" ] ; then \
        pecl install xdebug; \
        docker-php-ext-enable xdebug; \
        echo "xdebug.idekey = PHPSTORM" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
        echo "xdebug.default_enable = 1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
        echo "xdebug.remote_enable = 1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
        echo "xdebug.remote_autostart = 1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
        echo "xdebug.remote_connect_back = 1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
        echo "xdebug.profiler_enable = 1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
        echo "xdebug.remote_host=`/sbin/ip route|awk '/default/ { print $3 }'`" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
    fi ;

ENV SUPERCRONIC_URL=https://github.com/aptible/supercronic/releases/download/v0.1.8/supercronic-linux-amd64 \
    SUPERCRONIC=supercronic-linux-amd64 \
    SUPERCRONIC_SHA1SUM=be43e64c45acd6ec4fce5831e03759c89676a0ea

ARG WITH_SUPERCRONIC=false
RUN if [ $WITH_SUPERCRONIC = "true" ] ; then \
         curl -fsSLO "$SUPERCRONIC_URL"; \
         echo "${SUPERCRONIC_SHA1SUM}  ${SUPERCRONIC}" | sha1sum -c -; \
         chmod +x "$SUPERCRONIC"; \
         mv "$SUPERCRONIC" "/usr/local/bin/${SUPERCRONIC}"; \
         ln -s "/usr/local/bin/${SUPERCRONIC}" /usr/local/bin/supercronic; \
    fi ;

CMD ["php"]

