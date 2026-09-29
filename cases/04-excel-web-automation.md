<div align="center">
  <img src="../assets/case-excel.png" width="72%" alt="Веб + Excel" />
</div>

# 04 · Внутренний веб + Excel-автоматизация

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Проблема
Повторяющаяся работа с таблицами и почтой была ручной, ошибочной и плохо стандартизированной.

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Роль
Fullstack / data automation — Flask-приложение, LDAP-авторизация, Excel-пайплайны, email-уведомления.

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Что сделал
Внутреннее Flask-приложение:
- авторизация и доступ через каталог
- загрузка/обработка Excel через pandas + openpyxl
- нормализация, matching, выгрузки
- email-уведомления по завершённым сценариям

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Ограничения
- Сотрудникам нужен простой UI, а не notebook или CLI
- Форматы таблиц плавают; matching должен быть явным и проверяемым
- Доступ — через корпоративный каталог

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Стек
Python, Flask, LDAP, pandas, openpyxl, SMTP

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Связанные приватные репо
`YGO_WEB`

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Результат
Повторяющиеся табличные операции стали сервисным сценарием с простым UI и меньшим числом ручных ошибок.
