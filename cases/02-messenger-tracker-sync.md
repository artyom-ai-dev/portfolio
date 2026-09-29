<div align="center">
  <img src="../assets/case-integrations.png" width="72%" alt="Мессенджер ↔ трекер" />
</div>

# 02 · Синхронизация мессенджер ↔ трекер

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Проблема
Поддержка была разнесена между трекером задач и корпоративным мессенджером. Комментарии, файлы и статусы копировали вручную; SLA-дайджесты не доходили до нужных каналов вовремя.

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Роль
Backend / integration-инженер — дизайн сервисов, webhooks, персистентность, Docker, архитектура notification gateway.

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Что сделал

**jira-to-servicedesk** — двусторонний мост трекер ↔ мессенджер:
- входящие / исходящие webhooks
- автосоздание чатов, привязанных к задачам
- синхронизация комментариев и вложений
- статусы, CSAT, админ-команды, Docker

**bot_jira** — notification gateway для SLA/дайджестов:
- `POST /jira` и `POST /servicedesk` → рассылка в проектные чаты
- маршрутизация по префиксу issue key, fan-out в канал руководителей
- daily separators в будни
- слои `api` / `domain/formatting` / `NotificationService` / `ExpressClient`
- auth на webhook, TLS к BotX, ограниченный пул потоков вместо unbounded threads
- gunicorn в проде

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Архитектура дайджестов

```mermaid
flowchart LR
  Jira["Jira / ServiceDesk digests"] -->|POST + secret| API["bot_jira API"]
  API --> Pool["ThreadPoolExecutor"]
  Pool --> Notify["NotificationService"]
  Notify --> Format["formatters"]
  Notify --> Express["ExpressClient · TLS · cache"]
  Express --> Chats["Проектные чаты eXpress"]
  Sched["SeparatorScheduler"] --> Express
```

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Ограничения
- Две системы учёта с разными моделями событий
- Вложения и комментарии не должны теряться ни в одну сторону
- Устойчивость к повторным webhook и частичным сбоям
- Дайджесты не должны утекать без auth и не раскрывать watch-list в публичных GET

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Поток синхронизации

```mermaid
flowchart LR
  T[Трекер] --> API[API / webhooks]
  API --> H[Обработчики]
  H --> TC[Клиент трекера]
  H --> MC[Клиент мессенджера]
  H --> DB[(Связи)]
  TC --> T
  MC --> M[Мессенджер]
```

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Стек
Python, FastAPI, Flask, gunicorn, REST, webhooks, BotX/eXpress, SQLite, Docker

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Связанные приватные репо
`jira-to-servicedesk` · `bot_jira`

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Результат
Задачи можно вести в мессенджере end-to-end. SLA-дайджесты и разделители приходят в нужные каналы по расписанию и правилам маршрутизации — без ручного копирования.
