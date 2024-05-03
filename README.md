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

Ao receber o token em seu backend, é necessário validar o mesmo enviando ele em uma requisição `PATCH` para o endpoint com o seguinte conteudo em application/json:

```
PATCH https://olympus-charon-web-staging.jelastic.saveincloud.net/api/v1/remotes/customers/application_sessions
Content-Type: application/json
Remote-Authorization: <application-token>

{
  "token": "string"
}
```

Caso o token seja valido, a resposta sera a seguinte:

```json
{
  "session": {
    "id": "string",
    "token": "string",
    "metadata": {},
    "user": {
      "id": "string",
      "email": "string",
      "country_code": "string",
      "phone": "string",
      "name": "string",
      "provider": "string",
      "role_id": "string"
    },
    "created_at": "string",
    "updated_at": "string"
  }
}
```

Feito isso, a sessão entre as aplicações esta estabelecida, sendo que o `public_token` devera ser enviado para cada requisição feita para os serviços internos, para poder reconhecer o usuario na sessão atual.

Para consultar se o token é valido e recuperar dados da sessão do usuário, pode ser enviado uma requisição `GET` para o seguinte endpoint fornecendo o `token` obtido:

```
GET https://olympus-charon-web-staging.jelastic.saveincloud.net/api/v1/remotes/customers/application_sessions
Content-Type: application/json
Authorization: <token>
Remote-Authorization: <application-token>
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
          "is_system": true,
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
            "application_addons": {
              "allow_ifc_format": true,
              "allow_nwd_format": true,
              "allow_rvt_format": true,
              "allow_dxf_format": true,
              "allow_dwg_format": true,
              "allow_pdf_format": true,
              "allow_manage_point_clouds": true,
              "allow_manage_issues": true,
              "allow_manage_federated": true,
              "allow_manage_clash": true,
              "allow_manage_file_delivery": true,
              "revit_credit_amount": 0,
              "storage_amount": 0
            },
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

Os ambientes serão `production` e `staging`, conforme as seguintes urls:
```
STAGING_URL=https://olympus-charon-web-staging.jelastic.saveincloud.net
PRODUCTION_URL=https://olympus-charon-web.jelastic.saveincloud.net
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
  "data" : {
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
      "application_addons" : {
        "allow_ifc_format" : boolean,
        "allow_nwd_format" : boolean,
        "allow_rvt_format" : boolean,
        "allow_dxf_format" : boolean,
        "allow_dwg_format" : boolean,
        "allow_pdf_format" : boolean,
        "allow_manage_point_clouds" : boolean,
        "allow_manage_issues" : boolean,
        "allow_manage_federated" : boolean,
        "allow_manage_clash" : boolean,
        "allow_manage_file_delivery" : boolean,
        "revit_credit_amount" : number,
        "storage_amount" : number
      },
      "created_at" : string,
      "updated_at" : string
    }
  }
}
```

#### Licença da aplicação atualizada (sincrona);
```typescript
{
  "id" : string,
  "code" : "license_updated",
  "data" : {
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
      "application_addons" : {
        "allow_ifc_format" : boolean,
        "allow_nwd_format" : boolean,
        "allow_rvt_format" : boolean,
        "allow_dxf_format" : boolean,
        "allow_dwg_format" : boolean,
        "allow_pdf_format" : boolean,
        "allow_manage_point_clouds" : boolean,
        "allow_manage_issues" : boolean,
        "allow_manage_federated" : boolean,
        "allow_manage_clash" : boolean,
        "allow_manage_file_delivery" : boolean,
        "revit_credit_amount" : number,
        "storage_amount" : number
      },
      "created_at" : string,
      "updated_at" : string
    }
  }
}
```

#### Licença da aplicação excluída (assincrona);
```typescript
{
  "id" : string,
  "code" : "license_destroyed",
  "data" : {
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
      "application_addons" : {
        "allow_ifc_format" : boolean,
        "allow_nwd_format" : boolean,
        "allow_rvt_format" : boolean,
        "allow_dxf_format" : boolean,
        "allow_dwg_format" : boolean,
        "allow_pdf_format" : boolean,
        "allow_manage_point_clouds" : boolean,
        "allow_manage_issues" : boolean,
        "allow_manage_federated" : boolean,
        "allow_manage_clash" : boolean,
        "allow_manage_file_delivery" : boolean,
        "revit_credit_amount" : number,
        "storage_amount" : number
      },
      "created_at" : string,
      "updated_at" : string
    }
  }
}
```

### Empresas

#### Empresa atualizada (assincrona);
```typescript
{
  "id" : string,
  "code" : "company_updated",
  "data" : {
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
}
```

### Usuários

#### Usuário criado em uma empresa (assincrona);
```typescript
{
  "id" : string,
  "code" : "company_user_created",
  "data" : {
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
}
```

#### Usuário atualizado em uma empresa (assincrona);
```typescript
{
  "id" : string,
  "code" : "company_user_updated",
  "data" : {
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
}
```

#### Usuário excluido de uma empresa (assincrona);
```typescript
{
  "id" : string,
  "code" : "company_user_destroyed",
  "data" : {
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
}
```

### Setores

#### Setor criado (assincrona);
```typescript
{
  "id" : string,
  "code" : "group_created",
  "data" : {
    "group" : {
      "id" : string,
      "name" : string,
      "company_id" : string,
      "created_at" : string,
      "updated_at" : string
    }
  }
}
```

#### Setor atualizado (assincrona);
```typescript
{
  "id" : string,
  "code" : "group_updated",
  "data" : {
    "group" : {
      "id" : string,
      "name" : string,
      "company_id" : string,
      "created_at" : string,
      "updated_at" : string
    }
  }
}
```

#### Setor excluido (assincrona);
```typescript
{
  "id" : string,
  "code" : "group_destroyed",
  "data" : {
    "group" : {
      "id" : string,
      "name" : string,
      "company_id" : string,
      "created_at" : string,
      "updated_at" : string
    }
  }
}
```

#### Usuario adicionado em um setor (assincrona);
```typescript
{
  "id" : string,
  "code" : "company_user_group_created",
  "data" : {
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
}
```

#### Usuario atualizado em um setor (assincrona);
```typescript
{
  "id" : string,
  "code" : "company_user_group_updated",
  "data" : {
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
}
```

#### Usuario excluido de um setor (assincrona);
```typescript
{
  "id" : string,
  "code" : "company_user_group_destroyed",
  "data" : {
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
}
```

### Permissões

#### Permissão de acesso criada (assincrona);
```typescript
{
  "id" : string,
  "code" : "application_access_created",
  "data" : {
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
    },
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
      "application_addons" : {
        "allow_ifc_format" : boolean,
        "allow_nwd_format" : boolean,
        "allow_rvt_format" : boolean,
        "allow_dxf_format" : boolean,
        "allow_dwg_format" : boolean,
        "allow_pdf_format" : boolean,
        "allow_manage_point_clouds" : boolean,
        "allow_manage_issues" : boolean,
        "allow_manage_federated" : boolean,
        "allow_manage_clash" : boolean,
        "allow_manage_file_delivery" : boolean,
        "revit_credit_amount" : number,
        "storage_amount" : number
      },
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
  }
}
```

#### Permissão de acesso atualizada (assincrona);
```typescript
{
  "id" : string,
  "code" : "application_access_updated",
  "data" : {
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
    },
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
      "application_addons" : {
        "allow_ifc_format" : boolean,
        "allow_nwd_format" : boolean,
        "allow_rvt_format" : boolean,
        "allow_dxf_format" : boolean,
        "allow_dwg_format" : boolean,
        "allow_pdf_format" : boolean,
        "allow_manage_point_clouds" : boolean,
        "allow_manage_issues" : boolean,
        "allow_manage_federated" : boolean,
        "allow_manage_clash" : boolean,
        "allow_manage_file_delivery" : boolean,
        "revit_credit_amount" : number,
        "storage_amount" : number
      },
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
  }
}
```

#### Permissão de acesso excluida (assincrona);
```typescript
{
  "id" : string,
  "code" : "application_access_destroyed",
  "data" : {
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
    },
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
      "application_addons" : {
        "allow_ifc_format" : boolean,
        "allow_nwd_format" : boolean,
        "allow_rvt_format" : boolean,
        "allow_dxf_format" : boolean,
        "allow_dwg_format" : boolean,
        "allow_pdf_format" : boolean,
        "allow_manage_point_clouds" : boolean,
        "allow_manage_issues" : boolean,
        "allow_manage_federated" : boolean,
        "allow_manage_clash" : boolean,
        "allow_manage_file_delivery" : boolean,
        "revit_credit_amount" : number,
        "storage_amount" : number
      },
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
  }
}
```

## LISTA DE USUÁRIOS DISPONÍVEIS PARA TESTE

Senha padrão: ZXD89abcde@*

- ollie@test.example
- derek.blanda@test.example
- donald@test.example
- lulu.jacobi@test.test
- jenice.schulist@test.test
- kareen@test.test
- earl.white@test.example
- pinkie@test.example
- avery.wisozk@test.test
- rashad.towne@test.example
- adella@test.test
- dorris@test.test
- jody@test.example
- judi.turner@test.example
- pearly.schulist@test.test
- hollis@test.test
- kiana@test.test
- billie@test.example
- maryjane.schoen@test.test
- malcom.bernhard@test.example
- rod@test.test
- angie.volkman@test.test
- walker@test.test
- russel@test.example
- harry.kuhic@test.test
- henry.sanford@test.example
- eugene.kreiger@test.example
- jacquetta.rempel@test.example
- nguyet.doyle@test.example
- melania@test.example
