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
    --volume-name cidata \
    --volume-set-id "example volume set" \
    --preparer-id "example preparer" \
    --publisher-id "example publisher" \
    --application-id "example application" \
    --system-id "example system" \
    --output cidata.iso \
    cidata
```

Show information about the example iso file:

```bash
wazero run \
    -mount .:ro \
    hadris-iso-cli.wasm \
    info \
    cidata.iso
wazero run \
    -mount .:ro \
    hadris-iso-cli.wasm \
    tree \
    cidata.iso
wazero run \
    -mount .:ro \
    hadris-iso-cli.wasm \
    ls --long \
    cidata.iso
```

Build an example iso using wasmtime:

```bash
wasmtime run \
    --dir . \
    hadris-iso-cli.wasm \
    create \
    --joliet \
    --rock-ridge \
    --volume-name cidata \
    --volume-set-id "example volume set" \
    --preparer-id "example preparer" \
    --publisher-id "example publisher" \
    --application-id "example application" \
    --system-id "example system" \
    --output cidata.iso \
    cidata
```

Show information about the example iso file:

```bash
wasmtime run \
    --dir . \
    hadris-iso-cli.wasm \
    info \
    cidata.iso
wasmtime run \
    --dir . \
    hadris-iso-cli.wasm \
    tree \
    cidata.iso
wasmtime run \
    --dir . \
    hadris-iso-cli.wasm \
    ls --long \
    cidata.iso
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
# build the wasm flavor.
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
    -volid cidata \
    -volset "example volume set" \
    -preparer "example preparer" \
    -publisher "example publisher" \
    -appid "example application" \
    -sysid "example system" \
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
wazero run \
    -mount .:ro \
    ../../target/wasm32-wasip1/release/hadris-iso-cli.wasm \
    info \
    cidata-xorriso.iso
iso-info \
    --no-header \
    --input cidata-xorriso.iso
7z l cidata-xorriso.iso
```

For reference, build an example iso using native hadris-iso-cli:

```bash
../../target/release/hadris-iso-cli \
    create \
    --joliet \
    --rock-ridge \
    --volume-name cidata \
    --volume-set-id "example volume set" \
    --preparer-id "example preparer" \
    --publisher-id "example publisher" \
    --application-id "example application" \
    --system-id "example system" \
    --output cidata-native.iso \
    cidata
../../target/release/hadris-iso-cli \
    info \
    cidata-native.iso
xorriso \
    -dev cidata-native.iso \
    -toc
xorriso \
    -dev cidata-native.iso \
    -find / \
    -type f
iso-info \
    --no-header \
    --input cidata-native.iso
7z l cidata-native.iso
```

Build an example iso using wazero:

```bash
wazero run \
    -mount . \
    ../../target/wasm32-wasip1/release/hadris-iso-cli.wasm \
    create \
    --joliet \
    --rock-ridge \
    --volume-name cidata \
    --volume-set-id "example volume set" \
    --preparer-id "example preparer" \
    --publisher-id "example publisher" \
    --application-id "example application" \
    --system-id "example system" \
    --output cidata.iso \
    cidata
wazero run \
    -mount .:ro \
    ../../target/wasm32-wasip1/release/hadris-iso-cli.wasm \
    info \
    cidata.iso
wazero run \
    -mount .:ro \
    ../../target/wasm32-wasip1/release/hadris-iso-cli.wasm \
    tree \
    cidata.iso
wazero run \
    -mount .:ro \
    ../../target/wasm32-wasip1/release/hadris-iso-cli.wasm \
    ls --long \
    cidata.iso
xorriso \
    -dev cidata.iso \
    -toc
xorriso \
    -dev cidata.iso \
    -find / \
    -type f
iso-info \
    --no-header \
    --input cidata.iso
7z l cidata.iso
```

Build an example iso using wasmtime:

```bash
wasmtime run \
    --dir . \
    ../../target/wasm32-wasip1/release/hadris-iso-cli.wasm \
    create \
    --joliet \
    --rock-ridge \
    --volume-name cidata \
    --volume-set-id "example volume set" \
    --preparer-id "example preparer" \
    --publisher-id "example publisher" \
    --application-id "example application" \
    --system-id "example system" \
    --output cidata-wasmtime.iso \
    cidata
wasmtime run \
    --dir . \
    ../../target/wasm32-wasip1/release/hadris-iso-cli.wasm \
    info \
    cidata-wasmtime.iso
wasmtime run \
    --dir . \
    ../../target/wasm32-wasip1/release/hadris-iso-cli.wasm \
    tree \
    cidata-wasmtime.iso
wasmtime run \
    --dir . \
    ../../target/wasm32-wasip1/release/hadris-iso-cli.wasm \
    ls --long \
    cidata-wasmtime.iso
xorriso \
    -dev cidata-wasmtime.iso \
    -toc
xorriso \
    -dev cidata-wasmtime.iso \
    -find / \
    -type f
iso-info \
    --no-header \
    --input cidata-wasmtime.iso
7z l cidata-wasmtime.iso
```
