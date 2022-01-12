FROM php:8.1-fpm

RUN apt-get update && apt-get install -y \
    zlib1g-dev \
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
    && docker-php-ext-configure gd --with-webp=/usr/include/  --with-jpeg=/usr/include/  --with-freetype=/usr/include/

RUN docker-php-ext-install -j$(nproc) intl soap mysqli pdo_mysql zip bcmath pcntl gmp gd && \
    curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

ENV COMPOSER_HOME /.composer

# Install xdebug
ARG WITH_XDEBUG=false
RUN if [ $WITH_XDEBUG = "true" ] ; then \
        pecl install xdebug; \
        docker-php-ext-enable xdebug; \
        echo "xdebug.idekey = PHPSTORM" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
        echo "xdebug.mode=debug" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
    fi ;

# Install php-redis php-igbinary
RUN pecl install -o -f redis igbinary \
    &&  rm -rf /tmp/pear \
    && docker-php-ext-enable redis \
    && docker-php-ext-enable igbinary \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean

CMD ["php-fpm"]

