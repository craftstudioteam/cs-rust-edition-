# CS Rust Edition

**CS Rust Edition — Powered by Craft Studio** is the game engine that CS Launcher Plus ships as its
second, independent engine ("CS Engine"). It is a Rust reimplementation of the Minecraft Java Edition
client (protocol and world format of Java **26.3**), built for 64-bit Android and driven entirely by
CS Launcher Plus: installation, profiles, accounts, controls, settings, friends and voice all live in
the launcher; this repository is the engine underneath.

This is **not Minecraft**. It is an independent program that speaks the Minecraft Java Edition
protocol. It is not affiliated with, endorsed by or supported by Mojang Studios or Microsoft.

- Launcher: <https://github.com/craftstudioteam/CS-LAUNCHER-PLUS>
- Website: <https://official.craftstudios.site>
- Discord: <https://discord.gg/dmteYqZZyY>
- YouTube: <https://youtube.com/@craft-studio-official>

---

## What this is, and where it comes from

CS Rust Edition is a **derivative work**. The chain is, in full:

| Layer | Project | Author(s) | License |
|:--|:--|:--|:--|
| Engine | [Horizon](https://github.com/horizon-rust/horizon) | Ludovic Marie and the Horizon contributors | MIT |
| Android port | [Horizon Android](https://github.com/Vasysik/horizon-android) | Vasysik and contributors | MIT |
| This fork | CS Rust Edition | Craft Studio | MIT |

Craft Studio's changes on top of the Android port are, in summary:

- `cs-host` mode: the engine is launched by CS Launcher Plus through an intent extra and a small JNI
  bridge (`crates/horizon-client/src/android/cs_bridge.rs`); the launcher's own control system,
  cursor, keyboard and settings drive the engine. The Rust launcher UI of upstream is not shown.
- Craft Studio branding (title screen, loading screen, in-game menu, FPS overlay) in `crates/cs-brand`.
- In-game Craft Studio features: friends and invites, voice chat, chat bubbles, skin sharing,
  keybinds, mod browser, `.hmod` / shader / resource-pack handling, Discord Rich Presence hooks,
  play-time reporting to the launcher.
- e4mc hosting (`crates/horizon-e4mc`, `crates/cs-e4mc-android`): a Rust port of the e4mc protocol so
  a phone can host a world for friends over the public e4mc relay.
- Many Android-specific fixes (input, surfaces, lifecycle, storage, diagnostics).

`CS-FORK-NOTES.md` lists the changed files; `CS-ENGINE-ROADMAP.md` and `roadmap/` hold the plan.
Everything that is not listed there is upstream Horizon code, merged periodically and otherwise unchanged.

---

## Licensing — the short, complete answer

1. **This repository is MIT licensed** (`LICENSE`). The notice reads
   *Copyright (c) 2026 Ludovic Marie and the Horizon contributors*; Craft Studio's contributions are
   released under the same MIT license, *Copyright (c) 2026 Craft Studio*. See `NOTICE.md`.
2. **MIT lets you** use, copy, modify, merge, publish, distribute, sublicense and sell copies — as long
   as the copyright notice and the permission notice stay in every copy or substantial portion.
   That is the whole obligation. There is no copyleft, no source-disclosure duty, no fee.
3. **Third-party code and assets** inside the tree keep their own licenses (all permissive:
   MIT, Apache-2.0, OFL-1.1, CC0, CC BY 4.0). They are listed, file by file, in
   `THIRD_PARTY_NOTICES.md`. Nothing in this repository is GPL/LGPL.
4. **Minecraft content is never included.** No Mojang code, assets, textures, sounds, fonts or
   data files are in this repository or in any build of it. The engine downloads the game's
   resources **on the user's device, from Mojang's official endpoints, with pinned hashes**, exactly as
   upstream Horizon does — the same way the official launcher does. Users need their own copy of
   Minecraft Java Edition, bought from Mojang, to play online; that is enforced by Mojang's servers,
   not bypassed here.
5. **Trademarks are not licensed.** "Minecraft" is a trademark of Mojang/Microsoft; "Horizon" belongs
   to its authors; "e4mc" to Skye; "Craft Studio", "CS Launcher Plus", "CS Engine" and
   "CS Rust Edition" to Craft Studio. The MIT license covers code, not names and logos.
   See `POLICY.md` → Trademarks.

If you fork this: keep `LICENSE`, `NOTICE.md` and `THIRD_PARTY_NOTICES.md`, do not call your build
"Minecraft", and you are fully compliant.

---

## What it is not

- Not a Fabric, Forge, NeoForge or Quilt loader. Java mods for those loaders **do not run** on this
  engine. Supported mods are sandboxed `.hmod` packages written for Horizon.
- Not a server. It is a client; it joins Java Edition servers and can host for friends.
- Not an account service. Microsoft / Mojang sign-in is handled by CS Launcher Plus and the official
  Microsoft endpoints; this repository stores no credentials.
- Not an anti-cheat bypass. Server rules apply; see `docs/anticheat.md`.

---

## Repository layout

| Path | Contents |
|:--|:--|
| `crates/horizon-core`, `horizon-world`, `horizon-protocol`, `horizon-items`, … | Game logic, world, protocol (upstream Horizon) |
| `crates/horizon-client`, `horizon-client-render`, `horizon-client-input`, `horizon-client-core` | Client, renderer (wgpu/Vulkan), input |
| `crates/horizon-android` | Android entry point (`libhorizon_android.so`) |
| `crates/cs-brand`, `crates/cs-e4mc-android`, `crates/horizon-e4mc`, `crates/horizon-friends` | Craft Studio additions |
| `crates/horizon-mods`, `horizon-mod-sdk`, `horizon-mod-registry`, `cargo-horizon-mod` | `.hmod` sandbox and SDK |
| `launcher/` | Upstream's desktop launcher (not used by CS Launcher Plus) |
| `third_party/` | Vendored `wgpu-hal` and `winit` with local patches (own licenses inside) |
| `docs/`, `roadmap/`, `CS-ENGINE-ROADMAP.md` | Documentation and plans |
| `tools/`, `xtask/`, `tests/`, `bench/` | Build helpers, tests, benchmarks |

Builds for CS Launcher Plus are produced in Craft Studio's private build pipeline and bundled into the
launcher APK; this repository is the **source publication** of that engine. It contains no CI
configuration, no signing keys and no service credentials (see `POLICY.md` → Security).

---

## Building

Requirements: Rust (see `rust-toolchain.toml`), Android NDK r28, `cargo-ndk`, Java 17 for the mod SDK.

```sh
cargo ndk -t arm64-v8a -p horizon-android --features cs-host --release build
```

`docs/android.md` has the full walkthrough. Upstream's desktop client builds with plain `cargo build`
and is kept working so upstream merges stay clean.

---

## Documents

| File | What it covers |
|:--|:--|
| `LICENSE` | MIT license text (Horizon) |
| `NOTICE.md` | Copyright holders of this fork and how attribution must be kept |
| `THIRD_PARTY_NOTICES.md` | Every third-party component, where it sits, under which license |
| `POLICY.md` | Terms of use, privacy, content & trademark policy, security policy, takedown contact |
| `CONTRIBUTING.md`, `CONTRIBUTING_ANDROID.md` | How to contribute (DCO, code style, review) |
| `CS-FORK-NOTES.md` | Exact list of what Craft Studio changed |

---

## Credits

Horizon is the work of Ludovic Marie and the Horizon contributors. Horizon Android is the work of
Vasysik and contributors. CS Rust Edition would not exist without them; the MIT license is the only
thing they asked for, and it is honoured in full.

Minecraft is a trademark of Mojang Synergies AB / Microsoft. This project is independent and
non-commercial in nature with respect to Mojang's property; it contains none of it.

**CS Rust Edition — Powered by Craft Studio.**
