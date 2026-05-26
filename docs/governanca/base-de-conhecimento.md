# Gestão da Base de Conhecimento

## Autonomia do MEC

O MEC tem **total autonomia** para alimentar, atualizar e modificar a base de conhecimento do assistente, mesmo após o encerramento da parceria com o Instituto Unibanco. A documentação e o código-fonte permitem que a equipe técnica do Ministério opere o sistema de forma completamente independente.

## Como a base funciona

A base de conhecimento é um **banco vetorial ChromaDB** populado a partir de documentos convertidos para Markdown. O assistente responde exclusivamente com base nesses documentos — não acessa a internet nem utiliza conhecimento externo ao que foi indexado.

## Fluxo de atualização

```
Novo documento (PDF/DOCX/XLSX/MP4)
         │
         ▼
  make pipeline_assistant       ← converte para Markdown
         │
         ▼
  make pipeline_vector_storage  ← re-indexa no ChromaDB
         │
         ▼
  make run_api                  ← reinicia a API com a nova base
```

## Tipos de documento suportados

| Formato | Observação |
|---|---|
| PDF | Suporte a OCR para documentos escaneados |
| DOCX | Preserva tabelas e imagens |
| XLSX | Converte planilhas para tabelas Markdown |
| MP4 | Transcrição automática de vídeos institucionais |
| HTML | Via crawling de sites oficiais |

## Responsáveis pela curadoria

O fluxo definido durante a implantação:

| Responsabilidade | Equipe |
|---|---|
| Definição de quais documentos entram na base | MEC (Vanessa Carneiro / SEB) |
| Execução técnica da atualização | NEES/UFAL ou equipe técnica do MEC |
| Validação de qualidade das respostas | MEC + NEES |

## Guardrails e segurança da base

- O assistente responde **apenas** com base nos documentos indexados.
- Documentos adicionados à base devem ser **arquivos oficiais** do MEC — documentos não oficiais podem ser utilizados como fonte pelo assistente sem distinção.
- Qualquer pessoa com acesso ao sistema de arquivos do RAG Engine pode adicionar documentos. Restrinja o acesso ao servidor adequadamente.
- Consulte a seção de [Segurança](../seguranca.md) para os riscos de envenenamento da base (*indirect prompt injection*).

## Estrutura dos materiais atuais

Os materiais iniciais são organizados por público-alvo e sistema:

**SGP — Municípios**

- Formações SGP (gravações e materiais formativos)
- Encontro Inaugural MEC Gestão Presente — Municípios

**SGP — Estados e Capitais**

- Formações
- Encontro Inaugural MEC Gestão Presente — Estados e Capitais

A principal fonte é o **drive do operador** (NEES/UFAL) e os **documentos oficiais do MEC**.
