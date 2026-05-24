# Changelog

## v0.3.0 — 2026-05-24

Minor: new axis `release-artifact` (O34 close).

- **New axis** `release-artifact` (class: `supporting`,
  `applies_to: [node, php, ruby, python]`). Measures publish-readiness as
  metadata completeness + entry points + registry signals:
  - **L1**: required fields present (`name` + `version` + `license` for
    npm; `name` + `license` for composer).
  - **L2**: + `description` + `repository` + LICENSE file (npm) or
    + `description` + LICENSE file (composer).
  - **L3**: + `files` whitelist + entry point (`bin` / `main` / `exports`)
    for npm; or `autoload` + `type` for composer.
  - **L4**: + `publishConfig` + `homepage` + `bugs` (npm); or
    `extra.publish` (composer).
- `metadata.version` bumped: `"0.2.2"` → `"0.3.0"`.

Known design limitation: the axis applies broadly to all node/php/ruby/python
projects, including non-library apps that have no publish lifecycle. This
produces a fair-but-noisy signal for apps — they score L1-L2 instead of
being excluded. Suppress via `.vdx-overrides.yml` if the project is not
meant to be published. A future refinement (O35) will add an
`applies_when` predicate so the axis self-skips for apps.

Effect on the calibration projects (after this rubric upgrade):

| project | overall before v0.3.0 | overall after v0.3.0 |
|---------|:---:|:---:|
| telegram (PHP app) | L2 | L1 (release-artifact L1 — no publish metadata) |
| t23b (PHP app)    | L1 | L1 (unchanged) |
| bookmap (Node app) | L1 | L1 (unchanged) |
| vdx (meta + cli)  | L1 | L1 (release-artifact **L4** on cli/) |

Telegram dropped because the new axis honestly reports that a PHP
application is not a publishable artifact. This is a calibration signal,
not a bug — see O35 for the proper fix.

## v0.2.2 — 2026-05-23

Minor: `applies_to` filter for stack-specific axes (partial close of O30).

- New optional axis field `applies_to: [<stack-id>, ...]`. When set and
  `ctx.stack` is not in the list, the axis is marked
  `drift_kind: excluded` and doesn't count toward overall scoring. Does
  not change any existing predicate.
- Five axes are tagged `applies_to: [php, node, go, python]`:
  - **critical**: `tests`, `static-analysis`
  - **supporting**: `code-style`, `dependency-hygiene`, `mock-infra`
- Not tagged (universal): `lifecycle-interface`, `ci`, `reproducibility`,
  `secrets-config`, `git-hygiene`, `observability`, `docs`, `shared-infra`,
  `shared-infra-drift`.
- `metadata.version` aligned with the tag: `"0.2.0"` → `"0.2.2"`.

Effect: meta projects (documentation + a nested CLI such as vdx itself)
no longer get a false L0 on critical stack-specific axes; they are now
`excluded` and the overall computation ignores them under D7
`weighted_two_class`. Stack projects (php/node/go/python) are unchanged.

## v0.2.1 — 2026-05-23

Bugfix release after the Step D tuning of the native evaluator.

- `mock-infra` L3 regex: `mock[-_]?(server|api|service)` →
  `mock[-_]?[a-zA-Z0-9_-]*(server|api|service)`. Now matches services
  like `mock-bookmap-api`, `mock-stripe-server`, etc.
- `static-analysis.php.flags.phpstan_present`: widened to an `any_of`
  with a fallback on the presence of `phpstan.neon` /
  `phpstan.neon.dist` / `phpstan.dist.neon`. Previously, projects with
  PHPStan only in CI (no composer dependency) scored 0 on this flag.

Calibration on reference projects (after the fix):

| project | overall | reproducibility | static-analysis |
|---------|:-------:|:---------------:|:---------------:|
| telegram (PHP) | L2 | L2 | L4 |
| t23b (PHP) | L1 | L2 | L1 |
| bookmap (Node) | L1 | L2 | L1 |

## v0.2.0 — 2026-05-23

Initial publish.

- 14 axes: 4 critical (`lifecycle-interface`, `tests`, `static-analysis`,
  `ci`) + 10 supporting (`reproducibility`, `code-style`,
  `dependency-hygiene`, `secrets-config`, `git-hygiene`, `observability`,
  `docs`, `mock-infra`, `shared-infra`, `shared-infra-drift`).
- Scoring: weighted two-class (D7), `supporting_threshold: 0.8`.
- Stack implementations for PHP and Node/TS (the `static-analysis` axis
  uses `storage: flags`, the D2 refinement for orthogonal TS flags).
- Level semantics — delta-style: `levels.LN.requires` describes only the
  delta against L(N−1); the axis level is the longest continuous run.
- Calibration on reference projects (telegram / t23b / bookmap): all
  capped at L2 due to the `ci` axis (no real PR gating).
