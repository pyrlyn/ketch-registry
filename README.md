# ketch-registry

The default package registry for [ketch](https://github.com/pyrlyn/ketch).

Every top-level folder is a package and holds one `ketch.toml` describing it.
The folder name *is* the package name — it is what `ketch install <name>`
matches. There is no index file to keep in step with the contents, so a folder
without a `ketch.toml` is simply not a package.

```
ketch-registry/
├── README.md          ← not a package: no ketch.toml
└── ripgrep/
    └── ketch.toml
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

Most projects need no entry at all — ketch infers everything it needs from
`owner/repo`. Add one when inference gets it wrong: an unusually named release
asset, a binary worth linking under a different name, an `.app` bundle, or a
short alias worth remembering.

The full schema is
[docs/MANIFESTS.md](https://github.com/pyrlyn/ketch/blob/main/docs/MANIFESTS.md);
the layout and validation rules are
[docs/REGISTRY.md](https://github.com/pyrlyn/ketch/blob/main/docs/REGISTRY.md).

## Trying an entry before sending it

Drop the same file at `~/.ketch/manifests/<name>.toml` and install:

```bash
ketch install <name> --verbose
```

Local manifests take precedence over this registry, so a file that works there
is one that can be contributed here unchanged.

## Using this registry

It is the default; `ketch update` fetches it. To point somewhere else:

```bash
export KETCH_REGISTRY=someone/their-registry
```
