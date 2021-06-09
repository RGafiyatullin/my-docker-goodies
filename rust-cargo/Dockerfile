FROM rust:alpine

ARG BUILD_RELEASE_OPT=''
ARG BUILD_OPTS=''

USER root

RUN apk add --no-cache git openssh musl-dev openssl-dev
RUN rustup toolchain uninstall stable &&\
    rustup toolchain install stable &&\
    rustup update stable &&\
    rustup component add clippy &&\
    export USER=root &&\
    mkdir /src \
    mkdir /src/.cargo

WORKDIR /src
