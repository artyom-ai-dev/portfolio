<div align="center">
  <img src="../assets/case-identity.png" width="72%" alt="Self-service по учёткам" />
</div>

# 03 · Self-service бот по учёткам

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Проблема
Истекшие пароли в каталоге порождали однотипные заявки в поддержку. Нужен guided self-service прямо в корпоративном мессенджере — без ручной смены пароля админом и без хранения секретов «как попало».

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Роль
Backend / automation-инженер — conversational-бот + webhook-сервис, интеграция с каталогом, FSM, audit trail, hardening security.

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Что сделал

**pass_bot** — self-service смена просроченного пароля в мессенджере:
- webhook о истечении → чат → пошаговый FSM (логин → требования → верификация → пароль)
- политика пароля до записи в каталог
- синхронный LDAPS-modify; успех пользователю только после ответа AD
- пароль не пишется в SQLite — только short-TTL vault в памяти
- экранирование LDAP-фильтров, rate-limit верификации, auth на inbound webhook
- слои: `api` → `handlers/FSM` → `domain` → `adapters` (BotX / LDAP / persistence)
- прод-запуск через gunicorn

**usercreatealertbot** — alert-релей жизненного цикла учёток (1С/AD → HR-чат):
- парсинг событий ADD / UVOL / MOD / …
- защищённый webhook, TLS к BotX, без публичного dump PII
- пайплайн `format → notify`, отделённый от HTTP

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Архитектура

```mermaid
flowchart TB
  Expire["Событие истечения пароля"] -->|POST /pass + secret| API["api / thin routes"]
  BotXIn["Команды мессенджера"] --> API
  API --> FSM["bot/handlers · FSM"]
  FSM --> Reset["domain/reset_service"]
  Reset --> Policy["password policy"]
  Reset --> Vault["PasswordVault · memory TTL"]
  Reset --> LDAP["LdapAdClient · LDAPS"]
  FSM --> Sessions["SQLite sessions · без plaintext"]
  FSM --> BotX["BotxClient · TLS"]
  LDAP --> AD["Active Directory"]
  BotX --> Chat["Корпоративный мессенджер"]
```

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Ограничения
- Защищённое подключение к каталогу (LDAPS)
- Политика пароля проверяется локально и финально политикой AD
- Секреты только из env, fail-fast при старте
- Аудит смены — без хранения пароля
- Test-эндпоинты выключены в проде

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Поток

```text
Событие истечения
  → сообщение бота в мессенджере
  → верификация пользователя
  → новый пароль + проверки политики
  → синхронное обновление в каталоге
  → подтверждение только при LDAP success
```

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Стек
Python, Flask, gunicorn, LDAP/LDAPS, BotX/eXpress, webhooks, SQLite, Docker

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Связанные приватные репо
`pass_bot` · `usercreatealertbot`

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Результат
Типовые кейсы «пароль истёк» ушли в self-service. Контур алертов по приёму/увольнению/изменениям доставляет читаемые карточки в чат без ручного копирования из логов AD.
