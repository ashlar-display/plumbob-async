# Development Setup

**Requirements:** Rust 1.85+ (stable). Install via [rustup](https://rustup.rs/).

## Clone and build

```sh
git clone https://github.com/ashlar-display/plumbob-async.git
cd plumbob-async
cargo build
```

## Running checks

```sh
cargo fmt --check
cargo clippy --all-targets --features std -- -D warnings
cargo clippy --all-targets --features alloc -- -D warnings
cargo clippy --all-targets --no-default-features -- -D warnings
cargo rustdoc --all-features -- -D warnings
```

## Running tests

```sh
rustup target add thumbv7em-none-eabi                # once, for the no_std builds

cargo test --features std                            # std (default for tests)
cargo test --features alloc                          # alloc only
cargo test --no-default-features                     # tests without alloc
cargo build --no-default-features --features alloc --target thumbv7em-none-eabi
cargo build --no-default-features --target thumbv7em-none-eabi
```

The `no_std` builds target `thumbv7em-none-eabi`, as CI does: on the host, `std` is
always available, so a host build cannot catch a dependency that enables it. The README's
examples compile as doctests with the others.

## Checking the MSRV

The MSRV is `rust-version` in `Cargo.toml`. CI checks it with
[`cargo-msrv`](https://github.com/foresterre/cargo-msrv), which runs the checks on that
toolchain:

```sh
cargo install cargo-msrv --version 0.19.6 --locked
cargo msrv verify -- cargo test --locked --features std
cargo msrv verify -- cargo test --locked --features alloc
cargo msrv verify -- cargo test --locked --no-default-features
cargo msrv verify --target thumbv7em-none-eabi \
  -- cargo build --locked --no-default-features --features alloc --target thumbv7em-none-eabi
cargo msrv verify --target thumbv7em-none-eabi \
  -- cargo build --locked --no-default-features --target thumbv7em-none-eabi
```

## Measuring coverage

Coverage requires [`cargo-llvm-cov`](https://github.com/taiki-e/cargo-llvm-cov):

```sh
cargo install cargo-llvm-cov --locked
cargo llvm-cov --locked --features std
```

The current baseline is stored in `.coverage-baseline`. CI fails if coverage drops more
than 0.1 percentage points below it. On pushes to `main` or `develop`, an improvement
automatically opens a `ci/coverage-ratchet` PR to commit the new baseline.
