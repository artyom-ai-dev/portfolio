<div align="center">
  <img src="../assets/case-ai.png" width="72%" alt="AI-платформа" />
</div>

# 01 · AI-платформа: RAG, агенты, встречи

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Проблема
Нужен корпоративный AI-контур: ответы по внутренней базе знаний, агенты с tools, обработка встреч (аудио → текст → протокол) с учётом ограничений по данным.

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Роль
Инженер-программист / разработчик — проектирование, бэкенд, Docker, интеграции, сопровождение в проде.

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Что сделал
- Мультиагентский бэкенд на FastAPI: роутинг, tool-calling, коллекции документов, суммаризация встреч
- Индексатор знаний: страницы → очистка → чанкинг → эмбеддинги → векторная БД
- Микросервис очистки речи перед STT
- Сквозной поток встреч: сегменты записи → очистка → транскрибация → протокол

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Ограничения
- Чувствительные корпоративные данные — локальные / корпоративные LLM, где требует политика
- Нужно встраиваться в существующие источники знаний и путь записи встреч
- Сервисы должны быть в контейнерах и обслуживаемыми командой

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Поток

```mermaid
sequenceDiagram
  participant U as Пользователь
  participant A as Assistant
  participant V as Векторная БД
  participant C as Очистка аудио
  participant L as LLM

  U->>A: вопрос / файл / аудио встречи
  alt RAG
    A->>V: семантический поиск
    V-->>A: чанки
  else аудио
    A->>C: очистить аудио
    C-->>A: улучшенное аудио
  end
  A->>L: промпт + контекст / tools
  L-->>A: ответ / структурированный протокол
  A-->>U: результат
```

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Стек
Python, FastAPI, LangChain, Qdrant, эмбеддинги/TEI (`multilingual-e5-large`), Ollama/GigaChat, PyTorch (DeepFilterNet3), ffmpeg, Docker

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Связанные приватные репо
`ai-assistant` · `confluence-sync-worker` · `clean-audio-service` · участие в `Peregovornaya` (запись встреч)

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Результат
Прод-сервисы в корпоративном контуре: поиск по знаниям через RAG, протоколы встреч через аудио-пайплайн, контейнерный деплой и владение стеком.
