# 10. Arquitetura de Observabilidade

## 10.1 Visão Geral

A observabilidade é um requisito essencial da plataforma Examen Crediti.

Em uma arquitetura baseada em microsserviços, identificar falhas, acompanhar fluxos distribuídos e monitorar a saúde da aplicação exige mecanismos que permitam compreender o comportamento do sistema em tempo real.

Para isso, a plataforma adota uma estratégia baseada em três pilares:

- Logs.
- Métricas.
- Rastreamento Distribuído (Distributed Tracing).

Esses pilares permitem identificar problemas rapidamente, reduzir o tempo de diagnóstico e fornecer informações para monitoramento operacional.

---

## 10.2 Objetivos

A arquitetura de observabilidade possui os seguintes objetivos:

- Monitorar continuamente a saúde da plataforma.
- Detectar falhas rapidamente.
- Facilitar o diagnóstico de incidentes.
- Permitir rastreamento completo das requisições.
- Disponibilizar métricas para monitoramento operacional.
- Apoiar decisões relacionadas à capacidade da infraestrutura.
- Reduzir o tempo de identificação e resolução de problemas.

---

## 10.3 Arquitetura de Observabilidade

A estratégia de observabilidade integra todos os microserviços da plataforma.

```text
                 +------------------+
                 |     Cliente      |
                 +---------+--------+
                           |
                           ▼
                  API Gateway
                           |
         +-----------------+-----------------+
         |                 |                 |
         ▼                 ▼                 ▼
 Identity Service   Customer Service   Credit Analysis
         |                 |                 |
         +-----------------+-----------------+
                           |
                           ▼
            Audit / Notification Service
                           |
          +----------------+----------------+
          |                |                |
          ▼                ▼                ▼
       Logs           Métricas      Distributed Tracing
          |                |                |
          ▼                ▼                ▼
     OpenTelemetry     Prometheus        Jaeger
                           |
                           ▼
                        Grafana
```

Cada microserviço produz informações de observabilidade de forma independente, permitindo monitoramento distribuído e análise centralizada.

---

## 10.4 Pilares da Observabilidade

A plataforma adota os três pilares clássicos da observabilidade.

### Logs

Registram eventos ocorridos durante a execução da aplicação.

Permitem reconstruir o histórico de operações e identificar falhas.

---

### Métricas

Representam informações quantitativas sobre o comportamento da aplicação.

Permitem acompanhar desempenho, utilização de recursos e tendências.

---

### Rastreamento Distribuído

Permite acompanhar uma mesma requisição durante sua passagem por múltiplos microserviços.

É essencial para identificar gargalos e falhas em arquiteturas distribuídas.

---

## 10.5 Logs

Todos os microserviços produzem logs estruturados.

Os logs possuem informações suficientes para:

- Diagnóstico de falhas.
- Auditoria operacional.
- Investigação de incidentes.
- Monitoramento da aplicação.

Cada registro deve conter informações como:

- Timestamp.
- Nível do log.
- Nome do serviço.
- Correlation ID.
- Classe responsável.
- Mensagem.

Exemplo:

```text
2026-08-21T18:10:02Z
INFO
credit-analysis-service
correlationId=9d5d...
CreditDecisionService
Credit approved successfully.
```

Os logs não devem conter informações sensíveis.

Exemplos de informações proibidas:

- Senhas.
- Tokens JWT.
- Credenciais.
- Dados bancários.
- Informações pessoais desnecessárias.

---

## 10.6 Níveis de Log

Os seguintes níveis de log são utilizados pela plataforma.

| Nível | Finalidade |
|--------|------------|
| TRACE | Diagnóstico extremamente detalhado. |
| DEBUG | Informações úteis durante o desenvolvimento. |
| INFO | Eventos normais da aplicação. |
| WARN | Situações inesperadas sem interrupção da execução. |
| ERROR | Falhas que impediram a conclusão da operação. |

Em ambiente de produção, recomenda-se utilizar INFO como nível padrão.

TRACE e DEBUG devem ser utilizados apenas durante atividades de diagnóstico.

## 10.7 Correlation ID

Cada requisição recebida pela plataforma recebe um identificador único denominado **Correlation ID**.

Esse identificador acompanha toda a execução da requisição, incluindo chamadas entre microserviços e processamento de eventos assíncronos.

O Correlation ID permite correlacionar logs, métricas e rastreamentos distribuídos pertencentes ao mesmo fluxo de negócio.

Exemplo:

```text
Cliente
    │
    ▼
API Gateway
    │
    │ correlationId = a73c5f2b...
    ▼
Customer Service
    │
    ▼
Credit Analysis Service
    │
    ▼
Audit Service
    │
    ▼
Notification Service
```

Todos os microserviços preservam o mesmo Correlation ID durante o processamento da requisição.

---

## 10.8 Rastreamento Distribuído

A plataforma utiliza rastreamento distribuído para acompanhar o fluxo completo das requisições entre os microserviços.

Cada operação gera um Trace, composto por diversos Spans correspondentes às etapas executadas durante o processamento.

Essa abordagem permite:

- Visualizar o caminho percorrido por uma requisição.
- Identificar gargalos de desempenho.
- Localizar falhas entre microserviços.
- Medir o tempo gasto em cada operação.

O rastreamento distribuído é especialmente importante em fluxos assíncronos baseados em eventos.

Exemplo:

```text
Cliente
    │
    ▼
API Gateway
    │
    ▼
Customer Service
    │
    ▼
Kafka
    │
    ▼
Credit Analysis Service
    │
    ▼
Audit Service
    │
    ▼
Notification Service
```

Todo esse fluxo pode ser visualizado como uma única linha temporal.

---

## 10.9 Métricas

Cada microserviço publica métricas operacionais continuamente.

Essas métricas permitem acompanhar o comportamento da aplicação e identificar tendências de utilização.

Exemplos de métricas monitoradas:

- Quantidade de requisições.
- Tempo médio de resposta.
- Taxa de erros.
- Consumo de CPU.
- Consumo de memória.
- Utilização de conexões.
- Eventos processados.
- Mensagens publicadas.
- Mensagens consumidas.

As métricas são utilizadas para construção de dashboards e definição de alertas.

---

## 10.10 Dashboards

As métricas coletadas são consolidadas em dashboards operacionais.

Os dashboards fornecem uma visão centralizada da saúde da plataforma.

Exemplos de informações exibidas:

- Disponibilidade dos microserviços.
- Tempo médio de resposta.
- Taxa de erro.
- Requisições por minuto.
- Consumo de CPU.
- Consumo de memória.
- Eventos publicados.
- Eventos consumidos.
- Estado dos consumidores Kafka.

Essas informações permitem acompanhamento contínuo da operação.

---

## 10.11 Health Checks

Todos os microserviços disponibilizam endpoints de verificação de saúde.

Os Health Checks permitem identificar rapidamente se um serviço está apto para atender requisições.

São monitorados componentes como:

- Aplicação.
- PostgreSQL.
- MongoDB.
- Redis.
- Kafka.

Sempre que possível, a plataforma diferencia:

- Liveness.
- Readiness.

Essa separação permite detectar falhas sem interromper desnecessariamente o funcionamento do sistema.

---

## 10.12 Monitoramento da Mensageria

A arquitetura utiliza comunicação assíncrona por meio do Apache Kafka.

O monitoramento da mensageria acompanha indicadores como:

- Eventos publicados.
- Eventos consumidos.
- Consumer Lag.
- Falhas no consumo.
- Retentativas.
- Tempo médio de processamento.

Essas métricas permitem detectar rapidamente problemas de integração entre microserviços.

---

## 10.13 Monitoramento dos Bancos de Dados

Os bancos de dados também fazem parte da estratégia de observabilidade.

São monitorados indicadores como:

### PostgreSQL

- Número de conexões.
- Consultas por segundo.
- Tempo médio das consultas.
- Utilização de CPU.
- Utilização de memória.
- Espaço em disco.

### MongoDB

- Número de conexões.
- Operações por segundo.
- Tempo médio das consultas.
- Utilização de memória.
- Espaço utilizado.

O acompanhamento dessas métricas auxilia na identificação de gargalos e planejamento de capacidade.

---

## 10.14 Alertas

A plataforma prevê a configuração de alertas automáticos para eventos críticos.

Exemplos:

- Serviço indisponível.
- Taxa elevada de erros.
- Tempo de resposta acima do esperado.
- Alto consumo de CPU.
- Alto consumo de memória.
- Consumer Lag elevado.
- Falha na conexão com bancos de dados.
- Falha na comunicação com o Kafka.

Os alertas permitem atuação proativa antes que falhas impactem os usuários.

---

## 10.15 Incidentes Detectáveis

A estratégia de observabilidade foi projetada para permitir a identificação rápida de problemas operacionais e de desempenho.

A tabela a seguir apresenta alguns exemplos de incidentes que podem ser detectados por meio da combinação de logs, métricas e rastreamento distribuído.

| Incidente | Mecanismos Utilizados |
|-----------|-----------------------|
| Lentidão em um microserviço | Métricas de tempo de resposta, traces e dashboards. |
| Alta taxa de erros HTTP | Métricas, logs e alertas. |
| Indisponibilidade de um serviço | Health Checks e alertas. |
| Falha de comunicação entre microserviços | Logs, Correlation ID e Distributed Tracing. |
| Consumer Lag elevado no Kafka | Métricas do Kafka e dashboards. |
| Falhas recorrentes no consumo de eventos | Logs estruturados, métricas e alertas. |
| Degradação de desempenho | Métricas, traces e dashboards históricos. |
| Gargalos em chamadas distribuídas | Distributed Tracing. |
| Vazamento de memória | Métricas de memória e dashboards. |
| Alto consumo de CPU | Métricas e alertas. |
| Exaustão de conexões com banco de dados | Métricas do PostgreSQL e MongoDB. |
| Falhas de integração com bancos de dados | Logs, Health Checks e métricas. |
| Falhas de autenticação recorrentes | Logs estruturados e métricas de erro. |

A combinação desses mecanismos permite reduzir significativamente o tempo necessário para identificação e diagnóstico de incidentes em produção.

---

## 10.16 Boas Práticas Adotadas

A arquitetura de observabilidade adota práticas amplamente utilizadas em sistemas distribuídos.

Entre elas:

- Logs estruturados.
- Correlation ID em todas as requisições.
- Propagação do Correlation ID entre chamadas síncronas e assíncronas.
- Rastreamento distribuído.
- Coleta contínua de métricas.
- Dashboards centralizados.
- Health Checks para todos os microserviços.
- Monitoramento da mensageria.
- Monitoramento dos bancos de dados.
- Alertas automáticos para eventos críticos.
- Padronização das informações de observabilidade entre todos os microserviços.
- Não registro de informações sensíveis em logs.

Essas práticas aumentam a capacidade de diagnóstico, reduzem o tempo de resposta a incidentes e tornam a plataforma mais confiável e resiliente.

---

## 10.17 Considerações Arquiteturais

- Todos os microserviços produzem logs estruturados.
- Todas as requisições recebem um Correlation ID único.
- O Correlation ID é propagado entre chamadas síncronas e assíncronas.
- O rastreamento distribuído permite acompanhar o fluxo completo das requisições entre os microserviços.
- As métricas são coletadas continuamente e disponibilizadas para monitoramento operacional.
- Dashboards consolidam informações sobre disponibilidade, desempenho e utilização dos serviços.
- Health Checks verificam continuamente a saúde da aplicação e de suas dependências.
- O monitoramento da mensageria permite acompanhar o processamento de eventos distribuídos.
- PostgreSQL, MongoDB, Redis e Kafka fazem parte da estratégia de observabilidade da plataforma.
- Alertas automáticos permitem atuação proativa diante de falhas operacionais.
- Logs nunca devem conter informações sensíveis.
- A observabilidade é tratada como um requisito arquitetural da plataforma, sendo considerada desde o projeto da solução e não apenas durante sua operação.

