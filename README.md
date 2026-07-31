
# Manutenção Preditiva de Máquinas (AI4I 2020)

## Visão Geral do Projeto

No contexto da Indústria 4.0, a previsão de falhas em equipamentos antes que elas ocorram é fundamental para reduzir paradas não planejadas, custos de manutenção e riscos operacionais. Este projeto desenvolve um sistema de **manutenção preditiva** capaz de classificar, a partir de leituras de sensores, se uma máquina está prestes a falhar. O trabalho foi desenvolvido para a disciplina de Inteligência Artificial e Aprendizagem de Máquina do curso de Engenharia de Controle e Automação do IFPB - Campus Cajazeiras.

**Informações do Projeto:**
- **Disciplina:** Inteligência Artificial e Aprendizagem de Máquina
- **Curso:** Engenharia de Controle e Automação
- **Instituição:** IFPB - Campus Cajazeiras
- **Autores:** Jéssica Layanne Alves Leite, Julio Cesar Pereira Rodrigues, João Claudio de Oliveira Gomes, Alexandre Gonçalves Matias
- **Data de Apresentação:** 31/07/2026

---

## Objetivos

O objetivo geral deste trabalho é desenvolver um sistema de classificação binária de falhas em máquinas industriais, comparando o desempenho de três algoritmos de aprendizado de máquina.

Os objetivos específicos incluem:
- Realizar uma análise exploratória completa do dataset AI4I 2020.
- Tratar o forte desbalanceamento de classes presente nos dados, onde apenas cerca de 3,4% das amostras representam falhas.
- Implementar e comparar três algoritmos de Machine Learning, incluindo uma rede neural.
- Desenvolver um sistema interativo para predição de falhas.
- Avaliar o desempenho dos modelos utilizando métricas apropriadas para dados desbalanceados.

## Metodologia

O desenvolvimento deste projeto seguiu a metodologia **CRISP-DM** (Cross-Industry Standard Process for Data Mining), estruturada nas seguintes etapas:

1. **Entendimento do Negócio:** Definição do problema de manutenção preditiva.
2. **Entendimento dos Dados:** Análise exploratória inicial do dataset.
3. **Preparação dos Dados:** Limpeza, engenharia de atributos e balanceamento das classes.
4. **Modelagem:** Implementação e treinamento dos algoritmos de Machine Learning.
5. **Avaliação:** Comparação detalhada do desempenho dos modelos.
6. **Implantação:** Criação de um sistema interativo para predição de falhas.

---

## Base de Dados

O projeto utiliza o **AI4I 2020 Predictive Maintenance Dataset**, disponível no Kaggle [1]. A base foi selecionada por reunir características ideais para um projeto de classificação, apresentando dados sintéticos porém realistas, projetados para refletir cenários industriais.

**Características Principais:**
- **Tamanho:** 10.000 linhas e 14 colunas.
- **Desbalanceamento:** Apenas 3,4% das amostras representam falhas, exigindo tratamento específico (como SMOTE) e métricas de avaliação além da acurácia.
- **Variáveis:** Combina dados numéricos contínuos (temperatura, torque, rotação) e categóricos (tipo do produto).
- **Tipos de Falha:** Inclui múltiplos modos de falha específicos (TWF, HDF, PWF, OSF, RNF).

### Descrição das Variáveis

| Variável | Tipo | Descrição |
| :--- | :--- | :--- |
| `UDI` | Numérica | Identificador único |
| `Product ID` | Categórica | ID do produto (L/M/H + serial) |
| `Type` | Categórica | Qualidade: L (Low), M (Medium), H (High) |
| `Air temperature [K]` | Numérica | Temperatura do ar (Kelvin) |
| `Process temperature [K]` | Numérica | Temperatura do processo (Kelvin) |
| `Rotational speed [rpm]` | Numérica | Velocidade de rotação (rpm) |
| `Torque [Nm]` | Numérica | Torque (Nm) |
| `Tool wear [min]` | Numérica | Desgaste da ferramenta (min) |
| `Machine failure` | Alvo (binário) | 1 = falha, 0 = sem falha |
| `TWF, HDF, PWF, OSF, RNF` | Binárias | Modos específicos de falha |

---

## Processamento e Modelagem

### Pré-processamento dos Dados
A preparação dos dados envolveu a remoção de colunas irrelevantes que poderiam causar *data leakage* (como UDI, Product ID e os modos específicos de falha). Foi realizada a engenharia de atributos para criar novas variáveis, incluindo `Temp_diff` (diferença entre temperaturas), `Power [W]` (potência calculada) e `Torque_x_Wear` (interação entre torque e desgaste). 

Os dados categóricos foram codificados, e o conjunto foi dividido em treino e teste (80/20) de forma estratificada. A normalização foi aplicada usando `StandardScaler`, e o desbalanceamento foi tratado exclusivamente no conjunto de treino utilizando a técnica **SMOTE** (Synthetic Minority Over-sampling Technique).

### Algoritmos Implementados
Três algoritmos foram selecionados, treinados e otimizados utilizando Randomized Search com validação cruzada (3-fold), tendo o F1-score como métrica de otimização:

1. **Rede Neural (MLP - Multi-Layer Perceptron):** Com arquitetura de camadas ocultas (64, 32), função de ativação ReLU e otimizador Adam.
2. **Random Forest:** Utilizando 200 estimadores, profundidade máxima de 15, e peso de classe balanceado.
3. **K-Nearest Neighbors (KNN):** Configurado com 5 vizinhos, pesos baseados em distância e métrica de Minkowski.

### Avaliação de Desempenho
Dado o desbalanceamento do dataset, a acurácia isolada foi descartada como métrica principal. A avaliação focou em:
- **Precisão e Recall:** Foco especial no Recall (sensibilidade), pois uma falha não detectada tem alto custo operacional.
- **F1-Score:** Média harmônica entre precisão e recall.
- **ROC-AUC:** Para avaliar a qualidade do ranking probabilístico.

## Sistema Interativo

O projeto culminou na implementação de um sistema interativo de manutenção preditiva. O sistema permite que o usuário insira as leituras de sensores da máquina (tipo do produto, temperatura, RPM, torque e desgaste). Com base nesses dados, o melhor modelo identificado classifica a probabilidade de falha e emite recomendações de ação, como parada programada para inspeção ou monitoramento contínuo.

---

## Referências

[1] Stephan Matzka. (2020). AI4I 2020 Predictive Maintenance Dataset. Kaggle. Disponível em: https://www.kaggle.com/datasets/stephanmatzka/predictive-maintenance-dataset-ai4i-2020

[2] Link do colab: https://colab.research.google.com/drive/11G76k6_5_Jh3dbwbC0B27ilr0zgUqZz2#scrollTo=oZdyuAf0F0bn
