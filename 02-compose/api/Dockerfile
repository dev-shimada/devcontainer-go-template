FROM --platform=$BUILDPLATFORM golang:1.24.0-bookworm as vscode
ARG TARGETPLATFORM
ARG BUILDPLATFORM
ARG TARGETOS
ARG TARGETARCH
ENV LESSCHARSET utf-8
WORKDIR /workspace
RUN  <<EOF
apt-get update && apt-get install -y vim git unzip less bash-completion jq
go install -v golang.org/x/tools/gopls@latest
go install -v github.com/go-delve/delve/cmd/dlv@latest
go install -v github.com/cweill/gotests/gotests@latest
go install -v github.com/goreleaser/goreleaser/v2@latest
curl -sSfL https://raw.githubusercontent.com/golangci/golangci-lint/master/install.sh | sh -s -- -b $(go env GOPATH)/bin latest
EOF
COPY . /workspace

FROM --platform=$BUILDPLATFORM golang:1.24.0-bookworm AS build
ARG TARGETPLATFORM
ARG BUILDPLATFORM
ARG TARGETOS
ARG TARGETARCH
WORKDIR /workspace
COPY . /workspace
RUN  <<EOF
CGO_ENABLED=0 GOOS=${TARGETOS} GOARCH=${TARGETARCH} go build -o bin/app main.go
EOF

FROM --platform=$BUILDPLATFORM gcr.io/distroless/base-debian12:latest
WORKDIR /app
COPY --chmod=100 --from=build /workspace/bin/app /app/app
ENTRYPOINT [ "/app/app" ]
