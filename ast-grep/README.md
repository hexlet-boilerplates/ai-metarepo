# ast-grep

Правила, которые проверяются кодом. Общее соглашение для шаблонов записывается правилом, а не абзацем в `AGENTS.md`, потому что агент может абзац пропустить, а проверка не пропустит.

Конфиг лежит в `../sgconfig.yml`. Запуск из корня метарепозитория.

| Каталог | Что там |
| --- | --- |
| [rules/](rules/README.md) | Правила, по файлу на правило |
| [rule-tests/](rule-tests/README.md) | Тесты правил, по файлу на правило |

## Команды

```sh
ast-grep test --skip-snapshot-tests     # тесты правил
ast-grep scan --no-ignore parent repos/typescript-package   # один клон
mise run check                          # всё вместе
```

Флаг `--no-ignore parent` нужен, потому что `repos/*` лежит в `.gitignore` метарепозитория, и без флага ast-grep пропускает клоны целиком. Собственный `.gitignore` клона, например `node_modules`, при этом действует.

## Правила на сегодня

| Правило | Что ловит |
| --- | --- |
| `no-console-log-js` | `console.log` в JavaScript |
| `no-console-log-ts` | `console.log` в TypeScript |

Два файла вместо одного, потому что язык в ast-grep задаётся на правило.
