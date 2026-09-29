# Портфолио · Артём Тюкин

Публичные кейсы без исходников и без корпоративных секретов.

**Профиль:** [github.com/artyom-ai-dev](https://github.com/artyom-ai-dev)  
**Контакт:** [tyukin69@bk.ru](mailto:tyukin69@bk.ru) · гибрид / удалёнка · открыт к предложениям

> Артём Тюкин — fullstack / Python-инженер. Здесь только кейсы и схемы; прод-код в приватных репозиториях, разбор — на собеседовании.

---

## Кейсы

| # | Кейс | Тип | Стек |
|---|------|------|------|
| 01 | [AI-платформа: RAG, агенты, встречи](cases/01-ai-platform.md) | AI | FastAPI · LangChain · Qdrant · LLM · STT |
| 02 | [Синхронизация мессенджер ↔ трекер](cases/02-messenger-tracker-sync.md) | Интеграции | FastAPI · webhooks · REST · Docker |
| 03 | [Self-service бот по учёткам](cases/03-identity-self-service.md) | Автоматизация | Flask · LDAP/LDAPS · webhooks |
| 04 | [Внутренний веб + Excel-автоматизация](cases/04-excel-web-automation.md) | Fullstack / данные | Flask · LDAP · pandas · openpyxl |

---

## AI-контур

```mermaid
flowchart LR
  Docs[База знаний] --> Indexer[Индексатор / эмбеддинги]
  Indexer --> Vector[(Векторная БД)]
  Vector --> Assistant[AI Assistant]
  Audio[Аудио встреч] --> Clean[Очистка аудио]
  Clean --> Assistant
  Assistant --> Users[Пользователи / ответы / протоколы]
```

## Контур интеграций

```mermaid
flowchart TB
  Tracker[Трекер задач] <-->|REST / webhooks| Bridge[Сервис интеграции]
  Bridge <-->|bot API| Chat[Корпоративный мессенджер]
  Directory[Каталог / identity] <--> Bot[Self-service бот]
  Events[События жизненного цикла] --> Alerts[Alert-бот]
  Bot --> Chat
  Alerts --> Chat
```

---

## Что показывает это портфолио

- Полный цикл: проблема → сервис → Docker → пользователи
- AI там, где меняет реальный процесс (поиск по знаниям, встречи), а не как демо
- Интеграции на границах систем: трекеры, мессенджеры, каталог, таблицы

## Примечания

- Только описания и схемы.
- Прод-код остаётся в приватных репозиториях; доступ — по запросу на собеседовании.
- Без паролей, токенов, внутренних URL, IP и боевых конфигов.

## Стек

`Python` · `FastAPI` · `Flask` · `Docker` · `REST/webhooks` · `LangChain` · `Qdrant` · `LLM` · `PyTorch` · `LDAP` · `pandas` · `openpyxl`
