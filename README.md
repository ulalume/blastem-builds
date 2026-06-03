# blastem-builds

**Unofficial**, automated multi-platform builds of [BlastEm](https://www.retrodev.com/blastem/) — the fast and accurate Sega Genesis / Mega Drive emulator by **Michael Pavone**.

> [!IMPORTANT]
> This repository is **not affiliated with, nor endorsed by, the upstream BlastEm project**.
> It only adds a CI pipeline that fetches the official source and produces builds for
> platforms the official nightlies do not currently cover. The source code is **not modified**.
>
> - Official site / downloads: <https://www.retrodev.com/blastem/>
> - Official source (Mercurial): <https://www.retrodev.com/repos/blastem>

## What this provides

The official nightlies cover 32/64-bit Linux and Windows (x86). This project adds the
targets they do not (Apple Silicon / Intel macOS, Linux arm64, WebAssembly) and rebuilds
the x86 desktop ones, so every changeset gets one self-contained release:

| Target | Runner | CPU core | Status |
|---|---|---|---|
| WebAssembly (WebGL) | ubuntu + emsdk | interpreter | ✅ |
| macOS arm64 — Apple Silicon (`.app`) | macos-14 | interpreter | ✅ |
| macOS x86_64 — Intel (`.app`) | macos-14 | JIT | ✅ |
| Linux arm64 | ubuntu-24.04-arm | interpreter | ✅ |
| Linux x86_64 | ubuntu-latest | JIT | ✅ |
| Windows x86_64 | ubuntu + mingw-w64 | JIT | ✅ |
| Windows x86 (32-bit) | ubuntu + mingw-w64 | JIT | ✅ |

> Note: BlastEm uses a native-code JIT on x86/x86_64 only. On every other architecture
> (ARM, WebAssembly) it automatically falls back to a portable C interpreter (`NEW_CORE`).

## How it works

1. A scheduled workflow (daily) and manual `workflow_dispatch` detect every recent changeset
   that does **not** yet have a release, using upstream's lightweight **hgweb HTTP** API.
2. Each changeset gets one GitHub **pre-release** tagged `nightly-<changeset>` with the
   artifacts for every platform attached.

Builds are **portable bundles**: required runtime libraries (SDL2, etc.) are included, so no
separate installation is needed.

## Licensing

- **BlastEm** is licensed under the **GNU GPL v3** (see `COPYING` inside each build). Source
  is the unmodified upstream; the exact changeset is recorded in each release.
- Bundled libraries keep their own permissive, GPL-compatible licenses, shipped alongside:
  - **SDL2** — zlib license
  - **GLEW** — Modified BSD / MIT
  - **zlib** — zlib license

Bundling these does not change BlastEm's GPLv3 licensing.

## Credits

BlastEm is created and maintained by **Michael Pavone**. All emulator code is his work.
This repository only provides build automation.
