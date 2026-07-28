# Digital Banking API — Documentação de Implementação

## 1. Resultado esperado

Construir uma API bancária REST com qualidade de portfólio profissional, capaz de cadastrar clientes e contas, consultar saldo, realizar depósitos, saques e transferências e manter um histórico auditável de todas as movimentações.

O projeto deve demonstrar Java, Spring Boot, modelagem relacional, integridade transacional, tratamento de erros, testes automatizados, documentação de API e decisões técnicas.

## 2. Escopo do MVP

Incluído no MVP:

- Cadastro e consulta de clientes.
- Abertura e consulta de contas.
- Depósito, saque e transferência.
- Extrato paginado.
- Validação de saldo e status da conta.
- Persistência em PostgreSQL.
- Documentação Swagger/OpenAPI.
- Testes unitários e de integração.
- Execução local com Docker Compose.

Fora do MVP:

- PIX real, boletos ou integração com instituições financeiras.
- Processamento de cartão.
- Aplicativo mobile.
- Autenticação biométrica.
- Uso de dinheiro real.

## 3. Stack recomendada

- Java 21.
- Spring Boot 3.
- Spring Web.
- Spring Data JPA.
- Bean Validation.
- PostgreSQL.
- Flyway.
- Maven.
- Docker e Docker Compose.
- Swagger/OpenAPI.
- JUnit 5, Mockito e Testcontainers.

## 4. Arquitetura

```text
src/main/java/br/com/nay/digitalbanking
├── config
├── controller
├── dto
│   ├── request
│   └── response
├── exception
├── mapper
├── model
├── repository
├── service
└── validation
```

Fluxo principal:

```text
Cliente HTTP → Controller → Service → Repository → PostgreSQL
                         ↓
                  Regras de negócio
                         ↓
                 Registro de transação
```

## 5. Modelo de dados

### customer

- id: UUID.
- full_name: varchar.
- document: varchar único.
- email: varchar único.
- created_at: timestamp.
- status: ACTIVE ou BLOCKED.

### account

- id: UUID.
- customer_id: chave estrangeira.
- branch: varchar.
- account_number: varchar único.
- balance: numeric(19,2).
- status: ACTIVE, BLOCKED ou CLOSED.
- version: controle de concorrência otimista.
- created_at: timestamp.

### transaction

- id: UUID.
- account_id: chave estrangeira.
- related_account_id: conta relacionada em transferências.
- type: DEPOSIT, WITHDRAWAL, TRANSFER_IN ou TRANSFER_OUT.
- amount: numeric(19,2).
- balance_after: numeric(19,2).
- idempotency_key: varchar único quando aplicável.
- description: varchar.
- created_at: timestamp.

Use `BigDecimal` para valores monetários. Nunca use `double` ou `float`.

## 6. Regras de negócio

1. O documento e o e-mail do cliente devem ser únicos.
2. Uma conta só pode ser aberta para cliente ativo.
3. Valores devem ser maiores que zero.
4. Saques não podem exceder o saldo disponível.
5. Transferências não podem ter a mesma conta como origem e destino.
6. A transferência deve ocorrer em uma única transação de banco de dados.
7. Toda alteração de saldo deve gerar um registro imutável em `transaction`.
8. Contas bloqueadas ou encerradas não podem movimentar valores.
9. Requisições repetidas com a mesma chave de idempotência não podem duplicar uma operação.
10. Erros devem retornar código HTTP, mensagem clara, timestamp e identificador da requisição.

## 7. Endpoints do MVP

```text
POST   /api/v1/customers
GET    /api/v1/customers/{id}
POST   /api/v1/accounts
GET    /api/v1/accounts/{id}
GET    /api/v1/accounts/{id}/balance
GET    /api/v1/accounts/{id}/transactions?page=0&size=20
POST   /api/v1/accounts/{id}/deposits
POST   /api/v1/accounts/{id}/withdrawals
POST   /api/v1/transfers
```

Exemplo de transferência:

```json
{
  "sourceAccountId": "uuid",
  "destinationAccountId": "uuid",
  "amount": 250.00,
  "description": "Transferência de teste",
  "idempotencyKey": "transfer-2026-0001"
}
```

## 8. Passo a passo de desenvolvimento

### Etapa 1 — Preparação

1. Gerar o projeto no Spring Initializr.
2. Selecionar Java 21, Maven, Spring Web, JPA, Validation, PostgreSQL e Flyway.
3. Criar a estrutura de pacotes.
4. Configurar variáveis de ambiente em `.env.example`.
5. Criar `.gitignore`, `docker-compose.yml` e perfil `application-local.yml`.

Critério de aceite: aplicação inicia e se conecta ao PostgreSQL vazio.

### Etapa 2 — Banco e migrations

1. Criar migrations para `customer`, `account` e `transaction`.
2. Adicionar índices para documento, e-mail, número da conta e data da transação.
3. Definir chaves estrangeiras e restrições.
4. Criar dados de exemplo apenas em perfil de desenvolvimento.

Critério de aceite: banco é criado do zero somente executando a aplicação.

### Etapa 3 — Clientes e contas

1. Criar entidades e enums.
2. Criar DTOs de entrada e saída.
3. Implementar repositories.
4. Criar services com validações.
5. Criar controllers.
6. Implementar mapeamento sem expor entidades JPA diretamente.

Critério de aceite: cliente e conta podem ser criados e consultados pelo Swagger.

### Etapa 4 — Movimentações

1. Implementar depósito.
2. Implementar saque.
3. Implementar transferência com `@Transactional`.
4. Salvar saldo posterior em cada lançamento.
5. Adicionar controle de concorrência com `@Version`.
6. Implementar idempotência para transferências.

Critério de aceite: qualquer falha no meio da transferência desfaz toda a operação.

### Etapa 5 — Erros e observabilidade

1. Criar exceções de domínio.
2. Implementar `@RestControllerAdvice`.
3. Padronizar respostas de erro.
4. Adicionar logs sem registrar documentos completos ou dados sensíveis.
5. Adicionar Actuator com endpoints seguros de health e info.

### Etapa 6 — Testes

Criar testes para:

- Cliente duplicado.
- Conta para cliente inexistente.
- Depósito válido e inválido.
- Saque com saldo suficiente e insuficiente.
- Transferência válida.
- Transferência para mesma conta.
- Rollback em falha.
- Repetição da chave de idempotência.
- Paginação do extrato.

Meta: regras críticas cobertas por testes, e não apenas controllers.

### Etapa 7 — Documentação e demonstração

1. Documentar como executar.
2. Criar collection Postman.
3. Inserir diagrama de arquitetura.
4. Adicionar exemplos de request e response.
5. Gravar GIF ou vídeo curto usando Swagger.
6. Publicar uma versão demonstrativa apenas com dados fictícios.

## 9. Critérios para considerar pronto

- `docker compose up` inicia banco e aplicação.
- Migrations funcionam em banco vazio.
- Swagger lista todos os endpoints.
- Transferências são atômicas.
- Valores monetários usam `BigDecimal`.
- Testes principais passam no CI.
- Nenhum segredo está no repositório.
- README possui arquitetura, execução, exemplos e decisões técnicas.

## 10. Sequência de commits sugerida

```text
chore: bootstrap spring boot project
feat: add customer domain
feat: add bank account domain
feat: implement deposits and withdrawals
feat: implement transactional transfers
feat: add statement pagination
fix: prevent duplicate operations with idempotency
 test: cover banking business rules
docs: add api examples and architecture
ci: run build and tests on pull requests
```

## 11. Evoluções de nível sênior

- Autenticação JWT e papéis de acesso.
- Event-driven architecture com RabbitMQ.
- Outbox Pattern para publicação confiável de eventos.
- Limites transacionais e análise de risco.
- Métricas com Micrometer e Prometheus.
- Testes de contrato.
- Rate limiting.
- Auditoria de alterações administrativas.

## 12. Evidências para o portfólio

Apresentar prints do Swagger, diagrama de entidades, cobertura de testes, pipeline CI e um estudo de caso explicando por que transação atômica, idempotência, precisão monetária e concorrência são essenciais em sistemas financeiros.