# 11. Arquitetura de Deploy

## 11.1 Visão Geral

A arquitetura de deploy define como os microserviços da plataforma Examen Crediti são empacotados, configurados e implantados nos diferentes ambientes de execução.

Cada componente da aplicação é implantado de forma independente, permitindo evolução contínua, escalabilidade e redução do impacto durante novas versões.

A estratégia de deploy foi projetada para atender arquiteturas baseadas em microsserviços e princípios de Continuous Delivery.

---

## 11.2 Objetivos

A arquitetura de deploy possui os seguintes objetivos:

- Permitir implantação independente dos microserviços.
- Facilitar a automação do processo de entrega.
- Reduzir riscos durante atualizações.
- Garantir consistência entre os ambientes.
- Simplificar rollback de versões.
- Favorecer escalabilidade horizontal.
- Padronizar o empacotamento da aplicação.

---

## 11.3 Ambientes

A plataforma é organizada em ambientes independentes.

### Desenvolvimento (Development)

Utilizado durante o desenvolvimento da aplicação.

Características:

- Desenvolvimento local.
- Dados não produtivos.
- Execução via Docker Compose.
- Configurações específicas para desenvolvimento.

---

### Homologação (Staging)

Ambiente destinado à validação da aplicação antes da produção.

Características:

- Infraestrutura semelhante à produção.
- Dados controlados.
- Validação funcional.
- Testes de integração.
- Testes de regressão.

---

### Produção (Production)

Ambiente utilizado pelos usuários finais.

Características:

- Alta disponibilidade.
- Monitoramento contínuo.
- Logs centralizados.
- Observabilidade completa.
- Configurações otimizadas para desempenho.

---

## 11.4 Estratégia de Empacotamento

Cada microserviço é distribuído como uma aplicação independente.

Cada serviço possui:

- Código-fonte próprio.
- Processo de build independente.
- Imagem Docker própria.
- Configuração independente.
- Ciclo de vida independente.

Essa abordagem reduz o acoplamento entre os serviços e facilita a evolução da plataforma.

---

## 11.5 Containers

Todos os componentes da plataforma são executados em containers.

Os principais componentes são:

- API Gateway
- Identity Service
- Customer Service
- Credit Analysis Service
- Audit Service
- Notification Service
- PostgreSQL
- MongoDB
- Redis
- Kafka

A utilização de containers garante padronização entre os ambientes e facilita a replicação da infraestrutura.

---

## 11.6 Arquitetura de Implantação

```text
                 Cliente
                    │
                    ▼
             API Gateway
                    │
      ┌─────────────┼─────────────┐
      ▼             ▼             ▼
 Identity      Customer     Credit Analysis
 Service        Service          Service
      │             │             │
      └─────────────┼─────────────┘
                    ▼
        Audit / Notification
              Services
                    │
     ┌──────────────┼──────────────┐
     ▼              ▼              ▼
 PostgreSQL      MongoDB         Redis
                    │
                    ▼
                  Kafka
```

Todos os componentes são implantados de forma independente e comunicam-se pela rede interna da infraestrutura.

---

## 11.7 Configuração da Aplicação

As configurações da aplicação são externalizadas.

Exemplos:

- URLs de serviços.
- Credenciais.
- Portas.
- Configurações do banco de dados.
- Configurações do Kafka.
- Configurações do Redis.
- Chaves JWT.
- Níveis de log.

Essa abordagem permite que a mesma aplicação seja utilizada em diferentes ambientes apenas alterando suas configurações.

---

## 11.8 Perfis de Execução

A plataforma utiliza perfis específicos para cada ambiente.

Exemplos:

- development
- staging
- production

Cada perfil possui configurações apropriadas para seu ambiente de execução.

Essa separação reduz riscos de configuração incorreta entre ambientes.

## 11.9 Pipeline de Integração e Entrega Contínuas

A plataforma adota uma estratégia de Integração Contínua (Continuous Integration - CI) e Entrega Contínua (Continuous Delivery - CD).

O pipeline automatiza as etapas necessárias para validação e disponibilização de novas versões da aplicação.

O fluxo geral contempla:

1. Commit do código-fonte.
2. Execução automática do pipeline.
3. Compilação da aplicação.
4. Execução dos testes automatizados.
5. Análise de qualidade do código.
6. Construção das imagens Docker.
7. Publicação das imagens no registro de containers.
8. Implantação no ambiente correspondente.

Essa abordagem reduz atividades manuais, aumenta a confiabilidade das entregas e garante consistência entre os ambientes.

---

## 11.10 Versionamento das Imagens

Cada microserviço possui sua própria imagem Docker.

As imagens são versionadas independentemente, permitindo a evolução isolada de cada serviço.

Exemplo:

```text
identity-service:1.0.0

customer-service:1.0.0

credit-analysis-service:1.0.0
```

O versionamento independente permite implantar apenas os serviços modificados, reduzindo o impacto de novas versões.

---

## 11.11 Estratégia de Implantação

Os microserviços são implantados de forma independente.

Essa estratégia oferece benefícios como:

- Atualização individual dos serviços.
- Redução do tempo de indisponibilidade.
- Menor impacto em caso de falhas.
- Escalabilidade independente.
- Evolução desacoplada.

Sempre que possível, uma nova versão deve ser validada antes de substituir a versão em execução.

---

## 11.12 Rollback

Caso uma implantação apresente problemas, deve ser possível restaurar rapidamente a versão anterior do serviço.

A estratégia de rollback considera:

- Versionamento das imagens Docker.
- Configurações externalizadas.
- Independência entre microserviços.

Como cada serviço possui ciclo de vida próprio, o rollback pode ser realizado apenas no componente afetado, sem necessidade de reimplantar toda a plataforma.

---

## 11.13 Escalabilidade

A arquitetura foi projetada para permitir escalabilidade horizontal.

Quando necessário, novas instâncias de um microserviço podem ser adicionadas para distribuir a carga de processamento.

Essa estratégia permite escalar apenas os componentes que apresentarem maior demanda.

Exemplos:

- Aumento das instâncias do Credit Analysis Service durante picos de solicitações de crédito.
- Aumento das instâncias do Notification Service em períodos de elevado volume de eventos.
- Escalonamento do API Gateway para suportar maior quantidade de requisições simultâneas.

A escalabilidade independente otimiza a utilização dos recursos da infraestrutura.

---

## 11.14 Disponibilidade

A plataforma foi projetada para maximizar sua disponibilidade operacional.

Para isso, adota práticas como:

- Implantação independente dos microserviços.
- Health Checks contínuos.
- Monitoramento da infraestrutura.
- Observabilidade centralizada.
- Rollback de versões.
- Escalabilidade horizontal.

Essas medidas reduzem o impacto de falhas e aumentam a continuidade dos serviços.

---

## 11.15 Recuperação de Falhas

A arquitetura prevê mecanismos para recuperação de falhas operacionais.

Entre eles:

- Reinicialização de containers.
- Reprocessamento de eventos quando aplicável.
- Retry em integrações externas.
- Circuit Breaker para dependências indisponíveis.
- Rollback de versões.
- Monitoramento contínuo.

Esses mecanismos aumentam a resiliência da plataforma diante de falhas temporárias.

---

## 11.16 Cenários de Implantação

A arquitetura de deploy foi projetada para suportar diferentes cenários operacionais comuns em ambientes de produção.

A tabela a seguir apresenta alguns exemplos.

| Cenário | Como a arquitetura responde |
|----------|-----------------------------|
| Publicação de uma nova versão | O microserviço é implantado independentemente, sem necessidade de atualizar toda a plataforma. |
| Falha após uma implantação | A versão anterior da imagem Docker pode ser restaurada por meio do processo de rollback. |
| Aumento da carga de processamento | O microserviço pode ser escalado horizontalmente com novas instâncias. |
| Indisponibilidade de uma instância | Outra instância do mesmo serviço pode assumir o processamento das requisições. |
| Atualização de configurações | As configurações são externalizadas, permitindo alterações específicas para cada ambiente sem modificar o código-fonte. |
| Evolução de um único microserviço | Apenas o serviço alterado é recompilado, empacotado e implantado. |
| Correção urgente de um defeito | Uma nova versão do microserviço pode ser publicada rapidamente, reduzindo o impacto sobre os demais componentes. |
| Inclusão de novos microserviços | Novos serviços podem ser adicionados sem alterar o processo de implantação dos componentes existentes. |

Esses cenários demonstram que a arquitetura foi projetada para favorecer evolução contínua, disponibilidade e baixo acoplamento entre os componentes da plataforma.

---

## 11.17 Boas Práticas Adotadas

A arquitetura de deploy segue práticas amplamente utilizadas em aplicações corporativas.

Entre elas:

- Containers para todos os componentes.
- Configurações externalizadas.
- Ambientes independentes.
- Pipeline automatizado de CI/CD.
- Versionamento independente dos microserviços.
- Versionamento das imagens Docker.
- Deploy independente por serviço.
- Rollback de versões.
- Escalabilidade horizontal.
- Health Checks contínuos.
- Observabilidade integrada.
- Automação do processo de implantação.

Essas práticas aumentam a confiabilidade das implantações, reduzem riscos operacionais e simplificam a evolução da plataforma.

---

## 11.18 Considerações Arquiteturais

- Cada microserviço possui ciclo de vida independente.
- Todos os serviços são distribuídos como imagens Docker.
- As configurações são externalizadas e variam conforme o ambiente.
- A plataforma utiliza ambientes independentes de desenvolvimento, homologação e produção.
- O pipeline automatiza compilação, testes, empacotamento e implantação.
- O versionamento independente permite implantações parciais da plataforma.
- A arquitetura suporta rollback de serviços individuais.
- A escalabilidade ocorre de forma horizontal e independente por microserviço.
- A disponibilidade é reforçada por Health Checks, observabilidade e monitoramento contínuo.
- A estratégia de deploy foi projetada para reduzir indisponibilidades, facilitar a evolução contínua da plataforma e garantir consistência entre os diferentes ambientes de execução.