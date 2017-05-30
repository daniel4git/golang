FROM alpine/tsinghua:3.6
LABEL maintainer "2214695946@qq.com"

ENV GOLANG_VERSION 1.8.3
ENV GOPATH /go
ENV PATH $GOPATH/bin:/usr/local/go/bin:$PATH

# https://golang.org/issue/14851 (Go 1.8 & 1.7)
# https://golang.org/issue/17847 (Go 1.7)

COPY go1.8.3.src.tar.gz go.tgz

RUN set -eux; \
    ln -sf /usr/share/zoneinfo/Asia/Shanghai  /etc/localtime;\
    apk add --no-cache ca-certificates; \
	apk add --no-cache --virtual .build-deps \
		bash \
		gcc \
		musl-dev \
		openssl \
		go \
	; \
	export \
# set GOROOT_BOOTSTRAP such that we can actually build Go
		GOROOT_BOOTSTRAP="$(go env GOROOT)" \
# ... and set "cross-building" related vars to the installed system's values so that we create a build targeting the proper arch
# (for example, if our build host is GOARCH=amd64, but our build env/image is GOARCH=386, our build needs GOARCH=386)
		GOOS="$(go env GOOS)" \
		GOARCH="$(go env GOARCH)" \
		GO386="$(go env GO386)" \
		GOARM="$(go env GOARM)" \
		GOHOSTOS="$(go env GOHOSTOS)" \
		GOHOSTARCH="$(go env GOHOSTARCH)" \
	; \
	\
    tar -C /usr/local -xzf go.tgz; \
	rm go.tgz; \
    \
	cd /usr/local/go/src; \
	./make.bash; \
	\
	apk del .build-deps; \
	\
	export PATH="/usr/local/go/bin:$PATH"; \
	#go version

    mkdir -p "$GOPATH/src" && chmod -R 777 "$GOPATH"

WORKDIR $GOPATH

COPY go-wrapper /usr/local/bin/
