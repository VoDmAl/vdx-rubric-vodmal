# Changelog

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
