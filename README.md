# Identificação dos Fatores Associados à Inadimplência - ApexBank

Este repositório apresenta uma **Análise Exploratória de Dados (EDA)** detalhada da carteira de crédito de varejo do ApexBank. O objetivo principal é isolar, quantificar e ordenar os fatores que possuem associação estatística com a inadimplência (`loan_status`), servindo como base para a formulação de políticas de crédito e preparação para modelos preditivos.

A base analítica final conta com **32.409 registros** após uma etapa rigorosa de governança de dados e tratamento de inconsistências[cite: 1].

---

## Principais Achados & Insights de Negócio

*   **Comprometimento de Renda (DTI):** É a variável com maior poder discriminatório do estudo[cite: 1]. Acima do limiar de 30% de comprometimento, a taxa de inadimplência salta de **15,4% para 70,4%** (Qui-quadrado = 5.969,05; p < 0,001; V de Cramér = 0,43 - efeito forte)[cite: 1].
*   **Mito da Renda Bruta e Idade:** Isoladamente, a renda nominal e a idade contínua possuem baixíssimo poder discriminatório prático (efeito de Rosenthal r = 0,27 e sobreposição interquartílica quase total)[cite: 1]. Não devem ser utilizadas como réguas de corte lineares simples[cite: 1].
*   **Bolsões de Risco Não-Lineares:** O cruzamento entre faixa etária e finalidade revelou que clientes jovens (até 25 anos) tomando crédito para finalidades reativas (como Consolidação de Dívidas ou Tratamento Médico) concentram altíssimo risco (29,5% e 28,6% de default)[cite: 1].
*   **Sinalizadores Cadastrais:** Locatários (moram de aluguel) têm risco 4,2 vezes maior do que proprietários de imóveis quitados[cite: 1]. Clientes com restrição prévia em bureau têm taxa de default de 37,9% contra 18,4% dos sem restrição[cite: 1].

---

## Rigor Estatístico & Metodologia

Para garantir que os padrões encontrados fossem práticos (e não ruídos de uma amostra grande), o projeto aplicou:
1.  **Teste de Mann-Whitney U** para comparação de variáveis contínuas, acompanhado do tamanho de efeito pelo **r de Rosenthal**[cite: 1].
2.  **Teste Qui-quadrado de Independência** com **V de Cramér** para mensurar a força de associação das variáveis categóricas[cite: 1].
3.  **Correção de Bonferroni** para controle de múltiplos testes de hipóteses, reduzindo o limiar de significância global para $\alpha = 0,01$[cite: 1].
4.  **Intervalos de Confiança de Wilson (95%)** para proporções pontuais, garantindo robustez em subgrupos com menor volumetria (como a faixa de clientes sêniores acima de 50 anos)[cite: 1].

---

## 🛠️ Tecnologias Utilizadas

*   **Linguagem:** Python
*   **Manipulação e Análise:** Pandas e NumPy[cite: 1]
*   **Estatística Inferencial:** Scipy.Stats[cite: 1]
*   **Visualização de Dados:** Matplotlib e Seaborn[cite: 1]

---

## Dicionário de Dados Resumido

| Variável | Tipo | Descrição |
| :--- | :--- | :--- |
| `loan_status` | Categórica (0/1) | Variável-alvo (0 = Adimplente, 1 = Inadimplente)[cite: 1] |
| `person_income` | Numérica | Renda bruta anual do cliente[cite: 1] |
| `loan_percent_income`| Numérica (DTI) | Percentual da renda comprometido com a parcela[cite: 1] |
| `loan_grade` | Categórica (A-G) | Grau de risco interno já atribuído pelo banco[cite: 1] |
| `cb_person_default_on_file` | Categórica (Y/N) | Histórico de restrição prévia em bureau de crédito |

---

## Recomendações Táticas para o Negócio

1.  **Substituir réguas de corte lineares** (como idade mínima ou renda mínima fixa) por regras multivariadas ancoradas em DTI e finalidade do crédito[cite: 1].
2.  **Adotar o limiar de 30% de DTI como filtro de triagem**, exigindo mitigadores (garantias ou histórico 100% limpo) para propostas acima desta faixa, preservando os bons pagadores[cite: 1].
3.  **Aprimorar o Pricing do Grau D:** Como há uma quebra estrutural de risco entre o Grau C (20,7%) e o Grau D (59,0%), propostas no Grau D devem sofrer redução imediata de limite e precificação agressiva por risco[cite: 1].
4.  **Próxima Etapa (Modelagem):** Os padrões não-lineares e interações mapeadas indicam que modelos baseados em árvores de decisão (`XGBoost` ou `LightGBM`) são os mais adequados para a fase preditiva[cite: 1].
