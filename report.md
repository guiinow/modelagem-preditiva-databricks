# Checkpoint 5 - Modelagem Preditiva e Delivery

Este projeto implementa um pipeline completo de previsão de demanda no Databricks, baseado no dataset AdventureWorks. A solução cobre desde a preparação dos dados até a entrega de previsões e métricas, seguindo as orientações do checkpoint.

---

## 📌 Estrutura do Projeto

### 1. Preparação dos Dados (`dados_ml`)
- Criação de uma **view temporária** (`dados_ml`) via SQL no Databricks.  
- Agregação mensal de vendas por produto e território.  
- Cálculo de *lags* (1, 2, 3 e 12 meses).  
- Cálculo da **média móvel de 3 meses** (`media_movel_3m`).  
- Esta view é a base para todas as análises.  

---

### 2. Forecast por Produto × Loja
- Utilização de **applyInPandas** para rodar previsões por grupo (`produto`, `id_loja`).  
- Modelos utilizados:  
  - Random Forest  
  - XGBoost  
  - LightGBM  
  - Baseline: Média móvel 3 meses  
- Previsão acumulada para os **próximos 3 meses**.  
- Cálculo de métricas no holdout (últimos 3 meses reais):  
  - MAE, RMSE, R²  

**Exemplo de resultado:**  
- Para o produto *Chain*, os três modelos apresentaram previsões consistentes e próximas da baseline.  
- As métricas variam por produto, mas todos os modelos apresentam erros controlados (MAE < 2 em média).  

---

### 3. Comparação de Modelos de Regressão
- Features: lags, mês, trimestre, categoria e subcategoria do produto.  
- Modelos testados:  
  - Regressão Linear  
  - Random Forest  
  - Gradient Boosting  
  - LightGBM  
- Avaliação com MAE, RMSE, MAPE.  
- Seleção do melhor modelo com base no **menor RMSE**.  

**Resultados obtidos:**  
- **Linear Regression** → MAE = 1.07 | RMSE = 1.56 | MAPE = 0.85  
- Random Forest → MAE = 0.96 | RMSE = 1.79 | MAPE = 0.71  
- Gradient Boosting → MAE = 1.10 | RMSE = 1.61 | MAPE = 0.89  
- LightGBM → MAE = 0.95 | RMSE = 1.60 | MAPE = 0.73  

📌 **Melhor modelo**: *Linear Regression* (RMSE = 1.56).  

---

### 4. Análise de Sazonalidade
- Produto analisado: **AWC Logo Cap** (produto representativo com alta venda).  
- A série temporal mostra clara sazonalidade, com picos em determinados meses e quedas em outros.  
- Essa informação é fundamental para planejamento de estoque e campanhas.  

---

### 5. Crescimento por Centro de Distribuição
- Territórios classificados em dois grupos:  
  - **EUA** → `country_region_code = US`  
  - **Resto do Mundo** → demais países  
- Previsão da demanda futura com baseline (média móvel × 3).  

**Resultados:**  
- EUA: **443.594 unidades**  
- Resto do Mundo: **349.155 unidades**  

👉 Conclusão: **EUA apresenta maior crescimento previsto** para os próximos 3 meses.  

---

### 6. Estimativa de Zíperes (para Luvas)
- Filtro de produtos `Clothing > Gloves`.  
- Previsão baseline para os próximos 3 meses.  
- Estimativa de **zíperes necessários = pares previstos × 2**.  

**Resultados:**  
- Previsão total de pares de luvas (próx. 3 meses, baseline): **37.965 pares**  
- Estimativa de zíperes necessária: **75.930 zíperes**  

---

## 📊 Consolidação dos Resultados
- **Forecast granular** por produto × loja, com previsões de 3 meses e métricas de erro.  
- **Comparação objetiva** de modelos de regressão, com destaque para o melhor (Linear Regression).  
- **Visualização de sazonalidade** confirma padrões cíclicos em produtos representativos.  
- **Crescimento da demanda** maior nos EUA em comparação ao resto do mundo.  
- **Estimativa de insumos (zíperes)**: 75.930 zíperes necessários para atender à demanda prevista de luvas.  
