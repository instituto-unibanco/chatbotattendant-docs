# Custos Operacionais

## Dependência de serviços de terceiros

O funcionamento do chatbot depende de serviços de terceiros com **cobrança por token** (entrada e saída) nas requisições à API do LLM escolhido. A licença do software é gratuita, mas a infraestrutura de nuvem e o consumo de LLM geram custos recorrentes sob responsabilidade do licenciado (MEC), conforme previsto no instrumento contratual.

**Provedor atual em produção:** AWS Bedrock com Claude Sonnet 4.5, via conta da Dataprev.

## Custo por interação

| Modelo | Input (por M tokens) | Output (por M tokens) | Custo por interação* |
|---|---|---|---|
| Claude Sonnet 4.5 (Bedrock) | US$ 3,00 | US$ 15,00 | ~US$ 0,045 |
| Gemini 3.1 Pro | US$ 2,00 | US$ 12,00 | ~US$ 0,034 |

*Referência: 5.000 tokens de entrada e 2.000 tokens de saída por interação.

## Projeções mensais

As estimativas abaixo consideram dois ambientes (desenvolvimento e produção) e câmbio de R$ 5,40/US$.

### Cenário de pico
Até 12 interações/dia · 5.000 tokens entrada · 2.000 tokens saída

| Ambiente | Mensal (US$) | Mensal (R$) | Anual (R$) |
|---|---|---|---|
| Dev | 604,69 | 3.265,33 | 39.183,91 |
| Prd | 2.959,86 | 15.983,24 | 191.798,93 |
| **Total** | **3.564,55** | **19.248,57** | **230.982,84** |

Representa ~7% do orçamento de infraestrutura do ecossistema SGP.

### Cenário reduzido
Até 12 interações/dia · 5.000 tokens entrada · 650 tokens saída

| Ambiente | Mensal (US$) | Mensal (R$) | Anual (R$) |
|---|---|---|---|
| Dev | 604,69 | 3.265,33 | 39.183,91 |
| Prd | 1.528,45 | 8.253,63 | 99.043,56 |
| **Total** | **2.133,14** | **11.518,96** | **138.227,47** |

Representa ~4,3% do orçamento de infraestrutura do ecossistema SGP.

### Cenário reduzido "férias"
Até 7 interações/dia · 3.000 tokens entrada · 650 tokens saída

| Ambiente | Mensal (US$) | Mensal (R$) | Anual (R$) |
|---|---|---|---|
| Dev | 604,69 | 3.265,33 | 39.183,91 |
| Prd | 1.221,10 | 6.593,94 | 79.127,28 |
| **Total** | **1.825,79** | **9.859,27** | **118.311,19** |

Representa ~3,6% do orçamento de infraestrutura do ecossistema SGP.

## Estratégias de otimização

- **Cache semântico** — perguntas frequentes não geram nova chamada ao LLM, reduzindo custo e latência.
- **Limite de tokens por usuário** — controlado via variável `LIMIT_DAILY_MESSAGES` no backend.
- **Escolha do provedor** — o RAG Engine suporta múltiplos provedores; modelos mais baratos (ex: Gemini) podem ser usados em desenvolvimento.
- **Aproveitamento de infraestrutura existente** — o ambiente de produção pode ser incorporado à infraestrutura Dataprev já contratada pelo MEC.

## Alternativas de financiamento

A absorção dos custos pode ocorrer via **TED (Transferência Especial de Descentralização)** para o primeiro ano de operação, com avaliação posterior de incorporação à infraestrutura total da Dataprev.
