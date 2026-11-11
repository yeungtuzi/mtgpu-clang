# mtgpu-clang

A self-contained aarch64 clang/LLVM toolchain for the M1000 laptop (12× Cortex-A78,
Ubuntu 24.04 arm64). Built from the LLVM fork that Chromium pins, so the same
compiler can build Chromium, its Rust toolchain and crubit.

## Install

```sh
sudo apt install ./mtgpu-clang_24.0.0_arm64.deb
source /opt/mtgpu-toolchain/env.sh
clang --version
```

## Guarantees

- Installs **only** under `/opt/mtgpu-toolchain/clang`.
- Declares **no dependencies**, registers no alternatives, edits no system path.
- The distribution's `/usr/bin/clang` and `/usr/lib/llvm-*` are untouched.
- Removal: `dpkg -r mtgpu-clang` (or `rm -rf /opt/mtgpu-toolchain`).

## Build configuration

| option | value |
|---|---|
| targets | `AArch64` only |
| asserts | off |
| tools/runtimes | none |
| libxml2 | off |
| build dir | kept (`MTGPU_KEEP_BUILD=1`) |
| host flags | `-mcpu=cortex-a78+nocrypto` |

The host CPU has no AES extension, hence `+nocrypto`: without it every binary
crashes with SIGILL.  See `mtgpu-kde/tools/chromium-hdr/TOOLCHAIN.md` for the
full build recipe and the pitfalls (sysroot, `-std=c++20` ABI, crubit).
