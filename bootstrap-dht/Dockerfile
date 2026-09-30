FROM alpine:latest AS builder
ENV APK_DEPS alpine-sdk build-base git linux-headers boost-dev boost-libs boost-static patch
ENV DHT_REPO_URL https://github.com/arvidn/bootstrap-dht.git
COPY docker-bootstrap-autodetect-master.patch /tmp/docker-bootstrap-autodetect-master.patch

# Package dependencies installed to compile/execute the "bootstrap-dht" binary.
RUN apk add --no-cache $APK_DEPS &&\
    git clone --branch master $DHT_REPO_URL /tmp/bootstrap-dht &&\
    cd /tmp/bootstrap-dht/ &&\
    git rev-parse HEAD &&\
    patch -p1 < /tmp/docker-bootstrap-autodetect-master.patch &&\
    g++ -std=c++14 -O2 -static -Isrc src/main.cpp src/bdecode.cpp src/city.cc src/ip_set.cpp -lboost_system -pthread -o dht-bootstrap
FROM scratch
COPY --from=builder /tmp/bootstrap-dht/dht-bootstrap /
EXPOSE 6881
ENTRYPOINT ["/dht-bootstrap"]
