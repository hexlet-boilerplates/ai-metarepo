# ai-metarepo

Стартовый метарепозиторий для работы с ИИ-агентом. Он собирает шаблоны из организации [hexlet-boilerplates](https://github.com/hexlet-boilerplates) и хранит то, что у них общее, то есть правила для агента, спеки, решения и проверки. Код шаблонов остаётся в их репозиториях. Список клонов, правила и спеки дорабатываются под свои репозитории.

## Быстрый старт

Нужны [mise](https://mise.jdx.dev) и аккаунт GitHub, в который вы вошли через `gh`.

```sh
gh auth login
mise trust
mise run setup
mise run clone
mise run check
```

`setup` ставит инструменты из `mise.toml` и git-хуки. `clone` кладёт четыре шаблона в `repos/`. `check` запускает правила `ast-grep` по всем клонам и проверяет контракт из `specs/`.

## Что внутри

| Путь | Что там | Подробности |
| --- | --- | --- |
| `AGENTS.md` | Правила для агента и навигация по документам | читается агентом при старте |
| `GLOSSARY.md` | Термины предметной области | |
| `CODING_STANDARDS.md` | Общие стандарты кода для шаблонов | |
| `mise.toml`, `mise-tasks/` | Инструменты и задачи | [mise-tasks/README.md](mise-tasks/README.md) |
| `hk.pkl` | Git-хуки метарепозитория | [docs/agents/tooling.md](docs/agents/tooling.md) |
| `.ghorg.conf.yaml` | Какие репозитории клонировать | [repos/README.md](repos/README.md) |
| `sgconfig.yml`, `ast-grep/` | Правила, которые проверяются кодом | [ast-grep/README.md](ast-grep/README.md) |
| `specs/` | Спеки и контракты, общие для шаблонов | [specs/README.md](specs/README.md) |
| `docs/agents/` | Подробные правила для агента | [docs/agents/README.md](docs/agents/README.md) |
| `docs/adr/` | Решения, которые действуют на все репозитории | [docs/adr/README.md](docs/adr/README.md) |
| `.github/workflows/` | CI, запускает `mise run check` по клонам | [docs/agents/tooling.md](docs/agents/tooling.md) |
| `.claude/`, `.agents/` | Настройки агента и скиллы | [docs/agents/agent-settings.md](docs/agents/agent-settings.md) |
| `repos/` | Клоны шаблонов, в `.gitignore` | [repos/README.md](repos/README.md) |

## Задачи

| Команда | Что делает |
| --- | --- |
| `mise run setup` | Ставит инструменты и git-хуки |
| `mise run clone` | Клонирует шаблоны через ghorg, повторный запуск безопасен |
| `mise run pull` | Обновляет каждый клон через `git pull --rebase --autostash` |
| `mise run status` | Показывает ветку и число изменённых файлов в каждом клоне |
| `mise run check` | Тесты правил, `ast-grep scan` по клонам, проверка контракта |
| `mise run worktree <repo> <branch>` | Создаёт worktree под задачу в клоне |
| `mise run worktree-remove <repo> <branch>` | Удаляет такой worktree |

## Как работать с агентом

Запускайте агента из корня метарепозитория, подробности в [docs/agents/repository-layout.md](docs/agents/repository-layout.md). Параллельные задачи ведутся в worktrees ([docs/agents/worktrees.md](docs/agents/worktrees.md)).
