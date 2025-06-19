# README: Projeto de Predição de Idade de Caranguejos - Competição Kaggle

## Introdução

Este projeto consiste na resolução de um problema de regressão preditiva, proposto como uma competição na plataforma Kaggle pelo professor da disciplina de Estatística 3. O objetivo principal é desenvolver um modelo robusto capaz de predizer a idade de caranguejos a partir de diversas variáveis biométricas.

## Conjunto de Dados

O dataset, proveniente da competição Kaggle, contém informações sobre caranguejos, incluindo medidas como **Comprimento**, **Diâmetro**, **Altura**, **Peso** (total e suas componentes: Peso Descascado, Peso das Vísceras, Peso do Casco) e **Sexo**. A variável objetivo é a **Idade**, presente apenas no conjunto de dados de treino (`dados_treino`). Um conjunto de teste (`dados_test`) sem a variável `Idade` é fornecido para a submissão das predições.

## Análise Exploratória de Dados (EDA) e Pré-processamento

Uma análise exploratória aprofundada foi realizada para compreender a natureza das variáveis e prepará-las para a modelagem:

* **Variável Objetivo (`Idade`):** A `Idade` apresentou uma distribuição complexa, caracterizada por forte assimetria à direita nas categorias Fêmea (F) e Macho (M), e uma multimodalidade acentuada na categoria Indeterminado (I). Pontos estatisticamente identificados como "outliers" foram rigorosamente analisados e confirmados como dados válidos, representando subpopulações reais. Para adequar a `Idade` aos pressupostos da regressão linear e linearizar suas relações com os preditores, aplicou-se uma **transformação logarítmica (base 10)**, resultando na `Log_Idade`, com características de distribuição mais apropriadas para os pressupostos da regressão linear.

* **Variáveis Preditivas de Tamanho e Peso:** `Comprimento`, `Diâmetro`, `Altura`, `Peso` (e suas componentes: `Peso Descascado`, `Peso das Vísceras`, `Peso do Casco`) são preditores cruciais, mas suas distribuições também se mostraram complexas (multimodais e assimétricas). Como são medidas intrinsecamente correlacionadas, identificou-se um desafio significativo de **multicolinearidade** ao incluí-las simultaneamente no modelo. A solução adotada foi a **transformação logarítmica (base 10)** para cada uma delas, o que melhorou a linearidade das relações com `Log_Idade` e a homocedasticidade dos resíduos.

* **Variável Categórica (`Sexo`):** A variável `Sexo` (F, M, I) foi reconhecida como um preditor fundamental, pois as distribuições de `Idade` e das medidas de tamanho/peso variavam significativamente entre as categorias sexuais. Foi convertida para o tipo `factor` para uso no modelo.

## Modelagem de Regressão

A construção do modelo de regressão linear (`lm` no R) seguiu uma abordagem iterativa e estratégica:

* **Variável Resposta:** `Log_Idade`.
* **Seleção Estratégica de Preditores:** Para mitigar a severa multicolinearidade entre as variáveis de tamanho e peso, foi realizada uma seleção criteriosa, optando por um subconjunto parcimonioso que representasse as dimensões do caranguejo sem redundância excessiva. Priorizou-se a utilização das componentes de peso individuais.
* **Interações Essenciais:** Foi crucial a inclusão de **termos de interação entre o `Sexo` e as variáveis de tamanho/peso log-transformadas** (por exemplo, `Log_Comprimento:Sexo`, `Log_Peso_Casco:Sexo`). Esta estratégia permitiu ao modelo capturar as diferentes "curvas de crescimento" e relações tamanho-idade por sexo (F, M, I), aumentando a precisão e a plausibilidade biológica das predições.

## Validação e Avaliação

A avaliação do modelo foi focada na capacidade de generalização e na acurácia das predições:

* **Métrica Principal:** O **MAE (Mean Absolute Error)** na escala original da `Idade` (obtido após a reversão da transformação logarítmica das predições).
* **Processo de Validação:** Os modelos candidatos foram comparados calculando o MAE nos dados de treino (reconhecendo que esta é uma estimativa otimista para comparação interna entre modelos). A análise da distribuição das idades preditas no conjunto de teste (`dados_test`, sem `Idade` real) demonstrou que o modelo generaliza bem, com medianas e quartis muito próximos aos valores reais observados no conjunto de treino.
* **Score na Competição Kaggle:** O modelo final obteve um MAE de **1.38328** no conjunto de teste da competição Kaggle. Este score indica um excelente desempenho do modelo, com um erro médio de aproximadamente 1.38 anos na predição da idade dos caranguejos.

## Submissão

O arquivo de submissão (`test_predict.csv`) foi gerado contendo as colunas `Id` e `Age` (nome da coluna de idade predita, conforme formato exigido pela competição), com as previsões na escala original da idade.

## Tecnologias Utilizadas

* **Linguagem:** R
* **Ambiente:** RStudio

## Conclusão do Projeto

Este trabalho permitiu construir um modelo de regressão para a predição da idade de caranguejos que é robusto, estatisticamente válido e biologicamente plausível. Através de um processo analítico detalhado, que incluiu transformações de variáveis complexas e a modelagem estratégica de interações, o modelo demonstrou alta capacidade de generalização, representando um sucesso na aplicação de técnicas estatísticas para resolver um problema preditivo real.
