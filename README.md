# chatbotattendant-docs

Documentação de Assistente Virtual de Atendimento N1 para apoiar a implementação de sistemas de gestão.

Publicada em: **https://instituto-unibanco.github.io/chatbotattendant-docs/**

## Estrutura

```
docs/
├── index.md                          # Visão geral
├── arquitetura/
│   ├── visao-geral.md                # Pilha tecnológica
│   └── custos.md                     # Custos operacionais
├── uso/
│   ├── configuracao.md               # Variáveis de ambiente
│   ├── execucao-local.md             # Execução com Docker
│   └── execucao-producao.md          # Deploy em produção
├── integracao/
│   ├── sgp.md                        # Endpoints e integração com o SGP
│   └── autenticacao.md               # Cognito / Keycloak
├── governanca/
│   ├── licenca.md                    # Licença IU → MEC
│   └── base-de-conhecimento.md       # Gestão e atualização da base RAG
├── seguranca.md                      # Vulnerabilidades e testes
└── internalizacao.md                 # Guia para a STIC/CGGA
```

## Desenvolvimento local

```bash
pip install zensical
zensical serve
```

A documentação ficará disponível em `http://localhost:8000`.

## Deploy

O deploy é automático via GitHub Actions a cada push na branch `main`. Ver `.github/workflows/docs.yml`.

## Repositório do código-fonte

https://github.com/instituto-unibanco/chatbotattendant
