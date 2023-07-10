FROM alpine:latest AS builder

ENV APK_DEPS alpine-sdk build-base git linux-headers boost-build boost-dev boost-libs boost-static
ENV DHT_REPO_URL https://github.com/arvidn/bootstrap-dht.git

# Package dependencies installed to compile/execute the "bootstrap-dht" binary.
RUN apk add --no-cache $APK_DEPS &&\
    git clone --branch fix-build $DHT_REPO_URL /tmp/bootstrap-dht &&\
    cd /tmp/bootstrap-dht/ &&\
    b2 toolset=gcc runtime-link=static link=static cxxflags="-std=c++11"

FROM scratch
COPY --from=builder /tmp/bootstrap-dht/dht-bootstrap /

EXPOSE 6881
ENTRYPOINT ["/dht-bootstrap"]
