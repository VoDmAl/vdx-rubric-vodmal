# Changelog

## v0.2.2 — 2026-05-23

Minor: `applies_to` filter for stack-specific axes (O30 partial close).

- New optional axis field `applies_to: [<stack-id>, ...]`. Если задан и
  `ctx.stack` не в списке — ось получает `drift_kind: excluded` и не учитывается
  в overall scoring. Не меняет существующие предикаты.
- Помечены 5 осей `applies_to: [php, node, go, python]`:
  - **critical**: `tests`, `static-analysis`
  - **supporting**: `code-style`, `dependency-hygiene`, `mock-infra`
- Не помечены (универсальные): `lifecycle-interface`, `ci`, `reproducibility`,
  `secrets-config`, `git-hygiene`, `observability`, `docs`, `shared-infra`,
  `shared-infra-drift`.
- `metadata.version` приведён в соответствие с тегом: `"0.2.0"` → `"0.2.2"`.

Эффект: meta-проекты (документация + nested CLI типа vdx) больше не получают
ложный L0 на критических stack-специфичных осях; они теперь `excluded` и
overall расчёт игнорирует их по D7 weighted_two_class. Стек-проекты
(php/node/go/python) — без изменений.

## v0.2.1 — 2026-05-23

Bugfix-релиз по итогам Шага D дотюнинга нативного evaluator.

- `mock-infra` L3: regex `mock[-_]?(server|api|service)` → `mock[-_]?[a-zA-Z0-9_-]*(server|api|service)`.
  Теперь матчит сервисы вида `mock-bookmap-api`, `mock-stripe-server` и т.п.
- `static-analysis.php.flags.phpstan_present`: расширен до `any_of` с fallback
  на наличие `phpstan.neon` / `phpstan.neon.dist` / `phpstan.dist.neon`.
  Раньше проекты с phpstan только в CI (без composer-зависимости) получали 0
  по этому флагу.

Калибровка по референсам (после правок):

| project | overall | reproducibility | static-analysis |
|---------|:-------:|:---------------:|:---------------:|
| telegram (PHP) | L2 | L2 | L4 |
| t23b (PHP) | L1 | L2 | L1 |
| bookmap (Node) | L1 | L2 | L1 |

## v0.2.0 — 2026-05-23

Initial publish.

- 14 осей: 4 critical (`lifecycle-interface`, `tests`, `static-analysis`, `ci`) +
  10 supporting (`reproducibility`, `code-style`, `dependency-hygiene`,
  `secrets-config`, `git-hygiene`, `observability`, `docs`, `mock-infra`,
  `shared-infra`, `shared-infra-drift`).
- Скоринг: weighted two-class (D7), `supporting_threshold: 0.8`.
- Stack implementations для PHP и Node/TS (ось `static-analysis` через
  `storage: flags`, D2-уточнение про ортогональные TS-флаги).
- Семантика уровней — delta-style: `levels.LN.requires` описывает только
  дельту к L(N−1); уровень оси = max непрерывный.
- Калибровка по референсным проектам (telegram / t23b / bookmap): все
  capped at L2 из-за оси `ci` (нет реального PR-гейтования).
