# Pilha Tecnológica

## Visão geral da arquitetura

O sistema é composto por três repositórios independentes que se comunicam via API:

```
┌─────────────┐     HTTP/WS      ┌─────────────┐     HTTP      ┌─────────────┐
│   Frontend  │ ──────────────▶  │   Backend   │ ───────────▶  │  RAG Engine │
│  (React/TS) │                  │  (FastAPI)  │               │  (FastAPI)  │
└─────────────┘                  └──────┬──────┘               └──────┬──────┘
                                        │                              │
                                   ┌────┴────┐                  ┌─────┴─────┐
                                   │Postgres │                  │ ChromaDB  │
                                   │  Redis  │                  │ (vetorial)│
                                   │Cognito  │                  └───────────┘
                                   └─────────┘
```

## Componentes

### Frontend

| Item | Detalhe |
|---|---|
| Framework | React 18 |
| Linguagem | TypeScript |
| Estilização | Tailwind CSS |
| Comunicação API | Axios |
| Build | Vite |
| Deploy | Estáticos em S3/CloudFront, Nginx, ou qualquer CDN |

Funcionalidades: login, cadastro, confirmação de e-mail, aceite de termos de privacidade, interface de chat com streaming, histórico de conversas, registro de feedback, infinite scrolling.

### Backend

| Item | Detalhe |
|---|---|
| Framework | FastAPI (Python 3.12+) |
| Banco de dados | PostgreSQL via SQLAlchemy + Alembic |
| Cache | Redis |
| Autenticação | Amazon Cognito (JWT) |
| Observabilidade | StructLog + AWS CloudWatch |
| Containerização | Docker + Docker Compose |

Responsabilidades: gerenciamento de usuários, chats, mensagens, feedbacks, limite diário de mensagens, streaming de respostas via WebSocket.

### RAG Engine

| Item | Detalhe |
|---|---|
| Linguagem | Python 3.12+ |
| Gerenciador de pacotes | `uv` |
| Banco vetorial | ChromaDB |
| Orquestração | Makefile |
| Conversão de documentos | PDF, DOCX, XLSX, MP4 → Markdown |
| Servidor | FastAPI via Uvicorn (porta 8001) |

**Provedores de LLM suportados:**

- AWS Bedrock *(provedor atual em produção — Claude Sonnet 4.5 via conta Dataprev)*
- Anthropic (API direta)
- Google Gemini
- Microsoft Foundry

O provedor é configurado via variável de ambiente em `llm_factory.py`.

## Pipeline RAG

```
Documentos (PDF/DOCX/XLSX/MP4)
        │
        ▼
   Conversão para Markdown
        │
        ▼
   Chunking semântico
        │
        ▼
   Vetorização + indexação (ChromaDB)
        │
        ▼
   Consulta contextual com LLM
        │
        ▼
   Resposta com fontes citadas
```

## Desempenho

| Métrica | Valor |
|---|---|
| Latência para início do streaming | ~3 segundos (±2) |
| Latência para conclusão (respostas longas, 1000+ tokens) | ~20 segundos (±10) |
| Acurácia preliminar (N=13 perguntas) | 100% |
| Limite diário de mensagens por usuário | 20 (configurável via `LIMIT_DAILY_MESSAGES`) |

## Cache semântico

O sistema implementa cache semântico para perguntas frequentes, evitando chamadas redundantes à API do LLM e reduzindo custos operacionais.
