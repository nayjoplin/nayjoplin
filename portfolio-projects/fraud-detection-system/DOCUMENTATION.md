# Fraud Detection System — Documentação de Implementação

## 1. Resultado esperado

Construir um sistema analítico que processe transações simuladas, aplique regras explicáveis de detecção, gere um score de risco e apresente alertas em um dashboard. Depois do baseline por regras, o projeto pode comparar um modelo supervisionado.

O objetivo não é afirmar que a solução bloqueia fraudes reais, mas demonstrar raciocínio de risco, engenharia de atributos, avaliação de modelos desbalanceados, explicabilidade e operação de alertas.

## 2. Pergunta de negócio

Como priorizar transações suspeitas para análise sem gerar um volume inviável de falsos positivos e sem ignorar comportamentos de alto risco?

## 3. Escopo do MVP

- Dataset sintético de clientes e transações.
- Perfil comportamental histórico por cliente.
- Motor de regras configurável.
- Score de risco de 0 a 100.
- Classificação LOW, MEDIUM, HIGH e CRITICAL.
- Fila de alertas com motivos.
- Dashboard Streamlit ou Power BI.
- Avaliação com flag de fraude simulada.
- Relatório sobre falsos positivos e falsos negativos.

Fora do MVP:

- Bloqueio de transações reais.
- Integração com bancos, cartões ou dados pessoais.
- Reconhecimento de dispositivos reais.
- Promessa de prevenção completa de fraude.

## 4. Stack recomendada

- Python 3.12.
- Pandas e NumPy.
- Scikit-learn.
- SQL e PostgreSQL ou SQLite.
- Streamlit.
- Matplotlib.
- Pytest.
- YAML para configuração das regras.

## 5. Estrutura recomendada

```text
fraud-detection-system
├── app
├── config
│   └── fraud_rules.yml
├── data
│   ├── raw
│   └── processed
├── docs
├── models
├── notebooks
├── reports
├── src
│   ├── features
│   ├── rules
│   ├── scoring
│   └── modeling
├── tests
└── README.md
```

## 6. Dados mínimos

### customers

- customer_id.
- account_created_at.
- home_state.
- preferred_channel.
- average_transaction_amount.
- usual_start_hour.
- usual_end_hour.
- known_device_count.

### transactions

- transaction_id.
- customer_id.
- amount.
- transaction_at.
- transaction_type.
- channel.
- merchant_category.
- state.
- device_id.
- status.
- is_fraud: flag sintética para avaliação.

Gerar classe minoritária realista, por exemplo entre 1% e 5%, e documentar a escolha.

## 7. Engenharia de atributos

Criar features sem usar informações futuras:

- amount_vs_customer_mean.
- amount_zscore_by_customer.
- transactions_last_10min.
- transactions_last_1h.
- minutes_since_previous_transaction.
- unusual_hour_flag.
- unusual_state_flag.
- new_device_flag.
- channel_change_flag.
- merchant_risk_score.
- failed_attempts_last_24h.

Toda feature deve informar janela temporal, origem e risco de vazamento.

## 8. Motor de regras

Exemplo de configuração:

```yaml
rules:
  - id: HIGH_AMOUNT
    weight: 25
    description: Valor muito acima da média do cliente
  - id: TRANSACTION_VELOCITY
    weight: 30
    description: Muitas transações em curto período
  - id: NEW_DEVICE
    weight: 15
    description: Dispositivo ainda não associado ao cliente
  - id: UNUSUAL_LOCATION
    weight: 20
    description: Localidade fora do padrão histórico
  - id: UNUSUAL_HOUR
    weight: 10
    description: Horário fora do comportamento habitual
```

Cada alerta deve guardar:

- score total.
- nível de risco.
- regras acionadas.
- valores observados.
- threshold utilizado.
- timestamp da avaliação.

## 9. Política inicial de risco

- 0 a 29: LOW.
- 30 a 54: MEDIUM.
- 55 a 79: HIGH.
- 80 a 100: CRITICAL.

Essas faixas são hipóteses. Elas devem ser ajustadas com base em métricas e custo operacional.

## 10. Passo a passo de desenvolvimento

### Etapa 1 — Preparação

1. Criar ambiente e dependências.
2. Criar estrutura de diretórios.
3. Configurar seed e logging.
4. Criar arquivo YAML das regras.
5. Definir comandos para gerar dados, calcular features, avaliar regras e iniciar o dashboard.

### Etapa 2 — Geração do dataset

1. Gerar clientes com perfis diferentes.
2. Gerar histórico normal por cliente.
3. Inserir padrões suspeitos controlados.
4. Criar flag sintética de fraude com regras probabilísticas.
5. Manter registro de quais cenários foram introduzidos.
6. Criar dicionário de dados.

Cenários mínimos:

- valor abruptamente elevado.
- sequência rápida de compras.
- novo dispositivo.
- horário incomum.
- mudança geográfica.
- tentativas falhas seguidas de aprovação.

### Etapa 3 — Análise exploratória

1. Medir desbalanceamento.
2. Comparar valor e frequência por classe.
3. Analisar fraude por canal, horário e tipo.
4. Investigar outliers.
5. Verificar duplicatas e valores ausentes.
6. Documentar possíveis vieses da geração sintética.

### Etapa 4 — Perfil comportamental

1. Ordenar transações por cliente e tempo.
2. Calcular média e desvio histórico usando somente passado.
3. Criar janelas móveis.
4. Criar indicadores de dispositivo, canal e localização.
5. Salvar tabela analítica de features.

Critério de aceite: nenhuma feature usa a própria flag de fraude nem informação posterior à transação.

### Etapa 5 — Motor de regras

1. Criar uma interface comum para regras.
2. Carregar pesos do YAML.
3. Aplicar cada regra separadamente.
4. Somar score com limite máximo de 100.
5. Gerar reason codes.
6. Criar fila de alertas ordenada por score e data.
7. Testar fronteiras dos thresholds.

### Etapa 6 — Avaliação operacional

Medir:

- alertas gerados por dia.
- precision dos alertas HIGH e CRITICAL.
- recall das fraudes simuladas.
- falsos positivos.
- falsos negativos.
- taxa de alertas por mil transações.
- valor financeiro potencialmente exposto.

Criar uma matriz de custo simulada para discutir trade-offs.

### Etapa 7 — Modelo de machine learning

Somente depois do baseline:

1. Separar treino e teste por tempo, não apenas aleatoriamente.
2. Treinar regressão logística ou árvore como baseline.
3. Usar `class_weight` quando adequado.
4. Comparar PR-AUC, recall e precision.
5. Selecionar threshold operacional.
6. Comparar modelo, regras e abordagem híbrida.
7. Salvar Model Card.

### Etapa 8 — Dashboard

Páginas sugeridas:

- Resumo de risco.
- Fila de alertas.
- Detalhe da transação.
- Motivos do alerta.
- Análise de falsos positivos.
- Comparação de thresholds.
- Metodologia e limitações.

Nunca exibir dados reais ou identificadores sensíveis.

### Etapa 9 — Testes

Testar:

- Regra de valor alto.
- Velocidade de transações.
- Novo dispositivo.
- Mudança de localização.
- Score máximo.
- Classificação por faixa.
- Reason codes.
- Janelas temporais.
- Ausência de vazamento.
- Reprodutibilidade do dataset.

### Etapa 10 — Documentação e publicação

1. Adicionar diagrama do fluxo.
2. Documentar thresholds.
3. Inserir prints do dashboard.
4. Criar relatório de experimentos.
5. Publicar app com dados sintéticos.
6. Configurar CI.
7. Explicar limitações e próximos passos.

## 11. Critérios para considerar pronto

- Dataset e cenários de fraude estão documentados.
- Features usam apenas passado.
- Regras são configuráveis e explicáveis.
- Cada alerta possui motivo.
- Métricas consideram desbalanceamento.
- Threshold é justificado.
- Dashboard mostra fila e contexto.
- Testes passam no CI.
- Não há alegação de uso em produção real.

## 12. Sequência de commits sugerida

```text
chore: create fraud detection project structure
feat: generate synthetic transaction history
data: document fraud scenarios and variables
feat: build customer behavior features
feat: implement configurable fraud rules
feat: add risk score and reason codes
feat: create alert queue and operational metrics
feat: add streamlit fraud dashboard
feat: compare baseline machine learning model
test: cover rules and temporal features
docs: add model card and limitations
ci: run fraud detection tests
```

## 13. Evoluções de nível sênior

- Processamento de eventos com Kafka ou RabbitMQ.
- Feature store online e offline.
- Detecção de drift.
- Grafos de relacionamento entre contas e dispositivos.
- Estratégia rules plus model.
- Feedback de analistas para retreinamento.
- API de decisão com baixa latência.
- Monitoramento de thresholds e fila operacional.

## 14. Evidências para o portfólio

Apresentar dataset sintético, diagrama, regras configuráveis, features temporais, reason codes, comparação entre thresholds, dashboard de alertas e uma análise clara do equilíbrio entre proteção e experiência do cliente.