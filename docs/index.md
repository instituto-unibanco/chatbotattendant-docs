# Chatbot SGP

Assistente virtual de atendimento N1 para apoiar a implementação do **Sistema Gestão Presente (SGP)**.

---

- **Atenda operadores do SGP com IA**

    Chatbot com RAG que responde exclusivamente com base em documentos oficiais do MEC — manuais, tutoriais e ofícios — com tom instrucional e linguagem acessível.

    [Veja o escopo e arquitetura](arquitetura/visao-geral.md)

- **Integre ao portal do SGP**

    O assistente expõe uma API RESTful com suporte a streaming. Já está integrado ao portal em `sgp.gestaopresente.mec.gov.br/ajuda`.

    [Documentação de integração](integracao/sgp.md)

- **Rode e mantenha de forma independente**

    Todo o código-fonte está disponível e licenciado para uso irrestrito pelo MEC. A documentação cobre instalação, configuração e operação autônoma.

    [Guia de execução local](uso/execucao-local.md)

- **Desenvolvido pelo Instituto Unibanco em parceria com o IAEDU/NEES-UFAL**

    Licenciado gratuitamente à União, por intermédio do Ministério da Educação, nos termos do Acordo de Cooperação nº 2/2026.

    [Termos da licença](governanca/licenca.md)

---

## Contexto institucional

O chatbot foi desenvolvido pelo **Instituto Unibanco** em parceria com o **Instituto de Inteligência Artificial para a Educação (IAEDU/NEES-UFAL)** para apoiar a implementação do SGP 2.0 junto a estados e municípios.

O software é concedido à União, por intermédio da **Secretaria de Educação Básica (SEB/MEC)**, sob licença perpétua, gratuita e não exclusiva, conforme formalizado em abril de 2026.

## Base de conhecimento

A base de conhecimento inicial foi construída a partir de documentos oficiais fornecidos pela equipe de implementação (NEES/UFAL):

| Métrica | Valor |
|---|---|
| Arquivos indexados | 111 |
| Páginas | 1.183 |
| Caracteres | ~660 mil |
| Tokens | ~209 mil |
| Tabelas | 31 |
| Imagens | 3.726 |

A base pode ser atualizada e expandida pela equipe técnica do MEC de forma independente. Veja [Gestão da Base de Conhecimento](governanca/base-de-conhecimento.md).
