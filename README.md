### Releases

Push a `v*` tag to build and publish a release with four zips:

| Asset | Contents |
| --- | --- |
| `IosevkaMono.zip` | plain fixed-width family |
| `IosevkaSans.zip` | plain proportional family |
| `IosevkaMonoNerdFont.zip` | Nerd Font Mono, patched with `--complete --mono` |
| `IosevkaSansNerdFont.zip` | Nerd Font Propo, patched with `--complete --variable-width-glyphs` |

`workflow_dispatch` runs the same build but uploads the zips as a workflow artifact instead of
creating a release.

### Building locally

Needs `bun`, `ttfautohint`, and `fontforge` with Python support.

```sh
git submodule update --init --depth 1
./build   # dist/*.ttf
./patch   # nerd/*.ttf
```

`./patch` downloads `FontPatcher.zip` from the nerd-fonts release into `.cache/` and patches every
font in `dist/` in parallel. Override the version with `PATCHER_VERSION=v3.5.1 ./patch`.

### Updating Iosevka

The submodule is shallow, so fetch the tag you want before checking it out:

```sh
git -C iosevka fetch --depth 1 origin tag v34.8.1
git -C iosevka checkout v34.8.1
git commit -am "Update Iosevka to v34.8.1"
```
