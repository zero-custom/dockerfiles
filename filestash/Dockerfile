FROM machines/filestash:latest AS entrypoint

FROM alpine:latest
WORKDIR /app/
COPY --from=entrypoint /app/ .
RUN apk add --no-cache nano shadow gcompat ffmpeg &&\
    useradd -U filestash &&\
    chown -R filestash:filestash /app/ &&\
    find /app/data/ -type d -exec chmod 770 {} \; &&\
    find /app/data/ -type f -exec chmod 760 {} \; &&\
    chmod 730 /app/filestash

USER filestash
CMD ["/app/filestash"]
EXPOSE 8334
