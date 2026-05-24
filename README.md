# vdx-rubric-vodmal

A personal maturity rubric for multi-stack projects, owned by **vodmal**.

Consumed by [vdx](https://github.com/VoDmAl/vdx) — a thin layer that gives
projects a unified lifecycle interface (`up/down/build/test/check/fix`)
and an audit / drift-detection engine on top of mise.

## Usage

In the project's root `mise.toml`:

```toml
[vdx]
baseline = "github.com/VoDmAl/vdx-rubric-vodmal@v0.2.2"
stack    = "php"        # or "node", "python", "go", "meta"
verbs    = ["up", "down", "build", "test", "check", "fix"]
```

`vdx audit` loads the referenced rubric version and compares the project's
state against its requirements.

## Format

The full format spec lives in the vdx repo:
[docs/specs/rubric-format.md](https://github.com/VoDmAl/vdx/blob/main/docs/specs/rubric-format.md).

## Versioning

Semver via git tags. `metadata.version` inside [vdx-rubric.yaml](vdx-rubric.yaml)
must match the git tag. Current version: `v0.2.2`.

## Levels

| Level | Name | Description |
|-------|------|-------------|
| L0 | chaos | Doesn't come up; knowledge lives in someone's head |
| L1 | reproducible | Comes up via one documented command |
| L2 | testable | Tests exist; a unified `up/test/build` vocabulary |
| L3 | gated | Static analysis + style + CI gates on PRs (real ones!) |
| L4 | exemplar | Full depth: hooks, mock infra, observability, versioned shared infra |

## Axes (14)

**Critical** (must-max for a level): `lifecycle-interface`, `tests`,
`static-analysis`, `ci`.

**Supporting** (≥80% at the maximum for a level): `reproducibility`,
`code-style`, `dependency-hygiene`, `secrets-config`, `git-hygiene`,
`observability`, `docs`, `mock-infra`, `shared-infra`, `shared-infra-drift`.

Full definitions live in [vdx-rubric.yaml](vdx-rubric.yaml).

## Release history

See [CHANGELOG.md](CHANGELOG.md).
