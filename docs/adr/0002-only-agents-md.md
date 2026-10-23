---
status: accepted
---

# Правила для агента живут только в AGENTS.md

В метарепозитории и в его документах нет `CLAUDE.md`, `.claude/CLAUDE.md` и `CLAUDE.local.md`. Правила пишутся в `AGENTS.md`, его читают Claude Code и Codex. Claude Code читает `AGENTS.md` сам с версии 2.1.277 ([документация](https://code.claude.com/docs/en/memory#agents-md)).

## Considered Options

**`CLAUDE.md` с импортом `@AGENTS.md`.** Работает на любой версии, но добавляет второй файл, который можно забыть обновить, и вводит в заблуждение читателя, который ищет правила.

**Символическая ссылка `CLAUDE.md` на `AGENTS.md`.** Хуже импорта. Ссылки неудобны на Windows, а запись через ссылку у некоторых инструментов запрещена.

**Только `AGENTS.md` (выбрано).** Один файл, одно место правки.

## Consequences

- Нужна версия Claude Code не ниже 2.1.277. Для Bedrock, Vertex, Foundry и шлюзов LLM нужна 2.1.281 или выше.
- Любой `CLAUDE.md`, `.claude/CLAUDE.md` или `CLAUDE.local.md` в рабочем каталоге или выше отключает чтение `AGENTS.md`. Личный `CLAUDE.local.md` разработчика тоже, поэтому правило запрещает его создавать.
- `AGENTS.md` из каталогов `--add-dir` Claude Code не читает. Поэтому клоны лежат внутри метарепозитория.
- Загрузку проверяют командой `/context` или по строке `AGENTS.md loaded` в начале сессии.
