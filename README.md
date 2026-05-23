# vdx-rubric-vodmal

Personal maturity rubric for multi-stack projects, owned by **vodmal**.

Used by [vdx](https://github.com/vodmal/vdx) — тонкая надстройка, дающая единый
интерфейс жизненного цикла (`up/down/build/test/check/fix`) и аудит/drift-детект
поверх mise.

## Использование

В корневом `mise.toml` проекта:

```toml
[vdx]
baseline = "github.com/vodmal/vdx-rubric-vodmal@v0.2.0"
stack    = "php"        # или "node", "python", "go", "mixed"
verbs    = ["up", "down", "build", "test", "check", "fix"]
```

`vdx audit` загружает указанную версию рубрики и сравнивает состояние проекта
с её требованиями.

## Формат

Полная спека формата — в vdx-репо:
[docs/specs/rubric-format.md](https://github.com/vodmal/vdx/blob/main/docs/specs/rubric-format.md).

## Версионирование

Semver через git-теги. `metadata.version` в [vdx-rubric.yaml](vdx-rubric.yaml)
должна совпадать с git-тегом. Текущая версия — `v0.2.0`.

## Уровни

| Level | Name | Description |
|-------|------|-------------|
| L0 | chaos | Не поднимается, знание в голове |
| L1 | reproducible | Поднимается одной задокументированной командой |
| L2 | testable | Тесты есть; единый словарь `up/test/build` |
| L3 | gated | Статанализ + стиль + CI-гейты на PR (реальные!) |
| L4 | exemplar | Полная глубина: хуки, mock-инфра, observability, версионированная общая инфра |

## Оси (14)

**Critical** (must-max для уровня): `lifecycle-interface`, `tests`,
`static-analysis`, `ci`.

**Supporting** (≥80% на максимуме для уровня): `reproducibility`, `code-style`,
`dependency-hygiene`, `secrets-config`, `git-hygiene`, `observability`, `docs`,
`mock-infra`, `shared-infra`, `shared-infra-drift`.

Полные определения — в [vdx-rubric.yaml](vdx-rubric.yaml).

## История версий

См. [CHANGELOG.md](CHANGELOG.md).
