# .agents

Скиллы для агента. Скилл это каталог в `skills/` с файлом `SKILL.md`, в шапке которого `name` и `description`. Описание называет ситуацию, в которой скилл нужен.

Claude Code ищет скиллы в `.claude/skills`, это ссылка на `.agents/skills`, поэтому скиллы лежат в одном месте. Подробности в [docs/agents/agent-settings.md](../docs/agents/agent-settings.md).

| Скилл | Когда нужен |
| --- | --- |
| `add-ast-grep-rule` | Общее соглашение для шаблонов нужно проверять кодом |
