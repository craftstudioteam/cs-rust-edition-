# Policies — CS Rust Edition

This file collects, in plain words, everything that is not the license itself: what you may do with
the name, what the software does with your data, what the project will and will not host, and how to
reach Craft Studio about any of it. The license (`LICENSE`, MIT) always wins over this document for the
code; this document only adds the rules for names, services and conduct.

Last updated: 2026-11-10.

---

## 1. Terms of use

1. The software is provided **"as is", without warranty of any kind**, as the MIT License states.
   Craft Studio, the Horizon authors and the Horizon Android authors are not liable for any damage,
   data loss, account action or server ban that results from using it.
2. You need your own, legitimately purchased copy of **Minecraft: Java Edition** to play on online
   servers. The engine does not include the game, does not bypass authentication and does not give
   access to anything you have not bought. Offline/local play uses the same downloaded resources and
   the same rules Mojang applies to any third-party client.
3. You must comply with the [Minecraft EULA](https://www.minecraft.net/eula) and the
   [Minecraft Usage Guidelines](https://www.minecraft.net/usage-guidelines) when you play, and with the
   rules of every server you join. Third-party clients are allowed on some servers and forbidden on
   others; it is your responsibility to check.
4. Do not use the software to cheat, grief, harass, evade bans, or attack servers or other players.
   The engine ships no cheats; `docs/anticheat.md` describes what it reports about itself to servers.
5. Features that talk to other people — friends, invites, voice chat, e4mc hosting, chat — are
   provided by CS Launcher Plus and public relays. Treat them like any other public multiplayer: do not
   share codes with people you do not trust; a host can kick any guest at any time.
6. The software is intended for users who are old enough to hold a Microsoft account in their
   country. Parents and guardians should supervise younger players as they would with the official game.

## 2. Privacy

What the **engine in this repository** does, on its own:

- Downloads Minecraft resources (client jar, assets, libraries) from Mojang's official endpoints and
  verifies them with pinned hashes. Mojang sees the same requests the official launcher makes.
- Talks to the Minecraft servers you choose to join, and to Mojang's session server to join them
  (that is how the game works; nothing extra is sent).
- Stores worlds, settings, screenshots and downloaded resources in the app's private storage on your
  device (or the folder CS Launcher Plus assigns).
- Writes a local diagnostics log for crashes. It stays on the device unless **you** choose to share it.
- Contains **no analytics, no advertising SDK, no telemetry endpoint of Craft Studio's**. The
  `horizon-telemetry` crates are upstream's opt-in performance tooling and are not enabled in CS builds.

What is handled **by CS Launcher Plus, not by this repository**:

- Microsoft / Ely.by / custom Yggdrasil sign-in, token storage (Android Keystore), skins.
- Craft Studio friends, invites, chat and voice. The launcher provides those through its own backend;
  the engine only receives the result over JNI. The backend client is deliberately **not** part of this
  public tree (see §5). What the launcher collects for friends is described in the launcher's own
  privacy notice at <https://official.craftstudios.site>.
- e4mc hosting goes through Skye's public relay (`*.e4mc.link`); the relay sees the host's and
  guests' IP addresses, as any relay must. Craft Studio does not run the relay and does not log it.
- Optional Discord Rich Presence: the launcher sends your current activity to the Discord app on your
  device only when you enable it.

Craft Studio does not sell data, does not profile users and has no interest in doing so.
Requests about data the launcher holds for you: use the contact in §6.

## 3. Content and trademark policy

- **"Minecraft"** is a trademark of Mojang Synergies AB. This project is not "Minecraft" and must not be
  described, packaged or distributed as if it were, nor as an official or Mojang-approved client. Any
  fork must follow Mojang's brand and asset guidelines as well.
- **No Mojang assets are redistributed**, ever: not in this repository, not in builds, not in
  screenshots used as app icons or store listings. Screenshots of gameplay in documentation are fair
  depictions of the running program, as Mojang's guidelines permit.
- **"Craft Studio", "CS Launcher Plus", "CS Engine", "CS Rust Edition"** and the Craft Studio logo are
  Craft Studio's. MIT lets you reuse the code freely; it does not let you present your build as Craft
  Studio's. If you fork, rename your build and remove the Craft Studio logo from `crates/cs-brand`.
  Saying "based on CS Rust Edition by Craft Studio" is welcome and encouraged.
- **"Horizon"** belongs to the upstream project. Upstream Horizon's own naming wishes are in its
  repository and are respected here: this fork is called CS Rust Edition, not "Horizon".
- Mod loader logos (Quilt, NeoForge) and the Microsoft sign-in symbol are used exactly as their
  owners' guidelines allow; details and license references are in `THIRD_PARTY_NOTICES.md`.
- Mods: the engine runs sandboxed `.hmod` packages. Craft Studio does not host mods in this
  repository. Mods you download through CS Launcher Plus come from their own publishers (Modrinth,
  CurseForge, Horizon's mod index) under their own licenses.

## 4. Fork and redistribution checklist

You are free to fork, modify, ship and sell. To do it cleanly:

1. Keep `LICENSE`, `NOTICE.md` and `THIRD_PARTY_NOTICES.md` in the source and ship their text with any
   binary (an "About" screen or a bundled text file is enough).
2. Keep the per-folder license files under `third_party/` and in the font/icon folders.
3. Do not call the result "Minecraft", "Horizon", "CS Engine" or "Craft Studio …"; pick your own name
   and replace the branding crate.
4. Do not bundle Mojang assets. Keep the download-on-device design.
5. If you add third-party code, add it to `THIRD_PARTY_NOTICES.md`. Only permissive licenses are
   accepted into this repository; GPL/LGPL code will not be merged.

## 5. Security policy

- **No secrets in this repository.** It contains no CI workflows, no signing keys, no API keys, no
  service credentials. Release builds of the engine are produced in Craft Studio's private pipeline and
  signed there; the public tree is the source publication.
- **Friends backend client is withheld.** The module that talks to Craft Studio's friends/invite
  service (`crates/horizon-client/src/cs_friends.rs`) is intentionally excluded (`.gitignore`) so the
  endpoint, keys and request protocol are not public. The in-game friends UI, invite banner, voice and
  e4mc code are published; without that module they have no backend and the engine builds as a
  stand-alone client. This is a deliberate design: the public source cannot be used to impersonate the
  launcher against the service.
- **Downloads are verified.** Every Mojang resource is checked against the hash in the official
  manifest. Mods run in the `.hmod` sandbox (`docs/mods.md`, `crates/horizon-mods`).
- **Reporting a vulnerability:** please do **not** open a public issue. Write to the contact in §6
  with "SECURITY" in the subject, or message a moderator on Discord. Expect an acknowledgement within
  72 hours. Please give reasonable time to fix before disclosure; credit is given in release notes
  unless you prefer otherwise. Upstream Horizon's own policy is in `docs/security.md`; issues in
  upstream code are forwarded to the Horizon maintainers.

## 6. Contact, takedown and disputes

- Discord: <https://discord.gg/dmteYqZZyY> (fastest)
- Website: <https://official.craftstudios.site>
- GitHub: open an issue on this repository for anything that is not security-sensitive.

Rights holders who believe something in this repository infringes their rights can contact Craft
Studio through any channel above with: the material, the right claimed, and how to reach you. Valid
requests are acted on promptly; the repository is intended to contain only MIT/permissively licensed
code and no third-party assets beyond those listed in `THIRD_PARTY_NOTICES.md`.

## 7. Changes to this document

This file may be updated as the project evolves. The date at the top changes with every edit; the
commit history of this repository is the record of what changed.
