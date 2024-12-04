
FROM alpine:latest

ARG VERSION='2.1.5'
ARG TARGETARCH

RUN apk add --no-cache curl bind-tools

RUN \
    if [ "$TARGETARCH" = "amd64" ]; then \
    curl --silent -L https://github.com/DNSCrypt/dnscrypt-proxy/releases/download/${VERSION}/dnscrypt-proxy-linux_x86_64-${VERSION}.tar.gz > /tmp/dnscrypt-proxy.tar.gz; \
    elif [ "$TARGETARCH" = "arm64" ]; then \
    curl --silent -L https://github.com/DNSCrypt/dnscrypt-proxy/releases/download/${VERSION}/dnscrypt-proxy-linux_arm-${VERSION}.tar.gz > /tmp/dnscrypt-proxy.tar.gz; \
    else \
    echo "Unsupported architecture: $TARGETARCH"; \
    exit 1; \
    fi


RUN mkdir /opt/dnscrypt-proxy && \
    cd /opt/dnscrypt-proxy && \
    tar xvzf /tmp/dnscrypt-proxy.tar.gz --strip-components 1 && \
    rm /tmp/dnscrypt-proxy.tar.gz


ENTRYPOINT ["/opt/dnscrypt-proxy/dnscrypt-proxy", "-config", "/etc/dnscrypt-proxy/dnscrypt-proxy.toml"]

HEALTHCHECK --interval=300s CMD dig . @localhost > /dev/null || exit 1