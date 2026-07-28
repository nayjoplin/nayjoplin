# Personal Investment Portfolio Analyzer — Documentação de Implementação

## 1. Resultado esperado

Construir uma ferramenta de análise de carteira que consolide ativos, compras, vendas, aportes e preços simulados para calcular patrimônio, rentabilidade, alocação, concentração e cenários de rebalanceamento.

O projeto é educacional e não oferece recomendação financeira. Toda demonstração deve usar dados fictícios, simulados ou publicamente licenciados.

## 2. Pergunta de negócio

Como transformar movimentações dispersas de uma carteira em uma visão clara de evolução patrimonial, retorno, diversificação e exposição ao risco?

## 3. Escopo do MVP

- Cadastro de ativos e classes.
- Importação de compras, vendas e aportes.
- Histórico de preços simulado.
- Cálculo de posição e preço médio.
- Patrimônio total e evolução.
- Rentabilidade por ativo e carteira.
- Alocação por classe, ativo e setor.
- Indicadores de concentração.
- Simulador de rebalanceamento.
- Dashboard em Streamlit ou Power BI.

Fora do MVP:

- Ordem de compra ou venda real.
- Custódia de investimentos.
- Recomendação individualizada.
- Previsão garantida de retorno.
- Integração com corretora real.

## 4. Stack recomendada

- Python 3.12.
- Pandas e NumPy.
- SQL e PostgreSQL ou SQLite.
- Matplotlib.
- Streamlit ou Power BI.
- Pytest.
- Decimal para valores monetários quando usado fora de dataframes.

## 5. Estrutura recomendada

```text
personal-investment-portfolio-analyzer
├── app
├── data
│   ├── raw
│   └── processed
├── docs
├── notebooks
├── reports
├── sql
├── src
│   ├── positions
│   ├── returns
│   ├── risk
│   └── scenarios
├── tests
└── README.md
```

## 6. Modelo de dados

### asset

- asset_id.
- ticker.
- asset_name.
- asset_class.
- sector.
- currency.
- risk_level.

### transaction

- transaction_id.
- portfolio_id.
- asset_id.
- type: BUY, SELL, DIVIDEND, FEE ou CONTRIBUTION.
- quantity.
- unit_price.
- total_amount.
- transaction_date.

### price_history

- asset_id.
- price_date.
- close_price.

### target_allocation

- portfolio_id.
- asset_class.
- target_percentage.

## 7. Regras de cálculo

### Posição atual

```text
quantidade atual = compras - vendas
valor de mercado = quantidade atual × último preço
```

### Preço médio

Definir e documentar o método. Para o MVP, usar média ponderada das compras e ajustar a posição após vendas sem reescrever o histórico.

### Rentabilidade do ativo

Separar:

- ganho não realizado.
- ganho realizado.
- proventos.
- taxas.

### Rentabilidade da carteira

Implementar primeiro retorno simples por período. Como evolução, adicionar TWR e XIRR para tratar aportes e retiradas corretamente.

### Concentração

- maior exposição por ativo.
- maior exposição por classe.
- índice Herfindahl-Hirschman simplificado.
- percentual em ativos classificados como maior risco.

Todos os cálculos devem informar premissas e limitações.

## 8. Passo a passo de desenvolvimento

### Etapa 1 — Preparação

1. Criar ambiente Python.
2. Montar diretórios.
3. Definir classes de ativos usadas na simulação.
4. Criar dicionário de dados.
5. Configurar seed para geração dos preços.
6. Criar aviso explícito de que o projeto não é recomendação financeira.

### Etapa 2 — Dados simulados

1. Criar catálogo de ativos fictícios ou identificados como exemplos.
2. Gerar preços diários ou mensais.
3. Criar movimentações de compra, venda, aporte e provento.
4. Incluir cenários de venda parcial e mudança de alocação.
5. Validar que não existam vendas acima da posição.

Critério de aceite: a carteira pode ser reconstruída integralmente a partir das movimentações.

### Etapa 3 — Persistência

1. Criar tabelas SQL.
2. Adicionar restrições de quantidade e valor.
3. Criar índices por carteira, ativo e data.
4. Implementar carga de dados.
5. Criar queries para posição e histórico.

### Etapa 4 — Motor de posições

1. Ordenar transações por data.
2. Calcular quantidade acumulada por ativo.
3. Calcular preço médio.
4. Separar ganhos realizados e não realizados.
5. Incorporar proventos e taxas.
6. Criar snapshot diário ou mensal da carteira.

Testar especialmente compras sucessivas, vendas parciais e zeragem da posição.

### Etapa 5 — Indicadores

Calcular:

- patrimônio atual.
- total aportado.
- ganho realizado.
- ganho não realizado.
- proventos.
- retorno por ativo.
- retorno por classe.
- evolução patrimonial.
- concentração.
- alocação atual versus alvo.

### Etapa 6 — Risco e diversificação

1. Calcular volatilidade histórica simplificada.
2. Calcular drawdown máximo.
3. Criar matriz de correlação entre retornos.
4. Identificar concentração excessiva por regra configurável.
5. Classificar exposição por nível de risco.
6. Documentar que volatilidade histórica não representa todo o risco futuro.

### Etapa 7 — Simulador de rebalanceamento

Entrada:

- patrimônio disponível para novo aporte.
- alocação alvo.
- restrições opcionais.

Saída:

- valor sugerido por classe para aproximar a meta.
- diferença atual versus alvo.
- nova alocação simulada.

O resultado deve ser chamado de simulação matemática, não recomendação.

### Etapa 8 — Dashboard

Páginas sugeridas:

1. Resumo patrimonial.
2. Performance.
3. Alocação e concentração.
4. Risco e drawdown.
5. Movimentações.
6. Simulador de rebalanceamento.
7. Metodologia e limitações.

### Etapa 9 — Testes

Testar:

- Compra inicial.
- Múltiplas compras.
- Venda parcial.
- Venda total.
- Venda maior que posição.
- Cálculo de preço médio.
- Ganho realizado.
- Inclusão de proventos e taxas.
- Datas sem preço.
- Soma da alocação igual a 100%.
- Rebalanceamento com patrimônio zero.

### Etapa 10 — Documentação e publicação

1. Criar diagrama das tabelas.
2. Documentar fórmulas.
3. Adicionar exemplos calculados manualmente.
4. Inserir prints do dashboard.
5. Publicar somente com dados fictícios.
6. Configurar CI.
7. Listar limitações e evoluções.

## 9. Critérios para considerar pronto

- A posição é reconstruída por movimentações.
- Fórmulas possuem exemplos verificáveis.
- Venda acima da posição é bloqueada.
- Rentabilidade separa fluxo de caixa e valorização.
- Dashboard mostra alocação, retorno e risco.
- Simulação não é apresentada como recomendação.
- Testes cobrem os casos financeiros principais.
- Dados e premissas estão documentados.

## 10. Sequência de commits sugerida

```text
chore: create portfolio analyzer structure
feat: generate synthetic assets and prices
data: add transaction model and dictionary
feat: implement position reconstruction
feat: calculate average price and realized gains
feat: add portfolio performance indicators
feat: add concentration and risk metrics
feat: implement rebalancing simulation
feat: build portfolio dashboard
test: cover transaction and return calculations
docs: document formulas and limitations
ci: run financial calculation tests
```

## 11. Evoluções de nível sênior

- TWR e XIRR.
- Benchmark e attribution analysis.
- Suporte a moedas diferentes.
- Tratamento de eventos corporativos.
- Otimização de carteira apenas como estudo matemático.
- API de preços com cache e fallback.
- Versionamento de snapshots.
- Monitoramento de qualidade dos preços.

## 12. Evidências para o portfólio

Apresentar modelo de dados, exemplos manuais de cálculo, testes de venda parcial, evolução patrimonial, análise de concentração, dashboard e uma explicação transparente sobre premissas e limites da ferramenta.