# pre-commit

The [pre-commit](https://pre-commit.com) git-hooks toolchain for OpenCharly
images — the orchestrator plus the linters its hooks invoke.

The `pre-commit` candy installs three tools, each as a real executable at a
known path:

| Tool | What | Installed via | Path |
|---|---|---|---|
| `pre-commit` | the git-hooks orchestrator | pixi (conda-forge) | `~/.pixi/envs/default/bin/pre-commit` |
| `taplo` | TOML linter / formatter | cargo | `~/.cargo/bin/taplo` |
| `markdownlint` | Markdown linter | npm | `~/.npm-global/bin/markdownlint` |

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `pre-commit` |
| Requires | [`layer-nodejs`](https://github.com/opencharly/layer-nodejs), [`layer-rust`](https://github.com/opencharly/layer-rust) |
| Install files | `charly.yml`, `pixi.toml`, `package.json` |
| Service / port | none |

## How to use it

Compose the layer by adding this repo to a box's `candy:` composition (a `candy:` node carrying `base:` with a nested `candy:` list):

```yaml
my-dev-box:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-pre-commit:v2026.243.0409'
```

Then, inside the built image (or on a dev host):

```bash
pre-commit --version      # the orchestrator
taplo --version           # TOML linter/formatter
markdownlint --version    # Markdown linter
```

The candy's `plan:` asserts each binary exists at its fixed path and reports its
version with a clean exit — the hook toolchain is directly verifiable.

## Layout

- `charly.yml` — the `pre-commit:` candy entity (the `require:` list, the
  `package:` entry, the `cargo install taplo-cli` step, and the `check:`
  assertions) and the embedded `pre-commit-skill:` skill entity.
- `pixi.toml` / `pixi.lock` — the conda-forge environment providing
  `pre-commit`.
- `package.json` — pins `markdownlint-cli` for the global npm install.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-coder:pre-commit`
- Dependencies: `/charly-coder:nodejs`, `/charly-coder:rust`,
  `/charly-languages:pixi`
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
