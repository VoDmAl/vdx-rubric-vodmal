# vdx-rubric-vodmal

A personal maturity rubric for multi-stack projects, owned by **vodmal**.

Consumed by [vdx](https://github.com/VoDmAl/vdx) — a thin layer that gives
projects a unified lifecycle interface (`up/down/build/test/check/fix`)
and an audit / drift-detection engine on top of mise.

## Usage

In the project's root `mise.toml`:

```toml
[vdx]
baseline = "github.com/VoDmAl/vdx-rubric-vodmal@v1.2.0"
stack    = "php"        # or "node", "python", "go", "meta"
verbs    = ["up", "down", "build", "test", "check", "fix"]
```

`vdx audit` loads the referenced rubric version and compares the project's
state against its requirements. From v1.0.0 the rubric is schema 0.3, which
needs vdx 0.22 or newer; an older vdx is refused rather than misreading it.

## Environment profile

[vdx-environment.yaml](vdx-environment.yaml) is the personal half of the set:
which agent `vdx ai` starts in a project, with which flags, and whether inside
tmux; and the personal hooks (`git.hooks`) — the owner's gates for every
repository on the machine, kept in git's user config, which `vdx doctor`
checks and `vdx doctor --fix` writes. It is never bundled into vdx — point vdx
at it yourself:

```bash
ln -s "/path/to/vdx-rubric-vodmal/vdx-environment.yaml" ~/.vdx-environment.yaml
# or: export VDX_ENVIRONMENT=/path/to/vdx-rubric-vodmal/vdx-environment.yaml
```

## Format

The full format specs live in the vdx repo:
[docs/specs/rubric-format.md](https://github.com/VoDmAl/vdx/blob/main/docs/specs/rubric-format.md)
and [docs/specs/environment-format.md](https://github.com/VoDmAl/vdx/blob/main/docs/specs/environment-format.md).

## Versioning

Semver via git tags; one tag versions both documents. `metadata.version` inside
[vdx-rubric.yaml](vdx-rubric.yaml) and [vdx-environment.yaml](vdx-environment.yaml)
must match the git tag. Current version: `v1.2.0`.

## Levels

| Level | Name | Description |
|-------|------|-------------|
| L0 | chaos | Doesn't come up; knowledge lives in someone's head |
| L1 | reproducible | Comes up via one documented command |
| L2 | testable | Tests exist; a unified `up/test/build` vocabulary |
| L3 | gated | Static analysis + style; CI calls the project's own tasks; main on the hosting takes only checked changes |
| L4 | exemplar | Full depth: hooks repeat CI, mock infra, observability, versioned shared infra |

## Axes (16)

**Critical** (must-max for a level): `lifecycle-interface`, `tests`,
`static-analysis`, `ci`, `branch-protection`.

**Supporting** (≥80% at the maximum for a level): `reproducibility`,
`code-style`, `dependency-hygiene`, `secrets-config`, `git-hygiene`,
`observability`, `docs`, `mock-infra`, `shared-infra`, `shared-infra-drift`,
`release-artifact`.

Full definitions live in [vdx-rubric.yaml](vdx-rubric.yaml).

## Release history

See [CHANGELOG.md](CHANGELOG.md).
