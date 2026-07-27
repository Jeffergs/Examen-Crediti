# 9. Segurança

## 9.1 Visão Geral

A segurança é um requisito transversal da plataforma Examen Crediti e está presente em todos os microserviços.

A arquitetura adota mecanismos de autenticação, autorização, validação de entrada, proteção de dados e comunicação segura para garantir a confidencialidade, integridade e disponibilidade das informações.

As responsabilidades de autenticação e autorização são centralizadas no Identity Service, enquanto os demais microserviços atuam como Resource Servers, validando os tokens JWT recebidos nas requisições.

---

## 9.2 Objetivos de Segurança

A arquitetura foi projetada para atender aos seguintes objetivos:

- Garantir que apenas usuários autenticados possam acessar a plataforma.
- Controlar o acesso aos recursos de acordo com as permissões do usuário.
- Proteger dados sensíveis durante o armazenamento e a transmissão.
- Minimizar a superfície de ataque da aplicação.
- Garantir rastreabilidade das operações.
- Seguir boas práticas recomendadas pelo OWASP.
- Permitir evolução da arquitetura sem comprometer a segurança.

---

## 9.3 Arquitetura de Segurança

A segurança da plataforma está baseada na separação de responsabilidades entre autenticação e autorização.

```text
                    +------------------+
                    |     Cliente      |
                    +---------+--------+
                              |
                              |
                     Bearer JWT Token
                              |
                              ▼
                    +------------------+
                    |   API Gateway    |
                    +---------+--------+
                              |
               Validação do Token JWT
                              |
         +--------------------+--------------------+
         |                    |                    |
         ▼                    ▼                    ▼
 +---------------+   +----------------+   +-------------------+
 | Identity      |   | Customer       |   | Credit Analysis   |
 | Service       |   | Service        |   | Service           |
 +---------------+   +----------------+   +-------------------+
         |                    |                    |
         +--------------------+--------------------+
                              |
                              ▼
                Resource Servers (JWT Validation)
```

Cada microserviço valida localmente o JWT recebido, sem necessidade de consultar o Identity Service a cada requisição.

---

## 9.4 Princípios de Segurança

A arquitetura segue os seguintes princípios:

- Defense in Depth.
- Least Privilege.
- Fail Secure.
- Secure by Default.
- Separation of Concerns.
- Zero Trust entre clientes e serviços externos.
- Stateless Authentication.

Esses princípios orientam todas as decisões relacionadas à segurança da plataforma.

---

## 9.5 Autenticação

A autenticação é responsabilidade exclusiva do Identity Service.

O fluxo ocorre da seguinte forma:

1. O usuário informa suas credenciais.
2. O Identity Service valida as credenciais.
3. Um Access Token JWT é emitido.
4. O cliente envia o token em todas as requisições subsequentes.
5. Cada microserviço valida o token antes de processar a requisição.

As credenciais do usuário nunca são compartilhadas entre os microserviços.

---

## 9.6 Autorização

Após a autenticação, a autorização é realizada com base nas permissões presentes no JWT.

Cada endpoint define quais papéis possuem autorização para acessá-lo.

Exemplos:

| Papel | Descrição |
|--------|-----------|
| USER | Usuário comum da plataforma. |
| ADMIN | Administrador da aplicação. |

O controle de acesso é realizado pelo Spring Security.

A autorização ocorre antes da execução das regras de negócio.

---

## 9.7 JSON Web Token (JWT)

O Access Token segue o padrão JWT.

O token contém apenas informações necessárias para identificação e autorização do usuário.

Exemplo de claims:

```json
{
  "sub": "6f7d6db9-2f85-4f42-a3de-1a4eb8f0d27a",
  "username": "john.doe",
  "roles": [
    "USER"
  ],
  "iat": 1782020000,
  "exp": 1782023600
}
```

Os tokens possuem tempo de expiração limitado.

Após expirados, deixam de ser aceitos pelos microserviços.

Nenhum microserviço armazena estado de autenticação em memória ou sessão.

A autenticação permanece totalmente stateless.

## 9.8 Controle de Acesso

O controle de acesso é realizado em duas camadas:

- Autenticação do usuário.
- Autorização baseada em papéis (Roles).

A autenticação garante a identidade do usuário.

A autorização determina quais recursos poderão ser acessados.

O acesso aos endpoints é protegido por regras declaradas no Spring Security.

Exemplos:

| Endpoint | Acesso |
|----------|---------|
| `/api/v1/auth/**` | Público |
| `/api/v1/customers/**` | Usuário autenticado |
| `/api/v1/credit-requests/**` | Usuário autenticado |
| `/api/v1/audit-events/**` | Administrador |
| `/api/v1/notifications/**` | Usuário autenticado |

A autorização deve ocorrer antes da execução das regras de negócio.

---

## 9.9 Proteção de Senhas

As senhas dos usuários nunca são armazenadas em texto puro.

Antes da persistência, cada senha é submetida a um algoritmo criptográfico de hash.

A plataforma utiliza:

- BCrypt Password Encoder.
- Salt gerado automaticamente.
- Comparação segura durante a autenticação.

Essa abordagem impede a recuperação da senha original, mesmo em caso de comprometimento do banco de dados.

---

## 9.10 Comunicação Segura

Toda comunicação entre clientes e a plataforma deve utilizar HTTPS.

O protocolo TLS garante:

- Confidencialidade.
- Integridade.
- Autenticidade da comunicação.

Requisições utilizando HTTP não devem ser aceitas em ambientes produtivos.

As comunicações entre microserviços também devem ocorrer através de canais protegidos.

---

## 9.11 Validação de Entrada

Todos os dados recebidos pelas APIs devem ser validados antes da execução das regras de negócio.

As validações são realizadas em múltiplas camadas.

### Validação Sintática

Realizada automaticamente pelo Bean Validation.

Exemplos:

- Campos obrigatórios.
- Tamanho mínimo e máximo.
- Formato de e-mail.
- Valores mínimos e máximos.
- Expressões regulares.

---

### Validação Semântica

Realizada pela camada de aplicação ou domínio.

Exemplos:

- Cliente já cadastrado.
- CPF já existente.
- Valor solicitado maior que o permitido.
- Cliente sem renda cadastrada.

---

### Sanitização

A plataforma não interpreta HTML ou JavaScript enviados pelos clientes.

Entradas são tratadas como dados, reduzindo riscos relacionados a:

- Cross-Site Scripting (XSS).
- Injeção de conteúdo.

---

## 9.12 Tratamento de Erros

Mensagens de erro retornadas pelas APIs não devem expor detalhes internos da aplicação.

Exemplo de resposta:

```json
{
    "timestamp": "2026-08-21T18:10:02Z",
    "status": 400,
    "error": "Validation Error",
    "message": "Invalid request.",
    "path": "/api/v1/customers"
}
```

Informações como:

- stack trace;
- consultas SQL;
- nomes de tabelas;
- caminhos internos;
- detalhes de exceções;

não devem ser retornadas ao cliente.

Esses detalhes permanecem disponíveis apenas nos logs da aplicação.

---

## 9.13 Segurança dos Dados

A plataforma protege informações sensíveis durante todo o seu ciclo de vida.

As principais medidas adotadas incluem:

- Criptografia durante a transmissão dos dados.
- Armazenamento seguro de credenciais.
- Validação de autenticação em todas as requisições.
- Controle de acesso baseado em papéis.
- Identificadores utilizando UUID.
- Princípio do menor privilégio.

Além disso, dados sensíveis não devem ser registrados em logs da aplicação.

Exemplos:

- Senhas.
- Tokens JWT.
- Credenciais.
- Informações bancárias.
- Dados pessoais desnecessários para diagnóstico.

Os logs devem conter apenas informações suficientes para auditoria, monitoramento e investigação de incidentes.

---

## 9.14 Segurança entre Microsserviços

Os microserviços são desacoplados e se comunicam por meio de APIs REST e eventos publicados no Apache Kafka.

As seguintes práticas são adotadas:

- Comunicação autenticada entre clientes e APIs.
- Tokens JWT validados pelos Resource Servers.
- Comunicação assíncrona por eventos.
- Baixo acoplamento entre serviços.
- Isolamento das responsabilidades de autenticação no Identity Service.

Os eventos publicados não contêm informações sensíveis além do necessário para o processamento dos consumidores.

---

## 9.15 Ameaças Mitigadas

A arquitetura da plataforma foi projetada para reduzir os riscos associados às principais ameaças de segurança encontradas em aplicações web modernas.

A tabela a seguir apresenta as ameaças consideradas e os mecanismos adotados para mitigá-las.

| Ameaça | Mitigação |
|---------|-----------|
| Broken Authentication | Autenticação centralizada no Identity Service, JWT com expiração e senhas protegidas por BCrypt. |
| Broken Access Control | Autorização baseada em Roles utilizando Spring Security. |
| SQL Injection | Uso de JPA/Hibernate com consultas parametrizadas e validação das entradas. |
| Cross-Site Scripting (XSS) | APIs REST retornam JSON e tratam entradas como dados, sem interpretar HTML ou JavaScript. |
| Cross-Site Request Forgery (CSRF) | APIs stateless autenticadas por Bearer Token JWT. |
| Replay Attack | Tokens JWT com tempo de expiração limitado e comunicação protegida por HTTPS. |
| Credential Stuffing | Proteção de senhas com BCrypt e possibilidade de integração com mecanismos de limitação de tentativas de autenticação. |
| Brute Force | Limitação de tentativas de autenticação pode ser aplicada no API Gateway ou Identity Service. |
| Sensitive Data Exposure | HTTPS obrigatório, proteção de credenciais, ausência de dados sensíveis em logs e respostas das APIs. |
| Security Misconfiguration | Configuração centralizada, princípios Secure by Default e revisão das configurações de segurança. |

As decisões arquiteturais adotadas estão alinhadas às recomendações do OWASP Top 10 para aplicações web modernas.

---

## 9.16 Boas Práticas Adotadas

A arquitetura adota práticas amplamente utilizadas em sistemas corporativos.

Entre elas:

- HTTPS obrigatório.
- JWT com tempo de expiração.
- BCrypt para armazenamento de senhas.
- APIs Stateless.
- Validação de entrada em todas as requisições.
- Tratamento centralizado de exceções.
- Princípio do menor privilégio (Least Privilege).
- Defesa em profundidade (Defense in Depth).
- Separação entre autenticação e autorização.
- Resource Servers validando JWT localmente.
- Não exposição de informações internas nas respostas das APIs.
- Não armazenamento de credenciais em texto puro.
- Não registro de informações sensíveis em logs.
- Adoção das recomendações do OWASP Top 10.

Essas práticas reduzem a superfície de ataque da plataforma e contribuem para uma arquitetura mais segura, resiliente e de fácil evolução.

---

## 9.17 Considerações Arquiteturais

- O Identity Service é o único responsável pela autenticação dos usuários.
- Os demais microserviços atuam como Resource Servers, validando localmente os tokens JWT.
- A autenticação é totalmente stateless.
- O controle de acesso é baseado em papéis (Roles).
- As senhas são protegidas utilizando BCrypt.
- Toda comunicação ocorre sobre HTTPS.
- Todas as entradas são validadas antes da execução das regras de negócio.
- Informações sensíveis nunca são expostas nas respostas das APIs.
- Informações sensíveis não são registradas em logs.
- A arquitetura segue princípios como Defense in Depth, Least Privilege, Secure by Default e Separation of Concerns.
- As decisões de segurança adotadas estão alinhadas às recomendações do OWASP Top 10.