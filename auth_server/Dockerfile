FROM golang:1.24-alpine3.22 AS build

ARG VERSION
ENV VERSION="${VERSION}"
ARG BUILD_ID
ENV BUILD_ID="${BUILD_ID}"
ARG CGO_EXTRA_CFLAGS

# 换源
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.tuna.tsinghua.edu.cn/g' /etc/apk/repositories

RUN go env -w GO111MODULE=on && go env -w  GOPROXY=https://goproxy.cn,direct

RUN apk add -U --no-cache ca-certificates make git gcc musl-dev binutils-gold

COPY . /build

WORKDIR /build

RUN make build

FROM alpine:3.22

COPY --from=build /build/auth_server /docker_auth/

COPY --from=build /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/

ENTRYPOINT ["/docker_auth/auth_server"]

CMD ["/config/auth_config.yml"]

EXPOSE 5001
