FROM owelinux/openresty:1.13.6.1
MAINTAINER owelinux <owelinux@gmail.com>

ENV \
  STATUS_PAGE_ALLOWED_IP=127.0.0.1 \
  PHP_VERSION=7.1.2 \
  REDIS_VERSION=3.1.2 \
  YAF_VERSION=3.0.5 \
  SEASLOG_VERSION=1.6.9 \
  AMQP_VERSION=1.9.1

# Add install scripts needed by the next RUN command
#ADD container-files/etc/yum.repos.d/* /etc/yum.repos.d/

# 安装依赖
RUN \
   yum update -y \
&& yum -y install  epel-release \
&& yum -y install  gcc gcc-c++   C  autoconf  make mcrypt  mhash zlib zlib-devel pcre pcre-devel  libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel libxml2 libxml2-devel glibc glibc-devel glib2 glib2-devel bzip2 bzip2-devel ncurses ncurses-devel curl curl-devel e2fsprogs e2fsprogs-devel krb5 krb5-devel openssl openssl-devel openldap openldap-devel nss_ldap openldap-clients openldap-servers libxslt libxslt-devel libmcrypt libmcrypt-devel libpng12 libpng12-devel libcurl  libcurl-devel readline-devel libXpm-devel gmp gmp-devel  mysql-devel unixODBC unixODBC-devel pspell-devel net-snmp net-snmp-devel  \

#建立目录
&& mkdir -p /application/tools && cd /application/tools \
&& wget http://cn2.php.net/distributions/php-${PHP_VERSION}.tar.gz \
&& tar -zxvf php-${PHP_VERSION}.tar.gz \ 
&& cd php-${PHP_VERSION} \
&& ./configure   \
--prefix=/application/php7   \
--with-mysqli=mysqlnd   \
--with-pdo-mysql=mysqlnd   \
--with-iconv   \
--with-iconv-dir=/usr/local/libiconv   \
--with-freetype-dir   \
--with-pear   \
--with-fpm-user=www   \
--with-fpm-group=www   \
--with-jpeg-dir   \
--with-png-dir   \
--with-zlib   \
--with-zlib-dir   \
--with-bz2    \
--with-xsl   \
--with-xmlrpc   \
--with-mhash   \
--with-mcrypt   \
--with-gd    \
--with-openssl  \
--with-libxml-dir \
--with-readline   \
--with-gettext   \
--with-pcre-regex   \
--with-curl   \
--disable-rpath   \
--disable-ipv6    \
--disable-debug   \
--enable-xml   \
--enable-bcmath   \
--enable-shmop   \
--enable-sysvsem   \
--enable-sysvmsg   \
--enable-sysvshm   \
--enable-mbregex   \
--enable-mysqlnd   \
--enable-fpm   \
--enable-mbstring   \
--enable-gd-native-ttf   \
--enable-pcntl   \
--enable-sockets   \
--enable-soap   \
--enable-short-tags   \
--enable-static   \
--enable-ftp   \
--enable-opcache=yes   \
--enable-json   \
--enable-zip  \
--enable-exif   \
--enable-inline-optimization \
&& make -j4 && make install  \
&& /application/php7/bin/pear install channel://pear.php.net/PHP_Archive-0.12.0 \
&& make -j4 && make install && make clean \

#安装扩展依赖包
&& cd /application/tools \
&& wget https://sourceforge.net/projects/re2c/files/0.16/re2c-0.16.tar.gz \
&& tar zxf re2c-0.16.tar.gz && cd re2c-0.16 \
&& ./configure \
&& make -j4 && make install \

#安装yaf扩展
&& cd /application/tools \
&& wget https://pecl.php.net/get/yaf-${YAF_VERSION}.tgz \
&& tar -zxvf yaf-${YAF_VERSION}.tgz \
&& cd yaf-${YAF_VERSION} \
&& /application/php7/bin/phpize \
&& ./configure --with-php-config=/application/php7/bin/php-config \
&& make -j4 && make install \

#安装redis扩展
&& yum -y install unzip \
&& cd /application/tools \
&& wget https://github.com/phpredis/phpredis/archive/develop.zip \
&& unzip develop.zip \
&& cd phpredis-develop \
&& /application/php7/bin/phpize \
&& ./configure --with-php-config=/application/php7/bin/php-config \
&& make -j4 && make install \

#安装seaslog扩展
&& cd /application/tools \
&& wget https://pecl.php.net/get/SeasLog-${SEASLOG_VERSION}.tgz \
&& tar -zxvf SeasLog-${SEASLOG_VERSION}.tgz \
&& cd SeasLog-${SEASLOG_VERSION} \
&& /application/php7/bin/phpize \
&& ./configure --with-php-config=/application/php7/bin/php-config \
&& make -j4 && make install \

#安装amqp扩展
&& yum -y install cmake \
&& cd /application/tools \
&& wget https://github.com/alanxz/rabbitmq-c/releases/download/v0.8.0/rabbitmq-c-0.8.0.tar.gz \
&& tar -zxvf rabbitmq-c-0.8.0.tar.gz && cd rabbitmq-c-0.8.0 \
&& ./configure --prefix=/usr/local/rabbitmq-c \
&& make -j4 && make install \

&& cd /application/tools \
&& wget https://pecl.php.net/get/amqp-${AMQP_VERSION}.tgz \
&& tar -zxvf amqp-${AMQP_VERSION}.tgz \
&& cd amqp-${AMQP_VERSION} \
&& /application/php7/bin/phpize \
&& ./configure --with-php-config=/application/php7/bin/php-config --with-amqp --with-librabbitmq-dir=/usr/local/rabbitmq-c/  \
&& make -j4 && make install \

&& echo "export PATH=/application/php7/bin:/application/php7/sbin:\$PATH" >> /etc/profile.d/php70-paths.sh \
&& ln -sfF /application/php7/bin/{pear,pecl,phar,php,php-cgi,php-config,phpize} /usr/local/bin/. \
&& ln -sfF /application/php7/sbin/php-fpm /usr/local/sbin/. \
&& php --version \

# 复制配置文件
&& mkdir -p /application/php7/etc/php.d \
&& \cp /application/tools/php-${PHP_VERSION}/php.ini-production  /application/php7/etc/php.d/php.ini \
&& \cp /application/php7/etc/php-fpm.conf.default /application/php7/etc/php-fpm.conf \
&& \cp /application/php7/etc/php-fpm.d/www.conf.default  /application/php7/etc/php-fpm.d/www.conf \
&& echo 'PHP 7 installed.' \

&& curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
&& chown www /usr/local/bin/composer && composer --version \

&& yum clean all && rm -rf /tmp/yum* && rm -rf /application/tools

ADD container-files/config /config
ADD container-files/etc/supervisor.d /etc/supervisor.d
ADD container-files/etc/nginx /application/nginx
ADD container-files/etc/yum.repos.d /etc/yum.repos.d
ADD container-files/etc/php/lib /application/php7/lib
ADD container-files/etc/php/etc /application/php7/etc
