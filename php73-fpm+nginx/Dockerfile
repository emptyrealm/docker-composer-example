FROM php:7.3.8-fpm

RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak \
    && echo 'deb http://mirrors.aliyun.com/debian/ buster main non-free contrib' > /etc/apt/sources.list \
    && echo 'deb http://mirrors.aliyun.com/debian-security buster/updates main' >> /etc/apt/sources.list \
    && echo 'deb http://mirrors.aliyun.com/debian/ buster-updates main non-free contrib' >> /etc/apt/sources.list \
    && echo 'deb http://mirrors.aliyun.com/debian/ buster-backports main non-free contrib' >> /etc/apt/sources.list \
    && apt-get update \
    && export CFLAGS="$PHP_CFLAGS" CPPFLAGS="$PHP_CPPFLAGS" LDFLAGS="$PHP_LDFLAGS" \
    && apt-get install -y --no-install-recommends libfreetype6-dev libjpeg62-turbo-dev libpng-dev libmagickwand-dev libmcrypt-dev libmemcached-dev zlib1g-dev \
    && rm -rf /var/lib/apt/lists/*

COPY ./deployment/php-fpm/php.ini /usr/local/etc/php/

RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) bcmath calendar exif gettext sockets dba mysqli pcntl pdo_mysql shmop sysvmsg sysvsem sysvshm iconv gd mbstring

RUN pecl install mcrypt-1.0.2 redis-5.0.2 \
    && docker-php-ext-enable mcrypt redis \
    && docker-php-ext-configure opcache --enable-opcache && docker-php-ext-install opcache

RUN mkdir -p /var/www/html && chown -R www-data:www-data /var/www/html


CMD ["php-fpm"]