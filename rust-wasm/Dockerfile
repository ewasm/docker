# Author: Alex Beregszaszi

# Building kcov first.
FROM rust:1 as builder

# Install dependencies
RUN apt-get update -y && apt-get -y install binutils-dev wget cmake libelf-dev libdwarf-dev libdw-dev libiberty-dev

# Build kcov
RUN wget -O - https://github.com/SimonKagstrom/kcov/archive/v35.tar.gz | tar zxvf -
WORKDIR /build
RUN cmake /kcov-35 -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_BUILD_TYPE=Release
RUN make -j $(nproc) && make install

# Create our derived image off the official rust one.
FROM rust:1

RUN apt update -qy \
	&& apt-get install -y --no-install-recommends \
		# For kcov:
		libdw1 \
		# For tarpaulin:
		libssl-dev pkg-config cmake zlib1g-dev \
	&& rm -rf /var/lib/apt/lists/*

RUN rustup update \
	&& rustup install beta \
	&& rustup default stable \
	&& rustup target add wasm32-unknown-unknown \
	&& rustup component add rustfmt-preview
RUN cargo install wasm-gc \
	&& cargo install just \
	&& RUSTFLAGS="--cfg procmacro2_semver_exempt" cargo install cargo-tarpaulin \
	&& rm -rf /usr/local/cargo/registry

# Install codecov support
COPY --from=builder /usr/bin/kcov /usr/bin
ADD rust-codecov /usr/bin/rust-codecov
RUN wget -O - -q "https://codecov.io/bash" > /usr/bin/codecov \
	&& chmod +x /usr/bin/codecov \
	&& chmod +x /usr/bin/rust-codecov
