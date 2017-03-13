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