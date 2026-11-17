# ResolveAI

**Área**  Tech / IA generativa

**Origem**  TCC informado no currículo da Universidade Cruzeiro do Sul, em desenvolvimento

**Status**  Planejamento e desenvolvimento progressivo, sem prometer produção

## Problema a resolver

Responder perguntas sobre documentos mantendo evidências e controle de respostas sem fonte

## Stack proposta

Python, FastAPI, PostgreSQL, pgvector, LLM, pytest

## Capa do portfólio

Tela de chat com resposta e fontes ao lado, fundo off-white e marrom profundo, sem robô

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

### Dia 1 | Definir domínio, usuários, perguntas e métricas de acerto

**Objetivo**  Definir domínio, usuários, perguntas e métricas de acerto

**Na sua máquina**  Ler os arquivos existentes, implementar apenas esta etapa, executar localmente, validar a mudança e escrever em `docs/diario.md` o que funcionou e o que precisou ser corrigido

**Entrega**  Evidência reproduzível, teste pertinente e atualização do README apenas do que estiver funcionando

**Commit sugerido**  `dia 1: definir domínio, usuários, perguntas e métricas de acerto`

### Dia 2 | Criar projeto Python, ambiente virtual, FastAPI e testes básicos

**Objetivo**  Criar projeto Python, ambiente virtual, FastAPI e testes básicos

**Na sua máquina**  Ler os arquivos existentes, implementar apenas esta etapa, executar localmente, validar a mudança e escrever em `docs/diario.md` o que funcionou e o que precisou ser corrigido

**Entrega**  Evidência reproduzível, teste pertinente e atualização do README apenas do que estiver funcionando

**Commit sugerido**  `dia 2: criar projeto python, ambiente virtual, fastapi e testes básicos`

### Dia 3 | Implementar upload e extração de documentos de teste

**Objetivo**  Implementar upload e extração de documentos de teste

**Na sua máquina**  Ler os arquivos existentes, implementar apenas esta etapa, executar localmente, validar a mudança e escrever em `docs/diario.md` o que funcionou e o que precisou ser corrigido

**Entrega**  Evidência reproduzível, teste pertinente e atualização do README apenas do que estiver funcionando

**Commit sugerido**  `dia 3: implementar upload e extração de documentos de teste`

### Dia 4 | Separar trechos e gerar embeddings com armazenamento vetorial

**Objetivo**  Separar trechos e gerar embeddings com armazenamento vetorial

**Na sua máquina**  Ler os arquivos existentes, implementar apenas esta etapa, executar localmente, validar a mudança e escrever em `docs/diario.md` o que funcionou e o que precisou ser corrigido

**Entrega**  Evidência reproduzível, teste pertinente e atualização do README apenas do que estiver funcionando

**Commit sugerido**  `dia 4: separar trechos e gerar embeddings com armazenamento vetorial`

### Dia 5 | Implementar recuperação de contexto e geração de respostas com citações

**Objetivo**  Implementar recuperação de contexto e geração de respostas com citações

**Na sua máquina**  Ler os arquivos existentes, implementar apenas esta etapa, executar localmente, validar a mudança e escrever em `docs/diario.md` o que funcionou e o que precisou ser corrigido

**Entrega**  Evidência reproduzível, teste pertinente e atualização do README apenas do que estiver funcionando

**Commit sugerido**  `dia 5: implementar recuperação de contexto e geração de respostas com ci`

### Dia 6 | Criar interface de chat, histórico e tratamento de erros

**Objetivo**  Criar interface de chat, histórico e tratamento de erros

**Na sua máquina**  Ler os arquivos existentes, implementar apenas esta etapa, executar localmente, validar a mudança e escrever em `docs/diario.md` o que funcionou e o que precisou ser corrigido

**Entrega**  Evidência reproduzível, teste pertinente e atualização do README apenas do que estiver funcionando

**Commit sugerido**  `dia 6: criar interface de chat, histórico e tratamento de erros`

### Dia 7 | Criar conjunto de avaliação de fidelidade, latência e casos sem resposta

**Objetivo**  Criar conjunto de avaliação de fidelidade, latência e casos sem resposta

**Na sua máquina**  Ler os arquivos existentes, implementar apenas esta etapa, executar localmente, validar a mudança e escrever em `docs/diario.md` o que funcionou e o que precisou ser corrigido

**Entrega**  Evidência reproduzível, teste pertinente e atualização do README apenas do que estiver funcionando

**Commit sugerido**  `dia 7: criar conjunto de avaliação de fidelidade, latência e casos sem r`

### Dia 8 | Aplicar segurança, testes, documentação técnica e demonstração

**Objetivo**  Aplicar segurança, testes, documentação técnica e demonstração

**Na sua máquina**  Ler os arquivos existentes, implementar apenas esta etapa, executar localmente, validar a mudança e escrever em `docs/diario.md` o que funcionou e o que precisou ser corrigido

**Entrega**  Evidência reproduzível, teste pertinente e atualização do README apenas do que estiver funcionando

**Commit sugerido**  `dia 8: aplicar segurança, testes, documentação técnica e demonstração`

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
