VERSION 0.6
FROM golang:1.20.1-alpine3.17
WORKDIR /workspace

test:
	ENV khulnasoft_ROOT_DIR=/root/khulnasoftproj-khulnasoft
	RUN apk add curl bash
	RUN curl -sSfL -O https://raw.githubusercontent.com/khulnasoftproj/khulnasoft-installer/v2.1.2/khulnasoft-installer
	RUN echo "411caf1b5fcef4f5e74aa2a9fe99182ea13ab93ecd8ed4a983a7cff9f08edab9  khulnasoft-installer" | sha256sum -c
	RUN chmod +x khulnasoft-installer
	RUN ./khulnasoft-installer -v v2.10.1
	COPY khulnasoft/earthly-test.yaml khulnasoft.yaml
	COPY khulnasoft-policy.yaml khulnasoft-policy.yaml
	RUN echo "- import: pkg.yaml" >> khulnasoft.yaml
	ARG pkg
	ARG os=linux
	ARG arch=amd64
	ENV khulnasoft_GOOS=$os
	ENV khulnasoft_GOARCH=$arch
	ENV khulnasoft_LOG_COLOR=always
	ENV khulnasoft_POLICY_CONFIG=/workspace/khulnasoft-policy.yaml
	ENV PATH=$khulnasoft_ROOT_DIR/bin:$PATH
	COPY pkgs/$pkg/pkg.yaml pkg.yaml
	COPY pkgs/$pkg/registry.yaml registry.yaml
	RUN "$khulnasoft_ROOT_DIR/bin/khulnasoft" i --test
