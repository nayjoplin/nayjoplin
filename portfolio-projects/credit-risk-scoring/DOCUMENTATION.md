# Credit Risk Scoring — Documentação de Implementação

## 1. Resultado esperado

Construir um projeto analítico que classifique solicitações de crédito em faixas de risco, compare uma política baseada em regras com um modelo de machine learning e apresente os resultados em uma aplicação Streamlit.

O projeto é educacional e deve usar somente dados públicos ou simulados. Ele não deve ser apresentado como modelo pronto para conceder crédito real.

## 2. Pergunta de negócio

Como organizar dados financeiros e comportamentais para apoiar uma decisão de crédito mais consistente, explicável e mensurável, reduzindo risco sem excluir bons clientes desnecessariamente?

## 3. Escopo do MVP

- Gerar ou importar dataset anonimizado.
- Realizar análise exploratória.
- Criar score inicial baseado em regras.
- Treinar um modelo baseline de classificação.
- Comparar regras e modelo.
- Criar faixas de risco e sugestão simulada de decisão.
- Exibir métricas e explicações em Streamlit.
- Documentar limitações, vieses e cuidados éticos.

Fora do MVP:

- Consulta a birôs reais.
- Uso de CPF ou dados pessoais reais.
- Decisão automatizada em produção.
- Promessa de aprovação ou limite real.

## 4. Stack recomendada

- Python 3.12.
- Pandas e NumPy.
- Scikit-learn.
- Matplotlib.
- SQL e PostgreSQL ou SQLite.
- Streamlit.
- Joblib.
- Pytest.

## 5. Estrutura recomendada

```text
credit-risk-scoring
├── app
│   └── streamlit_app.py
├── data
│   ├── raw
│   └── processed
├── models
├── notebooks
│   ├── 01_data_quality.ipynb
│   ├── 02_eda.ipynb
│   └── 03_modeling.ipynb
├── reports
├── src
│   ├── data
│   ├── features
│   ├── modeling
│   └── scoring
├── tests
├── requirements.txt
└── README.md
```

## 6. Dicionário de dados mínimo

- customer_id: identificador sintético.
- age: idade.
- monthly_income: renda mensal.
- employment_months: tempo de emprego.
- debt_amount: dívida total.
- debt_to_income: comprometimento de renda.
- recent_delays: atrasos recentes.
- credit_history_months: tempo de histórico.
- current_products: quantidade de produtos.
- requested_amount: valor solicitado.
- relationship_months: relacionamento com a instituição.
- defaulted: variável alvo, 0 ou 1.

Não utilizar atributos sensíveis como raça, religião, orientação sexual ou deficiência. Avaliar também variáveis que possam funcionar como proxies indiretas.

## 7. Política de score por regras

Criar um score de 0 a 1000 como baseline explicável. Exemplo:

- Renda e estabilidade: até 180 pontos.
- Comprometimento de renda: até 220 pontos.
- Histórico de atrasos: até 250 pontos.
- Tempo de histórico: até 120 pontos.
- Relacionamento: até 80 pontos.
- Relação entre valor solicitado e renda: até 150 pontos.

Faixas simuladas:

- 750 a 1000: risco baixo.
- 550 a 749: risco moderado.
- 0 a 549: risco alto.

Os pesos devem ser justificados e tratados como hipótese de produto, não como verdade financeira.

## 8. Modelos do MVP

Começar simples:

1. Regressão logística como baseline interpretável.
2. Árvore de decisão limitada para comparação.
3. Random Forest apenas como evolução.

Métricas obrigatórias:

- Matriz de confusão.
- Precision.
- Recall.
- F1-score.
- ROC-AUC.
- PR-AUC, especialmente em classes desbalanceadas.
- Taxa de falsos negativos e falsos positivos.
- Curva de calibração.

Não usar apenas acurácia.

## 9. Passo a passo de desenvolvimento

### Etapa 1 — Ambiente

1. Criar ambiente virtual.
2. Instalar dependências.
3. Criar estrutura de diretórios.
4. Configurar `requirements.txt` ou `pyproject.toml`.
5. Criar `Makefile` ou comandos documentados para executar análise, testes e app.

### Etapa 2 — Dataset

1. Definir pelo menos 5.000 registros simulados.
2. Criar distribuições plausíveis para renda, dívidas, atrasos e valor solicitado.
3. Introduzir valores ausentes e inconsistências de forma controlada.
4. Criar a variável alvo com regras probabilísticas documentadas.
5. Salvar dados brutos sem alteração em `data/raw`.
6. Criar `DATA_DICTIONARY.md`.

Critério de aceite: qualquer pessoa entende cada coluna, unidade, faixa e regra de geração.

### Etapa 3 — Qualidade e exploração

1. Verificar duplicidades.
2. Medir valores ausentes.
3. Detectar faixas inválidas.
4. Analisar distribuição da variável alvo.
5. Avaliar correlações sem assumir causalidade.
6. Comparar perfil de clientes adimplentes e inadimplentes.
7. Registrar decisões de limpeza.

Entregável: notebook `01_data_quality.ipynb` e relatório resumido.

### Etapa 4 — Engenharia de atributos

Criar atributos como:

- debt_to_income = dívida / renda.
- request_to_income = valor solicitado / renda.
- delay_rate = atrasos / meses de histórico.
- stability_index baseado em emprego e relacionamento.

Construir transformações com `Pipeline` e `ColumnTransformer` para evitar vazamento de dados.

### Etapa 5 — Score por regras

1. Implementar função pura de scoring.
2. Separar regras em arquivo configurável.
3. Criar explicação dos pontos ganhos e perdidos.
4. Testar valores de fronteira.
5. Gerar distribuição das faixas de risco.

Critério de aceite: o mesmo input sempre produz score e justificativa reproduzíveis.

### Etapa 6 — Modelo preditivo

1. Separar treino e teste antes de ajustar transformações.
2. Usar estratificação pela variável alvo.
3. Treinar regressão logística.
4. Avaliar métricas.
5. Definir threshold orientado pelo custo de falsos negativos e falsos positivos.
6. Comparar com árvore de decisão.
7. Salvar pipeline completo com Joblib.
8. Registrar versão, data e métricas do modelo.

### Etapa 7 — Explicabilidade e equidade

1. Exibir coeficientes ou importância das variáveis.
2. Criar reason codes para cada decisão.
3. Comparar métricas entre faixas etárias apenas para diagnóstico.
4. Documentar limitações do dataset simulado.
5. Explicar que correlação não prova causalidade.
6. Criar um `MODEL_CARD.md`.

### Etapa 8 — Aplicação Streamlit

Páginas sugeridas:

- Visão executiva.
- Qualidade dos dados.
- Análise de risco.
- Simulador de perfil fictício.
- Comparação regras versus modelo.
- Metodologia, ética e limitações.

O simulador deve mostrar score, faixa de risco, probabilidade estimada e principais fatores, sem afirmar aprovação real.

### Etapa 9 — Testes

Testar:

- Cálculo de indicadores.
- Score mínimo e máximo.
- Valores nulos.
- Renda zero ou negativa.
- Thresholds das faixas.
- Pipeline de transformação.
- Predição com schema incorreto.
- Carregamento do modelo.

### Etapa 10 — Publicação

1. Adicionar prints e GIF do Streamlit.
2. Publicar app com dados fictícios.
3. Configurar CI para lint e testes.
4. Inserir resultados reais do experimento no README.
5. Informar data, versão e limitações.

## 10. Critérios para considerar pronto

- Dataset e dicionário estão versionados.
- Limpeza e features são reproduzíveis.
- Não há vazamento entre treino e teste.
- Métricas vão além de acurácia.
- Threshold possui justificativa.
- Score é explicável.
- Model Card documenta uso, riscos e limitações.
- Streamlit executa com um único comando.
- Testes passam no CI.

## 11. Sequência de commits sugerida

```text
chore: create credit risk project structure
feat: generate synthetic credit dataset
data: document variables and quality rules
feat: add exploratory risk analysis
feat: implement explainable rule based score
feat: add preprocessing and logistic regression pipeline
feat: compare model thresholds and metrics
feat: add streamlit risk dashboard
test: cover scoring and preprocessing rules
docs: add model card and ethical limitations
ci: validate tests and code quality
```

## 12. Evoluções de nível sênior

- MLflow para experimentos.
- Feature store simplificada.
- Monitoramento de drift.
- Recalibração de probabilidades.
- Estratégia champion/challenger.
- Análise de custo financeiro por threshold.
- API FastAPI para scoring.
- Docker e observabilidade.

## 13. Evidências para o portfólio

Mostrar arquitetura, dicionário, análise de qualidade, comparação entre regras e modelos, definição do threshold, Model Card, dashboard e uma narrativa clara sobre precisão, risco, explicabilidade e responsabilidade.