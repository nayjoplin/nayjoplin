# Hospital Microservices

**Área**  Tech / Backend Java

**Origem**  Projeto de desenvolvimento apresentado no currículo, sem vínculo acadêmico específico confirmado

**Status**  Planejamento e desenvolvimento progressivo, sem prometer produção

## Problema a resolver

Gerenciar serviços hospitalares com separação de responsabilidades, mensageria e autorização

## Stack proposta

Java 21, Spring Boot, MySQL, RabbitMQ, Keycloak, Docker

## Capa do portfólio

Diagrama de arquitetura à direita e tela de agendamentos à esquerda, tons areia e carvão

Formato de publicação 1600 x 900 px com título legível e captura real após implementação

## Arquitetura inicial

- `src/` código da aplicação
- `tests/` testes executáveis
- `docs/arquitetura.md` entidades, fluxos e decisões
- `docs/diario.md` reprodução local e aprendizados
- `assets/` capturas e capa
- `.env.example` variáveis sem segredos

A estrutura será adaptada à stack real, não criar pastas sem uso

## Plano de 8 dias, 4 horas por noite

### Dia 1 | Auditar código atual, levantar endpoints e registrar o que realmente funciona

**Objetivo**  Auditar código atual, levantar endpoints e registrar o que realmente funciona

**Na sua máquina**  Ler os arquivos existentes, implementar apenas esta etapa, executar localmente, validar a mudança e escrever em `docs/diario.md` o que funcionou e o que precisou ser corrigido

**Entrega**  Evidência reproduzível, teste pertinente e atualização do README apenas do que estiver funcionando

**Commit sugerido**  `dia 1: auditar código atual, levantar endpoints e registrar o que realme`

### Dia 2 | Preparar Java, Maven e containers para reprodução local

**Objetivo**  Preparar Java, Maven e containers para reprodução local

**Na sua máquina**  Ler os arquivos existentes, implementar apenas esta etapa, executar localmente, validar a mudança e escrever em `docs/diario.md` o que funcionou e o que precisou ser corrigido

**Entrega**  Evidência reproduzível, teste pertinente e atualização do README apenas do que estiver funcionando

**Commit sugerido**  `dia 2: preparar java, maven e containers para reprodução local`

### Dia 3 | Validar serviços de agendamento e regras de negócio

**Objetivo**  Validar serviços de agendamento e regras de negócio

**Na sua máquina**  Ler os arquivos existentes, implementar apenas esta etapa, executar localmente, validar a mudança e escrever em `docs/diario.md` o que funcionou e o que precisou ser corrigido

**Entrega**  Evidência reproduzível, teste pertinente e atualização do README apenas do que estiver funcionando

**Commit sugerido**  `dia 3: validar serviços de agendamento e regras de negócio`

### Dia 4 | Validar serviços de clínica e centro cirúrgico

**Objetivo**  Validar serviços de clínica e centro cirúrgico

**Na sua máquina**  Ler os arquivos existentes, implementar apenas esta etapa, executar localmente, validar a mudança e escrever em `docs/diario.md` o que funcionou e o que precisou ser corrigido

**Entrega**  Evidência reproduzível, teste pertinente e atualização do README apenas do que estiver funcionando

**Commit sugerido**  `dia 4: validar serviços de clínica e centro cirúrgico`

### Dia 5 | Revisar Gateway, autenticação e permissões

**Objetivo**  Revisar Gateway, autenticação e permissões

**Na sua máquina**  Ler os arquivos existentes, implementar apenas esta etapa, executar localmente, validar a mudança e escrever em `docs/diario.md` o que funcionou e o que precisou ser corrigido

**Entrega**  Evidência reproduzível, teste pertinente e atualização do README apenas do que estiver funcionando

**Commit sugerido**  `dia 5: revisar gateway, autenticação e permissões`

### Dia 6 | Testar comunicação assíncrona e cenários de erro

**Objetivo**  Testar comunicação assíncrona e cenários de erro

**Na sua máquina**  Ler os arquivos existentes, implementar apenas esta etapa, executar localmente, validar a mudança e escrever em `docs/diario.md` o que funcionou e o que precisou ser corrigido

**Entrega**  Evidência reproduzível, teste pertinente e atualização do README apenas do que estiver funcionando

**Commit sugerido**  `dia 6: testar comunicação assíncrona e cenários de erro`

### Dia 7 | Criar testes de integração e coleções de API

**Objetivo**  Criar testes de integração e coleções de API

**Na sua máquina**  Ler os arquivos existentes, implementar apenas esta etapa, executar localmente, validar a mudança e escrever em `docs/diario.md` o que funcionou e o que precisou ser corrigido

**Entrega**  Evidência reproduzível, teste pertinente e atualização do README apenas do que estiver funcionando

**Commit sugerido**  `dia 7: criar testes de integração e coleções de api`

### Dia 8 | Corrigir README, diagramas, evidências e instruções de deploy

**Objetivo**  Corrigir README, diagramas, evidências e instruções de deploy

**Na sua máquina**  Ler os arquivos existentes, implementar apenas esta etapa, executar localmente, validar a mudança e escrever em `docs/diario.md` o que funcionou e o que precisou ser corrigido

**Entrega**  Evidência reproduzível, teste pertinente e atualização do README apenas do que estiver funcionando

**Commit sugerido**  `dia 8: corrigir readme, diagramas, evidências e instruções de deploy`

## Rotina de cada noite

- 40 min entender o conceito e o problema
- 130 min implementar, primeiro com orientação e depois refazer localmente
- 45 min testar e investigar erros
- 25 min registrar evidências, diário e commit

## README final esperado

Problema, público, funcionalidades comprovadas, arquitetura real, instruções de execução, configuração segura, testes, exemplos de entrada e saída, prints, limites e próximos passos

## Critérios antes de publicar como concluído

- Executa do zero em ambiente documentado
- Possui testes passando e evidências
- Não expõe dados reais, credenciais ou informações pessoais
- Não anuncia funções ainda não implementadas
- Está conectado ao case no portfólio
