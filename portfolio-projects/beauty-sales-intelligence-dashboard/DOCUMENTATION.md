# Beauty Sales Intelligence Dashboard — Documentação de Implementação

## 1. Resultado esperado

Construir uma solução de inteligência comercial para uma marca fictícia de beleza, reunindo vendas, clientes, produtos, campanhas e canais em um modelo analítico confiável e um dashboard executivo no Power BI.

O projeto deve demonstrar que você não apenas cria gráficos: você define métricas, modela dados, identifica causas, transforma análise em recomendação e conecta marketing, e-commerce, CRM e produto.

## 2. Perguntas de negócio

- O que está impulsionando ou reduzindo a receita?
- Quais categorias, linhas e SKUs possuem maior participação?
- Quais canais geram mais pedidos, receita e recompra?
- Qual é a evolução do ticket médio?
- Quais clientes e produtos sustentam o crescimento?
- Onde existem oportunidades de cross-sell, retenção e otimização de campanha?

## 3. Escopo do MVP

- Dataset sintético de pedidos de beauty e e-commerce.
- Limpeza e análise em Python.
- Banco analítico ou arquivos tratados.
- Modelo estrela.
- Consultas SQL.
- Medidas DAX documentadas.
- Dashboard Power BI com cinco páginas.
- Relatório executivo de insights e recomendações.
- Documentação de métricas e qualidade.

Fora do MVP:

- Dados reais de clientes.
- Integração com plataforma comercial real.
- Previsão de vendas obrigatória.
- Atribuição perfeita de marketing.

## 4. Stack recomendada

- Python 3.12.
- Pandas e NumPy.
- SQL e PostgreSQL ou SQLite.
- Power BI.
- Excel apenas para conferência, não como fonte principal de lógica.
- Pytest para regras de transformação.

## 5. Estrutura recomendada

```text
beauty-sales-intelligence-dashboard
├── dashboard
├── data
│   ├── raw
│   └── processed
├── docs
├── notebooks
├── reports
├── sql
├── src
├── tests
└── README.md
```

## 6. Modelo de dados

### Dimensões

- dim_date.
- dim_customer.
- dim_product.
- dim_channel.
- dim_campaign.
- dim_region.

### Fatos

- fact_order_item: granularidade de item do pedido.
- fact_marketing_spend: granularidade de campanha, canal e data.

Campos essenciais de `fact_order_item`:

- order_item_id.
- order_id.
- date_key.
- customer_key.
- product_key.
- channel_key.
- campaign_key opcional.
- quantity.
- gross_revenue.
- discount_amount.
- net_revenue.
- cost_amount.
- shipping_amount.
- status.

## 7. Dataset sintético

Criar pelo menos:

- 12 a 18 meses de histórico.
- 5.000 clientes.
- 30 a 60 produtos.
- categorias como hair care, skincare, styling e kits.
- canais como e-commerce, marketplace, social commerce e distribuidores.
- campanhas sazonais.
- pedidos cancelados e devolvidos.
- clientes novos e recorrentes.

Usar nomes e dados totalmente fictícios. Inserir sazonalidade e padrões intencionais para que o dashboard tenha histórias analisáveis.

## 8. Definições de métricas

- Receita bruta: soma do valor antes de descontos e devoluções.
- Receita líquida: receita bruta menos descontos, cancelamentos e devoluções conforme regra documentada.
- Pedidos válidos: pedidos concluídos, excluindo cancelados.
- Ticket médio: receita líquida / pedidos válidos.
- Clientes ativos: clientes com pedido válido no período.
- Taxa de recompra: clientes com dois ou mais pedidos / clientes compradores.
- Itens por pedido: quantidade de itens / pedidos válidos.
- Margem bruta: receita líquida menos custo dos produtos.
- ROAS simulado: receita atribuída / investimento de mídia.
- Participação da categoria: receita da categoria / receita total.

Criar `METRICS_DICTIONARY.md` com fórmula, granularidade, filtros e responsável conceitual.

## 9. Passo a passo de desenvolvimento

### Etapa 1 — Planejamento analítico

1. Definir persona do dashboard: diretoria, growth, comercial e produto.
2. Criar perguntas que cada página deve responder.
3. Definir KPIs antes de desenhar gráficos.
4. Documentar regras de receita, cancelamento e recompra.
5. Criar wireframe simples no Figma ou papel.

### Etapa 2 — Geração dos dados

1. Criar catálogo de produtos.
2. Gerar clientes fictícios.
3. Gerar pedidos e itens.
4. Adicionar canais e campanhas.
5. Inserir custos, descontos, cancelamentos e devoluções.
6. Criar sazonalidade e ações promocionais.
7. Salvar seed e script de geração.
8. Criar dicionário de dados.

### Etapa 3 — Qualidade e limpeza

1. Verificar duplicidades.
2. Validar IDs e relacionamentos.
3. Corrigir tipos e datas.
4. Padronizar canais e categorias.
5. Validar valores negativos.
6. Tratar cancelamentos e devoluções.
7. Gerar relatório de qualidade.

Critério de aceite: totais tratados podem ser reconciliados com a origem simulada.

### Etapa 4 — Modelo estrela

1. Criar dimensões.
2. Criar tabela fato no nível de item.
3. Criar dimensão calendário completa.
4. Garantir relacionamento um-para-muitos.
5. Evitar relacionamento bidirecional sem necessidade.
6. Criar chaves substitutas quando usar banco.
7. Documentar granularidade de cada tabela.

### Etapa 5 — Análise em Python e SQL

Criar análises para:

- evolução mensal.
- decomposição de receita.
- ranking de produtos.
- performance de categorias.
- canais e campanhas.
- clientes novos versus recorrentes.
- recompra por coorte.
- descontos versus margem.
- produtos comprados juntos.

Cada análise deve terminar com uma conclusão e uma recomendação possível.

### Etapa 6 — Medidas DAX

Criar medidas para:

- receita bruta e líquida.
- pedidos.
- ticket médio.
- margem e margem percentual.
- clientes ativos.
- novos clientes.
- recorrentes.
- taxa de recompra.
- crescimento mês contra mês.
- crescimento ano contra ano quando houver base.
- participação por categoria.
- ROAS simulado.

Organizar medidas em tabela própria e usar nomes de negócio.

### Etapa 7 — Dashboard

#### Página 1 — Executive Overview

KPIs, tendência, meta simulada, principais variações e resumo de oportunidades.

#### Página 2 — Produtos e categorias

Ranking, margem, participação, curva ABC e evolução dos principais SKUs.

#### Página 3 — Clientes e retenção

Novos versus recorrentes, recompra, coortes, ticket por segmento e frequência.

#### Página 4 — Canais e campanhas

Receita, pedidos, investimento, ROAS e comparação entre canais.

#### Página 5 — Diagnóstico e oportunidades

Alertas de queda, produtos com desconto alto, categorias com potencial e recomendações priorizadas.

### Etapa 8 — Storytelling executivo

1. Selecionar cinco insights realmente importantes.
2. Informar evidência e impacto potencial.
3. Separar fato, hipótese e recomendação.
4. Criar uma matriz impacto versus esforço.
5. Não inventar resultado realizado. Usar termos como oportunidade estimada ou hipótese.

### Etapa 9 — Testes e validação

Testar:

- Soma de receita.
- Quantidade de pedidos únicos.
- Exclusão de cancelados.
- Cálculo de recompra.
- Margem.
- Relações do modelo.
- Filtros cruzados.
- Período sem vendas.
- Comparação de números Python, SQL e Power BI.

### Etapa 10 — Publicação

1. Exportar imagens das páginas.
2. Criar GIF ou vídeo de navegação.
3. Salvar `.pbix` quando possível e imagens para visualização rápida.
4. Publicar dataset reduzido e anonimizado.
5. Adicionar relatório executivo em PDF ou Markdown.
6. Configurar testes Python no CI.

## 10. Critérios para considerar pronto

- Métricas estão definidas e reconciliadas.
- Modelo estrela está correto.
- Dashboard responde perguntas de negócio.
- Insights possuem evidência.
- Recomendações não confundem hipótese com resultado.
- Dados são fictícios e reproduzíveis.
- Projeto possui prints, vídeo, SQL, notebook e relatório.
- README explica o seu papel e as decisões.

## 11. Sequência de commits sugerida

```text
chore: create beauty sales analytics structure
data: generate synthetic beauty commerce dataset
data: add dictionary and quality checks
feat: build star schema and sql transformations
feat: add exploratory sales analysis
feat: create customer and cohort metrics
feat: add documented dax measures
feat: build executive power bi dashboard
test: reconcile revenue and retention metrics
docs: add insights and recommendations report
ci: validate analytics transformations
```

## 12. Evoluções de nível sênior

- Forecast com intervalos de confiança.
- Incremental refresh.
- Row-level security.
- dbt para transformações.
- Modelo de propensão à recompra.
- Market basket analysis.
- Alertas automatizados de anomalia.
- Camada semântica e governança de métricas.

## 13. Evidências para o portfólio

Mostrar modelo estrela, dicionário de métricas, reconciliação dos números, páginas do dashboard e um case com problema, processo, decisões, insights e recomendações. A senioridade deve aparecer na qualidade da análise, não na quantidade de gráficos.