# Tech Challenge 3 — Análise Exploratória de Dados

**Aluno:** Gustavo Soriano Leandro | **RM:** 368023
**Curso:** Machine Learning Engineering — FIAP

## Descrição

Análise exploratória completa do dataset de voos domésticos dos EUA em 2015, com foco em atrasos, cancelamentos e padrões operacionais.

## Estrutura

```
├── 01_EDA.ipynb   # Notebook principal com a análise
├── data/          # Datasets (não versionados)
│   ├── flights.csv
│   ├── airlines.csv
│   └── airports.csv
└── fiap/          # Ambiente virtual (não versionado)
```

## Conteúdo do Notebook

1. Carregamento dos dados
2. Visão geral do schema
3. Estatísticas descritivas
4. Análise e tratamento de valores ausentes
5. Distribuição de atrasos
6. Atrasos por companhia aérea
7. Padrões temporais (mês e dia da semana)
8. Causas dos atrasos
9. Análise de cancelamentos
10. Aeroportos mais movimentados e com maiores atrasos
11. Distância × Atraso
12. Correlação entre variáveis numéricas
13. Resumo dos principais insights

## Como executar

```bash
# Criar e ativar o ambiente virtual
python -m venv fiap
source fiap/bin/activate

# Instalar dependências
pip install pandas numpy matplotlib seaborn jinja2 jupyter

# Iniciar o Jupyter
jupyter notebook 01_EDA.ipynb
```
