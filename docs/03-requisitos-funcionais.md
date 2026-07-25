
# 📋 Requisitos Funcionais

## 🎯 Objetivo

Este documento descreve os requisitos funcionais do **Examen Crediti**, especificando as funcionalidades que o sistema deve oferecer.

Seu objetivo é definir o comportamento esperado da aplicação, servindo como base para a implementação, modelagem, arquitetura, testes e documentação das APIs.

---

<a id="indice"></a>

# 📑 Índice

1. [📦 Escopo Funcional](#escopo)
2. [🏗️ Visão Geral dos Microsserviços](#visao)
3. [📋 Requisitos Funcionais](#rf)
   - [3.1 Auth Service](#aut)
   - [3.2 Customer Service](#customer)
   - [3.3 Credit Analysis Service](#credit)
   - [3.4 Audit Service](#audit)
4. [🔗 Matriz de Rastreabilidade](#matriz)
5. [📚 Referências](#ref)

---


<a id="escopo"></a>
## 📦 Escopo Funcional

Este documento descreve exclusivamente as funcionalidades que devem ser implementadas pelo sistema.

As regras de negócio, arquitetura, modelagem de dados, APIs, segurança, observabilidade e demais aspectos técnicos são detalhados em seus respectivos documentos.

---

⬆️ [Voltar ao índice](#indice)


<a id="visao"></a>
## 🏗️ Visão Geral dos Microsserviços

| Microsserviço | Responsabilidade |
|---------------|------------------|
| `auth-service` | Autenticação e autorização de usuários. |
| `customer-service` | Gerenciamento de clientes. |
| `credit-analysis-service` | Realização da análise de crédito. |
| `audit-service` | Registro e consulta dos eventos de auditoria. |

---

⬆️ [Voltar ao índice](#indice)

<a id="rf"></a>
# 📋 Requisitos Funcionais

<a id="aut"></a>
## 4.1 🔐 Auth Service

### Autenticação

| Código | Requisito |
|---------|-----------|
| RF-001 | O sistema deve permitir a autenticação de usuários. |
| RF-002 | O sistema deve emitir um token JWT após autenticação bem-sucedida. |
| RF-003 | O sistema deve permitir a renovação do token de acesso. |
| RF-004 | O sistema deve permitir o encerramento da sessão do usuário. |

### Autorização

| Código | Requisito |
|---------|-----------|
| RF-005 | O sistema deve validar o token JWT em requisições protegidas. |
| RF-006 | O sistema deve controlar o acesso aos recursos conforme o perfil do usuário. |
| RF-007 | O sistema deve impedir o acesso a recursos protegidos sem autenticação válida. |
| RF-008 | O sistema deve registrar tentativas de autenticação para auditoria. |

---

⬆️ [Voltar ao índice](#indice)


<a id="customer"></a>
## 4.2 👤 Customer Service

### Cadastro de Clientes

| Código | Requisito |
|---------|-----------|
| RF-009 | O sistema deve permitir cadastrar clientes. |
| RF-010 | O sistema deve permitir consultar um cliente pelo identificador. |
| RF-011 | O sistema deve permitir consultar um cliente pelo CPF. |
| RF-012 | O sistema deve permitir atualizar os dados cadastrais de um cliente. |
| RF-013 | O sistema deve permitir excluir um cliente. |

### Consulta de Clientes

| Código | Requisito |
|---------|-----------|
| RF-014 | O sistema deve permitir listar clientes. |
| RF-015 | O sistema deve permitir filtrar clientes pelos critérios disponíveis. |
| RF-016 | O sistema deve permitir paginação dos resultados. |
| RF-017 | O sistema deve permitir ordenação dos resultados. |

### Validações

| Código | Requisito |
|---------|-----------|
| RF-018 | O sistema deve validar o CPF informado durante o cadastro. |
| RF-019 | O sistema deve validar o endereço de e-mail informado. |
| RF-020 | O sistema não deve permitir o cadastro de clientes com CPF já existente. |
| RF-021 | O sistema não deve permitir o cadastro de clientes com e-mail já existente. |
| RF-022 | O sistema deve registrar alterações cadastrais para fins de auditoria. |

---

⬆️ [Voltar ao índice](#indice)


<a id="credit"></a>
## 4.3 💳 Credit Analysis Service

### Análise de Crédito

| Código | Requisito |
|---------|-----------|
| RF-023 | O sistema deve permitir solicitar uma análise de crédito para um cliente. |
| RF-024 | O sistema deve permitir consultar uma análise de crédito realizada. |
| RF-025 | O sistema deve permitir simular uma análise de crédito. |

### Score de Crédito

| Código | Requisito |
|---------|-----------|
| RF-026 | O sistema deve calcular um score de crédito utilizando regras internas de negócio. |
| RF-027 | O sistema deve classificar o nível de risco do cliente. |
| RF-028 | O sistema deve calcular um limite de crédito sugerido. |

### Histórico

| Código | Requisito |
|---------|-----------|
| RF-029 | O sistema deve registrar cada análise de crédito realizada. |
| RF-030 | O sistema deve permitir consultar o histórico de análises de crédito. |
| RF-031 | O sistema deve publicar um evento de auditoria após cada análise concluída. |

### Integração

| Código | Requisito |
|---------|-----------|
| RF-032 | O sistema deve utilizar os dados cadastrais do cliente durante a análise de crédito. |
| RF-033 | O sistema deve aplicar regras internas para calcular o resultado da análise. |
| RF-034 | O sistema deve retornar o resultado da análise contendo score, classificação de risco e limite sugerido. |

---

⬆️ [Voltar ao índice](#indice)


<a id="audit"></a>
## 4.4 📝 Audit Service

### Auditoria

| Código | Requisito |
|---------|-----------|
| RF-035 | O sistema deve consumir eventos publicados pelos demais microsserviços. |
| RF-036 | O sistema deve persistir os eventos de auditoria. |
| RF-037 | O sistema deve permitir consultar os registros de auditoria. |
| RF-038 | O sistema deve permitir filtrar auditorias por período. |
| RF-039 | O sistema deve permitir filtrar auditorias por usuário. |
| RF-040 | O sistema deve permitir filtrar auditorias por cliente. |
| RF-041 | O sistema deve permitir filtrar auditorias por tipo de evento. |
| RF-042 | O sistema deve registrar a data e a hora de cada evento de auditoria. |

---

⬆️ [Voltar ao índice](#indice)


<a id="matriz"></a>
# 🔗 Matriz de Rastreabilidade

| Requisitos | Microsserviço |
|------------|---------------|
| RF-001 a RF-008 | `auth-service` |
| RF-009 a RF-022 | `customer-service` |
| RF-023 a RF-034 | `credit-analysis-service` |
| RF-035 a RF-042 | `audit-service` |

---

⬆️ [Voltar ao índice](#indice)


<a id="ref"></a>
# 📚 Referências

- `02-visao-geral.md`
- `04-requisitos-nao-funcionais.md`
- `05-regras-de-negocio.md`
- `06-arquitetura.md`
- `07-modelagem.md`
- `08-api.md`

---

⬆️ [Voltar ao índice](#indice)

