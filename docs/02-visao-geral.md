
# 📖 Visão Geral do Sistema

<a id="indice"></a>

## 📑 Índice

1. [🎯 Objetivo](#1-objetivo)
2. [📌 Sobre o Projeto](#2-sobre-o-projeto)
3. [❓ Problema](#3-problema)
4. [💻 Objetivos do Sistema](#4-objetivos-do-sistema)
5. [📦 Escopo](#5-escopo)
6. [⚙️ Principais Funcionalidades](#6-principais-funcionalidades)
7. [🏛️ Arquitetura de Alto Nível](#7-arquitetura-de-alto-nível)
8. [🗄️ Estratégia de Persistência](#8-estratégia-de-persistência)
9. [📚 Organização da Documentação](#9-organização-da-documentação)

---

## 1. Objetivo

Este documento apresenta uma visão geral do **Examen Crediti**, descrevendo sua finalidade, escopo, objetivos, arquitetura em alto nível e organização da documentação.

Seu propósito é fornecer uma compreensão inicial do sistema antes da leitura dos documentos específicos.

---
⬆️ [Voltar ao índice](#indice)

## 2. Sobre o Projeto

O **Examen Crediti** é um sistema distribuído desenvolvido para simular uma plataforma de análise de crédito baseada em microsserviços.

O projeto foi concebido como uma aplicação de estudo e portfólio, com foco na aplicação de boas práticas de desenvolvimento backend, arquitetura de software e engenharia de software utilizando o ecossistema Java.

Sua arquitetura foi planejada para demonstrar conceitos modernos como APIs REST, comunicação síncrona e assíncrona, mensageria, segurança, observabilidade, conteinerização e persistência poliglota.

---

⬆️ [Voltar ao índice](#indice)

## 3. Problema

Aplicações de análise de crédito precisam processar informações provenientes de diferentes serviços, garantindo escalabilidade, disponibilidade, rastreabilidade e facilidade de manutenção.

O Examen Crediti simula esse cenário por meio de uma arquitetura baseada em microsserviços, permitindo demonstrar como diferentes componentes colaboram para construir um sistema distribuído moderno.

---

⬆️ [Voltar ao índice](#indice)

## 4. Objetivos do Sistema

O projeto possui os seguintes objetivos:

- Desenvolver uma aplicação backend utilizando Java e Spring Boot.
- Aplicar uma arquitetura baseada em microsserviços.
- Desenvolver APIs REST seguindo boas práticas.
- Implementar autenticação e autorização.
- Demonstrar comunicação síncrona e assíncrona entre serviços.
- Utilizar bancos de dados relacionais e não relacionais conforme a responsabilidade de cada serviço.
- Implementar monitoramento e observabilidade.
- Automatizar o processo de implantação utilizando containers.
- Servir como projeto de portfólio profissional.

---

⬆️ [Voltar ao índice](#indice)

## 5. Escopo

### Dentro do Escopo

- Microsserviços
- APIs REST
- Persistência de dados
- Validações
- Tratamento de exceções
- Segurança
- Mensageria
- Observabilidade
- Conteinerização
- Deploy em nuvem

### Fora do Escopo

- Aplicação mobile
- Interface web completa
- Integrações com instituições financeiras reais
- Processamento financeiro
- Consulta a serviços externos de crédito

---

⬆️ [Voltar ao índice](#indice)


## 6. Principais Funcionalidades

As funcionalidades detalhadas serão documentadas em **03-requisitos-funcionais.md**.

Em alto nível, o sistema contempla:

- Gerenciamento de clientes.
- Consulta de crédito.
- Autenticação e autorização.
- Comunicação entre microsserviços.
- Registro de eventos.
- Auditoria das operações.
- Monitoramento da aplicação.

---

⬆️ [Voltar ao índice](#indice)

## 7. Arquitetura de Alto Nível

O Examen Crediti adota uma arquitetura baseada em microsserviços.

Cada serviço possui responsabilidade única, autonomia de implantação e banco de dados próprio.

A comunicação ocorre por meio de APIs REST e eventos publicados em um broker de mensagens.

```text
                           Cliente
                              │
                              ▼
                        API Gateway
                              │
          ┌───────────────────┼───────────────────┐
          ▼                   ▼                   ▼
 Customer Service      Credit Service      Authentication Service
          │                   │                   │
          ▼                   ▼                   ▼
     PostgreSQL         PostgreSQL         PostgreSQL
          │                   │
          └──────────────┬────┘
                         ▼
                    Apache Kafka
                         │
                         ▼
                  Audit Service
                         │
                         ▼
                     MongoDB
```

A arquitetura detalhada será apresentada em **06-arquitetura.md**.

---

⬆️ [Voltar ao índice](#indice)

## 8. Estratégia de Persistência

O Examen Crediti utiliza o conceito de **Persistência Poliglota (Polyglot Persistence)**, adotando diferentes tecnologias de armazenamento conforme a responsabilidade de cada serviço.

### PostgreSQL

Responsável pelos dados transacionais da aplicação, como:

- Clientes
- Usuários
- Consultas de crédito
- Autenticação

### MongoDB

Responsável pelo armazenamento de documentos relacionados à auditoria e eventos do sistema, como:

- Histórico de operações
- Eventos publicados
- Logs de negócio
- Informações de rastreabilidade

Essa abordagem permite utilizar cada banco de dados de acordo com suas características e fortalezas.

---

⬆️ [Voltar ao índice](#indice)

## 9. Organização da Documentação

| Documento | Descrição |
|------------|-----------|
| 00-roadmap.md | Planejamento e evolução do projeto. |
| 01-convencoes-do-projeto.md | Convenções adotadas pelo projeto. |
| 03-requisitos-funcionais.md | Funcionalidades do sistema. |
| 04-requisitos-nao-funcionais.md | Requisitos de qualidade. |
| 05-regras-de-negocio.md | Regras de negócio. |
| 06-arquitetura.md | Arquitetura detalhada da solução. |
| 07-modelagem.md | Modelagem de dados. |
| 08-api.md | Documentação das APIs. |
| 09-seguranca.md | Estratégia de segurança. |
| 10-observabilidade.md | Monitoramento e métricas. |
| 11-deploy.md | Processo de implantação. |
| 12-testes.md | Estratégia de testes. |
| 13-stack-tecnologica.md | Tecnologias utilizadas no projeto. |

---

⬆️ [Voltar ao índice](#indice)

