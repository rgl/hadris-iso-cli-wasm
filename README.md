# About

[![Build](https://github.com/rgl/hadris-iso-cli-wasm/actions/workflows/build.yml/badge.svg)](https://github.com/rgl/hadris-iso-cli-wasm/actions/workflows/build.yml)

This builds and releases the [hadris-iso-cli application](https://github.com/hxyulin/hadris/tree/main/crates/hadris-iso-cli) as a [wasm binary](https://github.com/rgl/hadris-iso-cli-wasm/releases).

## Use

Download the binary from https://github.com/rgl/hadris-iso-cli-wasm/releases.

Install a wasm runtime like [wazero](https://github.com/wazero/wazero) or [wasmtime](https://github.com/bytecodealliance/wasmtime).

Build an example iso content tree:

```bash
rm -rf cidata*
install -d cidata
echo '#user-data' >cidata/user-data
echo '#meta-data' >cidata/meta-data
```

Build an example iso using wazero:

```bash
wazero run \
    -mount . \
    hadris-iso-cli.wasm \
    create \
    --joliet \
    --rock-ridge \
    --volume-name CIDATA \
    --output cidata.iso \
    cidata
```

Build an example iso using wasmtime:

```bash
wasmtime run \
    --dir . \
    hadris-iso-cli.wasm \
    create \
    --joliet \
    --rock-ridge \
    --volume-name CIDATA \
    --output cidata.iso \
    cidata
```

## Develop

Install the dependencies:

* [Docker](https://docs.docker.com/engine/install/).
* [Visual Studio Code](https://code.visualstudio.com).
* [Dev Container plugin](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers).

Open this directory with the Dev Container plugin.

Open `bash` inside the Visual Studio Code Terminal.

Build:

```bash
git clone https://github.com/hxyulin/hadris
cd hadris/crates/hadris-iso-cli
# build the asm flavor.
cargo build \
    --package hadris-iso-cli  \
    --release \
    --target wasm32-wasip1
ls -laF ../../target/wasm32-wasip1/release/hadris-iso-cli.wasm
# for reference, build the native flavor.
cargo build \
    --package hadris-iso-cli  \
    --release
ls -laF ../../target/release/hadris-iso-cli
```

Build an example iso content tree:

```bash
rm -rf cidata*
install -d cidata
echo '#user-data' >cidata/user-data
echo '#meta-data' >cidata/meta-data
```

For reference, build an example iso using xorriso:

```bash
xorriso \
    -as genisoimage \
    -output cidata-xorriso.iso \
    -volid CIDATA \
    -joliet \
    -rock \
    cidata
xorriso \
    -dev cidata-xorriso.iso \
    -toc
xorriso \
    -dev cidata-xorriso.iso \
    -find / \
    -type f
7z l cidata-xorriso.iso
```

For reference, build an example iso using native hadris-iso-cli:

```bash
../../target/release/hadris-iso-cli \
    create \
    --joliet \
    --rock-ridge \
    --volume-name CIDATA \
    --output cidata-native.iso \
    cidata
xorriso \
    -dev cidata-native.iso \
    -toc
xorriso \
    -dev cidata-native.iso \
    -find / \
    -type f
```

Build an example iso using wazero:

```bash
wazero run \
    -mount . \
    ../../target/wasm32-wasip1/release/hadris-iso-cli.wasm \
    create \
    --joliet \
    --rock-ridge \
    --volume-name CIDATA \
    --output cidata.iso \
    cidata
xorriso \
    -dev cidata.iso \
    -toc
xorriso \
    -dev cidata.iso \
    -find / \
    -type f
```

Build an example iso using wasmtime:

```bash
wasmtime run \
    --dir . \
    ../../target/wasm32-wasip1/release/hadris-iso-cli.wasm \
    create \
    --joliet \
    --rock-ridge \
    --volume-name CIDATA \
    --output cidata-wasmtime.iso \
    cidata
xorriso \
    -dev cidata-wasmtime.iso \
    -toc
xorriso \
    -dev cidata-wasmtime.iso \
    -find / \
    -type f
```
