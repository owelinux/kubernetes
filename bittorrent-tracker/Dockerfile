FROM node:latest
 
#RUN apk update && apk upgrade && \
#    apk add python && \
#    rm -rf /var/cache/apk/*

RUN npm config set user 0 && \
    npm config set unsafe-perm true && \
    npm install -g bittorrent-tracker

EXPOSE 8000 8000/udp 
CMD bittorrent-tracker
