# Digital Banking API

Projeto de back-end para simular operações bancárias essenciais, com foco em Java, Spring Boot, APIs REST, regras de negócio, persistência de dados e documentação técnica.

## Objetivo

Criar uma API bancária que demonstre fundamentos de engenharia de software aplicados ao mercado financeiro, incluindo cadastro de clientes, contas, movimentações, validações e histórico transacional.

## Problema de negócio

Bancos, fintechs e plataformas financeiras precisam de sistemas confiáveis para registrar clientes, controlar contas, validar operações e manter rastreabilidade de transações. Este projeto simula uma camada de back-end para esse tipo de operação.

## Funcionalidades previstas

- Cadastro de clientes.
- Cadastro de contas bancárias.
- Consulta de saldo.
- Depósito.
- Saque.
- Transferência entre contas.
- Histórico de transações.
- Validação de regras de negócio.
- Tratamento de erros.
- Documentação de endpoints.

## Stack planejada

- Java
- Spring Boot
- Spring Web
- Spring Data JPA
- PostgreSQL
- Maven
- Git e GitHub
- Swagger/OpenAPI
- JUnit

## Arquitetura sugerida

```text
src/main/java
├── controller
├── service
├── repository
├── model
├── dto
├── exception
└── config
```

## Regras de negócio iniciais

- Uma conta deve pertencer a um cliente cadastrado.
- Não deve ser possível sacar valor maior que o saldo disponível.
- Transferências devem registrar saída na conta de origem e entrada na conta de destino.
- Toda movimentação deve gerar histórico transacional.
- Operações inválidas devem retornar mensagens claras de erro.

## Entregáveis

- API REST funcional.
- Banco de dados relacional.
- Documentação Swagger.
- Collection de testes via Postman/Insomnia.
- README completo com instruções de execução.
- Testes unitários para regras principais.

## Roadmap

- [ ] Criar estrutura Spring Boot.
- [ ] Modelar entidades Cliente, Conta e Transação.
- [ ] Criar endpoints principais.
- [ ] Implementar validações.
- [ ] Configurar PostgreSQL.
- [ ] Adicionar Swagger.
- [ ] Criar testes unitários.
- [ ] Documentar execução local.

## Diferencial para recrutadores

Este projeto demonstra base sólida em back-end, APIs, modelagem de dados, regras de negócio e raciocínio aplicado a sistemas financeiros.
