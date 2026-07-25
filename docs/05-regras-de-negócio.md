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


