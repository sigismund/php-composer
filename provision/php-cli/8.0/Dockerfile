FROM php:8.0-cli

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
    mariadb-client \
    libfreetype6-dev \
    libjpeg-dev \
    libwebp-dev \
    libpng-dev \
    libjpeg62-turbo-dev \
    # mbstring fix
    libonig-dev \
    unzip

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
RUN pecl install -o -f redis-5.3.4 igbinary \
    &&  rm -rf /tmp/pear \
    && docker-php-ext-enable redis \
    && docker-php-ext-enable igbinary \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean

# Install supercronic
ENV SUPERCRONIC_URL=https://github.com/aptible/supercronic/releases/download/v0.1.9/supercronic-linux-amd64 \
    SUPERCRONIC=supercronic-linux-amd64 \
    SUPERCRONIC_SHA1SUM=5ddf8ea26b56d4a7ff6faecdd8966610d5cb9d85
RUN curl -fsSLO "$SUPERCRONIC_URL"; \
         echo "${SUPERCRONIC_SHA1SUM}  ${SUPERCRONIC}" | sha1sum -c -; \
         chmod +x "$SUPERCRONIC"; \
         mv "$SUPERCRONIC" "/usr/local/bin/${SUPERCRONIC}"; \
         ln -s "/usr/local/bin/${SUPERCRONIC}" /usr/local/bin/supercronic;

CMD ["php"]

