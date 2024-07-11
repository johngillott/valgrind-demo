FROM rust:1.79-bookworm AS build

ENV DEBIAN_FRONTEND=noninteractive

RUN set -eu && \
    apt-get update; \
    apt-get install -y \
    --no-install-recommends;

RUN rustup component add rustfmt;

WORKDIR /src

COPY . .

RUN cargo build --release --package example --bin example

# ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
FROM debian:bookworm-slim AS final

ENV DEBIAN_FRONTEND=noninteractive

RUN set -eu && \
    apt-get update; \
    apt-get install -y \
    --no-install-recommends \
    tini \
    openssl \
    ca-certificates \
    valgrind; \
    useradd svc;

COPY --from=build /src/target/release/example /


RUN chown -R svc /example
USER svc

ENTRYPOINT ["/usr/bin/tini", "--"]

CMD ["valgrind","--leak-check=full", "--show-leak-kinds=all", "-s", "./example"]