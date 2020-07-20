FROM hexpm/elixir:1.10.4-erlang-23.0.2-alpine-3.12.0

ENV VERSION elixir-1.10.4-erlang-23.0.2-rust-1.41.1-alpine-3.12.0

## Start Rust installation
# Copy from https://github.com/rust-lang/docker-rust/blob/96fd7e1527773900786ae76774e3aa9e21597a19/1.41.1/alpine3.11/Dockerfile
RUN apk add --no-cache \
        ca-certificates \
        gcc

ENV RUSTUP_HOME=/usr/local/rustup \
    CARGO_HOME=/usr/local/cargo \
    PATH=/usr/local/cargo/bin:$PATH \
    RUST_VERSION=1.41.1

RUN set -eux; \
    url="https://static.rust-lang.org/rustup/archive/1.21.1/x86_64-unknown-linux-musl/rustup-init"; \
    wget "$url"; \
    echo "0c86d467982bdf5c4b8d844bf8c3f7fc602cc4ac30b29262b8941d6d8b363d7e *rustup-init" | sha256sum -c -; \
    chmod +x rustup-init; \
    ./rustup-init -y --no-modify-path --profile minimal --default-toolchain $RUST_VERSION; \
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
