# Configuração

Cada componente do sistema é configurado via variáveis de ambiente em um arquivo `.env`. Os arquivos `.env.example` de cada repositório servem como ponto de partida — copie e preencha antes de subir qualquer serviço.

## Backend

### Banco de dados (PostgreSQL)

| Variável | Descrição | Exemplo |
|---|---|---|
| `POSTGRES_USER` | Usuário de autenticação | `sgp_user` |
| `POSTGRES_PASSWORD` | Senha do usuário | `senha_segura` |
| `POSTGRES_DB` | Nome do banco de dados | `chatbot_sgp` |
| `POSTGRES_HOST` | Hostname do serviço | `localhost` |
| `POSTGRES_PORT` | Porta TCP | `5432` |

### Cache (Redis)

| Variável | Descrição | Exemplo |
|---|---|---|
| `REDIS_URL` | URL de conexão completa | `redis://localhost:6379` |

### AWS

| Variável | Descrição |
|---|---|
| `AWS_ACCESS_KEY_ID` | Identificador da chave de acesso AWS |
| `AWS_SECRET_ACCESS_KEY` | Chave secreta associada |
| `AWS_DEFAULT_REGION` | Região padrão (ex: `us-east-1`) |

### Autenticação (Amazon Cognito)

| Variável | Descrição |
|---|---|
| `COGNITO_REGION` | Região do User Pool (ex: `us-east-1`) |
| `COGNITO_USER_POOL_ID` | ID do User Pool |
| `COGNITO_APP_CLIENT_ID` | ID do App Client |

### Comunicação com o RAG Engine

| Variável | Obrigatória | Descrição | Padrão |
|---|---|---|---|
| `RAG_HOST` | Sim | Hostname do serviço RAG | — |
| `RAG_PORT` | Sim | Porta do serviço RAG | — |
| `RAG_API_TOKEN` | Sim | Token de autenticação (deve ser igual ao configurado no RAG Engine) | — |
| `RAG_ENDPOINT` | Não | Nome do endpoint de chat | `chat-stream` |

!!! warning "Token compartilhado"
    O valor de `RAG_API_TOKEN` no backend **deve ser idêntico** ao valor de `CHAT_API_TOKEN` no RAG Engine. Valores diferentes causarão erro de autenticação 401.

### CORS

| Variável | Descrição | Exemplo |
|---|---|---|
| `ALLOWED_ORIGINS` | URLs permitidas, separadas por vírgula | `http://localhost:5173,https://sgp.gestaopresente.mec.gov.br` |

### Opcionais

| Variável | Descrição | Padrão |
|---|---|---|
| `LIMIT_DAILY_MESSAGES` | Limite de mensagens por usuário em 24h | `20` |
| `LOGGER_LEVEL` | Nível do logger | `INFO` |

---

## RAG Engine

O RAG Engine utiliza um arquivo `.env` próprio. Todas as variáveis em `.env.example` são obrigatórias.

| Variável | Descrição |
|---|---|
| `CHAT_API_TOKEN` | Token de autenticação (deve ser igual ao `RAG_API_TOKEN` do backend) |
| `LLM_PROVIDER` | Provedor de LLM (`bedrock`, `anthropic`, `gemini`, `foundry`) |
| Variáveis do provedor escolhido | Chaves de API, região, model ID — consulte `llm_factory.py` |

---

## Frontend

| Variável | Obrigatória | Descrição | Exemplo |
|---|---|---|---|
| `VITE_API_BASE_URL` | Sim | URL base do backend | `http://localhost:8000` |
| `VITE_LOGO` | Sim | Caminho ou URL do logotipo | `/assets/logo.svg` |
| `VITE_BACKGROUND` | Sim | Caminho ou URL do background da tela de login (SVG) | `/assets/bg.svg` |
