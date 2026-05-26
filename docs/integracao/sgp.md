# Integração com o SGP

O chatbot expõe uma **API RESTful** via backend FastAPI. A integração com o SGP pode ser feita de duas formas:

1. **Botão embarcado (atual)** — um componente web é embutido no portal do SGP apontando para a URL do frontend do chatbot.
2. **Chamada direta à API** — o SGP chama os endpoints do backend diretamente, exibindo as respostas em sua própria interface.

A integração atual está disponível em `https://sgp.gestaopresente.mec.gov.br/ajuda`.

---

## Autenticação

Todas as requisições ao backend exigem um token JWT válido emitido pelo **Amazon Cognito**. O token deve ser enviado no cabeçalho:

```http
Authorization: Bearer <token_jwt>
```

O fluxo de autenticação completo (login, cadastro, renovação de token) é gerenciado pelo frontend e pelos endpoints `/auth/*` do backend.

Para integração direta via SGP, a equipe da UFAL deve implementar o fluxo de obtenção e renovação do JWT via Cognito, ou usar o mecanismo de **Keycloak** já disponível na stack do SGP — neste caso, é necessária configuração de federação de identidade entre o Keycloak do SGP e o Cognito do chatbot. Veja [Autenticação](autenticacao.md).

---

## Endpoints principais

### Criar um novo chat

```http
POST /chats
Authorization: Bearer <token>
Content-Type: application/json
```

**Resposta:**
```json
{
  "id": "uuid-do-chat",
  "created_at": "2026-05-22T10:00:00Z"
}
```

---

### Enviar mensagem com streaming

Este é o endpoint principal. Retorna a resposta do assistente em tempo real via **Server-Sent Events (SSE)**.

```http
POST /chats/{chat_id}/messages/stream
Authorization: Bearer <token>
Content-Type: application/json

{
  "content": "Como faço para enviar frequências no SGP?"
}
```

**Resposta (stream SSE):**
```
data: {"token": "Para"}
data: {"token": " enviar"}
data: {"token": " frequências"}
...
data: {"done": true, "sources": ["Manual_SGP_v2.pdf", "Tutorial_Frequencia.docx"]}
```

Cada evento `data` contém um token da resposta. O evento final inclui as fontes utilizadas.

---

### Listar histórico de mensagens de um chat

```http
GET /chats/{chat_id}/messages?page=1&limit=20
Authorization: Bearer <token>
```

---

### Registrar feedback

```http
POST /chats/{chat_id}/messages/{message_id}/feedback
Authorization: Bearer <token>
Content-Type: application/json

{
  "rating": "positive"  // "positive" ou "negative"
}
```

---

## Documentação interativa

A documentação Swagger completa de todos os endpoints está disponível em:

```
http://<host_backend>:8000/docs
```

---

## Exemplo de integração (JavaScript)

```javascript
async function enviarMensagem(chatId, mensagem, token) {
  const response = await fetch(`/chats/${chatId}/messages/stream`, {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${token}`,
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({ content: mensagem }),
  });

  const reader = response.body.getReader();
  const decoder = new TextDecoder();

  while (true) {
    const { done, value } = await reader.read();
    if (done) break;

    const lines = decoder.decode(value).split('\n');
    for (const line of lines) {
      if (line.startsWith('data: ')) {
        const data = JSON.parse(line.slice(6));
        if (data.token) {
          // Acrescenta token à interface
          console.log(data.token);
        }
        if (data.done) {
          console.log('Fontes:', data.sources);
        }
      }
    }
  }
}
```

---

## Requisitos para a equipe da UFAL

Para que a UFAL implemente a integração no portal do SGP, são necessários:

1. **URL do backend** em produção (fornecida pelo MEC/Dataprev após deploy).
2. **Estratégia de autenticação** — federação Keycloak ↔ Cognito, ou geração de tokens de serviço. Ver [Autenticação](autenticacao.md).
3. **Liberação de CORS** — adicionar o domínio do SGP à variável `ALLOWED_ORIGINS` do backend.
4. **Acesso ao repositório** — o código-fonte está em `https://github.com/instituto-unibanco/chatbotattendant`.
