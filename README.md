# blogwatcher

Blog and RSS feed monitoring CLI for OpenCharly images.

The `blogwatcher` candy installs [blogwatcher](https://github.com/Hyaxia/blogwatcher)
from source with `go install`, landing the `blogwatcher` cobra CLI at
`~/go/bin/blogwatcher`. It adds, scans, and reads RSS/Atom feeds from the
terminal. The candy is a single `go install` step plus its verifiable assertions;
no service and no daemon.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `blogwatcher` |
| Binary | `~/go/bin/blogwatcher` (installed via `go install`) |
| Requires | `layer-golang` |
| Environment | `GOPATH=~/go`, PATH append `~/go/bin` |
| Service / port | none |

## How to use it

Compose the layer by pinning this repo in a box's `candy:` list:

```yaml
my-box:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-blogwatcher:v2026.243.0408'
```

Then, inside the built image:

```bash
blogwatcher add <feed-url>    # register a blog/feed
blogwatcher scan              # fetch new articles
blogwatcher blogs             # list registered feeds
blogwatcher articles          # list fetched articles
blogwatcher read              # read articles
```

## Layout

- `charly.yml` — the candy manifest: the `go install` `run:` step, the `GOPATH`
  environment and PATH append, an ordered `plan:` of build-time `check:` steps,
  and the embedded `skill:` entity.
- `.github/workflows/` — the org-wide `charly/pr-validator` gate; no per-repo candy gate.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-tools:blogwatcher`
- Requires: `/charly-coder:golang`
- Bundled by: `/charly-openclaw:openclaw-full`
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
