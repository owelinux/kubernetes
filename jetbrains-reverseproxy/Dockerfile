FROM alpine:latest 
MAINTAINER ilanyu <lanyu19950316@gmail.com> 
# http://blog.lanyus.com/archives/317.html
ENV l 0.0.0.0:8888 
ENV r http://idea.lanyus.com:80 

RUN apk add --no-cache wget \ 
  && wget -P /usr/bin/ https://github.com/ilanyu/ReverseProxy/releases/download/v1.4/ReverseProxy_linux_amd64  \
  && chmod a+x /usr/bin/ReverseProxy_linux_amd64

EXPOSE 8888 
CMD ["sh", "-c", "/usr/bin/ReverseProxy_linux_amd64 -l $l -r $r"]