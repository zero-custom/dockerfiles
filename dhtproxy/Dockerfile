FROM golang:alpine AS builder
RUN go install github.com/zero-custom/dhtproxy@latest

FROM alpine:latest
RUN mkdir -p /data
VOLUME /data
COPY --from=builder /go/bin/dhtproxy /bin/

EXPOSE 6969/tcp
EXPOSE 6881/udp
CMD ["/bin/dhtproxy", "-listen=:6969", "-dhtPortUDP=6881", "-dhtNodesFile=/data/dht.nodes"]
