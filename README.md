# Portfolio · Artem Tyukin

Public case studies without source code and without corporate secrets.

**Profile:** [github.com/artyom-ai-dev](https://github.com/artyom-ai-dev)  
**Contact:** tyukin69@bk.ru

---

## Cases

| # | Case | Type |
|---|------|-----|
| 01 | [AI platform: RAG, agents, meetings](cases/01-ai-platform.md) | AI |
| 02 | [Messenger ↔ issue tracker sync](cases/02-messenger-tracker-sync.md) | Integrations |
| 03 | [Identity self-service bot](cases/03-identity-self-service.md) | Automation |
| 04 | [Internal web + Excel automation](cases/04-excel-web-automation.md) | Fullstack / data |

---

## AI contour

```mermaid
flowchart LR
  Docs[Knowledge base] --> Indexer[Indexer / embeddings]
  Indexer --> Vector[(Vector DB)]
  Vector --> Assistant[AI Assistant]
  Audio[Meeting audio] --> Clean[Audio cleanup]
  Clean --> Assistant
  Assistant --> Users[Users / answers / protocols]
```

## Integrations contour

```mermaid
flowchart TB
  Tracker[Issue tracker] <-->|REST / webhooks| Bridge[Integration service]
  Bridge <-->|bot API| Chat[Corporate messenger]
  Directory[Directory / identity] <--> Bot[Self-service bot]
  Events[Lifecycle events] --> Alerts[Alert bot]
  Bot --> Chat
  Alerts --> Chat
```

---

## Notes

- Descriptions and diagrams only.
- Production source code stays in private repositories; available on request in interviews.
- No passwords, tokens, internal URLs, IPs, or live configs.

## Stack

`Python` · `FastAPI` · `Flask` · `Docker` · `REST/webhooks` · `LangChain` · `Qdrant` · `LLM` · `PyTorch` · `LDAP` · `pandas` · `openpyxl`
