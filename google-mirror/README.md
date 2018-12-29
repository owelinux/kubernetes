# 跑在 docker 里的 google 镜像

# 简单两步获得 Google 镜像，使用方法：

```
docker pull docker98k/google-mirror
docker run -d -p 80:80 docker98k/google-mirror
```

默认打开基本的“谷歌搜索”和“谷歌学术”，如需打开或者自定义其他功能，请下载项目文件修改nginx.conf并且自行构建镜像

# 自行构建镜像：

```
git clone https://github.com/GEM7/docker-google-mirror
cd docker-google-mirror
docker build -t google-mirror .
docker run -d -p 80:80 google-mirror
```
如要自定义`nginx.conf`文件，详情参考: [源项目](https://github.com/cuber/ngx_http_google_filter_module/blob/master/README.zh-CN.md)

# 配置https
# 使用其他镜像或者Nginx，Caddy进行反向代理

# 自身实现
1. 编辑Dockerfile文件,注释第22行，并解注释第24、25行
2. 申请域名证书，具体参考: [Nginx 配置 SSL 证书 + 搭建 HTTPS 网站教程](https://docker98k.github.io/2018/09/06/article31-linux-let-ssl/)
```
docker run --rm  -it  \
  -v "$(pwd)/out":/acme.sh  \
  -e CF_Key="xxx" \
  -e CF_Email="xxx" \
  neilpang/acme.sh  --issue --dns dns_cf -d domain.com -d *.domain.com
```
3. 添加自己的域名证书到该目录下

```shell
cd docker-google-mirror
cp ~/fullchain.cer domain.cer
cp ~/domain.com.key domain.key
docker build -t google-mirror .
docker run -d -p 80:80 -p 443:443 google-mirror
```
# Dockerfile
```
FROM alpine:latest
MAINTAINER Yifei Kong <kong@yifei.me>

ENV NGINX_VER 1.14.0

RUN apk add --update git openssl-dev pcre-dev zlib-dev wget build-base && \
    mkdir src && cd src && \
    wget http://nginx.org/download/nginx-${NGINX_VER}.tar.gz && \
    tar xzf nginx-${NGINX_VER}.tar.gz && \
    git clone https://github.com/cuber/ngx_http_google_filter_module && \
    git clone https://github.com/yaoweibin/ngx_http_substitutions_filter_module && \
    cd nginx-${NGINX_VER} && \
    ./configure --prefix=/opt/nginx \
        --with-http_ssl_module \
        --add-module=../ngx_http_google_filter_module \
        --add-module=../ngx_http_substitutions_filter_module && \
    make && make install && \
    apk del git build-base && \
    rm -rf /src && \
    rm -rf /var/cache/apk/*

ADD nginx.conf /opt/nginx/conf/nginx.conf
#如果需要https支持则注释上一行并解注释下两行
#ADD nginx-https.conf /opt/nginx/conf/nginx.conf
#ADD domain.cer domain.key /etc/ssl/private/

EXPOSE 80 443
CMD ["/opt/nginx/sbin/nginx", "-g", "daemon off;"]
```

# 参考
* [https://hub.docker.com/r/rpmdpkg/docker-google-mirror/](https://hub.docker.com/r/rpmdpkg/docker-google-mirror/)
