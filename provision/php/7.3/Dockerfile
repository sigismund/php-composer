FROM php:7.3-fpm

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

RUN docker-php-ext-configure intl \
    && docker-php-ext-configure gd --with-gd --with-webp-dir --with-jpeg-dir \
      --with-png-dir --with-freetype-dir

RUN docker-php-ext-install -j$(nproc) intl soap mysqli pdo_mysql mbstring zip bcmath pcntl gmp gd && \
    curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

ENV COMPOSER_HOME /.composer

# Install xdebug
ARG WITH_XDEBUG=false
RUN if [ $WITH_XDEBUG = "true" ] ; then \
        pecl install xdebug; \
        docker-php-ext-enable xdebug; \
        echo "xdebug.idekey = PHPSTORM" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
        echo "xdebug.mode=debug" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
        echo "xdebug.client_host=`/sbin/ip route|awk '/default/ { print $3 }'`" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
    fi ;

# Install php-redis php-igbinary
RUN pecl install -o -f redis-5.3.4 igbinary pcov \
    &&  rm -rf /tmp/pear \
    && docker-php-ext-enable redis \
    && docker-php-ext-enable igbinary \
    && docker-php-ext-enable pcov

CMD ["php-fpm"]

