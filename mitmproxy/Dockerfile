FROM mitmproxy/mitmproxy AS entrypoint

FROM python:alpine AS wheelbuilder
RUN mkdir /wheels &&\
    pip wheel --no-cache-dir --wheel-dir /wheels mitmproxy

FROM alpine:latest
RUN mkdir -p /wheels/
COPY --from=wheelbuilder /wheels/mitmproxy*.whl /wheels/aioquic*.whl /wheels/pylsqpack*.whl /wheels
RUN apk add --repository=https://dl-cdn.alpinelinux.org/alpine/edge/testing --no-cache python3 py3-pip libgcc gosu nano shadow bash &&\
    useradd -mU mitmproxy &&\
    pip install --break-system-packages --no-cache-dir --find-links=/wheels mitmproxy &&\
    rm -rf /wheels

VOLUME /home/mitmproxy/.mitmproxy

COPY --from=entrypoint /usr/local/bin/docker-entrypoint.sh /usr/local/bin/
ENTRYPOINT ["docker-entrypoint.sh"]

EXPOSE 8080 8081

CMD ["mitmproxy"]
