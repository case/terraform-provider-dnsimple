TEST         ?= ./...
WEBSITE_REPO = github.com/hashicorp/terraform-website
PKG_NAME     = dnsimple
HOSTNAME     = registry.terraform.io
NAMESPACE    = dnsimple
BINARY       = terraform-provider-${PKG_NAME}
VERSION      = $(shell git describe --tags --always | cut -c 2-)
OS_ARCH      = darwin_amd64

default: build

build: fmtcheck
	go install

install: build
	mkdir -p ~/.terraform.d/plugins/${HOSTNAME}/${NAMESPACE}/${PKG_NAME}/${VERSION}/${OS_ARCH}
	mv ${GOPATH}/bin/${BINARY} ~/.terraform.d/plugins/${HOSTNAME}/${NAMESPACE}/${PKG_NAME}/${VERSION}/${OS_ARCH}

test: fmtcheck
	go test $(TEST) $(TESTARGS) -timeout=5m

testacc: fmtcheck
	TF_ACC=1 go test $(TEST) -v $(TESTARGS) -timeout 120m

fmt:
	gofmt -s -w ./$(PKG_NAME)

fmtcheck:
	@sh -c "'$(CURDIR)/scripts/gofmtcheck.sh'"

errcheck:
	@sh -c "'$(CURDIR)/scripts/errcheck.sh'"

website:
	@echo "Use this site to preview markdown rendering: https://registry.terraform.io/tools/doc-preview"

.PHONY: build test testacc fmt fmtcheck errcheck website
