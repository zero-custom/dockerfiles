FROM mitmproxy/mitmproxy AS entrypoint

FROM alpine:latest
RUN apk add --no-cache nano shadow bash py3-pip &&\
    apk add --repository=https://dl-cdn.alpinelinux.org/alpine/edge/testing --no-cache gosu &&\
    apk add --repository=https://dl-cdn.alpinelinux.org/alpine/edge/community --no-cache mitmproxy &&\
    useradd -mU mitmproxy

VOLUME /home/mitmproxy/.mitmproxy

COPY --from=entrypoint /usr/local/bin/docker-entrypoint.sh /usr/local/bin/
ENTRYPOINT ["docker-entrypoint.sh"]

EXPOSE 8080 8081

CMD ["mitmproxy"]
