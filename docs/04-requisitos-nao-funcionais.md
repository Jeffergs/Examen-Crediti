# 📋 Requisitos Não Funcionais

<a id="objetivo"></a>

## 🎯 Objetivo

Este documento descreve os requisitos não funcionais do **Examen Crediti**, estabelecendo os atributos de qualidade esperados para o sistema.

Os requisitos não funcionais definem como o sistema deve se comportar em aspectos como desempenho, disponibilidade, escalabilidade, segurança, persistência, observabilidade, manutenibilidade, testabilidade e padronização das APIs.

---

<a id="indice"></a>

# 📑 Índice

1. [🎯 Objetivo](#objetivo)
2. [📦 Escopo](#escopo)
3. [🏷️ Categorias dos Requisitos](#categorias)
4. [📋 Requisitos Não Funcionais](#rnf)
   - [4.1 🚀 Desempenho](#desempenho)
   - [4.2 🌐 Disponibilidade](#disponibilidade)
   - [4.3 📈 Escalabilidade](#escalabilidade)
   - [4.4 🔒 Segurança](#seguranca)
   - [4.5 🗄️ Persistência](#persistencia)
   - [4.6 📊 Observabilidade](#observabilidade)
   - [4.7 🔧 Manutenibilidade](#manutenibilidade)
   - [4.8 🧪 Testabilidade](#testabilidade)
   - [4.9 🌍 APIs](#apis)
5. [🔗 Matriz de Rastreabilidade](#matriz)
6. [📚 Referências](#referencias)

---

<a id="escopo"></a>

## 📦 Escopo

Este documento especifica os requisitos relacionados aos atributos de qualidade do sistema.

Os requisitos funcionais são descritos em **03-requisitos-funcionais.md**.

---

⬆️ [Voltar ao índice](#indice)

<a id="categorias"></a>

# 🏷️ Categorias dos Requisitos

| Categoria | Objetivo |
|------------|----------|
| Desempenho | Garantir tempo de resposta adequado. |
| Disponibilidade | Manter os serviços acessíveis. |
| Escalabilidade | Permitir crescimento horizontal da aplicação. |
| Segurança | Proteger dados e acessos ao sistema. |
| Persistência | Garantir integridade e isolamento dos dados. |
| Observabilidade | Permitir monitoramento e rastreamento da aplicação. |
| Manutenibilidade | Facilitar evolução e manutenção do software. |
| Testabilidade | Garantir qualidade por meio de testes automatizados. |
| APIs | Padronizar a comunicação entre clientes e microsserviços. |

---

⬆️ [Voltar ao índice](#indice)

<a id="rnf"></a>

# 📋 Requisitos Não Funcionais

<a id="desempenho"></a>

## 4.1 🚀 Desempenho

| Código | Requisito |
|---------|-----------|
| RNF-001 | As operações de consulta devem responder, em condições normais, em até **500 ms**. |
| RNF-002 | As operações de escrita devem responder, em condições normais, em até **1 segundo**. |
| RNF-003 | O sistema deve suportar múltiplas requisições simultâneas preservando a integridade dos dados. |
| RNF-004 | As consultas que retornem coleções devem utilizar paginação. |

---
⬆️ [Voltar ao índice](#indice)


<a id="disponibilidade"></a>

## 4.2 🌐 Disponibilidade

| Código | Requisito |
|---------|-----------|
| RNF-005 | Os microsserviços devem operar de forma independente sempre que possível. |
| RNF-006 | A indisponibilidade de um microsserviço não deve interromper completamente os demais serviços, exceto quando houver dependência funcional obrigatória. |
| RNF-007 | Cada microsserviço deve disponibilizar um endpoint de verificação de saúde (*Health Check*). |

---
⬆️ [Voltar ao índice](#indice)


<a id="escalabilidade"></a>

## 4.3 📈 Escalabilidade

| Código | Requisito |
|---------|-----------|
| RNF-008 | Os microsserviços devem permitir implantação independente. |
| RNF-009 | Cada microsserviço deve possuir seu próprio banco de dados. |
| RNF-010 | O sistema deve utilizar comunicação assíncrona para desacoplar processos quando aplicável. |

---
⬆️ [Voltar ao índice](#indice)


<a id="seguranca"></a>

## 4.4 🔒 Segurança

| Código | Requisito |
|---------|-----------|
| RNF-011 | Os recursos protegidos devem exigir autenticação baseada em JWT. |
| RNF-012 | As senhas devem ser armazenadas utilizando algoritmos seguros de hash. |
| RNF-013 | As comunicações externas devem utilizar HTTPS em ambiente de produção. |
| RNF-014 | O acesso às funcionalidades deve respeitar os perfis e permissões definidos para cada usuário. |
| RNF-015 | Informações sensíveis não devem ser registradas em logs. |

---
⬆️ [Voltar ao índice](#indice)


<a id="persistencia"></a>

## 4.5 🗄️ Persistência

| Código | Requisito |
|---------|-----------|
| RNF-016 | Os dados transacionais devem ser armazenados em banco de dados relacional. |
| RNF-017 | Os eventos de auditoria devem ser armazenados em banco de dados não relacional. |
| RNF-018 | Cada microsserviço deve ser responsável exclusivamente pelos seus próprios dados. |
| RNF-019 | Um microsserviço não deve acessar diretamente o banco de dados de outro microsserviço. |

---
⬆️ [Voltar ao índice](#indice)


<a id="observabilidade"></a>

## 4.6 📊 Observabilidade

| Código | Requisito |
|---------|-----------|
| RNF-020 | O sistema deve registrar logs estruturados. |
| RNF-021 | O sistema deve disponibilizar métricas para monitoramento. |
| RNF-022 | O sistema deve disponibilizar informações de saúde dos serviços. |
| RNF-023 | O sistema deve permitir rastreamento distribuído entre os microsserviços. |

---
⬆️ [Voltar ao índice](#indice)


<a id="manutenibilidade"></a>

## 4.7 🔧 Manutenibilidade

| Código | Requisito |
|---------|-----------|
| RNF-024 | O projeto deve seguir arquitetura em camadas. |
| RNF-025 | O código deve seguir os princípios SOLID. |
| RNF-026 | O código deve seguir as convenções estabelecidas na documentação do projeto. |
| RNF-027 | Cada microsserviço deve possuir responsabilidade única e bem definida. |
| RNF-028 | A documentação do projeto deve permanecer atualizada durante sua evolução. |

---
⬆️ [Voltar ao índice](#indice)


<a id="testabilidade"></a>

## 4.8 🧪 Testabilidade

| Código | Requisito |
|---------|-----------|
| RNF-029 | O sistema deve possuir testes unitários para as regras de negócio. |
| RNF-030 | O sistema deve possuir testes de integração para APIs e persistência. |
| RNF-031 | Os testes automatizados devem poder ser executados durante o processo de integração contínua. |

---
⬆️ [Voltar ao índice](#indice)


<a id="apis"></a>

## 4.9 🌍 APIs

| Código | Requisito |
|---------|-----------|
| RNF-032 | As APIs devem seguir os princípios da arquitetura REST. |
| RNF-033 | As APIs devem utilizar JSON como formato padrão para troca de mensagens. |
| RNF-034 | As APIs devem possuir documentação utilizando OpenAPI. |
| RNF-035 | As respostas das APIs devem utilizar códigos de status HTTP apropriados. |

---

⬆️ [Voltar ao índice](#indice)

<a id="matriz"></a>

# 🔗 Matriz de Rastreabilidade

| Categoria | Documentação Relacionada |
|------------|--------------------------|
| Requisitos Funcionais | `03-requisitos-funcionais.md` |
| Arquitetura | `06-arquitetura.md` |
| Modelagem | `07-modelagem.md` |
| APIs | `08-api.md` |
| Segurança | `09-seguranca.md` |
| Observabilidade | `10-observabilidade.md` |
| Deploy | `11-deploy.md` |
| Testes | `12-testes.md` |

---

⬆️ [Voltar ao índice](#indice)

<a id="referencias"></a>

# 📚 Referências

- `02-visao-geral.md`
- `03-requisitos-funcionais.md`
- `05-regras-de-negocio.md`
- `06-arquitetura.md`
- `08-api.md`
- `09-seguranca.md`
- `10-observabilidade.md`
- `11-deploy.md`
- `12-testes.md`

---

⬆️ [Voltar ao índice](#indice)