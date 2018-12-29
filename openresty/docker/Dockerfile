FROM owelinux/centos-supervisor:7.4

MAINTAINER owelinux owelinux@gmail.com

# 安装依赖
RUN  \
   yum -y install epel-release wget perl \
&& yum -y install openssl openssl-devel  gcc C pcre pcre-devel bzip2-devel libcurl-devel libpng-devel libmcrypt-devel libxml2-devel readline-devel \

#建立目录
&& mkdir -p /application/tools && cd /application/tools \

#编译openresty
&& wget https://openresty.org/download/openresty-1.13.6.1.tar.gz \
&& tar zxvf openresty-1.13.6.1.tar.gz \
&& cd openresty-1.13.6.1 \
&& ./configure  \
--prefix=/application/openresty \
--with-luajit \  
--without-http_redis2_module  \
--with-http_iconv_module  \
--with-http_stub_status_module   \
--with-pcre  \
--user=www \
--group=www \
--with-http_v2_module  \
--with-http_ssl_module  \
--with-ipv6 \
--with-http_gzip_static_module  \
--with-http_realip_module \
--with-pcre-jit \
&& gmake -j4 && gmake install \ 
&& gmake clean \
&& ln -s /application/openresty/nginx/  /application/nginx \
&& ln -s /application/openresty/nginx/sbin/nginx  /usr/sbin/nginx \

&& rm -rf /application/tools \
&& yum clean all \

&& groupadd  www \
&& useradd -u 80 -g www  -s /bin/bash -d /data/www/ www \

# Prepare dummy SSL certificates
&& mkdir -p /application/nginx/ssl \
&& openssl genrsa -out /application/nginx/ssl/dummy.key 2048  \
&& openssl req -new -key /application/nginx/ssl/dummy.key -out /application/nginx/ssl/dummy.csr -subj "/C=GB/L=London/O=Company Ltd/CN=docker" \
&& openssl x509 -req -days 3650 -in /application/nginx/ssl/dummy.csr -signkey /application/nginx/ssl/dummy.key -out /application/nginx/ssl/dummy.crt 

ADD container-files/config /config
ADD container-files/etc/supervisor.d /etc/supervisor.d
ADD container-files/etc/nginx /application/nginx

ENV \
  NGINX_GENERATE_DEFAULT_VHOST=false \
  STATUS_PAGE_ALLOWED_IP=127.0.0.1

EXPOSE 80 443
