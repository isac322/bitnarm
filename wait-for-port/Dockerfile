FROM --platform=${BUILDPLATFORM} golang:alpine as builder
ARG TARGETOS
ARG TARGETARCH

ARG VERSION
ENV CGO_ENABLED=0

WORKDIR /app

RUN apk add --no-cache --update upx
RUN wget -qO- https://github.com/bitnami/wait-for-port/archive/refs/tags/${VERSION}.tar.gz | tar zx --strip-components=1 -C .
RUN --mount=type=cache,target=/root/.cache/go-build \
    --mount=type=cache,target=/go/pkg \
    GOOS=${TARGETOS:-linux} GOARCH=${TARGETARCH} go build -o main -ldflags="-w -s"
RUN upx --ultra-brute main

FROM gcr.io/distroless/static:nonroot
USER 65532:65532
ENTRYPOINT ["wait-for-port"]

COPY --from=builder /app/main /usr/local/bin/wait-for-port
