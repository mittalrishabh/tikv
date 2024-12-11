# This is a copy of the https://github.com/PingCAP-QE/artifacts/blob/69874d6687b4d2db7fe10133f7707ed43c045980/dockerfiles/cd/builders/tikv/Dockerfile header

# build requires:
#   - docker >= v20.10
#
# build steps:
#   - git clone --recurse-submodules https://github.com/tikv/tikv.git tikv
#   - docker build -t tikv -f Dockerfile ./tikv

########### stage: builder
FROM quay.io/rockylinux/rockylinux:8.10.20240528-ubi as builder
LABEL org.opencontainers.image.authors "wuhui.zuo@pingcap.com"
LABEL org.opencontainers.image.description "binary builder for TiKV"
LABEL org.opencontainers.image.source "https://github.com/PingCAP-QE/artifacts"

# install packages.
# install packages.
RUN --mount=type=cache,target=/var/cache/dnf \
    dnf upgrade-minimal -y && \
    dnf --enablerepo=powertools install -y \
      make git findutils gcc gcc-c++ cmake curl openssl-devel perl python3 \
      libstdc++-static

# install protoc.
# renovate: datasource=github-release depName=protocolbuffers/protobuf
ARG PROTOBUF_VER=v3.15.8
RUN FILE=$([ "$(arch)" = "aarch64" ] && echo "protoc-${PROTOBUF_VER#?}-linux-aarch_64.zip" || echo "protoc-${PROTOBUF_VER#?}-linux-$(arch).zip"); \
    curl -LO "https://github.com/protocolbuffers/protobuf/releases/download/${PROTOBUF_VER}/${FILE}" && unzip "$FILE" -d /usr/local/ && rm -f "$FILE"

# install rust toolchain
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s - -y --default-toolchain none
ENV PATH /root/.cargo/bin/:$PATH