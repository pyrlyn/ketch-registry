# pkgmgr-registry

The default package registry for the package manager.

Every top-level folder is a package and holds one `the package manager.toml` describing it.
The folder name *is* the package name — it is what `the package manager install <name>`
matches. There is no index file to keep in step with the contents, so a folder
without a `the package manager.toml` is simply not a package.

```
pkgmgr-registry/
├── README.md          ← not a package: no the package manager.toml
└── ripgrep/
    └── the package manager.toml
```

## Adding a package

Send a pull request adding `<name>/ketch.toml`:

```toml
source      = "github:BurntSushi/ripgrep"
description = "Recursively search directories for a regex pattern"
homepage    = "https://github.com/BurntSushi/ripgrep"
bin         = [{ name = "rg" }]
provides    = ["rg"]
```

`source` is the only required field. `name` may be given, but it must equal the
folder name.

Most projects need no entry at all — the package manager infers everything it needs from
`owner/repo`. Add one when inference gets it wrong: an unusually named release
asset, a binary worth linking under a different name, an `.app` bundle, or a
short alias worth remembering.

The full schema is
[docs/MANIFESTS.md](https://github.com/pyrlyn);
the layout and validation rules are
[docs/REGISTRY.md](https://github.com/pyrlyn).

## Trying an entry before sending it

Drop the same file at `~/.pkgmgr/manifests/<name>.toml` and install:

```bash
The package manager install <name> --verbose
```

Local manifests take precedence over this registry, so a file that works there
is one that can be contributed here unchanged.

## Using this registry

It is the default; `the package manager update` fetches it. To point somewhere else:

```bash
export PKGMGR_REGISTRY=someone/their-registry
```
