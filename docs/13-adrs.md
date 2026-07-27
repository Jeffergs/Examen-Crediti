# Architecture Decision Records (ADRs)

Os Architecture Decision Records (ADRs) registram as principais decisões arquiteturais tomadas durante o desenvolvimento da plataforma Examen Crediti.

Cada ADR documenta:

- Contexto da decisão.
- Problema identificado.
- Alternativas consideradas.
- Decisão adotada.
- Consequências da decisão.

O objetivo é preservar o histórico arquitetural da plataforma e facilitar sua evolução ao longo do tempo.

# ADR-001 — Arquitetura Baseada em Microsserviços

## Status

Aceito

---

## Contexto

A plataforma Examen Crediti possui domínios de negócio distintos, como autenticação, cadastro de clientes, análise de crédito, auditoria e notificações.

Era necessário definir uma arquitetura que favorecesse evolução contínua, escalabilidade e baixo acoplamento entre esses domínios.

---

## Alternativas Consideradas

- Monólito.
- Monólito Modular.
- Microsserviços.

---

## Decisão

Foi adotada uma arquitetura baseada em microsserviços.

Cada domínio de negócio é implementado como um serviço independente, com responsabilidades bem definidas e ciclo de vida próprio.

---

## Consequências

### Positivas

- Baixo acoplamento.
- Escalabilidade independente.
- Deploy independente.
- Evolução contínua.
- Melhor separação de responsabilidades.

### Negativas

- Maior complexidade operacional.
- Necessidade de observabilidade distribuída.
- Comunicação entre serviços.
- Maior esforço de monitoramento.

# ADR-002 — PostgreSQL para Dados Transacionais

## Status

Aceito

---

## Contexto

A plataforma Examen Crediti necessita armazenar dados transacionais relacionados a clientes, rendas, solicitações de crédito, análises e decisões de crédito.

Esses dados possuem relacionamentos bem definidos, exigem consistência transacional e devem preservar sua integridade ao longo do tempo.

---

## Alternativas Consideradas

- PostgreSQL
- MySQL
- MongoDB

---

## Decisão

Foi adotado o PostgreSQL como banco de dados relacional principal da plataforma.

Sua utilização concentra-se nos dados transacionais e relacionais do domínio de negócio.

---

## Consequências

### Positivas

- Suporte completo ao padrão SQL.
- Transações ACID.
- Integridade referencial.
- Excelente desempenho.
- Grande maturidade.
- Forte adoção no mercado.

### Negativas

- Menor flexibilidade para documentos semiestruturados.
- Escalabilidade horizontal mais complexa quando comparada a bancos NoSQL.

# ADR-003 — MongoDB para Auditoria e Notificações

## Status

Aceito

---

## Contexto

Os domínios de Auditoria e Notificações armazenam documentos cujo formato pode evoluir ao longo do tempo.

Além disso, esses domínios não necessitam de relacionamentos complexos entre entidades.

---

## Alternativas Consideradas

- PostgreSQL
- MongoDB

---

## Decisão

Foi adotado o MongoDB para armazenar documentos relacionados à auditoria e às notificações.

Essa escolha oferece maior flexibilidade para evolução do modelo de dados sem necessidade de alterações frequentes no esquema.

---

## Consequências

### Positivas

- Modelo orientado a documentos.
- Evolução flexível do schema.
- Boa performance para leitura e escrita de documentos.
- Baixo acoplamento ao modelo relacional.

### Negativas

- Não oferece os mesmos recursos relacionais de um banco SQL.
- Maior atenção à modelagem para evitar duplicação excessiva de dados.

# ADR-004 — Apache Kafka como Barramento de Eventos

## Status

Aceito

---

## Contexto

Os microserviços da plataforma precisam comunicar-se de forma assíncrona, reduzindo dependências diretas entre os componentes.

Também é necessário suportar processamento orientado a eventos.

---

## Alternativas Consideradas

- Comunicação REST síncrona.
- RabbitMQ.
- Apache Kafka.

---

## Decisão

Foi adotado o Apache Kafka como barramento de eventos da plataforma.

Os eventos publicados permitem integração assíncrona entre os microserviços, reduzindo o acoplamento e favorecendo escalabilidade.

---

## Consequências

### Positivas

- Comunicação desacoplada.
- Alta escalabilidade.
- Elevada taxa de processamento.
- Processamento orientado a eventos.
- Facilidade para inclusão de novos consumidores.

### Negativas

- Maior complexidade operacional.
- Necessidade de monitoramento dos consumidores.
- Exige controle de idempotência dos consumidores.

# ADR-005 — Não Utilização de HATEOAS

## Status

Aceito

---

## Contexto

A plataforma disponibiliza APIs REST destinadas principalmente à integração entre aplicações e microserviços.

Era necessário avaliar a utilização de HATEOAS na navegação dos recursos.

---

## Alternativas Consideradas

- REST com HATEOAS.
- REST sem HATEOAS.

---

## Decisão

As APIs REST da plataforma não utilizam HATEOAS.

Os endpoints são documentados por meio de contratos claros e versionados, tornando desnecessária a navegação baseada em hyperlinks.

---

## Consequências

### Positivas

- APIs mais simples.
- Menor volume das respostas.
- Implementação mais direta.
- Documentação mais objetiva.

### Negativas

- Menor descoberta dinâmica dos recursos.
- Navegação depende da documentação da API.

# ADR-006 — Autenticação Centralizada com JWT

## Status

Aceito

---

## Contexto

Todos os microserviços precisam validar a identidade dos usuários de forma consistente.

Era necessário evitar duplicação da lógica de autenticação.

---

## Alternativas Consideradas

- Autenticação em cada microserviço.
- Sessões HTTP.
- Identity Service com JWT.

---

## Decisão

Foi adotado um Identity Service responsável pela autenticação dos usuários e emissão de tokens JWT.

Os demais microserviços atuam como Resource Servers, validando localmente os tokens recebidos.

---

## Consequências

### Positivas

- Autenticação centralizada.
- Arquitetura stateless.
- Escalabilidade.
- Menor acoplamento.
- Reutilização da infraestrutura de segurança.

### Negativas

- Dependência inicial do Identity Service para emissão dos tokens.
- Maior cuidado na gestão das chaves de assinatura.

# ADR-007 — Redis para Cache

## Status

Aceito

---

## Contexto

Algumas informações da plataforma são consultadas com frequência e podem ser reutilizadas durante um período limitado.

Era necessário reduzir consultas repetitivas aos bancos de dados.

---

## Alternativas Consideradas

- Sem cache.
- Cache em memória da aplicação.
- Redis.

---

## Decisão

Foi adotado o Redis como mecanismo de cache distribuído da plataforma.

---

## Consequências

### Positivas

- Redução da carga sobre os bancos de dados.
- Menor tempo de resposta.
- Cache compartilhado entre instâncias.
- Alto desempenho.

### Negativas

- Necessidade de gerenciamento da expiração dos dados.
- Possibilidade de inconsistências temporárias entre cache e banco de dados.

# ADR-008 — Gradle como Ferramenta de Build

## Status

Aceito

---

## Contexto

A plataforma necessita de uma ferramenta para gerenciamento de dependências, automação do processo de build e integração com o pipeline de CI/CD.

Era necessário escolher uma solução compatível com o ecossistema Spring Boot e adequada a uma arquitetura de microsserviços.

---

## Alternativas Consideradas

- Maven.
- Gradle.

---

## Decisão

Foi adotado o Gradle como ferramenta de build da plataforma.

A escolha foi motivada por seu desempenho, flexibilidade, capacidade de automação e ampla adoção em projetos modernos baseados em Spring Boot.

---

## Consequências

### Positivas

- Builds mais rápidos.
- Maior flexibilidade de configuração.
- Excelente integração com Spring Boot.
- Automação simplificada do pipeline de CI/CD.

### Negativas

- Curva de aprendizado maior para equipes acostumadas ao Maven.
- Scripts de build podem se tornar complexos se não forem bem organizados.

# ADR-009 — Observabilidade com OpenTelemetry

## Status

Aceito

---

## Contexto

A arquitetura baseada em microsserviços exige mecanismos para acompanhar o fluxo das requisições entre os diferentes serviços.

---

## Alternativas Consideradas

- Apenas logs.
- Logs e métricas.
- OpenTelemetry com rastreamento distribuído.

---

## Decisão

Foi adotado o OpenTelemetry como padrão para rastreamento distribuído da plataforma, integrado às métricas e aos logs estruturados.

---

## Consequências

### Positivas

- Visibilidade ponta a ponta das requisições.
- Diagnóstico mais rápido de incidentes.
- Identificação de gargalos.
- Integração com ferramentas de observabilidade.

### Negativas

- Maior volume de dados de telemetria.
- Necessidade de infraestrutura para armazenamento e visualização dos traces.

# ADR-010 — Docker como Padrão de Containerização

## Status

Aceito

---

## Contexto

Os microserviços da plataforma devem ser executados de forma consistente em diferentes ambientes.

Era necessário padronizar o empacotamento e a execução da aplicação.

---

## Alternativas Consideradas

- Execução diretamente no sistema operacional.
- Máquinas virtuais.
- Containers Docker.

---

## Decisão

Foi adotado o Docker como padrão de containerização para todos os componentes da plataforma.

---

## Consequências

### Positivas

- Padronização entre ambientes.
- Facilidade de implantação.
- Portabilidade.
- Isolamento dos serviços.
- Integração com pipelines de CI/CD.

### Negativas

- Necessidade de gerenciamento das imagens.
- Curva de aprendizado para operação da infraestrutura baseada em containers.

