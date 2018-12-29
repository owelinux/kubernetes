FROM nginx:1.15.3-alpine

MAINTAINER docker98k docker98k@gmail.com

ARG TZ="Asia/Shanghai"

ENV TZ ${TZ}

RUN apk upgrade --update \
    && apk add bash tzdata \
    && ln -sf /usr/share/zoneinfo/${TZ} /etc/localtime \
    && echo ${TZ} > /etc/timezone \
    && rm -rf /var/cache/apk/*

COPY vip /usr/share/nginx/html 

CMD ["nginx", "-g", "daemon off;"]
