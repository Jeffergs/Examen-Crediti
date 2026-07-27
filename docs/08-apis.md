# 🌐 Especificação da API

<a id="indice"></a>

## Índice

1. [Objetivo](#objetivo)
2. [Escopo](#escopo)
3. [Padrões da API](#padroes-da-api)
   - [3.1 Arquitetura REST](#arquitetura-rest)
   - [3.2 Versionamento](#versionamento)
   - [3.3 Formato das Mensagens](#formato-das-mensagens)
   - [3.4 Métodos HTTP](#metodos-http)
   - [3.5 Códigos de Status HTTP](#codigos-de-status-http)
   - [3.6 Paginação](#paginacao)
   - [3.7 Ordenação](#ordenacao)
   - [3.8 Filtros](#filtros)
   - [3.9 Convenções de URL](#convencoes-de-url)
4. [Autenticação e Autorização](#autenticacao-e-autorizacao)
5. [Identity Service](#identity-service)
   - [5.1 Autenticação](#autenticacao)
   - [5.2 Usuários](#usuarios)
   - [5.3 Papéis](#papeis)
   - [5.4 Permissões](#permissoes)
6. [Customer Service](#customer-service)
   - [6.1 Clientes](#clientes)
   - [6.2 Endereços](#enderecos)
   - [6.3 Documentos](#documentos)
   - [6.4 Contatos](#contatos)
   - [6.5 Vínculos Empregatícios](#vinculos-empregaticios)
   - [6.6 Rendas](#rendas)
7. [Credit Analysis Service](#credit-analysis-service)
   - [7.1 Solicitações de Crédito](#solicitacoes-de-credito)
   - [7.2 Análises de Crédito](#analises-de-credito)
   - [7.3 Políticas de Crédito](#politicas-de-credito)
   - [7.4 Histórico de Análises](#historico-de-analises)
8. [Audit Service](#audit-service)
   - [8.1 Consulta de Eventos](#consulta-de-eventos)
9. [Notification Service](#notification-service)
   - [9.1 Consulta de Notificações](#consulta-de-notificacoes)
   - [9.2 Templates de Notificação](#templates-de-notificacao)
10. [Padrão de Respostas](#padrao-de-respostas)
11. [Tratamento de Erros](#tratamento-de-erros)
12. [Eventos da Plataforma](#eventos-da-plataforma)
13. [Considerações Finais](#consideracoes-finais)

<a id="objetivo"></a>

# 🎯 1. Objetivo

Este documento tem como objetivo especificar as interfaces de comunicação do **Examen Crediti**, definindo os padrões adotados para o consumo das APIs da plataforma.

São apresentados os recursos disponibilizados por cada microsserviço, seus respectivos endpoints, métodos HTTP, formatos de requisição e resposta, mecanismos de autenticação, códigos de status, tratamento de erros e eventos publicados pela aplicação.

Esta especificação serve como referência para o desenvolvimento, integração, testes e manutenção dos serviços, garantindo consistência na comunicação entre consumidores e provedores das APIs.

---

⬆️ [Voltar ao índice](#indice)


<a id="escopo"></a>

# 📋 2. Escopo

Este documento contempla a especificação das APIs disponibilizadas pelos microsserviços que compõem a plataforma **Examen Crediti**.

São descritos os padrões utilizados na comunicação entre clientes e serviços, bem como os contratos das operações disponibilizadas por cada domínio da aplicação.

A especificação abrange os seguintes microsserviços:

- Identity Service;
- Customer Service;
- Credit Analysis Service;
- Audit Service;
- Notification Service.

Também são apresentados:

- Os padrões arquiteturais adotados para as APIs REST;
- Os mecanismos de autenticação e autorização;
- Os formatos de requisição e resposta;
- Os códigos de status HTTP;
- O tratamento de erros;
- Os eventos publicados pela plataforma.

Este documento não contempla detalhes de implementação interna, regras de negócio ou modelagem de dados, os quais são descritos em seus respectivos documentos de arquitetura e modelagem.

---

⬆️ [Voltar ao índice](#indice)


<a id="padroes-da-api"></a>

# 🏗️ 3. Padrões da API

As APIs do **Examen Crediti** seguem os princípios da arquitetura **REST (Representational State Transfer)**, promovendo baixo acoplamento entre clientes e serviços, padronização das interfaces e facilidade de integração.

Todas as APIs utilizam o protocolo **HTTP/HTTPS** como meio de comunicação e adotam **JSON** como formato padrão para troca de mensagens.

Os recursos disponibilizados são organizados por domínio de negócio, sendo cada microsserviço responsável exclusivamente pelos endpoints relacionados às suas funcionalidades.

As seções a seguir descrevem os padrões adotados por toda a plataforma.

---

⬆️ [Voltar ao índice](#indice)

<a id="arquitetura-rest"></a>

## 🧩 3.1 Arquitetura REST

As APIs da plataforma seguem os princípios da arquitetura REST, utilizando recursos identificados por URLs, métodos HTTP padronizados e comunicação sem estado (*Stateless*).

Cada requisição contém todas as informações necessárias para seu processamento, não sendo mantido estado da sessão entre uma chamada e outra.

Os recursos são identificados por substantivos, utilizando nomes no plural e escritos em inglês.

Exemplos:

```text
/api/v1/customers

/api/v1/credit-requests

/api/v1/users

/api/v1/roles

/api/v1/permissions
```

As operações sobre os recursos são realizadas por meio dos métodos HTTP apropriados.

Sempre que possível, as APIs devem respeitar os princípios de:

- Simplicidade;
- Consistência;
- Idempotência;
- Baixo acoplamento;
- Evolução sem quebra de compatibilidade.

---

⬆️ [Voltar ao índice](#indice)

<a id="versionamento"></a>

## 🔖 3.2 Versionamento

As APIs do **Examen Crediti** utilizam versionamento por URL.

A versão da API é informada logo após o contexto principal da aplicação.

Exemplo:

```text
/api/v1/customers

/api/v1/users

/api/v1/credit-requests
```

O versionamento permite a evolução das APIs sem impactar consumidores que utilizam versões anteriores.

Sempre que houver alterações incompatíveis com versões existentes, uma nova versão da API deverá ser disponibilizada.

---

⬆️ [Voltar ao índice](#indice)

<a id="formato-das-mensagens"></a>

## 📦 3.3 Formato das Mensagens

As APIs utilizam **JSON (JavaScript Object Notation)** como formato padrão para requisições e respostas.

Os cabeçalhos HTTP devem utilizar os seguintes valores:

```http
Content-Type: application/json

Accept: application/json
```

As propriedades dos objetos JSON seguem a convenção **camelCase**.

Exemplo:

```json
{
  "firstName": "João",
  "lastName": "Silva",
  "birthDate": "1995-03-15"
}
```

Os valores de data e hora seguem o padrão **ISO 8601**.

Exemplo:

```text
2026-08-15T14:30:00Z
```

As respostas devem possuir estrutura consistente, facilitando o consumo pelos clientes da API.

---

⬆️ [Voltar ao índice](#indice)

<a id="metodos-http"></a>

## 🌐 3.4 Métodos HTTP

As operações disponibilizadas pelas APIs seguem a semântica definida pelo protocolo HTTP, utilizando o método mais adequado para cada tipo de ação sobre um recurso.

| Método | Finalidade |
|---------|------------|
| GET | Consultar um ou mais recursos. |
| POST | Criar um novo recurso. |
| PUT | Atualizar integralmente um recurso existente. |
| PATCH | Atualizar parcialmente um recurso existente. |
| DELETE | Remover um recurso. |

Sempre que possível, os métodos HTTP respeitam suas características de segurança e idempotência.

| Método | Seguro | Idempotente |
|---------|:------:|:-----------:|
| GET | ✔ | ✔ |
| POST | ✖ | ✖ |
| PUT | ✖ | ✔ |
| PATCH | ✖ | Depende da operação |
| DELETE | ✖ | ✔ |

---

⬆️ [Voltar ao índice](#indice)

<a id="codigos-de-status-http"></a>

## 📡 3.5 Códigos de Status HTTP

As APIs utilizam os códigos de status HTTP para indicar o resultado do processamento de cada requisição.

### Respostas de Sucesso

| Código | Descrição |
|---------|-----------|
| 200 OK | Requisição processada com sucesso. |
| 201 Created | Recurso criado com sucesso. |
| 204 No Content | Operação realizada com sucesso sem retorno de conteúdo. |

### Erros do Cliente

| Código | Descrição |
|---------|-----------|
| 400 Bad Request | A requisição possui dados inválidos. |
| 401 Unauthorized | O usuário não está autenticado. |
| 403 Forbidden | O usuário não possui permissão para acessar o recurso. |
| 404 Not Found | O recurso solicitado não foi encontrado. |
| 409 Conflict | Ocorreu conflito durante a operação. |
| 422 Unprocessable Entity | A requisição é válida, mas viola regras de negócio. |

### Erros do Servidor

| Código | Descrição |
|---------|-----------|
| 500 Internal Server Error | Erro interno da aplicação. |
| 503 Service Unavailable | Serviço temporariamente indisponível. |

Todos os erros retornados pelas APIs seguem um formato padronizado, descrito posteriormente neste documento.

---

⬆️ [Voltar ao índice](#indice)

<a id="paginacao"></a>

## 📄 3.6 Paginação

Os endpoints que retornam coleções de recursos suportam paginação para reduzir o volume de dados trafegados e melhorar o desempenho das consultas.

Os seguintes parâmetros de consulta são adotados como padrão:

| Parâmetro | Descrição |
|------------|-----------|
| page | Número da página. |
| size | Quantidade de registros por página. |

Exemplo:

```http
GET /api/v1/customers?page=0&size=20
```

A resposta contém a lista de registros e as informações necessárias para navegação entre as páginas.

---

⬆️ [Voltar ao índice](#indice)

<a id="ordenacao"></a>

## ↕️ 3.7 Ordenação

Os endpoints que retornam coleções permitem definir a ordenação dos resultados por meio do parâmetro `sort`.

O parâmetro informa o atributo utilizado para ordenação e sua direção.

Exemplos:

```http
GET /api/v1/customers?sort=name,asc
```

```http
GET /api/v1/customers?sort=createdAt,desc
```

Quando nenhum critério é informado, cada endpoint utiliza sua ordenação padrão.

---

⬆️ [Voltar ao índice](#indice)

<a id="filtros"></a>

## 🔎 3.8 Filtros

Os endpoints podem disponibilizar filtros para restringir os resultados retornados.

Cada recurso define os filtros compatíveis com suas regras de negócio.

Exemplo:

```http
GET /api/v1/customers?cpf=12345678901
```

```http
GET /api/v1/credit-requests?status=APPROVED
```

```http
GET /api/v1/credit-requests?status=PENDING&customerId=1
```

Os filtros são opcionais e podem ser combinados quando suportados pelo endpoint.

---

⬆️ [Voltar ao índice](#indice)

<a id="convencoes-de-url"></a>

## 🔗 3.9 Convenções de URL

As URLs das APIs seguem um padrão único em toda a plataforma.

As principais convenções adotadas são:

- Utilizar substantivos para representar recursos.
- Utilizar nomes no plural.
- Utilizar letras minúsculas.
- Separar palavras compostas com hífen (`-`).
- Não utilizar verbos nas URLs.
- Utilizar identificadores no caminho para representar recursos específicos.

Exemplos:

```text
/api/v1/customers

/api/v1/customers/{id}

/api/v1/credit-requests

/api/v1/credit-requests/{id}

/api/v1/users

/api/v1/roles
```

Exemplos que não devem ser utilizados:

```text
/createCustomer

/getCustomer

/deleteCustomer

/updateCustomer
```

As operações são determinadas exclusivamente pelo método HTTP utilizado na requisição.

---

⬆️ [Voltar ao índice](#indice)


<a id="autenticacao-e-autorizacao"></a>

# 🔐 4. Autenticação e Autorização

## Objetivo

Garantir que apenas usuários autenticados e autorizados possam acessar os recursos da plataforma, assegurando a confidencialidade, a integridade e o controle de acesso às funcionalidades do Examen Crediti.

## Estratégia de Autenticação

A plataforma utiliza autenticação baseada em **JSON Web Token (JWT)**.

Após uma autenticação bem-sucedida, o Identity Service emite:

- Um **Access Token**, utilizado para autenticar as requisições à API.
- Um **Refresh Token**, utilizado para obter um novo Access Token quando o atual expirar.

O Access Token deverá ser enviado em todas as requisições protegidas por meio do cabeçalho HTTP `Authorization`.

Exemplo:

```http
Authorization: Bearer <access_token>
```

## Fluxo de Autenticação

```text
Cliente
    │
    │ Login (usuário e senha)
    ▼
Identity Service
    │
    │ Valida as credenciais
    ▼
Banco de Dados
    │
    │ Usuário válido
    ▼
Identity Service
    │
    ├── Gera Access Token (JWT)
    ├── Gera Refresh Token
    ▼
Cliente
```

## Autorização

Após validar o Access Token, a plataforma verifica as permissões do usuário antes de permitir o acesso ao recurso solicitado.

As permissões são associadas aos papéis (roles) atribuídos ao usuário.

Fluxo simplificado:

```text
Cliente
    │
    │ Requisição autenticada
    ▼
API Gateway
    │
    │ Valida o Access Token
    ▼
Microsserviço
    │
    │ Verifica permissões
    ▼
Recurso protegido
```

## Papéis e Permissões

Cada usuário poderá possuir um ou mais papéis.

Cada papel poderá possuir uma ou mais permissões.

Essa abordagem permite controlar o acesso às funcionalidades da plataforma de forma flexível e centralizada.

Exemplo:

| Papel | Permissões |
|--------|------------|
| Administrador | Acesso completo à plataforma. |
| Analista de Crédito | Gerenciar análises de crédito. |
| Auditor | Consultar eventos de auditoria. |
| Operador | Consultar clientes e solicitar análises de crédito. |

## Endpoints Públicos

Os seguintes endpoints não exigem autenticação:

| Método | Endpoint |
|---------|----------|
| POST | `/api/v1/auth/login` |
| POST | `/api/v1/auth/refresh` |

## Endpoints Protegidos

Todos os demais endpoints exigem:

- Access Token válido.
- Usuário ativo.
- Permissões compatíveis com a operação solicitada.

## Expiração dos Tokens

O tempo de expiração do Access Token e do Refresh Token é configurável e poderá ser alterado sem impacto no contrato da API.

## Logout

O processo de logout invalida o Refresh Token e impede sua reutilização para emissão de novos Access Tokens.

Quando utilizado o mecanismo de blacklist, o Access Token também poderá ser invalidado antes de sua expiração natural.

## Considerações

As regras detalhadas sobre autenticação, autorização, JWT, criptografia, gerenciamento de credenciais, renovação de tokens, blacklist, políticas de senha e demais mecanismos de segurança são documentadas em **09-seguranca.md**.

Este documento apresenta apenas as informações necessárias para compreender o processo de autenticação e utilização da API.


# 👤 5. Identity Service

## Objetivo

O Identity Service é responsável pela autenticação, autorização e gerenciamento de identidade dos usuários da plataforma.

Entre suas responsabilidades estão:

- Autenticar usuários.
- Emitir e renovar tokens JWT.
- Gerenciar usuários.
- Gerenciar papéis (roles).
- Gerenciar permissões.
- Controlar o acesso aos recursos protegidos.

---

## Recursos

| Recurso | Descrição |
|----------|-----------|
| Autenticação | Realiza login, renovação de tokens e logout. |
| Usuários | Gerencia os usuários da plataforma. |
| Papéis | Gerencia os papéis (roles). |
| Permissões | Gerencia as permissões associadas aos papéis. |

---

## 5.1.1 Login - Está pronto

## 5.1.2 Renovar Access Token

### Objetivo

Emitir um novo Access Token utilizando um Refresh Token válido, sem a necessidade de uma nova autenticação com usuário e senha.

**Microsserviço:** Identity Service

**Recurso:** Autenticação

**Versão da API:** v1

---

### Endpoint

```http
POST /api/v1/auth/refresh
```

---

### Autenticação

Não requerida.

---

### Permissões

Não se aplica.

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Content-Type | Sim | Deve ser `application/json`. |

---

### Parâmetros de Caminho

Não possui.

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

DTO utilizado:

```text
RefreshTokenRequest
```

Campos:

| Campo | Tipo | Obrigatório | Descrição |
|--------|------|-------------|-----------|
| refreshToken | String | Sim | Refresh Token emitido durante a autenticação. |

---

### Exemplo de Requisição

DTO: `RefreshTokenRequest`

```json
{
  "refreshToken": "<refresh_token>"
}
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ POST /auth/refresh
    ▼
Identity Service
    │
    │ Valida o Refresh Token
    ▼
Banco de Dados / Redis
    │
    │ Token válido
    ▼
Identity Service
    │
    ├── Gera novo Access Token
    ├── Gera novo Refresh Token (opcional)
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
RefreshTokenResponse
```

---

### Exemplo de Resposta

DTO: `RefreshTokenResponse`

```json
{
  "accessToken": "<novo_access_token>",
  "refreshToken": "<novo_refresh_token>",
  "tokenType": "Bearer",
  "expiresIn": 900
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Novo Access Token emitido com sucesso. |
| 400 Bad Request | Requisição inválida. |
| 401 Unauthorized | Refresh Token inválido ou expirado. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- O Refresh Token deverá ser válido.
- O Refresh Token não poderá estar expirado.
- O Refresh Token não poderá ter sido revogado.
- Um novo Access Token será emitido para o usuário autenticado.
- A renovação do Refresh Token dependerá da política de segurança adotada pela plataforma.

---

### Observações

Os detalhes sobre expiração, revogação, rotação de tokens e blacklist são apresentados em **09-seguranca.md**.

---

⬆️ [Voltar ao índice](#indice)

## 5.1.3 Logout

### Objetivo

Encerrar a sessão do usuário autenticado, invalidando o Refresh Token e impedindo sua reutilização para emissão de novos Access Tokens.

**Microsserviço:** Identity Service

**Recurso:** Autenticação

**Versão da API:** v1

---

### Endpoint

```http
POST /api/v1/auth/logout
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

Usuário autenticado.

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |
| Content-Type | Sim | Deve ser `application/json`. |

---

### Parâmetros de Caminho

Não possui.

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

DTO utilizado:

```text
RefreshTokenRequest
```

Campos:

| Campo | Tipo | Obrigatório | Descrição |
|--------|------|-------------|-----------|
| refreshToken | String | Sim | Refresh Token que deverá ser invalidado. |

---

### Exemplo de Requisição

DTO: `RefreshTokenRequest`

```json
{
  "refreshToken": "<refresh_token>"
}
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ POST /auth/logout
    ▼
Identity Service
    │
    │ Valida o Access Token
    │
    │ Localiza o Refresh Token
    │
    │ Invalida o Refresh Token
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
MessageResponse
```

---

### Exemplo de Resposta

DTO: `MessageResponse`

```json
{
  "message": "Logout realizado com sucesso."
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Logout realizado com sucesso. |
| 400 Bad Request | Requisição inválida. |
| 401 Unauthorized | Access Token inválido ou expirado. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- O Access Token deverá ser válido.
- O Refresh Token deverá pertencer ao usuário autenticado.
- O Refresh Token será invalidado e não poderá ser reutilizado.
- O usuário deverá realizar uma nova autenticação para obter novos tokens após o logout.

---

### Observações

Os detalhes sobre blacklist de tokens, revogação, expiração e políticas de logout são apresentados em **09-seguranca.md**.

---

⬆️ [Voltar ao índice](#indice)

## 5.2 Usuários

Os endpoints desta seção são responsáveis pelo gerenciamento dos usuários da plataforma.

---

## 5.2.1 Criar Usuário

### Objetivo

Cadastrar um novo usuário na plataforma.

**Microsserviço:** Identity Service

**Recurso:** Usuários

**Versão da API:** v1

---

### Endpoint

```http
POST /api/v1/users
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`USER_CREATE`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |
| Content-Type | Sim | Deve ser `application/json`. |

---

### Parâmetros de Caminho

Não possui.

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

DTO utilizado:

```text
CreateUserRequest
```

Campos:

| Campo | Tipo | Obrigatório | Descrição |
|--------|------|-------------|-----------|
| name | String | Sim | Nome completo do usuário. |
| email | String | Sim | Endereço de e-mail. |
| password | String | Sim | Senha de acesso. |
| roleIds | List<UUID> | Sim | Identificadores dos papéis atribuídos ao usuário. |
| active | Boolean | Sim | Situação inicial do usuário. |

---

### Exemplo de Requisição

DTO: `CreateUserRequest`

```json
{
  "name": "João Silva",
  "email": "joao.silva@email.com",
  "password": "Senha@123",
  "roleIds": [
    "3e0d4bb2-6bc9-4f6b-9cf7-8a6fd90d5e11"
  ],
  "active": true
}
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ POST /users
    ▼
Identity Service
    │
    │ Valida a requisição
    │
    │ Verifica permissões
    │
    │ Verifica duplicidade de e-mail
    │
    │ Criptografa a senha
    ▼
Banco de Dados
    │
    │ Persiste o usuário
    ▼
Identity Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
UserResponse
```

---

### Exemplo de Resposta

DTO: `UserResponse`

```json
{
  "id": "efbdf77f-24dd-45b3-a130-3e60ef7b1f6a",
  "name": "João Silva",
  "email": "joao.silva@email.com",
  "roles": [
    {
      "id": "3e0d4bb2-6bc9-4f6b-9cf7-8a6fd90d5e11",
      "name": "ADMIN"
    }
  ],
  "active": true,
  "createdAt": "2026-08-15T14:30:00Z",
  "updatedAt": "2026-08-15T14:30:00Z"
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 201 Created | Usuário criado com sucesso. |
| 400 Bad Request | Requisição inválida. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para criar usuários. |
| 409 Conflict | Já existe um usuário com o e-mail informado. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- O e-mail deverá ser único na plataforma.
- A senha deverá atender à política de segurança definida pela aplicação.
- Todos os papéis informados deverão existir.
- O usuário será criado com os papéis informados na requisição.
- A senha será armazenada apenas em formato criptografado.

---

### Observações

As regras relacionadas à política de senhas, algoritmo de criptografia, autenticação e controle de acesso são detalhadas em **09-seguranca.md**.

⬆️ [Voltar ao índice](#indice)

## 5.2.2 Listar Usuários

### Objetivo

Retornar uma lista paginada de usuários cadastrados na plataforma.

**Microsserviço:** Identity Service

**Recurso:** Usuários

**Versão da API:** v1

---

### Endpoint

```http
GET /api/v1/users
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`USER_READ`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |

---

### Parâmetros de Caminho

Não possui.

---

### Parâmetros de Consulta

| Parâmetro | Tipo | Obrigatório | Descrição |
|------------|------|-------------|-----------|
| page | Integer | Não | Número da página. Valor padrão: `0`. |
| size | Integer | Não | Quantidade de registros por página. Valor padrão: `20`. |
| sort | String | Não | Campo utilizado para ordenação. |
| direction | String | Não | Direção da ordenação (`ASC` ou `DESC`). |

---

### Corpo da Requisição

Não possui.

---

### Exemplo de Requisição

```http
GET /api/v1/users?page=0&size=20&sort=name&direction=ASC
Authorization: Bearer <access_token>
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ GET /users
    ▼
Identity Service
    │
    │ Valida o Access Token
    │
    │ Verifica permissões
    ▼
Banco de Dados
    │
    │ Consulta usuários
    ▼
Identity Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
PageResponse<UserResponse>
```

---

### Exemplo de Resposta

DTO: `PageResponse<UserResponse>`

```json
{
  "content": [
    {
      "id": "efbdf77f-24dd-45b3-a130-3e60ef7b1f6a",
      "name": "João Silva",
      "email": "joao.silva@email.com",
      "active": true
    },
    {
      "id": "0fd9ddc5-12f0-42cb-bfc7-3dc6dbf2c26f",
      "name": "Maria Oliveira",
      "email": "maria.oliveira@email.com",
      "active": true
    }
  ],
  "page": 0,
  "size": 20,
  "totalElements": 2,
  "totalPages": 1
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Lista de usuários retornada com sucesso. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para consultar usuários. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- Apenas usuários autenticados poderão consultar usuários.
- A consulta deverá respeitar as permissões do usuário autenticado.
- O resultado deverá ser paginado.
- Os critérios de ordenação deverão utilizar apenas campos permitidos pela API.

---

### Observações

Os parâmetros de paginação e ordenação seguem as convenções definidas em **01-convencoes-do-projeto.md**.

⬆️ [Voltar ao índice](#indice)

## 5.2.3 Buscar Usuário por ID

### Objetivo

Retornar os dados de um usuário específico a partir do seu identificador.

**Microsserviço:** Identity Service

**Recurso:** Usuários

**Versão da API:** v1

---

### Endpoint

```http
GET /api/v1/users/{id}
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`USER_READ`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |

---

### Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
|------------|------|-------------|-----------|
| id | UUID | Sim | Identificador único do usuário. |

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

Não possui.

---

### Exemplo de Requisição

```http
GET /api/v1/users/efbdf77f-24dd-45b3-a130-3e60ef7b1f6a
Authorization: Bearer <access_token>
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ GET /users/{id}
    ▼
Identity Service
    │
    │ Valida o Access Token
    │
    │ Verifica permissões
    ▼
Banco de Dados
    │
    │ Consulta o usuário
    ▼
Identity Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
UserResponse
```

---

### Exemplo de Resposta

DTO: `UserResponse`

```json
{
  "id": "efbdf77f-24dd-45b3-a130-3e60ef7b1f6a",
  "name": "João Silva",
  "email": "joao.silva@email.com",
  "roles": [
    {
      "id": "3e0d4bb2-6bc9-4f6b-9cf7-8a6fd90d5e11",
      "name": "ADMIN"
    }
  ],
  "active": true,
  "createdAt": "2026-08-15T14:30:00Z",
  "updatedAt": "2026-08-15T14:30:00Z"
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Usuário encontrado com sucesso. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para consultar usuários. |
| 404 Not Found | Usuário não encontrado. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- Apenas usuários autenticados poderão consultar usuários.
- A consulta deverá respeitar as permissões do usuário autenticado.
- O identificador informado deverá corresponder a um usuário existente.

---

### Observações

O identificador do usuário é um UUID gerado automaticamente pela plataforma e não poderá ser alterado após sua criação.

⬆️ [Voltar ao índice](#indice)

## 5.2.4 Atualizar Usuário

### Objetivo

Atualizar os dados cadastrais de um usuário existente.

**Microsserviço:** Identity Service

**Recurso:** Usuários

**Versão da API:** v1

---

### Endpoint

```http
PUT /api/v1/users/{id}
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`USER_UPDATE`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |
| Content-Type | Sim | Deve ser `application/json`. |

---

### Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
|------------|------|-------------|-----------|
| id | UUID | Sim | Identificador único do usuário. |

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

DTO utilizado:

```text
UpdateUserRequest
```

Campos:

| Campo | Tipo | Obrigatório | Descrição |
|--------|------|-------------|-----------|
| name | String | Sim | Nome completo do usuário. |
| email | String | Sim | Endereço de e-mail. |
| roleIds | List<UUID> | Sim | Papéis atribuídos ao usuário. |
| active | Boolean | Sim | Situação do usuário. |

---

### Exemplo de Requisição

DTO: `UpdateUserRequest`

```json
{
  "name": "João Silva Santos",
  "email": "joao.santos@email.com",
  "roleIds": [
    "3e0d4bb2-6bc9-4f6b-9cf7-8a6fd90d5e11"
  ],
  "active": true
}
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ PUT /users/{id}
    ▼
Identity Service
    │
    │ Valida a requisição
    │
    │ Verifica permissões
    │
    │ Localiza o usuário
    │
    │ Atualiza os dados
    ▼
Banco de Dados
    ▼
Identity Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
UserResponse
```

---

### Exemplo de Resposta

DTO: `UserResponse`

```json
{
  "id": "efbdf77f-24dd-45b3-a130-3e60ef7b1f6a",
  "name": "João Silva Santos",
  "email": "joao.santos@email.com",
  "roles": [
    {
      "id": "3e0d4bb2-6bc9-4f6b-9cf7-8a6fd90d5e11",
      "name": "ADMIN"
    }
  ],
  "active": true,
  "createdAt": "2026-08-15T14:30:00Z",
  "updatedAt": "2026-08-18T09:15:00Z"
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Usuário atualizado com sucesso. |
| 400 Bad Request | Requisição inválida. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para atualizar usuários. |
| 404 Not Found | Usuário não encontrado. |
| 409 Conflict | Já existe um usuário com o e-mail informado. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- O usuário deverá existir.
- O e-mail deverá permanecer único na plataforma.
- Todos os papéis informados deverão existir.
- A atualização deverá preservar o identificador do usuário.

---

### Observações

A alteração da senha é realizada por um processo específico e não faz parte deste endpoint.

⬆️ [Voltar ao índice](#indice)

## 5.2.5 Excluir Usuário

### Objetivo

Excluir um usuário da plataforma.

**Microsserviço:** Identity Service

**Recurso:** Usuários

**Versão da API:** v1

---

### Endpoint

```http
DELETE /api/v1/users/{id}
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`USER_DELETE`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |

---

### Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| id | UUID | Sim | Identificador único do usuário. |

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

Não possui.

---

### Exemplo de Requisição

```http
DELETE /api/v1/users/efbdf77f-24dd-45b3-a130-3e60ef7b1f6a
Authorization: Bearer <access_token>
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ DELETE /users/{id}
    ▼
Identity Service
    │
    │ Valida o Access Token
    │
    │ Verifica permissões
    │
    │ Localiza o usuário
    │
    │ Remove o usuário
    ▼
Banco de Dados
    ▼
Identity Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
MessageResponse
```

---

### Exemplo de Resposta

DTO: `MessageResponse`

```json
{
  "message": "Usuário removido com sucesso."
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Usuário removido com sucesso. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para excluir usuários. |
| 404 Not Found | Usuário não encontrado. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- O usuário deverá existir.
- Apenas usuários autorizados poderão excluir usuários.
- A exclusão removerá permanentemente o usuário da plataforma.

---

### Observações

A exclusão de um usuário poderá ser impedida caso existam regras de negócio que preservem o histórico de auditoria ou relacionamentos obrigatórios.

⬆️ [Voltar ao índice](#indice)


# 5.3 Papéis

Os endpoints desta seção são responsáveis pelo gerenciamento dos papéis (roles) da plataforma.

Cada papel representa um conjunto de permissões que poderá ser atribuído a um ou mais usuários.

Os papéis são utilizados pelo mecanismo de autorização para controlar o acesso aos recursos da API.

## 5.3.1 Criar Papel

### Objetivo

Cadastrar um novo papel (role) na plataforma.

**Microsserviço:** Identity Service

**Recurso:** Papéis

**Versão da API:** v1

---

### Endpoint

```http
POST /api/v1/roles
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`ROLE_CREATE`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |
| Content-Type | Sim | Deve ser `application/json`. |

---

### Parâmetros de Caminho

Não possui.

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

DTO utilizado:

```text
CreateRoleRequest
```

Campos:

| Campo | Tipo | Obrigatório | Descrição |
|--------|------|-------------|-----------|
| name | String | Sim | Nome do papel. |
| description | String | Sim | Descrição do papel. |

---

### Exemplo de Requisição

DTO: `CreateRoleRequest`

```json
{
  "name": "ADMIN",
  "description": "Administrador do sistema."
}
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ POST /roles
    ▼
Identity Service
    │
    │ Valida a requisição
    │
    │ Verifica permissões
    │
    │ Verifica duplicidade
    ▼
Banco de Dados
    │
    │ Persiste o papel
    ▼
Identity Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
RoleResponse
```

---

### Exemplo de Resposta

DTO: `RoleResponse`

```json
{
  "id": "a9a0cb8f-c8e3-4e88-8f54-c96385c689ef",
  "name": "ADMIN",
  "description": "Administrador do sistema.",
  "createdAt": "2026-08-15T14:30:00Z",
  "updatedAt": "2026-08-15T14:30:00Z"
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 201 Created | Papel criado com sucesso. |
| 400 Bad Request | Requisição inválida. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para criar papéis. |
| 409 Conflict | Já existe um papel com o nome informado. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- O nome do papel deverá ser único.
- O papel será criado sem permissões associadas.
- As permissões serão vinculadas posteriormente.

---

### Observações

A associação entre papéis e permissões é realizada por endpoints específicos de gerenciamento de permissões.

⬆️ [Voltar ao índice](#indice)

## 5.3.2 Listar Papéis

### Objetivo

Retornar uma lista paginada dos papéis cadastrados na plataforma.

**Microsserviço:** Identity Service

**Recurso:** Papéis

**Versão da API:** v1

---

### Endpoint

```http
GET /api/v1/roles
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`ROLE_READ`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |

---

### Parâmetros de Caminho

Não possui.

---

### Parâmetros de Consulta

| Parâmetro | Tipo | Obrigatório | Descrição |
|------------|------|-------------|-----------|
| page | Integer | Não | Número da página. Valor padrão: `0`. |
| size | Integer | Não | Quantidade de registros por página. Valor padrão: `20`. |
| sort | String | Não | Campo utilizado para ordenação. |
| direction | String | Não | Direção da ordenação (`ASC` ou `DESC`). |

---

### Corpo da Requisição

Não possui.

---

### Exemplo de Requisição

```http
GET /api/v1/roles?page=0&size=20&sort=name&direction=ASC
Authorization: Bearer <access_token>
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ GET /roles
    ▼
Identity Service
    │
    │ Valida o Access Token
    │
    │ Verifica permissões
    ▼
Banco de Dados
    │
    │ Consulta papéis
    ▼
Identity Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
PageResponse<RoleResponse>
```

---

### Exemplo de Resposta

DTO: `PageResponse<RoleResponse>`

```json
{
  "content": [
    {
      "id": "a9a0cb8f-c8e3-4e88-8f54-c96385c689ef",
      "name": "ADMIN",
      "description": "Administrador do sistema."
    },
    {
      "id": "b3dcfbc7-9b54-463f-b47f-5407a74c11d2",
      "name": "ANALYST",
      "description": "Analista de crédito."
    }
  ],
  "page": 0,
  "size": 20,
  "totalElements": 2,
  "totalPages": 1
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Lista de papéis retornada com sucesso. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para consultar papéis. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- Apenas usuários autenticados poderão consultar papéis.
- A consulta deverá respeitar as permissões do usuário autenticado.
- O resultado deverá ser paginado.
- Os critérios de ordenação deverão utilizar apenas campos permitidos pela API.

---

### Observações

Os parâmetros de paginação e ordenação seguem as convenções definidas em **01-convencoes-do-projeto.md**.

⬆️ [Voltar ao índice](#indice)

## 5.3.3 Buscar Papel por ID

### Objetivo

Retornar os dados de um papel específico a partir do seu identificador.

**Microsserviço:** Identity Service

**Recurso:** Papéis

**Versão da API:** v1

---

### Endpoint

```http
GET /api/v1/roles/{id}
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`ROLE_READ`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |

---

### Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| id | UUID | Sim | Identificador único do papel. |

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

Não possui.

---

### Exemplo de Requisição

```http
GET /api/v1/roles/a9a0cb8f-c8e3-4e88-8f54-c96385c689ef
Authorization: Bearer <access_token>
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ GET /roles/{id}
    ▼
Identity Service
    │
    │ Valida o Access Token
    │
    │ Verifica permissões
    ▼
Banco de Dados
    │
    │ Consulta o papel
    ▼
Identity Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
RoleResponse
```

---

### Exemplo de Resposta

DTO: `RoleResponse`

```json
{
  "id": "a9a0cb8f-c8e3-4e88-8f54-c96385c689ef",
  "name": "ADMIN",
  "description": "Administrador do sistema.",
  "createdAt": "2026-08-15T14:30:00Z",
  "updatedAt": "2026-08-15T14:30:00Z"
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Papel encontrado com sucesso. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para consultar papéis. |
| 404 Not Found | Papel não encontrado. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- O papel deverá existir.
- Apenas usuários autorizados poderão consultar papéis.

---

### Observações

O identificador do papel é um UUID gerado automaticamente pela plataforma.

⬆️ [Voltar ao índice](#indice)

## 5.3.4 Atualizar Papel

### Objetivo

Atualizar os dados de um papel existente.

**Microsserviço:** Identity Service

**Recurso:** Papéis

**Versão da API:** v1

---

### Endpoint

```http
PUT /api/v1/roles/{id}
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`ROLE_UPDATE`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |
| Content-Type | Sim | Deve ser `application/json`. |

---

### Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| id | UUID | Sim | Identificador único do papel. |

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

DTO utilizado:

```text
UpdateRoleRequest
```

Campos:

| Campo | Tipo | Obrigatório | Descrição |
|--------|------|-------------|-----------|
| name | String | Sim | Nome do papel. |
| description | String | Sim | Descrição do papel. |

---

### Exemplo de Requisição

DTO: `UpdateRoleRequest`

```json
{
  "name": "ADMIN",
  "description": "Administrador geral da plataforma."
}
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ PUT /roles/{id}
    ▼
Identity Service
    │
    │ Valida a requisição
    │
    │ Verifica permissões
    │
    │ Localiza o papel
    │
    │ Atualiza os dados
    ▼
Banco de Dados
    ▼
Identity Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
RoleResponse
```

---

### Exemplo de Resposta

DTO: `RoleResponse`

```json
{
  "id": "a9a0cb8f-c8e3-4e88-8f54-c96385c689ef",
  "name": "ADMIN",
  "description": "Administrador geral da plataforma.",
  "createdAt": "2026-08-15T14:30:00Z",
  "updatedAt": "2026-08-18T09:15:00Z"
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Papel atualizado com sucesso. |
| 400 Bad Request | Requisição inválida. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para atualizar papéis. |
| 404 Not Found | Papel não encontrado. |
| 409 Conflict | Já existe um papel com o nome informado. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- O papel deverá existir.
- O nome do papel deverá permanecer único.
- A atualização não altera as permissões associadas ao papel.

---

### Observações

O gerenciamento das permissões associadas ao papel é realizado por endpoints específicos.

⬆️ [Voltar ao índice](#indice)

## 5.3.5 Excluir Papel

### Objetivo

Excluir um papel da plataforma.

**Microsserviço:** Identity Service

**Recurso:** Papéis

**Versão da API:** v1

---

### Endpoint

```http
DELETE /api/v1/roles/{id}
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`ROLE_DELETE`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |

---

### Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| id | UUID | Sim | Identificador único do papel. |

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

Não possui.

---

### Exemplo de Requisição

```http
DELETE /api/v1/roles/a9a0cb8f-c8e3-4e88-8f54-c96385c689ef
Authorization: Bearer <access_token>
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ DELETE /roles/{id}
    ▼
Identity Service
    │
    │ Valida o Access Token
    │
    │ Verifica permissões
    │
    │ Localiza o papel
    │
    │ Remove o papel
    ▼
Banco de Dados
    ▼
Identity Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
MessageResponse
```

---

### Exemplo de Resposta

DTO: `MessageResponse`

```json
{
  "message": "Papel removido com sucesso."
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Papel removido com sucesso. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para excluir papéis. |
| 404 Not Found | Papel não encontrado. |
| 409 Conflict | O papel não pode ser removido porque está associado a um ou mais usuários. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- O papel deverá existir.
- O papel não poderá ser removido enquanto estiver associado a usuários.
- Apenas usuários autorizados poderão excluir papéis.

---

### Observações

Antes da exclusão, todas as associações entre usuários e o papel deverão ser removidas.

⬆️ [Voltar ao índice](#indice)


# 5.4 Permissões

Os endpoints desta seção são responsáveis pelo gerenciamento das permissões da plataforma.

Cada permissão representa uma ação que poderá ser executada dentro do sistema.

As permissões são associadas aos papéis (roles), que por sua vez são atribuídos aos usuários.

## 5.4.1 Criar Permissão

### Objetivo

Cadastrar uma nova permissão na plataforma.

**Microsserviço:** Identity Service

**Recurso:** Permissões

**Versão da API:** v1

---

### Endpoint

```http
POST /api/v1/permissions
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`PERMISSION_CREATE`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |
| Content-Type | Sim | Deve ser `application/json`. |

---

### Parâmetros de Caminho

Não possui.

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

DTO utilizado:

```text
CreatePermissionRequest
```

Campos:

| Campo | Tipo | Obrigatório | Descrição |
|--------|------|-------------|-----------|
| name | String | Sim | Nome da permissão. |
| description | String | Sim | Descrição da permissão. |

---

### Exemplo de Requisição

DTO: `CreatePermissionRequest`

```json
{
  "name": "CUSTOMER_CREATE",
  "description": "Permite cadastrar clientes."
}
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ POST /permissions
    ▼
Identity Service
    │
    │ Valida a requisição
    │
    │ Verifica permissões
    │
    │ Verifica duplicidade
    ▼
Banco de Dados
    │
    │ Persiste a permissão
    ▼
Identity Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
PermissionResponse
```

---

### Exemplo de Resposta

DTO: `PermissionResponse`

```json
{
  "id": "ddf18cb9-cfb2-49ff-b17d-5fd44e2ef8d1",
  "name": "CUSTOMER_CREATE",
  "description": "Permite cadastrar clientes.",
  "createdAt": "2026-08-15T14:30:00Z",
  "updatedAt": "2026-08-15T14:30:00Z"
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 201 Created | Permissão criada com sucesso. |
| 400 Bad Request | Requisição inválida. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para criar permissões. |
| 409 Conflict | Já existe uma permissão com o nome informado. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- O nome da permissão deverá ser único.
- A permissão será criada sem associação a qualquer papel.

---

### Observações

A associação entre permissões e papéis é realizada por endpoints específicos.

⬆️ [Voltar ao índice](#indice)

## 5.4.2 Listar Permissões

### Objetivo

Retornar uma lista paginada das permissões cadastradas na plataforma.

**Microsserviço:** Identity Service

**Recurso:** Permissões

**Versão da API:** v1

---

### Endpoint

```http
GET /api/v1/permissions
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`PERMISSION_READ`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |

---

### Parâmetros de Caminho

Não possui.

---

### Parâmetros de Consulta

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| page | Integer | Não | Número da página. Valor padrão: `0`. |
| size | Integer | Não | Quantidade de registros por página. Valor padrão: `20`. |
| sort | String | Não | Campo utilizado para ordenação. |
| direction | String | Não | Direção da ordenação (`ASC` ou `DESC`). |

---

### Corpo da Requisição

Não possui.

---

### Exemplo de Requisição

```http
GET /api/v1/permissions?page=0&size=20&sort=name&direction=ASC
Authorization: Bearer <access_token>
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ GET /permissions
    ▼
Identity Service
    │
    │ Valida o Access Token
    │
    │ Verifica permissões
    ▼
Banco de Dados
    │
    │ Consulta permissões
    ▼
Identity Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
PageResponse<PermissionResponse>
```

---

### Exemplo de Resposta

DTO: `PageResponse<PermissionResponse>`

```json
{
  "content": [
    {
      "id": "ddf18cb9-cfb2-49ff-b17d-5fd44e2ef8d1",
      "name": "CUSTOMER_CREATE",
      "description": "Permite cadastrar clientes."
    },
    {
      "id": "53b4db7b-2bc9-42d0-9d72-f54a7e7b6d35",
      "name": "CUSTOMER_READ",
      "description": "Permite consultar clientes."
    }
  ],
  "page": 0,
  "size": 20,
  "totalElements": 2,
  "totalPages": 1
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Lista de permissões retornada com sucesso. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para consultar permissões. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- Apenas usuários autenticados poderão consultar permissões.
- O resultado deverá ser paginado.
- Os critérios de ordenação deverão utilizar apenas campos permitidos pela API.

---

### Observações

Os parâmetros de paginação e ordenação seguem as convenções definidas em **01-convencoes-do-projeto.md**.

⬆️ [Voltar ao índice](#indice)

## 5.4.3 Buscar Permissão por ID

### Objetivo

Retornar os dados de uma permissão específica a partir do seu identificador.

**Microsserviço:** Identity Service

**Recurso:** Permissões

**Versão da API:** v1

---

### Endpoint

```http
GET /api/v1/permissions/{id}
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`PERMISSION_READ`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |

---

### Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| id | UUID | Sim | Identificador único da permissão. |

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

Não possui.

---

### Exemplo de Requisição

```http
GET /api/v1/permissions/ddf18cb9-cfb2-49ff-b17d-5fd44e2ef8d1
Authorization: Bearer <access_token>
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ GET /permissions/{id}
    ▼
Identity Service
    │
    │ Valida o Access Token
    │
    │ Verifica permissões
    ▼
Banco de Dados
    │
    │ Consulta a permissão
    ▼
Identity Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
PermissionResponse
```

---

### Exemplo de Resposta

DTO: `PermissionResponse`

```json
{
  "id": "ddf18cb9-cfb2-49ff-b17d-5fd44e2ef8d1",
  "name": "CUSTOMER_CREATE",
  "description": "Permite cadastrar clientes.",
  "createdAt": "2026-08-15T14:30:00Z",
  "updatedAt": "2026-08-15T14:30:00Z"
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Permissão encontrada com sucesso. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para consultar permissões. |
| 404 Not Found | Permissão não encontrada. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- A permissão deverá existir.
- Apenas usuários autorizados poderão consultar permissões.

---

### Observações

O identificador da permissão é um UUID gerado automaticamente pela plataforma.

⬆️ [Voltar ao índice](#indice)

## 5.4.4 Atualizar Permissão

### Objetivo

Atualizar os dados de uma permissão existente.

**Microsserviço:** Identity Service

**Recurso:** Permissões

**Versão da API:** v1

---

### Endpoint

```http
PUT /api/v1/permissions/{id}
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`PERMISSION_UPDATE`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |
| Content-Type | Sim | Deve ser `application/json`. |

---

### Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| id | UUID | Sim | Identificador único da permissão. |

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

DTO utilizado:

```text
UpdatePermissionRequest
```

Campos:

| Campo | Tipo | Obrigatório | Descrição |
|--------|------|-------------|-----------|
| name | String | Sim | Nome da permissão. |
| description | String | Sim | Descrição da permissão. |

---

### Exemplo de Requisição

DTO: `UpdatePermissionRequest`

```json
{
  "name": "CUSTOMER_CREATE",
  "description": "Permite cadastrar e registrar novos clientes."
}
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ PUT /permissions/{id}
    ▼
Identity Service
    │
    │ Valida a requisição
    │
    │ Verifica permissões
    │
    │ Localiza a permissão
    │
    │ Atualiza os dados
    ▼
Banco de Dados
    ▼
Identity Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
PermissionResponse
```

---

### Exemplo de Resposta

DTO: `PermissionResponse`

```json
{
  "id": "ddf18cb9-cfb2-49ff-b17d-5fd44e2ef8d1",
  "name": "CUSTOMER_CREATE",
  "description": "Permite cadastrar e registrar novos clientes.",
  "createdAt": "2026-08-15T14:30:00Z",
  "updatedAt": "2026-08-18T09:15:00Z"
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Permissão atualizada com sucesso. |
| 400 Bad Request | Requisição inválida. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para atualizar permissões. |
| 404 Not Found | Permissão não encontrada. |
| 409 Conflict | Já existe uma permissão com o nome informado. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- A permissão deverá existir.
- O nome da permissão deverá permanecer único.
- A atualização não altera os papéis associados à permissão.

---

### Observações

O gerenciamento das associações entre papéis e permissões é realizado por endpoints específicos.

⬆️ [Voltar ao índice](#indice)

## 5.4.5 Excluir Permissão

### Objetivo

Excluir uma permissão da plataforma.

**Microsserviço:** Identity Service

**Recurso:** Permissões

**Versão da API:** v1

---

### Endpoint

```http
DELETE /api/v1/permissions/{id}
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`PERMISSION_DELETE`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |

---

### Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| id | UUID | Sim | Identificador único da permissão. |

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

Não possui.

---

### Exemplo de Requisição

```http
DELETE /api/v1/permissions/ddf18cb9-cfb2-49ff-b17d-5fd44e2ef8d1
Authorization: Bearer <access_token>
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ DELETE /permissions/{id}
    ▼
Identity Service
    │
    │ Valida o Access Token
    │
    │ Verifica permissões
    │
    │ Localiza a permissão
    │
    │ Remove a permissão
    ▼
Banco de Dados
    ▼
Identity Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
MessageResponse
```

---

### Exemplo de Resposta

DTO: `MessageResponse`

```json
{
  "message": "Permissão removida com sucesso."
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Permissão removida com sucesso. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para excluir permissões. |
| 404 Not Found | Permissão não encontrada. |
| 409 Conflict | A permissão não pode ser removida porque está associada a um ou mais papéis. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- A permissão deverá existir.
- A permissão não poderá ser removida enquanto estiver associada a um ou mais papéis.
- Apenas usuários autorizados poderão excluir permissões.

---

### Observações

Antes da exclusão, todas as associações entre papéis e a permissão deverão ser removidas.

⬆️ [Voltar ao índice](#indice)

## 5.4.5 Excluir Permissão

### Objetivo

Excluir uma permissão da plataforma.

**Microsserviço:** Identity Service

**Recurso:** Permissões

**Versão da API:** v1

---

### Endpoint

```http
DELETE /api/v1/permissions/{id}
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`PERMISSION_DELETE`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |

---

### Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| id | UUID | Sim | Identificador único da permissão. |

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

Não possui.

---

### Exemplo de Requisição

```http
DELETE /api/v1/permissions/ddf18cb9-cfb2-49ff-b17d-5fd44e2ef8d1
Authorization: Bearer <access_token>
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ DELETE /permissions/{id}
    ▼
Identity Service
    │
    │ Valida o Access Token
    │
    │ Verifica permissões
    │
    │ Localiza a permissão
    │
    │ Remove a permissão
    ▼
Banco de Dados
    ▼
Identity Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
MessageResponse
```

---

### Exemplo de Resposta

DTO: `MessageResponse`

```json
{
  "message": "Permissão removida com sucesso."
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Permissão removida com sucesso. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para excluir permissões. |
| 404 Not Found | Permissão não encontrada. |
| 409 Conflict | A permissão não pode ser removida porque está associada a um ou mais papéis. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- A permissão deverá existir.
- A permissão não poderá ser removida enquanto estiver associada a um ou mais papéis.
- Apenas usuários autorizados poderão excluir permissões.

---

### Observações

Antes da exclusão, todas as associações entre papéis e a permissão deverão ser removidas.

⬆️ [Voltar ao índice](#indice)


# 5.5 Operações Especiais

Os endpoints desta seção são responsáveis por operações específicas de gerenciamento de identidade que não fazem parte do CRUD de Usuários, Papéis ou Permissões.

Essas operações complementam o gerenciamento de acesso da plataforma, permitindo administrar senhas, status de usuários e relacionamentos entre usuários, papéis e permissões.

## 5.5.1 Alterar Senha

### Objetivo

Alterar a senha de um usuário existente.

**Microsserviço:** Identity Service

**Recurso:** Usuários

**Versão da API:** v1

---

### Endpoint

```http
PUT /api/v1/users/{id}/password
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`USER_UPDATE`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |
| Content-Type | Sim | Deve ser `application/json`. |

---

### Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| id | UUID | Sim | Identificador do usuário. |

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

DTO utilizado:

```text
ChangePasswordRequest
```

Campos:

| Campo | Tipo | Obrigatório | Descrição |
|--------|------|-------------|-----------|
| currentPassword | String | Sim | Senha atual do usuário. |
| newPassword | String | Sim | Nova senha. |
| confirmPassword | String | Sim | Confirmação da nova senha. |

---

### Exemplo de Requisição

DTO: `ChangePasswordRequest`

```json
{
  "currentPassword": "Senha@123",
  "newPassword": "NovaSenha@123",
  "confirmPassword": "NovaSenha@123"
}
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ PUT /users/{id}/password
    ▼
Identity Service
    │
    │ Valida o Access Token
    │
    │ Verifica permissões
    │
    │ Valida a senha atual
    │
    │ Valida a nova senha
    │
    │ Criptografa a nova senha
    ▼
Banco de Dados
    ▼
Identity Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
MessageResponse
```

---

### Exemplo de Resposta

DTO: `MessageResponse`

```json
{
  "message": "Senha alterada com sucesso."
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Senha alterada com sucesso. |
| 400 Bad Request | Requisição inválida. |
| 401 Unauthorized | Usuário não autenticado ou senha atual inválida. |
| 403 Forbidden | Usuário sem permissão para alterar a senha. |
| 404 Not Found | Usuário não encontrado. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- O usuário deverá existir.
- A senha atual deverá estar correta.
- A nova senha deverá atender à política de segurança da plataforma.
- A confirmação da senha deverá ser idêntica à nova senha.
- A nova senha será armazenada apenas em formato criptografado.

---

### Observações

As políticas de senha e os algoritmos de criptografia são documentados em **09-seguranca.md**.

⬆️ [Voltar ao índice](#indice)

## 5.5.2 Alterar Status do Usuário

### Objetivo

Alterar a situação de um usuário, permitindo ativá-lo ou inativá-lo.

**Microsserviço:** Identity Service

**Recurso:** Usuários

**Versão da API:** v1

---

### Endpoint

```http
PATCH /api/v1/users/{id}/status
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`USER_UPDATE`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |
| Content-Type | Sim | Deve ser `application/json`. |

---

### Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| id | UUID | Sim | Identificador do usuário. |

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

DTO utilizado:

```text
UpdateUserStatusRequest
```

Campos:

| Campo | Tipo | Obrigatório | Descrição |
|--------|------|-------------|-----------|
| active | Boolean | Sim | Novo status do usuário. |

---

### Exemplo de Requisição

DTO: `UpdateUserStatusRequest`

```json
{
  "active": false
}
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ PATCH /users/{id}/status
    ▼
Identity Service
    │
    │ Valida o Access Token
    │
    │ Verifica permissões
    │
    │ Localiza o usuário
    │
    │ Atualiza o status
    ▼
Banco de Dados
    ▼
Identity Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
UserResponse
```

---

### Exemplo de Resposta

DTO: `UserResponse`

```json
{
  "id": "efbdf77f-24dd-45b3-a130-3e60ef7b1f6a",
  "name": "João Silva",
  "email": "joao.silva@email.com",
  "active": false,
  "updatedAt": "2026-08-20T10:30:00Z"
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Status atualizado com sucesso. |
| 400 Bad Request | Requisição inválida. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para alterar o status. |
| 404 Not Found | Usuário não encontrado. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- O usuário deverá existir.
- Apenas usuários autorizados poderão alterar o status.
- Usuários inativos não poderão realizar autenticação na plataforma.

---

### Observações

A inativação do usuário não remove seus papéis nem suas permissões.

⬆️ [Voltar ao índice](#indice)

## 5.5.3 Associar Papéis ao Usuário

### Objetivo

Associar um ou mais papéis a um usuário existente.

**Microsserviço:** Identity Service

**Recurso:** Usuários

**Versão da API:** v1

---

### Endpoint

```http
POST /api/v1/users/{id}/roles
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`USER_UPDATE`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |
| Content-Type | Sim | Deve ser `application/json`. |

---

### Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| id | UUID | Sim | Identificador do usuário. |

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

DTO utilizado:

```text
AssignRolesRequest
```

Campos:

| Campo | Tipo | Obrigatório | Descrição |
|--------|------|-------------|-----------|
| roleIds | List<UUID> | Sim | Lista de identificadores dos papéis que serão associados ao usuário. |

---

### Exemplo de Requisição

DTO: `AssignRolesRequest`

```json
{
  "roleIds": [
    "c7bdf593-fb7d-4dbf-b72c-f50b4bc4c36f",
    "e6937c35-fdb9-4975-bb84-5e16f1d52cb9"
  ]
}
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ POST /users/{id}/roles
    ▼
Identity Service
    │
    │ Valida o Access Token
    │
    │ Verifica permissões
    │
    │ Localiza o usuário
    │
    │ Localiza os papéis
    │
    │ Cria as associações
    ▼
Banco de Dados
    ▼
Identity Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
UserResponse
```

---

### Exemplo de Resposta

DTO: `UserResponse`

```json
{
  "id": "efbdf77f-24dd-45b3-a130-3e60ef7b1f6a",
  "name": "João Silva",
  "email": "joao.silva@email.com",
  "roles": [
    {
      "id": "c7bdf593-fb7d-4dbf-b72c-f50b4bc4c36f",
      "name": "ADMIN"
    },
    {
      "id": "e6937c35-fdb9-4975-bb84-5e16f1d52cb9",
      "name": "ANALYST"
    }
  ],
  "active": true
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Papéis associados com sucesso. |
| 400 Bad Request | Requisição inválida. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para associar papéis. |
| 404 Not Found | Usuário ou papel não encontrado. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- O usuário deverá existir.
- Todos os papéis informados deverão existir.
- Papéis já associados ao usuário deverão ser ignorados.
- A operação poderá associar um ou mais papéis em uma única requisição.

---

### Observações

A associação de papéis não altera os dados cadastrais do usuário.

⬆️ [Voltar ao índice](#indice)

## 5.5.4 Remover Papéis do Usuário

### Objetivo

Remover a associação entre um usuário e um papel.

**Microsserviço:** Identity Service

**Recurso:** Usuários

**Versão da API:** v1

---

### Endpoint

```http
DELETE /api/v1/users/{id}/roles/{roleId}
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`USER_UPDATE`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |

---

### Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| id | UUID | Sim | Identificador do usuário. |
| roleId | UUID | Sim | Identificador do papel. |

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

Não possui.

---

### Exemplo de Requisição

```http
DELETE /api/v1/users/{id}/roles/{roleId}
Authorization: Bearer <access_token>
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ DELETE /users/{id}/roles/{roleId}
    ▼
Identity Service
    │
    │ Valida o Access Token
    │
    │ Verifica permissões
    │
    │ Localiza a associação
    │
    │ Remove a associação
    ▼
Banco de Dados
    ▼
Identity Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
MessageResponse
```

---

### Exemplo de Resposta

DTO: `MessageResponse`

```json
{
  "message": "Papel removido do usuário com sucesso."
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Papel removido com sucesso. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para remover papéis. |
| 404 Not Found | Usuário, papel ou associação não encontrada. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- O usuário deverá existir.
- O papel deverá existir.
- A associação entre usuário e papel deverá existir.
- A remoção afetará imediatamente as permissões efetivas do usuário.

---

### Observações

Caso o usuário possua outros papéis, suas respectivas permissões permanecerão inalteradas.

⬆️ [Voltar ao índice](#indice)

## 5.5.5 Listar Papéis de um Usuário

### Objetivo

Retornar todos os papéis atualmente associados a um usuário.

**Microsserviço:** Identity Service

**Recurso:** Usuários

**Versão da API:** v1

---

### Endpoint

```http
GET /api/v1/users/{id}/roles
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`USER_READ`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |

---

### Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| id | UUID | Sim | Identificador do usuário. |

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

Não possui.

---

### Exemplo de Requisição

```http
GET /api/v1/users/efbdf77f-24dd-45b3-a130-3e60ef7b1f6a/roles
Authorization: Bearer <access_token>
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ GET /users/{id}/roles
    ▼
Identity Service
    │
    │ Valida o Access Token
    │
    │ Verifica permissões
    │
    │ Localiza o usuário
    │
    │ Consulta os papéis associados
    ▼
Banco de Dados
    ▼
Identity Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
List<RoleResponse>
```

---

### Exemplo de Resposta

DTO: `List<RoleResponse>`

```json
[
  {
    "id": "c7bdf593-fb7d-4dbf-b72c-f50b4bc4c36f",
    "name": "ADMIN",
    "description": "Administrador da plataforma."
  },
  {
    "id": "e6937c35-fdb9-4975-bb84-5e16f1d52cb9",
    "name": "ANALYST",
    "description": "Analista de crédito."
  }
]
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Papéis retornados com sucesso. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para consultar papéis. |
| 404 Not Found | Usuário não encontrado. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- O usuário deverá existir.
- Apenas usuários autorizados poderão consultar os papéis.
- A resposta deverá conter apenas os papéis atualmente associados ao usuário.

---

### Observações

A resposta não inclui as permissões de cada papel. Essas informações podem ser obtidas pelos endpoints específicos de gerenciamento de permissões.

⬆️ [Voltar ao índice](#indice)

## 5.5.6 Associar Permissões ao Papel

### Objetivo

Associar uma ou mais permissões a um papel existente.

**Microsserviço:** Identity Service

**Recurso:** Papéis

**Versão da API:** v1

---

### Endpoint

```http
POST /api/v1/roles/{id}/permissions
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`ROLE_UPDATE`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |
| Content-Type | Sim | Deve ser `application/json`. |

---

### Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| id | UUID | Sim | Identificador do papel. |

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

DTO utilizado:

```text
AssignPermissionsRequest
```

Campos:

| Campo | Tipo | Obrigatório | Descrição |
|--------|------|-------------|-----------|
| permissionIds | List<UUID> | Sim | Lista de identificadores das permissões que serão associadas ao papel. |

---

### Exemplo de Requisição

DTO: `AssignPermissionsRequest`

```json
{
  "permissionIds": [
    "20a4d90d-2d3c-47b8-b12b-3471cf76970d",
    "4c39195b-4ea3-45cb-8bd4-df34666f7e38"
  ]
}
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ POST /roles/{id}/permissions
    ▼
Identity Service
    │
    │ Valida o Access Token
    │
    │ Verifica permissões
    │
    │ Localiza o papel
    │
    │ Localiza as permissões
    │
    │ Cria as associações
    ▼
Banco de Dados
    ▼
Identity Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
RoleResponse
```

---

### Exemplo de Resposta

DTO: `RoleResponse`

```json
{
  "id": "c7bdf593-fb7d-4dbf-b72c-f50b4bc4c36f",
  "name": "ADMIN",
  "description": "Administrador da plataforma."
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Permissões associadas com sucesso. |
| 400 Bad Request | Requisição inválida. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para alterar papéis. |
| 404 Not Found | Papel ou permissão não encontrado. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- O papel deverá existir.
- Todas as permissões informadas deverão existir.
- Permissões já associadas ao papel deverão ser ignoradas.
- A operação poderá associar uma ou mais permissões em uma única requisição.

---

### Observações

As permissões associadas passam a ser concedidas automaticamente aos usuários que possuírem esse papel.

⬆️ [Voltar ao índice](#indice)

## 5.5.7 Remover Permissões do Papel

### Objetivo

Remover a associação entre um papel e uma permissão.

**Microsserviço:** Identity Service

**Recurso:** Papéis

**Versão da API:** v1

---

### Endpoint

```http
DELETE /api/v1/roles/{id}/permissions/{permissionId}
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`ROLE_UPDATE`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |

---

### Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| id | UUID | Sim | Identificador do papel. |
| permissionId | UUID | Sim | Identificador da permissão. |

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

Não possui.

---

### Exemplo de Requisição

```http
DELETE /api/v1/roles/{id}/permissions/{permissionId}
Authorization: Bearer <access_token>
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ DELETE /roles/{id}/permissions/{permissionId}
    ▼
Identity Service
    │
    │ Valida o Access Token
    │
    │ Verifica permissões
    │
    │ Localiza a associação
    │
    │ Remove a associação
    ▼
Banco de Dados
    ▼
Identity Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
MessageResponse
```

---

### Exemplo de Resposta

DTO: `MessageResponse`

```json
{
  "message": "Permissão removida do papel com sucesso."
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Permissão removida com sucesso. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para alterar papéis. |
| 404 Not Found | Papel, permissão ou associação não encontrada. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- O papel deverá existir.
- A permissão deverá existir.
- A associação entre papel e permissão deverá existir.
- A remoção afetará imediatamente todos os usuários vinculados ao papel.

---

### Observações

Usuários que possuírem outros papéis continuarão recebendo as permissões provenientes desses papéis.

⬆️ [Voltar ao índice](#indice)

## 5.5.8 Listar Permissões de um Papel

### Objetivo

Retornar todas as permissões atualmente associadas a um papel.

**Microsserviço:** Identity Service

**Recurso:** Papéis

**Versão da API:** v1

---

### Endpoint

```http
GET /api/v1/roles/{id}/permissions
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`ROLE_READ`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |

---

### Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| id | UUID | Sim | Identificador do papel. |

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

Não possui.

---

### Exemplo de Requisição

```http
GET /api/v1/roles/c7bdf593-fb7d-4dbf-b72c-f50b4bc4c36f/permissions
Authorization: Bearer <access_token>
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ GET /roles/{id}/permissions
    ▼
Identity Service
    │
    │ Valida o Access Token
    │
    │ Verifica permissões
    │
    │ Localiza o papel
    │
    │ Consulta as permissões associadas
    ▼
Banco de Dados
    ▼
Identity Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
List<PermissionResponse>
```

---

### Exemplo de Resposta

DTO: `List<PermissionResponse>`

```json
[
  {
    "id": "20a4d90d-2d3c-47b8-b12b-3471cf76970d",
    "name": "CUSTOMER_CREATE",
    "description": "Permite cadastrar clientes."
  },
  {
    "id": "4c39195b-4ea3-45cb-8bd4-df34666f7e38",
    "name": "CUSTOMER_READ",
    "description": "Permite consultar clientes."
  }
]
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Permissões retornadas com sucesso. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para consultar papéis. |
| 404 Not Found | Papel não encontrado. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- O papel deverá existir.
- Apenas usuários autorizados poderão consultar as permissões.
- A resposta deverá conter apenas as permissões atualmente associadas ao papel.

---

### Observações

A resposta não inclui os usuários que possuem o papel. Essas informações podem ser obtidas pelos endpoints específicos de gerenciamento de usuários.

⬆️ [Voltar ao índice](#indice)


# 6 Customer Service

## 6.1 Objetivo

O Customer Service é responsável pelo gerenciamento dos clientes da plataforma.

Este microsserviço centraliza todas as informações cadastrais utilizadas durante o processo de análise de crédito, incluindo dados pessoais, documentos, endereços, contatos, vínculos empregatícios e rendas.

---

## 6.2 Recursos

O Customer Service disponibiliza os seguintes recursos:

- Clientes
- Endereços
- Contatos
- Documentos
- Vínculos Empregatícios
- Rendas

Os recursos relacionados são gerenciados por meio de sub-recursos REST.

⬆️ [Voltar ao índice](#indice)

## 6.3 Clientes

Os endpoints desta seção são responsáveis pelo gerenciamento dos clientes da plataforma.

## 6.3.1 Criar Cliente

### Objetivo

Cadastrar um novo cliente na plataforma.

**Microsserviço:** Customer Service

**Recurso:** Clientes

**Versão da API:** v1

---

### Endpoint

```http
POST /api/v1/customers
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`CUSTOMER_CREATE`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |
| Content-Type | Sim | Deve ser `application/json`. |

---

### Parâmetros de Caminho

Não possui.

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

DTO utilizado:

```text
CreateCustomerRequest
```

Campos:

| Campo | Tipo | Obrigatório | Descrição |
|--------|------|-------------|-----------|
| fullName | String | Sim | Nome completo do cliente. |
| birthDate | Date | Sim | Data de nascimento. |
| cpf | String | Sim | CPF do cliente. |
| maritalStatus | String | Sim | Estado civil. |
| nationality | String | Sim | Nacionalidade. |

---

### Exemplo de Requisição

DTO: `CreateCustomerRequest`

```json
{
  "fullName": "João da Silva",
  "birthDate": "1992-04-15",
  "cpf": "12345678901",
  "maritalStatus": "SINGLE",
  "nationality": "BRAZILIAN"
}
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ POST /customers
    ▼
Customer Service
    │
    │ Valida o Access Token
    │
    │ Verifica permissões
    │
    │ Valida os dados
    │
    │ Verifica CPF
    ▼
Banco de Dados
    │
    │ Persiste o cliente
    ▼
Customer Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
CustomerResponse
```

---

### Exemplo de Resposta

DTO: `CustomerResponse`

```json
{
  "id": "98cb9fb0-d944-4305-8eb8-c0b7ab5c69b8",
  "fullName": "João da Silva",
  "birthDate": "1992-04-15",
  "cpf": "12345678901",
  "maritalStatus": "SINGLE",
  "nationality": "BRAZILIAN",
  "createdAt": "2026-08-25T09:10:00Z",
  "updatedAt": "2026-08-25T09:10:00Z"
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 201 Created | Cliente criado com sucesso. |
| 400 Bad Request | Requisição inválida. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para cadastrar clientes. |
| 409 Conflict | Já existe um cliente cadastrado com o CPF informado. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- O CPF deverá ser válido.
- O CPF deverá ser único.
- O cliente deverá possuir idade igual ou superior à idade mínima definida pela política da plataforma.
- O nome completo deverá ser informado.

---

### Observações

Após o cadastro, o cliente poderá receber endereços, contatos, documentos, vínculos empregatícios e rendas por meio dos endpoints específicos.

⬆️ [Voltar ao índice](#indice)

## 6.3.2 Listar Clientes

### Objetivo

Retornar uma lista paginada dos clientes cadastrados.

**Microsserviço:** Customer Service

**Recurso:** Clientes

**Versão da API:** v1

---

### Endpoint

```http
GET /api/v1/customers
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`CUSTOMER_READ`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |

---

### Parâmetros de Caminho

Não possui.

---

### Parâmetros de Consulta

| Parâmetro | Tipo | Obrigatório | Descrição |
|------------|------|-------------|-----------|
| page | Integer | Não | Número da página. |
| size | Integer | Não | Quantidade de registros por página. |
| sort | String | Não | Campo utilizado para ordenação. |
| direction | String | Não | Direção da ordenação (`ASC` ou `DESC`). |

---

### Corpo da Requisição

Não possui.

---

### Exemplo de Requisição

```http
GET /api/v1/customers?page=0&size=20&sort=fullName&direction=ASC
Authorization: Bearer <access_token>
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ GET /customers
    ▼
Customer Service
    │
    │ Valida o Access Token
    │
    │ Verifica permissões
    ▼
Banco de Dados
    │
    │ Consulta clientes
    ▼
Customer Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
PageResponse<CustomerResponse>
```

---

### Exemplo de Resposta

DTO: `PageResponse<CustomerResponse>`

```json
{
  "content": [
    {
      "id": "98cb9fb0-d944-4305-8eb8-c0b7ab5c69b8",
      "fullName": "João da Silva",
      "cpf": "12345678901"
    },
    {
      "id": "20e95b53-fc1d-4b6b-b760-5675abccaf57",
      "fullName": "Maria Oliveira",
      "cpf": "98765432100"
    }
  ],
  "page": 0,
  "size": 20,
  "totalElements": 2,
  "totalPages": 1
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Clientes retornados com sucesso. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para consultar clientes. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- Apenas usuários autorizados poderão consultar clientes.
- A resposta deverá ser paginada.
- A ordenação deverá utilizar apenas campos permitidos.

---

### Observações

Os parâmetros de paginação seguem as convenções definidas em **01-convencoes-do-projeto.md**.

⬆️ [Voltar ao índice](#indice)

## 6.3.3 Buscar Cliente por Identificador

### Objetivo

Retornar os dados de um cliente específico.

**Microsserviço:** Customer Service

**Recurso:** Clientes

**Versão da API:** v1

---

### Endpoint

```http
GET /api/v1/customers/{id}
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`CUSTOMER_READ`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |

---

### Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| id | UUID | Sim | Identificador do cliente. |

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

Não possui.

---

### Exemplo de Requisição

```http
GET /api/v1/customers/98cb9fb0-d944-4305-8eb8-c0b7ab5c69b8
Authorization: Bearer <access_token>
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ GET /customers/{id}
    ▼
Customer Service
    │
    │ Valida o Access Token
    │
    │ Verifica permissões
    │
    │ Localiza o cliente
    ▼
Banco de Dados
    ▼
Customer Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
CustomerResponse
```

---

### Exemplo de Resposta

DTO: `CustomerResponse`

```json
{
  "id": "98cb9fb0-d944-4305-8eb8-c0b7ab5c69b8",
  "fullName": "João da Silva",
  "birthDate": "1992-04-15",
  "cpf": "12345678901",
  "maritalStatus": "SINGLE",
  "nationality": "BRAZILIAN",
  "createdAt": "2026-08-25T09:10:00Z",
  "updatedAt": "2026-08-25T09:10:00Z"
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Cliente encontrado com sucesso. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para consultar clientes. |
| 404 Not Found | Cliente não encontrado. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- O cliente deverá existir.
- Apenas usuários autorizados poderão consultar clientes.

---

### Observações

Este endpoint retorna apenas os dados do cliente. Endereços, contatos, documentos, vínculos empregatícios e rendas deverão ser consultados pelos respectivos endpoints.

⬆️ [Voltar ao índice](#indice)

## 6.3.4 Atualizar Cliente

### Objetivo

Atualizar os dados cadastrais de um cliente.

**Microsserviço:** Customer Service

**Recurso:** Clientes

**Versão da API:** v1

---

### Endpoint

```http
PUT /api/v1/customers/{id}
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`CUSTOMER_UPDATE`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |
| Content-Type | Sim | Deve ser `application/json`. |

---

### Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| id | UUID | Sim | Identificador do cliente. |

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

DTO utilizado:

```text
UpdateCustomerRequest
```

Campos:

| Campo | Tipo | Obrigatório | Descrição |
|--------|------|-------------|-----------|
| fullName | String | Sim | Nome completo do cliente. |
| birthDate | Date | Sim | Data de nascimento. |
| maritalStatus | String | Sim | Estado civil. |
| nationality | String | Sim | Nacionalidade. |

---

### Exemplo de Requisição

DTO: `UpdateCustomerRequest`

```json
{
  "fullName": "João Carlos da Silva",
  "birthDate": "1992-04-15",
  "maritalStatus": "MARRIED",
  "nationality": "BRAZILIAN"
}
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ PUT /customers/{id}
    ▼
Customer Service
    │
    │ Valida o Access Token
    │
    │ Verifica permissões
    │
    │ Localiza o cliente
    │
    │ Atualiza os dados
    ▼
Banco de Dados
    ▼
Customer Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
CustomerResponse
```

---

### Exemplo de Resposta

DTO: `CustomerResponse`

```json
{
  "id": "98cb9fb0-d944-4305-8eb8-c0b7ab5c69b8",
  "fullName": "João Carlos da Silva",
  "birthDate": "1992-04-15",
  "cpf": "12345678901",
  "maritalStatus": "MARRIED",
  "nationality": "BRAZILIAN",
  "createdAt": "2026-08-25T09:10:00Z",
  "updatedAt": "2026-08-28T13:45:00Z"
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Cliente atualizado com sucesso. |
| 400 Bad Request | Requisição inválida. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para atualizar clientes. |
| 404 Not Found | Cliente não encontrado. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- O cliente deverá existir.
- O CPF não poderá ser alterado.
- Apenas os dados cadastrais poderão ser atualizados.
- Todas as validações deverão ser executadas antes da persistência.

---

### Observações

Caso seja necessária a alteração de documentos, contatos, endereços, vínculos empregatícios ou rendas, deverão ser utilizados os respectivos endpoints.

⬆️ [Voltar ao índice](#indice)

## 6.3.5 Alterar Status do Cliente

### Objetivo

Alterar o status de um cliente da plataforma.

**Microsserviço:** Customer Service

**Recurso:** Clientes

**Versão da API:** v1

---

### Endpoint

```http
PATCH /api/v1/customers/{id}/status
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`CUSTOMER_UPDATE`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |
| Content-Type | Sim | Deve ser `application/json`. |

---

### Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| id | UUID | Sim | Identificador do cliente. |

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

DTO utilizado:

```text
UpdateCustomerStatusRequest
```

Campos:

| Campo | Tipo | Obrigatório | Descrição |
|--------|------|-------------|-----------|
| status | String | Sim | Novo status do cliente. |

Valores permitidos:

```text
ACTIVE
INACTIVE
BLOCKED
UNDER_REVIEW
```

---

### Exemplo de Requisição

DTO: `UpdateCustomerStatusRequest`

```json
{
    "status": "INACTIVE"
}
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ PATCH /customers/{id}/status
    ▼
Customer Service
    │
    │ Valida o Access Token
    │
    │ Verifica permissões
    │
    │ Localiza o cliente
    │
    │ Atualiza o status
    ▼
Banco de Dados
    │
    │ Persiste a alteração
    ▼
Customer Service
    │
    │ Publica evento para auditoria
    ▼
Audit Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
CustomerResponse
```

---

### Exemplo de Resposta

DTO: `CustomerResponse`

```json
{
    "id": "98cb9fb0-d944-4305-8eb8-c0b7ab5c69b8",
    "fullName": "João da Silva",
    "birthDate": "1992-04-15",
    "cpf": "12345678901",
    "maritalStatus": "SINGLE",
    "nationality": "BRAZILIAN",
    "status": "INACTIVE",
    "createdAt": "2026-08-25T09:10:00Z",
    "updatedAt": "2026-08-28T15:30:00Z"
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Status do cliente alterado com sucesso. |
| 400 Bad Request | Requisição inválida ou status informado inválido. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para alterar o status do cliente. |
| 404 Not Found | Cliente não encontrado. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- O cliente deverá existir.
- Apenas usuários autorizados poderão alterar o status do cliente.
- Clientes nunca serão removidos fisicamente da plataforma.
- O histórico do cliente será preservado.
- Clientes com status `INACTIVE` não poderão solicitar novas análises de crédito.
- Clientes com status `BLOCKED` não poderão realizar operações até a remoção do bloqueio.
- Toda alteração de status deverá ser registrada para fins de auditoria.

---

### Observações

A plataforma utiliza **inativação lógica (soft delete)** para preservar o histórico dos clientes e garantir a rastreabilidade das operações. Dessa forma, não existe endpoint para exclusão de clientes.

⬆️ [Voltar ao índice](#indice)

## 6.4 Endereços

### Objetivo

Gerenciar os endereços associados a um cliente.

Cada cliente poderá possuir um ou mais endereços cadastrados, utilizados para fins cadastrais, correspondência e análise de crédito.

Os endereços são tratados como sub-recursos do cliente e somente poderão existir vinculados a um cliente previamente cadastrado.

---

### Recursos

| Recurso | Descrição |
|---------|-----------|
| Endereço | Representa um endereço pertencente a um cliente. |

⬆️ [Voltar ao índice](#indice)

## 6.4.1 Adicionar Endereço

### Objetivo

Cadastrar um novo endereço para um cliente.

**Microsserviço:** Customer Service

**Recurso:** Endereços

**Versão da API:** v1

---

### Endpoint

```http
POST /api/v1/customers/{id}/addresses
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`CUSTOMER_UPDATE`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |
| Content-Type | Sim | Deve ser `application/json`. |

---

### Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| id | UUID | Sim | Identificador do cliente. |

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

DTO utilizado:

```text
CreateAddressRequest
```

Campos:

| Campo | Tipo | Obrigatório | Descrição |
|------|------|-------------|-----------|
| street | String | Sim | Logradouro. |
| number | String | Sim | Número. |
| complement | String | Não | Complemento. |
| neighborhood | String | Sim | Bairro. |
| city | String | Sim | Cidade. |
| state | String | Sim | Estado. |
| zipCode | String | Sim | CEP. |
| country | String | Sim | País. |
| addressType | String | Sim | Tipo do endereço. |
| primary | Boolean | Sim | Indica se é o endereço principal. |

Valores permitidos para `addressType`:

```text
RESIDENTIAL
COMMERCIAL
MAILING
```

---

### Exemplo de Requisição

DTO: `CreateAddressRequest`

```json
{
  "street": "Rua das Flores",
  "number": "120",
  "complement": "Apto 42",
  "neighborhood": "Centro",
  "city": "São Paulo",
  "state": "SP",
  "zipCode": "01000-000",
  "country": "Brasil",
  "addressType": "RESIDENTIAL",
  "primary": true
}
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ POST /customers/{id}/addresses
    ▼
Customer Service
    │
    │ Valida o Access Token
    │
    │ Verifica permissões
    │
    │ Localiza o cliente
    │
    │ Valida o endereço
    ▼
Banco de Dados
    ▼
Customer Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
AddressResponse
```

---

### Exemplo de Resposta

DTO: `AddressResponse`

```json
{
  "id": "ef93d6d2-47c5-49d3-a44d-49305f391905",
  "street": "Rua das Flores",
  "number": "120",
  "complement": "Apto 42",
  "neighborhood": "Centro",
  "city": "São Paulo",
  "state": "SP",
  "zipCode": "01000-000",
  "country": "Brasil",
  "addressType": "RESIDENTIAL",
  "primary": true,
  "createdAt": "2026-08-29T14:30:00Z",
  "updatedAt": "2026-08-29T14:30:00Z"
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 201 Created | Endereço cadastrado com sucesso. |
| 400 Bad Request | Dados inválidos. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para cadastrar endereços. |
| 404 Not Found | Cliente não encontrado. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- O cliente deverá existir.
- Cada endereço pertence a um único cliente.
- Um cliente poderá possuir vários endereços.
- Apenas um endereço poderá ser marcado como principal.
- Caso um novo endereço seja definido como principal, o endereço principal anterior deverá perder essa condição automaticamente.

---

### Observações

Os endereços são recursos independentes, porém sempre vinculados a um cliente.

⬆️ [Voltar ao índice](#indice)

## 6.4.2 Listar Endereços

### Objetivo

Listar todos os endereços associados a um cliente.

**Microsserviço:** Customer Service

**Recurso:** Endereços

**Versão da API:** v1

---

### Endpoint

```http
GET /api/v1/customers/{id}/addresses
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`CUSTOMER_READ`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |

---

### Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| id | UUID | Sim | Identificador do cliente. |

---

### Parâmetros de Consulta

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| page | Integer | Não | Número da página. |
| size | Integer | Não | Quantidade de registros por página. |
| sort | String | Não | Campo para ordenação. |

---

### Corpo da Requisição

Não possui.

---

### Exemplo de Requisição

```http
GET /api/v1/customers/98cb9fb0-d944-4305-8eb8-c0b7ab5c69b8/addresses?page=0&size=10&sort=street,asc
Authorization: Bearer <access_token>
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ GET /customers/{id}/addresses
    ▼
Customer Service
    │
    │ Valida o Access Token
    │
    │ Verifica permissões
    │
    │ Localiza o cliente
    ▼
Banco de Dados
    ▼
Customer Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
PageResponse<AddressResponse>
```

---

### Exemplo de Resposta

DTO: `PageResponse<AddressResponse>`

```json
{
  "content": [
    {
      "id": "ef93d6d2-47c5-49d3-a44d-49305f391905",
      "street": "Rua das Flores",
      "number": "120",
      "city": "São Paulo",
      "state": "SP",
      "zipCode": "01000-000",
      "addressType": "RESIDENTIAL",
      "primary": true,
      "status": "ACTIVE"
    }
  ],
  "page": 0,
  "size": 10,
  "totalElements": 1,
  "totalPages": 1
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Endereços listados com sucesso. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para consultar endereços. |
| 404 Not Found | Cliente não encontrado. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- O cliente deverá existir.
- Apenas usuários autorizados poderão consultar os endereços.
- O endpoint retornará apenas os endereços pertencentes ao cliente informado.

---

### Observações

O resultado será paginado conforme o padrão definido para a plataforma.

⬆️ [Voltar ao índice](#indice)

## 6.4.3 Buscar Endereço por Identificador

### Objetivo

Retornar um endereço específico de um cliente.

**Microsserviço:** Customer Service

**Recurso:** Endereços

**Versão da API:** v1

---

### Endpoint

```http
GET /api/v1/customers/{customerId}/addresses/{addressId}
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`CUSTOMER_READ`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |

---

### Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| customerId | UUID | Sim | Identificador do cliente. |
| addressId | UUID | Sim | Identificador do endereço. |

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

Não possui.

---

### Exemplo de Requisição

```http
GET /api/v1/customers/98cb9fb0-d944-4305-8eb8-c0b7ab5c69b8/addresses/ef93d6d2-47c5-49d3-a44d-49305f391905
Authorization: Bearer <access_token>
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ GET /customers/{customerId}/addresses/{addressId}
    ▼
Customer Service
    │
    │ Valida o Access Token
    │
    │ Verifica permissões
    │
    │ Localiza o cliente
    │
    │ Localiza o endereço
    ▼
Banco de Dados
    ▼
Customer Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
AddressResponse
```

---

### Exemplo de Resposta

```json
{
  "id": "ef93d6d2-47c5-49d3-a44d-49305f391905",
  "street": "Rua das Flores",
  "number": "120",
  "complement": "Apto 42",
  "neighborhood": "Centro",
  "city": "São Paulo",
  "state": "SP",
  "zipCode": "01000-000",
  "country": "Brasil",
  "addressType": "RESIDENTIAL",
  "primary": true,
  "status": "ACTIVE",
  "createdAt": "2026-08-29T14:30:00Z",
  "updatedAt": "2026-08-29T14:30:00Z"
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Endereço encontrado com sucesso. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para consultar endereços. |
| 404 Not Found | Cliente ou endereço não encontrado. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- O cliente deverá existir.
- O endereço deverá pertencer ao cliente informado.
- Apenas usuários autorizados poderão consultar endereços.

---

### Observações

Este endpoint retorna um único endereço pertencente ao cliente.

⬆️ [Voltar ao índice](#indice)

## 6.4.4 Atualizar Endereço

### Objetivo

Atualizar os dados de um endereço pertencente a um cliente.

**Microsserviço:** Customer Service

**Recurso:** Endereços

**Versão da API:** v1

---

### Endpoint

```http
PUT /api/v1/customers/{customerId}/addresses/{addressId}
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`CUSTOMER_UPDATE`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |
| Content-Type | Sim | Deve ser `application/json`. |

---

### Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| customerId | UUID | Sim | Identificador do cliente. |
| addressId | UUID | Sim | Identificador do endereço. |

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

DTO utilizado:

```text
UpdateAddressRequest
```

Campos:

| Campo | Tipo | Obrigatório | Descrição |
|--------|------|-------------|-----------|
| street | String | Sim | Logradouro. |
| number | String | Sim | Número. |
| complement | String | Não | Complemento. |
| neighborhood | String | Sim | Bairro. |
| city | String | Sim | Cidade. |
| state | String | Sim | Estado. |
| zipCode | String | Sim | CEP. |
| country | String | Sim | País. |
| addressType | String | Sim | Tipo do endereço. |
| primary | Boolean | Sim | Indica se é o endereço principal. |

Valores permitidos para `addressType`:

```text
RESIDENTIAL
COMMERCIAL
MAILING
```

---

### Exemplo de Requisição

DTO: `UpdateAddressRequest`

```json
{
  "street": "Rua das Palmeiras",
  "number": "350",
  "complement": "Casa 2",
  "neighborhood": "Jardins",
  "city": "São Paulo",
  "state": "SP",
  "zipCode": "01400-000",
  "country": "Brasil",
  "addressType": "RESIDENTIAL",
  "primary": true
}
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ PUT /customers/{customerId}/addresses/{addressId}
    ▼
Customer Service
    │
    │ Valida o Access Token
    │
    │ Verifica permissões
    │
    │ Localiza o cliente
    │
    │ Localiza o endereço
    │
    │ Atualiza os dados
    ▼
Banco de Dados
    ▼
Customer Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
AddressResponse
```

---

### Exemplo de Resposta

```json
{
  "id": "ef93d6d2-47c5-49d3-a44d-49305f391905",
  "street": "Rua das Palmeiras",
  "number": "350",
  "complement": "Casa 2",
  "neighborhood": "Jardins",
  "city": "São Paulo",
  "state": "SP",
  "zipCode": "01400-000",
  "country": "Brasil",
  "addressType": "RESIDENTIAL",
  "primary": true,
  "status": "ACTIVE",
  "createdAt": "2026-08-29T14:30:00Z",
  "updatedAt": "2026-08-29T16:45:00Z"
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Endereço atualizado com sucesso. |
| 400 Bad Request | Dados inválidos. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para atualizar endereços. |
| 404 Not Found | Cliente ou endereço não encontrado. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- O cliente deverá existir.
- O endereço deverá pertencer ao cliente informado.
- Apenas usuários autorizados poderão atualizar endereços.
- Apenas um endereço poderá permanecer como principal.
- Caso um endereço seja definido como principal, o endereço principal anterior deverá perder essa condição automaticamente.

---

### Observações

A atualização substitui integralmente os dados do endereço.

⬆️ [Voltar ao índice](#indice)

## 6.4.5 Alterar Status do Endereço

### Objetivo

Alterar o status de um endereço pertencente a um cliente.

**Microsserviço:** Customer Service

**Recurso:** Endereços

**Versão da API:** v1

---

### Endpoint

```http
PATCH /api/v1/customers/{customerId}/addresses/{addressId}/status
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`CUSTOMER_UPDATE`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |
| Content-Type | Sim | Deve ser `application/json`. |

---

### Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| customerId | UUID | Sim | Identificador do cliente. |
| addressId | UUID | Sim | Identificador do endereço. |

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

DTO utilizado:

```text
UpdateAddressStatusRequest
```

Campos:

| Campo | Tipo | Obrigatório | Descrição |
|--------|------|-------------|-----------|
| status | String | Sim | Novo status do endereço. |

Valores permitidos:

```text
ACTIVE
INACTIVE
```

---

### Exemplo de Requisição

DTO: `UpdateAddressStatusRequest`

```json
{
  "status": "INACTIVE"
}
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ PATCH /customers/{customerId}/addresses/{addressId}/status
    ▼
Customer Service
    │
    │ Valida o Access Token
    │
    │ Verifica permissões
    │
    │ Localiza o cliente
    │
    │ Localiza o endereço
    │
    │ Atualiza o status
    ▼
Banco de Dados
    │
    │ Persiste a alteração
    ▼
Customer Service
    │
    │ Publica evento para auditoria
    ▼
Audit Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
AddressResponse
```

---

### Exemplo de Resposta

```json
{
  "id": "ef93d6d2-47c5-49d3-a44d-49305f391905",
  "street": "Rua das Palmeiras",
  "number": "350",
  "complement": "Casa 2",
  "neighborhood": "Jardins",
  "city": "São Paulo",
  "state": "SP",
  "zipCode": "01400-000",
  "country": "Brasil",
  "addressType": "RESIDENTIAL",
  "primary": true,
  "status": "INACTIVE",
  "createdAt": "2026-08-29T14:30:00Z",
  "updatedAt": "2026-08-29T17:10:00Z"
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Status do endereço alterado com sucesso. |
| 400 Bad Request | Requisição inválida ou status informado inválido. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para alterar o status do endereço. |
| 404 Not Found | Cliente ou endereço não encontrado. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- O cliente deverá existir.
- O endereço deverá pertencer ao cliente informado.
- Apenas usuários autorizados poderão alterar o status.
- Endereços nunca serão removidos fisicamente da plataforma.
- O histórico do endereço será preservado.
- Caso um endereço principal seja inativado, outro endereço ativo poderá ser definido como principal.

---

### Observações

A plataforma utiliza inativação lógica (*soft delete*) para preservar o histórico dos endereços.

⬆️ [Voltar ao índice](#indice)


## 6.5 Contatos

### Objetivo

Gerenciar os contatos associados a um cliente.

Cada cliente poderá possuir um ou mais contatos cadastrados, utilizados para comunicação, notificações e validação cadastral.

Os contatos são tratados como sub-recursos do cliente e somente poderão existir vinculados a um cliente previamente cadastrado.

---

### Recursos

| Recurso | Descrição |
|---------|-----------|
| Contato | Representa uma informação de contato pertencente a um cliente. |

⬆️ [Voltar ao índice](#indice)

## 6.5.1 Adicionar Contato

### Objetivo

Cadastrar um novo contato para um cliente.

**Microsserviço:** Customer Service

**Recurso:** Contatos

**Versão da API:** v1

---

### Endpoint

```http
POST /api/v1/customers/{customerId}/contacts
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`CUSTOMER_UPDATE`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |
| Content-Type | Sim | Deve ser `application/json`. |

---

### Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| customerId | UUID | Sim | Identificador do cliente. |

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

DTO utilizado:

```text
CreateContactRequest
```

Campos:

| Campo | Tipo | Obrigatório | Descrição |
|--------|------|-------------|-----------|
| contactType | String | Sim | Tipo do contato. |
| value | String | Sim | Valor do contato. |
| primary | Boolean | Sim | Indica se é o contato principal. |

Valores permitidos para `contactType`:

```text
EMAIL
MOBILE_PHONE
HOME_PHONE
WORK_PHONE
```

---

### Exemplo de Requisição

DTO: `CreateContactRequest`

```json
{
  "contactType": "EMAIL",
  "value": "joao.silva@email.com",
  "primary": true
}
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ POST /customers/{customerId}/contacts
    ▼
Customer Service
    │
    │ Valida o Access Token
    │
    │ Verifica permissões
    │
    │ Localiza o cliente
    │
    │ Valida o contato
    ▼
Banco de Dados
    ▼
Customer Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
ContactResponse
```

---

### Exemplo de Resposta

```json
{
  "id": "d7d273ef-24dd-4b54-92c5-c9fd2d1b7c58",
  "contactType": "EMAIL",
  "value": "joao.silva@email.com",
  "primary": true,
  "status": "ACTIVE",
  "createdAt": "2026-08-29T18:30:00Z",
  "updatedAt": "2026-08-29T18:30:00Z"
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 201 Created | Contato cadastrado com sucesso. |
| 400 Bad Request | Dados inválidos. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para cadastrar contatos. |
| 404 Not Found | Cliente não encontrado. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- O cliente deverá existir.
- Cada contato pertence a um único cliente.
- Um cliente poderá possuir vários contatos.
- Apenas um contato poderá ser definido como principal para cada tipo de contato.
- Caso um novo contato seja definido como principal, o contato principal anterior do mesmo tipo perderá essa condição automaticamente.

---

### Observações

Os contatos são recursos independentes, porém sempre vinculados a um cliente.

⬆️ [Voltar ao índice](#indice)

## 6.5.2 Listar Contatos

### Objetivo

Listar todos os contatos associados a um cliente.

**Microsserviço:** Customer Service

**Recurso:** Contatos

**Versão da API:** v1

---

### Endpoint

```http
GET /api/v1/customers/{customerId}/contacts
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`CUSTOMER_READ`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |

---

### Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| customerId | UUID | Sim | Identificador do cliente. |

---

### Parâmetros de Consulta

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| page | Integer | Não | Número da página. |
| size | Integer | Não | Quantidade de registros por página. |
| sort | String | Não | Campo para ordenação. |

---

### Corpo da Requisição

Não possui.

---

### Exemplo de Requisição

```http
GET /api/v1/customers/98cb9fb0-d944-4305-8eb8-c0b7ab5c69b8/contacts?page=0&size=10&sort=contactType,asc
Authorization: Bearer <access_token>
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ GET /customers/{customerId}/contacts
    ▼
Customer Service
    │
    │ Valida o Access Token
    │
    │ Verifica permissões
    │
    │ Localiza o cliente
    ▼
Banco de Dados
    ▼
Customer Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
PageResponse<ContactResponse>
```

---

### Exemplo de Resposta

```json
{
  "content": [
    {
      "id": "d7d273ef-24dd-4b54-92c5-c9fd2d1b7c58",
      "contactType": "EMAIL",
      "value": "joao.silva@email.com",
      "primary": true,
      "status": "ACTIVE"
    }
  ],
  "page": 0,
  "size": 10,
  "totalElements": 1,
  "totalPages": 1
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Contatos listados com sucesso. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para consultar contatos. |
| 404 Not Found | Cliente não encontrado. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- O cliente deverá existir.
- Apenas usuários autorizados poderão consultar contatos.
- O endpoint retornará apenas os contatos pertencentes ao cliente informado.

---

### Observações

O resultado será paginado conforme o padrão definido para a plataforma.

⬆️ [Voltar ao índice](#indice)

## 6.5.3 Buscar Contato por Identificador

### Objetivo

Retornar um contato específico de um cliente.

**Microsserviço:** Customer Service

**Recurso:** Contatos

**Versão da API:** v1

---

### Endpoint

```http
GET /api/v1/customers/{customerId}/contacts/{contactId}
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`CUSTOMER_READ`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |

---

### Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| customerId | UUID | Sim | Identificador do cliente. |
| contactId | UUID | Sim | Identificador do contato. |

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

Não possui.

---

### Exemplo de Requisição

```http
GET /api/v1/customers/98cb9fb0-d944-4305-8eb8-c0b7ab5c69b8/contacts/d7d273ef-24dd-4b54-92c5-c9fd2d1b7c58
Authorization: Bearer <access_token>
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ GET /customers/{customerId}/contacts/{contactId}
    ▼
Customer Service
    │
    │ Valida o Access Token
    │
    │ Verifica permissões
    │
    │ Localiza o cliente
    │
    │ Localiza o contato
    ▼
Banco de Dados
    ▼
Customer Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
ContactResponse
```

---

### Exemplo de Resposta

```json
{
  "id": "d7d273ef-24dd-4b54-92c5-c9fd2d1b7c58",
  "contactType": "EMAIL",
  "value": "joao.silva@email.com",
  "primary": true,
  "status": "ACTIVE",
  "createdAt": "2026-08-29T18:30:00Z",
  "updatedAt": "2026-08-29T18:30:00Z"
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Contato encontrado com sucesso. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para consultar contatos. |
| 404 Not Found | Cliente ou contato não encontrado. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- O cliente deverá existir.
- O contato deverá pertencer ao cliente informado.
- Apenas usuários autorizados poderão consultar contatos.

---

### Observações

Este endpoint retorna um único contato pertencente ao cliente.

⬆️ [Voltar ao índice](#indice)

## 6.5.4 Atualizar Contato

### Objetivo

Atualizar os dados de um contato pertencente a um cliente.

**Microsserviço:** Customer Service

**Recurso:** Contatos

**Versão da API:** v1

---

### Endpoint

```http
PUT /api/v1/customers/{customerId}/contacts/{contactId}
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`CUSTOMER_UPDATE`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |
| Content-Type | Sim | Deve ser `application/json`. |

---

### Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| customerId | UUID | Sim | Identificador do cliente. |
| contactId | UUID | Sim | Identificador do contato. |

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

DTO utilizado:

```text
UpdateContactRequest
```

Campos:

| Campo | Tipo | Obrigatório | Descrição |
|--------|------|-------------|-----------|
| contactType | String | Sim | Tipo do contato. |
| value | String | Sim | Valor do contato. |
| primary | Boolean | Sim | Indica se é o contato principal. |

Valores permitidos para `contactType`:

```text
EMAIL
MOBILE_PHONE
HOME_PHONE
WORK_PHONE
```

---

### Exemplo de Requisição

DTO: `UpdateContactRequest`

```json
{
  "contactType": "EMAIL",
  "value": "joao.silva@empresa.com.br",
  "primary": true
}
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ PUT /customers/{customerId}/contacts/{contactId}
    ▼
Customer Service
    │
    │ Valida o Access Token
    │
    │ Verifica permissões
    │
    │ Localiza o cliente
    │
    │ Localiza o contato
    │
    │ Atualiza os dados
    ▼
Banco de Dados
    ▼
Customer Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
ContactResponse
```

---

### Exemplo de Resposta

```json
{
  "id": "d7d273ef-24dd-4b54-92c5-c9fd2d1b7c58",
  "contactType": "EMAIL",
  "value": "joao.silva@empresa.com.br",
  "primary": true,
  "status": "ACTIVE",
  "createdAt": "2026-08-29T18:30:00Z",
  "updatedAt": "2026-08-29T19:10:00Z"
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Contato atualizado com sucesso. |
| 400 Bad Request | Dados inválidos. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para atualizar contatos. |
| 404 Not Found | Cliente ou contato não encontrado. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- O cliente deverá existir.
- O contato deverá pertencer ao cliente informado.
- Apenas usuários autorizados poderão atualizar contatos.
- Apenas um contato poderá ser definido como principal para cada tipo de contato.
- Caso um contato seja definido como principal, o contato principal anterior do mesmo tipo deverá perder essa condição automaticamente.

---

### Observações

A atualização substitui integralmente os dados do contato.

⬆️ [Voltar ao índice](#indice)

## 6.5.5 Alterar Status do Contato

### Objetivo

Alterar o status de um contato pertencente a um cliente.

**Microsserviço:** Customer Service

**Recurso:** Contatos

**Versão da API:** v1

---

### Endpoint

```http
PATCH /api/v1/customers/{customerId}/contacts/{contactId}/status
```

---

### Autenticação

Obrigatória (Bearer Token).

---

### Permissões

`CUSTOMER_UPDATE`

---

### Cabeçalhos

| Nome | Obrigatório | Descrição |
|------|-------------|-----------|
| Authorization | Sim | Access Token no formato `Bearer <access_token>`. |
| Content-Type | Sim | Deve ser `application/json`. |

---

### Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| customerId | UUID | Sim | Identificador do cliente. |
| contactId | UUID | Sim | Identificador do contato. |

---

### Parâmetros de Consulta

Não possui.

---

### Corpo da Requisição

DTO utilizado:

```text
UpdateContactStatusRequest
```

Campos:

| Campo | Tipo | Obrigatório | Descrição |
|--------|------|-------------|-----------|
| status | String | Sim | Novo status do contato. |

Valores permitidos:

```text
ACTIVE
INACTIVE
```

---

### Exemplo de Requisição

DTO: `UpdateContactStatusRequest`

```json
{
  "status": "INACTIVE"
}
```

---

### Fluxo da Requisição

```text
Cliente
    │
    │ PATCH /customers/{customerId}/contacts/{contactId}/status
    ▼
Customer Service
    │
    │ Valida o Access Token
    │
    │ Verifica permissões
    │
    │ Localiza o cliente
    │
    │ Localiza o contato
    │
    │ Atualiza o status
    ▼
Banco de Dados
    │
    │ Persiste a alteração
    ▼
Customer Service
    │
    │ Publica evento para auditoria
    ▼
Audit Service
    ▼
Cliente
```

---

### Resposta de Sucesso

DTO utilizado:

```text
ContactResponse
```

---

### Exemplo de Resposta

```json
{
  "id": "d7d273ef-24dd-4b54-92c5-c9fd2d1b7c58",
  "contactType": "EMAIL",
  "value": "joao.silva@email.com",
  "primary": true,
  "status": "INACTIVE",
  "createdAt": "2026-08-29T18:30:00Z",
  "updatedAt": "2026-08-29T19:45:00Z"
}
```

---

### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 OK | Status do contato alterado com sucesso. |
| 400 Bad Request | Requisição inválida ou status informado inválido. |
| 401 Unauthorized | Usuário não autenticado. |
| 403 Forbidden | Usuário sem permissão para alterar o status do contato. |
| 404 Not Found | Cliente ou contato não encontrado. |
| 500 Internal Server Error | Erro interno do servidor. |

---

### Regras de Negócio

- O cliente deverá existir.
- O contato deverá pertencer ao cliente informado.
- Apenas usuários autorizados poderão alterar o status.
- Contatos nunca serão removidos fisicamente da plataforma.
- O histórico do contato será preservado.
- Caso um contato principal seja inativado, outro contato ativo do mesmo tipo poderá ser definido como principal.

---

### Observações

A plataforma utiliza inativação lógica (*soft delete*) para preservar o histórico dos contatos.

⬆️ [Voltar ao índice](#indice)

## 6.6 Vínculos Empregatícios

### 6.6.1 Objetivo

O recurso **Vínculos Empregatícios** é responsável por representar o histórico profissional do cliente.

Cada vínculo registra exclusivamente a relação entre o cliente e uma empresa, não armazenando informações financeiras. As rendas provenientes desse vínculo serão representadas pelo recurso **Rendas (Income)**, que é a única fonte de verdade para informações financeiras relacionadas aos rendimentos do cliente.

Um cliente pode possuir nenhum, um ou vários vínculos empregatícios ao longo da vida.

### Recursos

- Adicionar vínculo empregatício.
- Listar vínculos empregatícios.
- Buscar vínculo empregatício por identificador.
- Atualizar vínculo empregatício.
- Encerrar vínculo empregatício.

---

### 6.6.2 Adicionar Vínculo Empregatício

#### Objetivo

Cadastrar um novo vínculo empregatício para um cliente.

#### Microsserviço

Customer Service

#### Recurso

Employment

#### Versão da API

v1

#### Endpoint

```http
POST /customers/{customerId}/employments
```

#### Autenticação

Bearer Token (JWT)

#### Permissões

- CUSTOMER_WRITE

#### Cabeçalhos

```http
Authorization: Bearer <token>
Content-Type: application/json
```

#### Parâmetros de Caminho

| Nome | Tipo | Obrigatório | Descrição |
|------|------|-------------|-----------|
| customerId | UUID | Sim | Identificador do cliente. |

#### Corpo da Requisição

```json
{
    "companyName": "Tech Solutions",
    "companyTaxId": "12345678000195",
    "position": "Backend Developer",
    "employmentType": "CLT",
    "startDate": "2024-01-10"
}
```

#### Fluxo da Requisição

1. Validar autenticação.
2. Verificar existência do cliente.
3. Validar dados informados.
4. Criar vínculo empregatício.
5. Retornar o vínculo criado.

#### Resposta de Sucesso

**HTTP 201 Created**

#### Exemplo de Resposta

```json
{
    "id": "uuid",
    "companyName": "Tech Solutions",
    "companyTaxId": "12345678000195",
    "position": "Backend Developer",
    "employmentType": "CLT",
    "startDate": "2024-01-10",
    "endDate": null,
    "createdAt": "2026-07-26T15:20:10Z",
    "updatedAt": "2026-07-26T15:20:10Z"
}
```

#### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 201 | Vínculo criado com sucesso. |
| 400 | Dados inválidos. |
| 401 | Não autenticado. |
| 403 | Sem permissão. |
| 404 | Cliente não encontrado. |
| 409 | Conflito de negócio. |

#### Regras de Negócio

- O cliente deve existir.
- A data de admissão é obrigatória.
- O vínculo inicia ativo quando `endDate` é nulo.
- Não são armazenadas informações financeiras neste recurso.

#### Observações

- O histórico profissional é preservado.
- As rendas associadas ao vínculo serão cadastradas no recurso **Income**.

⬆️ [Voltar ao índice](#indice)

---

### 6.6.3 Listar Vínculos Empregatícios

```
GET /customers/{customerId}/employments
```

Retorna todos os vínculos empregatícios cadastrados para o cliente.

⬆️ [Voltar ao índice](#indice)

---

### 6.6.4 Buscar Vínculo Empregatício por Identificador

```
GET /customers/{customerId}/employments/{employmentId}
```

Retorna um vínculo empregatício específico.

⬆️ [Voltar ao índice](#indice)

---

### 6.6.5 Atualizar Vínculo Empregatício

```
PUT /customers/{customerId}/employments/{employmentId}
```

Atualiza as informações do vínculo empregatício.

Campos permitidos:

- companyName
- companyTaxId
- position
- employmentType
- startDate

Não é permitido atualizar o identificador do vínculo.

⬆️ [Voltar ao índice](#indice)

---

### 6.6.6 Encerrar Vínculo Empregatício

#### Endpoint

```http
PATCH /customers/{customerId}/employments/{employmentId}/termination
```

#### Objetivo

Registrar o encerramento do vínculo empregatício.

#### Corpo da Requisição

```json
{
    "endDate": "2026-08-31"
}
```

#### Regras de Negócio

- O vínculo deve existir.
- O vínculo deve pertencer ao cliente informado.
- O vínculo não pode estar encerrado.
- A data de encerramento deve ser igual ou posterior à data de admissão.
- O histórico do vínculo é preservado.
- O vínculo nunca é removido fisicamente.

#### Resposta de Sucesso

**HTTP 200 OK**

Retorna o vínculo empregatício atualizado.

⬆️ [Voltar ao índice](#indice)

## 6.7 Rendas

### 6.7.1 Objetivo

O recurso **Rendas** é responsável por representar todas as fontes de renda declaradas pelo cliente que poderão ser utilizadas durante o processo de análise de crédito.

Cada registro representa uma única fonte de renda.

Este recurso é a única fonte de verdade para informações financeiras relacionadas aos rendimentos do cliente.

### Recursos

- Adicionar renda.
- Listar rendas.
- Buscar renda por identificador.
- Atualizar renda.
- Encerrar renda.

---

### 6.7.2 Adicionar Renda

#### Objetivo

Cadastrar uma nova fonte de renda para um cliente.

#### Microsserviço

Customer Service

#### Recurso

Income

#### Versão da API

v1

#### Endpoint

```http
POST /customers/{customerId}/incomes
```

#### Autenticação

Bearer Token (JWT)

#### Permissões

- CUSTOMER_WRITE

#### Cabeçalhos

```http
Authorization: Bearer <token>
Content-Type: application/json
```

#### Parâmetros de Caminho

| Nome | Tipo | Obrigatório | Descrição |
|------|------|-------------|-----------|
| customerId | UUID | Sim | Identificador do cliente. |

#### Corpo da Requisição

```json
{
    "incomeType": "SALARY",
    "sourceName": "Tech Solutions Ltda.",
    "amount": 8500.00,
    "currency": "BRL",
    "frequency": "MONTHLY",
    "incomeVerificationStatus": "DECLARED",
    "startDate": "2024-01-10",
    "employmentId": "uuid"
}
```

#### Fluxo da Requisição

1. Validar autenticação.
2. Verificar existência do cliente.
3. Validar os dados informados.
4. Verificar o vínculo empregatício, quando informado.
5. Cadastrar a renda.
6. Retornar a renda criada.

#### Resposta de Sucesso

**HTTP 201 Created**

#### Exemplo de Resposta

```json
{
    "id": "uuid",
    "incomeType": "SALARY",
    "sourceName": "Tech Solutions Ltda.",
    "amount": 8500.00,
    "currency": "BRL",
    "frequency": "MONTHLY",
    "incomeVerificationStatus": "DECLARED",
    "startDate": "2024-01-10",
    "endDate": null,
    "employmentId": "uuid",
    "createdAt": "2026-07-26T15:20:10Z",
    "updatedAt": "2026-07-26T15:20:10Z"
}
```

#### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 201 | Renda cadastrada com sucesso. |
| 400 | Dados inválidos. |
| 401 | Não autenticado. |
| 403 | Sem permissão. |
| 404 | Cliente ou vínculo empregatício não encontrado. |
| 409 | Conflito de negócio. |

#### Regras de Negócio

- O cliente deve existir.
- O valor da renda deve ser maior que zero.
- A moeda deve ser suportada pelo sistema.
- O vínculo empregatício, quando informado, deve pertencer ao cliente.
- O recurso representa uma única fonte de renda.

#### Observações

- As informações financeiras são centralizadas neste recurso.
- Não são armazenadas informações profissionais.

⬆️ [Voltar ao índice](#indice)

---

### 6.7.3 Listar Rendas

#### Objetivo

Listar todas as rendas cadastradas para um cliente.

#### Microsserviço

Customer Service

#### Recurso

Income

#### Versão da API

v1

#### Endpoint

```http
GET /customers/{customerId}/incomes
```

#### Resposta de Sucesso

**HTTP 200 OK**

Retorna todas as rendas cadastradas para o cliente.

#### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 | Consulta realizada com sucesso. |
| 401 | Não autenticado. |
| 403 | Sem permissão. |
| 404 | Cliente não encontrado. |

⬆️ [Voltar ao índice](#indice)

---

### 6.7.4 Buscar Renda por Identificador

#### Objetivo

Buscar uma renda específica.

#### Microsserviço

Customer Service

#### Recurso

Income

#### Versão da API

v1

#### Endpoint

```http
GET /customers/{customerId}/incomes/{incomeId}
```

#### Resposta de Sucesso

**HTTP 200 OK**

Retorna a renda solicitada.

#### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 | Consulta realizada com sucesso. |
| 401 | Não autenticado. |
| 403 | Sem permissão. |
| 404 | Cliente ou renda não encontrados. |

⬆️ [Voltar ao índice](#indice)

---

### 6.7.5 Atualizar Renda

#### Objetivo

Atualizar as informações de uma renda.

#### Microsserviço

Customer Service

#### Recurso

Income

#### Versão da API

v1

#### Endpoint

```http
PUT /customers/{customerId}/incomes/{incomeId}
```

#### Campos Atualizáveis

- incomeType
- sourceName
- amount
- currency
- frequency
- incomeVerificationStatus
- startDate
- employmentId

#### Regras de Negócio

- A renda deve pertencer ao cliente.
- O valor deve ser maior que zero.
- O vínculo empregatício, quando informado, deve pertencer ao cliente.
- Não é permitido alterar o identificador da renda.

#### Resposta de Sucesso

**HTTP 200 OK**

Retorna a renda atualizada.

⬆️ [Voltar ao índice](#indice)

---

### 6.7.6 Encerrar Renda

#### Objetivo

Registrar o encerramento de uma fonte de renda.

#### Microsserviço

Customer Service

#### Recurso

Income

#### Versão da API

v1

#### Endpoint

```http
PATCH /customers/{customerId}/incomes/{incomeId}/termination
```

#### Corpo da Requisição

```json
{
    "endDate": "2026-08-31"
}
```

#### Regras de Negócio

- A renda deve existir.
- A renda deve pertencer ao cliente.
- A renda não pode estar encerrada.
- A data de encerramento deve ser igual ou posterior à data de início.
- O histórico da renda é preservado.
- A renda nunca é removida fisicamente.

#### Resposta de Sucesso

**HTTP 200 OK**

Retorna a renda atualizada.

#### Códigos HTTP

| Código | Descrição |
|---------|-----------|
| 200 | Renda encerrada com sucesso. |
| 400 | Dados inválidos. |
| 401 | Não autenticado. |
| 403 | Sem permissão. |
| 404 | Cliente ou renda não encontrados. |
| 409 | Conflito de negócio. |

⬆️ [Voltar ao índice](#indice)

