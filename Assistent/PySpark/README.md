# Introdução ao Apache Spark com PySpark

## Introdução

Apache Spark é uma estrutura de processamento de dados em grande escala que pode distribuir tarefas de processamento de dados em vários nós em um cluster. Ele é especialmente projetado para o processamento rápido de grandes conjuntos de dados, oferecendo APIs fáceis de usar e compatíveis com várias linguagens, incluindo Python (PySpark), Scala, Java e R.

### Definição
Apache Spark é uma plataforma de computação de código aberto usada para processamento de dados em larga escala. Originalmente desenvolvido pela Universidade da Califórnia, em Berkeley, o Spark oferece processamento em memória, o que melhora significativamente a velocidade das operações em comparação com o processamento em disco.

### Utilização Prática
Spark é utilizado em uma variedade de cenários, incluindo:
- **Análise de dados em larga escala**: Permite processar grandes volumes de dados rapidamente.
- **Aprendizado de Máquina**: Possui a biblioteca MLlib para algoritmos de aprendizado de máquina.
- **Processamento de streaming**: Permite o processamento de dados em tempo real com Spark Streaming.
- **SQL**: Spark SQL permite consultas SQL sobre grandes conjuntos de dados.
- **Graph Processing**: A biblioteca GraphX do Spark suporta algoritmos de processamento de grafos.

### Pandas x PySpark
- **Pandas**:
  - Ideal para manipulação e análise de dados em pequenas e médias escalas.
  - Funciona bem em uma única máquina.
  - Possui uma API muito expressiva e fácil de usar.
- **PySpark**:
  - Projetado para processamento de dados em larga escala.
  - Utiliza a arquitetura distribuída do Spark, permitindo processamento paralelo em clusters.
  - Pode lidar com datasets que são muito grandes para caber na memória de uma única máquina.

### IDE’s
Para desenvolvimento com Spark, o uso de Jupyter Notebook é altamente recomendado devido à sua interatividade e facilidade de uso. Outras opções incluem PyCharm, Visual Studio Code, e IDEs baseadas em Apache Zeppelin.

## Software Requeridos

Para começar a trabalhar com PySpark, é necessário configurar o ambiente de desenvolvimento adequadamente.

### Pré-requisitos
- **Python 3.8 ou superior**: Necessário para rodar o PySpark.
- **JDK 8 ou superior**: O Spark é baseado em Java, então é necessário ter o Java Development Kit (JDK) instalado.

### IDE Utilizada
- **Jupyter Notebook**:
    - **Instalação**:
        ```sh
        pip install notebook
        ```
    - **Execução**:
        ```sh
        jupyter notebook
        ```

## Instalação do PySpark

Para instalar o PySpark, pode-se usar o pip, o gerenciador de pacotes do Python. A documentação oficial do PySpark fornece orientações detalhadas e está disponível [aqui](https://spark.apache.org/docs/latest/api/python/getting_started/index.html).

### Forma Simplificada
- **Instalação**:
    ```sh
    pip install pyspark
    ```

### Validação
Para verificar se o PySpark foi instalado corretamente:
1. Abra o Jupyter Notebook ou um arquivo Python.
2. Importe o PySpark:
    ```python
    import pyspark
    ```
3. Execute o código para garantir que não há erros.

## Arquitetura do Spark

O Spark possui uma arquitetura distribuída que permite o processamento de dados de maneira eficiente e escalável. A arquitetura é composta por vários componentes e camadas.

### Execução Distribuída
- **Application**: Representa o programa Spark que está sendo executado, contendo uma série de operações lógicas.
- **Driver**: A parte do Spark que coordena a execução do programa. Ele traduz a lógica de alto nível do programa Spark em uma série de etapas para serem executadas no cluster.
- **Session**: O ponto de entrada principal para a funcionalidade do Spark.

### Cluster Manager
O Cluster Manager gerencia os recursos do cluster, alocando recursos para as várias aplicações Spark. Existem diferentes tipos de gerenciadores de cluster:
- **Standalone**: Um gerenciador de cluster próprio do Spark.
- **Hadoop YARN**: Integração com o sistema de gerenciamento de recursos do Hadoop.
- **Mesos**: Gerenciador de cluster de propósito geral que pode suportar várias aplicações distribuídas.
- **Kubernetes**: Sistema de orquestração de contêineres que pode ser usado para gerenciar recursos do Spark em um ambiente de contêineres.

### Camada de Execução
- **Executor**: Cada nó do cluster executa um ou mais executores. Os executores são responsáveis por executar tasks atribuídas pelo driver.
- **Cores**: Cada executor possui várias unidades de processamento chamadas cores. Estas são responsáveis pela execução das tasks.

### Componentes
- **Spark Application**: A aplicação escrita pelo usuário, que roda no cluster Spark.
- **Spark Driver**: O processo que executa a função principal da aplicação e cria a Spark Session.
- **Spark Session**: Interface para conectar-se a um cluster Spark e criar DataFrames, Datasets, etc.
- **Cluster Manager**: Coordena a alocação de recursos no cluster.
- **Spark Executor**: Executa as tasks e armazena dados particionados.

## Estrutura de Execução

A execução no Spark é estruturada em várias camadas hierárquicas, permitindo que o trabalho seja distribuído e executado de maneira eficiente.

### Driver
O Driver é responsável por:
- Converter as operações de alto nível em um plano lógico.
- Converter o plano lógico em um plano físico (DAG de stages e tasks).
- Distribuir as tasks para os executores.

### Jobs
- Cada ação no Spark (por exemplo, `count`, `collect`) cria um job.
- Cada job é dividido em stages.
- Um job pode ser composto por múltiplos stages.

### Stages
- Cada stage é um conjunto de tasks que pode ser executado em paralelo.
- Os stages são criados com base nas dependências de shuffle.

### Tasks
- As menores unidades de trabalho no Spark.
- Cada task é executada em um executor.
- As tasks dentro de um stage podem ser executadas em paralelo.

## API’s do Spark

O Spark oferece diversas APIs para diferentes tipos de processamento e análise de dados, abrangendo desde operações estruturadas de alto nível até processamento de dados em tempo real.

### O que são
- **Estruturadas**: APIs de alto nível que permitem operações com DataFrames e Datasets. Incluem Spark SQL.
- **Baixo Nível**: APIs como o RDD (Resilient Distributed Dataset) que oferecem maior controle sobre o processamento.
- **Streaming Estruturado**: APIs para processamento de dados em tempo real, permitindo a criação de pipelines de processamento de streaming.
- **Analytics Avançado**: Ferramentas e bibliotecas para análises avançadas, incluindo aprendizado de máquina (MLlib) e processamento de grafos (GraphX).
- **Bibliotecas**:
  - **MLlib**: Biblioteca de aprendizado de máquina que fornece algoritmos e ferramentas para machine learning.
  - **GraphX**: API para processamento de grafos.
  - **Spark SQL**: Permite executar consultas SQL e interagir com DataFrames.
  - **Structured Streaming**: API para processamento de dados de streaming em tempo real.

### Exemplos de Uso
- **DataFrames**: Estruturas de dados tabulares semelhantes a tabelas de um banco de dados relacional.
    ```python
    from pyspark.sql import SparkSession

    spark = SparkSession.builder.appName("example").getOrCreate()
    df = spark.read.csv("data.csv", header=True, inferSchema=True)
    df.show()
    ```
- **RDDs**: Estrutura de dados de baixo nível para processamento distribuído.
    ```python
    rdd = spark.sparkContext.parallelize([1, 2, 3, 4, 5])
    rdd.map(lambda x: x * 2).collect()
    ```
- **Streaming**: Processamento de dados em tempo real.
    ```python
    from pyspark.sql import SparkSession

    spark = SparkSession.builder.appName("streaming").getOrCreate()
    df = spark.readStream.format("socket").option("host", "localhost").option("port", 9999).load()
    query = df.writeStream.outputMode("append").format("console").start()
    query.awaitTermination()
    ```
- **Machine Learning com MLlib**:
    ```python
    from pyspark.ml.classification import LogisticRegression

    training = spark.read.format("libsvm").load("sample_libsvm_data.txt")
    lr = LogisticRegression(maxIter=10, regParam=0.3, elasticNetParam=0.8)
    lrModel = lr.fit(training)
    lrModel.summary.predictions.show()
    ```
## Referências Adicionais

Para aprofundar seus conhecimentos em Apache Spark e PySpark, recomendamos o curso "Introdução ao Apache Spark com PySpark" disponível na Udemy. Este curso oferece uma visão abrangente do Spark, desde a instalação até a execução de tarefas complexas de processamento de dados. Você pode acessar o curso [aqui](https://www.udemy.com/course/introducao-ao-apache-spark-com-pyspark/).
