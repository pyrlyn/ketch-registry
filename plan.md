# ketch-registry

<https://github.com/pyrlyn/ketch-registry>

Default package registry for ketch: every top-level folder is a package with one `ketch.toml` manifest (cox, ketch, ripgrep, rtok, runa, swarfr).

| # | Status | Priority | Complexity | Readiness | Agent |
| --- | --- | --- | --- | --- | --- |
| T1 | todo | P1 | 1 | 0% | |
| T2 | todo | P2 | 2 | 0% | |
| T3 | todo | P2 | 1 | 0% | |
| T4 | todo | P3 | 1 | 0% | |
| T5 | todo | P3 | 1 | 0% | |
| T6 | todo | P3 | 1 | 0% | |
| T7 | todo | P3 | 3 | 0% | |

### T1. swarfr installs the wrong command name

`swarfr/ketch.toml` has no `bin` entry, but the v0.1.0 release ships a single executable named `dunnage` (crate was renamed to swarfr only after the tag). ketch does not rename it (`dunnage` carries no build metadata), so `ketch install swarfr` links `~/.ketch/bin/dunnage` and `swarfr` never reaches PATH. Done means: either the manifest pins an explicit `bin` entry covering the rename transition, or the first `swarfr`-named release is cut and verified with `ketch info swarfr`.

### T2. runa entry points at a repo with zero releases

`runa/ketch.toml` include/exclude globs (GPU variants, `*-installer.*`, `*.rb`) have never run against a real release — `pyrlyn/runa` has no published releases, only the v0.1.0 tag — and the CI that asserted them was removed in #8. Done means: runa's first release is published and the globs verified with `ketch info runa --assets`, or the entry is marked provisional.

### T3. Re-push the stale ketch manifest

`ketch/ketch.toml:5` references `src/builtin.toml`; the real path (documented in ketch's MANIFESTS.md) is `crates/ketch-core/src/builtin.toml`, which the ketch repo's own root `ketch.toml` already carries. Done means: the registry copy is refreshed from a `ketch push`.

### T4. Restore the `#:schema` directive on all manifests

The ketch repo's own manifest carries `#:schema https://raw.githubusercontent.com/pyrlyn/ketch/main/docs/manifest.schema.json`; none of the six registry copies do, although MANIFESTS.md recommends it and unknown keys are a hard error. Done means: all six manifests start with the directive.

### T5. Fix wrong excludes and standardize the dist exclude block

`cox/ketch.toml:18` excludes `SHA256SUMS` while cox actually publishes `sha256.sum`; `swarfr/ketch.toml:16-25`'s comment claims to skip the installer script and the Homebrew formula but the exclude list covers neither — both saved only by the include whitelist. runa is the only dist-based manifest with explicit installer/formula/checksum excludes (rtok's release also ships `rtok-installer.sh`, `rtok.rb`, `sha256.sum`). Done means: one standard exclude template for all cargo-dist packages and comments that match it.

### T6. Ship ripgrep completions and man page

ketch's own MANIFESTS.md uses ripgrep as its `extra_paths` example (`complete/rg.bash`, `doc/rg.1`, both present in the 15.2.0 payload); the registry entry omits them. Done means: the ripgrep manifest installs completions and the man page.

### T7. Sign releases and pin `[trust]` per manifest

No manifest pins a `[trust]` policy and no upstream release publishes signatures — installs verify checksums only, not publisher identity. Done means: upstream release workflows publish verifiable attestations (e.g. sigstore via GitHub Actions, which ketch verifies offline) and each registry manifest pins its `[trust]` policy.
