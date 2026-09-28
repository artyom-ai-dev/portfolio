# 03 · AD password self-service bot

## Задача
Дать пользователям безопасную смену просроченного пароля Active Directory прямо из корпоративного мессенджера.

## Что сделал
**pass_bot** (Flask):
- приём webhook о истекающих паролях
- верификация сотрудника
- проверка сложности пароля
- смена через LDAPS
- сессии/история в SQLite, Docker, healthcheck

## Поток

```text
Webhook (password expiry)
    → сообщение в eXpress
    → верификация пользователя
    → ввод нового пароля + checks
    → LDAPS password set
    → подтверждение в чат
```

## Стек
Python, Flask, LDAP3/LDAPS, webhooks, SQLite, Docker

## Результат
Самообслуживание вместо ручных заявок в поддержку на типовом сценарии «пароль истёк».

## Репозиторий
Private: `pass_bot`  
Смежный: `usercreatealertbot` (алерты по событиям учёток)
