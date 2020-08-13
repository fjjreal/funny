FROM php:7.3-fpm
ENV TZ=Asia/Shanghai

COPY ./pkg/zip-1.15.4.tgz /home/zip.tgz
COPY ./pkg/mcrypt-1.0.2.tgz /home/mcrypt.tgz
COPY ./pkg/redis-4.3.0.tgz /home/redis.tgz
COPY ./pkg/swoole-4.5.0.tgz /home/swoole.tgz
COPY ./sources.list /home/sources.list

RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak \
    && mv /home/sources.list /etc/apt/ \
    && apt-get upgrade \
    && apt-get update \
    && apt-get install -y \
    apt-utils \
    git \
    g++ \
    unzip \
    libzip-dev \
    libxml2-dev \
    libcurl3-dev \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libmcrypt-dev \
    libbz2-dev \
    curl \
    && docker-php-ext-install bcmath iconv mbstring curl pdo pdo_mysql mysqli json xml bz2 \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd \
    && docker-php-ext-enable gd \
    && docker-php-ext-enable pdo_mysql \
    && docker-php-ext-enable mysqli \
    && docker-php-ext-enable bcmath \
    && pecl install /home/zip.tgz \
    && docker-php-ext-enable zip \
    && pecl install /home/mcrypt.tgz \
    && docker-php-ext-enable mcrypt \
    && pecl install /home/redis.tgz \
    && docker-php-ext-enable redis \
    && pecl install /home/swoole.tgz \
    && docker-php-ext-enable swoole \
    && apt-get purge -y --auto-remove \
    && rm -r /var/lib/apt/lists/* \
    && rm -rf /var/cache/apt/* \
    && rm -rf /home/*

COPY ./pkg/composer.phar /usr/local/bin/composer
RUN chmod +x /usr/local/bin/composer \
    && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
