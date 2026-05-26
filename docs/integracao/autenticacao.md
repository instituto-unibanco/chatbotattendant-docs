# Autenticação

## Mecanismo atual

O backend utiliza **Amazon Cognito** para gerenciamento de usuários e emissão de tokens JWT. O frontend implementa o fluxo completo de autenticação:

- Cadastro com confirmação de e-mail
- Login com "lembrar de mim"
- Recuperação de senha
- Renovação automática de token (refresh)
- Aceite de termos de privacidade no primeiro acesso

## Integração com Keycloak (SGP)

O SGP utiliza **Keycloak** como provedor de identidade. Para que usuários do SGP acessem o chatbot sem um segundo cadastro, há duas opções:

### Opção 1 — Federação Cognito ↔ Keycloak (recomendada)

Configure o Cognito como *Identity Provider* externo apontando para o Keycloak do SGP (via OIDC ou SAML). Desta forma, o usuário autentica no SGP e recebe um token Cognito válido para o chatbot.

Documentação AWS: [Federação com provedores externos no Cognito](https://docs.aws.amazon.com/cognito/latest/developerguide/external-identity-providers.html)

### Opção 2 — Substituição do Cognito por Keycloak

O backend valida tokens JWT. É possível substituir o Cognito pelo Keycloak do SGP ajustando o módulo `app/auth/` do backend para validar tokens emitidos pelo Keycloak.

Variáveis a ajustar:

```bash
# Substitua as variáveis COGNITO_* por:
OIDC_ISSUER=https://<keycloak-sgp>/realms/<realm>
OIDC_AUDIENCE=chatbot-sgp
```

!!! note "Ambiente de teste"
    No ambiente de teste atual (`http://18.220.237.166:5173`), o cadastro é feito diretamente no Cognito. Esse fluxo será substituído pela autenticação via SGP em produção.

## Variáveis relevantes

| Variável | Descrição |
|---|---|
| `COGNITO_REGION` | Região AWS do User Pool |
| `COGNITO_USER_POOL_ID` | ID do User Pool |
| `COGNITO_APP_CLIENT_ID` | ID do App Client |
| `ALLOWED_ORIGINS` | Domínios autorizados (CORS) — adicionar o domínio do SGP |
