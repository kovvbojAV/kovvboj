# Contributing to KOVVBOJ

Bug reports, documentation fixes, and code contributions are welcome. When you
report a bug, include your OS, the KOVVBOJ version, and the steps that reproduce
it. Logs help too: run with `RUST_LOG=info`.

## Building from source

KOVVBOJ is a single Cargo crate. It pulls
[rustjay-engine](https://github.com/BlueJayLouche/rustjay-engine) from git, so
you don't need an engine checkout. Install Rust through
[rustup](https://rustup.rs/), then:

```sh
git clone https://github.com/kovvbojAV/kovvboj.git
cd kovvboj
cargo run --release --locked --features projection
```

Build and test with `--locked` so `Cargo.lock` stays authoritative.

### macOS

Install the Xcode Command Line Tools. For the `ffmpeg` feature, also install
FFmpeg:

```sh
xcode-select --install
brew install ffmpeg pkg-config
```

Syphon needs nothing extra, because `syphon-core` bundles the framework.

### Linux

On Debian or Ubuntu, install the same packages as
[CI](.github/workflows/ci.yml). The FFmpeg packages are only needed for the
`ffmpeg` feature:

```sh
sudo apt-get install -y libasound2-dev libudev-dev pkg-config clang ninja-build \
  libavcodec-dev libavformat-dev libavutil-dev libswscale-dev
```

### Windows

Use the MSVC toolchain with the Visual Studio C++ build tools, plus
[Ninja](https://ninja-build.org/). `.cargo/config.toml` forces the Ninja
generator for the CMake builds. For the `ffmpeg` feature, download the shared
FFmpeg 8.0 SDK that [the release workflow](.github/workflows/release.yml) pins,
and set `FFMPEG_DIR` to its root.

### NDI

The `ndi` feature needs the [NDI SDK](https://ndi.video/for-developers/ndi-sdk/)
installed. Set `NDI_SDK_DIR` if the SDK isn't in its default location.

## Cargo features

| Feature | Adds |
|---|---|
| *(default)* | `mixer`, `egui`, `webcam`, `led` |
| `projection` | The Stage and projector outputs (projection mapping, edge blend) |
| `ndi` | NDI in and out (needs the NDI SDK) |
| `ffmpeg` | Video files and streams through FFmpeg |
| `hap` | GPU-native HAP playback and encoding |
| `api` | The web parameter server |
| `link`, `prodj` | Ableton Link and Pro DJ Link tempo |
| `laser`, `laser-dac` | Laser decks. `laser-dac` adds hardware output and pulls in libusb and CMake. |
| `sysmon` | CPU and memory readout |

Releases are built with `--all-features`.

## Working on the engine at the same time

Every rustjay-engine crate is pinned to one engine `rev` in `Cargo.toml`, and
that includes the engine's vendored `wgpu-hal`, `nokhwa-bindings-macos`, and
`imgui-wgpu` under `[patch.crates-io]`. To make an engine change:

1. Uncomment the `[patch."https://github.com/BlueJayLouche/rustjay-engine"]`
   block in `Cargo.toml`. It points every engine crate at `../rustjay-engine`.
2. Change the engine and land it there first.
3. Bump **every** `rev = "…"` in `Cargo.toml` to the merged engine commit, run
   `cargo update -p rustjay-engine`, then comment the patch block out again.

Never commit the local patch block uncommented. CI can't see
`../rustjay-engine`.

## Checks

Before you open a PR, run the same checks as CI:

```sh
cargo clippy --locked --all-targets -- -D warnings
cargo clippy --locked --all-targets --features projection -- -D warnings
cargo test --locked --features projection
```

The kittest snapshot tests in `tests/ui_kittest.rs` render on Linux CI (lavapipe).
Two of them, `stage_preview_disabled_snapshot` and
`stage_edge_blend_preview_snapshot`, fail on macOS. That's expected, so trust
the CI result.

## Map of the code

| Area | Where |
|---|---|
| App assembly, state, render hook | `src/lib.rs` |
| Entry point, projector windows | `src/main.rs` |
| Three-column shell (menus, modes, panels) | `src/shell.rs` |
| Layer stack, library, inspector | `src/ui/mod.rs` |
| LED map and laser tabs | `src/ui/ledmap_tab.rs`, `src/ui/laser_tab.rs` |
| Sources (camera, solid, FFmpeg, HAP, streams, text) | `src/sources/` |
| Stage and projector outputs | `src/stage/` |
| Scene model, workspaces, save/load | `src/scene/`, `src/persistence/` |
| MIDI/OSC/HTTP parameter routing, keymap | `src/control/`, `src/keymap.rs` |
| Bundled shaders and credits | `shaders/`, `shaders/CREDITS.md` |
| Icons and packaging | `packaging/`, `.github/packaging/` |

[docs/architecture.md](docs/architecture.md) explains how these pieces fit
together.

## Shaders

Shaders you drop into `shaders/` stay local, because `.gitignore` ignores
`shaders/*.fs`, so third-party shaders don't get redistributed by accident. To
bundle one, `git add -f` it and add a row to
[`shaders/CREDITS.md`](shaders/CREDITS.md) with its author, source, and licence.
