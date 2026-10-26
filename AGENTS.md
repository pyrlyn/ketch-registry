# AGENTS.md

If an AGENTS.md or CLAUDE.md exists higher in the tree, follow it too; on conflict, ask the creator.

## What this repo is

The default package registry for the package manager. Every top-level folder that contains `the package manager.toml` is one package. The folder name is the package name. There is no index file. This checkout's `origin` is `git@github.com:pyrlyn/pkgmgr-registry.git`.

| Folder | `source` |
| --- | --- |
| the coding agent | `github:pyrlyn/cox` |
| the package manager | `github:pyrlyn/ketch` |
| `ripgrep` | `github:BurntSushi/ripgrep` |
| the token CLI | `github:pyrlyn/rtok` |
| `runa` | `github:pyrlyn/runa` |
| `swarfr` | `github:listepo/swarfr` |

`source` is the only required field. The schema and validation rules are not in this repo. The README points at `docs/MANIFESTS.md` and `docs/REGISTRY.md` in `https://github.com/pyrlyn

A local check before a pull request, from the README:

```bash
The package manager install <name> --verbose
```

with the same file at `~/.pkgmgr/manifests/<name>.toml`. Local manifests win over this registry.

## Tests

This repo is data. There is no checker script, schema file, or test runner here. Do not add a test framework for the TOML files. Validate with the package manager, which lives in its own repository.
