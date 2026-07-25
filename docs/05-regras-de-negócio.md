<a id="indice"></a>

# 📑 Índice

1. [🎯 Objetivo](#objetivo)
2. [📌 Escopo](#escopo)
3. [📚 Glossário](#glossario)
4. [⚙️ Especificação das Regras de Negócio](#especificacao-das-regras-de-negocio)
   - 4.1 [🔐 Identity Service](#identity-service)
   - 4.2 [👤 Customer Service](#customer-service)
   - 4.3 [💳 Credit Analysis Service](#credit-analysis-service)
   - 4.4 [📝 Audit Service](#audit-service)
   - 4.5 [🔔 Notification Service](#notification-service)

---

<a id="objetivo"></a>

# 🎯 1. Objetivo

Este documento tem como objetivo especificar as regras de negócio do sistema **Examen Crediti**, estabelecendo as políticas, restrições e comportamentos que regem o domínio da aplicação.

As regras aqui descritas definem como cada processo de negócio deve se comportar, independentemente da tecnologia utilizada em sua implementação. Elas servem como referência para a modelagem do domínio, desenvolvimento dos microsserviços, definição das APIs, elaboração dos testes e evolução do sistema.

Este documento complementa os requisitos funcionais, detalhando as condições, validações e decisões necessárias para garantir que os processos de negócio sejam implementados de forma consistente e alinhada aos objetivos da aplicação.

---

⬆️ [Voltar ao índice](#indice)

<a id="escopo"></a>


# 📌 2. Escopo

Este documento abrange as regras de negócio que governam o funcionamento do sistema **Examen Crediti**, definindo as políticas, restrições, validações e decisões que orientam os processos executados pelos microsserviços da aplicação.

As regras aqui especificadas descrevem o comportamento esperado do domínio de negócio, independentemente de detalhes de arquitetura, infraestrutura ou implementação.

Este documento contempla as regras de negócio relacionadas aos seguintes microsserviços:

- **Identity Service**: gerenciamento de usuários, autenticação, autorização, emissão e gerenciamento de tokens, controle de sessões e permissões de acesso.
- **Customer Service**: gerenciamento do cadastro e manutenção dos dados dos clientes.
- **Credit Analysis Service**: elegibilidade, solicitação e processamento da análise de crédito, cálculo do score, classificação de risco, política de concessão, definição do limite sugerido, simulações e histórico de análises.
- **Audit Service**: registro, consulta e preservação dos eventos de auditoria gerados pelos processos da aplicação.
- **Notification Service**: geração, envio, consulta e preservação do histórico das notificações da plataforma.

Não fazem parte do escopo deste documento:

- requisitos funcionais e casos de uso detalhados;
- requisitos não funcionais;
- arquitetura da solução;
- modelagem do domínio e do banco de dados;
- especificação das APIs;
- mecanismos técnicos de segurança;
- observabilidade, monitoramento e registro de métricas;
- infraestrutura, deploy e ambientes;
- detalhes de implementação, bibliotecas, frameworks ou tecnologias utilizadas.

Os assuntos acima são tratados em seus respectivos documentos da documentação do projeto.

---

⬆️ [Voltar ao índice](#indice)


<a id="glossario"></a>

# 📚 3. Glossário

Os termos definidos nesta seção representam conceitos fundamentais utilizados ao longo deste documento.

| Termo | Definição |
|-------|-----------|
| **Análise de Crédito** | Processo de avaliação da solicitação de crédito de um cliente, considerando as regras de negócio da aplicação para determinar a decisão da análise. |
| **Autenticação** | Processo de verificação da identidade de um usuário por meio de suas credenciais de acesso. |
| **Autorização** | Processo de verificação das permissões de um usuário autenticado para acessar determinados recursos ou executar determinadas operações. |
| **Cliente** | Pessoa cadastrada na aplicação que pode solicitar análises de crédito. |
| **Decisão da Análise** | Resultado final da análise de crédito, podendo ser aprovada, aprovada com restrições ou reprovada. |
| **Elegibilidade** | Conjunto de critérios mínimos que devem ser atendidos para que uma solicitação de análise de crédito possa ser processada. |
| **Evento de Auditoria** | Registro imutável de uma ação relevante realizada durante a execução da aplicação. |
| **Histórico de Análises** | Conjunto das análises de crédito realizadas para um determinado cliente. |
| **Identity Service** | Microsserviço responsável pelo gerenciamento de identidade, autenticação, autorização, usuários, permissões e tokens de acesso. |
| **Limite Sugerido** | Valor máximo de crédito calculado pela aplicação para uma análise aprovada. |
| **Permissão** | Autorização concedida a um usuário para executar uma determinada ação dentro do sistema. |
| **Política de Concessão** | Conjunto de regras utilizadas para determinar a decisão da análise de crédito com base nas informações avaliadas. |
| **Papel (Role)** | Conjunto de permissões atribuídas a um usuário. |
| **Score** | Pontuação calculada pela aplicação para representar o nível de risco associado ao cliente durante a análise de crédito. |
| **Sessão** | Período durante o qual um usuário autenticado permanece autorizado a utilizar a aplicação. |
| **Simulação** | Processo que estima o resultado de uma análise de crédito sem produzir efeitos permanentes no sistema. |
| **Solicitação de Análise** | Pedido realizado para que uma análise de crédito seja processada pela aplicação. |
| **Token de Acesso (Access Token)** | Credencial emitida pelo Identity Service que comprova a autenticação do usuário e permite o acesso aos recursos autorizados. |
| **Token de Renovação (Refresh Token)** | Credencial utilizada para obter um novo Access Token sem exigir uma nova autenticação do usuário. |
| **Usuário** | Pessoa autorizada a acessar a aplicação mediante autenticação. |

---

⬆️ [Voltar ao índice](#indice)


<a id="especificacao-das-regras-de-negocio"></a>

# ⚙️ 4. Especificação das Regras de Negócio

<a id="identity-service"></a>

## 🔐 4.1 Identity Service

O **Identity Service** é responsável pelo gerenciamento da identidade dos usuários da aplicação, abrangendo autenticação, autorização, gerenciamento de usuários, emissão de tokens, controle de sessões e gerenciamento de permissões de acesso.

Seu objetivo é garantir que apenas usuários autenticados e autorizados possam acessar os recursos disponibilizados pelos demais microsserviços.

---

<a id="gerenciamento-de-usuarios"></a>

### 4.1.1 Gerenciamento de Usuários

Esta seção define as regras de negócio relacionadas ao ciclo de vida dos usuários responsáveis por acessar a aplicação.

---

#### RN-ID-001 — Cadastro de Usuário

**Requisitos relacionados**

- RF-ID-001

**Tipo**

Validação

**Regra**

Todo usuário deverá possuir um cadastro único na aplicação.

**Restrições**

- Não deve existir mais de um usuário com o mesmo endereço de e-mail.
- Todo usuário deverá possuir pelo menos um papel (Role) associado.
- O usuário somente poderá acessar a aplicação após a conclusão do cadastro.

**Critérios de Aceitação**

- O sistema deve impedir o cadastro de usuários com endereços de e-mail duplicados.
- Todo usuário cadastrado deve possuir pelo menos um papel associado.

**Entidades Envolvidas**

- User
- Role

---

#### RN-ID-002 — Identificação do Usuário

**Requisitos relacionados**

- RF-ID-001

**Tipo**

Integridade

**Regra**

Cada usuário deverá possuir um identificador único e imutável.

**Restrições**

- O identificador não poderá ser alterado após sua criação.
- O identificador deverá ser utilizado como referência entre os microsserviços.

**Critérios de Aceitação**

- Cada usuário deve possuir um identificador exclusivo.
- O identificador deve permanecer inalterado durante todo o ciclo de vida do usuário.

**Entidades Envolvidas**

- User

---

#### RN-ID-003 — Status do Usuário

**Requisitos relacionados**

- RF-ID-001

**Tipo**

Validação

**Regra**

Todo usuário deverá possuir um status que determine sua disponibilidade para autenticação.

**Restrições**

Os status permitidos são:

- Ativo
- Inativo
- Bloqueado

Usuários com status **Inativo** ou **Bloqueado** não poderão iniciar novas sessões.

**Critérios de Aceitação**

- Apenas usuários com status **Ativo** podem autenticar-se.
- Usuários **Inativos** ou **Bloqueados** devem ter o acesso negado.

**Entidades Envolvidas**

- User

---

#### RN-ID-004 — Atualização de Usuário

**Requisitos relacionados**

- RF-ID-002

**Tipo**

Processo

**Regra**

Os dados cadastrais do usuário poderão ser atualizados sem alterar sua identidade.

**Restrições**

- O identificador do usuário permanece imutável.
- As alterações devem respeitar as validações definidas pela aplicação.

**Critérios de Aceitação**

- O identificador do usuário permanece inalterado após qualquer atualização cadastral.
- Apenas os atributos permitidos podem ser modificados.

**Entidades Envolvidas**

- User

---

#### RN-ID-005 — Inativação de Usuário

**Requisitos relacionados**

- RF-ID-003

**Tipo**

Segurança

**Regra**

Usuários não deverão ser excluídos definitivamente da aplicação.

**Restrições**

- O usuário deverá ser inativado.
- O histórico de operações deverá ser preservado.
- Os registros de auditoria relacionados ao usuário não poderão ser removidos.

**Critérios de Aceitação**

- Usuários inativados não podem iniciar novas sessões.
- O histórico de operações permanece disponível para consulta.
- Os registros de auditoria permanecem íntegros após a inativação do usuário.

**Entidades Envolvidas**

- User
- AuditEvent

---

⬆️ [Voltar ao índice](#indice)


<a id="customer-service"></a>

## 👤 4.2 Customer Service

O **Customer Service** é responsável pelo gerenciamento dos dados cadastrais dos clientes da plataforma.

Seu objetivo é manter informações consistentes, íntegras e atualizadas para subsidiar os processos de análise de crédito e demais funcionalidades da aplicação.

---

<a id="gerenciamento-de-clientes"></a>

### 4.2.1 Gerenciamento de Clientes

Esta seção define as regras de negócio relacionadas ao ciclo de vida dos clientes cadastrados na plataforma.

---

#### RN-CU-001 — Cadastro de Cliente

**Requisitos Relacionados**

- RF-CU-001

**Tipo**

Validação

**Regra**

Todo cliente deverá possuir um cadastro único na plataforma.

**Restrições**

- Não deve existir mais de um cliente com o mesmo CPF.
- O CPF deverá possuir formato válido.
- O cliente deverá possuir nome completo.

**Critérios de Aceitação**

- O sistema deve impedir o cadastro de clientes com CPF duplicado.
- O sistema deve validar o formato do CPF antes do cadastro.
- O cadastro somente poderá ser concluído quando todos os dados obrigatórios forem informados.

**Entidades Envolvidas**

- Customer

---

#### RN-CU-002 — Identificação do Cliente

**Requisitos Relacionados**

- RF-CU-001

**Tipo**

Integridade

**Regra**

Cada cliente deverá possuir um identificador único e imutável.

**Restrições**

- O identificador não poderá ser alterado após sua criação.
- O identificador deverá ser utilizado como referência entre os microsserviços.

**Critérios de Aceitação**

- Cada cliente deve possuir um identificador exclusivo.
- O identificador permanece inalterado durante todo o ciclo de vida do cliente.

**Entidades Envolvidas**

- Customer

---

#### RN-CU-003 — Atualização Cadastral

**Requisitos Relacionados**

- RF-CU-002

**Tipo**

Processo

**Regra**

Os dados cadastrais do cliente poderão ser atualizados sempre que necessário.

**Restrições**

- O identificador do cliente permanece imutável.
- O CPF não poderá ser alterado por operações de atualização cadastral.
- As alterações deverão respeitar as validações definidas pela aplicação.

**Critérios de Aceitação**

- Apenas os atributos permitidos podem ser atualizados.
- O CPF permanece inalterado.
- O identificador do cliente permanece inalterado.

**Entidades Envolvidas**

- Customer

---

#### RN-CU-004 — Correção Excepcional de CPF

**Requisitos Relacionados**

- RF-CU-003

**Tipo**

Segurança

**Regra**

A alteração do CPF somente poderá ocorrer por meio de um processo administrativo específico destinado à correção de erros cadastrais.

**Restrições**

- A solicitação deverá conter justificativa.
- A operação somente poderá ser executada por usuário autorizado.
- O novo CPF deverá ser válido.
- O novo CPF não poderá estar vinculado a outro cliente.
- Toda alteração deverá ser registrada na auditoria.
- O histórico da alteração deverá ser preservado.

**Critérios de Aceitação**

- O sistema valida a justificativa antes da alteração.
- O sistema valida a unicidade do novo CPF.
- O sistema registra o CPF anterior e o novo CPF.
- O sistema registra o usuário responsável pela alteração.
- O sistema registra data e hora da operação.
- O sistema registra o motivo informado para a alteração.
- A alteração somente é concluída após todas as validações serem aprovadas.

**Entidades Envolvidas**

- Customer
- AuditEvent

---

#### RN-CU-005 — Situação Cadastral

**Requisitos Relacionados**

- RF-CU-004

**Tipo**

Validação

**Regra**

Todo cliente deverá possuir uma situação cadastral válida.

**Restrições**

Os status permitidos são:

- Ativo
- Inativo

Clientes inativos não poderão solicitar novas análises de crédito.

**Critérios de Aceitação**

- Apenas clientes ativos podem iniciar novas solicitações de crédito.
- Clientes inativos permanecem com seu histórico preservado.

**Entidades Envolvidas**

- Customer

---

#### RN-CU-006 — Inativação de Cliente

**Requisitos Relacionados**

- RF-CU-005

**Tipo**

Segurança

**Regra**

Clientes não deverão ser excluídos definitivamente da plataforma.

**Restrições**

- O cliente deverá ser inativado.
- O histórico de análises de crédito deverá ser preservado.
- Os registros de auditoria relacionados ao cliente não poderão ser removidos.

**Critérios de Aceitação**

- Clientes inativados não podem iniciar novas análises de crédito.
- O histórico do cliente permanece íntegro.
- Os registros de auditoria permanecem disponíveis para consulta.

**Entidades Envolvidas**

- Customer
- CreditAnalysis
- AuditEvent

---

⬆️ [Voltar ao índice](#indice)


<a id="credit-analysis-service"></a>


## 💳 4.3 Credit Analysis Service

O **Credit Analysis Service** é responsável por processar as solicitações de crédito, selecionar a política de concessão de crédito aplicável, avaliar a elegibilidade do cliente, executar as regras definidas pela política selecionada e determinar o resultado da análise.

Seu objetivo é garantir que todas as solicitações sejam avaliadas de forma consistente, rastreável e conforme as políticas de concessão de crédito da plataforma.

---

### 4.3.1 Solicitação de Crédito

Esta seção define as regras de negócio relacionadas ao recebimento e ao registro das solicitações de crédito.

---

#### RN-CA-001 — Elegibilidade para Solicitação de Crédito

**Requisitos Relacionados**

- RF-CA-001

**Tipo**

Validação

**Regra**

Somente clientes elegíveis poderão solicitar uma análise de crédito.

**Restrições**

- O cliente deverá possuir cadastro ativo.
- O cliente deverá possuir as informações cadastrais obrigatórias preenchidas.
- O cliente deverá atender aos critérios mínimos para submissão da análise.

**Critérios de Aceitação**

- Solicitações realizadas por clientes elegíveis são aceitas.
- Solicitações realizadas por clientes não elegíveis são rejeitadas.

**Entidades Envolvidas**

- Customer
- CreditAnalysis

---

#### RN-CA-002 — Registro da Solicitação

**Requisitos Relacionados**

- RF-CA-001

**Tipo**

Processo

**Regra**

Toda solicitação de crédito deverá ser registrada antes do início da análise.

**Restrições**

- Cada solicitação deverá possuir um identificador único.
- A data e hora da solicitação deverão ser registradas.
- O cliente responsável pela solicitação deverá ser identificado.
- A política de concessão de crédito vigente, aplicável ao produto de crédito solicitado, deverá ser identificada e vinculada à análise.

**Critérios de Aceitação**

- Toda solicitação recebe um identificador único.
- A solicitação permanece disponível para consulta durante todo o seu ciclo de vida.
- A análise registra a política de concessão de crédito utilizada e sua respectiva versão.

**Entidades Envolvidas**

- Customer
- CreditAnalysis
- CreditPolicy

---

### 4.3.2 Processamento da Análise

Esta seção define as regras de negócio relacionadas ao processamento da análise de crédito e à determinação do resultado da avaliação.

---

#### RN-CA-003 — Status da Análise

**Requisitos Relacionados**

- RF-CA-002

**Tipo**

Integridade

**Regra**

Toda análise de crédito deverá possuir um status que represente sua situação durante o processamento.

**Restrições**

Os status permitidos são:

- Pendente
- Em Análise
- Aprovada
- Reprovada
- Cancelada

**Critérios de Aceitação**

- Toda análise possui exatamente um status válido.
- O status deve refletir corretamente o estágio do processamento.

**Entidades Envolvidas**

- CreditAnalysis

---

#### RN-CA-004 — Resultado da Análise

**Requisitos Relacionados**

- RF-CA-003

**Tipo**

Processo

**Regra**

Toda análise de crédito deverá produzir um resultado final com base na avaliação das regras definidas pela política de concessão de crédito correspondente ao produto de crédito solicitado.

**Restrições**

- O resultado deverá ser obtido somente após a execução completa das regras da política de concessão de crédito selecionada.
- Os resultados possíveis são:
  - Aprovado
  - Reprovado

**Critérios de Aceitação**

- Toda análise concluída possui um resultado final.
- O resultado corresponde à avaliação das regras da política de concessão de crédito utilizada.
- O resultado permanece registrado para consultas futuras.

**Entidades Envolvidas**

- CreditAnalysis
- CreditPolicy
- CreditRule

---

### 4.3.3 Histórico das Análises

Esta seção define as regras de negócio relacionadas à preservação do histórico das análises de crédito.

---

#### RN-CA-005 — Histórico das Análises

**Requisitos Relacionados**

- RF-CA-004

**Tipo**

Auditoria

**Regra**

O histórico das análises de crédito deverá ser preservado para garantir a rastreabilidade e a auditoria das decisões tomadas.

**Restrições**

- Análises concluídas não poderão ser excluídas.
- Todas as alterações relevantes deverão ser registradas.
- A política de concessão de crédito utilizada e sua respectiva versão deverão permanecer vinculadas à análise.

**Critérios de Aceitação**

- O histórico permanece disponível para consulta.
- Os registros de auditoria permanecem íntegros.
- É possível identificar a política de concessão de crédito e sua versão utilizadas em qualquer análise realizada.

**Entidades Envolvidas**

- CreditAnalysis
- CreditPolicy
- AuditEvent

---

### 4.3.4 Políticas de Concessão de Crédito

O Examen Crediti utiliza políticas de concessão de crédito pré-estabelecidas pelo sistema para avaliar solicitações de diferentes produtos de crédito.

Cada política de concessão de crédito é composta por:

- Identificação.
- Produto de crédito.
- Versão.
- Conjunto de regras de avaliação.

Durante o processamento de uma solicitação, o sistema seleciona automaticamente a política de concessão de crédito correspondente ao produto de crédito solicitado e executa suas regras para determinar o resultado da análise.

As políticas de concessão de crédito não são cadastradas nem alteradas pelos usuários da plataforma, sendo tratadas como configurações internas do sistema.

---

⬆️ [Voltar ao índice](#indice)


<a id="audit-service"></a>

## 📝 4.4 Audit Service

O **Audit Service** é responsável por registrar, armazenar e disponibilizar os eventos relevantes ocorridos na plataforma, garantindo rastreabilidade, integridade e suporte aos processos de auditoria.

Seu objetivo é preservar o histórico das operações realizadas pelos usuários e pelos serviços da plataforma, permitindo a reconstrução de eventos, a investigação de incidentes e o atendimento aos requisitos de conformidade.

---


### 4.4.1 Registro de Eventos

Esta seção define as regras de negócio relacionadas ao registro dos eventos de auditoria.

---

#### RN-AU-001 — Registro de Eventos

**Requisitos Relacionados**

- RF-AU-001

**Tipo**

Auditoria

**Regra**

Todo evento auditável ocorrido na plataforma deverá ser registrado automaticamente.

**Restrições**

- O registro deverá ocorrer imediatamente após a ocorrência do evento.
- Cada evento deverá possuir um identificador único.
- A data e hora da ocorrência deverão ser registradas.

**Critérios de Aceitação**

- Todo evento auditável gera um registro de auditoria.
- O registro é armazenado com sucesso.

**Entidades Envolvidas**

- AuditEvent

---

#### RN-AU-002 — Integridade dos Registros

**Requisitos Relacionados**

- RF-AU-002

**Tipo**

Integridade

**Regra**

Os registros de auditoria deverão preservar sua integridade após serem armazenados.

**Restrições**

- Registros não poderão ser alterados.
- Registros não poderão ser excluídos durante o período de retenção.

**Critérios de Aceitação**

- Os registros permanecem íntegros durante todo o seu ciclo de vida.
- Toda consulta retorna exatamente os dados originalmente registrados.

**Entidades Envolvidas**

- AuditEvent

---

### 4.4.2 Consulta aos Registros

Esta seção define as regras de negócio relacionadas à consulta dos registros de auditoria.

---

#### RN-AU-003 — Consulta ao Histórico

**Requisitos Relacionados**

- RF-AU-003

**Tipo**

Consulta

**Regra**

Os registros de auditoria deverão permanecer disponíveis para consulta durante todo o período de retenção definido pela plataforma.

**Restrições**

- A consulta não poderá alterar os registros.
- Os registros deverão ser apresentados em ordem cronológica.

**Critérios de Aceitação**

- O histórico pode ser consultado a qualquer momento durante o período de retenção.
- Os registros são apresentados em ordem cronológica.

**Entidades Envolvidas**

- AuditEvent

---

#### RN-AU-004 — Rastreabilidade

**Requisitos Relacionados**

- RF-AU-004

**Tipo**

Auditoria

**Regra**

Todo registro de auditoria deverá conter informações suficientes para rastrear a operação que lhe deu origem.

**Restrições**

Cada registro deverá armazenar, quando aplicável:

- Serviço responsável pela operação.
- Tipo do evento.
- Identificador da entidade relacionada.
- Identificador do usuário responsável.
- Data e hora da ocorrência.
- Resultado da operação.

**Critérios de Aceitação**

- É possível identificar a origem de qualquer evento auditado.
- As informações registradas permitem reconstruir a operação realizada.

**Entidades Envolvidas**

- AuditEvent

---

### 4.4.3 Retenção dos Registros

Esta seção define as regras de negócio relacionadas à preservação dos registros de auditoria.

---

#### RN-AU-005 — Retenção dos Registros

**Requisitos Relacionados**

- RF-AU-005

**Tipo**

Retenção

**Regra**

Os registros de auditoria deverão ser preservados durante todo o período de retenção definido pela plataforma.

**Restrições**

- Os registros não poderão ser removidos antes do término do período de retenção.
- O término do período de retenção deverá seguir a política de retenção estabelecida pela plataforma.

**Critérios de Aceitação**

- Todos os registros permanecem disponíveis durante o período de retenção.
- Os registros são tratados conforme a política de retenção da plataforma.

**Entidades Envolvidas**

- AuditEvent

---

⬆️ [Voltar ao índice](#indice)

<a id="notification-service"></a>

## 🔔 4.5 Notification Service

O **Notification Service** é responsável por consumir eventos publicados na plataforma, gerar notificações para os usuários e registrar o histórico das comunicações realizadas.

Seu objetivo é garantir que eventos relevantes sejam comunicados aos destinatários por meio dos canais de notificação suportados pela plataforma, preservando a rastreabilidade de todo o processo de envio.

---


### 4.5.1 Envio de Notificações

Esta seção define as regras de negócio relacionadas à geração e ao envio das notificações.

---

#### RN-NT-001 — Geração da Notificação

**Requisitos Relacionados**

- RF-NT-001

**Tipo**

Processo

**Regra**

Toda notificação deverá ser gerada automaticamente a partir de um evento elegível publicado na plataforma.

**Restrições**

- Somente eventos previamente definidos poderão originar notificações.
- Cada notificação deverá estar vinculada ao evento que a originou.
- O destinatário deverá ser identificado antes da geração da notificação.

**Critérios de Aceitação**

- A ocorrência de um evento elegível gera uma notificação.
- A notificação permanece vinculada ao evento de origem.

**Entidades Envolvidas**

- Notification
- AuditEvent

---

#### RN-NT-002 — Envio da Notificação

**Requisitos Relacionados**

- RF-NT-002

**Tipo**

Processo

**Regra**

Toda notificação gerada deverá ser enviada por um canal de notificação suportado pela plataforma.

**Restrições**

- O envio somente poderá ocorrer após a geração da notificação.
- O canal de notificação utilizado deverá ser identificado.
- O resultado da tentativa de envio deverá ser registrado.

**Critérios de Aceitação**

- Toda notificação possui uma tentativa de envio registrada.
- O canal de notificação utilizado é identificado.
- O resultado do envio permanece disponível para consulta.

**Entidades Envolvidas**

- Notification
- NotificationChannel

---


### 4.5.2 Consulta às Notificações

Esta seção define as regras de negócio relacionadas à consulta das notificações.

---

#### RN-NT-003 — Consulta às Notificações

**Requisitos Relacionados**

- RF-NT-003

**Tipo**

Consulta

**Regra**

As notificações deverão permanecer disponíveis para consulta durante seu período de retenção.

**Restrições**

- A consulta não poderá alterar os registros.
- As notificações deverão ser apresentadas em ordem cronológica.

**Critérios de Aceitação**

- O histórico pode ser consultado durante o período de retenção.
- As notificações são apresentadas em ordem cronológica.

**Entidades Envolvidas**

- Notification

---

#### RN-NT-004 — Status da Notificação

**Requisitos Relacionados**

- RF-NT-004

**Tipo**

Integridade

**Regra**

Toda notificação deverá possuir um status que represente sua situação durante o processo de envio.

**Restrições**

Os status permitidos são:

- Pendente
- Enviada
- Falhou

**Critérios de Aceitação**

- Toda notificação possui exatamente um status válido.
- O status representa corretamente a situação da notificação.

**Entidades Envolvidas**

- Notification
- NotificationStatus

---


### 4.5.3 Histórico das Notificações

Esta seção define as regras de negócio relacionadas à preservação do histórico das notificações.

---

#### RN-NT-005 — Histórico das Notificações

**Requisitos Relacionados**

- RF-NT-005

**Tipo**

Auditoria

**Regra**

O histórico das notificações deverá ser preservado para garantir a rastreabilidade das comunicações realizadas pela plataforma.

**Restrições**

- Notificações registradas não poderão ser excluídas durante o período de retenção.
- O histórico deverá preservar o evento que originou a notificação.
- O canal de notificação utilizado deverá permanecer registrado.
- O status final da notificação deverá permanecer registrado.

**Critérios de Aceitação**

- O histórico permanece disponível para consulta.
- É possível identificar o evento que originou cada notificação.
- É possível identificar o canal de notificação utilizado.
- É possível identificar o status final da notificação.

**Entidades Envolvidas**

- Notification
- NotificationChannel
- NotificationStatus
- AuditEvent

---

⬆️ [Voltar ao índice](#indice)
