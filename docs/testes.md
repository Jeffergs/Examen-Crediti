# 12. Estratégia de Testes

## 12.1 Visão Geral

A estratégia de testes define como a qualidade da plataforma Examen Crediti é verificada durante seu desenvolvimento e evolução.

Cada microserviço é responsável por validar seu próprio comportamento por meio de testes automatizados executados continuamente durante o processo de integração.

A combinação de diferentes níveis de testes permite identificar defeitos precocemente, reduzir regressões e aumentar a confiabilidade da plataforma.

---

## 12.2 Objetivos

A estratégia de testes possui os seguintes objetivos:

- Garantir o funcionamento correto das regras de negócio.
- Validar o comportamento das APIs.
- Verificar a integração entre os componentes.
- Detectar regressões.
- Reduzir falhas em produção.
- Apoiar a evolução contínua da plataforma.
- Automatizar a validação da aplicação durante o pipeline de CI/CD.

---

## 12.3 Pirâmide de Testes

A plataforma adota a estratégia da Pirâmide de Testes.

```text
                 ▲
                 │
          End-to-End Tests
        ---------------------
          Integration Tests
      -------------------------
            Unit Tests
```

Essa abordagem prioriza grande quantidade de testes rápidos e menor quantidade de testes mais custosos.

---

## 12.4 Estratégia Geral

Cada microserviço possui sua própria suíte de testes.

Os testes são executados automaticamente durante o pipeline de integração contínua.

A validação da plataforma ocorre em diferentes níveis:

- Testes Unitários.
- Testes de Integração.
- Testes End-to-End.

Cada nível possui responsabilidades específicas e complementares.

## 12.5 Testes Unitários

Os testes unitários verificam o comportamento isolado de classes, métodos e regras de negócio.

Seu objetivo é garantir que cada unidade da aplicação funcione corretamente sem depender de componentes externos.

Nesse nível, dependências como bancos de dados, APIs externas e mensageria são simuladas (mockadas).

Os testes unitários concentram-se principalmente em:

- Entidades de domínio.
- Value Objects.
- Domain Services.
- Casos de uso.
- Policies.
- Validadores.
- Mapeamentos.

Os testes unitários devem ser rápidos, independentes e determinísticos.

---

## 12.6 Testes de Integração

Os testes de integração verificam a comunicação entre componentes reais da aplicação.

Seu objetivo é garantir que a integração entre as camadas funcione corretamente.

Entre os componentes validados estão:

- Controllers.
- Services.
- Repositories.
- PostgreSQL.
- MongoDB.
- Redis.
- Kafka.

Sempre que possível, esses testes devem utilizar ambientes isolados para garantir resultados consistentes.

---

## 12.7 Testes End-to-End

Os testes End-to-End validam o funcionamento completo dos principais fluxos da plataforma.

Esses testes simulam o comportamento de um usuário utilizando a aplicação como um todo.

Exemplos de fluxos:

- Cadastro de cliente.
- Atualização de dados cadastrais.
- Registro de renda.
- Solicitação de crédito.
- Análise de crédito.
- Decisão de crédito.
- Geração de auditoria.
- Geração de notificações.

O objetivo é verificar se todos os componentes trabalham corretamente em conjunto.

---

## 12.8 Testes das APIs

As APIs REST são validadas por meio de testes automatizados.

Esses testes verificam:

- Códigos HTTP.
- Estrutura das respostas.
- Validação das entradas.
- Tratamento de erros.
- Paginação.
- Ordenação.
- Filtros.
- Regras de autorização.
- Serialização dos objetos.

Também são validados cenários de sucesso e falha.

---

## 12.9 Testes de Mensageria

Como a plataforma utiliza comunicação assíncrona baseada em eventos, a mensageria também faz parte da estratégia de testes.

São verificados aspectos como:

- Publicação de eventos.
- Consumo de eventos.
- Serialização.
- Desserialização.
- Processamento correto.
- Idempotência.
- Tratamento de falhas.
- Retentativas quando aplicável.

Esses testes garantem que os microserviços permaneçam desacoplados e interoperáveis.

---

## 12.10 Testes de Persistência

A camada de persistência é validada por testes específicos.

São verificados:

- Operações de CRUD.
- Consultas customizadas.
- Paginação.
- Ordenação.
- Relacionamentos.
- Restrições de integridade.
- Mapeamentos JPA.
- Índices e consultas críticas.

O objetivo é garantir que o comportamento esperado seja preservado independentemente da evolução do modelo de dados.

---

## 12.11 Testes de Segurança

Os principais mecanismos de segurança também são validados por testes automatizados.

Entre eles:

- Autenticação.
- Autorização.
- Acesso baseado em Roles.
- Expiração de tokens JWT.
- Validação de credenciais.
- Proteção de endpoints.
- Respostas para acessos não autorizados.
- Validação de entradas maliciosas.

Esses testes ajudam a evitar regressões relacionadas à segurança da plataforma.

---

## 12.12 Testes de Resiliência

A arquitetura prevê testes voltados ao comportamento da aplicação diante de falhas.

São avaliados cenários como:

- Indisponibilidade de serviços.
- Timeout em integrações.
- Falha de comunicação entre microserviços.
- Circuit Breaker aberto.
- Retry em operações externas.
- Falha na publicação ou consumo de eventos.

Esses testes verificam se a plataforma continua operando de forma previsível mesmo diante de falhas parciais.

## 12.13 Cobertura de Testes

A cobertura de testes representa o percentual do código executado durante a execução da suíte de testes.

Embora a cobertura seja um indicador importante, ela não deve ser utilizada como única métrica de qualidade.

A prioridade da plataforma é garantir que os principais comportamentos do negócio estejam protegidos por testes automatizados.

Especial atenção deve ser dada a:

- Regras de negócio.
- Casos de uso.
- APIs públicas.
- Processamento de eventos.
- Mecanismos de segurança.

A cobertura deve ser utilizada como apoio para identificação de áreas com baixa validação, e não como objetivo isolado.

---

## 12.14 Automação dos Testes

Todos os testes automatizados fazem parte do pipeline de Integração Contínua.

Antes da publicação de uma nova versão são executadas, entre outras, as seguintes etapas:

1. Compilação da aplicação.
2. Execução dos testes unitários.
3. Execução dos testes de integração.
4. Análise de qualidade do código.
5. Construção da imagem Docker.
6. Publicação da imagem.
7. Implantação no ambiente correspondente.

Caso qualquer etapa falhe, o processo de implantação é interrompido.

Essa estratégia impede que versões com falhas conhecidas sejam disponibilizadas.

---

## 12.15 Ambientes de Teste

Os testes são executados em ambientes apropriados para cada finalidade.

### Desenvolvimento

Destinado à execução rápida dos testes durante o desenvolvimento.

Características:

- Execução local.
- Testes unitários.
- Testes de integração.

---

### Homologação

Destinado à validação da aplicação antes da produção.

Características:

- Ambiente semelhante ao de produção.
- Testes End-to-End.
- Testes de regressão.
- Validação funcional.

---

### Produção

Em produção não são executados testes funcionais.

São utilizados mecanismos contínuos de monitoramento, observabilidade e Health Checks para verificar a saúde da plataforma.

---

## 12.16 Cenários Validados

A estratégia de testes foi projetada para validar os principais cenários de negócio da plataforma.

| Cenário | Estratégia de Validação |
|----------|-------------------------|
| Cadastro de cliente | Testes unitários, integração e End-to-End. |
| Atualização cadastral | Testes de integração e End-to-End. |
| Registro de renda | Testes unitários e integração. |
| Solicitação de crédito | Testes unitários, integração e End-to-End. |
| Análise de crédito | Testes das regras de negócio e integração. |
| Decisão de crédito | Testes unitários, integração e End-to-End. |
| Publicação de eventos | Testes de mensageria. |
| Consumo de eventos | Testes de mensageria. |
| Persistência dos dados | Testes de persistência. |
| Autenticação e autorização | Testes de segurança. |
| Falhas em integrações | Testes de resiliência. |

Essa abordagem garante que os fluxos críticos da plataforma sejam continuamente verificados.

---

## 12.17 Boas Práticas Adotadas

A estratégia de testes segue práticas amplamente utilizadas em aplicações corporativas.

Entre elas:

- Automação de todos os níveis de testes.
- Prioridade para testes unitários.
- Testes de integração para validar a comunicação entre componentes.
- Testes End-to-End para os principais fluxos de negócio.
- Testes específicos para APIs REST.
- Testes da mensageria baseada em eventos.
- Testes da camada de persistência.
- Testes de segurança.
- Testes de resiliência.
- Execução automática no pipeline de CI/CD.
- Ambientes de teste independentes.
- Isolamento entre os casos de teste.

Essas práticas aumentam a confiabilidade da plataforma e reduzem o risco de regressões durante sua evolução.

---

## 12.18 Considerações Arquiteturais

- Cada microserviço possui sua própria estratégia de testes.
- A validação ocorre em múltiplos níveis, desde testes unitários até testes End-to-End.
- APIs, eventos, persistência, segurança e resiliência fazem parte da estratégia de testes.
- Todos os testes automatizados são executados durante o pipeline de Integração Contínua.
- A arquitetura prioriza a detecção precoce de falhas.
- Os principais fluxos de negócio permanecem protegidos por testes automatizados.
- A estratégia de testes foi projetada para acompanhar a evolução contínua da plataforma, preservando sua qualidade, confiabilidade e estabilidade.