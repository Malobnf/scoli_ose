FROM php:8.3-fpm-alpine

# Outils et libs
RUN apk add --no-cache git unzip icu-dev oniguruma-dev libzip-dev autoconf g++ make libpq-dev openssl-dev

# Extensions PHP (ORM + Intl + perf)
RUN docker-php-ext-install intl pdo pdo_pgsql mbstring zip opcache

# Extension MongoDB (ODM)
RUN pecl install mongodb \
 && docker-php-ext-enable mongodb

# Composer
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# Xdebug
RUN apk add --no-cache $PHPIZE_DEPS \
 && pecl install xdebug \
 && docker-php-ext-enable xdebug \
 && { \
      echo "xdebug.mode=develop,debug"; \
      echo "xdebug.client_host=host.docker.internal"; \
    } > /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini || true

WORKDIR /var/www/html
