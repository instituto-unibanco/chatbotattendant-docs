# Internalização da Solução (STIC/CGGA)

Este documento foi elaborado em resposta à solicitação da **Coordenação-Geral de Governança e Arquitetura de TIC (CGGA/STIC/MEC)** de 20 de maio de 2026, consolidando as informações necessárias para a análise de arquitetura, governança e viabilidade de internalização da solução.

---

## 1. Pilha tecnológica e LLM

O chatbot **utiliza LLM externa via API**. Não há modelo de linguagem hospedado localmente.

| Componente | Tecnologia |
|---|---|
| Backend | Python 3.12 + FastAPI |
| Banco vetorial | ChromaDB (indexação e busca semântica) |
| Banco relacional | PostgreSQL |
| Cache | Redis |
| Autenticação | Amazon Cognito |
| Frontend | React + TypeScript + Tailwind |
| Containerização | Docker + Docker Compose |

**Provedores de LLM suportados:**

| Provedor | Modelo atual em produção |
|---|---|
| AWS Bedrock | ✅ **Claude Sonnet 4.5** (via conta Dataprev) |
| Anthropic (API direta) | Suportado |
| Google Gemini | Suportado |
| Microsoft Foundry | Suportado |

A troca de provedor é feita via variável de ambiente — não requer alteração de código.

**Modelos de código aberto hospedados localmente** não estão suportados nativamente, mas podem ser integrados implementando o contrato de `llm_factory.py` com um endpoint local (ex: Ollama, vLLM).

---

## 2. Custos operacionais com terceiros

**Sim, o funcionamento depende de serviços de terceiros com custos recorrentes.**

Os custos decorrem de:

- **API do LLM** — cobrança por token (entrada e saída) a cada interação do usuário
- **Infraestrutura de nuvem** — EC2/ECS para o backend e RAG Engine, RDS para PostgreSQL, ElastiCache para Redis, Cognito para autenticação, CloudWatch para logs

A licença do **software** é gratuita. Os custos de **infraestrutura e LLM** são de responsabilidade do licenciado (MEC), conforme o instrumento contratual.

**Estimativa de custo mensal (cenário típico):** R$ 9.859 a R$ 19.248 dependendo do volume de uso. Veja o detalhamento completo em [Custos Operacionais](arquitetura/custos.md).

**Situação atual:** Os custos de LLM estão sendo absorvidos pela conta AWS da **Dataprev**, já contratada pelo MEC. Isso elimina um custo adicional para o primeiro período de operação.

---

## 3. Documentação para infraestrutura e manutenção

**O código-fonte está disponível e licenciado para uso irrestrito pelo MEC** em:

```
https://github.com/instituto-unibanco/chatbotattendant
```

A equipe técnica do MEC (ou da UFAL, via TED da SEB) tem acesso completo para manutenções futuras e análises de segurança.

**Documentação disponível neste site:**

| Tema | Página |
|---|---|
| Variáveis de ambiente e configuração | [Configuração](uso/configuracao.md) |
| Instalação e execução local | [Execução Local](uso/execucao-local.md) |
| Deploy em produção | [Deploy em Produção](uso/execucao-producao.md) |
| Arquitetura completa | [Pilha Tecnológica](arquitetura/visao-geral.md) |
| Atualização da base de conhecimento | [Base de Conhecimento](governanca/base-de-conhecimento.md) |
| Segurança e vulnerabilidades | [Segurança](seguranca.md) |

**Requisitos mínimos de infraestrutura:**

| Serviço | Especificação mínima recomendada |
|---|---|
| Backend (EC2/VM) | 2 vCPU, 4 GB RAM |
| RAG Engine (EC2/VM) | 4 vCPU, 8 GB RAM (indexação consome mais) |
| PostgreSQL | RDS db.t3.medium ou equivalente |
| Redis | cache.t3.micro ou equivalente |
| Armazenamento (ChromaDB) | 20 GB SSD para a base atual |

---

## 4. Integração com o SGP

O SGP é desenvolvido pela UFAL via TED da SEB. A integração técnica é viável e já está em produção em `sgp.gestaopresente.mec.gov.br/ajuda`.

**Para que a equipe da UFAL implemente ou mantenha a integração:**

- O backend expõe uma **API RESTful** documentada em `/docs` (Swagger)
- Os endpoints principais são `/chats` e `/chats/{id}/messages/stream`
- A autenticação é via **JWT (Bearer token)** no cabeçalho HTTP
- O streaming de respostas usa **Server-Sent Events (SSE)**

Documentação completa: [Integração com o SGP](integracao/sgp.md)

Para acesso ao repositório, contatos:

- **Instituto Unibanco:** Samara Fonteles — `samara.cunha@institutounibanco.org.br`
- **NEES/UFAL:** Filipe Dwan — `filipe.pereira@nees.ufal.br`

---

## Checklist para internalização

- [ ] Acesso ao repositório `instituto-unibanco/chatbotattendant` concedido à equipe técnica do MEC
- [ ] Infraestrutura provisionada (EC2/RDS/ElastiCache ou equivalente Dataprev)
- [ ] Variáveis de ambiente configuradas (ver [Configuração](uso/configuracao.md))
- [ ] LLM configurado — provedor e credenciais definidos
- [ ] Migrações do banco aplicadas
- [ ] Base de conhecimento indexada com documentos oficiais do MEC
- [ ] CORS configurado com o domínio do SGP em `ALLOWED_ORIGINS`
- [ ] Autenticação integrada ao Keycloak do SGP (ver [Autenticação](integracao/autenticacao.md))
- [ ] Análise de vulnerabilidades realizada pela equipe STIC
- [ ] Processo de curadoria da base de conhecimento definido com a SEB
