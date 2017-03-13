# How to update ripgrep for VS Code

- Clone ripgrep and checkout the latest release tag (from upstream)
- Build for each platform and architecture (see below)
- Zip each build, and name with the same scheme as in earlier releases
- Create a release for the tag on `roblourens/ripgrep`, and upload the .zips

## How to produce ripgrep builds

Start by installing Rust and following the directions at https://github.com/roblourens/ripgrep#building - it's generally very easy. `cargo build --release` will leave the resulting binary at `./target/release/rg`.

### Mac OS
Nothing special.

### Windows
We need to ship a 32-bit binary for Windows, but Rust will probably be set up for 64-bit mode by default.

- You probably have `rustup` installed and on your path. If not, [install it](https://www.rustup.rs/).
- Run `rustup target add i686-pc-windows-msvc` to install the 32 bit toolchain
- Run `cargo build --release --target=i686-pc-windows-msvc`, which will drop its binary at `./target/i686-pc-windows-msvc/release/rg`.
- Done!

### Linux
On Linux we need to build 64-bit, 32-bit, and ARM. These steps assume that your Linux install is 64-bit.

- Build following the typical directions, which should produce your 64-bit binary.
- To build 32-bit, first add the Rust target, as on Windows: `rustup target add i686-unknown-linux-gnu`
  - Then ensure you have the 32-bit libraries you need for GCC: `apt-get install gcc-multilib`
  - Then ensure that GCC uses the stuff you just installed: `export CFLAGS=-m32`
  - Finally, `cargo build --release --target=i686-unknown-linux-gnu`
- To build ARM, add the ARM target: `arm-unknown-linux-gnueabi`
  - Install a bunch of stuff: `sudo apt-get install arm-unknown-linux-gnueabi binutils-arm-linux-gnueabi gcc-arm-linux-gnueabi`
  - Create a file at `.cargo/config` with the following contents:
    ```toml
    [target.arm-unknown-linux-gnueabi]
    linker = "arm-linux-gnueabi-gcc"
    ```
  - Build: `cargo build --release --target=arm-unknown-linux-gnueabi`