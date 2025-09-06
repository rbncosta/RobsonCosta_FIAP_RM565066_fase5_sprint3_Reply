# Hermes Reply — Fase 5 (PT-BR) • RM 565066 — README Final

Este repositório contém a modelagem de dados (Oracle) e o pipeline de Machine Learning **personalizados** para o RM 565066.  
A seguir, você encontra um **guia completo** de execução e a **explicação de cada imagem** usada no projeto.

---

## 📦 Estrutura principal
- `db/schema_oracle_ptbr.sql` — criação do schema (Oracle 12c+)
- `db/schema_oracle_ptbr_legacy.sql` — criação do schema (Oracle 11g, com sequences + triggers)
- `db/consultas_exemplo.sql` — consultas úteis (médias, anomalias, junções, top vibração)
- `docs/dicionario_dados.csv` — dicionário de dados em PT-BR
- `data/leituras_sensores.csv` — **base longa** simulada (por sensor, por data/hora)
- `data/metricas_ativos_regressao.csv` — **base larga** para ML (features + target)
- `notebooks/ml_regressao_ptbr.ipynb` — regressão (prever vibração média móvel +10min)
- `assets/der_alta_resolucao_rm565066.png` — **DER em alta resolução**
- `assets/serie_temporal_reg.png`, `assets/paridade_reg.png`, `assets/residuos_reg.png` — **gráficos do ML**

---

## 🧰 Passo a passo — Banco de Dados (Oracle)

1. **Criar o schema**  
   - Rode `db/schema_oracle_ptbr.sql` (ou a versão `*_legacy.sql` se estiver em 11g).

2. **Popular com dados reais da simulação**  
   - Execute o script: **`script_insert_dados_rm565066.sql`** (fornecido junto a este README).  
   - Ele insere:
     - 1 **PLANTA**
     - 5 **ATIVOS**
     - 15 **SENSORES**
     - ~16 mil **LEITURAS** (toda a base `leituras_sensores.csv`)

3. **Explorar com queries prontas**  
   - `db/consultas_exemplo.sql` inclui:
     - últimas 24h (`V_LEITURAS_24H`),
     - médias por hora (`V_RESUMO_ATIVO_HORA`),
     - possíveis anomalias (`V_ANOMALIAS`),
     - top vibração do dia e junções completas.

---

## 🤖 Passo a passo — ML (Regressão)
1. Abra `notebooks/ml_regressao_ptbr.ipynb` no Colab ou local.  
2. As entradas vêm de `data/metricas_ativos_regressao.csv` (já gerado a partir da base longa).  
3. Modelos comparados: **LinearRegression**, **RandomForestRegressor**, **GradientBoostingRegressor**.  
4. Métricas: **MAE**, **RMSE**, **R²**.  
5. Gráficos gerados automaticamente em `assets/`.

---

## 🖼️ Imagens e explicações

### 1) Diagrama ER (alta resolução)
![DER](assets/der_alta_resolucao_rm565066.png)

**O que mostra:**  
- Entidades **PLANTA**, **ATIVO**, **SENSOR**, **LEITURA_SENSOR**, **EVENTO_MANUTENCAO**.  
- Cardinalidades principais: `PLANTA 1:N ATIVO`, `ATIVO 1:N SENSOR`, `SENSOR 1:N LEITURA_SENSOR`, `ATIVO 1:N EVENTO_MANUTENCAO`.  
- Campos e chaves evidenciados (P=PK, F=FK, U=UNIQUE, *=NOT NULL).  
**Por que está assim:** layout otimizado e cores leves para leitura em telas e relatórios.

### 2) Série temporal — Real vs. Previsto (regressão)
![Série temporal](assets/serie_temporal_reg.png)

**O que mostra:** comparação das primeiras amostras do conjunto de teste, entre o valor real da **vibração média móvel (+10 min)** e o valor **previsto** pelo melhor modelo.  
**Por que é útil:** evidencia a aderência do modelo ao padrão temporal.

### 3) Dispersão (Paridade) — Real vs. Previsto
![Paridade](assets/paridade_reg.png)

**O que mostra:** cada ponto representa um par *(y_real, y_previsto)*.  
**Como ler:** quanto mais os pontos estiverem próximos da diagonal, melhor a performance.  
**Uso:** rápida verificação de viés e dispersão do erro.

### 4) Distribuição dos resíduos
![Resíduos](assets/residuos_reg.png)

**O que mostra:** histograma dos **resíduos** *(erro = real − previsto)*.  
**Como ler:** distribuição aproximadamente centrada em 0 e sem caudas muito longas sugere bom ajuste e ausência de viés sistemático.

---

## 🧪 Validações e boas práticas aplicadas
- **Constraints** (PK, FK, UNIQUE, CHECK) para integridade.  
- **Índices** por tempo e FK para performance.  
- **Views** para análise rápida (24h, médias por hora e anomalias).  
- **Feature engineering** com médias móveis e **target futuro** (+10 min).  
- **Comparação de modelos** e análise de **resíduos**.

---

## 📎 Créditos e personalização
Entrega personalizada para **RM 565066** (seed, thresholds, nomenclatura PT-BR e gráficos).  
Dúvidas ou ajustes (ex.: incluir novas views, índices compostos, mais features de série temporal), só avisar.

