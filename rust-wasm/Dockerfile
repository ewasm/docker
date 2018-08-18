# Author: Alex Beregszaszi

FROM rust:1

RUN rustup update

RUN rustup install beta
RUN rustup default stable

RUN rustup target add wasm32-unknown-unknown
RUN rustup component add rustfmt-preview

RUN cargo install wasm-gc
RUN cargo install just
