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

```text
.
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
```
