FROM php:7.2-fpm-alpine3.7

RUN apk upgrade && apk add --update --no-cache --virtual .ext-deps \
  autoconf \
  automake \
  libtool \
  nasm \
  g++ \
  make \
  freetype-dev \
  libjpeg-turbo-dev \
  libmcrypt-dev \
  libpng-dev \
  libwebp-dev

RUN set -xe \
    && apk add --update --no-cache \
    ca-certificates \
    curl \
    openssh-client \
    libmemcached-libs \
    libevent \
    libssl1.0 \
    musl \
    yaml \
    php7 \
    php7-apcu \
    php7-bcmath \
    php7-ctype \
    php7-curl \
    php7-dom \
    php7-fileinfo \
    php7-iconv \
    php7-intl \
    php7-json \
    php7-openssl \
    php7-opcache \
    php7-mbstring \
    php7-memcached \
    php7-mcrypt \
    php7-mysqlnd \
    php7-mysqli \
    php7-pcntl \
    php7-pgsql \
    php7-pdo_mysql \
    php7-pdo_pgsql \
    php7-pdo_sqlite \
    php7-phar \
    php7-posix \
    php7-session \
    php7-simplexml \
    php7-soap \
    php7-sockets \
    php7-tokenizer \
    php7-xml \
    php7-xmlreader \
    php7-xmlwriter \
    php7-zip \
    php7-zlib

# install pdo_mysql and composer and other utilities
RUN apk update --no-cache \
    && apk add --no-cache $PHPIZE_DEPS \
    && apk add --no-cache mysql-dev \
    && apk add --no-cache git \
    && php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php --install-dir=/usr/bin --filename=composer \
    && php -r "unlink('composer-setup.php');" \
    && docker-php-ext-install pdo pdo_mysql

# install mysql client
RUN apk add --update mysql mysql-client && rm -f /var/cache/apk/*

RUN docker-php-ext-install mysqli && \
	docker-php-ext-install pdo_mysql && \
	docker-php-ext-install mbstring && \
	docker-php-ext-install opcache

# install zip
RUN apk add --no-cache libzip-dev && docker-php-ext-configure zip --with-libzip=/usr/include && docker-php-ext-install zip

# XDebug
RUN apk add --no-cache --virtual .build-deps $PHPIZE_DEPS \
    && pecl install xdebug-2.7.0 \
    && docker-php-ext-enable xdebug \
    && apk del -f .build-deps

# Configure
COPY php.ini /usr/local/etc/php/php.ini
COPY php-fpm.conf /usr/local/etc/
COPY ext-xdebug.ini /usr/local/etc/php/conf.d/ext-xdebug.ini

# Make sure the volume mount point is empty
RUN rm -rf /var/www/html/*

RUN apk add --no-cache sudo

# install mysql client
RUN apk add --update mysql mysql-client && rm -f /var/cache/apk/*
