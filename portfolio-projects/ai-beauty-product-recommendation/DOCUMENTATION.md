# AI Beauty Product Recommendation — Documentação de Implementação

## 1. Resultado esperado

Construir uma aplicação de recomendação de produtos de beleza que receba um perfil fictício, aplique filtros de segurança e compatibilidade, calcule um ranking explicável e apresente produtos com justificativas claras.

O MVP deve começar com um motor baseado em conteúdo e regras. Machine learning entra somente depois que o baseline estiver funcionando, testado e documentado.

## 2. Pergunta de produto

Como reduzir a insegurança na escolha de produtos e facilitar a descoberta de itens compatíveis com necessidades, preferências e objetivos, sem apresentar a ferramenta como diagnóstico profissional?

## 3. Escopo do MVP

- Catálogo fictício de produtos.
- Questionário de perfil.
- Filtros obrigatórios e preferências.
- Motor de ranking baseado em atributos.
- Top 3 ou Top 5 recomendações.
- Explicação dos critérios.
- API FastAPI.
- Interface web responsiva.
- Registro anonimizado de eventos de recomendação.
- Testes de regras e endpoints.

Fora do MVP:

- Diagnóstico médico ou dermatológico.
- Promessa de tratamento.
- Uso de imagem facial ou corporal.
- Dados sensíveis reais.
- Geração de recomendação sem explicação.
- Integração de compra real.

## 4. Stack recomendada

- Python 3.12.
- FastAPI.
- Pydantic.
- SQLAlchemy.
- PostgreSQL ou SQLite no desenvolvimento.
- Alembic.
- HTML, CSS e JavaScript ou React em evolução.
- Pytest.
- Docker Compose.
- Figma.

## 5. Arquitetura

```text
Interface web
     ↓
FastAPI routes
     ↓
Profile validation
     ↓
Eligibility filters
     ↓
Recommendation scoring
     ↓
Explanation builder
     ↓
Product repository / PostgreSQL
```

Estrutura:

```text
ai-beauty-product-recommendation
├── backend
│   ├── app
│   │   ├── api
│   │   ├── core
│   │   ├── models
│   │   ├── repositories
│   │   ├── schemas
│   │   └── services
│   └── tests
├── frontend
├── data
├── docs
├── prototypes
└── README.md
```

## 6. Modelo de dados

### product

- product_id.
- name.
- brand fictícia.
- category.
- hair_types ou skin_types compatíveis.
- concerns atendidas.
- benefits.
- ingredients ou ativos informativos.
- excluded_preferences.
- price_range.
- usage_frequency.
- active.

### user_profile

Para o MVP, o perfil pode existir apenas durante a requisição:

- profile_type.
- primary_goal.
- concerns.
- preferred_categories.
- excluded_ingredients.
- fragrance_preference.
- price_range.
- routine_complexity.

### recommendation_event

- event_id.
- anonymous_session_id.
- profile_summary sem dados sensíveis.
- recommended_product_ids.
- engine_version.
- created_at.

### feedback_event

- event_id.
- recommendation_event_id.
- product_id.
- action: VIEW, LIKE, DISLIKE ou SELECT.
- created_at.

## 7. Estratégia de recomendação do MVP

### Fase 1 — Filtros eliminatórios

Remover produtos que:

- não atendem ao tipo de perfil.
- pertencem a categoria excluída.
- contêm atributo que o usuário marcou como evitar.
- estão inativos.
- estão fora de uma restrição obrigatória.

### Fase 2 — Score de compatibilidade

Exemplo de pesos:

- objetivo principal: 35 pontos.
- necessidade específica: 25 pontos.
- compatibilidade com perfil: 20 pontos.
- preferência de categoria: 10 pontos.
- faixa de preço: 5 pontos.
- rotina e frequência: 5 pontos.

### Fase 3 — Diversificação

Evitar que o Top 5 seja composto por itens quase idênticos. Aplicar regra para variar categoria, benefício secundário ou faixa de preço quando houver opções compatíveis.

### Fase 4 — Explicação

Cada recomendação deve retornar:

- score.
- principais motivos.
- preferências atendidas.
- alertas ou limitações.
- versão do motor.

## 8. Endpoints do MVP

```text
GET    /api/v1/products
GET    /api/v1/products/{id}
POST   /api/v1/recommendations
POST   /api/v1/recommendations/{eventId}/feedback
GET    /api/v1/health
```

Exemplo de request:

```json
{
  "profileType": "CURLY_HAIR",
  "primaryGoal": "DEFINITION",
  "concerns": ["FRIZZ", "DRYNESS"],
  "preferredCategories": ["LEAVE_IN", "MASK"],
  "excludedIngredients": [],
  "priceRange": "MEDIUM",
  "routineComplexity": "SIMPLE"
}
```

Exemplo de item da resposta:

```json
{
  "productId": "product-001",
  "name": "Definition Leave-in",
  "score": 92,
  "reasons": [
    "Compatível com o perfil informado",
    "Atende ao objetivo de definição",
    "Relaciona-se às necessidades de frizz e ressecamento"
  ],
  "engineVersion": "rules-v1"
}
```

## 9. Passo a passo de desenvolvimento

### Etapa 1 — Descoberta de produto

1. Definir persona e problema.
2. Criar jornada do questionário à recomendação.
3. Definir quais perguntas realmente alteram o ranking.
4. Remover perguntas sem uso claro.
5. Criar critérios de sucesso: conclusão do questionário, clique e seleção simulada.
6. Adicionar aviso de que a ferramenta não substitui avaliação profissional.

### Etapa 2 — Catálogo

1. Criar 30 a 50 produtos fictícios.
2. Definir taxonomia de categorias, objetivos, necessidades e perfis.
3. Criar dicionário de atributos.
4. Validar consistência dos valores.
5. Criar script de seed.
6. Evitar alegações médicas e promessas absolutas.

Critério de aceite: cada produto possui atributos suficientes para ser filtrado, ranqueado e explicado.

### Etapa 3 — Banco e API base

1. Criar projeto FastAPI.
2. Configurar SQLAlchemy e Alembic.
3. Criar tabelas.
4. Criar repository de produtos.
5. Implementar endpoints de catálogo.
6. Adicionar validação Pydantic.
7. Documentar erros na OpenAPI.

### Etapa 4 — Filtros

1. Criar função de elegibilidade.
2. Aplicar filtros obrigatórios antes do score.
3. Registrar motivo de exclusão apenas para diagnóstico interno.
4. Testar combinações sem resultado.
5. Criar fallback seguro: ampliar preferências opcionais sem ignorar restrições obrigatórias.

### Etapa 5 — Ranking

1. Criar pesos configuráveis.
2. Calcular contribuição de cada critério.
3. Normalizar score para 0 a 100.
4. Ordenar por score.
5. Aplicar desempate estável.
6. Diversificar a lista final.
7. Registrar versão das regras.

### Etapa 6 — Explicabilidade

1. Transformar contribuições em reason codes.
2. Criar textos curtos e objetivos.
3. Informar quando uma preferência não pôde ser atendida.
4. Evitar textos que pareçam diagnóstico.
5. Testar se a explicação corresponde ao score calculado.

### Etapa 7 — Interface

Telas sugeridas:

1. Apresentação e aviso.
2. Questionário em etapas.
3. Estado de carregamento.
4. Lista de recomendações.
5. Detalhe do produto.
6. Feedback simples.
7. Metodologia.

Requisitos de UX:

- Indicador de progresso.
- Botão voltar sem perder respostas.
- Campos acessíveis por teclado.
- Contraste adequado.
- Linguagem simples.
- Layout responsivo.

### Etapa 8 — Feedback e analytics

1. Gerar session ID anônimo.
2. Registrar produtos recomendados.
3. Registrar visualização, like, dislike e seleção.
4. Não registrar nome, e-mail ou imagem.
5. Criar métricas de produto simuladas.

Métricas:

- taxa de conclusão.
- recomendações sem resultado.
- clique por posição.
- seleção por categoria.
- feedback positivo.
- diversidade da lista.

### Etapa 9 — Testes

Testar:

- Perfil válido e inválido.
- Restrição obrigatória.
- Produto inativo.
- Score máximo e mínimo.
- Empate.
- Diversificação.
- Fallback sem quebrar restrições.
- Reason codes.
- Endpoint com catálogo vazio.
- Persistência do evento sem dados sensíveis.

### Etapa 10 — Protótipo e publicação

1. Criar wireframe no Figma.
2. Validar jornada com cinco cenários fictícios.
3. Adicionar screenshots e GIF.
4. Criar Docker Compose.
5. Configurar CI.
6. Publicar API e interface com dados fictícios.
7. Documentar versão do motor e limitações.

## 10. Critérios para considerar pronto

- Catálogo e taxonomia estão documentados.
- Restrições obrigatórias são respeitadas.
- Ranking é determinístico e explicável.
- Lista final possui diversidade controlada.
- API está documentada.
- Interface é responsiva e acessível.
- Eventos não contêm dados sensíveis.
- Testes passam no CI.
- A ferramenta não faz diagnóstico ou promessa clínica.

## 11. Sequência de commits sugerida

```text
chore: create beauty recommendation structure
data: add product taxonomy and synthetic catalog
feat: create product api and database models
feat: implement eligibility filters
feat: add configurable recommendation score
feat: add recommendation explanations
feat: diversify ranked products
feat: build responsive questionnaire interface
feat: add anonymous feedback events
test: cover filters ranking and api
docs: add product methodology and limitations
ci: validate recommendation engine
```

## 12. Evoluções de nível sênior

- Recomendação híbrida com comportamento agregado.
- Embeddings de descrições, com avaliação contra baseline.
- A/B test do questionário.
- Feature flags para pesos.
- Model registry.
- Monitoramento de cobertura e diversidade.
- Painel de gestão do catálogo.
- API rate limiting e cache.

## 13. Evidências para o portfólio

Apresentar jornada, taxonomia, arquitetura, contrato da API, testes, exemplos de score e explicação, protótipo e métricas. O diferencial é mostrar que a IA começa com um problema bem definido e um baseline confiável, não com uma tecnologia escolhida antes da necessidade.