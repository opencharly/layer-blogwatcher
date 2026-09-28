# AGENTS.md — layer-blogwatcher

Standalone candy repo for the `blogwatcher` layer. The candy lives in `charly.yml`
at the repo root: a single `go install` `run:` step that lands the `blogwatcher`
cobra CLI at `~/go/bin/blogwatcher`, the `GOPATH` environment and PATH append, an
ordered `plan:` of build-time `check:` steps, and the embedded `skill:` entity
projected into the marketplace corpus as `/charly-tools:blogwatcher`. There is no
source tree and no service of its own.

Canonical files:

- `charly.yml` — the `blogwatcher:` candy entity and the `blogwatcher-skill:` skill entity.
- `.github/workflows/` — the org-wide `charly/pr-validator` gate; there is no per-repo candy gate.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-tools:blogwatcher` — the owning skill. What the candy installs, the
  `go install` provenance, and the feed-management subcommands. Load before
  editing or troubleshooting the candy.
- `/charly-coder:golang` — the required Go runtime parent dependency whose
  `GOPATH`/`go install` behaviour this candy relies on. Load when touching the
  install path.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, `run:`, `env:`, `path_append:`). Load before
  editing any entity field or plan step.

## Build / validate / test

- `charly box validate` at the repo root — the structural check: the manifest
  must parse and validate at the installed charly.
- The merge gate is the org-wide `charly/pr-validator` (required check
  `validate / validate`); there is no per-repo candy gate.
- There is no live bed: the candy is a `go install`, so the evidence is its
  `plan:` `check:` steps, which assert the binary exists and is executable and
  that `blogwatcher --help` runs and lists its subcommands.

## Modify this repo

- Edit the `blogwatcher:` candy entity AND the `blogwatcher-skill:` skill entity
  in `charly.yml` together. The skill is the projected usage source, so an
  install or behaviour change that is not mirrored in the skill leaves the corpus
  stale.
- The install is a `run:` step; new behaviour claims go in `plan:` as observable
  `check:` steps.
- Keep `version:` at the schema stamp the pinned CI charly supports.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
