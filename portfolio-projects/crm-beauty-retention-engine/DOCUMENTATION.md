# CRM Beauty Retention Engine — Documentação de Implementação

## 1. Resultado esperado

Construir um motor de segmentação de clientes para um e-commerce fictício de beleza, usando comportamento de compra para identificar VIPs, clientes novos, recorrentes, em risco e inativos, além de gerar recomendações de campanhas mensuráveis.

O projeto deve conectar CRM, growth, SQL, Python, análise de coorte, regras de negócio e ativação, demonstrando conhecimento do ciclo completo: dado, diagnóstico, segmento, ação e mensuração.

## 2. Perguntas de negócio

- Quem são os clientes de maior valor?
- Quem está próximo da janela esperada de recompra?
- Quais clientes apresentam risco de inatividade?
- Quais categorias geram maior recorrência?
- Que campanha deve ser usada para cada segmento?
- Como medir se a campanha gerou retenção incremental?

## 3. Escopo do MVP

- Base sintética de clientes, pedidos, itens e campanhas.
- Métricas RFM.
- Segmentação baseada em regras.
- Janela de recompra por categoria.
- Identificação de churn comportamental.
- Tabela de audiência para ativação simulada.
- Dashboard de retenção e segmentos.
- Playbook de campanhas.
- Medição de resultado simulada.

Fora do MVP:

- Disparo real de e-mail, SMS ou WhatsApp.
- Uso de contatos reais.
- Integração obrigatória com plataforma de CRM.
- Alegação de causalidade sem experimento.

## 4. Stack recomendada

- Python 3.12.
- Pandas.
- SQL e PostgreSQL ou SQLite.
- Power BI ou Streamlit.
- Pytest.
- YAML ou JSON para regras de segmentos.

## 5. Estrutura recomendada

```text
crm-beauty-retention-engine
├── config
│   └── segments.yml
├── data
│   ├── raw
│   └── processed
├── dashboard
├── docs
├── notebooks
├── reports
├── sql
├── src
│   ├── features
│   ├── segmentation
│   ├── campaigns
│   └── measurement
├── tests
└── README.md
```

## 6. Modelo de dados

### customer

- customer_id.
- created_at.
- acquisition_channel.
- state.
- consent_email.
- consent_sms.
- consent_whatsapp.

### order

- order_id.
- customer_id.
- order_date.
- status.
- gross_revenue.
- discount_amount.
- net_revenue.

### order_item

- order_item_id.
- order_id.
- product_id.
- category.
- quantity.
- net_revenue.

### campaign_exposure

- exposure_id.
- customer_id.
- campaign_id.
- channel.
- sent_at.
- opened_at.
- clicked_at.
- converted_at.

Usar somente consentimentos fictícios, mas respeitar a lógica de opt-in para demonstrar privacidade por design.

## 7. Features de CRM

Calcular por cliente:

- recency_days.
- frequency_orders.
- monetary_value.
- average_order_value.
- total_items.
- favorite_category.
- category_diversity.
- discount_share.
- days_between_orders_mean.
- expected_repurchase_days.
- days_over_expected_repurchase.
- last_campaign_response.
- customer_tenure_days.

Definir uma data de referência fixa para que o cálculo seja reproduzível.

## 8. Segmentos do MVP

### VIP

Alto valor, alta frequência e compra recente.

### Loyal

Frequência consistente e recência saudável, mesmo sem estar no topo de valor.

### New

Primeira compra recente e pouco histórico.

### Promising

Cliente recente com sinais de segunda compra.

### At Risk

Já foi recorrente, mas ultrapassou a janela esperada de recompra.

### Hibernating

Baixa frequência e longo período sem compra.

### Promotion Sensitive

Grande parcela das compras ocorre com desconto.

### Lost

Inatividade superior ao limite definido e baixo engajamento.

As regras devem ser mutuamente exclusivas ou possuir prioridade documentada.

## 9. Exemplo de prioridade

```text
1. Lost
2. At Risk
3. VIP
4. Loyal
5. New
6. Promising
7. Promotion Sensitive
8. Hibernating
```

A ordem evita que um cliente seja classificado em vários segmentos finais sem explicação.

## 10. Passo a passo de desenvolvimento

### Etapa 1 — Planejamento

1. Definir data de referência.
2. Definir objetivo de retenção.
3. Documentar conceito de cliente ativo e recompra.
4. Definir segmentos e prioridade.
5. Criar matriz segmento × campanha × KPI.
6. Documentar regras de consentimento.

### Etapa 2 — Dados simulados

1. Gerar clientes.
2. Gerar pedidos durante 12 a 24 meses.
3. Criar categorias com ciclos de recompra diferentes.
4. Inserir clientes de alta e baixa recorrência.
5. Gerar exposições a campanhas.
6. Adicionar respostas simuladas.
7. Criar dicionário de dados.

Exemplo de ciclos hipotéticos:

- shampoo: 45 dias.
- máscara: 60 dias.
- finalizador: 75 dias.
- kit profissional: 90 dias.

Esses números são hipóteses simuladas e devem ser identificados como tal.

### Etapa 3 — Qualidade e preparação

1. Remover pedidos cancelados.
2. Validar clientes e pedidos.
3. Padronizar datas e canais.
4. Verificar duplicidades.
5. Tratar receita líquida.
6. Garantir que cada exposição de campanha tenha um cliente válido.

### Etapa 4 — RFM

1. Calcular recência, frequência e valor.
2. Criar scores de 1 a 5 por quantis ou regras.
3. Avaliar distribuição.
4. Evitar quantis quando houver muitos empates sem tratamento.
5. Criar tabela final com R, F, M e score combinado.
6. Documentar diferenças entre RFM e segmentos de negócio.

### Etapa 5 — Janela de recompra

1. Calcular intervalo entre pedidos por cliente e categoria.
2. Estimar mediana por categoria.
3. Comparar tempo desde a última compra com a janela.
4. Criar indicador `days_over_expected_repurchase`.
5. Tratar clientes com apenas uma compra separadamente.
6. Documentar limitações da estimativa.

### Etapa 6 — Motor de segmentação

1. Criar regras em YAML ou módulo configurável.
2. Aplicar prioridade.
3. Gerar segmento final e reason code.
4. Criar data da classificação e versão da regra.
5. Validar que todo cliente recebe exatamente um segmento.
6. Produzir histórico de mudanças de segmento como evolução.

### Etapa 7 — Audiências

Criar tabela com:

- customer_id anonimizado.
- segment.
- recommended_campaign.
- recommended_channel conforme consentimento.
- favorite_category.
- reason_code.
- audience_created_at.

Não incluir dados de contato reais.

### Etapa 8 — Playbook de campanhas

Para cada segmento, documentar:

- objetivo.
- mensagem.
- oferta ou conteúdo.
- canal.
- frequência máxima.
- condição de exclusão.
- KPI principal.
- hipótese.

Exemplos:

- New: educação pós-compra e incentivo à segunda compra.
- At Risk: lembrete de reposição e recomendação por categoria.
- VIP: acesso antecipado e benefícios de relacionamento.
- Promotion Sensitive: campanha controlada sem desconto indiscriminado.

### Etapa 9 — Medição

1. Criar grupo tratado e controle simulados.
2. Definir janela de conversão.
3. Medir recompra, receita por cliente e margem.
4. Comparar grupos.
5. Separar correlação de causalidade.
6. Registrar o resultado como experimento simulado.

KPIs:

- taxa de recompra.
- retenção em 30, 60 e 90 dias.
- receita por segmento.
- margem por segmento.
- uplift simulado.
- conversão por campanha.
- unsubscribe simulado.

### Etapa 10 — Dashboard

Páginas sugeridas:

1. Visão executiva de retenção.
2. Segmentos e valor.
3. Recompra e coortes.
4. Clientes em risco.
5. Campanhas e medição.
6. Regras e metodologia.

### Etapa 11 — Testes

Testar:

- RFM com data de referência.
- Cliente sem pedido válido.
- Cliente com uma compra.
- Prioridade entre segmentos.
- Limites de recência.
- Janela por categoria.
- Consentimento por canal.
- Um único segmento final.
- Reprodutibilidade da audiência.
- Cálculo de taxa de recompra.

### Etapa 12 — Publicação

1. Adicionar fluxograma.
2. Publicar regras e exemplos fictícios.
3. Inserir prints do dashboard.
4. Criar playbook em Markdown ou PDF.
5. Configurar CI para cálculos e segmentação.
6. Explicar LGPD, consentimento e minimização de dados.

## 11. Critérios para considerar pronto

- Segmentos têm definição e prioridade.
- Cada cliente recebe exatamente um segmento.
- Toda classificação possui reason code.
- Recompra usa uma definição explícita.
- Audiências respeitam consentimento.
- Dashboard conecta segmento a resultado.
- Campanhas são hipóteses mensuráveis.
- Testes passam.
- Não existem contatos ou dados reais.

## 12. Sequência de commits sugerida

```text
chore: create crm retention structure
data: generate synthetic customers and orders
data: document consent and customer fields
feat: calculate rfm features
feat: estimate category repurchase windows
feat: implement configurable segmentation rules
feat: create activation audience table
feat: add campaign playbook and measurement
feat: build retention dashboard
test: cover segments and repurchase metrics
docs: add privacy and methodology notes
ci: validate retention engine tests
```

## 13. Evoluções de nível sênior

- Histórico de segmentos com SCD tipo 2.
- Modelo de propensão à recompra.
- Survival analysis para tempo até churn.
- Next Best Action.
- Orquestração diária.
- Integração simulada via API.
- Experimentos com cálculo de significância.
- Monitoramento de fadiga e pressão de comunicação.

## 14. Evidências para o portfólio

Apresentar a jornada completa: dados, features, regras, reason codes, audiência, playbook, dashboard e mensuração. O diferencial é demonstrar que CRM não termina na segmentação; ele precisa gerar ação controlada e aprendizado.