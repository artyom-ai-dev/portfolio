<div align="center">
  <img src="../assets/case-ai.png" width="72%" alt="OCR чертежей" />
</div>

# 05 · OCR чертежей → структурированные данные

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Проблема
Сканы чертежей (PDF без текстового слоя) живут в архиве, а технические требования, паспорт и поля штампа приходится переносить в ERP/PLM вручную. Ошибки и задержки на разборе первого листа.

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Роль
Backend / document-AI инженер — HTTP API, пайплайн PDF→ROI→OCR→parse, Docker, контракт JSON для интеграции.

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Что сделал
Микросервис извлечения с первого листа чертежа:
- рендер PDF (PyMuPDF) с настраиваемым DPI
- эвристическая детекция зон ТТ и основной надписи (ГОСТ-раскладка, без жёстких пикселей под один формат)
- OCR (Tesseract `rus+eng`) за протоколом `OcrEngine` — можно сменить движок без смены API
- парсинг нумерованных ТТ, паспорта, обозначения, массы, масштаба, листов
- REST: `POST /v1/drawings/extract` → структурированный JSON + `meta.warnings` / confidence
- health/ready, лимит upload, Docker Compose как основной деплой

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Ограничения
- v1: только первый лист (ТТ + штамп), без геометрии и спецификаций на других листах
- качество зависит от скана; частичный успех — HTTP 200 с warnings, не «тихий» провал
- без хранения PDF/результатов в БД и без LLM как основного движка

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Поток

```mermaid
flowchart LR
  PDF[PDF скан] --> R[Рендер листа 1]
  R --> ROI[ROI: ТТ + штамп]
  ROI --> OCR[OCR]
  OCR --> P[Парсер]
  P --> JSON[JSON для ERP/PLM]
```

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Стек
Python, FastAPI, Pydantic, PyMuPDF, Tesseract, Docker Compose

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Связанные приватные репо
`drawing-ocr-service`

<img src="https://raw.githubusercontent.com/artyom-ai-dev/artyom-ai-dev/main/assets/divider.svg?v=6" width="100%" alt="" />

## Результат
Чертёж по API превращается в машиночитаемые поля (ТТ, паспорт, штамп) с явной диагностикой качества — готово к подключению в производственный контур.
