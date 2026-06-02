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
missing targets and rebuilds the x86 ones too, so every changeset gets one self-contained
release with all platforms:

| Target | Runner | CPU core | Status |
|---|---|---|---|
| WebAssembly (WebGL) | ubuntu + emsdk | interpreter | ✅ implemented |
| macOS (Universal `.app`, Intel + Apple Silicon) | macos-13 + macos-14 | JIT / interpreter | ⏳ planned |
| Linux arm64 | ubuntu-24.04-arm | interpreter | ⏳ planned |
| Linux x86_64 | ubuntu-latest | JIT | ⏳ planned |
| Linux x86 (32-bit) | ubuntu + multilib | JIT | ⏳ planned |
| Windows x86_64 | ubuntu + mingw-w64 | JIT | ⏳ planned |
| Windows x86 (32-bit) | ubuntu + mingw-w64 | JIT | ⏳ planned |

> Note: BlastEm uses a native-code JIT on x86/x86_64 only. On every other architecture
> (ARM, WebAssembly) it automatically falls back to a portable C interpreter (`NEW_CORE`).

## How it works

1. A scheduled workflow (daily) and manual `workflow_dispatch` detect every recent changeset
   that does **not** yet have a release, using upstream's lightweight **hgweb HTTP** API.
2. **Every unbuilt changeset is built** (not just the latest), so multiple commits in a day
   are all covered. A per-run cap prevents runaway backfills.
3. Each changeset gets one GitHub **pre-release** tagged `nightly-<changeset>` with the
   artifacts for every platform attached.

### Easy on upstream

No `hg clone` is ever performed. Each run makes **one small JSON request** (`/json-log`,
a few KB) to find new changesets, and downloads a **~2 MB source archive only for genuinely
new revisions** (usually none on a quiet day). "What has been built" is tracked entirely by
the GitHub release tags, so nothing extra is mirrored.

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
