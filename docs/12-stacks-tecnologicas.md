# 13. Stack Tecnológica

## 13.1 Visão Geral

A plataforma Examen Crediti foi desenvolvida utilizando tecnologias amplamente adotadas no mercado para construção de aplicações backend baseadas em microsserviços.

A escolha da stack prioriza:

- Baixo acoplamento.
- Escalabilidade.
- Manutenibilidade.
- Observabilidade.
- Segurança.
- Facilidade de evolução.
- Adoção de padrões consolidados pela comunidade.

Cada tecnologia foi selecionada considerando seu papel dentro da arquitetura da plataforma.

---

## 13.2 Linguagem

### Java 21 (LTS)

A plataforma utiliza Java como linguagem principal de desenvolvimento.

Motivos da escolha:

- Plataforma madura.
- Alto desempenho.
- Grande ecossistema.
- Forte adoção no mercado corporativo.
- Excelente integração com Spring Boot.
- Suporte de Longo Prazo (LTS).

---

## 13.3 Framework

### Spring Boot

Framework responsável pela construção dos microserviços.

Principais benefícios:

- Configuração simplificada.
- Injeção de Dependências.
- Desenvolvimento orientado a convenções.
- Grande ecossistema.
- Integração com diversos módulos Spring.

---

## 13.4 Persistência

### Spring Data JPA

Responsável pela abstração da camada de persistência relacional.

Utilizado para:

- CRUD.
- Paginação.
- Ordenação.
- Consultas customizadas.
- Mapeamento Objeto-Relacional.

---

### Hibernate

Implementação da especificação Jakarta Persistence (JPA).

Responsável por:

- ORM.
- Geração de SQL.
- Controle do ciclo de vida das entidades.
- Cache de primeiro nível.
- Dirty Checking.

---

## 13.5 Bancos de Dados

### PostgreSQL

Banco de dados relacional utilizado para armazenamento das informações transacionais.

Motivos da escolha:

- Confiabilidade.
- Integridade referencial.
- Excelente desempenho.
- Amplo suporte ao SQL.
- Forte adoção no mercado.

---

### MongoDB

Banco de dados NoSQL utilizado para documentos com estrutura flexível.

Utilizado principalmente para:

- Auditoria.
- Notificações.
- Dados orientados a documentos.

---

### Redis

Banco de dados em memória utilizado para otimização de desempenho.

Principais utilizações:

- Cache.
- Redução de consultas repetitivas.
- Armazenamento temporário de dados.

---

## 13.6 Mensageria

### Apache Kafka

Responsável pela comunicação assíncrona entre os microserviços.

Benefícios:

- Desacoplamento.
- Escalabilidade.
- Alta taxa de processamento.
- Processamento orientado a eventos.
- Alta confiabilidade.

## 13.7 Segurança

### Spring Security

Framework responsável pela implementação da camada de segurança da plataforma.

Principais responsabilidades:

- Autenticação.
- Autorização.
- Proteção de endpoints.
- Validação de JWT.
- Controle de acesso baseado em Roles.

---

### JSON Web Token (JWT)

Utilizado para autenticação stateless entre clientes e microserviços.

Benefícios:

- Independência de sessão.
- Escalabilidade.
- Facilidade de integração entre serviços.
- Redução da necessidade de armazenamento de sessões no servidor.

---

### BCrypt

Algoritmo utilizado para proteção das senhas dos usuários.

Principais benefícios:

- Hash unidirecional.
- Salt automático.
- Alta resistência a ataques por força bruta.

---

## 13.8 Observabilidade

### Spring Boot Actuator

Responsável pela disponibilização de informações sobre a saúde da aplicação.

Utilizado para:

- Health Checks.
- Métricas.
- Informações da aplicação.

---

### Micrometer

Biblioteca utilizada para coleta e publicação de métricas da aplicação.

Permite monitorar indicadores operacionais de forma padronizada.

---

### OpenTelemetry

Responsável pelo rastreamento distribuído da aplicação.

Permite acompanhar requisições entre diferentes microserviços por meio de traces e spans.

---

### Prometheus

Responsável pela coleta e armazenamento das métricas publicadas pelos microserviços.

---

### Grafana

Responsável pela visualização das métricas por meio de dashboards operacionais.

---

## 13.9 Testes

### JUnit 5

Framework utilizado para testes automatizados.

Aplicado principalmente em:

- Testes unitários.
- Testes de integração.

---

### Mockito

Biblioteca utilizada para criação de objetos simulados (Mocks).

Permite isolar dependências durante os testes unitários.

---

### Testcontainers

Utilizado para execução de testes utilizando containers reais.

Permite validar integrações com:

- PostgreSQL.
- MongoDB.
- Redis.
- Kafka.

---

## 13.10 Build e Gerenciamento de Dependências

### Gradle

Ferramenta responsável pelo gerenciamento das dependências e automação do processo de build da plataforma.

Principais funcionalidades:

- Gerenciamento de bibliotecas.
- Build automatizado.
- Execução dos testes.
- Empacotamento da aplicação.
- Geração das imagens da aplicação.
- Integração com o pipeline de CI/CD.

A escolha do Gradle foi motivada pelo seu desempenho, flexibilidade e ampla adoção em projetos modernos baseados em Spring Boot.

---

## 13.11 Containerização

### Docker

Utilizado para empacotamento e execução dos microserviços.

Benefícios:

- Padronização dos ambientes.
- Facilidade de implantação.
- Isolamento das aplicações.
- Portabilidade.

---

### Docker Compose

Utilizado durante o desenvolvimento local.

Permite inicializar toda a infraestrutura da plataforma por meio de um único comando.

---

## 13.12 Qualidade de Código

### Checkstyle

Responsável pela padronização do código-fonte conforme convenções estabelecidas pelo projeto.

---

### SpotBugs

Ferramenta utilizada para análise estática e identificação de possíveis defeitos no código.

---

### SonarQube

Responsável pela análise contínua da qualidade do código.

Permite identificar:

- Bugs.
- Vulnerabilidades.
- Code Smells.
- Duplicações.
- Cobertura de testes.

---

## 13.13 Documentação

A documentação da plataforma é mantida em arquivos Markdown versionados juntamente com o código-fonte.

Essa abordagem garante que a documentação evolua de forma sincronizada com a implementação da aplicação.

Os principais documentos incluem:

- Visão Geral.
- Requisitos Funcionais.
- Requisitos Não Funcionais.
- Arquitetura.
- Modelagem.
- APIs.
- Arquitetura de Segurança.
- Arquitetura de Observabilidade.
- Arquitetura de Deploy.
- Estratégia de Testes.
- Stack Tecnológica.
- ADRs (Architecture Decision Records).

---

## 13.14 Infraestrutura

A infraestrutura da plataforma é baseada em componentes independentes.

Os principais componentes são:

- API Gateway.
- Identity Service.
- Customer Service.
- Credit Analysis Service.
- Audit Service.
- Notification Service.
- PostgreSQL.
- MongoDB.
- Redis.
- Apache Kafka.

Todos os componentes podem ser implantados e escalados de forma independente.

---

## 13.15 Resumo da Stack

| Categoria | Tecnologia |
|-----------|------------|
| Linguagem | Java 21 (LTS) |
| Framework | Spring Boot |
| Persistência Relacional | Spring Data JPA + Hibernate |
| Banco Relacional | PostgreSQL |
| Banco NoSQL | MongoDB |
| Cache | Redis |
| Mensageria | Apache Kafka |
| Segurança | Spring Security + JWT + BCrypt |
| Observabilidade | Spring Boot Actuator + Micrometer + OpenTelemetry + Prometheus + Grafana |
| Testes | JUnit 5 + Mockito + Testcontainers |
| Build | Gradle |
| Containers | Docker + Docker Compose |
| Qualidade de Código | Checkstyle + SpotBugs + SonarQube |
| Documentação | Markdown + ADRs |

---

## 13.16 Justificativa Arquitetural

A stack tecnológica foi selecionada considerando os requisitos funcionais e não funcionais da plataforma.

As tecnologias adotadas oferecem:

- Baixo acoplamento.
- Escalabilidade horizontal.
- Facilidade de manutenção.
- Alto desempenho.
- Segurança.
- Observabilidade.
- Automação de testes.
- Facilidade de implantação.
- Amplo suporte da comunidade.
- Forte adoção no mercado corporativo.

Essa combinação fornece uma base sólida para o desenvolvimento de aplicações backend modernas baseadas em microsserviços.

---

## 13.17 Considerações Arquiteturais

- A stack tecnológica foi escolhida priorizando estabilidade, maturidade e ampla adoção no mercado.
- Cada tecnologia possui responsabilidades bem definidas dentro da arquitetura.
- A plataforma utiliza ferramentas consolidadas para persistência, mensageria, segurança, observabilidade e testes.
- A arquitetura favorece evolução contínua, escalabilidade e baixo acoplamento entre os componentes.
- A documentação e as decisões arquiteturais acompanham a evolução do código-fonte.
- A combinação das tecnologias adotadas atende aos requisitos técnicos e operacionais da plataforma Examen Crediti.