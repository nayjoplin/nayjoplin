# Guia Mestre de Desenvolvimento do Portfólio

## 1. Como usar esta documentação

Cada projeto possui dois documentos:

- `README.md`: apresentação rápida para recrutadores.
- `DOCUMENTATION.md`: manual técnico e passo a passo de construção.

Não desenvolva os oito projetos ao mesmo tempo. Trabalhe em ciclos e só avance quando o projeto atual tiver evidências visuais, testes e instruções de execução.

## 2. Ordem estratégica recomendada

### Ciclo 1 — Engenharia de software

1. [Digital Banking API](./digital-banking-api/DOCUMENTATION.md)

Objetivo: provar Java, Spring Boot, API REST, PostgreSQL, transações, testes e documentação.

### Ciclo 2 — Dados e risco

2. [Credit Risk Scoring](./credit-risk-scoring/DOCUMENTATION.md)
3. [Financial Data Pipeline](./financial-data-pipeline/DOCUMENTATION.md)
4. [Fraud Detection System](./fraud-detection-system/DOCUMENTATION.md)

Objetivo: provar Python, SQL, qualidade de dados, modelos, risco, explicabilidade e engenharia de dados.

### Ciclo 3 — Investimentos

5. [Personal Investment Portfolio Analyzer](./personal-investment-portfolio-analyzer/DOCUMENTATION.md)

Objetivo: demonstrar cálculos financeiros, visualização, modelagem de movimentações e cuidado com premissas.

### Ciclo 4 — Beauty Tech

6. [Beauty Sales Intelligence Dashboard](./beauty-sales-intelligence-dashboard/DOCUMENTATION.md)
7. [CRM Beauty Retention Engine](./crm-beauty-retention-engine/DOCUMENTATION.md)
8. [AI Beauty Product Recommendation](./ai-beauty-product-recommendation/DOCUMENTATION.md)

Objetivo: transformar experiência real em beleza, marketing e growth em produtos tecnológicos diferenciados.

## 3. Fases obrigatórias de cada projeto

### Fase A — Descoberta

- Definir problema e usuário.
- Escrever perguntas de negócio.
- Definir MVP e itens fora de escopo.
- Identificar riscos e limitações.
- Definir critérios de aceite.

Saída: README inicial e backlog priorizado.

### Fase B — Design técnico

- Criar arquitetura.
- Modelar dados.
- Definir contratos, regras e métricas.
- Definir estrutura de pastas.
- Registrar decisões em ADRs quando relevantes.

Saída: diagramas e documentação técnica.

### Fase C — Fundação

- Configurar ambiente.
- Criar banco ou dataset.
- Configurar lint, testes e CI.
- Criar `.env.example`.
- Garantir execução com comandos claros.

Saída: projeto executável, mesmo sem funcionalidades completas.

### Fase D — MVP funcional

- Implementar o fluxo principal de ponta a ponta.
- Testar regras críticas.
- Tratar erros.
- Gerar dados de demonstração.
- Validar resultados manualmente.

Saída: demonstração funcional.

### Fase E — Qualidade

- Aumentar cobertura das regras críticas.
- Revisar segurança e privacidade.
- Melhorar logs e observabilidade.
- Verificar acessibilidade e responsividade quando houver interface.
- Remover segredos, arquivos temporários e código morto.

Saída: projeto confiável e revisável.

### Fase F — Storytelling

- Criar screenshots e GIF.
- Escrever estudo de caso.
- Explicar decisões e trade-offs.
- Informar métricas reais do projeto.
- Separar resultado observado de hipótese.
- Documentar limitações e próximos passos.

Saída: case pronto para recrutadores.

## 4. Estrutura padrão

```text
project-name
├── README.md
├── DOCUMENTATION.md
├── CHANGELOG.md
├── .env.example
├── .github/workflows
├── data ou database
├── docs
│   ├── architecture
│   ├── adr
│   └── images
├── src
├── tests
├── dashboard, app ou frontend
└── scripts
```

Nem todo projeto precisa de todas as pastas. A estrutura deve refletir o que realmente existe.

## 5. Definition of Done

Um projeto só pode ser marcado como concluído quando:

- O problema está claro.
- O MVP funciona de ponta a ponta.
- Dados são fictícios, públicos ou devidamente licenciados.
- A arquitetura está documentada.
- As regras principais possuem testes.
- Existe CI executando validações.
- O projeto pode ser iniciado seguindo o README.
- Há prints, GIF ou demonstração.
- Resultados e métricas não foram inventados.
- Limitações estão explícitas.
- O repositório não contém segredos.
- O case explica decisões, não apenas ferramentas.

## 6. Modelo de estudo de caso

Cada case do site deve seguir:

1. Contexto.
2. Problema.
3. Usuários ou stakeholders.
4. Meu papel.
5. Restrições.
6. Hipóteses.
7. Arquitetura ou metodologia.
8. Principais decisões.
9. Implementação.
10. Testes e validação.
11. Resultados observados.
12. Limitações.
13. Aprendizados.
14. Próximas evoluções.

## 7. Estratégia de branches e commits

- Uma issue por funcionalidade relevante.
- Uma branch curta por entrega.
- Pull Requests com contexto e evidências.
- Commits pequenos e descritivos.
- Nunca fazer um único commit chamado `projeto pronto`.

Padrões úteis:

```text
feat: nova funcionalidade
fix: correção
refactor: melhoria interna
 test: testes
docs: documentação
chore: configuração
ci: automação
 data: dataset ou transformação
```

## 8. Priorização para candidaturas

### Para back-end Java

Destacar primeiro Digital Banking API.

### Para dados e BI

Destacar Financial Data Pipeline, Beauty Sales Intelligence e Credit Risk Scoring.

### Para bancos e fintechs

Destacar Digital Banking API, Credit Risk Scoring e Fraud Detection System.

### Para produto digital

Destacar AI Beauty Product Recommendation e CRM Beauty Retention Engine.

### Para empresas de beleza e consumo

Destacar Beauty Sales Intelligence, CRM Retention e AI Recommendation.

## 9. Regra de senioridade

Senioridade no portfólio não é colocar `sênior` no título. É demonstrar:

- Clareza sobre o problema.
- Boas decisões técnicas.
- Conhecimento dos riscos.
- Capacidade de priorizar.
- Qualidade e testes.
- Trade-offs explícitos.
- Métricas confiáveis.
- Comunicação para áreas técnicas e de negócio.
- Consistência entre código, documentação e demonstração.

## 10. Checklist antes de publicar um projeto no site

- [ ] Link do repositório funciona.
- [ ] Demonstração funciona.
- [ ] README começa pelo problema e resultado.
- [ ] Stack corresponde ao código existente.
- [ ] Prints estão legíveis.
- [ ] Dados não expõem pessoas ou empresas.
- [ ] Testes e CI estão verdes.
- [ ] Métricas são verdadeiras e contextualizadas.
- [ ] Meu papel está descrito.
- [ ] Existe uma chamada clara para ver código, demo e documentação.