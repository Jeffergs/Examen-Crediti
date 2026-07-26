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

As APIs do **Examen Crediti** utilizam autenticação baseada em **JSON Web Token (JWT)**.

Após a autenticação do usuário, é emitido um token de acesso que deverá ser enviado em todas as requisições para recursos protegidos.

O token deve ser informado no cabeçalho **Authorization**, utilizando o esquema **Bearer**.

Exemplo:

```http
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI...
```

As permissões de acesso são definidas de acordo com os papéis (*Roles*) e permissões (*Permissions*) associados ao usuário autenticado.

Cada endpoint informa as permissões necessárias para sua utilização.

Quando o token estiver ausente, inválido, expirado ou o usuário não possuir autorização para acessar determinado recurso, a API retornará o código HTTP apropriado.

| Código HTTP | Situação |
|--------------|----------|
| 401 Unauthorized | Usuário não autenticado ou token inválido. |
| 403 Forbidden | Usuário autenticado, porém sem permissão para acessar o recurso. |

Os detalhes sobre geração, validação, renovação e revogação de tokens são apresentados no documento **09-seguranca.md**.

---

⬆️ [Voltar ao índice](#indice)


