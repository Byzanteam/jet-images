FROM hexpm/elixir:1.12.1-erlang-24.0.2-alpine-3.13.3

ENV VERSION elixir-1.12.1-erlang-24.0.2-rust-1.49.0-alpine-3.13.3

## Start Rust installation
# Copy from https://github.com/rust-lang/docker-rust/blob/a035f6fa1f14754bab8f34376c386fafc831b652/1.49.0/buster/Dockerfile
RUN apk add --no-cache \
        ca-certificates \
        gcc

ENV RUSTUP_HOME=/usr/local/rustup \
    CARGO_HOME=/usr/local/cargo \
    PATH=/usr/local/cargo/bin:$PATH \
    RUST_VERSION=1.49.0

RUN set -eux; \
    apkArch="$(apk --print-arch)"; \
    case "$apkArch" in \
        x86_64) rustArch='x86_64-unknown-linux-musl'; rustupSha256='05c5c05ec76671d73645aac3afbccf2187352fce7e46fc85be859f52a42797f6' ;; \
        aarch64) rustArch='aarch64-unknown-linux-musl'; rustupSha256='6a8a480d8d9e7f8c6979d7f8b12bc59da13db67970f7b13161ff409f0a771213' ;; \
        *) echo >&2 "unsupported architecture: $apkArch"; exit 1 ;; \
    esac; \
    url="https://static.rust-lang.org/rustup/archive/1.23.1/${rustArch}/rustup-init"; \
    wget "$url"; \
    echo "${rustupSha256} *rustup-init" | sha256sum -c -; \
    chmod +x rustup-init; \
    ./rustup-init -y --no-modify-path --profile minimal --default-toolchain $RUST_VERSION --default-host ${rustArch}; \
    rm rustup-init; \
    chmod -R a+w $RUSTUP_HOME $CARGO_HOME; \
    rustup --version; \
    cargo --version; \
    rustc --version;

## Finish Rust installation

RUN set -xe \
      && apk update \
      && apk upgrade --no-cache \
      && apk add --no-cache git build-base \
      && mix local.rebar --force \
      && mix local.hex --force
