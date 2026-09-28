# 01 · AI Assistant + RAG + meeting pipeline

## Задача
Собрать корпоративный AI-контур: ответы по базе знаний, агенты с инструментами, обработка встреч (аудио → текст → протокол) без утечки чувствительных данных во внешние SaaS там, где это запрещено политикой.

## Что сделал
- Мультиагентский бэкенд **AI Assistant** (FastAPI): роутинг агентов, tool-calling, коллекции документов, суммаризация встреч
- **Confluence Sync Worker**: инкрементальная индексация Confluence → очистка HTML → чанкинг → эмбеддинги (TEI / multilingual-e5) → **Qdrant**
- **Clean Audio Service**: очистка речи DeepFilterNet3 + ffmpeg как отдельный микросервис
- Связка с контуром записи встреч (**Peregovornaya**): сегменты → очистка → STT → протокол

## Архитектура

```mermaid
sequenceDiagram
  participant U as User
  participant AI as AI Assistant
  participant Q as Qdrant
  participant CA as Clean Audio
  participant LLM as Ollama / GigaChat

  U->>AI: вопрос / файл / аудио встречи
  alt RAG
    AI->>Q: semantic search
    Q-->>AI: chunks
  else audio
    AI->>CA: clean audio
    CA-->>AI: enhanced wav/ogg
  end
  AI->>LLM: prompt + context / tools
  LLM-->>AI: answer / protocol JSON
  AI-->>U: результат
```

## Стек
Python, FastAPI, LangChain, Qdrant, TEI embeddings, Ollama, GigaChat, PyTorch, Docker

## Результат
Рабочий production-контур: знания из Confluence доступны агенту, встречи проходят через очистку и суммаризацию, сервисы деплоятся контейнерами и сопровождаются внутри контура предприятия.

## Репозитории
Private: `ai-assistant`, `confluence-sync-worker`, `clean-audio-service` (+ участие в `Peregovornaya`)
