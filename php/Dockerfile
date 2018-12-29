FROM php:7.2.3-cli
RUN set -xe \
    && apt-get update &&  apt-get install -y \
        zlib1g-dev wget\
        libmemcached-dev \
        libfreetype6-dev \
        libmcrypt-dev \
        libjpeg62-turbo-dev \
        libxml2-dev \
        libxslt1.1 libxslt1-dev \
        libedit-dev libedit2 \
    && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) pdo_mysql mbstring bcmath  pcntl shmop gd soap sockets zip opcache  xsl \
    && pecl install igbinary memcached redis mcrypt xdebug-2.6.0 \
    && docker-php-ext-enable igbinary  memcached redis opcache xdebug \
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

