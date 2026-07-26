# 🏛️ Arquitetura da Solução

---

<a id="indice"></a>

# 📑 Índice

1. [🎯 Objetivo](#objetivo)
2. [📌 Escopo](#escopo)
3. [🏗️ Visão Geral da Arquitetura](#visao-geral-da-arquitetura)
4. [🧩 Estilo Arquitetural](#estilo-arquitetural)
5. [⚙️ Componentes da Arquitetura](#componentes-da-arquitetura)
   - 5.1 [🌐 API Gateway](#api-gateway)
   - 5.2 [🔐 Identity Service](#identity-service)
   - 5.3 [👤 Customer Service](#customer-service)
   - 5.4 [💳 Credit Analysis Service](#credit-analysis-service)
   - 5.5 [📝 Audit Service](#audit-service)
   - 5.6 [🔔 Notification Service](#notification-service)
6. [🔄 Comunicação entre Componentes](#comunicacao-entre-componentes)
   - 6.1 [🔗 Comunicação Síncrona](#comunicacao-sincrona)
   - 6.2 [📨 Comunicação Assíncrona](#comunicacao-assincrona)
7. [💾 Persistência de Dados](#persistencia-de-dados)
8. [⚡ Cache](#cache)
9. [🔒 Segurança da Arquitetura](#seguranca-da-arquitetura)
10. [☁️ Infraestrutura](#infraestrutura)
11. [📊 Observabilidade](#observabilidade)
12. [📐 Decisões Arquiteturais](#decisoes-arquiteturais)

---

<a id="objetivo"></a>

# 🎯 1. Objetivo

Este documento tem como objetivo descrever a arquitetura da solução do sistema **Examen Crediti**, apresentando sua organização em microsserviços, os componentes arquiteturais, os mecanismos de comunicação, a estratégia de persistência de dados, a infraestrutura utilizada e as principais decisões arquiteturais adotadas.

A arquitetura aqui definida estabelece como os componentes da aplicação se relacionam para atender aos requisitos funcionais, aos requisitos não funcionais e às regras de negócio especificadas na documentação do projeto, servindo como referência para a implementação, evolução e manutenção da plataforma.

Este documento complementa os documentos de visão geral, requisitos funcionais, requisitos não funcionais e regras de negócio, descrevendo a estrutura técnica da solução sem abordar detalhes específicos de implementação, modelagem do domínio, especificação das APIs ou configurações de tecnologias e frameworks.

---

⬆️ [Voltar ao índice](#indice)


<a id="escopo"></a>

# 📌 2. Escopo

Este documento descreve a arquitetura da solução do sistema **Examen Crediti**, apresentando a organização dos componentes da aplicação, suas responsabilidades, os mecanismos de comunicação, a estratégia de persistência de dados, os recursos de cache, a infraestrutura utilizada e as principais decisões arquiteturais adotadas.

A arquitetura especificada neste documento contempla os seguintes componentes da solução:

- API Gateway;
- Identity Service;
- Customer Service;
- Credit Analysis Service;
- Audit Service;
- Notification Service;
- Mecanismos de comunicação síncrona e assíncrona entre os componentes;
- Estratégia de persistência de dados;
- Utilização de cache;
- Aspectos arquiteturais de segurança;
- Infraestrutura da aplicação;
- Observabilidade;
- Decisões arquiteturais.

Não fazem parte do escopo deste documento:

- Regras de negócio;
- Modelagem do domínio e banco de dados;
- Especificação das APIs;
- Detalhes de implementação;
- Configurações de tecnologias, frameworks e bibliotecas;
- Cstratégia de testes;
- Procedimentos de implantação.

Os assuntos acima são tratados em seus respectivos documentos da documentação do projeto.

---

⬆️ [Voltar ao índice](#indice)


<a id="visao-geral-da-arquitetura"></a>

# 🏗️ 3. Visão Geral da Arquitetura

A arquitetura do **Examen Crediti** foi projetada com base no estilo arquitetural de microsserviços, no qual cada componente da aplicação possui responsabilidades bem definidas, autonomia para evolução e persistência de dados própria.

Os microsserviços comunicam-se por meio de chamadas síncronas utilizando APIs REST e por comunicação assíncrona baseada em eventos, permitindo o desacoplamento entre componentes e aumentando a escalabilidade, a resiliência e a flexibilidade da solução.

O acesso aos serviços é realizado por meio de um **API Gateway**, responsável por centralizar o ponto de entrada da aplicação, encaminhar as requisições aos microsserviços correspondentes e aplicar políticas arquiteturais comuns.

Cada microsserviço é responsável pelo gerenciamento do seu próprio conjunto de dados, evitando compartilhamento direto de bancos de dados e reduzindo o acoplamento entre os componentes da solução.

A arquitetura também incorpora mecanismos de cache, observabilidade, auditoria e segurança para atender aos requisitos de desempenho, rastreabilidade, disponibilidade e proteção das informações processadas pela plataforma.



> *O diagrama geral da arquitetura será inserido posteriormente.*

---

⬆️ [Voltar ao índice](#indice)


<a id="estilo-arquitetural"></a>

# 🧩 4. Estilo Arquitetural

O **Examen Crediti** adota uma arquitetura baseada em **microsserviços**, na qual a aplicação é composta por serviços independentes, cada um responsável por um conjunto específico de funcionalidades do domínio de negócio.

Cada microsserviço possui responsabilidade bem definida, ciclo de vida independente, persistência de dados própria e pode evoluir sem impactar diretamente os demais componentes da solução, desde que sejam preservados os contratos de comunicação estabelecidos.

A comunicação entre os componentes ocorre por meio de dois modelos complementares:

- **Comunicação síncrona**, utilizando APIs REST para operações que exigem resposta imediata.
- **Comunicação assíncrona**, baseada na publicação e no consumo de eventos para processos desacoplados e de longa duração.

A arquitetura também adota o princípio de **banco de dados por serviço**, no qual cada microsserviço é responsável pelo gerenciamento exclusivo de seus próprios dados, evitando compartilhamento direto entre os componentes da aplicação.

O acesso aos microsserviços é centralizado por meio de um **API Gateway**, responsável por receber as requisições externas e encaminhá-las aos serviços apropriados.

Esse modelo arquitetural proporciona benefícios como:

- baixo acoplamento entre os componentes;
- alta coesão das responsabilidades de cada microsserviço;
- escalabilidade independente dos serviços;
- facilidade de manutenção e evolução da aplicação;
- maior resiliência diante de falhas;
- flexibilidade para adoção de diferentes tecnologias quando necessário.

Os componentes que compõem essa arquitetura são apresentados na seção seguinte.

---

⬆️ [Voltar ao índice](#indice)


<a id="componentes-da-arquitetura"></a>

# ⚙️ 5. Componentes da Arquitetura

A arquitetura do **Examen Crediti** é composta por um conjunto de componentes especializados que atuam de forma integrada para atender às necessidades da plataforma.

Cada componente possui responsabilidades bem definidas e comunica-se com os demais por meio de mecanismos padronizados, preservando o desacoplamento, a coesão e a independência entre os serviços.

As subseções a seguir apresentam uma visão geral de cada componente da arquitetura.

---

<a id="api-gateway"></a>

## 🌐 5.1 API Gateway

O **API Gateway** constitui o ponto único de entrada da aplicação, sendo responsável por receber as requisições dos clientes, encaminhá-las aos microsserviços correspondentes e centralizar funcionalidades comuns de acesso à plataforma.

---

<a id="identity-service"></a>

## 🔐 5.2 Identity Service

O **Identity Service** é responsável pelo gerenciamento da identidade dos usuários da aplicação, contemplando autenticação, autorização, gerenciamento de usuários, papéis, permissões, sessões e emissão de tokens de acesso.

---

<a id="customer-service"></a>

## 👤 5.3 Customer Service

O **Customer Service** é responsável pelo gerenciamento dos dados cadastrais dos clientes, garantindo a integridade, consistência e disponibilidade das informações utilizadas pelos demais componentes da solução.

---

<a id="credit-analysis-service"></a>

## 💳 5.4 Credit Analysis Service

O **Credit Analysis Service** é responsável pelo processamento das solicitações de crédito, pela seleção da política de concessão aplicável e pela determinação do resultado da análise de crédito de acordo com as regras de negócio estabelecidas.

---

<a id="audit-service"></a>

## 📝 5.5 Audit Service

O **Audit Service** é responsável pelo registro, armazenamento e consulta dos eventos de auditoria gerados pelos componentes da aplicação, garantindo rastreabilidade, integridade e suporte às atividades de auditoria.

---

<a id="notification-service"></a>

## 🔔 5.6 Notification Service

O **Notification Service** é responsável pelo consumo de eventos publicados pela plataforma, pela geração e envio de notificações aos usuários e pela manutenção do histórico das comunicações realizadas.

---

⬆️ [Voltar ao índice](#indice)


<a id="comunicacao-entre-componentes"></a>

# 🔄 6. Comunicação entre Componentes

Os componentes da arquitetura do **Examen Crediti** comunicam-se por meio de mecanismos síncronos e assíncronos, escolhidos de acordo com as características e os requisitos de cada processo da aplicação.

A comunicação síncrona é utilizada quando uma resposta imediata é necessária para a continuidade do processamento da requisição. Já a comunicação assíncrona é empregada em processos desacoplados, orientados a eventos, nos quais a execução pode ocorrer independentemente do processamento da requisição original.

Essa combinação permite reduzir o acoplamento entre os microsserviços, aumentar a escalabilidade da solução e melhorar sua resiliência diante de falhas.

---

<a id="comunicacao-sincrona"></a>

## 🔗 6.1 Comunicação Síncrona

A comunicação síncrona entre os componentes da aplicação é realizada por meio de APIs REST utilizando o protocolo HTTP.

Nesse modelo, o componente solicitante aguarda a resposta do componente responsável antes de prosseguir com o processamento da requisição.

Esse mecanismo é utilizado em operações que exigem retorno imediato, como autenticação de usuários, consultas cadastrais e recuperação de informações necessárias para o processamento da requisição.

As chamadas síncronas são realizadas exclusivamente entre componentes autorizados, respeitando os contratos definidos pelas APIs e os mecanismos de autenticação e autorização da plataforma.

---

<a id="comunicacao-assincrona"></a>

## 📨 6.2 Comunicação Assíncrona

A comunicação assíncrona é baseada na publicação e no consumo de eventos utilizando o **Apache Kafka**.

Nesse modelo, os componentes produtores publicam eventos relacionados às operações realizadas, enquanto os componentes consumidores processam esses eventos de forma independente, sem necessidade de comunicação direta entre eles.

Essa abordagem é utilizada em processos que não exigem resposta imediata ao solicitante, permitindo maior desacoplamento entre os serviços e melhor escalabilidade da aplicação.

A utilização de eventos também contribui para a resiliência da arquitetura, reduzindo dependências diretas entre os componentes e possibilitando o processamento independente das operações.

---

⬆️ [Voltar ao índice](#indice)


<a id="persistencia-de-dados"></a>

# 💾 7. Persistência de Dados

A arquitetura do **Examen Crediti** adota o princípio de **banco de dados por serviço** (*Database per Service*), no qual cada microsserviço é responsável pelo gerenciamento exclusivo de seus próprios dados.

Essa estratégia reduz o acoplamento entre os componentes da aplicação, preserva a autonomia dos microsserviços e permite que cada serviço evolua de forma independente, desde que mantenha os contratos de comunicação estabelecidos pela arquitetura.

A persistência de dados da solução está organizada conforme a tabela a seguir.

| Componente | Tecnologia de Persistência |
|------------|----------------------------|
| Identity Service | PostgreSQL |
| Customer Service | PostgreSQL |
| Credit Analysis Service | PostgreSQL |
| Audit Service | MongoDB |
| Notification Service | PostgreSQL |

Cada microsserviço é proprietário de seu banco de dados e responsável por garantir a consistência das informações sob seu domínio.

O compartilhamento direto de dados entre bancos de diferentes microsserviços não é permitido pela arquitetura da solução. Toda troca de informações ocorre por meio das APIs disponibilizadas pelos componentes ou pela comunicação baseada em eventos.

A modelagem das entidades, seus atributos, relacionamentos e demais detalhes da persistência são apresentados no documento **07-modelagem.md**.

---

⬆️ [Voltar ao índice](#indice)


<a id="cache"></a>

# ⚡ 8. Cache

A arquitetura do **Examen Crediti** utiliza um mecanismo de cache distribuído para reduzir o tempo de resposta das operações mais frequentes, diminuir a carga sobre os bancos de dados e melhorar o desempenho geral da aplicação.

O cache é implementado utilizando o **Redis**, sendo empregado exclusivamente para o armazenamento temporário de informações que podem ser reconstruídas ou recuperadas pelos componentes da solução.

Na arquitetura da plataforma, o Redis é utilizado para:

- Armazenamento temporário de informações de clientes frequentemente consultadas;
- Armazenamento das políticas de concessão de crédito utilizadas pelo processo de análise;
- Manutenção da lista de bloqueio (*blacklist*) de tokens JWT invalidados antes do término de sua validade.

Os dados armazenados em cache possuem tempo de vida limitado e podem ser atualizados ou removidos sempre que necessário, garantindo a consistência das informações utilizadas pelos microsserviços.

O cache atua como um mecanismo de otimização de desempenho e não substitui a persistência permanente dos dados da aplicação.

---

⬆️ [Voltar ao índice](#indice)


<a id="seguranca-da-arquitetura"></a>

# 🔒 9. Segurança da Arquitetura

A arquitetura do **Examen Crediti** adota uma estratégia de segurança baseada em autenticação, autorização e controle de acesso aos recursos da aplicação.

O acesso aos microsserviços é realizado por meio do **API Gateway**, responsável por receber as requisições externas e encaminhá-las aos componentes internos da plataforma.

A autenticação dos usuários é realizada pelo **Identity Service**, responsável pela validação das credenciais e pela emissão dos tokens de acesso utilizados nas requisições autenticadas.

Após a autenticação, as requisições são autorizadas de acordo com as permissões atribuídas ao usuário, garantindo que apenas operações compatíveis com seu perfil possam ser executadas.

A arquitetura utiliza tokens **JWT (JSON Web Token)** para representar a identidade autenticada do usuário durante sua sessão na aplicação.

Para aumentar a segurança da plataforma, tokens invalidados antes do término de sua validade são registrados em uma lista de bloqueio (*blacklist*) mantida em cache, impedindo sua reutilização.

A especificação dos mecanismos de autenticação, autorização, gerenciamento de usuários, papéis, permissões e tokens é apresentada no documento **09-seguranca.md**.

---

⬆️ [Voltar ao índice](#indice)


<a id="infraestrutura"></a>

# ☁️ 10. Infraestrutura

A infraestrutura do **Examen Crediti** foi projetada para oferecer escalabilidade, isolamento entre componentes e facilidade de implantação dos microsserviços.

Cada microsserviço é executado de forma independente em um ambiente conteinerizado utilizando **Docker**, permitindo padronização do ambiente de execução, portabilidade e simplificação do processo de implantação.

A arquitetura foi concebida para ser executada em ambiente de computação em nuvem, utilizando serviços da **Amazon Web Services (AWS)** como plataforma de hospedagem.

A infraestrutura contempla a execução dos seguintes componentes:

- API Gateway;
- Identity Service;
- Customer Service;
- Credit Analysis Service;
- Audit Service;
- Notification Service;
- Apache Kafka;
- Redis;
- PostgreSQL;
- MongoDB;
- Prometheus;
- Grafana.

A separação dos componentes em serviços independentes possibilita escalabilidade individual, maior disponibilidade da aplicação e manutenção com impacto reduzido sobre os demais componentes da plataforma.

Os procedimentos de implantação, configuração da infraestrutura e estratégias de entrega da aplicação são documentados no documento **10-deploy.md**.

---

⬆️ [Voltar ao índice](#indice)


<a id="observabilidade"></a>

# 📊 11. Observabilidade

A arquitetura do **Examen Crediti** incorpora mecanismos de observabilidade para permitir o monitoramento contínuo da aplicação, facilitar a identificação de falhas e apoiar a análise do comportamento dos microsserviços em ambiente de execução.

A estratégia de observabilidade é baseada na coleta, armazenamento e visualização de métricas e informações operacionais produzidas pelos componentes da plataforma.

Para esse propósito, a arquitetura utiliza as seguintes tecnologias:

- **OpenTelemetry** para instrumentação da aplicação e coleta de dados de telemetria;
- **Prometheus** para coleta e armazenamento de métricas;
- **Grafana** para visualização de métricas e criação de painéis de monitoramento.

Os dados coletados permitem acompanhar indicadores relacionados ao desempenho, disponibilidade e utilização dos microsserviços, contribuindo para a identificação de problemas e para a tomada de decisões operacionais.

A configuração das ferramentas de observabilidade, bem como a definição das métricas, painéis e alertas, é tratada na documentação técnica correspondente.

---

⬆️ [Voltar ao índice](#indice)


<a id="decisoes-arquiteturais"></a>

# 📐 12. Decisões Arquiteturais

As principais decisões arquiteturais adotadas no desenvolvimento do **Examen Crediti** estão resumidas na tabela a seguir.

| Decisão | Justificativa |
|---------|---------------|
| Arquitetura baseada em microsserviços | Favorecer baixo acoplamento, alta coesão, escalabilidade e evolução independente dos componentes. |
| API Gateway como ponto único de entrada | Centralizar o acesso à plataforma e simplificar o roteamento das requisições. |
| Banco de dados por serviço (*Database per Service*) | Garantir autonomia dos microsserviços e reduzir dependências entre componentes. |
| PostgreSQL como banco relacional principal | Armazenar dados transacionais com integridade, consistência e suporte a relacionamentos. |
| MongoDB para auditoria | Armazenar registros de eventos de forma flexível e adequada a grandes volumes de dados históricos. |
| Apache Kafka para comunicação assíncrona | Desacoplar os microsserviços e permitir o processamento orientado a eventos. |
| Redis como mecanismo de cache distribuído | Reduzir a carga sobre os bancos de dados e melhorar o desempenho das operações mais frequentes. |
| JWT para autenticação | Permitir autenticação stateless e simplificar a comunicação entre os componentes da arquitetura. |
| Docker para conteinerização | Padronizar os ambientes de execução e facilitar a implantação da aplicação. |
| AWS como plataforma de hospedagem | Disponibilizar uma infraestrutura escalável, confiável e adequada ao modelo de microsserviços. |
| OpenTelemetry, Prometheus e Grafana para observabilidade | Possibilitar monitoramento, coleta de métricas e acompanhamento da saúde da aplicação. |

As decisões apresentadas neste documento representam a arquitetura definida para a solução no momento de sua elaboração. Alterações futuras poderão ser registradas por meio de **Architecture Decision Records (ADRs)**, preservando o histórico de evolução arquitetural do projeto.

---

⬆️ [Voltar ao índice](#indice)