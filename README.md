# Especificação dos endpoints para comunicação entre serviços externos - CDE

## 1 - CRIANDO A SESSÃO

O esquema de criação de sessão entre as aplicações irá ocorrer da seguinte forma:

Acessar https://aries.orcafascio.com/sign_in e efetuar o login. No final desse documento tem uma lista de usuários que ja foram criados e podem ser utilizados.

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
Em ambas as requisições devera ser enviado no header em `Authorization` o token definido para sua aplicação (fazer a solicitação do mesmo).

## 2 - RECEBENDO ATUALIZAÇÕES REALIZADAS PELA EMPRESA

As atualizações realizadas por alterações na empresa serão enviadas para o endpoint informado através de uma requisição `POST` com conteudo em json. As alterações são baseadas em uma empresa que tem uma licença ativa do CDE. Todas as possiveis alterações e seus conteudos serão os seguintes:

> Licença da aplicação CDE criada;
```json
{
  "code" : "license_created",
  "user_id" : "string",
  "company_id" : "string",
  "company_user_id" : "string"
}
```

> Licença da aplicação CDE removida;
```json
{
  "code" : "license_removed",
  "user_id" : "string",
  "company_id" : "string",
  "company_user_id" : "string"
}
```

> Usuário criado em uma empresa;
```json
{
  "code" : "user_created",
  "user_id" : "string",
  "company_id" : "string",
  "company_user_id" : "string"
}
```

> Usuário atualizado em uma empresa;
```json
{
  "code" : "user_updated",
  "user_id" : "string",
  "company_id" : "string",
  "company_user_id" : "string"
}
```

> Usuário removido de uma empresa;
```json
{
  "code" : "user_removed",
  "user_id" : "string",
  "company_id" : "string",
  "company_user_id" : "string"
}
```

> Cargo de um usuário alterado;
```json
{
  "code" : "user_role_updated",
  "user_id" : "string",
  "company_id" : "string",
  "company_user_id" : "string"
  "role" : {
    "id" : "string",
    "code" : "string"
  }
}
```


## LISTA DE USUÁRIOS DISPONÍVEIS PARA TESTE

Senha padrão: ZXDas796611*@abc

- cruz@test.test
- elliott@test.example
- melida.schaefer@test.example
- hassan@test.test
- pete.macgyver@test.example
- rosana.leffler@test.test
- rosie.kerluke@test.test
- ty@test.test
- kelvin.heller@test.example
- sharan.legros@test.test
- hassie@test.example
- nick@test.test
- quintin.lubowitz@test.example
- wilson@test.test
- margarett@test.example
- malka.rogahn@test.test
- junior@test.test
- hong.stehr@test.example
- sean.kilback@test.example
- les.schulist@test.test
