FROM php:7.3.31-fpm-bullseye

COPY ./sources.list  /etc/apt/sources.list

RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN echo 'Asia/Shanghai' >/etc/timezone

RUN apt-get update \
    && export CFLAGS="$PHP_CFLAGS" CPPFLAGS="$PHP_CPPFLAGS" LDFLAGS="$PHP_LDFLAGS" \
    && apt-get install -y --no-install-recommends cron libonig-dev libfreetype6-dev \
    libjpeg62-turbo-dev libpng-dev libmagickwand-dev libmcrypt-dev libmemcached-dev \
    libzip-dev git libzmq3-dev libssl-dev \
    && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/  \
    && docker-php-ext-configure zip --with-libzip  \
    && docker-php-ext-configure opcache --enable-opcache \
    && docker-php-ext-install -j$(nproc) soap xmlrpc opcache zip  bcmath calendar exif gettext sockets dba mysqli pcntl \
    pdo_mysql shmop sysvmsg sysvsem sysvshm iconv gd mbstring

RUN pecl install mcrypt redis\
    && docker-php-ext-enable mcrypt redis

COPY ./php-fpm/php.ini /usr/local/etc/php/

RUN mkdir -p /var/www/html && chown -R www-data:www-data /var/www/html

WORKDIR /var/www/html

CMD ["php-fpm"]
