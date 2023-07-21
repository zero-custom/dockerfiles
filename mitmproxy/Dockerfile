FROM mitmproxy/mitmproxy AS entrypoint
FROM alpine:latest
RUN apk add --repository=https://dl-cdn.alpinelinux.org/alpine/edge/testing --no-cache mitmproxy gosu nano shadow bash &&\
    useradd -mU mitmproxy

VOLUME /home/mitmproxy/.mitmproxy

COPY --from=entrypoint /usr/local/bin/docker-entrypoint.sh /usr/local/bin/
ENTRYPOINT ["docker-entrypoint.sh"]

EXPOSE 8080 8081

CMD ["mitmproxy"]