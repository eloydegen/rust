# Rust with AVR support

[![Gitter](https://img.shields.io/gitter/room/nwjs/nw.js.svg)](https://gitter.im/avr-rust)

This project adds support for the [AVR](https://en.wikipedia.org/wiki/Atmel_AVR)
microcontroller to Rust.

It uses the [AVR-LLVM backend](http://llvm.org/viewvc/llvm-project/llvm/trunk/lib/Target/AVR/).

This will compile Rust with AVR support. This will not create a
fully-fledged cross-compiler, however, as it does not compile any libraries
such as `libcore` or `liblibc`. To do this, the `--target=avr-unknown-unknown`
flag must be passed to `configure`, which is not fully supported yet due to bugs.

``` bash
# Grab the avr-rust sources
git clone https://github.com/avr-rust/rust.git

# Create a directory to place built files in
mkdir build && cd build

# Generate Makefile using settings suitable for an experimental compiler
../rust/configure \
  --enable-debug \
  --disable-docs \
  --enable-llvm-assertions \
  --enable-debug-assertions \
  --enable-optimize \
  --prefix=/opt/avr-rust

# Build the compiler, optionally install it to /opt/avr-rust
make
make install

# Register the toolchain with rustup
rustup toolchain link avr-toolchain $(realpath $(find . -name 'stage1'))

# Optionally enable the avr toolchain globally
rustup default avr-toolchain
```

## Usage

# With Xargo (recommended)

Take a look at the example [blink](https://github.com/avr-rust/blink) program.

# Vanilla `rustc`

AVR support is enabled by passing the `--target avr-unknown-unknown` flag to `rustc`.

Note that the Rust `libcore` library (essentially required for every Rust program),
must be manually compiled for it to be used, as it will not be built for AVR during
compiler compilation (yet). Work is currently being done in order to allow `libcore`
to be automatically compiled for AVR.
