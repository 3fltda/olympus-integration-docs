# Especificação dos endpoints para comunicação entre serviços externos

## 1 - CRIANDO A SESSÃO

O esquema de criação de sessão entre as aplicações irá ocorrer da seguinte forma:

Acessar https://aries.orcafascio.com/sign_in e efetuar o login. No final desse documento tem uma lista de usuários que ja foram criados e podem ser utilizados.

Na home, clicar na aplicação integrada em “Aplicações disponíveis para testar”.

![image](https://github.com/3fltda/olympus-integration-docs/assets/23532516/868116f6-9054-4de3-ac85-d1bfc892ea1e)

Ao fazer isso, a aplicação ficará disponível para uso em “Aplicações disponíveis para usar”.

Ao clicar na aplicação integrada, será feito o redirecionamento para a url cadastrada na aplicação.

![image](https://github.com/3fltda/olympus-integration-docs/assets/23532516/7d7d3e84-17a0-43f1-af7c-ba0e01aaea3a)

Junto a url será acrescentado um token temporário que deverá ser recebido pela sua aplicação para ser posteriormente redirecionado de volta.

![image](https://github.com/3fltda/olympus-integration-docs/assets/23532516/d693c409-a8bd-45f2-b8e0-41efc5dbd980)

O token tem uma expiração de 30 segundos. A ação `auth` é configurável portanto deverá informar qual ação será responsável por receber o token na sua aplicação.

Ao receber o token em seu backend, é necessário validar o mesmo enviando ele em uma requisição `PATCH` para https://charon.orcafascio.com/api/v1/remotes/customers/external_sessions com o seguinte conteudo em application/json:

```
PATCH https://charon.orcafascio.com/api/v1/remotes/customers/external_sessions
Content-Type: application/json
Authorization: <application-token>

{
  "token": "string"
}
```

Caso o token seja valido, a resposta sera a seguinte:

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

```
PATCH https://charon.orcafascio.com/api/v1/remotes/customers/sessions
Content-Type: application/json
Authorization: <application-token>

{
  "token": "public_token"
}
```

Caso válido, o retorno será o seguinte:

```json
{
  "session": {
    "id": "string",
    "token": "string",
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
              "id": "Unknown Type: String",
              "code": "Unknown Type: String",
              "description": "Unknown Type: String",
              "type": "Unknown Type: String",
              "application_id": "Unknown Type: String",
              "created_at": "Unknown Type: String",
              "updated_at": "Unknown Type: String"
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
          "id": "Unknown Type: String",
          "code": "Unknown Type: String",
          "description": "Unknown Type: String",
          "type": "Unknown Type: String",
          "application_id": "Unknown Type: String",
          "created_at": "Unknown Type: String",
          "updated_at": "Unknown Type: String"
        }
      ],
      "company_user_licenses": [
        {
          "id": "string",
          "company_id": "string",
          "company_user_id": "string",
          "application_id": "string",
          "license_id": "string",
          "application_access_id": "string",
          "application": {
            "id": "string",
            "code": "string",
            "description": "string",
            "active": true,
            "playable": true,
            "created_at": "string",
            "updated_at": "string"
          },
          "license": {
            "id": "string",
            "starts_at": "string",
            "ends_at": "string",
            "paid": true,
            "users_amount": 0,
            "expired": true,
            "approved": true,
            "approved_at": "string",
            "application_id": "string",
            "commercial_condition_id": "string",
            "company_id": "string",
            "license_kind": "string",
            "approver_id": "string",
            "approver_code": "string",
            "user_id": "string",
            "created_at": "string",
            "updated_at": "string"
          },
          "application_access": {
            "id": "string",
            "grantee_code": "string",
            "self_content": 0,
            "same_group_content": 0,
            "other_group_content": 0,
            "company_id": "string",
            "company_user_id": "string",
            "grantee_id": "string",
            "application_id": "string",
            "created_at": "string",
            "updated_at": "string"
          }
        }
      ],
      "document_accesses": [
        {
          "id": "string",
          "kind": "string",
          "document_id": "string",
          "actor_id": "string",
          "grantee_code": "string",
          "self_content": 0,
          "same_group_content": 0,
          "other_group_content": 0,
          "company_id": "string",
          "company_user_id": "string",
          "grantee_id": "string",
          "application_id": "string",
          "created_at": "string",
          "updated_at": "string"
        }
      ]
    }
  }
}
```

## 2 - RECEBENDO ATUALIZAÇÕES REALIZADAS PELA EMPRESA

As atualizações realizadas por alterações na empresa serão enviadas para o endpoint informado através de uma requisição `POST` com o conteudo em json. As alterações são baseadas em uma empresa que possui uma licença ativa da aplicação integrada.

As requisições são dividades em **sincronas** e **assincronas**. As **requisições sincronas** dependem do retorno da requisição para continuarem com o sucesso da operação, portanto se não for retornado um  `http code` maior ou igual a 200 e menor que 300, a operação sera cancelada. As **requisições assincronas** não dependem do retorno da requisição para continuarem com o sucesso da operação.

Todas as possiveis alterações e seus conteudos serão os seguintes:

### Licenças

#### Licença da aplicação criada (sincrona);
```typescript
{
  "id" : string,
  "code" : "license_created",
  "license" : {
    "id" : string,
    "license_kind" : "lifetime" | "test" | "commercial",
    "starts_at" : string,
    "ends_at" : string,
    "paid" : boolean,
    "users_amount" : number,
    "expired" : boolean,
    "approved" : boolean,
    "approved_at" : string,
    "application_id" : string,
    "commercial_condition_id" : string,
    "company_id" : string,
    "approver_id" : string,
    "approver_code" : string,
    "user_id" : string,
    "created_at" : string,
    "updated_at" : string
  }
}
```

#### Licença da aplicação atualizada (sincrona);
```typescript
{
  "id" : string,
  "code" : "license_updated",
  "license" : {
    "id" : string,
    "license_kind" : "lifetime" | "test" | "commercial",
    "starts_at" : string,
    "ends_at" : string,
    "paid" : boolean,
    "users_amount" : number,
    "expired" : boolean,
    "approved" : boolean,
    "approved_at" : string,
    "application_id" : string,
    "commercial_condition_id" : string,
    "company_id" : string,
    "approver_id" : string,
    "approver_code" : string,
    "user_id" : string,
    "created_at" : string,
    "updated_at" : string
  }
}
```

#### Licença da aplicação excluída (assincrona);
```typescript
{
  "id" : string,
  "code" : "license_destroyed",
  "license" : {
    "id" : string
  }
}
```

### Empresas

#### Empresa atualizada (assincrona);
```typescript
{
  "id" : string,
  "code" : "company_updated",
  "company" : {
    "id" : string,
    "code" : string,
    "name" : string,
    "trade_name" : string,
    "kind" : string,
    "deleted" : boolean,
    "deleted_at" : string,
    "user_id" : string,
    "created_at" : string,
    "updated_at" : string
  }
}
```

### Usuários

#### Usuário criado em uma empresa (assincrona);
```typescript
{
  "id" : string,
  "code" : "company_user_created",
  "company_user" : {
    "id" : string,
    "accepted_invitation" : boolean,
    "accepted_invitation_at" : string,
    "email" : string,
    "country_code" : string,
    "phone" : string,
    "name" : string,
    "personal_code" : string,
    "business_code" : string,
    "identification" : string,
    "job" : string,
    "enabled_login" : boolean,
    "role_id" : string,
    "company_id" : string,
    "user_id" : string,
    "created_at" : string,
    "updated_at" : string,
    "general_role" : {
      "id" : string,
      "code" : "owner" | "admin" | "collaborator" | "viewer" | "associate" | "guest" | "invited"
      "type" : string,
      "description" : string,
      "created_at" : string,
      "updated_at" : string
    }
  }
}
```

#### Usuário atualizado em uma empresa (assincrona);
```typescript
{
  "id" : string,
  "code" : "company_user_updated",
  "company_user" : {
    "id" : string,
    "accepted_invitation" : boolean,
    "accepted_invitation_at" : string,
    "email" : string,
    "country_code" : string,
    "phone" : string,
    "name" : string,
    "personal_code" : string,
    "business_code" : string,
    "identification" : string,
    "job" : string,
    "enabled_login" : boolean,
    "role_id" : string,
    "company_id" : string,
    "user_id" : string,
    "created_at" : string,
    "updated_at" : string,
    "general_role" : {
      "id" : string,
      "code" : "owner" | "admin" | "collaborator" | "viewer" | "associate" | "guest" | "invited"
      "type" : string,
      "description" : string,
      "created_at" : string,
      "updated_at" : string
    }
  }
}
```

#### Usuário excluido de uma empresa (assincrona);
```typescript
{
  "id" : string,
  "code" : "company_user_destroyed",
  "company_user" : {
    "id" : string
  }
}
```

### Setores

#### Setor criado (assincrona);
```typescript
{
  "id" : string,
  "code" : "group_created",
  "group" : {
    "id" : string,
    "name" : string,
    "company_id" : string,
    "created_at" : string,
    "updated_at" : string
  }
}
```

#### Setor atualizado (assincrona);
```typescript
{
  "id" : string,
  "code" : "group_updated",
  "group" : {
    "id" : string,
    "name" : string,
    "company_id" : string,
    "created_at" : string,
    "updated_at" : string
  }
}
```

#### Setor excluido (assincrona);
```typescript
{
  "id" : string,
  "code" : "group_destroyed",
  "group" : {
    "id" : string
  }
}
```

#### Usuario adicionado em um setor (assincrona);
```typescript
{
  "id" : string,
  "code" : "company_user_group_created",
  "group" : {
    "id" : string,
    "name" : string,
    "company_id" : string,
    "created_at" : string,
    "updated_at" : string
  },
  "company_user" : {
    "id" : string,
    "accepted_invitation" : boolean,
    "accepted_invitation_at" : string,
    "email" : string,
    "country_code" : string,
    "phone" : string,
    "name" : string,
    "personal_code" : string,
    "business_code" : string,
    "identification" : string,
    "job" : string,
    "enabled_login" : boolean,
    "role_id" : string,
    "company_id" : string,
    "user_id" : string,
    "created_at" : string,
    "updated_at" : string
  },
  "group_role" : {
    "id" : string,
    "code" : "admin" | "collaborator"
    "type" : string,
    "description" : string,
    "created_at" : string,
    "updated_at" : string
  }
}
```

#### Usuario atualizado em um setor (assincrona);
```typescript
{
  "id" : string,
  "code" : "company_user_group_updated",
  "group" : {
    "id" : string,
    "name" : string,
    "company_id" : string,
    "created_at" : string,
    "updated_at" : string
  },
  "company_user" : {
    "id" : string,
    "accepted_invitation" : boolean,
    "accepted_invitation_at" : string,
    "email" : string,
    "country_code" : string,
    "phone" : string,
    "name" : string,
    "personal_code" : string,
    "business_code" : string,
    "identification" : string,
    "job" : string,
    "enabled_login" : boolean,
    "role_id" : string,
    "company_id" : string,
    "user_id" : string,
    "created_at" : string,
    "updated_at" : string
  },
  "group_role" : {
    "id" : string,
    "code" : "admin" | "collaborator"
    "type" : string,
    "description" : string,
    "created_at" : string,
    "updated_at" : string
  }
}
```

#### Usuario excluido de um setor (assincrona);
```typescript
{
  "id" : string,
  "code" : "company_user_group_destroyed",
  "group" : {
    "id" : string
  },
  "company_user" : {
    "id" : string
  }
}
```

### Permissões

#### Permissão de acesso criada (assincrona);
```typescript
{
  "id" : string,
  "code" : "application_access_created",
  "application_access" : {
    "id" : string,
    "grantee_code" : string,
    "self_content" : number,
    "same_group_content" : number,
    "other_group_content" : number,
    "company_id" : string,
    "company_user_id" : string,
    "grantee_id" : string,
    "application_id" : string,
    "created_at" : string,
    "updated_at" : string
  }
}
```

#### Permissão de acesso atualizada (assincrona);
```typescript
{
  "id" : string,
  "code" : "application_access_updated",
  "application_access" : {
    "id" : string,
    "grantee_code" : string,
    "self_content" : number,
    "same_group_content" : number,
    "other_group_content" : number,
    "company_id" : string,
    "company_user_id" : string,
    "grantee_id" : string,
    "application_id" : string,
    "created_at" : string,
    "updated_at" : string
  }
}
```

#### Permissão de acesso excluida (assincrona);
```typescript
{
  "id" : string,
  "code" : "application_access_destroyed",
  "application_access" : {
    "id" : string
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
