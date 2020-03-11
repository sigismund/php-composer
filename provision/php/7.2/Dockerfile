FROM php:7.2-fpm

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
    libfreetype6-dev \
    libjpeg-dev \
    libwebp-dev \
    libpng-dev \
    libjpeg62-turbo-dev \
    libjpeg-dev

RUN docker-php-ext-configure intl gd --with-png-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-webp-dir=/usr/include/ --with-freetype-dir=/usr/include/

RUN docker-php-ext-install -j$(nproc) intl soap mysqli pdo_mysql mbstring zip bcmath pcntl gmp gd && \
    curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN pecl install mongodb \
    && docker-php-ext-enable mongodb

ENV COMPOSER_HOME /.composer

ARG WITH_XDEBUG=false
RUN if [ $WITH_XDEBUG = "true" ] ; then \
        pecl install xdebug; \
        docker-php-ext-enable xdebug; \
        echo "xdebug.idekey = PHPSTORM" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
        echo "xdebug.default_enable = 0" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
        echo "xdebug.remote_enable = 1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
        echo "xdebug.remote_autostart = 0" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
        echo "xdebug.remote_connect_back = 1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
        echo "xdebug.profiler_enable = 0" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
        echo "xdebug.remote_host=`/sbin/ip route|awk '/default/ { print $3 }'`" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
    fi ;

CMD ["php-fpm"]

