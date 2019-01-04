FROM alpine:latest
RUN apk --update --no-cache  add bash socat && rm -rf /var/cache/apk/*
ADD run.sh /usr/local/bin/run
ENV PORT=2375
EXPOSE $PORT
ENTRYPOINT ["/usr/local/bin/run"]
