# repos

Сюда `mise run clone` клонирует шаблоны из организации hexlet-boilerplates. Каталог в `.gitignore`, отслеживается только этот файл.

У каждого клона свой `.git`, своя ветка и свой `AGENTS.md`, если он у шаблона есть. Правки клонов коммитятся в их репозиториях.

## Какие репозитории клонируются

Список задаёт секция `[bootstrap.repos]` в `mise.toml`. Ключ это путь клона, значение это адрес.

| Клон | Стек |
| --- | --- |
| `nodejs-package` | пакет для Node.js на JavaScript |
| `typescript-package` | пакет на TypeScript |
| `vite-package` | пакет на Vite |
| `fastify-api-application` | API-приложение на Fastify |
| `fastify-nodejs-application` | веб-приложение на Fastify с сессиями и Drizzle ORM |
| `java-package` | пакет на Java и Gradle |
| `spring-boot-application` | приложение на Spring Boot |
| `csharp-package` | пакет на C# и .NET |
| `gopackage` | пакет на Go |
| `php-package` | пакет на PHP |
| `ruby-gem` | гем на Ruby |
| `python-package` | пакет на Python и uv |
| `data_analytics_package` | анализ данных в Jupyter |
| `html-boilerplate` | HTML и CSS |
| `sicp-racket` | задачи SICP на Racket |
| `ansible` | плейбук Ansible |
| `vagrant-ansible` | Vagrant и Ansible |

## Как это работает

`mise run clone` вызывает `mise bootstrap repos apply`. Готовые клоны остаются как есть, а клон с локальными правками mise не трогает и останавливается с ошибкой. `mise run pull` выполняет в каждом клоне `git pull --rebase --autostash`, поэтому клон с правками обновляется вместе с ними. Если rebase или возврат правок из stash дал конфликт, задача перечисляет такие клоны в конце и завершается с ошибкой.

Токен не нужен, адреса публичные. Причина выбора в [ADR 0004](../docs/adr/0004-clones-via-mise-bootstrap-repos.md).

Каталог должен оставаться пустым до первого клонирования, потому что `git clone` не пишет в непустой каталог. Поэтому здесь только этот README.

## Что здесь запрещено

Не создавай файлы в `repos/` руками, всё содержимое приходит из клонирования. Не клонируй репозитории в обход `[bootstrap.repos]`, иначе они не попадут в проверки и `mise doctor project`.
