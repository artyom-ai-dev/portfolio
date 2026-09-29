<div align="center">
  <img src="../assets/case-identity.png" width="72%" alt="Self-service по учёткам" />
</div>

# 03 · Self-service бот по учёткам

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Проблема
Истекшие пароли в каталоге порождали однотипные заявки в поддержку. Нужен guided self-service прямо в корпоративном мессенджере.

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Роль
Backend / automation-инженер — бот + webhook-сервис, интеграция с каталогом, базовый audit trail.

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Что сделал
Бот в мессенджере + webhook-сервис:
- принимает события об истечении пароля
- проверяет пользователя
- валидирует политику пароля
- обновляет учётные данные через защищённое подключение к каталогу
- хранит состояние сессии и базовый аудит

Связанно: алерты по событиям жизненного цикла учёток (`usercreatealertbot`).

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Ограничения
- Защищённое подключение к каталогу (LDAPS)
- Политика пароля должна проверяться до записи
- Сессия и аудит — для сопровождения

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Поток

```text
Событие истечения
  → сообщение бота в мессенджере
  → проверка пользователя
  → новый пароль + проверки политики
  → безопасное обновление в каталоге
  → подтверждение
```

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Стек
Python, Flask, LDAP/LDAPS, webhooks, SQLite, Docker

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Связанные приватные репо
`pass_bot` · `usercreatealertbot`

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Результат
Типовые кейсы «пароль истёк» ушли в self-service вместо ручной обработки поддержкой.
