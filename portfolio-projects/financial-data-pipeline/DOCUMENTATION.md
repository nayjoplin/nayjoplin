# Financial Data Pipeline — Documentação de Implementação

## 1. Resultado esperado

Construir um pipeline ETL financeiro reproduzível que extraia dados de arquivos simulados, valide qualidade, transforme o conteúdo, carregue um PostgreSQL analítico e alimente um dashboard executivo.

O projeto deve provar domínio de Python, SQL, modelagem dimensional, automação, qualidade de dados, rastreabilidade e documentação de arquitetura.

## 2. Cenário de negócio

Uma empresa financeira recebe arquivos de clientes, produtos e transações de fontes diferentes. Os campos chegam com formatos inconsistentes, registros duplicados e dados ausentes. O objetivo é criar uma fonte confiável para indicadores de operação, receita e comportamento.

## 3. Escopo do MVP

- Extração de CSV e JSON locais.
- Camadas raw, staging e analytics.
- Validações de schema e qualidade.
- Transformações em Python.
- Carga incremental em PostgreSQL.
- Modelo dimensional.
- Consultas SQL analíticas.
- Dashboard Power BI.
- Logs de execução e tabela de auditoria.
- Execução local via Docker Compose.

Fora do MVP:

- Dados financeiros reais.
- Streaming em tempo real.
- Cluster distribuído.
- Airflow obrigatório. Ele pode entrar na evolução.

## 4. Stack recomendada

- Python 3.12.
- Pandas.
- SQLAlchemy e psycopg.
- PostgreSQL.
- Docker Compose.
- Pytest.
- Power BI.
- Logging padrão do Python.
- Pandera ou validações próprias de schema.

## 5. Arquitetura

```text
Arquivos CSV/JSON
       ↓
data/raw — cópia imutável da origem
       ↓
Extract — leitura e metadados
       ↓
Validate — schema e qualidade
       ↓
Transform — padronização e regras
       ↓
PostgreSQL staging
       ↓
PostgreSQL analytics
       ↓
Power BI e relatórios
```

Estrutura:

```text
financial-data-pipeline
├── config
├── data
│   ├── raw
│   ├── rejected
│   └── samples
├── docs
├── notebooks
├── sql
│   ├── ddl
│   ├── transformations
│   └── analytics
├── src
│   ├── extract
│   ├── validate
│   ├── transform
│   ├── load
│   └── common
├── tests
├── docker-compose.yml
└── README.md
```

## 6. Fontes simuladas

### customers.csv

- customer_id.
- full_name.
- birth_date.
- state.
- segment.
- created_at.
- status.

### products.json

- product_id.
- product_name.
- category.
- monthly_fee.
- risk_level.

### transactions.csv

- transaction_id.
- customer_id.
- product_id.
- transaction_type.
- channel.
- amount.
- transaction_at.
- status.

Gerar pelo menos 20.000 transações para permitir análises relevantes sem tornar o projeto pesado.

## 7. Modelo dimensional

### Dimensões

- dim_customer.
- dim_product.
- dim_date.
- dim_channel.

### Fato

- fact_transaction.

Campos principais da fato:

- transaction_key.
- transaction_id.
- customer_key.
- product_key.
- date_key.
- channel_key.
- transaction_type.
- amount.
- status.
- loaded_at.

Adicionar constraints, índices e chaves substitutas nas dimensões.

## 8. Regras de qualidade

- IDs obrigatórios e únicos na origem esperada.
- Valores de transação maiores que zero.
- Datas válidas e não futuras, salvo dado de teste identificado.
- Clientes e produtos referenciados devem existir.
- Status deve pertencer ao domínio configurado.
- Estado deve seguir padrão de duas letras.
- Duplicatas devem ser rejeitadas ou tratadas de forma documentada.
- Linhas inválidas vão para `data/rejected` com motivo.

Métricas de qualidade:

- total recebido.
- total válido.
- total rejeitado.
- percentual de completude.
- duplicidades.
- integridade referencial.
- tempo de processamento.

## 9. Carga incremental e idempotência

1. Calcular hash ou usar ID natural para detectar registros já processados.
2. Registrar nome, hash e data do arquivo em `etl_file_control`.
3. Impedir que o mesmo arquivo seja carregado duas vezes.
4. Usar upsert apenas quando a regra de negócio permitir.
5. Carregar fatos em transação de banco.
6. Atualizar controle somente depois do sucesso.

Tabela de auditoria `etl_run`:

- run_id.
- pipeline_name.
- started_at.
- finished_at.
- status.
- source_rows.
- valid_rows.
- rejected_rows.
- error_message.

## 10. Passo a passo de desenvolvimento

### Etapa 1 — Preparação

1. Criar ambiente Python.
2. Montar diretórios.
3. Configurar Docker Compose com PostgreSQL.
4. Criar `.env.example`.
5. Configurar logs e arquivo de configuração.
6. Definir comandos `make setup`, `make test` e `make pipeline` ou equivalentes.

### Etapa 2 — Geração dos dados

1. Criar gerador determinístico com seed.
2. Produzir clientes, produtos e transações.
3. Incluir alguns erros controlados.
4. Documentar o dicionário de dados.
5. Manter dados pessoais totalmente fictícios.

Critério de aceite: dados podem ser recriados com o mesmo comando e seed.

### Etapa 3 — Banco

1. Criar scripts DDL de staging.
2. Criar dimensões e fato.
3. Criar tabelas de controle e auditoria.
4. Adicionar índices.
5. Criar script de reset apenas para desenvolvimento.

### Etapa 4 — Extract

1. Criar leitores para CSV e JSON.
2. Registrar metadados do arquivo.
3. Copiar ou preservar origem na camada raw.
4. Padronizar retorno dos extratores.
5. Tratar arquivo ausente, vazio ou ilegível.

### Etapa 5 — Validate

1. Validar colunas e tipos.
2. Validar domínios.
3. Separar registros válidos e rejeitados.
4. Gerar relatório de qualidade.
5. Interromper pipeline quando erro crítico ultrapassar limite configurado.

### Etapa 6 — Transform

1. Padronizar nomes de colunas.
2. Converter datas para UTC ou padrão documentado.
3. Normalizar estados, canais e status.
4. Criar chaves de data.
5. Derivar indicadores necessários.
6. Remover duplicatas segundo regra explícita.

As transformações devem ser funções pequenas e testáveis.

### Etapa 7 — Load

1. Carregar staging.
2. Atualizar dimensões.
3. Carregar fato.
4. Aplicar idempotência.
5. Registrar execução.
6. Reverter transação em falha.

### Etapa 8 — SQL analítico

Criar consultas para:

- volume e valor por mês.
- receita por produto.
- clientes ativos.
- ticket médio.
- crescimento mensal.
- canais mais utilizados.
- taxa de transações rejeitadas.
- segmentos com maior valor movimentado.

### Etapa 9 — Dashboard

Páginas sugeridas:

1. Resumo executivo.
2. Clientes e segmentos.
3. Produtos e receita.
4. Canais e comportamento.
5. Qualidade do pipeline.

Criar medidas com nomes claros e uma página de definição dos KPIs.

### Etapa 10 — Testes

Testar:

- Leitura de arquivos.
- Schema ausente.
- Conversão de datas.
- Valores inválidos.
- Deduplicação.
- Integridade referencial.
- Idempotência.
- Rollback da carga.
- Cálculo dos indicadores.

Criar pelo menos um teste de integração usando PostgreSQL temporário ou Testcontainers.

### Etapa 11 — Automação e CI

1. Criar CLI `python -m src.pipeline`.
2. Permitir execução por data ou arquivo.
3. Configurar GitHub Actions para lint e testes.
4. Armazenar logs de exemplo nos artefatos somente quando útil.
5. Documentar agendamento como evolução, sem fingir que existe.

## 11. Critérios para considerar pronto

- Pipeline pode ser executado do zero.
- Reprocessamento não duplica dados.
- Registros inválidos são rastreáveis.
- Toda execução gera auditoria.
- Modelo dimensional está documentado.
- Consultas SQL retornam resultados reproduzíveis.
- Dashboard usa a camada analytics.
- Testes e CI passam.
- README explica arquitetura, decisões e execução.

## 12. Sequência de commits sugerida

```text
chore: bootstrap financial data pipeline
feat: generate synthetic financial sources
data: add schemas and data dictionary
feat: create postgres staging and warehouse tables
feat: implement file extraction
feat: add data quality validations
feat: implement transformations
feat: add incremental idempotent load
feat: create analytical sql queries
test: cover pipeline quality and load rules
feat: add power bi dashboard assets
docs: add architecture and operating guide
ci: validate pipeline tests
```

## 13. Evoluções de nível sênior

- Orquestração com Airflow.
- dbt para transformação e testes SQL.
- Slowly Changing Dimension tipo 2.
- Particionamento da fato.
- Data lineage.
- OpenTelemetry.
- Alertas de SLA e qualidade.
- Arquitetura em nuvem com storage, warehouse e secrets manager.

## 14. Evidências para o portfólio

Mostrar diagrama do fluxo, modelo dimensional, relatório de qualidade, logs de auditoria, testes de idempotência, consultas SQL e dashboard. O case deve explicar como o pipeline transforma dados inconsistentes em informação confiável para decisão.