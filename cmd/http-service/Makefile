LDFLAGS += -X "github.com/pingcap/tidb-operator/http-service/version.ReleaseVersion=$(shell git describe --tags --dirty="-dev")"
LDFLAGS += -X "github.com/pingcap/tidb-operator/http-service/version.BuildTS=$(shell date -u '+%Y-%m-%d %H:%M:%S')"
LDFLAGS += -X "github.com/pingcap/tidb-operator/http-service/version.GitHash=$(shell git rev-parse HEAD)"
LDFLAGS += -X "github.com/pingcap/tidb-operator/http-service/version.GitBranch=$(shell git rev-parse --abbrev-ref HEAD)"
LDFLAGS += -X "github.com/pingcap/tidb-operator/http-service/version.GoVersion=$(shell go version)"

GO       := $(GOENV) go
GOBUILD  := CGO_ENABLED=0 $(GO) build

build:
	$(GOBUILD) -ldflags '$(LDFLAGS)' -o bin/http-service .

buf:
	GOBIN=$(shell pwd)/bin/ $(GO) install github.com/bufbuild/buf/cmd/buf@v1.26.1

buf-format: buf
	$(shell pwd)/bin/buf format ./idl/api/service.proto -w

buf-lint: buf buf-format
	$(shell pwd)/bin/buf lint ./idl

buf-generate: buf buf-format
	$(shell pwd)/bin/buf generate ./idl