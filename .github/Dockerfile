# build stage
FROM golang:alpine AS build
RUN apk update && apk add git
ADD . /src
WORKDIR /src
ENV CGO_ENABLED=0
RUN go build \
    -ldflags "-X github.com/jpillora/chisel/share.BuildVersion=$(git describe --abbrev=0 --tags)" \
    -o /tmp/asterctl
# run stage
FROM scratch
LABEL maintainer="dev@jpillora.com"
COPY --from=build /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/
WORKDIR /app
COPY --from=build /tmp/asterctl /app/asterctl
ENTRYPOINT ["/app/asterctl"]
