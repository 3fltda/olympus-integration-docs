# olympus-integration-docs

## Especificação dos endpoints para comunicação entre serviços externos

O esquema de criação de sessão entre as aplicações irá ocorrer da seguinte forma:

Acessar https://aries.orcafascio.com/sign_in, e ao efetuar o login, será feito o redirecionamento para https://taurus.orcafascio.com.

Na home, clicar na aplicação CDE em “Aplicações disponíveis para testar”.

![image](https://github.com/3fltda/olympus-integration-docs/assets/23532516/868116f6-9054-4de3-ac85-d1bfc892ea1e)

Ao fazer isso, a aplicação ficará disponível para uso em “Aplicações disponíveis para usar”.

Ao clicar na aplicação CDE, será feito o redirecionamento para a url cadastrada na aplicação.

![image](https://github.com/3fltda/olympus-integration-docs/assets/23532516/7d7d3e84-17a0-43f1-af7c-ba0e01aaea3a)

Junto a url será acrescentado um token temporário que deverá ser recebido pela sua aplicação para ser posteriormente redirecionado de volta.

![image](https://github.com/3fltda/olympus-integration-docs/assets/23532516/d693c409-a8bd-45f2-b8e0-41efc5dbd980)

O token tem uma expiração de 30 segundos. A ação `auth` é configurável portanto deverá informar qual ação será responsável por receber o token na sua aplicação.

Ao receber o token em seu backend, é necessário validar o mesmo enviando ele em uma requisição `PATCH` para https://charon.orcafascio.com/api/v1/remotes/customers/external_sessions com o seguinte conteudo em application/json:
> 
```json
{
  "token": "string"
}
```

Caso o token seja valido, a resposta sera a seguinte:
> http_code: 200
```json
{
  "session": {
    "public_token": "string",
    "metadata": {},
    "created_at": "string",
    "updated_at": "string"
  }
}
```

Feito isso, a sessão entre as aplicações esta estabelecida, sendo que o `public_token` devera ser enviado para cada requisição feita para os serviços internos, para poder reconhecer o usuario na sessão atual.

Para consultar se o token é valido e recuperar dados da sessão do usuário, pode ser enviado uma requisição `PATCH` para https://charon.orcafascio.com/api/v1/remotes/customers/sessions fornecendo o `public_token`:
```json
{
  "token": "string"
}
```

Caso válido, o retorno será o seguinte:
```json
{
  "session": {
    "metadata": {},
    "created_at": "string",
    "updated_at": "string",
    "user": {
      "id": "string",
      "name": "string",
      "email": "string",
      "country_code": "string",
      "phone": "string",
      "provider": "string",
      "role_id": "string",
      "created_at": "string",
      "updated_at": "string"
    },
    "company": {
      "id": "string",
      "code": "string",
      "name": "string",
      "trade_name": "string",
      "kind": "string",
      "deleted": true,
      "deleted_at": "string",
      "user_id": "string",
      "created_at": "string",
      "updated_at": "string"
    },
    "company_user": {
      "id": "string",
      "accepted_invitation": true,
      "accepted_invitation_at": "string",
      "email": "string",
      "country_code": "string",
      "phone": "string",
      "name": "string",
      "personal_code": "string",
      "business_code": "string",
      "identification": "string",
      "job": "string",
      "role_id": "string",
      "company_id": "string",
      "user_id": "string",
      "created_at": "string",
      "updated_at": "string",
      "general_role": {
        "id": "string",
        "code": "string",
        "type": "string",
        "description": "string",
        "created_at": "string",
        "updated_at": "string"
      },
      "custom_roles": [
        {
          "id": "string",
          "is_system": "string",
          "code": "string",
          "name": "string",
          "pluralized_name": "string",
          "description": "string",
          "company_id": "string",
          "permissions": [
            {
              "id": "string",
              "code": "string",
              "description": "string",
              "type": "string",
              "application_id": "string",
              "created_at": "string",
              "updated_at": "string"
            }
          ],
          "created_at": "string",
          "updated_at": "string"
        }
      ],
      "company_user_groups": [
        {
          "id": "string",
          "company_user_id": "string",
          "group_id": "string",
          "company_id": "string",
          "role_id": "string",
          "group_role": {
            "id": "string",
            "code": "string",
            "type": "string",
            "description": "string",
            "created_at": "string",
            "updated_at": "string"
          }
        }
      ],
      "permissions": [
        {
          "id": "string",
          "code": "string",
          "description": "string",
          "type": "string",
          "application_id": "string",
          "created_at": "string",
          "updated_at": "string"
        }
      ]
    }
  }
}
```
