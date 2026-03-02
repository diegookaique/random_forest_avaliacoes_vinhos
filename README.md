# random_forest_avaliacoes_vinhos

## Projeto de base de Vinhos utilizando Random Forest

### Base de dados de avaliações de vinhos, onde o objetivo é prever a pontuação dos vinhos usando o algoritmo de Random Forest para classificação multiclasse.

**Variável de Saída (Target):**

Quality: Pontuação do vinho baseada em dados sensoriais, variando de 0 a 10.

Foi identificado Outliers nas variaveis: Acido Citrico, Açucar Residual, Dioxido de Enxofre Livre e Dioxido de Enxofre Total, porem decidimos manter as variaveis, já que o modelo que vamos utilizar roda bem com presenças de Outliers.

Variaveis que tem a maior correlação com a Target são: 'alcool', 'acidez_volatil', 'sulfatos', 'acido_cítrico'.

**Insights sobre o Modelo Random Forest:**

**Acurácia Geral:** A acurácia do modelo é de 0.66 (66%). Isso significa que 66% das previsões do modelo estavam corretas no conjunto de teste. Para um problema de classificação multiclasse, essa é uma acurácia razoável, mas há espaço para melhorias, especialmente considerando a distribuição das classes.



**Relatório de Classificação (Precision, Recall, F1-Score):**

Classes 5 e 6: O modelo performou melhor nas classes 5 e 6, que são as classes mais frequentes. Para a classe 5, temos uma precisão de 0.72, recall de 0.75 e F1-score de 0.73. Para a classe 6, os valores são 0.63, 0.69 e 0.66, respectivamente. Isso é esperado, pois o modelo teve mais exemplos para aprender sobre essas classes.

Classe 7: A classe 7 teve uma performance aceitável, com F1-score de 0.57 (Precision: 0.63, Recall: 0.52). Embora menor que as classes 5 e 6, ainda demonstra alguma capacidade de previsão.

Classes 3, 4 e 8: O modelo teve uma performance muito ruim para as classes 3, 4 e 8, apresentando Precision, Recall e F1-score de 0.00. Isso indica que o modelo não conseguiu prever corretamente nenhuma instância dessas classes. Olhando para a matriz de confusão, podemos ver que estas classes foram quase totalmente ignoradas ou mal classificadas.

**Matriz de Confusão:**

A matriz de confusão reforça o que o relatório de classificação mostrou. Por exemplo:

Para a classe 3 (1 amostra no teste), o modelo não previu nenhuma corretamente (predição 0, recall 0).

Para a classe 4 (10 amostras no teste), o modelo não previu nenhuma corretamente.

Para a classe 8 (5 amostras no teste), o modelo não previu nenhuma corretamente.

A maioria das previsões para as classes minoritárias (3, 4, 7, 8) foi 'desviada' para as classes majoritárias (5 e 6), indicando uma tendência do modelo a prever as classes mais comuns.



**Podemos dizer que o modelo teve uma dificuldade evidente em prever as classes minoritárias (3, 4 e 8), e isso tem uma relação direta e muito forte com o balanceamento dos dados.**

Foi observado uma enorme diferença no número de amostras entre as classes. Por exemplo, a classe '5' e '6' juntas representam mais de 80% do dataset. As classes '3', '4' e '8' são muito raras.
O modelo de Random Forest (ou qualquer outro modelo de aprendizado de máquina) naturalmente tem dificuldades em aprender padrões para classes que aparecem muito poucas vezes nos dados de treino. Ele tende a focar nas classes majoritárias para maximizar a acurácia geral, o que leva a uma performance pobre nas classes com poucos exemplos (baixa representatividade).

Para melhorar a capacidade do modelo de prever essas classes minoritárias, técnicas de tratamento de desbalanceamento de classes serão essenciais, como oversampling (ex: SMOTE) ou undersampling, ou ainda o uso de algoritmos que considerem o custo do erro de classificação de forma diferente para cada classe.

## Conclusão da Otimização de Hiperparâmetros e Desafios Remanescentes

### Análise Comparativa do Modelo Original e Otimizado

Ao comparar os resultados do modelo Random Forest original com o modelo otimizado via Randomized Search, observamos o seguinte:

*   **Acurácia Geral:** Ambos os modelos apresentaram uma acurácia geral de 0.66 (66%). Isso sugere que, em termos de classificações corretas em todo o conjunto de dados, a otimização de hiperparâmetros não trouxe uma melhoria significativa na acurácia global.

*   **Relatório de Classificação:**
    *   **Classes 5 e 6 (Majoritárias):** O desempenho nessas classes permaneceu robusto e consistente. No modelo original, a classe 5 tinha F1-score de 0.73 e a classe 6 de 0.66. No modelo otimizado, a classe 5 teve F1-score de 0.72 e a classe 6 de 0.66. Pequenas variações podem ser observadas, mas sem mudanças drásticas.
    *   **Classe 7:** O F1-score para a classe 7 foi de 0.57 no modelo original e 0.58 no modelo otimizado, indicando uma ligeira melhora, mas ainda mantendo um desempenho moderado.
    *   **Classes 3, 4 e 8 (Minoritárias):** Infelizmente, para as classes com menor representatividade (3, 4 e 8), o cenário permaneceu inalterado. Ambos os modelos apresentaram Precision, Recall e F1-score de 0.00 para essas classes. Isso significa que, mesmo após a otimização de hiperparâmetros, o modelo ainda não conseguiu prever corretamente nenhuma amostra dessas classes.

*   **Matriz de Confusão:** As matrizes de confusão de ambos os modelos são muito semelhantes. Continuamos a observar que as amostras das classes 3, 4 e 8 são erroneamente classificadas como classes majoritárias (principalmente 5 e 6). Por exemplo, a classe 3 (1 amostra) foi classificada como 5, a classe 4 (10 amostras) foi classificada como 5 ou 6, e a classe 8 (5 amostras) foi classificada como 6 ou 7. Isso reforça a incapacidade do modelo de aprender e identificar essas classes minoritárias.

### Conclusão sobre a Otimização de Hiperparâmetros

Embora o Randomized Search tenha encontrado uma combinação de hiperparâmetros (`n_estimators`: 200, `min_samples_split`: 2, `min_samples_leaf`: 1, `max_features`: 'sqrt', `max_depth`: 20) que poderiam ser considerados ótimos para a acurácia geral, **não houve melhorias tangíveis no desempenho do modelo em relação às classes minoritárias (3, 4 e 8)**. A otimização se concentrou em refinar a capacidade do modelo dentro do contexto existente, que já era dominado pelas classes majoritárias.

### Persistência dos Desafios de Balanceamento de Classes

Sim, **os desafios relacionados ao balanceamento de classes persistem e são a principal causa da baixa performance nas classes minoritárias**. A distribuição desbalanceada das classes é evidente tanto no conjunto de treino quanto no de teste:

*   **Y_train:** Classe 5 (551), Classe 6 (506), Classe 7 (157), Classe 4 (43), Classe 8 (13), Classe 3 (9).
*   **Y_test:** Classe 6 (132), Classe 5 (130), Classe 7 (42), Classe 4 (10), Classe 8 (5), Classe 3 (1).

Com tão poucas amostras para as classes 3, 4 e 8, o modelo não consegue extrair padrões suficientes para fazer previsões precisas para elas. Ele tende a priorizar as classes mais frequentes, onde um erro de classificação é menos penalizador para a acurácia geral.

### Aspectos que ainda necessitam de atenção e Técnicas Futuras

Para construir um modelo mais robusto e capaz de prever todas as classes de qualidade de vinho, os seguintes aspectos e técnicas precisam ser considerados:

1.  **Tratamento do Desbalanceamento de Classes:** Esta é a prioridade máxima.
    *   **Oversampling:** Técnicas como SMOTE (Synthetic Minority Oversampling Technique) podem ser aplicadas para gerar amostras sintéticas das classes minoritárias, aumentando sua representatividade no conjunto de treino.
    *   **Undersampling:** Reduzir o número de amostras das classes majoritárias para equilibrar o conjunto de dados. No entanto, esta técnica pode levar à perda de informações valiosas.
    *   **Combinação de Oversampling e Undersampling:** Abordagens híbridas podem ser eficazes para balancear o dataset sem perda excessiva de dados ou criação excessiva de dados sintéticos.
    *   **Ajuste de Pesos de Classes:** Muitos algoritmos de classificação, incluindo o Random Forest, permitem atribuir pesos maiores às classes minoritárias durante o treinamento, penalizando mais os erros de classificação nessas classes.

2.  **Métricas de Avaliação Adequadas:** Para datasets desbalanceados, a acurácia não é uma métrica suficiente. Deve-se focar em:
    *   **F1-Score:** Especialmente o F1-score ponderado (weighted F1-score) ou F1-score por classe, que oferece um equilíbrio entre precisão e recall para cada classe.
    *   **Recall:** Essencial para garantir que o modelo identifique o maior número possível de instâncias das classes minoritárias.
    *   **Precision:** Para garantir que as previsões para as classes minoritárias sejam corretas.
    *   **Área sob a curva ROC (AUC-ROC) e AUC-PR (Precision-Recall):** Estas são métricas mais robustas para avaliar modelos em dados desbalanceados, especialmente AUC-PR para classes minoritárias.

3.  **Outras Abordagens de Modelagem:** Explorar outros modelos que lidam bem com desbalanceamento ou que podem ser facilmente adaptados:
    *   **Algoritmos sensíveis a custos:** Modelos que permitem definir diferentes custos para diferentes tipos de erros de classificação.
    *   **Ensembles Avançados:** Técnicas como EasyEnsemble ou BalanceCascade que combinam múltiplos classificadores treinados em subconjuntos balanceados dos dados.

Em resumo, a otimização de hiperparâmetros por si só não foi suficiente para resolver o problema fundamental do desbalanceamento de classes. A próxima etapa crucial seria implementar estratégias para tratar esse desbalanceamento, juntamente com uma avaliação mais aprofundada usando métricas apropriadas, para que o modelo possa prever com eficácia as qualidades de vinho menos comuns.

**Gostou da Análise?** Conecte-se para trocarmos experiências e ideias sobre projetos de dados!

🔗 **Meu LinkedIn:** [https://www.linkedin.com/in/diego-kaique-9ba3697b]

📧 **Contato:** [kaique_0208@hotmail.com]
