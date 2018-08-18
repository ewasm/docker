# Author: Alex Beregszaszi

# Building kcov first.
FROM rust:1 as builder

# Install dependencies
RUN apt-get install binutils-dev wget

# Build kcov
RUN wget -O - https://github.com/SimonKagstrom/kcov/archive/v35.tar.gz | tar zxvf -
WORKDIR /build
RUN cmake /kcov-35 -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_BUILD_TYPE=Release
RUN make -j $(nproc) && make install

# Create our derived image off the official rust one.
FROM rust:1

RUN rustup update

RUN rustup install beta
RUN rustup default stable

RUN rustup target add wasm32-unknown-unknown
RUN rustup component add rustfmt-preview

RUN cargo install wasm-gc
RUN cargo install just

COPY --from=builder /usr/bin/kcov /usr/bin
