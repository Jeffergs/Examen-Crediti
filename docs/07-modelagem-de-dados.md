# 🗂️ Modelagem de Dados

<a id="indice"></a>

# 📑 Índice

1. [Objetivo](#objetivo)
2. [Escopo](#escopo)
3. [Visão Geral da Modelagem](#visao-geral-da-modelagem)
4. [Estratégia de Persistência](#estrategia-de-persistencia)
5. [Modelagem por Microsserviço](#modelagem-por-microsservico)
   - [5.1 Identity Service](#identity-service)
   - [5.2 Customer Service](#customer-service)
   - [5.3 Credit Analysis Service](#credit-analysis-service)
   - [5.4 Audit Service](#audit-service)
   - [5.5 Notification Service](#notification-service)
6. [Relacionamentos entre Domínios](#relacionamentos-entre-dominios)
7. [Convenções de Modelagem](#convencoes-de-modelagem)

<a id="objetivo"></a>

# 🎯 1. Objetivo

Este documento tem como objetivo apresentar a modelagem de dados do **Examen Crediti**, descrevendo a organização das informações persistidas pela aplicação e sua distribuição entre os microsserviços.

São apresentados os modelos de dados utilizados por cada domínio da solução, incluindo suas entidades, atributos, relacionamentos e responsabilidades de persistência, em conformidade com a arquitetura definida para a plataforma.

Este documento serve como referência para o entendimento da estrutura dos dados da aplicação, apoiando as atividades de desenvolvimento, manutenção e evolução do sistema.

---

⬆️ [Voltar ao índice](#indice)


<a id="escopo"></a>

# 📋 2. Escopo

Este documento contempla:

- A visão geral da modelagem de dados da aplicação;
- A estratégia de persistência adotada pela arquitetura;
- A distribuição dos dados entre os microsserviços;
- As entidades pertencentes a cada domínio;
- Os atributos e relacionamentos das entidades;
- As convenções utilizadas na modelagem de dados.

Este documento não contempla:

- Regras de negócio;
- Arquitetura da solução;
- Especificação das APIs;
- Implementação das entidades em código;
- Consultas SQL;
- Scripts de criação ou migração de banco de dados;
- Configurações de persistência ou de frameworks.

---

⬆️ [Voltar ao índice](#indice)


<a id="visao-geral-da-modelagem"></a>

# 🗂️ 3. Visão Geral da Modelagem

A Modelagem de Dados do **Examen Crediti** foi concebida para atender aos princípios arquiteturais definidos para a solução, adotando a separação dos dados por domínio de negócio e a autonomia dos microsserviços.

Cada microsserviço é responsável pela persistência, manutenção e integridade das informações pertencentes ao seu contexto, sendo proprietário exclusivo de seu modelo de dados.

A Distribuição dos dados segue o princípio de **Banco de Dados por Serviço** (*Database per Service*), permitindo que cada domínio evolua de forma independente, sem compartilhamento direto de tabelas ou coleções com outros microsserviços.

A Comunicação entre domínios ocorre exclusivamente por meio das APIs disponibilizadas pelos serviços ou pela troca de eventos, preservando o baixo acoplamento entre os componentes da plataforma.

A Modelagem apresentada neste documento descreve as entidades, seus atributos e seus relacionamentos, organizados de acordo com as responsabilidades de cada microsserviço.

---

⬆️ [Voltar ao índice](#indice)


<a id="estrategia-de-persistencia"></a>

# 💾 4. Estratégia de Persistência

A Persistência de Dados do **Examen Crediti** está organizada de acordo com o princípio de **Banco de Dados por Serviço** (*Database per Service*), no qual cada microsserviço possui seu próprio mecanismo de armazenamento e é responsável pela gestão exclusiva de seus dados.

Essa estratégia garante o isolamento entre os domínios da aplicação, reduz o acoplamento entre os componentes e permite que cada microsserviço evolua de forma independente.

A Distribuição das tecnologias de persistência utilizadas na solução é apresentada na tabela a seguir.

| Microsserviço | Tecnologia de Persistência |
|---------------|----------------------------|
| Identity Service | PostgreSQL |
| Customer Service | PostgreSQL |
| Credit Analysis Service | PostgreSQL |
| Audit Service | MongoDB |
| Notification Service | PostgreSQL |

Cada microsserviço é proprietário de seu modelo de dados e responsável por garantir a integridade e a consistência das informações sob seu domínio.

O Compartilhamento direto de tabelas, coleções ou bancos de dados entre microsserviços não é permitido pela arquitetura da solução.

A Integração entre os domínios ocorre exclusivamente por meio das APIs disponibilizadas pelos serviços ou pela comunicação baseada em eventos.

---

⬆️ [Voltar ao índice](#indice)


<a id="modelagem-por-microsservico"></a>

# 🏗️ 5. Modelagem por Microsserviço

A Modelagem de Dados do **Examen Crediti** está organizada de acordo com os domínios de negócio da aplicação.

Cada microsserviço possui seu próprio modelo de dados, contendo apenas as entidades necessárias para atender às suas responsabilidades, em conformidade com o princípio de **Banco de Dados por Serviço** (*Database per Service*).

As Seções seguintes apresentam as entidades pertencentes a cada domínio, seus atributos, seus relacionamentos e suas responsabilidades dentro da solução.

---

<a id="identity-service"></a>

## 👤 5.1 Identity Service

O **Identity Service** é responsável pelo gerenciamento da identidade dos usuários da plataforma, contemplando autenticação, autorização e controle de acesso aos recursos da aplicação.

Seu modelo de dados é composto pelas seguintes entidades:

- Usuário;
- Papel (Role);
- Permissão (Permission).

### Entidades

#### Usuário

A Entidade **Usuário** representa as credenciais e as informações necessárias para autenticação e identificação de um usuário na plataforma.

| Atributo | Descrição |
|----------|-----------|
| Id | Identificador único do usuário. |
| Nome | Nome completo do usuário. |
| E-mail | Endereço de e-mail utilizado para autenticação. |
| Senha | Senha armazenada de forma criptografada. |
| Status | Situação da conta do usuário. |

#### Papel (Role)

A Entidade **Papel** representa um conjunto de permissões atribuídas aos usuários da plataforma.

| Atributo | Descrição |
|----------|-----------|
| Id | Identificador único do papel. |
| Nome | Nome do papel. |
| Descrição | Descrição das responsabilidades do papel. |

#### Permissão (Permission)

A Entidade **Permissão** representa uma autorização específica para execução de determinada funcionalidade da aplicação.

| Atributo | Descrição |
|----------|-----------|
| Id | Identificador único da permissão. |
| Nome | Nome da permissão. |
| Descrição | Descrição da permissão concedida. |

### Relacionamentos

O Modelo de Dados do **Identity Service** possui os seguintes relacionamentos:

- Um Usuário Pode Possuir Um ou Mais Papéis.
- Um Papel Pode Ser Atribuído a Um ou Mais Usuários.
- Um Papel Pode Possuir Uma ou Mais Permissões.
- Uma Permissão Pode Pertencer a Um ou Mais Papéis.

### Diagrama Entidade-Relacionamento

> **Figura X – Diagrama Entidade-Relacionamento do Identity Service.**
>
> *(Inserir diagrama posteriormente.)*

---

⬆️ [Voltar ao índice](#indice)


<a id="customer-service"></a>

## 👥 5.2 Customer Service

O **Customer Service** é responsável pelo gerenciamento das informações cadastrais, de contato, profissionais e financeiras dos clientes da plataforma.

Seu modelo de dados é composto pelas seguintes entidades:

- Cliente;
- Endereço;
- Contato;
- Vínculo Empregatício;
- Renda.

### Entidades

#### Cliente

A Entidade **Cliente** representa a pessoa que solicita análises de crédito na plataforma.

| Atributo | Descrição |
|----------|-----------|
| Id | Identificador único do cliente. |
| Nome | Nome completo do cliente. |
| CPF | Cadastro de Pessoa Física do cliente. |
| Data de Nascimento | Data de nascimento do cliente. |
| Estado Civil | Estado civil do cliente. |
| Nacionalidade | Nacionalidade do cliente. |
| Status | Situação cadastral do cliente. |

#### Endereço

A Entidade **Endereço** representa os endereços cadastrados pelo cliente.

| Atributo | Descrição |
|----------|-----------|
| Id | Identificador único do endereço. |
| Tipo | Tipo do endereço. |
| CEP | Código de Endereçamento Postal. |
| Logradouro | Nome da via pública. |
| Número | Número do imóvel. |
| Complemento | Informação complementar do endereço. |
| Bairro | Bairro do endereço. |
| Cidade | Cidade do endereço. |
| Estado | Unidade federativa do endereço. |

#### Contato

A Entidade **Contato** representa os meios de comunicação do cliente.

| Atributo | Descrição |
|----------|-----------|
| Id | Identificador único do contato. |
| Tipo | Tipo do contato. |
| Valor | Informação do contato. |
| Principal | Indica se o contato é o principal. |

#### Vínculo Empregatício

A Entidade **Vínculo Empregatício** representa os vínculos profissionais do cliente.

| Atributo | Descrição |
|----------|-----------|
| Id | Identificador único do vínculo empregatício. |
| Empresa | Nome da empresa. |
| Cargo | Cargo ocupado pelo cliente. |
| Data de Admissão | Data de admissão. |
| Tipo de Contrato | Tipo de vínculo empregatício. |

#### Renda

A Entidade **Renda** representa as fontes de renda declaradas pelo cliente.

| Atributo | Descrição |
|----------|-----------|
| Id | Identificador único da renda. |
| Tipo | Tipo da renda. |
| Valor | Valor da renda. |
| Fonte | Origem da renda. |
| Comprovada | Indica se a renda foi comprovada documentalmente. |

### Relacionamentos

O Modelo de Dados do **Customer Service** possui os seguintes relacionamentos:

- Um Cliente Pode Possuir Um ou Mais Endereços.
- Um Endereço Pertence a Um Único Cliente.
- Um Cliente Pode Possuir Um ou Mais Documentos.
- Um Documento Pertence a Um Único Cliente.
- Um Cliente Pode Possuir Um ou Mais Contatos.
- Um Contato Pertence a Um Único Cliente.
- Um Cliente Pode Possuir Um ou Mais Vínculos Empregatícios.
- Um Vínculo Empregatício Pertence a Um Único Cliente.
- Um Cliente Pode Possuir Uma ou Mais Rendas.
- Uma Renda Pertence a Um Único Cliente.

### Diagrama Entidade-Relacionamento

> **Figura X – Diagrama Entidade-Relacionamento do Customer Service.**
>
> *(Inserir diagrama posteriormente.)*

---

⬆️ [Voltar ao índice](#indice)


<a id="credit-analysis-service"></a>

## 💳 5.3 Credit Analysis Service

O **Credit Analysis Service** é responsável pelo processamento das solicitações de crédito, pela aplicação das políticas de concessão e pelo registro do histórico das análises realizadas.

Seu modelo de dados é composto pelas seguintes entidades:

- Solicitação de Crédito;
- Política de Crédito;
- Análise de Crédito;
- Histórico de Análise.

### Entidades

#### Solicitação de Crédito

A Entidade **Solicitação de Crédito** representa o pedido de crédito realizado pelo cliente.

| Atributo | Descrição |
|----------|-----------|
| Id | Identificador único da solicitação. |
| Cliente Id | Identificador do cliente solicitante. |
| Valor Solicitado | Valor solicitado para crédito. |
| Prazo | Quantidade de parcelas ou prazo solicitado. |
| Data da Solicitação | Data de criação da solicitação. |
| Status | Situação atual da solicitação. |

#### Política de Crédito

A Entidade **Política de Crédito** representa as regras utilizadas para avaliação das solicitações de crédito.

| Atributo | Descrição |
|----------|-----------|
| Id | Identificador único da política. |
| Nome | Nome da política de crédito. |
| Score Mínimo | Score mínimo exigido. |
| Renda Mínima | Renda mínima exigida. |
| Valor Máximo | Valor máximo permitido. |
| Status | Situação da política. |

#### Análise de Crédito

A Entidade **Análise de Crédito** representa o resultado da avaliação realizada sobre uma solicitação de crédito.

| Atributo | Descrição |
|----------|-----------|
| Id | Identificador único da análise. |
| Solicitação Id | Identificador da solicitação analisada. |
| Política Id | Identificador da política aplicada. |
| Score Obtido | Score calculado durante a análise. |
| Decisão | Resultado da análise. |
| Valor Aprovado | Valor aprovado para concessão do crédito. |
| Justificativa | Motivo da decisão tomada. |
| Data da Análise | Data da execução da análise. |

#### Histórico de Análise

A Entidade **Histórico de Análise** registra todas as alterações ocorridas durante o processo de análise de crédito, preservando a rastreabilidade das decisões.

| Atributo | Descrição |
|----------|-----------|
| Id | Identificador único do registro. |
| Análise Id | Identificador da análise relacionada. |
| Evento | Evento ocorrido durante a análise. |
| Descrição | Descrição do evento registrado. |
| Responsável | Identificação do responsável pela alteração. |
| Data do Registro | Data e hora do registro do evento. |

### Relacionamentos

O Modelo de Dados do **Credit Analysis Service** possui os seguintes relacionamentos:

- Uma Solicitação de Crédito Pode Gerar Uma ou Mais Análises de Crédito.
- Uma Análise de Crédito Utiliza Uma Política de Crédito.
- Uma Análise de Crédito Pode Possuir Um ou Mais Registros de Histórico.
- Um Registro de Histórico Pertence a Uma Única Análise de Crédito.

### Diagrama Entidade-Relacionamento

> **Figura X – Diagrama Entidade-Relacionamento do Credit Analysis Service.**
>
> *(Inserir diagrama posteriormente.)*

---

⬆️ [Voltar ao índice](#indice)


<a id="audit-service"></a>

## 📜 5.4 Audit Service

O **Audit Service** é responsável pelo registro dos eventos relevantes ocorridos na plataforma, garantindo rastreabilidade, auditoria e suporte à análise de incidentes.

Seu modelo de dados é composto pela seguinte entidade:

- Evento de Auditoria.

### Entidades

#### Evento de Auditoria

A Entidade **Evento de Auditoria** representa um registro imutável de um evento ocorrido durante a execução da aplicação.

| Atributo | Descrição |
|----------|-----------|
| Id | Identificador único do evento. |
| Serviço | Microsserviço responsável pela geração do evento. |
| Tipo de Evento | Categoria do evento registrado. |
| Entidade | Entidade relacionada ao evento. |
| Identificador da Entidade | Identificador do registro relacionado ao evento. |
| Operação | Operação executada sobre a entidade. |
| Usuário | Identificação do usuário responsável pela operação, quando aplicável. |
| Data do Evento | Data e hora da ocorrência do evento. |
| Payload | Conteúdo da mensagem ou do evento, contendo as informações de negócio relacionadas à operação realizada. |

### Relacionamentos

O Modelo de Dados do **Audit Service** não possui relacionamentos diretos com outras entidades persistidas.

Os Eventos de Auditoria são produzidos pelos demais microsserviços e armazenados de forma independente, preservando a rastreabilidade das operações realizadas na plataforma.

### Diagrama Entidade-Relacionamento

> **Figura X – Modelo do Documento do Audit Service.**
>
> *(Inserir diagrama posteriormente.)*

---

⬆️ [Voltar ao índice](#indice)


<a id="notification-service"></a>

## 🔔 5.5 Notification Service

O **Notification Service** é responsável pelo gerenciamento dos modelos de mensagens e pelo envio das notificações geradas pelos eventos da plataforma.

Seu modelo de dados é composto pelas seguintes entidades:

- Template de Notificação;
- Notificação.

### Entidades

#### Template de Notificação

A Entidade **Template de Notificação** representa um modelo reutilizável utilizado para geração das mensagens enviadas aos usuários.

| Atributo | Descrição |
|----------|-----------|
| Id | Identificador único do template. |
| Nome | Nome do template. |
| Tipo de Evento | Evento da plataforma associado ao template. |
| Canal | Canal de comunicação para o qual o template foi definido. |
| Assunto | Assunto da mensagem, quando aplicável. |
| Conteúdo | Modelo da mensagem contendo textos e variáveis parametrizadas. |
| Status | Situação do template. |

#### Notificação

A Entidade **Notificação** representa uma mensagem gerada a partir de um template e destinada a um usuário da plataforma.

| Atributo | Descrição |
|----------|-----------|
| Id | Identificador único da notificação. |
| Template Id | Identificador do template utilizado. |
| Destinatário | Identificação do destinatário da notificação. |
| Canal | Canal efetivamente utilizado para o envio da notificação. |
| Status | Situação atual da notificação. |
| Data de Criação | Data e hora da criação da notificação. |
| Data de Envio | Data e hora do envio da notificação. |

### Relacionamentos

O Modelo de Dados do **Notification Service** possui os seguintes relacionamentos:

- Um Template de Notificação Pode Estar Associado a Um Tipo de Evento.
- Um Template de Notificação Pode Gerar Uma ou Mais Notificações.
- Uma Notificação É Gerada a Partir de Um Único Template de Notificação.

As Notificações são geradas a partir dos eventos produzidos pelos demais microsserviços. Ao receber um evento, o **Notification Service** identifica seu tipo, seleciona o template correspondente, gera a mensagem e realiza o envio pelo canal configurado.

Embora o atributo **Canal** esteja presente tanto na entidade **Template de Notificação** quanto na entidade **Notificação**, essa redundância é intencional.

Na Entidade **Template de Notificação**, o atributo define o canal previsto para geração da mensagem. Já na Entidade **Notificação**, ele registra o canal efetivamente utilizado no momento do envio, preservando o histórico da operação mesmo que o template seja alterado futuramente.

Essa abordagem favorece a rastreabilidade, a auditoria e a integridade histórica das notificações enviadas pela plataforma.

### Diagrama Entidade-Relacionamento

> **Figura X – Diagrama Entidade-Relacionamento do Notification Service.**
>
> *(Inserir diagrama posteriormente.)*

---

⬆️ [Voltar ao índice](#indice)


<a id="relacionamentos-entre-dominios"></a>

# 🔗 6. Relacionamentos entre Domínios

A Modelagem de Dados do **Examen Crediti** está organizada em domínios independentes, sendo cada microsserviço responsável pelo gerenciamento exclusivo de suas próprias entidades.

Os relacionamentos entre os domínios não ocorrem por meio do compartilhamento direto de tabelas ou coleções, mas por referências lógicas, chamadas entre APIs e troca de eventos, em conformidade com a arquitetura baseada em microsserviços.

A integração entre os domínios ocorre conforme descrito a seguir:

- O **Identity Service** é responsável pela autenticação e autorização dos usuários da plataforma.
- O **Customer Service** gerencia as informações cadastrais, profissionais e financeiras dos clientes.
- O **Credit Analysis Service** consulta as informações do cliente para realizar a análise de crédito e registrar seu resultado.
- O **Audit Service** consome os eventos produzidos pelos demais microsserviços para registrar a rastreabilidade das operações.
- O **Notification Service** consome eventos da plataforma para gerar e enviar notificações aos usuários.

A Figura a seguir apresenta uma visão conceitual do relacionamento entre os domínios da aplicação.

> **Figura X – Relacionamento entre os Domínios do Examen Crediti.**
>
> *(Inserir diagrama posteriormente.)*

## Fluxo Conceitual

```text
                 ┌──────────────────────┐
                 │   Identity Service   │
                 └──────────┬───────────┘
                            │
                 Autentica o Usuário
                            │
                            ▼
                 ┌──────────────────────┐
                 │   Customer Service   │
                 └──────────▲───────────┘
                            │
            Consulta os Dados do Cliente
                            │
                            │
                 ┌──────────┴───────────┐
                 │ Credit Analysis      │
                 │ Service              │
                 └──────────┬───────────┘
                            │
                    Publica Eventos
                            │
              ┌─────────────┴─────────────┐
              │                           │
              ▼                           ▼
     ┌──────────────────┐        ┌──────────────────────┐
     │  Audit Service   │        │ Notification Service │
     └──────────────────┘        └──────────┬───────────┘
                                            │
                                  Envia Notificações
                                            │
                                            ▼
                                         Cliente
```

O fluxo apresentado representa apenas a interação lógica entre os domínios da aplicação, não correspondendo a relacionamentos físicos entre bancos de dados.

Cada microsserviço permanece responsável pelo gerenciamento exclusivo de seu próprio modelo de dados, preservando o princípio de **Banco de Dados por Serviço** (*Database per Service*).

As integrações síncronas ocorrem por meio de APIs REST, enquanto as integrações assíncronas utilizam eventos publicados na plataforma. Essa abordagem reduz o acoplamento entre os serviços, favorece a escalabilidade da solução e permite que novos consumidores de eventos sejam adicionados sem impactar os produtores.

---

⬆️ [Voltar ao índice](#indice)


<a id="convencoes-de-modelagem"></a>

# 📐 7. Convenções de Modelagem

As convenções apresentadas neste capítulo definem os padrões utilizados na Modelagem de Dados do **Examen Crediti**, garantindo consistência entre os microsserviços e facilitando a evolução da plataforma.

## Identificadores

Foram adotados identificadores únicos para todas as entidades da aplicação.

Cada entidade possui um atributo **Id**, responsável por identificar unicamente seus registros.

## Chaves Estrangeiras

Os relacionamentos internos de cada microsserviço são representados por chaves estrangeiras.

Não existem chaves estrangeiras entre bancos de dados pertencentes a microsserviços distintos.

As referências entre domínios são realizadas por identificadores e mecanismos de integração, como APIs REST e eventos.

## Responsabilidade dos Dados

Cada microsserviço é proprietário exclusivo de seus dados.

Nenhum serviço realiza operações diretas de leitura ou escrita no banco de dados de outro microsserviço.

Esse princípio preserva a autonomia dos domínios e reduz o acoplamento entre os serviços.

## Persistência

Cada microsserviço utiliza a tecnologia de persistência mais adequada às suas necessidades.

| Microsserviço | Banco de Dados |
|--------------|----------------|
| Identity Service | PostgreSQL |
| Customer Service | PostgreSQL |
| Credit Analysis Service | PostgreSQL |
| Audit Service | MongoDB |
| Notification Service | PostgreSQL |

Essa abordagem segue o padrão **Database per Service**, permitindo que cada domínio evolua de forma independente.

## Integridade dos Dados

A integridade dos dados é garantida por meio de:

- Restrições do banco de dados.
- Validações implementadas na camada de domínio.
- Regras de negócio dos microsserviços.
- Comunicação entre serviços por APIs e eventos.

## Evolução da Modelagem

A Modelagem de Dados poderá evoluir ao longo do ciclo de vida da aplicação.

Novas entidades, atributos e relacionamentos poderão ser incorporados conforme surgirem novos requisitos de negócio, preservando os princípios arquiteturais adotados pela plataforma.

---

⬆️ [Voltar ao índice](#indice)