# Changelog

## v1.2.0 — 2026-10-08

Minor: the echelon channel flag for every echelon consumer, and none for a
focused session. The rubric is unchanged.

- `vdx-environment.yaml`: the `echelon-channel` rule matches a project with
  `signals/sources.yaml` (`has_file`) instead of one with `mail.watch`.
  echelon now sends every consumer "the project's slice is complete" over the
  channel, not only mail; without the flag Claude Code drops the event. On
  2026-10-08 the file is in all seven projects of echelon's `consumers.yaml`
  and in no other. New with the flag: telegram.vorobyev.name, www.t23b.org,
  global-auth-risk-model — their running sessions are reported as running
  without it until `vdx ai --restart`.
- `vdx-environment.yaml`: `echelon-channel` is marked `wakes: true` — vdx 0.23
  leaves it out for `vdx ai --focused`, a session nothing wakes. An older vdx
  ignores the key.

## v1.1.0 — 2026-10-08

Minor: a second personal hook. The rubric is unchanged.

- `vdx-environment.yaml`: `git.hooks` gets `echelon-guard` — echelon's guard
  on pre-commit, without arguments: it asks echelon whose repository it runs
  in (echelon, a consumer with its `guard.allow`, or neither — then it
  passes). Applies where the echelon clone is (`when_exists`).
  `vdx doctor --fix` writes it into `~/.gitconfig`. A repository that still
  calls the guard from its own `pre-commit` runs it twice until echelon drops
  that line.

## v1.0.0 — 2026-10-07

Major: the rubric is schema 0.3, and three axes measure what they name. Needs
vdx 0.22; an older vdx would score the new axes as L0 without a word, and vdx
0.22 refuses a schema newer than it reads.

- `ci` is the signal, read from GitHub Actions: L2 — tests run on every push
  to main/master and their failure is not masked (`|| true`,
  `continue-on-error`); a PR-only trigger does not count. L3 — CI calls the
  project's `check` and `test` tasks (or their `name:*` forms) through its
  runner. L4 — the checking job runs across a matrix, or everything CI ships
  waits for it via `needs:`. Above L1 other CI systems are "unknown". Sentry
  release and the `deploy` check name left L4 (Sentry release stays in
  `observability`).
- New critical axis `branch-protection`: main takes changes only after a
  required check. Only the hosting knows, and vdx has no hosting extension yet,
  so L3 is "unknown" for everyone; below L3 the axis asks nothing
  (`not_required`) and does not hold the overall level down.
- `git-hygiene`: L2 — the repository installs its hooks itself on a normal
  install (husky in `prepare`/`postinstall`, only `postinstall` under Yarn 2+;
  the lefthook and simple-git-hooks packages; `cghooks add` in
  `post-install-cmd`; `git config core.hooksPath` in an install step or the
  `up` task). L3 — pre-commit or pre-push calls a vocabulary task. L4 —
  pre-push calls every vocabulary task CI calls. `commit-msg`, `post-commit`,
  `post-merge` left the ladder. Whether the hooks are on in a clone is a note
  next to the axis (`clone_check: git-hooks`), not a level.
- Removed predicates, refused in a schema 0.3 set: `gh_workflow_blocks_pr`,
  `git_hook_installed`, `command_succeeds`.
- Levels L3 and L4 reworded to match.
- `vdx-environment.yaml`: `git.hooks` — personal hooks in git's user config
  (Git 2.54); the first is `vdm-crystal`, vdm's crystal gate on pre-commit.
  `vdx doctor` checks them, `vdx doctor --fix` writes them. Older vdx ignores
  the key.
- Calibration (2026-10-07): overall levels unchanged — vdx L1, telegram L2,
  t23b L1, bookmap L1, cc-vdm-plugins L0. Axis moves: vdx `ci` L4 → L2,
  telegram `ci` L2 → L4, bookmap `ci` L2 → L1 with L2 unknown (its tests are
  in GitLab CI and masked), limeflow `git-hygiene` L2 → L1.

## v0.7.0 — 2026-10-07

Minor: `vdx-environment.yaml` names the machines a project's agent runs on.

- `session.machines`: `lft`, `m3`. Before a start `vdx ai` asks the other one
  over ssh which Claude Code sessions run there and in which directories. A
  conversation started there before vdx named machines is now seen as that
  machine's and continued there, and a new conversation (`--new`, a project
  with none yet) while an agent of the project runs on the other machine
  starts only after a yes. Needs vdx 0.21; older vdx ignores the key.
- `vdx-rubric.yaml`: `metadata.version` `"0.6.0"` → `"0.7.0"` to match the tag;
  axes, levels and predicates are unchanged, so audit results do not move.

## v0.6.0 — 2026-09-30

Minor: `vdx-environment.yaml` says where the repos with authors live.

- `git.author_pool`: `~/AI Projects`, `~/PhpstormProjects`,
  `~/PhpstormProjects/git.vorobyev.name`. In a repo without its own commit
  author `vdx ai` proposes one from the authors of the repos there — by this
  repo's history, the same remote group and similar names — and Enter writes
  the first into `.git/config`; `vdx doctor` names it. Needs vdx 0.15; older
  vdx ignores the key.
- `vdx-rubric.yaml`: `metadata.version` `"0.5.0"` → `"0.6.0"` to match the tag;
  axes, levels and predicates are unchanged, so audit results do not move.

## v0.5.0 — 2026-09-30

Minor: `vdx-environment.yaml` names the project in the session.

- `session.project_names`: after `[vdx] name` in a project's `mise.toml`, the
  session's `{project}` is the first name the intercom directory of the vdm
  plugin keeps for the repo (`~/.claude/vdm/intercom/_registry/{repo}.json`,
  `names.0`) — `vodmalbot@lft` rather than `telegram_vorobyev_name@lft`.
  Without either, the repo name as before. Needs vdx 0.14; older vdx ignores
  the key.
- `vdx-rubric.yaml`: `metadata.version` `"0.4.0"` → `"0.5.0"` to match the tag;
  axes, levels and predicates are unchanged, so audit results do not move.

## v0.4.0 — 2026-09-28

Minor: the set gains a second document, `vdx-environment.yaml`.

- **New document** [vdx-environment.yaml](vdx-environment.yaml): the
  person-and-machine half of the set, read by `vdx ai` to start an agent in a
  project. `vdx-rubric.yaml` stays the project half; one tag versions both.
  - `agent`: `claude --dangerously-skip-permissions`, resumed with `--continue`.
  - `agent.when[echelon-channel]`: a project whose `signals/sources.yaml`
    declares `mail.watch` also gets
    `--dangerously-load-development-channels plugin:echelon@echelon`, and the
    confirmation dialog that flag causes on every start is answered with Enter.
  - `session`: tmux, named `{project}@{host}`.
- Never bundled into vdx: it is found through `$VDX_ENVIRONMENT` or a
  `~/.vdx-environment.yaml` symlink. Format:
  [environment-format.md](https://github.com/VoDmAl/vdx/blob/main/docs/specs/environment-format.md).
- `vdx-rubric.yaml`: `metadata.version` `"0.3.1"` → `"0.4.0"` to match the tag;
  axes, levels and predicates are unchanged, so audit results do not move.

## v0.3.1 — 2026-05-24

Patch: `applies_when` predicate on `release-artifact` (O35 close).

- New optional axis field `applies_when: <Predicate>`. When set and the
  predicate evaluates to `false` against `evalCtx`, the axis is marked
  `drift_kind: excluded` and doesn't count toward overall scoring — same
  semantics as `applies_to`-non-match. Evaluated *after* `applies_to` so
  the stack filter still gates first.
- `release-artifact` is now gated by `applies_when`: any of
  - **Node lib signal**: `package.json` exists AND `private != true` AND
    at least one of `publishConfig`/`bin`/`main`/`exports`/`module` is
    present.
  - **PHP lib signal**: `composer.json` exists AND `name` is present AND
    `type != "project"`.
  Applications (`private: true` for Node, `type: project` for PHP) get
  `excluded` instead of a noisy L1-L2 reading.
- `metadata.version` bumped: `"0.3.0"` → `"0.3.1"`.

Effect on the calibration projects (vs v0.3.0):

| project | release-artifact before | release-artifact after | overall |
|---------|:---:|:---:|:---:|
| telegram (PHP app, `type: project`) | L1 | **excluded** | L1 → L2 (restored) |
| t23b (PHP app, `type: project`)    | L1 | **excluded** | L1 (unchanged) |
| bookmap (Node app, `private: true`) | L1 | **excluded** | L1 (unchanged) |
| vdx-cli (Node lib, `private: false`, `bin`+`publishConfig`) | L4 | **L4** | L1 (unchanged) |

Telegram is restored to L2 — the false-positive from v0.3.0 is closed.

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
