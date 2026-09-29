<div align="center">
  <img src="assets/portfolio-hero.png" width="100%" alt="Портфолио" />
</div>

<br />

# Портфолио · Артём Тюкин

Публичные кейсы без исходников и без корпоративных секретов.

**Профиль:** [github.com/artyom-ai-dev](https://github.com/artyom-ai-dev)  
**Контакт:** [tyukin69@bk.ru](mailto:tyukin69@bk.ru) · гибрид / удалёнка · открыт к предложениям

> Артём Тюкин — fullstack / Python-инженер. Здесь только кейсы и схемы; прод-код в приватных репозиториях, разбор — на собеседовании.

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Кейсы

<table>
  <tr>
    <td align="center" width="50%">
      <a href="cases/01-ai-platform.md">
        <img src="assets/case-ai.png" width="100%" alt="AI-платформа" />
      </a><br/>
      <b><a href="cases/01-ai-platform.md">01 · AI-платформа</a></b><br/>
      <sub>RAG · агенты · встречи</sub>
    </td>
    <td align="center" width="50%">
      <a href="cases/02-messenger-tracker-sync.md">
        <img src="assets/case-integrations.png" width="100%" alt="Интеграции" />
      </a><br/>
      <b><a href="cases/02-messenger-tracker-sync.md">02 · Мессенджер ↔ трекер</a></b><br/>
      <sub>webhooks · двусторонняя синхронизация</sub>
    </td>
  </tr>
  <tr>
    <td align="center" width="50%">
      <a href="cases/03-identity-self-service.md">
        <img src="assets/case-identity.png" width="100%" alt="Identity" />
      </a><br/>
      <b><a href="cases/03-identity-self-service.md">03 · Self-service по учёткам</a></b><br/>
      <sub>LDAP · бот · автоматизация</sub>
    </td>
    <td align="center" width="50%">
      <a href="cases/04-excel-web-automation.md">
        <img src="assets/case-excel.png" width="100%" alt="Excel" />
      </a><br/>
      <b><a href="cases/04-excel-web-automation.md">04 · Веб + Excel</a></b><br/>
      <sub>Flask · pandas · openpyxl</sub>
    </td>
  </tr>
</table>

| # | Кейс | Тип | Стек |
|---|------|------|------|
| 01 | [AI-платформа: RAG, агенты, встречи](cases/01-ai-platform.md) | AI | FastAPI · LangChain · Qdrant · LLM · STT |
| 02 | [Синхронизация мессенджер ↔ трекер](cases/02-messenger-tracker-sync.md) | Интеграции | FastAPI · Flask · webhooks · gunicorn · Docker |
| 03 | [Self-service бот по учёткам](cases/03-identity-self-service.md) | Автоматизация | Flask · LDAP/LDAPS · FSM · webhooks · gunicorn |
| 04 | [Внутренний веб + Excel-автоматизация](cases/04-excel-web-automation.md) | Fullstack / данные | Flask · LDAP · pandas · openpyxl |

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

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

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Контур интеграций

```mermaid
flowchart TB
  Tracker[Трекер задач] <-->|REST / webhooks| Bridge[Сервис интеграции]
  Digests[SLA / дайджесты] -->|notification gateway| DigestBot[bot_jira]
  Bridge <-->|bot API| Chat[Корпоративный мессенджер]
  DigestBot --> Chat
  Directory[Каталог / identity] <--> Bot[pass_bot · FSM]
  Events[События жизненного цикла] --> Alerts[usercreatealertbot]
  Bot --> Chat
  Alerts --> Chat
```

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Что показывает это портфолио

- Полный цикл: проблема → сервис → Docker → пользователи
- AI там, где меняет реальный процесс (поиск по знаниям, встречи), а не как демо
- Интеграции на границах систем: трекеры, мессенджеры, каталог, таблицы
- Корпоративные боты в прод-стиле: слои api/domain/adapters, auth webhook, TLS, без утечек секретов

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Примечания

- Только описания и схемы.
- Прод-код остаётся в приватных репозиториях; доступ — по запросу на собеседовании.
- Без паролей, токенов, внутренних URL, IP и боевых конфигов.

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Стек

`Python` · `FastAPI` · `Flask` · `gunicorn` · `Docker` · `REST/webhooks` · `LangChain` · `Qdrant` · `LLM` · `PyTorch` · `LDAP` · `pandas` · `openpyxl`
