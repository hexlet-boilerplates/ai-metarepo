# ai-metarepo

Стартовый метарепозиторий для работы с ИИ-агентом. Он собирает шаблоны из организации [hexlet-boilerplates](https://github.com/hexlet-boilerplates) и хранит то, что у них общее, то есть правила для агента, спеки, решения и проверки. Код шаблонов остаётся в их репозиториях. Список клонов, правила и спеки дорабатываются под свои репозитории.

## Быстрый старт

Нужен [mise](https://mise.jdx.dev).

```sh
mise run setup
mise run clone
mise run check
```

`setup` ставит git-хуки, инструменты mise доставляет сам. `clone` кладёт шаблоны из `[bootstrap.repos]` в `repos/`. `check` запускает правила `ast-grep` по всем клонам и проверяет контракт из `specs/`.

## Что внутри

| Путь | Что там | Подробности |
| --- | --- | --- |
| `AGENTS.md` | Правила для агента и навигация по документам | читается агентом при старте |
| `GLOSSARY.md` | Термины предметной области | |
| `CODING_STANDARDS.md` | Общие стандарты кода для шаблонов | |
| `mise.toml`, `mise-tasks/` | Инструменты, список клонов и задачи | [mise-tasks/README.md](mise-tasks/README.md) |
| `hk.pkl` | Git-хуки метарепозитория | [docs/agents/tooling.md](docs/agents/tooling.md) |
| `mise.lock` | Точные версии инструментов | [docs/agents/tooling.md](docs/agents/tooling.md) |
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
| `mise run setup` | Ставит git-хуки |
| `mise run clone` | Клонирует шаблоны из `[bootstrap.repos]`, повторный запуск безопасен |
| `mise run pull` | Обновляет каждый клон через `git pull --rebase --autostash`, конфликты выводит списком |
| `mise run status` | Показывает ветку и число изменённых файлов в каждом клоне |
| `mise run check` | Тесты правил, `ast-grep scan` по клонам, проверка контракта |
| `mise run lint` | `rumdl`, `typos`, `jactionlint` и `agnix` по текстам, workflow и настройкам агента |
| `mise run worktree <repo> <branch>` | Создаёт worktree под задачу в клоне |
| `mise run worktree-remove <repo> <branch>` | Удаляет такой worktree |

## Как работать с агентом

Запускайте агента из корня метарепозитория, подробности в [docs/agents/repository-layout.md](docs/agents/repository-layout.md). Параллельные задачи ведутся в worktrees ([docs/agents/worktrees.md](docs/agents/worktrees.md)).

## Related

Похожие репозитории, статьи и инструменты. Список собран 23 октября 2026 года, часть проектов небольшие и могут устареть.

### Книга

- [Agentic Design Patterns](https://mokevnin.github.io/agentic-coding-design-patterns/), книга о паттернах работы с агентами, на английском, русском и испанском.

### Источник тулов

- [mokevnin/dev-guardrails](https://github.com/mokevnin/dev-guardrails), откуда берутся тулы и проверки для метарепозитория.

### Репозитории для работы с агентами

- [Gotop1711/multi-repos-ai-agent-workspace](https://github.com/Gotop1711/multi-repos-ai-agent-workspace), клоны в `.gitignore` под `projects/`, манифест с правами доступа по репозиториям.
- [dean0x/mars](https://github.com/dean0x/mars), рабочее место для полирепозиторной разработки с агентами, манифест `mars.yaml`.
- [Vainstel/agent-workspace](https://github.com/Vainstel/agent-workspace), один вход для агента в многорепозиторном контексте.
- [HomericIntelligence/Odysseus](https://github.com/HomericIntelligence/Odysseus), метарепозиторий на submodules с журналом ADR.
- [MichaelYochpaz/ai-workspace-template](https://github.com/MichaelYochpaz/ai-workspace-template), шаблон на submodules с общими скиллами, архивирован.
- [tapelab-io/polyrepo-brain](https://github.com/tapelab-io/polyrepo-brain) и [filipealva/virtual-workspace](https://github.com/filipealva/virtual-workspace).

### Статьи

- [The Meta-Repo Pattern](https://devnewsletter.com/p/meta-repo-pattern/) и [Operating a Meta-Repo](https://devnewsletter.com/p/operating-a-meta-repo/).
- [In Which We Give Our AI Agent a Map](https://seylox.github.io/2026/03/05/blog-agents-meta-repo-pattern.html), карта репозиториев в `AGENTS.md`.
- [The Spine Pattern](https://tsoporan.com/blog/spine-pattern-multi-repo-ai-development/).
- [The Meta-Repo as AI Multiplier](https://dev.to/jensreynderstech/the-meta-repo-as-ai-multiplier-2dda).
- [The context problem nobody solved](https://dev.to/mnzs/the-context-problem-nobody-solved-ai-agents-across-multiple-repos-4533), пять способов собрать контекст из нескольких репозиториев.
- [The missing workspace layer for agentic polyrepo development](https://dev.to/dean0x/the-missing-workspace-layer-for-agentic-polyrepo-development-pae).
- [One entry point for your coding agent](https://dev.to/vains/one-entry-point-for-your-coding-agent-organizing-multi-repo-multi-product-context-52nd).
- [A single dev workspace for AI agents](https://www.superblocks.com/blog/a-single-dev-workspace-for-ai-agents).
- [Agentic tooling across multiple repositories](https://ricky-dev.com/coding/2026/01/agentic-tooling-across-multiple-repositories/), worktrees по одному на задачу.
- [How I Use Claude Code](https://goanpeca.co/en/blog/how-i-use-claude-code/), метарепозитории, сохранение планов, журналы итераций.
- [The Omnirepo](https://www.goldsborough.io/the-omnirepo-ai-orchestration-across-enterprises/).
- [Central agent instructions via git submodules](https://blog.g9n.com/a-centralised-approach-to-ai-llm-agent-instruction-using-git-submodules).

### Монорепозиторий как альтернатива

Это позиция вендоров, измерений метарепозитория против монорепозитория там нет.

- [monorepo.tools/ai](https://monorepo.tools/ai), Nx о влиянии монорепозиториев на агентов.
- [Monorepo vs polyrepo, AI's new rules for repo architecture](https://www.augmentcode.com/guides/monorepo-vs-polyrepo-ai-s-new-rules-for-repo-architecture), Augment.

### Инструменты, которые связывают репозитории

- [mateodelnorte/meta](https://github.com/mateodelnorte/meta), исходный инструмент «meta repo».
- [west](https://docs.zephyrproject.org/latest/develop/west/workspaces.html), Zephyr, манифест `west.yml` в [zephyr](https://github.com/zephyrproject-rtos/zephyr/blob/main/west.yml).
- [repo](https://gerrit.googlesource.com/git-repo/+/HEAD/docs/manifest-format.md), Android, [манифест AOSP](https://android.googlesource.com/platform/manifest/+/refs/heads/main/default.xml).
- [git submodule](https://git-scm.com/docs/git-submodule).
- [ghorg](https://github.com/gabrie30/ghorg), клонирование организации.

### Документация агентов

- [Claude Code, AGENTS.md и память](https://code.claude.com/docs/en/memory#agents-md).
- [anthropics/claude-code#12962](https://github.com/anthropics/claude-code/issues/12962), [#26489](https://github.com/anthropics/claude-code/issues/26489), [#56853](https://github.com/anthropics/claude-code/issues/56853), запросы про несколько репозиториев.
- [openai/codex#27943](https://github.com/openai/codex/issues/27943), то же для Codex.
