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
6. Atrasos por companhia aérea (chegada, partida e % de voos atrasados)
7. Padrões temporais — % de atrasos e volume por mês, dia da semana e heatmap mês × dia
8. Causas dos atrasos — visão geral e participação por mês
9. Análise de cancelamentos — taxa por mês, dia da semana e motivos por mês
10. Aeroportos — volume, maiores atrasos, maiores cancelamentos e causas por aeroporto
11. Distância × Atraso — atraso médio e recuperação de atraso por faixa de distância
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
