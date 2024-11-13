---

title: "Terrain initial setup"
menu_order: 1
post_status: publish
post_excerpt: 
featured_image: 
taxonomy:
    epkb_post_type_1_category:
        - quick-start
    epkb_post_type_1_tag:
        - Developer resources
        - dApp
        - Terrain initial setup
        - Terra Classic

---
<p>[giw_edit_link]</p>

# ** NOT UPDATED SINCE MAY 2022 **

# Terrain initial setup

This tutorial uses a Terra-specific development suite called Terrain.

Terrain will help you:

* Scaffold your dApp project
* Develop and deploy smart contracts
* Create custom tasks for blockchain and contract interaction
* Access a console (or REPL) for interacting with the Terra blockchain
* Create predefined functions used in tasks and in the console

## Prerequisites

- [Install Docker](https://www.docker.com/)
- [Install `docker-compose`](https://github.com/docker/compose)
- [Install NPM](https://www.npmjs.com/)
- [Install Node JS v16](https://nodejs.org/download/release/latest-v16.x/)

## 1. Set up Rust

Rust is the main programming language used for CosmWasm smart contracts. While WASM smart contracts can theoretically be written in any programming language, CosmWasm libraries and tooling work best with Rust.

First, install the latest version of [Rust](https://www.rust-lang.org/tools/install).  

Then run the following commands:

```sh
# 1. Set 'stable' as the default release channel:

rustup default stable

# 2. Add WASM as the compilation target:

rustup target add wasm32-unknown-unknown

# 3. Install the following packages to generate the contract:

cargo install cargo-generate --features vendored-openssl
cargo install cargo-run-script
```

## 2. Install Terrain

Use npm to install the terrain command-line tool globally:

```sh
npm install -g @terra-money/terrain
```

 
## Next steps: LocalTerra or Testnet

Depending on your setup, you can either install LocalTerra or use the Bombay testnet to power Terrain. 

LocalTerra is a development environment designed to make it easy for smart contract developers to test their contracts locally. Terrain can also interact with the Terra blockchain's live testing environment using the Bombay testnet. 

::: {caution}
Localterra may not work properly on machines with less than 16 GB of RAM. Please use the [Bombay testnet](using-terrain-testnet.md) if your device does not meet this requirement.
:::

:::::::{grid}
:gutter: 3

:::{grid-item-card}
:link: using-terrain-testnet.html
:class-card: sd-text-center sd-shadow-md sd-rounded-3
```{image} /img/icon_node.svg
:class: sd-width-auto sd-animate-grow50-rot20 sd-pb-2
```
**Terrain and the testnet**  
8+ GB of RAM
:::

:::{grid-item-card}
:link: using-terrain-localterra.html
:class-card: sd-text-center sd-shadow-md sd-rounded-3
```{image} /img/LocalTerra.svg
:class: sd-width-auto sd-animate-grow50-rot20 sd-pb-2
```
**Terrain and LocalTerra**  
16+ GB of RAM
:::


:::::::
