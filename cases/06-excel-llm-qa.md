<div align="center">
  <img src="../assets/case-excel.png" width="72%" alt="LLM-проверка Excel" />
</div>

# 06 · LLM-проверка текстов в Excel (тысячи строк)

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Проблема
В актах о несоответствиях тысячи строк текста: орфография и сильные сокращения ломают смысл. Ручная вычитка не масштабируется; нужна пакетная проверка с понятным Excel-результатом.

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Роль
Backend / applied LLM — отдельный сервис вне чат-хаба, jobs API, промпт с эталонами, строгая последовательная обработка через локальную модель.

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Что сделал
Standalone data-agent (не часть `ai-assistant`):
- загрузка Excel → job с прогрессом (polling + SSE)
- проверка каждой строки только по двум критериям: орфография и смысловые сокращения/обрывы
- локальный LLM через OpenAI-compatible API (llama.cpp)
- выходной Excel: колонки «Оценка» / «Комментарий» + зелёная/красная заливка
- auth по токену, ретраи, финальные проходы по сбойным строкам
- Docker Compose, mock-режим для офлайн self-check без модели

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Ограничения
- Строго 1 строка → 1 запрос (параллель даёт деградацию модели)
- Прогон 8000+ строк — часы; клиент ждёт без общего таймаута
- Модель не «улучшает» текст и не оценивает стиль вне критериев 1–2
- Технические обозначения (ГОСТ, размеры, номера) не считаются ошибкой

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Поток

```mermaid
flowchart LR
  X[Excel upload] --> J[JobManager]
  J --> R[Строка N]
  R --> M[Локальный LLM]
  M --> V{verdict}
  V --> Next[Строка N+1]
  Next --> Out[Excel + заливка]
```

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Стек
Python, FastAPI, openpyxl/pandas, llama.cpp (OpenAI-compatible), Docker, SSE

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Связанные приватные репо
`ai-data-agent` (рядом по контуру: `ai-assistant` как источник той же локальной модели)

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Результат
Пакетная проверка текстов актов стала сервисом: загрузил таблицу → дождался прогресса → скачал размеченный Excel без ручного прохода тысяч строк.
