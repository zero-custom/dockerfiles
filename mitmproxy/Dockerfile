FROM mitmproxy/mitmproxy AS entrypoint

FROM rust:alpine AS mitmproxy_rs_builder
RUN apk add --no-cache build-base git py3-pip py3-ruamel.yaml.clib python3-dev &&\
    pip install --no-cache --break-system-packages maturin[patchelf] &&\
    git clone --branch 0.6.3 https://github.com/mitmproxy/mitmproxy_rs.git &&\
    cd mitmproxy_rs/mitmproxy-rs/ &&\
    maturin build --release &&\
    mkdir /wheels && cd /wheels &&\
    pip wheel ruamel.yaml.clib

FROM python:alpine AS wheelbuilder
WORKDIR /wheels
COPY --from=mitmproxy_rs_builder /mitmproxy_rs/target/wheels/*.whl /wheels
COPY --from=mitmproxy_rs_builder /wheels/*.whl /wheels
RUN apk add --repository=https://dl-cdn.alpinelinux.org/alpine/edge/main --no-cache bsd-compat-headers build-base openssl-dev &&\
    pip wheel --no-cache-dir --find-links /wheels --wheel-dir /wheels mitmproxy

FROM alpine:latest
RUN mkdir -p /wheels/
COPY --from=wheelbuilder /wheels/mitmproxy*.whl /wheels/aioquic*.whl /wheels/pylsqpack*.whl /wheels/ruamel*.whl /wheels
RUN apk add --no-cache python3 py3-pip libgcc shadow bash &&\
    apk add --repository=https://dl-cdn.alpinelinux.org/alpine/edge/testing --no-cache gosu &&\
    useradd -mU mitmproxy &&\
    pip install --break-system-packages --no-cache-dir --find-links /wheels mitmproxy &&\
    rm -rf /wheels

VOLUME /home/mitmproxy/.mitmproxy

COPY --from=entrypoint /usr/local/bin/docker-entrypoint.sh /usr/local/bin/
ENTRYPOINT ["docker-entrypoint.sh"]

EXPOSE 8080 8081

CMD ["mitmproxy"]
