# Hermes Reply — Fase 5

## 👤 Integrante
- **Nome:** Robson Alves Costa  
- **RM:** 565066  
- **Curso:** Inteligência Artificial – FIAP  
- **Fase:** 5-SET-2025  

Este repositório contém a modelagem de dados (Oracle) e o pipeline de Machine Learning **personalizados** para o RM 565066.  
A seguir, você encontra um **guia completo** de execução e a **explicação de cada imagem** usada no projeto.

---

## 📦 Estrutura principal
- `db/hermes_reply_fase5_modelo_relacional_rm565066.dmd` — **arquivo DMD do Data Modeler**
- `db/hermes_reply_fase5_modelo_relacional_rm565066` — **pasta com os arquivos criados pelo Data Modeler**
- `db/schema_oracle_rm565066.sql` — **schema de criação do banco de dados**
- `db/script_insert_dados_rm565066.sql` — **script de criação de registros no banco de dados**
- `db/consultas_demo_rm565066.sql` — **consultas úteis de demonstração**
- `docs/dicionario_dados_rm565066.csv` — **dicionário de dados**
- `data/leituras_sensores.csv` — **base simulada (por sensor, por data/hora)**
- `data/metricas_ativos_regressao.csv` — **base para ML (features + target)**
- `notebooks/ml_regressao_rm565066.ipynb` — **arquivo do notebook**
- `assets/schema_oracle_Data_Modeler_rm565066.png` — **DER do banco de dados**
- `assets/Distribuicao_rm565066.png`, `assets/Paridade_rm565066.png`, `assets/Serie_Temporal_rm565066.png` — **gráficos do ML**

---

## 🧰 Passo a passo — Banco de Dados (Oracle)

1. **Criar o schema**  
   - Rode `db/schema_oracle_rm565066.sql`.

2. **Popular com dados reais da simulação**  
   - Execute o script: **`script_insert_dados_rm565066.sql`** (fornecido junto a este README).  
   - Ele insere:
     - 1 **PLANTA**
     - 5 **ATIVOS**
     - 15 **SENSORES**
     - ~16 mil **LEITURAS** (toda a base `leituras_sensores.csv`)

3. **Explorar com queries prontas**  
   - `db/consultas_demo_rm565066.sql` inclui:
---

## 🤖 Passo a passo — ML (Regressão)
1. Abra `notebooks/ml_regressao_rm565066.ipynb` no Colab ou local.  
2. As entradas vêm de `data/metricas_ativos_regressao.csv` (já gerado a partir da base longa).  
3. Modelos comparados: **LinearRegression**, **RandomForestRegressor**, **GradientBoostingRegressor**.  
4. Métricas: **MAE**, **RMSE**, **R²**.  
5. Gráficos gerados automaticamente em `assets/`.

---

## 🖼️ Imagens e explicações

### 1) Diagrama ER (alta resolução)
![DER](assets/schema_oracle_Data_Modeler_rm565066.png)

**O que mostra:**  
- Entidades **PLANTA**, **ATIVO**, **SENSOR**, **LEITURA_SENSOR**, **EVENTO_MANUTENCAO**.  
- Cardinalidades principais: `PLANTA 1:N ATIVO`, `ATIVO 1:N SENSOR`, `SENSOR 1:N LEITURA_SENSOR`, `ATIVO 1:N EVENTO_MANUTENCAO`.  
- Campos e chaves evidenciados (P=PK, F=FK, U=UNIQUE, *=NOT NULL).  
**Por que está assim:** layout otimizado e cores leves para leitura em telas e relatórios.

### 2) Série temporal — Real vs. Previsto (regressão)
![Série temporal](assets/Serie_Temporal_rm565066.png.png)

**O que mostra:** comparação das primeiras amostras do conjunto de teste, entre o valor real da **vibração média móvel (+10 min)** e o valor **previsto** pelo melhor modelo.  
**Por que é útil:** evidencia a aderência do modelo ao padrão temporal.

### 3) Dispersão (Paridade) — Real vs. Previsto
![Paridade](assets/Paridade_rm565066.png)

**O que mostra:** cada ponto representa um par *(y_real, y_previsto)*.  
**Como ler:** quanto mais os pontos estiverem próximos da diagonal, melhor a performance.  
**Uso:** rápida verificação de viés e dispersão do erro.

### 4) Distribuição dos resíduos
![Resíduos](assets/Distribuicao_rm565066.png)

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
