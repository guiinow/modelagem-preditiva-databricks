# Checkpoint 5 - Modelagem Preditiva e Delivery

Este projeto implementa um pipeline completo de **previsão de demanda** no Databricks, baseado no dataset AdventureWorks. A solução cobre desde a preparação dos dados até a entrega de previsões e métricas, seguindo as orientações do checkpoint.

---

## 📌 Estrutura do Projeto

### 1. Preparação dos Dados (`dados_ml`)

* Criação de uma **view temporária** (`dados_ml`) via SQL no Databricks.
* Agregação mensal de vendas por produto e território.
* Cálculo de *lags* (1, 2, 3 e 12 meses).
* Cálculo da **média móvel de 3 meses** (`media_movel_3m`).
* Esta view é a base para todas as análises.

### 2. Forecast por Produto × Loja

* Utilização de **applyInPandas** para rodar previsões por grupo (`produto`, `id_loja`).
* Modelos utilizados:

  * Random Forest
  * XGBoost
  * LightGBM
  * Baseline: Média móvel 3 meses
* Previsão acumulada para os **próximos 3 meses**.
* Cálculo de métricas no holdout (últimos 3 meses reais):

  * MAE, RMSE, R²

### 3. Comparação de Modelos de Regressão

* Features: lags, mês, trimestre, categoria e subcategoria do produto.
* Modelos testados:

  * Regressão Linear
  * Random Forest
  * Gradient Boosting
  * LightGBM
* Avaliação com MAE, RMSE, MAPE.
* Seleção do melhor modelo com base no **menor RMSE**.

### 4. Análise de Sazonalidade

* Seleção do produto mais vendido.
* Série temporal mensal agregada.
* Visualização em linha para identificação de padrões sazonais.

### 5. Crescimento por Centro de Distribuição

* Classificação de territórios:

  * **EUA**: `country_region_code = 'US'`
  * **Resto do Mundo**: demais códigos.
* Previsão da demanda futura com baseline (média móvel × 3).
* Comparação de crescimento entre grupos.

### 6. Estimativa de Zíperes (para Luvas)

* Filtro de produtos `Clothing > Gloves`.
* Previsão baseline para os próximos 3 meses.
* Estimativa de **zíperes necessários = pares previstos × 2**.

---

## 🚀 Como Executar

1. Subir o notebook no **Databricks**.
2. Executar a célula de **preparação da view `dados_ml`**.
3. Rodar a célula principal do script (forecast, comparação de modelos e análises).
4. (Opcional) Salvar os resultados em tabelas Delta ou exportar para CSV.

---

## 📊 Resultados Esperados

* **Forecast granular** por produto × loja, com previsões de 3 meses e métricas de erro.
* **Comparação objetiva** de modelos de regressão, com destaque para o melhor.
* **Visualização de sazonalidade** para um produto representativo.
* **Crescimento da demanda** agregado por EUA × Resto do Mundo.
* **Estimativa de insumos (zíperes)** para atender à demanda prevista de luvas.

---

## 🛠 Tecnologias Utilizadas

* **Databricks (Spark + Pandas)**
* **Python 3.12**
* **Bibliotecas**:

  * `pyspark`
  * `scikit-learn`
  * `xgboost`
  * `lightgbm`
  * `matplotlib`, `seaborn`
  * `pandas`, `numpy`

---

## 📂 Estrutura de Saída

* `df_forecast`: previsões por produto × loja.
* `results`: métricas dos modelos de regressão.
* `crescimento`: comparativo EUA × Resto do Mundo.
* `zippers_needed`: estimativa de zíperes para luvas.

---

## ✅ Entregáveis do Checkpoint

* [x] Preparação da base de dados (`dados_ml`)
* [x] Forecast por produto × loja (3 modelos + baseline)
* [x] Métricas de avaliação (MAE, RMSE, R²)
* [x] Comparação de modelos de regressão
* [x] Análise de sazonalidade
* [x] Crescimento por centro de distribuição (EUA vs Resto)
* [x] Estimativa de zíperes para luvas
