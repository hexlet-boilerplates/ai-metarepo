# Инструменты

Документ читается перед правкой `mise.toml`, `mise.lock`, `hk.pkl`, `sgconfig.yml` и задач.

## mise

`mise.toml` объявляет инструменты (`ast-grep`, `hk`, `pkl`), список клонов в `[bootstrap.repos]` и проверку окружения в `[doctor.checks]`. Задачи лежат файлами в `mise-tasks/`. Свежий клон готов после `mise run setup`, потому что `mise run` сам доставляет недостающие инструменты и записывает доверие конфигу. Минимальная версия mise задана в `min_version`.

Задачи выполняются с рабочим каталогом в корне метарепозитория, даже если `mise run` вызван из клона. Каталог вызова хранит `MISE_ORIGINAL_CWD`. Из клона видны все задачи метарепозитория, а одноимённая задача клона перекрывает нашу.

`mise run` из клона исполняет и `mise.toml` самого клона без вопроса. Остановить это можно переменной `MISE_PARANOID=config`, а `MISE_SAFE=1` запрещает запуск задач. Обе настройки глобальные и в `mise.toml` проекта не задаются.

Проверка окружения запускается командой `mise doctor project`. Проверка самих задач идёт командой `mise tasks validate`.

## Версии

В `mise.toml` инструменты заданы как `latest`, а точные версии и контрольные суммы лежат в `mise.lock` для `linux-x64`, `linux-arm64` и `macos-arm64`. Замок нужен, потому что `hk.pkl` импортирует конфиг hk по адресу пакета с номером версии, и без замка `latest` разошёлся бы с ним. Обновление идёт командой `mise lock --bump`, после неё запускают `mise run check`. Новую платформу добавляют ключом `--platform`. `mise-action` в CI видит замок и ставит инструменты с `--locked`.

## Клоны

Список клонов в `[bootstrap.repos]`, клонирует `mise run clone`, обновляет `mise run pull`. Правила и ограничения в [repos/README.md](../../repos/README.md), причина выбора в [ADR 0004](../adr/0004-clones-via-mise-bootstrap-repos.md).

## Git-хуки

Хуки ставит `mise run setup`, то есть `hk install --mise`, шаги описаны в `hk.pkl`. Ключ `--mise` пишет в хук `mise x -- hk`, и версии берутся из `mise.toml` и `mise.lock` независимо от PATH git-клиента. Установка идёт в конкретный репозиторий, общий `core.hooksPath` не используется, потому что он переносит хуки одного репозитория на другие.

Хук метарепозитория работает только в его git. У клонов в `repos/` свой `.git` и свои хуки, поэтому коммит внутри клона наш хук не запускает. Это граница, на которой общая проверка клонов держится на задаче `mise run check`, а не на хуке.

Шаги в `hk.pkl`:

| Шаг | Когда срабатывает | Что делает |
| --- | --- | --- |
| `ast-grep-rules` | правится `ast-grep/` или `sgconfig.yml` | `ast-grep test --skip-snapshot-tests` |
| `mise-tasks` | правится `mise-tasks/` | `bash -n` по каждому файлу |
| `mise-validate` | правится `mise.toml` или `mise-tasks/` | `mise tasks validate --errors-only` |
| `mise-fmt` | правится `mise.toml` | `mise fmt --check`, исправление `mise fmt` |

Проверка самого файла: `hk validate`. Запуск без коммита: `hk check --all`.

## ast-grep

Правила в `ast-grep/rules/`, тесты в `ast-grep/rule-tests/`, конфиг `sgconfig.yml`. Запуск по клонам идёт с `--no-ignore parent`, причина в [ast-grep/README.md](../../ast-grep/README.md).

Правило добавляют скиллом `add-ast-grep-rule`. Серьёзность `error` роняет `mise run check`, `warning` только печатает. Правило по умолчанию стоит делать `error`, а исключения выносить в `ignores`, иначе находки копятся и их перестают читать.

## CI

Workflow `.github/workflows/check.yml` ставит инструменты через `mise`, проверяет `mise.toml`, задачи и `hk.pkl` командой `hk check --all`, клонирует шаблоны и запускает `mise run check`. Он идёт на push в `main` и на pull request, расписания у него нет. Новый запуск на ту же ветку отменяет предыдущий, а время ограничено десятью минутами.

## Источник тулов

Тулы и общие проверки берутся из [mokevnin/dev-guardrails](https://github.com/mokevnin/dev-guardrails).

## Что не подключено

Нет релизного процесса, метарепозиторий ничего не публикует. Нет проверки Conventional Commits хуком, правило записано в `AGENTS.md` и подтверждается человеком при коммите.
