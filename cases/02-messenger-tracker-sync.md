<div align="center">
  <img src="../assets/case-integrations.png" width="72%" alt="Мессенджер ↔ трекер" />
</div>

# 02 · Синхронизация мессенджер ↔ трекер

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Проблема
Поддержка была разнесена между трекером задач и корпоративным мессенджером. Комментарии, файлы и статусы копировали вручную.

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Роль
Backend / integration-инженер — дизайн сервиса, webhooks, персистентность, Docker.

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Что сделал
Сервис интеграции на FastAPI:
- входящие / исходящие webhooks
- автосоздание чатов, привязанных к задачам
- двусторонняя синхронизация комментариев и вложений
- уведомления о статусах и простой CSAT-флоу
- хранение связей чат↔задача, админ-команды, упаковка в Docker

Связанно: дайджесты SLA / очередей ServiceDesk в каналы (`bot_jira`).

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Ограничения
- Две системы учёта с разными моделями событий
- Вложения и комментарии не должны теряться ни в одну сторону
- Устойчивость к повторным webhook и частичным сбоям

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Поток

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
Python, FastAPI, REST, webhooks, SQLite, Docker

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Связанные приватные репо
`jira-to-servicedesk` · `bot_jira`

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Результат
Задачи можно вести в мессенджере end-to-end: меньше ручного копирования и потерь вложений/комментариев.
