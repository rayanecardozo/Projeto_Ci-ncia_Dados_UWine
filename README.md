# Projeto Uwine – Data Science  (Partes 1 e 2)

**Aluna:** Rayane Cardozo Gama Botelho
**Curso:** Ciência de Dados – Universidade Vila Velha (UVV)
**Base:** `table49.csv` – 1.120.000 notas fiscais x 23 variáveis do clube de vinhos fictício UVVine

---

## Sobre o projeto

Como *Data Science Consultant* do clube de vinhos UVVine, o projeto analisa a base de notas fiscais dos clientes em duas etapas:

| Etapa | Notebook | Conteúdo |
|---|---|---|
| **Parte 1** – Estatística | `WORKFLOW_PARTE_1.ipynb` | Tamanho ideal da amostra (estratificação + Neyman), outliers, testes de hipóteses, estatística descritiva e pesquisa de satisfação |
| **Parte 2** – Machine Learning | `WORKFLOW_PARTE_2.ipynb` | Regressor, clusterizador e classificador treinados nos conjuntos *train*, *validation* e *test* definidos a partir da Parte 1 |

## Estrutura do repositório

```
├── README.md
├── Projeto_UWine.ipynb             # Parte 1 – estatística e amostragem
├── Projeto_UWine_PARTE_2.ipynb     # Parte 2 – machine learning 
└── requirements.txt
```

> A base `table49.csv` (~400 MB) não está no repositório por causa do tamanho. Para executar, coloque o arquivo na mesma pasta do notebook ou faça o upload no Colab.

## Como executar

1. Abra o `rojeto_UWine_PARTE_2.ipynb` no Google Colab (ou no Jupyter local).
2. Faça o upload do `table49.csv` (ou monte o Google Drive e ajuste `CAMINHO_CSV` na primeira célula de código).
3. Execute todas as células em ordem (*Ambiente de execução → Executar tudo*). O tempo total é de cerca de 5 minutos.

O notebook confere automaticamente se a base foi carregada completa (1.120.000 linhas) e interrompe a execução se não estiver. Todas as amostragens usam `SEED = 42`, então os resultados são reprodutíveis.

## Metodologia da Parte 2

**Conjuntos de dados (continuidade da Parte 1)**
- O tamanho ideal **n = 50.300** foi recalculado sobre a base completa (estratificação por TIPO DA CONTA + alocação de Neyman).
- *train*, *validation* e *test* têm 50.300 notas cada. São disjuntos e estratificados com alocação proporcional (29.180 ESSENTIAL + 12.127 VIP + 8.993 PRIME).
- Uma amostra aleatória simples de 50.300 notas não usadas (SAMPLE) simula dados novos da população.

**Preparação (Normalizar – Mapear – Discretizar)**
- Valores químicos negativos (impossíveis) são substituídos pela mediana do *train*.
- As variáveis qualitativas são mapeadas (one-hot / ordinal) e a OPINIÃO DO CLIENTE é discretizada na escala 1–5 da Parte 1.
- Normalização z-score ajustada apenas no *train*, sem vazamento de informação.
- As variáveis de entrada são selecionadas por teste estatístico com correção de Bonferroni e tamanho de efeito mínimo.

**Modelos**

| Modelo | Algoritmo escolhido | Resultado na amostra da população |
|---|---|---|
| Regressor (y = TOTAL R$) | Regressão Linear (melhor de 6 regressores) | R² = 0,7338; MAE = R$ 288,59 |
| Clusterizador | K-Means, k = 3 (cotovelo + silhueta) | Silhueta = 0,5977; ARI com tipo de conta = 0,7802 |
| Classificador (y = TIPO DA CONTA) | Random Forest (melhor de 10 classificadores) | Acurácia = 91,22% (linha de base: 57,86%) |

## Principais conclusões

- A **renda** é o que determina o valor do cliente. Sexo, região, estado civil, dependentes, opinião e as variáveis químicas dos vinhos não têm relação relevante com o gasto nem com o tipo de conta.
- O regressor serve para **projetar faturamento** (erro inferior a 0,3% na média por tipo de conta), mas não para prever uma compra isolada.
- O segmento de **alto valor** (≈ 19% das notas) gera **≈ 51% do faturamento**.
- Clientes ESSENTIAL com perfil de renda VIP/PRIME são candidatos a *upgrade* de conta.

## Tecnologias

Python 3 · pandas · NumPy · SciPy · statsmodels · scikit-learn · matplotlib · seaborn

## Referência

Buitinck, L. *et al.* (2013). *API design for machine learning software: experiences from the scikit-learn project.* ECML PKDD Workshop: Languages for Data Mining and Machine Learning, 108–122.
