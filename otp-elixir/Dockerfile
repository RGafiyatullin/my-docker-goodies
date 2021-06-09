### ### ### ### ### ### ### ### ###
FROM alpine:latest AS otp_elixir_prep

ARG OTP_VER=23.3
ARG ELIXIR_VER=1.11.3

RUN \
    apk add \
        autoconf \
        bash \
        curl \
        gcc \
        git \
        g++ \
        make \
        musl-dev \
        ncurses-dev \
        openssl-dev \
        perl

RUN mkdir -p /opt/kerl && \
    curl https://raw.githubusercontent.com/kerl/kerl/master/kerl > /opt/kerl/kerl && \
    chmod +x /opt/kerl/kerl && \
    /opt/kerl/kerl update releases && \
    /opt/kerl/kerl build ${OTP_VER} && \
    /opt/kerl/kerl install ${OTP_VER} /opt/otp

RUN \
    source /opt/otp/activate && \
    curl -sSL https://raw.githubusercontent.com/taylor/kiex/master/install | bash -s && \
    ${HOME}/.kiex/bin/kiex install ${ELIXIR_VER} && \
    cp -R ${HOME}/.kiex/elixirs/elixir-${ELIXIR_VER} /opt/elixir && \
    echo "export ELIXIR_VERSION=\"${ELIXIR_VER}\"" > /opt/elixir/activate && \
    echo "export PATH=\"/opt/elixir/bin:\$PATH\"" >> /opt/elixir/activate

FROM alpine:latest

RUN \
    apk add \
        autoconf \
        bash \
        curl \
        gcc \
        git \
        g++ \
        make \
        musl \
        ncurses \
        openssl \
        perl

COPY --from=otp_elixir_prep /opt/otp /opt/otp
COPY --from=otp_elixir_prep /opt/elixir /opt/elixir

RUN mkdir -p /src
WORKDIR /src

