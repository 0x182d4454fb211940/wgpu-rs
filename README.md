<img align="right" width="25%" src="logo.png">

# wgpu-rs
[![Build Status](https://github.com/gfx-rs/wgpu-rs/workflows/CI/badge.svg?branch=master)](https://github.com/gfx-rs/wgpu-rs/actions)
[![Crates.io](https://img.shields.io/crates/v/wgpu.svg)](https://crates.io/crates/wgpu)
[![Docs.rs](https://docs.rs/wgpu/badge.svg)](https://docs.rs/wgpu)
[![Matrix](https://img.shields.io/matrix/wgpu:matrix.org)](https://matrix.to/#/#wgpu:matrix.org)

wgpu-rs is an idiomatic Rust wrapper over [wgpu-core](https://github.com/gfx-rs/wgpu). It's designed to be suitable for general purpose graphics and computation needs of Rust community.

wgpu-rs can target both the natively supported backends and WASM directly.

## Gallery

![Cube](etc/example-cube.png) ![Shadow](etc/example-shadow.png) ![MipMap](etc/example-mipmap.png) ![Skybox](etc/example-skybox.gif)
![vange-rs](etc/vange-rs.png) ![Brawl](etc/brawl-attack.gif) ![GLX map](etc/glx-map.png) ![Harmony](etc/harmony-rs.jpg)

## Usage

### How to Run Examples

All examples are located under the [examples](examples) directory.

These examples use the default syntax for running examples, as found in the [Cargo](https://doc.rust-lang.org/cargo/reference/manifest.html#examples) documentation. For example, to run the `cube` example:

```bash
cargo run --example cube
```

The `hello-triangle` and `hello-compute` examples show bare-bones setup without any helper code. For `hello-compute`, pass 4 numbers separated by spaces as arguments:

```bash
cargo run --example hello-compute 1 2 3 4
```

#### Run Examples on the Web (`wasm32-unknown-unknown`)

Running on the web is still work-in-progress. You may need to enable experimental flags on your browser. Check browser implementation status on [webgpu.io](https://webgpu.io).

To run examples on the `wasm32-unknown-unknown` target, first build the example as usual, then run `wasm-bindgen`:

```bash
# Install or update wasm-bindgen-cli
cargo install -f wasm-bindgen-cli
# Build with the wasm target
RUSTFLAGS=--cfg=web_sys_unstable_apis cargo build --target wasm32-unknown-unknown --example hello-triangle
# Generate bindings in a `target/generated` directory
wasm-bindgen --out-dir target/generated --web target/wasm32-unknown-unknown/debug/examples/hello-triangle.wasm
```

Create an `index.html` file into `target/generated` directory and add the following code:

```html
<html>
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
  </head>
  <body>
    <script type="module">
      import init from "./hello-triangle.js";
      init();
    </script>
  </body>
</html>
```

Now run a web server locally inside the `target/generated` directory to see the `hello-triangle` in the browser.

If you want to adapt the above to run your own crate on the web you will also need to add the following to your Cargo.toml:
```toml
[patch.crates-io]
wasm-bindgen = { git = "https://github.com/rustwasm/wasm-bindgen" }
wasm-bindgen-futures = { git = "https://github.com/rustwasm/wasm-bindgen" }
web-sys = { git = "https://github.com/rustwasm/wasm-bindgen" }
js-sys = { git = "https://github.com/rustwasm/wasm-bindgen" }
```

## Friends

Shout out to the following projects that work best with wgpu-rs:

- [wgpu_glyph](https://github.com/hecrj/wgpu_glyph) - for your text-y rendering needs
- [coffee](https://github.com/hecrj/coffee) - a whole 2D engine
- [iced](https://github.com/hecrj/iced) - a cross-platform GUI library
- [rgx](https://github.com/cloudhead/rgx) - a 2D graphics library
- [imgui-wgpu](https://github.com/Yatekii/imgui-wgpu-rs) - Dear ImGui interfacing
- [pixels](https://github.com/parasyte/pixels) - the easiest way to create a hardware-accelerated pixel frame buffer
- [kas](https://github.com/dhardy/kas) - tooKit Abstraction System
- [oxidator](https://github.com/Ruddle/oxidator) - RTS game engine
- [nannou](https://github.com/nannou-org/nannou) - a creative coding framework
- [harmony](https://github.com/StarArawn/harmony) - a modern 2D/3D engine

Also, libraries that have support for wgpu-rs:

- [conrod](https://github.com/PistonDevelopers/conrod) - shader-based UI
- [grr-2d](https://github.com/norse-rs/grr-2d) - experimental 2D renderer
- [lyon](https://github.com/nical/lyon) - a path tessellation library

## Development

If you need to test local fixes to gfx-rs or other dependencies, the simplest way is to add a Cargo patch. For example, when working on DX12 backend on Windows, you can check out the "hal-0.2" branch of gfx-rs repo and add this to the end of "Cargo.toml":

```toml
[patch.crates-io]
gfx-backend-dx12 = { path = "../gfx/src/backend/dx12" }
gfx-hal = { path = "../gfx/src/hal" }
```

If a version needs to be changed, you need to do `cargo update -p gfx-backend-dx12`.
