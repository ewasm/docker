# ewasm/rust

Builds off the [official rust docker](https://hub.docker.com/_/rust/).

Adds `rustfmt`, wasm32 support, `wasm-gc`, `just`, `kcov`, `tarpaulin` and codecov.io.

Available as [ewasm/rust-wasm](https://hub.docker.com/ewasm/rust-wasm/).

Need to run with `--security-opt seccomp=unconfined` option to enable `kcov` or `tarpaulin` support.