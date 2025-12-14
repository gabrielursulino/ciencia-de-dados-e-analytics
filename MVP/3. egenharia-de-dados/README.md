# MVP Engenharia de Dados

**Nome:** Gabriel Lopes Ursulino

**Matrícula:** 4052025000406

**Dataset:** [Cerebral Stroke Prediction-Imbalanced Dataset](https://www.kaggle.com/datasets/shashwatwork/cerebral-stroke-predictionimbalaced-dataset/data)

## 1. Objetivo
**Problema a resolver:** 

O Acidente Vascular Cerebral (AVC), também conhecido pelo termo em inglês *stroke*, é uma das principais causas de morte e invalidez no mundo. O AVC acontece quando vasos que levam sangue ao cérebro entopem ou se rompem, provocando a paralisia da área cerebral que ficou sem circulação sanguínea. O AVC pode ser hemorrágico, quando há rompimento de um vaso cerebral, provocando hemorragia, ou isquêmico, quando há obstrução de uma artéria, impedindo a passagem de oxigênio para células cerebrais, que acabam morrendo. Essa obstrução pode acontecer devido a um trombo (trombose) ou a um êmbolo (embolia).

O objetivo deste MVP é identificar quais fatores clínicos e demográficos estão mais fortemente associados à ocorrência de um AVC. Compreender esses padrões pode auxiliar em campanhas de prevenção direcionadas e triagem médica.

### Perguntas a serem respondidas:

- Qual é a relação entre a idade e a ocorrência de AVC?
- Genero influencia na ocorrência de AVC?
- O tipo de trabalho e o tipo de residência (Urbana/Rural) influenciam no risco?
- Pacientes com histórico de hipertensão ou doença cardíaca têm uma taxa significativamente maior de AVC?
- Níveis elevados de glicose média estão correlacionados com casos de AVC?
- O IMC (Índice de Massa Corporal) tem correlação direta?
- Status de tabagismo influencia na ocorrência de avcs?

- ## 2. Coleta e conjunto de dados
O conjunto de dados escolhido é o *Cerebral Stroke Prediction-Imbalanced Dataset*, um arquivo CSV contendo dados históricos de pacientes e seus estados de saúde. Este é um conjunto de dados multivariado que reúne diversas características que podem estar associadas ao risco de AVC, como idade, hábitos, estilo de vida e condições prévias de saúde como hipertensão e doenças cardiovasculares. O dataset apresentado é amplamente disponível e seu uso é para fins didáticos. Não é necessária uma etapa de seleção de dados externa, pois o dataset já está curado e pronto para uso.

*Para mais detalhes a respeito deste dataset, acesse:* https://www.kaggle.com/datasets/shashwatwork/cerebral-stroke-predictionimbalaced-dataset/data

## 3. Modelagem e Catálogo de Dados
Para este projeto, será utilizada a Arquitetura Medallion (Bronze, Silver, Gold).

Bronze (Raw): Cópia fiel dos dados CSV em formato Delta.

Silver (Cleaned): Dados limpos, tipados e com valores nulos tratados.

Gold (Aggregated/Business): Tabela analítica pronta para BI, modelada em formato "Flat" para facilitar a análise direta, dado o escopo do MVP.

**Catálogo de Dados**

| Coluna | Tipo | Descrição | Domínio/Valores Esperados |
| :--- | :--- | :--- | :--- |
| `id` | Numérico | Identificador único | 1 a 72943 |
| `gender` | Categórico | Gênero do paciente | Male, Female, Other |
| `age` | Numérico | Idade do paciente em anos | 0.0 a 82.0 |
| `hypertension` | Binário | 0: Não tem, 1: Tem hipertensão | 0, 1 |
| `heart_disease` | Binário | 0: Não tem, 1: Tem doença cardíaca | 0, 1 |
| `ever_married` | Categórico | Já foi casado? | No, Yes |
| `work_type` | Categórico | Tipo de trabalho | Private, Self-employed, Govt_job, children, Never_worked |
| `Residence_type` | Categórico | Tipo de residência | Urban, Rural |
| `avg_glucose_level` | Numérico | Nível médio de glicose no sangue | 55.0 a 291.05 |
| `bmi` | Numérico | Índice de Massa Corporal | 10.1 a 97.6 (Contém Nulos) |
| `smoking_status` | Categórico | Status de fumante | formerly smoked, never smoked, smokes, Unknown (Nulos) |
| `stroke` | Binário | **Variável Alvo**: 0: Sem AVC, 1: Teve AVC | 0, 1 |

## 4. Carga e preparação

**Procedimento no Databricks:** Para este MVP, a coleta consiste na ingestão do arquivo estático para o ambiente de Data Lake da plataforma (DBFS - Databricks File System).

### Configuração e Inicialização do Ambiente

Esta etapa atua como o script de configuração inicial do ambiente, sendo fundamental para garantir a governança e a organização correta do MVP. Ela é importante porque materializa fisicamente a Arquitetura Medallion no Databricks, criando os recipientes lógicos isolados (Schemas Bronze, Silver e Gold) para cada etapa do processamento, ao mesmo tempo que assegura a reprodutibilidade do projeto ao limpar resquícios de testes anteriores, oferecendo um "terreno limpo" e estruturado para a execução segura dos pipelines de dados.

Esta seção está dentro do notebook.

## 5. Implementação da Camada Bronze

Esta etapa é responsável pela Ingestão e Catalogação dos dados brutos. Seu objetivo principal é capturar os dados da fonte original e persistí-los no Data Lake em formato otimizado, sem aplicar transformações de negócio, garantindo uma cópia fiel do histórico (raw data).

Esta seção está dentro do notebook.

## 6. Implementação da Camada Silver

Este notebook executa o processo de transformação dos dados brutos em dados confiáveis (Trusted Data). O objetivo central é tratar problemas de qualidade identificados, padronizar tipos de dados e aplicar regras de negócio preliminares para preparar o dataset para análise.

Esta seção está dentro do notebook.

## 7. Implementação da Camada Gold

Este notebook representa a etapa final do pipeline de Engenharia de Dados, onde os dados técnicos tratados na camada Silver são transformados em Informação de Negócio. O objetivo desta camada é facilitar o consumo por analistas e ferramentas de visualização, pré-calculando métricas e aplicando regras de negócio específicas.

Esta seção está dentro do notebook.

## 8. Análise

Este notebook representa a etapa de Consumo de Dados e Visualização. O objetivo é explorar a tabela gold.stroke_analytics_gold utilizando bibliotecas Python pandas, matplotlib e seaborn para responder às perguntas de negócio definidas no início do projeto e identificar padrões de risco.

Esta seção está dentro do notebook.

## 9. Autoavaliação
O objetivo de criar um MVP para análise de previsão de AVC foi atingido com sucesso. Consegui estruturar um pipeline de dados no Databricks que ingere, limpa e disponibiliza os dados para análise. Dificuldades: A principal dificuldade encontrada foi o tratamento de valores nulos na variável IMC (bmi), onde a decisão de usar a mediana precisou ser validada para não introduzir viés. Trabalhos Futuros: Como evolução, planejo implementar um modelo de Machine Learning (Regressão Logística ou Random Forest) utilizando a biblioteca MLlib do Spark para prever a probabilidade de AVC em novos pacientes, transformando este MVP de análise descritiva em um produto preditivo.

## 10. Referências
Documentação completa em PDF.

Repositório público no GitHub

Pipeline de Dados no Databricks






