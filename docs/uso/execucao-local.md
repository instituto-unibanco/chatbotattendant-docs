# Execução Local

## Pré-requisitos

- Docker e Docker Compose instalados
- Python 3.12+ (para o RAG Engine)
- Node.js 18+ (para o Frontend)
- `uv` instalado ([instalação](https://docs.astral.sh/uv/))

---

## 1. Backend

### Variáveis de ambiente

```bash
cp .env.example .env
# Edite o .env com suas credenciais
```

Consulte o guia de [Configuração](configuracao.md) para o detalhamento de cada variável.

### Subir os containers

```bash
docker compose up --build -d
```

Isso sobe três serviços: **API** (FastAPI na porta 8000), **PostgreSQL** e **Redis**.

### Aplicar migrações do banco

```bash
docker exec -it <nome_container_backend> alembic upgrade head
```

!!! tip "Encontrando o nome do container"
    Use `docker ps` para listar os containers em execução e identificar o nome correto.

A API estará disponível em `http://localhost:8000`. Documentação Swagger em `http://localhost:8000/docs`.

### Criar novas migrações (ao alterar modelos)

```bash
docker exec -it <nome_container_backend> alembic revision --autogenerate -m "descricao_da_mudanca"
```

---

## 2. RAG Engine

### Variáveis de ambiente

```bash
cp .env.example .env
# Edite o .env com as credenciais do LLM escolhido
```

### Setup completo (primeira execução)

```bash
make complete_setup
```

Isso executa em sequência:

1. **`make pipeline_assistant`** — cria o ambiente virtual `.venv_etl`, converte todos os documentos (PDF, DOCX, XLSX, MP4) para Markdown e gera logs em `assistant_etl.log`.

2. **`make pipeline_vector_storage`** — cria o ambiente virtual `.venv_chat`, indexa os documentos no ChromaDB e gera logs em `vector_storage_etl.log`.

### Iniciar a API do RAG

```bash
make run_api
```

O servidor FastAPI sobe via Uvicorn em `0.0.0.0:8001`. Logs em `api.log`.

### Comandos disponíveis

```bash
make help
```

| Comando | Descrição |
|---|---|
| `make install_uv` | Instala o gerenciador de pacotes `uv` |
| `make pipeline_assistant` | Converte documentos para Markdown |
| `make pipeline_vector_storage` | Indexa documentos no ChromaDB |
| `make complete_setup` | Executa os dois pipelines acima em sequência |
| `make run_api` | Sobe o servidor da API RAG |
| `make clean` | Remove os ambientes virtuais `.venv_chat` e `.venv_etl` |

---

## 3. Frontend

### Instalar dependências

```bash
npm install
```

### Variáveis de ambiente

```bash
cp .env.example .env
# Configure VITE_API_BASE_URL=http://localhost:8000
```

### Ambiente de desenvolvimento

```bash
npm run dev
```

O frontend estará disponível em `http://localhost:5173`.

!!! warning "Não usar em produção"
    O comando `npm run dev` é exclusivo para desenvolvimento local. Para produção, use `npm run build`.
