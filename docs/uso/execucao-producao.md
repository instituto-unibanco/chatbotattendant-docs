# Deploy em Produção

## Frontend

O comando de build gera arquivos estáticos na pasta `/dist`:

```bash
npm run build
```

Os arquivos gerados podem ser servidos em qualquer infraestrutura estática:

- **AWS S3 + CloudFront** — recomendado para integração com a infraestrutura AWS já utilizada
- **Nginx** — servidor web em qualquer instância EC2 ou VM
- **Dataprev** — infra já contratada pelo MEC

Exemplo de configuração Nginx mínima:

```nginx
server {
    listen 80;
    server_name chatbot.sgp.mec.gov.br;

    root /var/www/chatbot/dist;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

---

## Backend

Em produção, o Docker Compose continua sendo o método recomendado. Ajuste as variáveis de ambiente para apontar para serviços gerenciados (RDS, ElastiCache, Cognito de produção).

```bash
docker compose -f compose.yml up -d
```

### Recomendações de infraestrutura

| Serviço | Opção recomendada |
|---|---|
| PostgreSQL | AWS RDS PostgreSQL ou Dataprev |
| Redis | AWS ElastiCache |
| Autenticação | Amazon Cognito |
| Logs | AWS CloudWatch (já integrado via StructLog) |

---

## RAG Engine

```bash
make run_api
```

Em produção, o processo deve ser gerenciado por um supervisor (systemd, supervisord) ou executado dentro de um container.

### Dockerfile disponível

O repositório inclui um `Dockerfile` e `docker-compose.yml` para execução containerizada do RAG Engine.

```bash
docker compose up --build -d
```

---

## Integração com o SGP

O chatbot já está integrado ao portal do SGP em:

```
https://sgp.gestaopresente.mec.gov.br/ajuda
```

A integração foi realizada via botão embarcado (*embedded*) que chama os endpoints do backend. Para detalhes técnicos da integração, consulte [Integração com o SGP](../integracao/sgp.md).
