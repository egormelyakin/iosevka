### Releases

Push a `v*` tag to build and publish a release with six zips:

| Asset | Contents |
| --- | --- |
| `IosevkaMono.zip` | plain fixed-width family |
| `IosevkaSans.zip` | plain proportional family |
| `IosevkaMonoNerdFont.zip` | Nerd Font Mono, patched with `--complete --mono` |
| `IosevkaSansNerdFont.zip` | Nerd Font Propo, patched with `--complete --variable-width-glyphs` |
| `IosevkaMonoWebFont.zip` | woff2 files plus `IosevkaMono.css` |
| `IosevkaSansWebFont.zip` | woff2 files plus `IosevkaSans.css` |

`workflow_dispatch` runs the same build but uploads the zips as a workflow artifact instead of
creating a release.

### Building locally

Needs `bun`, `ttfautohint`, and `fontforge` with Python support.

```sh
git submodule update --init --depth 1
./build   # dist/*.ttf, web/<family>/{<family>.css,WOFF2/*.woff2}
./patch   # nerd/*.ttf
```

The web font zips unpack to a directory per family. Serve it as-is and link the CSS, which
references `WOFF2/*.woff2` relatively and declares the families `Iosevka Mono Web` and
`Iosevka Sans Web`.

`./patch` downloads `FontPatcher.zip` from the nerd-fonts release into `.cache/` and patches every
font in `dist/` in parallel. Override the version with `PATCHER_VERSION=v3.5.1 ./patch`.

### Updating Iosevka

The submodule is shallow, so fetch the tag you want before checking it out:

```sh
git -C iosevka fetch --depth 1 origin tag v34.8.1
git -C iosevka checkout v34.8.1
git commit -am "Update Iosevka to v34.8.1"
```
