# Brasileirão: SVM + GridSearchCV

Classificação com SVM (SVC) e ajuste de hiperparâmetros com
GridSearchCV para prever se o time da casa vence uma partida do
Campeonato Brasileiro. Projeto da disciplina de Inteligência
Artificial (UMC), feito em dupla.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/pedrodiniz474/brasileirao-svm-gridsearch/blob/main/brasileirao_svm_gridsearch.ipynb)

## Objetivo
Prever se o **mandante vence** a partida (1 = vitória do mandante,
0 = empate ou derrota). O placar é usado apenas para criar o alvo,
nunca como variável de entrada.

## Dados
- Fonte: [Campeonato Brasileiro de Futebol (Kaggle)](https://www.kaggle.com/datasets/adaoduque/campeonato-brasileiro-de-futebol), de Adão Duque. Arquivo usado: `campeonato-brasileiro-full.csv`.
- 9.165 partidas no arquivo original. Após a limpeza: [número de linhas, veja `dados.shape`].
- O CSV não está neste repositório. Os créditos e a licença dos dados são do autor original.

## Pipeline
| Etapa | O que foi feito |
|---|---|
| Limpeza | Remoção de linhas com valores vazios e de duplicadas |
| Alvo | `resultado` = 1 se o mandante fez mais gols, senão 0 |
| Variáveis de entrada | Rodada, estado do mandante, formação do mandante |
| Encode | `pd.get_dummies`, porque o SVM não trabalha com texto |
| Normalização | `StandardScaler` |
| Divisão | 80% treino, 20% teste (`random_state=42`) |
| Modelo | `GridSearchCV` com `SVC`, `cv=3`, C em [0.1, 1, 10] e kernel em [linear, rbf] |

## Resultados
- Acurácia no teste: **44,74%**
- Melhores parâmetros: **C = 1, kernel = rbf**
- Baseline (classe mais frequente): [valor, veja `y.value_counts(normalize=True)`]

O resultado é modesto. Com apenas rodada, estado e formação do
mandante, o modelo tem pouca informação sobre a força dos times,
que é o que mais pesa no resultado de um jogo. O objetivo do
trabalho foi construir o pipeline completo de classificação com
SVM e ajuste de hiperparâmetros.

## Limitações e próximos passos
- Poucas variáveis, e nenhuma sobre desempenho recente ou força dos times.
- Só a acurácia foi avaliada, sem matriz de confusão, F1 ou comparação com baseline.
- Não foi testado o hiperparâmetro `gamma`.
- Próximos passos: criar variáveis de forma recente de cada time, comparar com um `DummyClassifier` e testar outros modelos.

## Como rodar
1. Baixe `campeonato-brasileiro-full.csv` no link do Kaggle acima.
2. Abra o notebook no Colab pelo botão acima.
3. Envie o CSV na aba **Arquivos**, à esquerda.
4. Execute as células de cima para baixo.

Para rodar localmente: `pip install pandas scikit-learn`.

## Autores
- Pedro Henrique Diniz Matos
- Matheus Barbosa Nunes de Oliveira
