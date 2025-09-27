# 🎬 Projeto de Análise e Modelagem Preditiva de Notas IMDB

## 📌 Visão Geral
Este projeto tem como objetivo prever a **nota IMDB de filmes** a partir de variáveis disponíveis em uma base de dados.  
O trabalho inclui **análise exploratória (EDA)**, **engenharia de atributos**, **modelagem preditiva** e **boas práticas para evitar data leakage e overfitting**.  

O projeto foi desenvolvido como parte de um desafio técnico de Ciência de Dados.  

---

## 📊 Fluxo do Projeto

### 1. **Análise Exploratória (EDA)**
- Distribuições das variáveis (`IMDB_Rating`, `Meta_score`).  
- Correlação de **Pearson** entre variáveis numéricas.  
- Identificação de pares de features altamente correlacionadas (|r| > 0.85).  
- Visualização dos gêneros mais frequentes.  

### 2. **Feature Engineering Seguro**
- **One-hot encoding** de gêneros.  
- **Reputação em bins** para diretor e ator principal (`baixo`, `médio-baixo`, `médio-alto`, `alto`).  
- **Média de rating dos 3 principais atores** (Star1–Star3).  
- **Transformações logarítmicas** em `Gross` e `No_of_Votes`.  
- **Validação visual de cada transformação**: histogramas, countplots e distribuições.  

### 3. **Modelagem**
- Baselines: **Regressão Linear** e **Random Forest**.  
- Modelo principal: **XGBoost**.  
- Métrica: **RMSE (Root Mean Squared Error)**.  

### 4. **Monitoramento e Robustez**
- **Curvas de aprendizado** para detecção de overfitting/underfitting.  
- **Importância de features** no XGBoost.  
- **Remoção da variável de maior importância** e re-treino para avaliar resiliência do modelo.  

---

## ⚠️ Principais Cuidados

1. **Evitar Data Leakage**  
   - `Meta_score` foi analisado, mas **removido da modelagem**, pois possui correlação muito alta com `IMDB_Rating`.  
   - `Target encoding` direto em atores/diretores foi evitado — optamos por **bins de reputação**.  

2. **Controle de Overfitting**  
   - Curvas de aprendizado usadas para comparar desempenho em treino e validação.  
   - Comparação com baselines mostrou se o ganho do XGBoost era legítimo.  

3. **Interpretação de Features**  
   - Importância de features analisada criticamente.  
   - Re-treino sem a variável mais importante aumentou a robustez do pipeline.  

---

## 🚀 Como Executar o Projeto

### 1. Clonar o repositório
```
git clone https://github.com/seu-usuario/projeto-imdb.git
cd projeto-imdb

2. Criar ambiente virtual
python -m venv venv
source venv/bin/activate   # Linux/Mac
venv\Scripts\activate      # Windows

3. Instalar dependências
pip install -r requirements.txt

4. Executar o notebook
Abra o Jupyter Notebook:
jupyter notebook
E rode LH_CD_RENATOSAMICO.ipynb.

📦 Principais Dependências
Python 3.9+
pandas, numpy
seaborn, matplotlib
scikit-learn
xgboost
joblib
```

## 📈 Resultados

- **Desempenho comparativo:**  
  - O **XGBoost** apresentou o melhor RMSE entre os modelos testados (Regressão Linear, Random Forest e XGBoost), servindo como modelo principal.  
  - Os modelos baseline (Linear e Random Forest) foram usados como referência para validar o ganho real do XGBoost.

- **Impacto da remoção da variável mais importante:**  
  - Identificou-se a *feature* mais importante a partir da importância calculada pelo XGBoost.  
  - Ao remover essa variável e re-treinar o modelo, avaliamos o efeito na generalização:  
    - **Se o RMSE não aumentar significativamente**, a remoção melhora a robustez contra *leakage* sem perda de performance.  
    - **Se o RMSE aumentar bastante**, é preciso verificar se essa *feature* é de fato legítima para uso em produção (ex.: informações só disponíveis depois do lançamento do filme).

- **Features com maior influência:**  
  - `No_of_Votes_log` e `Gross_log` aparecem como variáveis fortes, junto com colunas de gênero e a agregação de reputação do elenco.  
  - As *bins* de reputação (diretor/ator) ajudaram a capturar sinal histórico sem expor diretamente o target.

- **Observações gerais:**  
  - A distribuição do target (`IMDB_Rating`) tende a ser concentrada (pouca variância), o que exige features informativas e validações rigorosas.  
  - Curvas de aprendizado foram usadas para diagnosticar **overfitting/underfitting** e orientar a escolha de complexidade e regularização.
  - O projeto deverá ser revisto e melhorado no futuro.

