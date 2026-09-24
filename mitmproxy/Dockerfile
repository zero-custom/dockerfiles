# pinned to official container release by default
ARG MITMPROXY_VERSION=12.2.3
FROM mitmproxy/mitmproxy AS entrypoint

FROM python:3.14-alpine AS wheelbuilder
ARG MITMPROXY_VERSION

ENV CARGO_HOME=/usr/local/cargo RUSTUP_HOME=/usr/local/rustup PATH=/usr/local/cargo/bin:$PATH
RUN apk add --no-cache build-base libffi-dev openssl-dev curl \
    && apk add --no-cache --repository=https://dl-cdn.alpinelinux.org/alpine/edge/main llvm23-dev \
    && curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs \
        | sh -s -- -y --default-toolchain stable --profile minimal \
    && rustup component add rust-src \
    && rustup toolchain install nightly --profile minimal --component rust-src \
    && export PATH="/usr/lib/llvm23/bin:$PATH" \
    && RUSTFLAGS="-C target-feature=-crt-static" cargo install --locked bpf-linker@0.11.1 \
    && pip install --no-cache-dir wheel \
    && pip wheel --no-cache-dir --wheel-dir /wheels mitmproxy==${MITMPROXY_VERSION}

FROM python:3.14-alpine
ARG MITMPROXY_VERSION

RUN apk add --no-cache bash libgcc nano shadow \
    && apk add --no-cache --repository=https://dl-cdn.alpinelinux.org/alpine/edge/testing gosu \
    && useradd -mU mitmproxy \
    && mkdir -p /home/mitmproxy/.mitmproxy \
    && chown mitmproxy:mitmproxy /home/mitmproxy/.mitmproxy

COPY --from=wheelbuilder /wheels /wheels
RUN pip install --no-cache-dir --no-index --find-links=/wheels mitmproxy==${MITMPROXY_VERSION} \
    && rm -rf /wheels

VOLUME /home/mitmproxy/.mitmproxy

COPY --from=entrypoint /usr/local/bin/docker-entrypoint.sh /usr/local/bin/
ENTRYPOINT ["docker-entrypoint.sh"]

EXPOSE 8080 8081

CMD ["mitmproxy"]
