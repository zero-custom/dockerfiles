FROM golang:alpine AS builder
RUN go install github.com/die-net/dhtproxy@latest

FROM alpine:latest
COPY --from=builder /go/bin/dhtproxy /bin/

EXPOSE 6969
ENTRYPOINT ["/bin/dhtproxy"]