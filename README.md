# Tech Challenge 3 — Análise de Atrasos de Voos

**Aluno:** Gustavo Soriano Leandro | **RM:** 368023
**Curso:** Machine Learning Engineering — FIAP

## Descrição

Projeto completo de análise e modelagem sobre o dataset de voos domésticos dos EUA em 2015, abrangendo análise exploratória, modelagem supervisionada para predição de atrasos e modelagem não supervisionada para segmentação de aeroportos e companhias aéreas.

## Estrutura

```
├── 01_EDA.ipynb                  # Análise exploratória de dados
├── 02_supervisioned_modeling.ipynb  # Modelagem supervisionada
├── 03_unsupervised_modeling.ipynb   # Modelagem não supervisionada
├── data/                         # Datasets (não versionados)
│   ├── flights.csv
│   ├── airlines.csv
│   └── airports.csv
└── fiap/                         # Ambiente virtual (não versionado)
```

---

## Notebook 1 — Análise Exploratória (01_EDA.ipynb)

Análise exploratória completa do dataset com foco em atrasos, cancelamentos e padrões operacionais.

**Conteúdo:**

1. Carregamento dos dados e visão geral do schema
2. Estatísticas descritivas e tratamento de valores ausentes
3. Distribuição de atrasos e análise por companhia aérea
4. Padrões temporais — % de atrasos por mês, dia da semana e heatmap
5. Causas dos atrasos — visão geral e participação por mês
6. Análise de cancelamentos — taxa, motivos e padrões temporais
7. Aeroportos — volume, maiores atrasos, maiores cancelamentos
8. Distância × Atraso — atraso médio e recuperação por faixa de distância
9. Correlação entre variáveis numéricas e resumo de insights

---

## Notebook 2 — Modelagem Supervisionada (02_supervisioned_modeling.ipynb)

**Objetivo:** Prever atrasos de chegada (> 15 minutos) utilizando apenas informações pré-voo, evitando data leakage.

**Pré-processamento:**
- Filtro de voos concluídos (sem cancelamentos/desvios): 5,7M → 5,2M registros
- Amostragem estratificada: 500k registros
- 8 features: mês, dia da semana, hora de partida, companhia aérea, aeroportos de origem/destino, distância e tempo programado
- Label encoding para features categóricas; StandardScaler para Regressão Logística
- Divisão treino/teste: 80/20 estratificado

**Modelos e Resultados:**

| Modelo | Acurácia | Precisão (Atraso) | Recall (Atraso) | F1 (Atraso) | ROC-AUC |
|---|---|---|---|---|---|
| Regressão Logística (class_weight) | 0.5828 | 0.2434 | 0.5955 | 0.3456 | 0.6141 |
| Random Forest (class_weight) | 0.6316 | 0.2746 | 0.6038 | 0.3775 | 0.6712 |
| **Random Forest (balanced)** | **0.6058** | **0.2716** | **0.6720** | **0.3868** | **0.6848** |

**Conclusões:**
- Random Forest com treino balanceado (50/50) foi o melhor modelo — detecta 67% dos atrasos reais com ROC-AUC de 0.6848
- O desbalanceamento de classes (82% pontuais / 18% atrasados) foi o principal desafio
- Validação cruzada estratificada (5-fold) confirmou estabilidade sem overfitting (std < 0.003)
- Teto de desempenho (~F1 0.38) limitado pela ausência de features operacionais em tempo real (clima, atraso de partida); incluir `DEPARTURE_DELAY` elevaria ROC-AUC acima de 0.95
- Feature importance (RF): aeroportos de origem/destino e companhia aérea são os mais relevantes

---

## Notebook 3 — Modelagem Não Supervisionada (03_unsupervised_modeling.ipynb)

**Objetivo:** Descobrir estruturas naturais nos dados — identificar clusters de aeroportos e companhias aéreas com comportamento operacional similar.

**Algoritmos:** K-Means + PCA (redução de dimensionalidade)

**Aeroportos (k=4, Silhouette: 0.1883):**
- 7 métricas operacionais por aeroporto (atraso médio, % cancelamentos, tempo de táxi, causas de atraso)
- Filtro: aeroportos com ≥ 1.000 voos → 224 aeroportos
- 3 componentes PCA capturam 77,9% da variância
- Clusters identificados:
  - **Baixo Atraso:** Aeroportos regionais/menores com operações eficientes (ex: ATL, HNL)
  - **Intermediário — Cascata:** Aeroportos impactados por propagação de atrasos na rede
  - **Intermediário — Clima:** Aeroportos no Midwest/Nordeste com atrasos climáticos (ex: ORD, BOS)
  - **Alto Atraso:** Hubs congestionados com longos tempos de táxi (ex: LGA, EWR)

**Companhias Aéreas (k=3, Silhouette: 0.4093):**
- 9 métricas de desempenho por companhia
- Todos os 14 airlines incluídos
- 3 componentes PCA capturam 85,1% da variância
- Clusters identificados:
  - **Baixo Desempenho:** Spirit, Frontier — maiores atrasos e cancelamentos, alta responsabilidade interna
  - **Intermediário:** United, American, Delta, Southwest — desempenho mediano
  - **Alto Desempenho:** Alaska, Hawaiian — menores atrasos, rotas longas com boa recuperação

**Conclusões:**
- A clusterização separou aeroportos pelo *tipo de problema* (cascata vs. clima), não apenas pela severidade
- Resultados confirmaram outliers identificados na EDA (Spirit/Frontier como underperformers consistentes)
- Alta correlação interna entre features valida a eficácia da redução dimensional

---

## Como executar

```bash
# Criar e ativar o ambiente virtual
python -m venv fiap
source fiap/bin/activate

# Instalar dependências
pip install pandas numpy matplotlib seaborn scikit-learn jupyter

# Iniciar o Jupyter
jupyter notebook
```

Execute os notebooks na ordem: `01_EDA.ipynb` → `02_supervisioned_modeling.ipynb` → `03_unsupervised_modeling.ipynb`
