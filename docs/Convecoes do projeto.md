# 12. Convenções do Projeto

## 1. Objetivo

Este documento define as convenções adotadas no projeto Examen Crediti para garantir consistência na organização do código, da documentação e do fluxo de desenvolvimento.

Todas as contribuições devem seguir estas convenções, assegurando padronização, legibilidade e facilidade de manutenção ao longo da evolução do projeto.

## 2. Estratégia de Branches

O projeto adota o GitHub Flow como estratégia de desenvolvimento.

### Branch principal

- `main`: contém apenas código estável.

### Convenção de nomes

- `feature/<nome>`
- `fix/<nome>`
- `refactor/<nome>`
- `docs/<nome>`
- `test/<nome>`
- `chore/<nome>`

### Fluxo de trabalho

1. Criar uma branch a partir da `main`.
2. Desenvolver a alteração.
3. Realizar commits seguindo o padrão Conventional Commits.
4. Enviar a branch para o repositório remoto.
5. Abrir um Pull Request.
6. Realizar o merge na `main`.
7. Excluir a branch após o merge.

### Regras

- Nunca realizar alterações diretamente na `main`.
- Cada branch deve representar apenas uma funcionalidade, correção ou tarefa.
- Todo merge deve ser realizado por meio de um Pull Request.

## 3. Convenção de Commits

O projeto adota o padrão **Conventional Commits 1.0.0** para padronizar as mensagens de commit, facilitar a compreensão do histórico de alterações e permitir integração com ferramentas de versionamento e geração automática de changelogs.

### Formato

```text
<tipo>(<escopo>): <descrição>
```

> **Observação:** Embora o escopo seja opcional no padrão Conventional Commits, neste projeto seu uso é obrigatório.

### Tipos de Commit

| Tipo | Descrição |
|------|-----------|
| `feat` | Adiciona uma nova funcionalidade. |
| `fix` | Corrige um bug. |
| `docs` | Cria ou atualiza a documentação. |
| `refactor` | Refatora o código sem alterar seu comportamento. |
| `test` | Adiciona ou modifica testes. |
| `chore` | Realiza tarefas de manutenção, configurações ou atualização de dependências. |
| `build` | Altera o processo de build ou empacotamento da aplicação. |
| `ci` | Altera a configuração de Integração Contínua (CI/CD). |
| `perf` | Melhora o desempenho da aplicação. |
| `revert` | Reverte um commit anterior. |

### Escopos

Os escopos devem representar o componente ou área afetada pela alteração.

Exemplos:

```text
auth
customer
credit
notification
gateway
config
database
security
docker
github
api
docs
readme
architecture
```

### Exemplos

feat(customer): adicionar endpoint de cadastro de clientes

feat(credit): implementar análise de crédito

fix(auth): validar token JWT expirado

refactor(notification): simplificar publicador de eventos

docs(api): adicionar documentação OpenAPI

test(customer): adicionar testes de integração

chore(github): configurar templates de issues

build(docker): atualizar Docker Compose

ci(github): adicionar workflow do GitHub Actions

perf(credit): otimizar cálculo do score de crédito

### Regras

- Utilizar mensagens de commit em **português**.
- Utilizar o formato `<tipo>(<escopo>): <descrição>`.
- O **escopo é obrigatório**.
- Escrever a descrição no **imperativo**.
- Iniciar a descrição com **letra minúscula**.
- Não finalizar a descrição com ponto (`.`).
- Cada commit deve representar **uma única alteração lógica e coesa**.

## 4. Versionamento

O projeto adota o padrão **Semantic Versioning (SemVer) 2.0.0** para identificar e controlar a evolução das versões da aplicação.

### Formato

```text
MAJOR.MINOR.PATCH
```

Exemplo:

```text
1.0.0
```

### Significado

| Versão | Quando incrementar |
|---------|--------------------|
| **MAJOR** | Alterações incompatíveis com versões anteriores (*breaking changes*). |
| **MINOR** | Adição de novas funcionalidades mantendo compatibilidade. |
| **PATCH** | Correções de bugs e pequenas melhorias sem alteração de funcionalidades. |

### Estratégia de Versionamento

O **Examen Crediti** é composto por múltiplos microsserviços distribuídos em repositórios independentes.

Cada microsserviço possui seu **próprio versionamento**, evoluindo de forma independente dos demais.

Isso significa que uma nova versão de um serviço **não exige** a criação de uma nova versão para os outros serviços.

### Exemplos

```text
api-gateway          1.2.0
auth-service         1.0.3
customer-service     1.4.1
credit-service       2.0.0
notification-service 1.1.0
```

Caso apenas o `customer-service` receba uma nova funcionalidade, somente sua versão será incrementada.

Exemplo:

```text
Antes

customer-service     1.4.1
credit-service       2.0.0
notification-service 1.1.0

Depois

customer-service     1.5.0
credit-service       2.0.0
notification-service 1.1.0
```

### Regras

- Cada microsserviço possui seu próprio ciclo de versionamento.
- O versionamento de um microsserviço é independente dos demais.
- Toda nova versão deve seguir o padrão **Semantic Versioning (SemVer)**.
- Durante o desenvolvimento inicial, as versões devem permanecer na série `0.x.x`.
- A versão `1.0.0` representa a primeira versão estável de cada microsserviço.

## 5. Convenções de Nomenclatura

Este projeto adota convenções de nomenclatura para garantir consistência, legibilidade e padronização entre todos os microsserviços e artefatos.

### Repositórios

Os repositórios devem utilizar apenas letras minúsculas e palavras separadas por hífen (`-`).

Exemplos:
```text
examen-crediti
examen-crediti-api-gateway
examen-crediti-auth-service
examen-crediti-customer-service
examen-crediti-credit-service
examen-crediti-notification-service
```

### Branches

As branches devem seguir a convenção definida na seção **Estratégia de Branches**.

Exemplos:

```text
feature/customer-registration
fix/jwt-validation
refactor/credit-analysis
docs/project-conventions
test/customer-service
chore/update-dependencies
```

### Pacotes Java

Os pacotes devem utilizar apenas letras minúsculas.

Pacote base:

```text
br.com.examencrediti
```

Subpacotes:

```text
controller
service
repository
entity
dto
mapper
config
exception
security
client
event
```

### Classes

Os nomes das classes devem utilizar o padrão **PascalCase**.

Exemplos:
```text
CustomerController
CustomerService
CreditAnalysisService
NotificationProducer
JwtAuthenticationFilter
```

### Interfaces

Interfaces também devem utilizar **PascalCase**, sem prefixos como `I`.

Exemplos:

```text
CustomerRepository
NotificationService
CreditAnalyzer
```

### Métodos

Os métodos devem utilizar o padrão **camelCase**.

Exemplos:
```text
findCustomerById()
calculateCreditScore()
sendNotification()
generateToken()
```

### Variáveis e Atributos

Variáveis e atributos devem utilizar o padrão **camelCase**.

Exemplos:

```text
customerId
creditScore
createdAt
expirationDate
```

### Constantes

Constantes devem utilizar letras maiúsculas com palavras separadas por `_`.

Exemplos:

```text
MAX_SCORE
DEFAULT_TIMEOUT
JWT_EXPIRATION_TIME
```

### Endpoints REST

Os endpoints devem utilizar letras minúsculas e palavras separadas por hífen (`-`).

Exemplos:

```text
/api/customers
/api/credit-analysis
/api/notifications
```

### Banco de Dados

Tabelas e colunas devem utilizar **snake_case**.

Exemplos:

```text
customer
credit_analysis
notification

customer_id
created_at
updated_at
credit_score
```

### Arquivos

Arquivos de documentação devem utilizar letras minúsculas e hífen (`-`).

Exemplos:

```text
01-visao-geral.md
05-arquitetura.md
12-convencoes-do-projeto.md
```

## 6. Idioma do Projeto

O Examen Crediti adota uma política de idiomas para manter a consistência entre a documentação e os artefatos técnicos do projeto.

| Elemento | Idioma |
|----------|---------|
| Documentação (`docs/`) | Português |
| README | Português |
| Issues | Português |
| Pull Requests | Português |
| Milestones | Português |
| GitHub Projects | Português |
| Comentários em código | Português* |
| Commits | Português |
| Classes | Inglês |
| Interfaces | Inglês |
| Métodos | Inglês |
| Variáveis | Inglês |
| Constantes | Inglês |
| Pacotes | Inglês |
| Microsserviços | Inglês |
| APIs REST | Inglês |
| Endpoints | Inglês |
| Banco de dados | Inglês |
| Eventos Kafka | Inglês |
| Mensagens de erro da API | Inglês |

> **\*** Comentários no código devem ser evitados sempre que possível. Quando forem realmente necessários para explicar uma regra de negócio complexa ou uma decisão técnica importante, devem ser escritos em português.

### Regras

- Toda a documentação do projeto deve ser escrita em português.
- Todo o código-fonte deve ser escrito em inglês.
- Os identificadores técnicos (classes, métodos, variáveis, pacotes, endpoints e banco de dados) devem ser escritos em inglês.
- As mensagens de commit devem seguir o padrão **Conventional Commits**, utilizando os tipos (`feat`, `fix`, `docs`, etc.) e escopos em inglês, com a descrição em português.
- Comentários no código devem ser utilizados apenas quando realmente agregarem valor ao entendimento da implementação.


## 7. Estrutura dos Microsserviços

Todos os microsserviços do projeto devem seguir uma estrutura de diretórios padronizada, garantindo consistência, organização e facilidade de manutenção.

### Estrutura Base

```text
src
├── main
│   ├── java
│   │   └── br
│   │       └── com
│   │           └── examencrediti
│   │               ├── config
│   │               ├── controller
│   │               ├── dto
│   │               ├── entity
│   │               ├── exception
│   │               ├── mapper
│   │               ├── repository
│   │               ├── service
│   │               └── ExamenCreditiApplication.java
│   │
│   └── resources
│       ├── application.yml
│       └── db
│           └── migration
│
└── test
    └── java
```

### Estrutura da Raiz do Projeto

Todos os microsserviços devem manter uma estrutura semelhante na raiz do repositório.

```text.
├── src
├── .github
├── .gitignore
├── Dockerfile
├── docker-compose.yml
├── pom.xml
├── README.md
└── LICENSE
```

### Responsabilidade dos Pacotes

| Pacote | Responsabilidade |
|---------|------------------|
| `config` | Configurações da aplicação e beans do Spring. |
| `controller` | Exposição dos endpoints REST. |
| `dto` | Objetos de transferência de dados. |
| `entity` | Entidades do domínio. |
| `exception` | Tratamento de exceções. |
| `mapper` | Conversão entre entidades e DTOs. |
| `repository` | Acesso aos dados. |
| `service` | Regras de negócio. |

### Regras

- Todos os microsserviços devem seguir esta estrutura como padrão.
- Novos pacotes podem ser adicionados quando houver necessidade, desde que possuam uma responsabilidade bem definida.
- A organização dos pacotes deve respeitar os princípios de coesão e responsabilidade única.
- A estrutura deve permanecer consistente entre todos os microsserviços do projeto.



## 8. Padrões de Documentação

Toda a documentação do projeto deve seguir um padrão único de organização, escrita e formatação, facilitando a leitura e a manutenção ao longo do ciclo de vida do sistema.

### Estrutura

A documentação deve ser organizada no diretório `docs/`, utilizando arquivos Markdown (`.md`) numerados para indicar a ordem de leitura.

Exemplo:
```text
docs/
├── 00-roadmap.md
├── 01-visao-geral.md
├── 02-requisitos-funcionais.md
├── 03-requisitos-nao-funcionais.md
├── 04-regras-de-negocio.md
├── 05-arquitetura.md
├── 06-modelagem.md
├── 07-api.md
├── 08-seguranca.md
├── 09-observabilidade.md
├── 10-deploy.md
├── 11-testes.md
└── 12-convencoes-do-projeto.md
```

### Formatação

A documentação deve utilizar:

- Títulos hierárquicos (`#`, `##`, `###`).
- Tabelas para informações comparativas.
- Listas para regras, requisitos e procedimentos.
- Blocos de código para exemplos técnicos.
- Diagramas quando contribuírem para o entendimento da arquitetura ou dos fluxos do sistema.

### Escrita

A documentação deve ser:

- Clara e objetiva.
- Consistente em todo o projeto.
- Escrita em português.
- Atualizada sempre que houver mudanças relevantes no sistema.

Sempre que possível, deve descrever **o motivo** de uma decisão técnica, e não apenas **como** ela foi implementada.

### Exemplos

Exemplos de código devem:

- Ser curtos e objetivos.
- Representar cenários reais do projeto.
- Permanecer atualizados com a implementação.

### Regras

- Toda alteração significativa no projeto deve ser refletida na documentação correspondente.
- Não devem existir documentos duplicados abordando o mesmo assunto.
- Informações obsoletas devem ser corrigidas ou removidas.
- Novos documentos devem seguir o padrão de nomenclatura definido neste guia.
- O README de cada microsserviço deve permanecer alinhado com sua implementação e documentação técnica.


## 9. Convenções para Pull Requests

Toda alteração na branch `main` deve ocorrer exclusivamente por meio de **Pull Request (PR)**.

O objetivo é garantir revisão de código, rastreabilidade das alterações e manutenção da qualidade do projeto.

### Quando abrir um Pull Request

Um Pull Request deve ser aberto quando:

- Uma funcionalidade for concluída.
- Um bug for corrigido.
- Uma refatoração estiver finalizada.
- A documentação for atualizada.
- Uma melhoria técnica estiver pronta para revisão.

### Título

O título do Pull Request deve seguir o mesmo padrão utilizado nos commits.

Formato:

```text
<tipo>(<escopo>): <descrição>
```

Exemplos:

```text
feat(customer): adicionar endpoint de cadastro de clientes

fix(auth): corrigir validação do JWT

docs(api): atualizar documentação da API
```

### Descrição

A descrição do Pull Request deve informar:

- Objetivo da alteração.
- Principais modificações realizadas.
- Impactos em outros componentes (quando houver).
- Issue relacionada (quando existir).

Exemplo:

```text
## Objetivo

Implementar o cadastro de clientes.

## Alterações

- Criado CustomerController
- Criado CustomerService
- Criado CustomerRepository
- Adicionados testes de integração

## Issue

Closes #15
```

### Critérios para Merge

Antes do merge, o Pull Request deve atender aos seguintes critérios:

- Código compilando sem erros.
- Testes automatizados executados com sucesso.
- Padrões de código respeitados.
- Convenções do projeto seguidas.
- Documentação atualizada, quando necessário.
- Aprovação da revisão de código (quando aplicável).

### Regras

- Não realizar merge diretamente na branch `main`.
- Cada Pull Request deve representar uma única alteração lógica.
- Pull Requests muito grandes devem ser evitados.
- O histórico de commits deve permanecer organizado e compreensível.
- Após o merge, a branch utilizada deve ser removida.


## 10. Convenções para Issues

Todas as funcionalidades, correções, melhorias e tarefas do projeto devem ser registradas por meio de **Issues**.

O objetivo é garantir rastreabilidade, organização e transparência durante o desenvolvimento.

### Quando criar uma Issue

Uma Issue deve ser criada para:

- Implementação de novas funcionalidades.
- Correção de bugs.
- Melhorias técnicas.
- Refatorações.
- Atualizações de documentação.
- Criação de testes.
- Estudos ou pesquisas técnicas relevantes para o projeto.

### Título

O título da Issue deve ser curto, claro e objetivo.

Exemplos:

```text
Implementar cadastro de clientes

Adicionar autenticação JWT

Criar documentação da API

Corrigir cálculo do score de crédito
```

### Descrição

A descrição da Issue deve conter, sempre que possível:

- Objetivo.
- Contexto.
- Requisitos.
- Critérios de aceite.
- Informações adicionais relevantes.

Exemplo:

```text
## Objetivo

Permitir o cadastro de novos clientes.

## Requisitos

- Criar endpoint REST.
- Validar CPF.
- Persistir dados no banco.

## Critérios de Aceite

- Cadastro realizado com sucesso.
- CPF duplicado deve retornar erro.
- Testes automatizados implementados.
```

### Organização

Sempre que aplicável, uma Issue deve possuir:

- Tipo (Label).
- Área (Label).
- Prioridade (Label).
- Milestone.
- Tamanho da implementação (`XS`, `S`, `M`, `L` ou `XL`).

### Relacionamento com Pull Requests

Sempre que possível, um Pull Request deve estar vinculado à Issue correspondente.

Exemplo:

```text
Closes #15
```

Após o merge do Pull Request, a Issue será encerrada automaticamente pelo GitHub.

### Regras

- Cada Issue deve representar uma única demanda.
- O título deve descrever claramente o objetivo da tarefa.
- A descrição deve conter informações suficientes para implementação.
- Labels, Milestone e Tamanho devem ser definidos sempre que aplicável.
- Issues concluídas devem ser encerradas após o merge do Pull Request correspondente.
- Todas as novas Issues devem ser criadas utilizando os templates disponibilizados pelo repositório.



## 11. Referências

As convenções adotadas neste projeto são baseadas nas seguintes especificações, guias e documentações oficiais:

| Referência | Descrição | Link |
|------------|-----------|------|
| Semantic Versioning (SemVer) | Padrão para versionamento de software. | [semver.org](https://semver.org/) |
| Conventional Commits | Convenção para padronização de mensagens de commit. | [conventionalcommits.org](https://www.conventionalcommits.org/) |
| GitHub Flow | Fluxo de trabalho baseado em branches e Pull Requests. | [docs.github.com](https://docs.github.com/en/get-started/using-github/github-flow) |
| Markdown Guide | Guia de sintaxe Markdown utilizado na documentação. | [markdownguide.org](https://www.markdownguide.org/) |
| Java Language Specification | Especificação oficial da linguagem Java. | [docs.oracle.com](https://docs.oracle.com/javase/specs/) |
| Spring Framework Documentation | Documentação oficial do Spring Framework. | [docs.spring.io](https://docs.spring.io/spring-framework/reference/) |
| Spring Boot Reference Documentation | Documentação oficial do Spring Boot. | [docs.spring.io](https://docs.spring.io/spring-boot/documentation.html) |
| Spring Security Reference | Documentação oficial do Spring Security. | [docs.spring.io](https://docs.spring.io/spring-security/reference/) |
| Spring Cloud Reference | Documentação oficial do Spring Cloud. | [docs.spring.io](https://docs.spring.io/spring-cloud/docs/current/reference/html/) |
| Jakarta Persistence (JPA) Specification | Especificação oficial da Jakarta Persistence (JPA). | [jakarta.ee](https://jakarta.ee/specifications/persistence/) |
| PostgreSQL Documentation | Documentação oficial do PostgreSQL. | [postgresql.org](https://www.postgresql.org/docs/) |
| MongoDB Documentation | Documentação oficial do MongoDB. | [mongodb.com](https://www.mongodb.com/docs/) |
| Apache Kafka Documentation | Documentação oficial do Apache Kafka. | [kafka.apache.org](https://kafka.apache.org/documentation/) |
| Docker Documentation | Documentação oficial do Docker. | [docs.docker.com](https://docs.docker.com/) |
| OpenAPI Specification | Especificação para documentação de APIs REST. | [spec.openapis.org](https://spec.openapis.org/oas/latest.html) |
| OWASP Top 10 | Principais riscos de segurança para aplicações web. | [owasp.org](https://owasp.org/www-project-top-ten/) |
| REST Architectural Style | Princípios da arquitetura REST descritos por Roy Fielding. | [ics.uci.edu](https://ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm) |
| JUnit 5 User Guide | Guia oficial do JUnit 5 para testes automatizados. | [junit.org](https://junit.org/junit5/docs/current/user-guide/) |
| Mockito Documentation | Documentação oficial do Mockito. | [site.mockito.org](https://site.mockito.org/) |
| Testcontainers | Framework para testes de integração utilizando containers. | [testcontainers.com](https://testcontainers.com/) |
| Micrometer | Biblioteca para coleta de métricas em aplicações Java. | [docs.micrometer.io](https://docs.micrometer.io/) |
| OpenTelemetry | Framework para observabilidade (métricas, logs e traces). | [opentelemetry.io](https://opentelemetry.io/docs/) |
| Prometheus | Plataforma de monitoramento e coleta de métricas. | [prometheus.io](https://prometheus.io/docs/) |
| Grafana | Plataforma para visualização de métricas e observabilidade. | [grafana.com](https://grafana.com/docs/) |

Este documento deve ser revisado sempre que novas convenções, padrões ou tecnologias forem adotados pelo projeto.


