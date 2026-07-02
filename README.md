# MVP-ML-Analytics_Joao_Luiz_Frossard_Pestana_da_Silva

Guarda os dados e o notebook do MVP da disciplina Machine Learning e Analytics - PUC-RJ - 2026/1º semestre.

---

# ⚖️ Classificação Automatizada de Estados Processuais via Machine Learning

Este repositório contém o Produto Mínimo Viável (MVP) desenvolvido para automatizar a triagem e a classificação do estado atual de processos judiciais (Ativo, Arquivado e Suspenso) com base no histórico textual de suas movimentações. 

O projeto aplica técnicas de Processamento de Linguagem Natural (NLP) e Engenharia de Dados para buscar classificar corretamente os processos judiciais e, assim, mitigar os riscos operacionais decorrentes da identificação da sua situação de forma equivocada.
---

## 📌 Organização do Notebook

> **OBS:** Sempre que o título de uma célula de texto contiver a palavra **COMENTÁRIOS** em caixa alta, significa que aquela seção apresenta a análise crítica do processamento executado na célula de código imediatamente anterior.

---

## 🚀 Estrutura Metodológica e Modelos Testados

A seleção dos algoritmos obedece a critérios de adequação volumétrica à base de 6.366 processos, tipagem esparsa dos dados e aos desafios do desbalanceamento de classes. A estratégia seguiu uma linha evolutiva:

### 1. Modelos Tradicionais e Classificadores Clássicos
* **Regressão Logística (`LogReg`):** Definida como o *baseline* obrigatório do projeto.
* **Multinomial Naive Bayes (`Naive Bayes`):** Candidato probabilístico adequado a matrizes de frequências.
* **Linear Support Vector Classifier (`Linear SVC`):** Hiperplano de separação de margem máxima em alta dimensão.
* **K-Neighbors Classifier (`KNN_Cosseno`):** Configurado com distância de cosseno para avaliar a similaridade semântica real.
* **Árvore de Decisão Simples (`Decision Tree`):** Focado em regras lógicas explícitas e interpretabilidade.

### 2. Modelos de Avanço por Comitês (Ensembles)
Incorporados para elevar o patamar do *F1-Score Macro* e do *Recall*, unindo a força de múltiplos classificadores:
* **Random Forest (`RandomForest`):** Abordagem robusta de *bagging* com controle de profundidade máxima para evitar sobreajuste.
* **LightGBM (`LightGBM`):** Estado da arte em *Gradient Boosting*, otimizado para matrizes esparsas e aprendizado sequencial focado nos erros das árvores anteriores.

### 3. Abordagem por Rede Neural
* **Multi-Layer Perceptron (`Rede Neural MLP`):** Injeta não-linearidade profunda para capturar correlações complexas de longo alcance.

> **OBS:** O uso dos modelos citados é possível porque os dados serão vetorizados pelo `TfidfVectorizer`, que os colocará na escala correta ($[0,1]$), dispensando normalização adicional. Os tempos de processamento foram gerenciados com a paralelização interna do KNN, da Random Forest e do LightGBM (`n_jobs=-1`), garantindo a viabilidade de execução do notebook.

---

## 🏆 Resultados e Conclusão do MVP

* **Melhor Solução Encontrada:** A arquitetura que se consagrou ideal para a produção foi o **LightGBM Otimizado aplicado à Janela Temporal 20** (`num_leaves=15` e `colsample_bytree=0.7`).
* **Comparação com o Baseline:** O LightGBM Otimizado superou de forma sólida o baseline em todas as frentes. Enquanto a Regressão Logística sucumbiu ao desbalanceamento e tendeu a concentrar excessivamente suas previsões na classe `Ativo` (gerando falsos ativos e perdendo 20% das suspensões), o modelo campeão mapeou perfeitamente as fronteiras de decisão. Ele identificou a classe crítica com precisão cirúrgica (capturando **quase 89% dos processos suspensos**), corrigiu os erros na classe majoritária (`H:Arquivado`) e entregou um tempo de inferência de apenas **0,10 segundos**.

### 💼 Valor de Negócio do MVP
Com a homologação deste classificador, o jurimetrista passa a contar com um artefato capaz de responder a perguntas estratégicas fundamentais, tais como: *Quantos e quais processos estão atualmente arquivados, ativos ou suspensos na carteira?* Isso viabiliza análises estatísticas descritivas e preditivas de alto nível para provisionamento financeiro e otimização de fluxos de trabalho.

---

## 🛑 Limitações Conhecidas

1. **Dependência Volumétrica:** A classe crítica (`H:Suspenso`) contou com poucas amostras de treino (~392 registros).
2. **Restrição Jurisdicional:** O modelo foi exposto apenas à semântica processual do TJSP e TJRJ. Aplicá-lo a tribunais superiores (STJ/STF) ou à Justiça do Trabalho exige um novo ciclo de ajuste fino.

---

## 🔮 Próximos Passos (Exploração Tecnológica)

Para a evolução do ecossistema rumo a uma versão definitiva de mercado, propõe-se:
1. Expandir a base amostral coletando dados de outros Tribunais de Justiça estaduais.
2. **Explorar o Estado da Arte em NLP:** Transicionar a vetorização atual (TF-IDF) para arquiteturas baseadas em Deep Learning, substituindo a contagem estatística por modelos de **Transformers** e **BERT** (como *BERTimbau* ou *JurisBERT*), que capturam a semântica contextualizada das petições.
3. **Implementar Pipelines via Hugging Face:** Realizar o *fine-tuning* (ajuste fino) de modelos de linguagem pré-treinados no domínio jurídico.

---

## 🛠️ Como Executar o Projeto

1. Abra o arquivo do notebook (`.ipynb`) no ambiente do **Google Colab**.
2. No menu superior, clique em **Ambiente de execução** e selecione **Executar tudo** (ou utilize o atalho `Ctrl + F9`).
3. **Automação de Dados:** Não é necessário fazer o upload manual da base de dados. O pipeline em Python já está configurado para buscar e carregar automaticamente o arquivo `labeled.json` direto da *Release* oficial do projeto hospedada no GitHub.

*Nota sobre o Apêndice do MLP:* Como o processamento deste experimento específico é longo (aproximadamente 8 minutos), a célula correspondente foi desativada no MVP por meio da instrução `if False:`. Caso deseje executá-lo para reproduzir os testes, basta alterar esse comando para `if True:` e garantir o reprocessamento da Janela 1 no item `3.5.1`.
