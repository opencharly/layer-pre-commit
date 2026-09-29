# AGENTS.md — layer-pre-commit

Standalone candy repo for the `pre-commit` layer — the pre-commit git-hooks
toolchain plus the taplo (TOML) and markdownlint (Markdown) linters its hooks
invoke. The candy lives in `charly.yml` at the repo root: the `require:` list,
the `cargo install taplo-cli` step, the `check:` assertions, and the embedded
`skill:` entity projected into the marketplace corpus as `/charly-coder:pre-commit`.

Canonical files:

- `charly.yml` — the `pre-commit:` candy entity and the `pre-commit-skill:`
  skill entity.
- `pixi.toml` / `pixi.lock` — the conda-forge environment providing
  `pre-commit`.
- `package.json` — pins `markdownlint-cli` for the global npm install.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-coder:pre-commit` — the owning skill. The three tools, their install
  paths, and the pixi/rust/nodejs dependencies. Load before editing or
  troubleshooting the candy.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, package sections, service declarations).
  Load before editing any entity field or plan step.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- `charly box validate` at the repo root checks the manifest parses and
  validates.
- The candy's `plan:` `check:` steps are the functional evidence: each of
  `pre-commit`, `taplo`, and `markdownlint` exists at its fixed path and reports
  a version with a clean exit.
- Regenerate `pixi.lock` whenever `pixi.toml` changes — the build installs with
  `pixi install --frozen` and fails loudly on a stale lock.

## Modify this repo

- Edit the `pre-commit:` candy entity AND the `pre-commit-skill:` skill entity
  in `charly.yml` together. The skill is the projected usage source, so a tool
  or path change not mirrored in the skill leaves the corpus stale.
- New behaviour claims belong in the `plan:` as an observable `check:` step, and
  in the skill body.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
