# Author: Alex Beregszaszi

# Building kcov first.
FROM rust:1 as builder

# Install dependencies
RUN apt update -y && apt-get -y install binutils-dev wget cmake libelf-dev libdwarf-dev libdw-dev libiberty-dev

# Build kcov
RUN wget -O - https://github.com/SimonKagstrom/kcov/archive/v35.tar.gz | tar zxvf -
WORKDIR /build
RUN cmake /kcov-35 -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_BUILD_TYPE=Release
RUN make -j $(nproc) && make install

# Create our derived image off the official rust one.
FROM rust:1

RUN apt update -y

RUN rustup update

RUN rustup install beta
RUN rustup default stable

RUN rustup target add wasm32-unknown-unknown
RUN rustup component add rustfmt-preview

RUN cargo install wasm-gc
RUN cargo install just

RUN apt-get -y install libdw1
COPY --from=builder /usr/bin/kcov /usr/bin

# Install codecov support
RUN wget -O - -q "https://codecov.io/bash" > /usr/bin/codecov
RUN chmod +x /usr/bin/codecov

ADD rust-codecov /usr/bin/rust-codecov
RUN chmod +x /usr/bin/rust-codecov
