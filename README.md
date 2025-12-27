# 🧬 Thyroid Cancer Recurrence: EDA & Statistical Inference

## 📋 Sobre o Projeto

Este projeto apresenta uma análise exploratória (EDA) e inferencial aprofundada sobre um conjunto de dados de **Câncer de Tireoide**. Os dados compreendem 13 características clinicopatológicas coletadas ao longo de **15 anos**, com cada paciente acompanhado por um período mínimo de 10 anos.

O diferencial deste projeto é o rigor na escolha dos testes estatísticos. Cada hipótese passou por uma validação de premissas (normalidade dos dados e frequências esperadas), garantindo que o método aplicado (como **Mann-Whitney U** ou **Teste Exato de Fisher via R**) fosse o mais adequado para a realidade dos dados.

## 🎯 Questões de Pesquisa

O estudo busca responder perguntas críticas sobre fatores de risco e recorrência:

* **Perfil Demográfico:** Qual a distribuição etária e relação com gênero/tabagismo?
* **Recorrência:** O tabagismo ou a idade influenciam o retorno da doença?
* **Risco Clínico:** A adenopatia e a focalidade estão estatisticamente ligadas ao grau de risco? Grupos de alto risco são compostos por pacientes mais velhos?

## 🔬 Metodologia e Testes de Hipótese

Para todas as análises, o nível de significância foi definido em **5% ($\alpha = 0.05$)**.

Abaixo, o resumo dos testes aplicados e a justificativa técnica baseada na validação dos dados:

| Hipótese Analisada | Teste Final Aplicado | Justificativa / Validação |
| :--- | :--- | :--- |
| **Focalidade vs. Risco** | **Qui-Quadrado** | Variáveis categóricas. Valores esperados na tabela de contingência adequados (> 5). |
| **Idade vs. Recorrência** | **Mann-Whitney U** | Comparação de 2 grupos independentes que **não seguem distribuição normal**. |
| **Tabagismo vs. Recorrência** | **Teste Exato de Fisher** | Variáveis categóricas. Identificada frequência esperada < 5 no teste inicial (Qui-Quadrado), exigindo o método exato de Fisher. |
| **Idade vs. Grupos de Risco** | **Kruskal-Wallis + Post-Hoc** | Comparação de mais de 2 grupos independentes sem distribuição normal. |
| **Adenopatia vs. Risco** | **Teste Exato de Fisher** | Variáveis categóricas. Frequência esperada < 5 identificada, tornando o Qui-Quadrado inadequado. |

### 🛠️ Solução Técnica: Integração Python + R (`rpy2`)
Nas análises de **Tabagismo** e **Adenopatia**, a validação das premissas indicou frequências baixas, exigindo o **Teste Exato de Fisher**.

Para contornar limitações de bibliotecas padrão em tabelas complexas, utilizou-se a biblioteca `rpy2` para executar a função nativa `fisher.test` da linguagem R diretamente no ambiente Python, garantindo precisão nos p-valores.

```python
# Exemplo de uso: Executando Fisher do R dentro do Python
import rpy2.robjects as ro
from rpy2.robjects import pandas2ri

# Conversão e execução
r_contingency_table = pandas2ri.py2rpy(contingency_table)
fisher_test = ro.r["fisher.test"]
result = fisher_test(r_contingency_table, simulate_p_value=True)
```


## 📊 Principais Resultados

Os testes revelaram padrões estatisticamente significativos:

  1. Idade e Risco (Kruskal-Wallis + Post-Hoc):

      * Confirmada diferença significativa nas idades entre os grupos.

      * Post-Hoc: Pacientes de Alto Risco têm idades significativamente maiores que os de Baixo e Intermediário Risco. Não houve diferença entre Baixo e Intermediário.

  2. Idade e Recorrência (Mann-Whitney U):

      As idades no grupo que teve recorrência do câncer são significativamente maiores do que no grupo que não teve.

  3. Fatores Clínicos (Fisher & Qui-Quadrado):

      * Tabagismo: Há relação significativa com a recorrência.

      * Adenopatia: Há relação significativa com o risco do câncer.

      * Focalidade: Há relação significativa com o risco do câncer.
    
## 🚀 Como Executar
1. Clone este repositório
```bash
git clone https://github.com/enzoribeirodev/Thyroid_Disease-EDA-Hypothesis-Testing
```

2. Instale as dependências (necessário ter R instalado no sistema):
```bash
pip install pandas numpy seaborn scipy rpy2
```
